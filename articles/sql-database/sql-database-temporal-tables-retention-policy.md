<properties
   pageTitle="Zarządzanie historycznych danych w tabelach czasowy z zasad przechowywania | Microsoft Azure"
   description="Dowiedz się, jak stosowanie zasad przechowywania czasowy w celu zachowania danych historycznych w obszarze kontrolę."
   services="sql-database"
   documentationCenter=""
   authors="bonova"
   manager="drasumic"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="10/12/2016"
   ms.author="bonova"/>

#<a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Zarządzanie historycznych danych w tabelach czasowy z zasad przechowywania

Tabele czasowy może zwiększyć rozmiar bazy danych, więcej niż zwykłe tabel, zwłaszcza jeśli zachowanie danych historycznych na okres dłuższy czas. W związku z tym zasad przechowywania danych historycznych jest ważnych aspektów planowanie i zarządzanie cyklem życia każda tabela czasowy. Czasowy tabel w bazie danych SQL Azure pochodzą z mechanizmu przechowywania prostych w użyciu, który pomoże Ci wykonania tego zadania.

Przechowywania historii czasowy może być skonfigurowane na poziomie poszczególnych tabeli, który pozwala na tworzenie wiekowania elastyczne zasady. Stosowanie zasad przechowywania czasowy jest proste: wymaga tylko jeden parametr ustalenie podczas zmiany schematu lub tworzenie tabeli.

Po zdefiniowaniu zasad przechowywania bazy danych SQL Azure rozpocznie się regularnie sprawdzanie, czy występują problemy z poprzednich wierszy, które mają uprawniony do oczyszczania danych. Identyfikacja pasujące wiersze i ich usuwanie z tabeli historii wystąpienia przejrzysty, zadania w tle, który jest planowane i wykonywane przez system. Warunek wieku dla wierszy tabeli historii jest zaznaczona opcja oparte na kolumnie reprezentujący koniec okresu SYSTEM_TIME. Jeśli okres przechowywania, na przykład jest ustawiona na sześć miesięcy, wiersze tabeli jest uprawniony do oczyszczania spełniać poniższy warunek:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

W powyższym przykładzie firma Microsoft zakłada, że kolumna **ValidTo** odnosi się do końca okresu SYSTEM_TIME.

##<a name="how-to-configure-retention-policy"></a>Jak skonfigurować zasady przechowywania?

Przed skonfigurowaniem zasad przechowywania do czasowy tabeli, najpierw sprawdź czy czasowy przechowywania historycznych jest włączone *na poziomie bazy danych*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Flaga bazy danych **is_temporal_history_retention_enabled** jest domyślnie włączone, ale użytkownicy mogą zmieniać jego instrukcji ALTER DATABASE. Ustawiono go również automatycznie wyłączone po wykonaniu operacji [punktu przywracania czasu](sql-database-point-in-time-restore-portal.md) . Aby włączyć oczyszczanie przechowywania historii czasowy dla bazy danych, wykonaj następującą instrukcję:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [AZURE.IMPORTANT] Można skonfigurować przechowywania czasowy tabel, nawet jeśli **is_temporal_history_retention_enabled** jest wyłączone, ale automatycznego oczyszczania wieku wiersze nie zostanie w takim przypadku wyzwolony.


Zasady przechowywania skonfigurowano podczas tworzenia tabeli, określając wartość parametru HISTORY_RETENTION_PERIOD:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Baza danych SQL Azure pozwala określić okres przechowywania przy użyciu różne jednostki: dni, TYGODNI, miesięcy i lat. Jeżeli HISTORY_RETENTION_PERIOD zostanie pominięty, przyjmowana jest NIEOGRANICZONY przechowywania. Można również użyć słowa kluczowego NIESKOŃCZONEJ jawnie.

W niektórych scenariuszach może chcesz skonfigurować przechowywania po utworzeniu tabeli lub zmienić wcześniej skonfigurowane wartość. W takim przypadku użyj instrukcja ALTER TABLE:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [AZURE.IMPORTANT]  Ustawianie SYSTEM_VERSIONING wyłączone *nie zostaną zachowane* wartość okresu przechowywania. Ustawienie SYSTEM_VERSIONING ON bez HISTORY_RETENTION_PERIOD jawnie określonych wyniki w okres przechowywania NIEOGRANICZONY.

Aby zapoznać się z bieżącym stanie zasady przechowywania, należy użyć następującej kwerendy sprzęgającej czasowy przechowywania aktywacji flagi na poziomie bazy danych z okresów przechowywania dla poszczególnych tabel:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


