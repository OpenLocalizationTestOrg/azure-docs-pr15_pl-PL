<properties
    pageTitle="Poziomu zgodności, jak oceny | Microsoft Azure"
    description="Czynności i narzędzia umożliwiające określenie poziomu zgodności, który jest zalecane dla bazy danych programu Microsoft SQL Server lub bazy danych SQL Azure"
    services="sql-database"
    documentationCenter=""
    authors="alainlissoir"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.devlang="NA"
    ms.tgt_pltfrm="NA"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="alainl"/>


# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Ulepszone funkcje wydajności kwerend z zgodności 130 poziom w bazie danych SQL Azure


Baza danych SQL Azure działa przezroczysty tysiące baz danych na wiele różnych zgodności poziomach, zachowania i gwarantującego zgodność z poprzednimi wersjami do odpowiedniej wersji programu Microsoft SQL Server dla wszystkich klientów!

W związku z tym nic się nie uniemożliwia klienci, którzy zmienić żadnych istniejących baz danych do poziomu zgodności najnowszą korzystały z nowego optymalizatora zapytań i funkcji procesora kwerendy. Jako przypomnienie Historia wyrównania w wersji SQL domyślne poziomy zgodności są następujące:

- 100: w języku SQL Server 2008 i Azure SQL bazy danych V11.
- 110: w języku SQL Server 2012 i Azure SQL bazy danych V11.
- 120: w języku SQL Server 2014 i Azure SQL bazy danych w wersji 12.
- 130: w języku SQL Server 2016 i Azure SQL bazy danych w wersji 12.


> [AZURE.IMPORTANT] Uruchamianie w **2016 mid czerwca**w bazie danych SQL Azure, domyślny poziom zgodności będzie 130 zamiast 120 dla **nowych** baz danych.
> 
> Będzie baz danych utworzonych przed mid czerwca 2016 *nie* dotyczyć i zachowa ich bieżącego poziomu zgodności (100, 110 lub 120). Bazy danych, które migracji z bazy danych SQL Azure w wersji V11 do wersji 12 nie będą mieć poziom zgodności zmianie albo.


W tym artykule możemy zalety korzystania z poziomu zgodności 130 i jak je wykorzystać te korzyści. Określamy możliwe efekty strony na wydajność kwerendy istniejących aplikacji SQL.


## <a name="about-compatibility-level-130"></a>Informacje o poziom zgodności 130


Najpierw Jeśli chcesz wiedzieć bieżącego poziomu zgodności bazy danych, należy wykonać następującą instrukcję języku Transact-SQL.


```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Przed tę zmianę do poziomu 130 się nie dzieje w przypadku **nowo** utworzony baz danych, Przejdźmy Przejrzyj jakie tę zmianę dotyczy wszystkich niektórych przykładów zapytania bardzo podstawowego i zobacz, jak każdy mogą korzystać z niego.

Przetwarzanie kwerend w relacyjnych baz danych mogą być bardzo złożone i może prowadzić do dużej ilości matematycznych i naukowych komputer należy rozumieć związane projekt opcje i zachowania. W tym dokumencie zawartość został celowo uproszczony aby upewnić się, że każda osoba dysponująca minimalne informacje techniczne zrozumieć wpływ zmiany poziomu zgodności i określić, jak mogą korzystać aplikacji.

Przyjrzyjmy się krótki przegląd poziom zgodności 130 żywą w tabeli.  Więcej informacji można znaleźć w [Zmienić poziom zgodności bazy danych (w języku Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), ale Oto krótkie podsumowanie:

- Operacji wstawiania instrukcji select Wstawianie mogą być wielowątkowe lub mieć planu równoległego podczas przed pojedynczym wątku tej operacji.
- Pamięci zoptymalizowany pod kątem tabel i kwerend zmienne tabeli można już równoległe plany, podczas gdy przed operacja została również pojedynczym wątku.
- Statystyki dla tabeli zoptymalizowane pamięci mogą być kodowane i są aktualizowane automatycznie. Zobacz [Co nowego w aparat bazy danych: W pamięci OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) uzyskać więcej szczegółowych informacji.
- Tryb partii v/s zmiany trybu wiersza z indeksów magazynu kolumny
  - Sortowanie w tabeli z indeksem sklepu kolumny są teraz w trybie partię.
  - Obsługa okien agregacje teraz działać w trybie partii przykład instrukcji TSQL czasu ZWŁOKI i potencjalny klient.
  - Kwerendy dla kolumn ze sklepu tabele z wielu różnych klauzule działać w trybie partię.
  - Kwerendy uruchamianie w obszarze DOP = 1 lub plan szeregowego również wykonywanie w trybie partię.
- Ostatnia ulepszenia oszacowanie kardynalności rzeczywiście pochodzą z poziomu zgodności 120, ale dla osób, które działa na niższym poziomie zgodności (to znaczy 100 lub 110), Przenieś zgodności poziom 130 wywoła też następujące ulepszenia i te mogą również skorzystać wydajności kwerend aplikacji.


## <a name="practicing-compatibility-level-130"></a>Ćwiczenia poziom zgodności 130


Pierwszy korzystaj niektórych tabel, indeksy i dane losowe utworzono do przećwicz niektóre z tych nowych funkcji. Przykłady skryptów TSQL mogą być wykonywane w obszarze SQL Server 2016 lub bazy danych SQL Azure. Jednak podczas tworzenia bazy danych programu Azure SQL, upewnij się, że możesz wybrać co najmniej bazy danych P2 ponieważ potrzebny jest co najmniej kilka rdzeni, aby zezwolić na wiele wątków i dlatego skorzystać z tych funkcji.


```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Teraz Przyjrzyjmy się temu bliżej do niektórych funkcji przetwarzania kwerend, pochodzące z poziomu zgodności 130.


