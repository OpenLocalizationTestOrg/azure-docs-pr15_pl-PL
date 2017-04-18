<properties
   pageTitle="Wprowadzenie do programu czasowy tabel w bazie danych Azure SQL | Microsoft Azure"
   description="Dowiedz się, jak rozpocząć pracę z przy użyciu czasowy tabel w bazie danych SQL Azure."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="08/29/2016"
   ms.author="carlrab"/>

#<a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Wprowadzenie do programu czasowy tabel w bazie danych Azure SQL

Czasowy tabele są nowością programowania bazy danych SQL Azure, która umożliwia śledzenie i analizowania pełny historii zmian w danych bez konieczności programowania. Tabele czasowy Zachowaj dane ściśle związane z kontekstu czasu, tak, aby przechowywane fakty może zostać odebrana za ważne tylko w określonym okresie. Ta właściwość czasowy tabel umożliwia wydajną obsługę analizy opartych na czasie i uzyskiwanie wniosków z kształtowanie danych.

##<a name="temporal-scenario"></a>Scenariusz czasowy

W tym artykule przedstawiono kroki umożliwiające są używane tabele czasowy w scenariuszu aplikacji. Załóżmy, że chcesz śledzić aktywność użytkownika w nowej witrynie sieci Web, opracowanym od początku lub na istniejącej witryny internetowej, który chcesz wydłużyć z analizy aktywności użytkownika. W tym przykładzie uproszczone przyjęto założenie, że liczba odwiedzanych stron sieci web w okresie jest wskaźnik, który wymaga rejestrowania i monitorowania w bazie danych witryny sieci Web, który znajduje się na bazy danych SQL Azure. Celem historycznej analizy aktywności użytkownika jest uzyskanie dane wejściowe do projektowania witryny sieci Web i zapewnia lepsze pracę odwiedzających.

Modelu bazy danych, w tym scenariuszu jest bardzo proste — metryki aktywności użytkownika jest reprezentowane za pomocą pola jeden **PageVisited**jest rejestrowany wraz z podstawowych informacji w profilu użytkownika. Ponadto do czasu, na podstawie analizy, czy przechowujesz serię wierszy dla każdego użytkownika, gdzie każdy wiersz reprezentuje liczbę stron odwiedzonej określonego użytkownika w określonym okresie czasu.

![Schematu](./media/sql-database-temporal-tables/AzureTemporal1.png)

Na szczęście nie musisz umieścić dowolną nakładu w aplikacji do obsługi informacji o działaniach. Czasowy tabel ten proces jest zautomatyzowany — które zapewnia pełną elastyczność podczas projektowania witryny sieci Web i dłużej skoncentrować się na samej analizy danych. Tylko kwestią, o których należy wykonać, jest upewnij się, że tabela **WebSiteInfo** jest skonfigurowana jako [czasowy wersji systemu](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). Aby uzyskać szczegółowe instrukcje, aby wykorzystywać czasowy tabel w tym scenariuszu opisano poniżej.

##<a name="step-1-configure-tables-as-temporal"></a>Krok 1: Konfigurowanie tabel jako czasowy

W zależności od tego, czy są uruchamianie nowego rozwoju lub uaktualniania istniejącej aplikacji będzie Tworzenie tabel czasowy lub modyfikować istniejące, dodając czasowy atrybutów. Ogólnie przypadku rozwiązania mogą być zarówno te dwie opcje. Wykonanie tych akcji przy użyciu [Programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) lub innego narzędzia do projektowania języku Transact-SQL.