##<a name="how-sql-database-deletes-aged-rows"></a>Jak usunąć bazy danych SQL w wieku wierszy?

Proces oczyszczania zależy od układu indeks tabeli historii. Należy zauważyć, tego *tylko tabel historii z indeksem grupowany (B-drzewa lub columnstore) może zawierać zasady przechowywania skończonej skonfigurowane*. Zadania w tle jest tworzona w celu wykonywania oczyszczania danych wieku dla wszystkich tabel czasowy z okres przechowywania skończonej.
Logika Oczyszczanie indeksu grupowany rowstore (B-drzewa) usuwa wieku wiersza w mniejszym fragmentów (do 10 kilobajtów) zminimalizowania ciśnienia w dzienniku bazy danych i wyjścia. Mimo że wykorzystuje logiczny Oczyszczanie wymagane B-drzewa indeksu kolejności usunięcia starsze niż okres przechowywania nie mocno zagwarantować wierszy. W związku z tym *nie zostać dowolnego zależność od kolejności oczyszczanie w aplikacjach*.

Zadania oczyszczania dla grupowany columnstore jednocześnie powoduje usunięcie całej [grupy wierszy](https://msdn.microsoft.com/library/gg492088.aspx) (zazwyczaj zawierają 1 mln wierszy każdego), która jest bardzo efektywne, zwłaszcza w przypadku, gdy danych historycznych jest generowany w szybkim tempie.

![Przechowywanie columnstore grupowany](./media/sql-database-temporal-tables-retention-policy/cciretention.png)


Doskonałe kompresja i ułatwia oczyszczanie wydajną obsługę przechowywania wykres kolumnowy grupowany indeks columnstore doskonałym rozwiązaniem dla scenariuszy po z pracą szybko generuje wysoka ilości danych historycznych. Ten wzorzec jest zazwyczaj intensywnie [obciążenia przetwarzanie transakcji używających czasowy tabele](https://msdn.microsoft.com/library/mt631669.aspx) śledzenia zmian i inspekcji, analizy trendu lub IoT spożyciu danych.

##<a name="index-considerations"></a>Zagadnienia dotyczące indeksu

Zadania oczyszczania dla tabel z indeksem grupowany rowstore wymaga indeksu zaczynać się kolumnie odpowiadający koniec okresu SYSTEM_TIME. Jeśli takie indeks nie istnieje, nie będzie można skonfigurować okres przechowywania skończonej:

*Msg 13765 16 poziomu 1 stanie <br> </br> Ustawianie okres przechowywania skończonej nie powiodło się na wersji systemu tymczasowa tabela "temporalstagetestdb.dbo.WebsiteUserInfo" ponieważ tabeli historii "temporalstagetestdb.dbo.WebsiteUserInfoHistory" nie zawiera wymagane grupowany indeksu. Warto rozważyć utworzenie grupowany columnstore B-drzewa indeksu, rozpoczynając od kolumny, która jest zgodna z końca SYSTEM_TIME okresu w tabeli historii.*

Należy zauważyć, że domyślną tabelę historii utworzone przez bazy danych SQL Azure już zawiera wykres kolumnowy grupowany indeks, który jest zgodny z zasady przechowywania. Jeśli próbujesz usunąć tego indeksu dla tabeli zawierającej okres przechowywania skończonej, operacja nie powiedzie się następujący błąd:

*Msg 13766 16 poziomu 1 stanie <br> </br> nie można usunąć indeks grupowany "WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory", ponieważ jest on używany dla automatycznego oczyszczania danych wieku. Rozważ ustawienie HISTORY_RETENTION_PERIOD do NIESKOŃCZONEJ w odpowiedniej wersji systemu tabeli czasowy, jeśli musisz upuszczać tego indeksu.*

Oczyszczanie w indeksie grupowany columnstore zastosowanie optymalnie historycznych wiersze są wstawiane w kolejności rosnącej (uporządkowane według na koniec okresu kolumny), która jest zawsze wielkości liter, gdy tabeli historii jest wypełniony wyłącznie przez mechanizm SYSTEM_VERSIONIOING. Jeśli wiersze w tabeli historii, nie są zorganizowane końca kolumna okres (który może być wielkości liter, po migracji istniejących danych historycznych), należy ponownie utworzyć indeks grupowany columnstore u góry drzewa B rowstore indeks, który jest prawidłowo zamówiono, uzyskanie optymalnej wydajności.

Unikaj odbudowanie indeksu columnstore grupowany w tabeli historii okres przechowywania ograniczone, ponieważ może zmienić kolejność w grupy wierszy, w sposób naturalny narzucane przez operację przechowywania wersji systemu. Jeśli potrzebujesz odbudowanie indeksu columnstore grupowany w tabeli historii, można to zrobić przez ponowne utworzenie u góry zgodny z indeksu B-drzewa zachowania kolejnością rowgroups niezbędne do oczyszczania danych w regularnych. To samo podejście należy po utworzeniu tabeli czasowy z istniejącej tabeli historii, która zawiera wykres kolumnowy grupowany indeksu kolumny bez danych gwarantowanej zamówienia:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Po skonfigurowaniu okres przechowywania skończonej tabeli historii, w której indeks grupowany columnstore, nie można utworzyć dodatkowe indeksy drzewa B-grupowany w tej tabeli:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Próba wykonania nad instrukcja nie powiedzie się następujący błąd:

*Msg 13772 16 poziomu 1 stanie <br> </br> nie można utworzyć bez klastrów indeksu w tabeli historii czasowy "WebsiteUserInfoHistory" ponieważ ma okres przechowywania skończonej i indeks columnstore grupowany zdefiniowane.*

##<a name="querying-tables-with-retention-policy"></a>Kwerenda tabele z zasad przechowywania

Wszystkie kwerendy w tabeli czasowy odfiltrowywania automatycznie odpowiadające zasady przechowywania ograniczone, aby uniknąć niespójne i nieoczekiwane wyniki, ponieważ wieku wierszy można usunąć przez zadania oczyszczania, *w dowolnym momencie w czasie i w kolejności dowolnego*historycznych wiersze.

Na poniższej ilustracji przedstawiono plan kwerendy prostej kwerendy:

````
SELECT * FROM dbo.WebsiteUserInfo FROM SYSTEM_TIME ALL;
````

Plan zapytania zawiera dodatkowe filtr zastosowany do końca okresu kolumny (ValidTo) w operatora "Grupowany skanowanie indeksu" w tabeli historii (wyróżniony). W tym przykładzie przyjęto założenie, okres przechowywania tego miesiąca została ustawiona na WebsiteUserInfo tabeli.

![Filtr kwerendy przechowywania](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Jednak jeśli kwerenda jest bezpośrednio tabeli historii, można zobaczyć wiersze, które są starsze niż przechowywania określonego okresu, ale bez dowolnego gwarancji dla wyników kwerendy powtarzalnych. Na poniższej ilustracji przedstawiono plan wykonania kwerend dla zapytania w tabeli historii bez dodatkowe filtry zastosowane:

![Kwerenda Historia bez filtru przechowywania](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Logiki biznesowej nie są oparte na odczytywania tabeli historii poza okres przechowywania, jak możesz uzyskać niespójne lub nieoczekiwane wyniki. Zaleca się za pomocą kwerendy czasowy klauzula FOR SYSTEM_TIME do analizowania danych w tabelach czasowy.

##<a name="point-in-time-restore-considerations"></a>Wskaż w sekcji Uwagi Przywróć czasu

Po utworzeniu nowej bazy danych przywracając [istniejącej bazy danych do określonego punktu w czasie](sql-database-point-in-time-restore-portal.md)ma czasowy przechowywania wyłączona na poziomie bazy danych. (flaga**is_temporal_history_retention_enabled** ustawiona na wyłączone). Ta funkcja umożliwia sprawdzenie wszystkie wiersze historycznych na przywracanie, nie martwiąc się, że wieku wiersze są usuwane, zanim przejdziesz do nich kwerendy. Umożliwia jego *Sprawdzanie danych historycznych poza okres przechowywania skonfigurowane*.

Przykład, że tymczasowa tabela ma określony okres przechowywania jeden miesiąc. Jeśli bazy danych został utworzony w warstwie usługi Premium, czy można utworzyć kopię bazy danych z stan bazy danych w górę do 35 dni wstecz w przeszłości. Które skutecznie umożliwi analizowania historycznych wierszy, które mają do 65 dni przez badanie bezpośrednio w tabeli historii.

Jeśli chcesz aktywować Oczyszczanie czasowy przechowywania, uruchom następującą instrukcję języku Transact-SQL po dziesiętnym w Przywróć czasu:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

##<a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak używać czasowy tabel w aplikacji, zapoznaj się z [Wprowadzenie czasowy tabel w bazie danych SQL Azure](sql-database-temporal-tables.md).

Odwiedź stronę kanału 9 [opowieści sukcesu czasowy implementacji rzeczywistą klienta](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) a Obejrzyj [Pokaz czasowy na żywo](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Aby uzyskać szczegółowe informacje o tabelach czasowy Przejrzyj [dokumentację w witrynie MSDN](https://msdn.microsoft.com/library/dn935015.aspx).