## <a name="parallel-insert"></a>Wstawianie równoległego


Wykonywanie poniższych instrukcji TSQL wykonuje operacji WSTAWIANIA w obszarze poziom zgodności 120 i 130, która jest odpowiednio wykonywana operacji WSTAWIANIA w jednym modelu wątków (120), a w modelu wielowątkowe (130).


```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Przez żądanie rzeczywistych planu kwerend na jej reprezentację lub jej zawartości XML, można określić, które oszacowanie kardynalności funkcji znajduje się na odtwarzania. Spojrzenie na plany side-by-side na rysunku 1, wyraźnie widać, że wykonanie Wstawianie sklepu kolumny pozwala przejść z szeregowego w 120 równolegle w 130. Należy również zauważyć, że zmiana ikony iteracyjne w planie 130 z dwiema strzałkami równoległe ilustrującymi fakt tego teraz wykonanie iteracyjne jest faktycznie równoległe. Jeśli masz duże operacje WSTAWIANIA, aby ukończyć równoległego powiązany z numerem podstawowego, do których masz do dyspozycji w bazie danych, będą działać lepiej; do 100 razy szybciej, w zależności od sytuacji!


*Rysunek 1: Operacji WSTAWIANIA zmienia się z szeregowego równolegle zgodności z poziomu 130.*


![Rysunek 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)


## <a name="serial-batch-mode"></a>Tryb wsadowy SZEREGOWEGO


Umożliwia przenoszenie poziom zgodności 130 podczas przetwarzania wierszy danych podobnie tryb przetwarzanie. Najpierw operacje trybu partii są dostępne tylko po umieszczeniu indeksu magazynu kolumny w odpowiednim miejscu. Drugi partię zazwyczaj reprezentuje ~ 900 wierszy i będzie używał logiki kodu, zoptymalizowana pod kątem wielordzeniowych Procesora, wyższej przepustowości pamięci i bezpośrednio wykorzystuje skompresowane dane w sklepie kolumny, o ile to możliwe. Pod tymi warunkami SQL Server 2016 można przetwarzać wierszy ~ 900 jednocześnie, zamiast wiersza 1 w czasie, i w konsekwencji koszty całkowite ogólnych działania jest udostępniany przez całość zmniejszania całkowity koszt według wierszy. Ten udostępnionego ilość połączonymi z kompresji sklepu kolumny zasadniczo zmniejsza opóźnienie biorących udział w operacji partii wybierz tryb. Można znaleźć więcej informacji o magazynie kolumny i partii tryb [Columnstore indeksy przewodnika](https://msdn.microsoft.com/library/gg492088.aspx).


```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