> [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


###<a name="create-new-table"></a>Tworzenie nowej tabeli

Umożliwia element menu kontekstowego "Nowa tabela wersji systemu" w Eksploratorze obiektów SSMS otwieranie edytora zapytań za pomocą skryptu szablonu tymczasowa tabela, a następnie użyj "Określ wartości dla parametrów szablonu" (Ctrl + Shift + M) do wypełniania szablonu:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

W SSDT należy wybrać szablon "(System numerów wersji) tymczasowa tabela" podczas dodawania nowych elementów projektu bazy danych. Które będzie Otwórz projektanta tabel i można łatwo określić układ tabeli:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Możesz także Utwórz tabelę czasowy, określając instrukcji Transact-SQL bezpośrednio, jak pokazano w poniższym przykładzie. Zauważ, że każda tabela czasowy obowiązkowe elementy są definicji okresu i klauzuli SYSTEM_VERSIONING w odniesieniu do innej tabeli użytkownika, w której będzie przechowywana wersji historycznych wiersza:

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

Podczas tworzenia wersji systemu tymczasowa tabela towarzyszące tabeli historii o domyślnej konfiguracji są tworzone. Tabela historii domyślnego zawiera grupowane indeks B-drzewa w kolumnach okresów (zakończenie, rozpoczęcie) z włączoną kompresją strony. Ta konfiguracja jest optymalna dla większości scenariuszy, w których czasowy tabele są używane, w szczególności dla [danych inspekcja](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

W tym przypadku firma Microsoft mają na celu przeprowadzenia analizy trendu opartych na czasie, przez dłuższy historii danych i większe zestawy danych, dlatego wybór miejsca do magazynowania dla tabeli historii indeks columnstore grupowany. Grupowany columnstore zapewnia bardzo dobrą kompresję i wydajności analitycznych kwerend. Czasowy tabele zawierają możliwość skonfigurowania indeksów w tabeli bieżącej i czasowy całkowicie niezależnie. 

**Uwaga**: indeksy Columnstore są dostępne tylko w warstwie usługi premium.

Poniższy skrypt pokazano, jak można zmienić domyślny indeks w tabeli historii grupowany columnstore:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Czasowy tabele są przedstawione w Eksploratorze obiektów z określonym ikonę ułatwiają identyfikację podczas jego tabeli historii jest wyświetlany jako poziomu węzła podrzędnego.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

###<a name="alter-existing-table-to-temporal"></a>Instrukcja ALTER table istniejących do czasowy

Załóżmy obejmuje alternatywny scenariusza, w którym tabeli WebsiteUserInfo już istnieje, ale nie jest przeznaczone do przechowywania historii zmian. W takim przypadku możesz po prostu rozszerzyć istniejącej tabeli, aby zostać czasowy, jak pokazano w poniższym przykładzie:

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

##<a name="step-2-run-your-workload-regularly"></a>Krok 2: Regularnego uruchamiania usługi Obciążenie pracą

Główną zaletą czasowy tabel jest, nie trzeba zmienić i dostosowywanie witryny sieci Web w żaden sposób przeprowadzić śledzenia zmian. Po utworzeniu czasowy tabel przezroczysty zachowywane poprzednich wersji wiersza każdorazowo wykonywania modyfikacji na danych. 

Aby można było korzystać z automatycznych zmian dla tego scenariusza, Przejdźmy aktualizując kolumny **PagesVisited** każdym razem, gdy użytkownik kończy jego sesji w witrynie sieci Web:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Należy zauważyć, że kwerendy aktualizującej nie musi ustalić dokładną datę i godzinę, kiedy wystąpiła operacja rzeczywisty ani sposób danych historycznych zostaną zachowane dla analizowanie danych w przyszłości. Obydwa aspekty automatycznie są obsługiwane przez bazy danych SQL Azure. Na poniższym diagramie przedstawiono, jak dane historii jest generowany przy każdej aktualizacji.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

##<a name="step-3-perform-historical-data-analysis"></a>Krok 3: Analiza danych historycznych

Teraz po włączeniu czasowy wersji systemu analizy danych historycznych jest tylko jeden kwerendy od siebie. W tym artykule udostępnimy kilka przykładów tego adresu analizy scenariuszach — Aby dowiedzieć się wszystkie szczegóły, Poznaj różne opcje klauzuli [SYSTEM_TIME dla](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) programu Microsoft.

Aby wyświetlić górny 10 użytkowników uporządkowanych według liczbę odwiedzanych stron sieci web na godzinę temu, uruchom tę kwerendę:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

Można łatwo modyfikować tej kwerendy do analizowania wizyty witryny dzień temu miesiąc temu lub w dowolnym momencie w przeszłości, które chcesz.

Aby wykonać podstawowe analiz statystycznych dla poprzedniego dnia, należy użyć następującego przykładu:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

Aby wyszukać działania określonego użytkownika, w terminie, za pomocą klauzuli zawarte w:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Wizualizacja grafiki jest szczególnie wygodne w przypadku czasowy zapytań, jak można wyświetlić trendów i typów użycia w intuicyjny sposób bardzo łatwo:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

##<a name="evolving-table-schema"></a>Rozwijających się schematu tabeli

Zazwyczaj należy zmienić schemat tymczasowa tabela robią opracowywania aplikacji. Do tego wystarczy uruchomić zwykła ALTER TABLE instrukcje i bazy danych SQL Azure będzie prawidłowo propagowanie zmian w tabeli historii. Poniższy skrypt pokazano, jak dodać dodatkowe atrybut śledzenia:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Podobnie możesz zmienić definicji kolumny, gdy jest aktywny z pracą:

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Na koniec możesz usunąć kolumnę, które nie są już potrzebne.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````
    
Możesz też umożliwia najnowszą [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) modyfikowanie schematu czasowy tabeli podczas połączenia z bazą danych (w trybie online) lub jako część projektu bazy danych (w trybie offline).

##<a name="controlling-retention-of-historical-data"></a>Sterowanie przechowywania danych historycznych

System wersji czasowy tabel tabel historii może zwiększyć rozmiar bazy danych, więcej niż zwykłe tabel. Tabel historii duże i ciągle rosnącym może stać się problem zarówno ze względu na koszty wyłącznie miejsca do magazynowania, jak również nakładania wydajności podatek czasowy kwerendy. Opracowanie zasad przechowywania danych zarządzania danymi w tabeli historii, w związku z tym jest ważnych aspektów planowanie i zarządzanie cyklem życia każda tabela czasowy. Z bazy danych SQL Azure masz do zarządzania historycznych danych w tabeli czasowy poniższych metod:

- [Podziału tabeli](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
- [Skrypt niestandardowy Oczyszczanie](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

##<a name="next-steps"></a>Następne kroki

Szczegółowe informacje na temat czasowy tabel zapoznaj się z [dokumentacją w witrynie MSDN](https://msdn.microsoft.com/library/dn935015.aspx).
Odwiedź stronę kanału 9 [opowieści sukcesu czasowy implementacji rzeczywistą klienta](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) a Obejrzyj [Pokaz czasowy na żywo](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).