– The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Jako widoczne poniżej obserwując kwerendy plany obok siebie na rysunku 2, możemy wyświetlić, że tryb przetwarzania został zmieniony w poziom zgodności i w konsekwencji podczas wykonywania kwerend w obu poziom zgodności całkowicie widać, że większość czasu przetwarzania poświęca w trybie wiersza (86%) w porównaniu z trybu partii (14%), gdzie zostały przetworzone partie 2. Zwiększanie zestawu danych, zwiększa korzyści.


*Rysunek 2: Wybierz wprowadzone zmiany z szeregowego partii tryb zgodności poziomu 130.*


![Rysunek 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)


## <a name="batch-mode-on-sort-execution"></a>Tryb partii na wykonanie sortowania


Podobne do powyżej, ale zastosowanego do operacji sortowania przejścia z trybu wiersza (poziom zgodności 120) do trybu partii (poziom zgodności 130) zwiększa wydajność operacji sortowania z tych samych powodów.


```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Widoczne przez siebie na rysunku 3, są wyświetlane reprezentującą operacji sortowania w trybie wiersza — 81% kosztów, podczas tryb wsadowy odpowiada 19% kosztu (odpowiednio 81% i 56% na sortowanie się).


*Rysunek 3: Operacje sortowania zmienia się z wierszy do trybu partii z poziomu zgodności 130.*


![Rysunek 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)


Oczywiście te przykłady zawierają tylko dziesiątki tysięcy wierszy, które ma podczas przeglądania danych w większości serwerów SQL tych dni. Po prostu projektu tych przed miliony wierszy zamiast, a to można przetłumaczyć za kilka minut wykonanie oszczędza codziennie oczekujących rodzaj z pracą.


## <a name="cardinality-estimation-ce-improvements"></a>Poprawa oszacowania (CE) kardynalności


Programu Microsoft SQL Server 2014, wszystkie bazy danych, zainstalowany na poziomie zgodności 120 lub powyżej spowoduje, że nowe oceny kardynalności za pomocą funkcji. Szacowanie kardynalności to zasadniczo logiczny służące do ustalania, jak program SQL server będzie wykonywana kwerenda oparta na jego szacowany koszt. Oszacowanie jest obliczane za pomocą danych wejściowych z statystyki skojarzony z obiektami związane z tej kwerendy. W praktyce, na najwyższym poziomie oszacowanie kardynalności funkcje są szacuje liczba wierszy oraz informacje na temat rozpowszechniania wartości liczby wartości odrębnych i zduplikowane liczby zawarte w tabel i obiektów, do których odwołuje się zapytanie. Wprowadzenie tych szacuje problem, może powodować niepotrzebne dyskowej z powodu dotacji za mało pamięci (to znaczy tymczasowe wycieki) lub do zaznaczenia wykonywaniu szeregowego planu na wykonanie planu równoległego kilka. Zawarcia, niepoprawne oszacowania może prowadzić do obniżenie wydajności ogólnej wydajności wykonywania zapytania. Na drugiej stronie szacowania dokładniejszego szacuje prowadzi do lepiej wykonania kwerendy!

Wymienione przed optymalizacji zapytania i szacuje są złożone sprawy, ale jeśli chcesz dowiedzieć się więcej o planach kwerendy i studenckich kardynalności, można używać odwołań do dokumentu na [Optymalizowania Twoje plany kwerend z programu SQL Server 2014 kardynalności Studenckich](https://msdn.microsoft.com/library/dn673537.aspx) w celu szczegółowego dive.


## <a name="which-cardinality-estimation-do-you-currently-use"></a>Które oszacowania kardynalności aktualnie używasz?


Aby określić, w których oszacowanie kardynalności zapytań jest uruchomiony, po prostu używanie grafiki poniżej pokazano kwerendy. Zauważ, że w tym przykładzie pierwszy będzie uruchamiana poziom zgodności 110, co oznacza, użyj funkcji oszacowanie kardynalności stare.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Po zakończeniu wykonywania kliknij łącze XML, a następnie spójrz na właściwościach pierwszego iteracyjne, tak jak pokazano poniżej. Zanotuj nazwę właściwości o nazwie CardinalityEstimationModelVersion obecnie ustawiać 70. Nie oznacza to, że poziom zgodności bazy danych jest ustawiony na wersję programu SQL Server 7.0 (jest ustawiona na 110 jako widoczne w instrukcjach TSQL powyżej), ale wartość 70 po prostu reprezentuje starsze funkcje oszacowanie kardynalności dostępne od programu SQL Server 7.0, którego został nie głównych poprawki 2014 serwera SQL (instalowanego wraz z poziomu zgodności 120).


*Rysunek 4: CardinalityEstimationModelVersion jest równa 70 podczas korzystania z poziomu zgodności 110 lub poniżej.*


![Rysunek 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)


Można także zmienić poziom zgodności na 130 i wyłączyć użycie tej nowej funkcji oszacowanie kardynalności przy użyciu LEGACY_CARDINALITY_ESTIMATION ustawiona na ON [zmiany](https://msdn.microsoft.com/library/mt629158.aspx)konfiguracji występujące bazy danych. Są to dokładnie taki sam, jak przy użyciu 110 z oszacowanie kardynalności funkcji punktu widzenia, podczas korzystania z najnowszych przetwarzania poziom zgodności kwerend. Ten sposób można korzystać z nowej kwerendy przetwarzania funkcje pochodzące z najnowszych poziom zgodności (to znaczy w trybie partii), ale nadal korzystają z funkcji oszacowanie kardynalności stare, jeśli to konieczne.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Po prostu przenoszenie do poziomu zgodności 120 lub 130 udostępnia nowe funkcje oszacowanie kardynalności. W takim przypadku domyślnego CardinalityEstimationModelVersion zostanie ustawiony odpowiednio do 120 lub 130 jako widoczne poniżej.


```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Rysunek 5: CardinalityEstimationModelVersion ustawiono 130 podczas korzystania z poziomu zgodności 130.*


![Rysunek 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)


## <a name="witnessing-the-cardinality-estimation-differences"></a>Będący świadkiem różnice kardynalności oszacowania


Teraz Przyjrzyjmy Uruchom nieco bardziej złożone kwerendy dotyczące INNER JOIN z klauzuli WHERE przy niektórych orzeczenia i Przyjrzyjmy się oszacowanie liczba wierszy z stare funkcji oszacowanie kardynalności najpierw.


```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Skuteczne wykonywanie tej kwerendy zwraca 200,704 wierszy, gdy oszacowanie wiersza z funkcją szacowania kardynalności stare roszczeń 194,284 wierszy. Oczywiście jak już wspomniano, przed, te wyniki liczba wierszy również zależy od częstotliwości uruchomiono poprzedniego próbkach, które wypełnia Przykładowe tabele wielokrotnie przy każdym uruchomieniu. Oczywiście orzeczenia w kwerendzie także będzie mieć wpływ na rzeczywiste oszacowania poza kształt stołu danych zawartości, i jak te dane są faktyczną przeniesionym ze sobą.


*Rysunek 6: Oszacowanie liczba wierszy jest 194,284 lub 6000 wierszy wyłączone z wierszy 200,704 poprawnie.*


![Rysunek 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)


W ten sam sposób Przejdźmy teraz wykonać tę samą kwerendę z nowych funkcji oszacowanie kardynalności.


```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Spojrzenie na poniżej teraz zobaczymy, że oszacowanie wiersza jest 202,877, lub znacznie dokładniejsze i wyższymi niż stare oszacowania kardynalności.

*Rysunek 7: Oszacowanie liczba wierszy jest teraz 202,877 zamiast 194,284.*


![Rysunek 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)


W rzeczywistości zestaw wyników jest 200,704 wierszy (ale wszystkie zależy, jak często zostało uruchomione kwerendy wcześniejszych próbek, ale co ważne, ponieważ TSQL korzysta instrukcję RAND(), rzeczywistych wartości zwracane mogą się różnić w jednym Uruchom do następnego). W związku z tym w tym przykładzie określonego nowego oszacowania kardynalności ma lepsze zadania na szacowanie liczbę wierszy, ponieważ 202,877 jest o wiele bliżej 200704, niż 194,284! Nazwisko, jeśli zmienisz klauzuli WHERE orzeczenia równości (zamiast ">" na przykład), to może spowodować szacuje między starą i nową funkcję kardynalności bardziej różne w zależności od tego, ile odpowiada będą mogły uzyskać dostęp.

Oczywiście w tym przypadku jest wierszy ~ 6000 wyłączone z rzeczywistej liczby nie reprezentuje wiele danych w niektórych sytuacjach. Teraz Funkcja TRANSPONUJ, które tę opcję, aby miliony wierszy w kilku tabel i kwerend bardziej złożone i w czasie oszacowanie można wyłączyć przez miliony wierszy, a więc ryzyko pobrania up plan wykonania problem lub żąda dotacji za mało pamięci prowadzące do zalania tymczasowe, a więc więcej we/wy, są znacznie wyższe.

Jeśli masz szansy sprzedaży, ćwiczenia porównania z najbardziej typowych kwerend i zestawów danych i wyświetlać dla siebie, jaka część szacuje stare i nowe występuje, podczas niektórych tylko przestanie więcej poza w rzeczywistości lub niektóre inne osoby wystarczy po prostu bliżej wiersz rzeczywista zlicza rzeczywiście zwrócona w zestawach wyników. Wszystkie zależy od kształtu o kwerendach, właściwości bazy danych Azure SQL, rodzaj i rozmiar usługi zestawy danych i statystyki dostępne o nich. Jeśli utworzono wystąpienia bazy danych SQL Azure Optymalizator kwerend będzie trzeba utworzyć jej wiedzy od podstaw zamiast ponowne używanie statystyki z poprzedniego zostanie uruchomiona kwerenda. Tak oszacowania są bardzo kontekstowe i niemal specyficzne dla każdej sytuacji serwera i aplikacji. Jest ważnym aspektem należy pamiętać!


## <a name="some-considerations-to-take-into-account"></a>Zagadnienia do uwzględnienia


Mimo że większość obciążenia będzie korzystać z poziomu zgodności 130, przed przyjęcia poziom zgodności dla środowisku produkcyjnym problem masz 3 opcje:

1. Przenoszenie do poziomu zgodności 130 i zobacz, jak wykonywać czynności. W przypadku, gdy okaże się kilka strat, wystarczy po prostu ustawiania zgodność poziomu powrót do poziomu oryginalnego albo zachować 130 i tylko odwrotnej oszacowania kardynalności z powrotem do trybu starsze (jak wyjaśniono powyżej, to samodzielnie może rozwiązać problem z).
2. Dokładnie przetestuj istniejące aplikacje obciążeniu produkcji podobne, precyzyjnie dostosować i sprawdź poprawność wydajności przed przejściem do produkcji. W przypadku problemów tak samo jak powyżej, można zawsze powrócić do oryginalnego poziomu zgodności, lub po prostu odwracanie szacowania kardynalności powrócić do trybu starsze.
3. Jako wersję ostateczną opcja, a ostatnio sposobem rozwiązania tych pytań należy korzystać z magazynu kwerend. To zalecana opcja dzisiejszą! Ułatwiających analizę zapytań w obszarze zgodności poziomu 120 lub poniżej i 130, nie zachęcamy wystarczająco do używania magazynu kwerendy. Magazyn kwerendy jest dostępne z najnowszą wersją bazy danych SQL Azure w wersji 12, a jego opracowano z myślą ułatwiające rozwiązywanie problemów z wydajnością kwerend. W sklepie kwerendy można traktować jako rejestrator danych lotu zbierania i prezentowanie szczegółowe informacje historyczne o wszystkie kwerendy bazy danych. To znacznie upraszcza forensics wydajności przez zmniejszenie czasu do diagnozowania i rozwiązywania problemów. Można znaleźć więcej informacji na [kwerendy ze Sklepu: Rejestrator danych lotu dla bazy danych](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).


At wysokiego poziomu, jeśli masz już zbiór bazy danych działają na poziomie zgodności 120 lub poniżej oraz planuje przenieść niektóre z nich do 130 lub ponieważ z pracą automatycznie dodawać nowych baz danych, które mają być szybko ustawić domyślnie 130, należy rozważyć następujących typów:

- Przed zmianą na nowy poziom zgodności produkcji, Włącz magazyn kwerendy. Aby uzyskać więcej informacji może dotyczyć [zmienić tryb zgodności bazy danych i korzystania z pamięci kwerendy](https://msdn.microsoft.com/library/bb895281.aspx) .
- Następnie przetestuj wszystkie krytyczne obciążenia przy użyciu przedstawiciela danych i kwerendy środowisku podobnym produkcji i porównaj wydajności wystąpił i zgodnie z raportem w sklepie kwerendy. Występują pewne strat, można zidentyfikować regressed kwerend ze sklepem kwerendy i używać plan wymuszania opcja ze sklepu kwerendy (czyli planowanie przypinanie). W takim przypadku możesz ostatecznie pozostanie z poziomu zgodności 130 i użyć planu kwerend byłego sugerowane przez sklep kwerendy.
- Jeśli chcesz korzystać z nowych funkcji i możliwości bazy danych SQL Azure (który jest uruchomiony program SQL Server 2016), ale rozróżniają zmian wprowadzonych przez poziom zgodności 130, ostatecznym, można rozważyć wymuszania poziom zgodności z powrotem do poziomu, który pasuje do obciążenie pracą przy użyciu instrukcji ALTER DATABASE. Ale najpierw należy pamiętać, że plan magazynu kwerendy przypinanie opcja jest usługi najlepszym rozwiązaniem, ponieważ nie używa 130 przebywa zasadniczo na poziomie funkcjonalności ze starszej wersji programu SQL Server.
- Jeśli masz aplikacje multitenant obejmujące wiele baz danych, może być konieczne aktualizowanie logiki obsługi administracyjnej baz danych do zapewnienia poziomu zgodności spójne w całej wszystkich baz danych; stare i nowo ustanawianie z nich. Wydajność obciążenie pracą aplikacji może być poufne fakt, że niektóre bazy danych są uruchomione na zgodności różnych poziomach i w związku z tym, zgodności poziomu spójności w dowolnej bazy danych może być wymagane w celu zapewnienia obsługi samej klientom wszystkie w tablicy. Należy zauważyć, że nie jest upoważnienia, to naprawdę zależy od rodzaju wpływu poziom zgodności aplikacji.
- Ostatnia dotyczące oszacowania kardynalności oraz tak jak zmiana poziomu zgodności, przed przejściem do produkcji, zaleca się testowanie z pracą produkcji warunkach nowe ustalenie, jeśli aplikacja korzysta z ulepszeń oszacowanie kardynalności.


## <a name="conclusion"></a>Wnioski


Za pomocą bazy danych SQL Azure do korzystania z wszystkie ulepszenia programu SQL Server 2016 wyraźnie poprawić do wykonania kwerendy. Podobnie jak-jest! Oczywiście takich jak nowej funkcji prawidłowej oceny trzeba zrobić, aby określić dokładne warunki, na jakich obciążenie pracą z bazy danych działa najlepiej. Doświadczenie wskazuje, że większość obciążenie pracą mają co najmniej uruchamiana przezroczysty poziom zgodności 130, pracownikom nowe zapytanie przetwarzania funkcje i nowe oszacowanie kardynalności. Będącej said praktyce, są zawsze niektóre wyjątki i wykonując pisane z wielkiej litery ukończenia starannością ocenę ważne, aby ustalić, ile mogą korzystać z tych usprawnień. I ponownie sklepu kwerendy może być doskonałe pomocy w tym tej pracy!

Jak rozwoju platformy SQL Azure, można się spodziewać poziom zgodności 140 w przyszłości. Gdy jest godzina, firma Microsoft rozpocznie się mówić co ten poziom zgodności przyszłych 140 wywoła, tak jak możemy krótko omawiane poziom zgodności 130 jest dostosowanie dzisiaj.

Na obecnym Przejdźmy nie zapomnisz, począwszy od czerwca 2016, bazy danych SQL Azure zmieni się domyślny poziom zgodności z 120 do 130 dla nowo utworzonego baz danych. Należy pamiętać!


## <a name="references"></a>Odwołania


- [Co nowego w aparat bazy danych](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)

- [Blog: Kwerendy Sklepu: Rejestrator danych lotu dla bazy danych, przez Borko Novakovic, 2016 8 czerwca](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)

- [ZMIEŃ poziom zgodności bazy danych (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)

- [ZMIANY ZAKRESU KONFIGURACJI BAZY DANYCH](https://msdn.microsoft.com/library/mt629158.aspx)

- [Poziom zgodności 130 dla wersji 12 bazy danych Azure SQL](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)

- [Optymalizacja zapytania plany z programem SQL Server Studenckich kardynalności 2014 r.](https://msdn.microsoft.com/library/dn673537.aspx)

- [Przewodnik indeksy Columnstore](https://msdn.microsoft.com/library/gg492088.aspx)

- [Blog: Ulepszone kwerendy wydajności za pomocą poziom zgodności 130 w bazie danych Azure SQL, przez Alain Lissoir 2016 6 maja](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)



<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
