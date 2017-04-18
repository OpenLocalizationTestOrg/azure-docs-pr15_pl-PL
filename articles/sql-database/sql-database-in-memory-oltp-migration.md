<properties
    pageTitle="W pamięci OLTP poprawia wydajności txn SQL | Microsoft Azure"
    description="Przyjąć OLTP w pamięci, aby zwiększyć wydajność transakcji w istniejącej bazy danych SQL."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor="MightyPen"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="use-in-memory-oltp-preview-to-improve-your-application-performance-in-sql-database"></a>Używanie w pamięci OLTP (wersja preview) aby poprawić wydajność aplikacji w bazie danych SQL

[OLTP w pamięci](sql-database-in-memory.md) , można zwiększyć wydajność programu OLTP obciążenie pracą w bazach danych programu SQL Azure [Premium](sql-database-service-tiers.md) bez zwiększania poziom wydajności.

Wykonaj poniższe czynności podjęcie OLTP w pamięci w istniejącej bazie danych.

## <a name="step-1-ensure-your-premium-database-supports-in-memory-oltp"></a>Krok 1: Upewnij się, że baza danych Premium obsługuje OLTP w pamięci

Premium baz danych utworzonych w 2015 listopada lub nowszy obsługuje funkcję w pamięci. Aby ustalić, czy bazę danych Premium obsługuje funkcję w pamięci, uruchamiając następujące instrukcji Transact-SQL. W pamięci jest obsługiwana, gdy jest zwracany wynik 1 (nie 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* oznacza *Skrajnych przetwarzania transakcji*

Jeśli istniejącej bazy danych należy przenieść do nowej bazy danych w wersji 12 Premium, możesz za pomocą następujące techniki wyeksportować, a następnie zaimportować dane.

#### <a name="export-steps"></a>Kroki eksportowania

Eksportowanie bazy danych produkcji do Plecak przy użyciu:

- Funkcja [eksportu](sql-database-export.md) w [portalu](https://portal.azure.com/).

- Funkcję **Aplikacja warstwy danych eksportu** w [aktualne SSMS.exe](http://msdn.microsoft.com/library/mt238290.aspx) (program SQL Server Management Studio).
 1. W **Eksploratorze obiektów**rozwiń węzeł **bazy danych** .
 2. Kliknij prawym przyciskiem myszy węzeł bazy danych.
 3. Kliknij pozycję **zadania** > **eksportowania aplikacji warstwy danych**.
 4. Działać wyświetlone okno kreatora.


#### <a name="import-steps"></a>Kroki importowania

Importowanie Plecak do nowej bazy danych Premium.

1. W Azure [portalu](https://portal.azure.com/)
 - Przejdź na serwerze.
 - Wybierz opcję [Importuj bazę danych](sql-database-import.md) .
 - Wybierz pozycję Premium ceny warstwy.

2. Umożliwia importowanie Plecak SSMS:
 - W **Eksploratorze obiektów**kliknij prawym przyciskiem myszy węzeł **bazy danych** .
 - Kliknij przycisk **Importuj warstwy danych aplikacji**.
 - Działać wyświetlone okno kreatora.


## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Krok 2: Identyfikowanie obiektów, aby przeprowadzić migrację do OLTP w pamięci

SSMS zawiera **Omówienie analizy wydajność transakcji** raportu, który może zostać uruchomiony w bazie danych z pracą aktywne. Raport służy do identyfikowania tabel i procedur składowanych, które są kandydatami do migracji do OLTP w pamięci.

W SSMS, aby wygenerować raport:
- W **Eksploratorze obiektów**kliknij prawym przyciskiem myszy węzeł bazy danych.
- Kliknij pozycję **Raporty** > **Raporty standardowe** > **Omówienie analizy wydajność transakcji**.

Aby uzyskać więcej informacji zobacz [Określanie tabela lub przechowywane procedury powinny być przenoszone do OLTP w pamięci](http://msdn.microsoft.com/library/dn205133.aspx).


## <a name="step-3-create-a-comparable-test-database"></a>Krok 3: Tworzenie porównywalna testowej bazy danych

Załóżmy, że raport wskazuje, że baza danych zawiera tabelę, która będzie korzystać z zostanie przekonwertowany na tabelę zoptymalizowane pamięci. Zaleca się najpierw przetestowanie o potwierdzenie oznaczenie, testując.

Potrzebujesz kopię produkcyjnej bazy danych. Na tym samym poziomie Warstwa usługi baza danych programu produkcji musi być testowej bazy danych.

Aby ułatwić testowanie, dostosować a test bazy danych w następujący sposób:

1. Łączenie z bazą danych testowych za pomocą SSMS.

2. Aby uniknąć konieczności opcja z (MIGAWKĘ) w kwerendach, ustaw dla opcji bazy danych, jak pokazano w poniższej instrukcji T SQL:
```
ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```


## <a name="step-4-migrate-tables"></a>Krok 4: Migrowanie tabel

Należy utworzyć i wypełnić zoptymalizowane pamięci kopii tabeli, która ma zostać sprawdzona. Można go utworzyć przy użyciu:

- Przydatne Kreator optymalizacji pamięci w SSMS.
- Ręczne T-SQL.


#### <a name="memory-optimization-wizard-in-ssms"></a>Kreator optymalizacji pamięci w SSMS

Aby użyć tej opcji migracji:

1. Połączenia z bazą danych test z SSMS.

2. W **Eksploratorze obiektów**kliknij prawym przyciskiem myszy tabelę, a następnie kliknij **Advisor optymalizacji pamięci**.
 - Zostanie wyświetlony Kreator **Advisor optymalizatora pamięci tabeli** .

3. W kreatorze kliknij pozycję **sprawdzania poprawności migracji** (lub przycisk **Dalej** ), aby wyświetlić, jeśli tabela zawiera nieobsługiwane funkcje, które nie są obsługiwane w tabelach zoptymalizowane pamięci. Aby uzyskać więcej informacji zobacz:
 - *Lista kontrolna optymalizacji pamięci* w [Advisor optymalizacji pamięci](http://msdn.microsoft.com/library/dn284308.aspx).
 - [Nieobsługiwane przez OLTP w pamięci konstrukcji transact-SQL](http://msdn.microsoft.com/library/dn246937.aspx).
 - [Migrowanie do OLTP w pamięci](http://msdn.microsoft.com/library/dn247639.aspx).

4. Jeśli tabela zawiera nie nieobsługiwanych funkcji, advisor można wykonywać schematu rzeczywiste i migracji danych dla Ciebie.


#### <a name="manual-t-sql"></a>Ręczne T-SQL

Aby użyć tej opcji migracji:

1. Połączenia z bazą danych test przy użyciu SSMS (lub narzędzie podobne).

2. Uzyskanie kompletny skrypt T-SQL dla tabeli i jego indeksy.
 - W SSMS kliknij prawym przyciskiem myszy węzeł tabeli.
 - Kliknij **tabelę skrypt jako** > **Utwórz, aby** > **nowe okno kwerendy**.

3. W oknie Skrypt Dodaj z (MEMORY_OPTIMIZED = ON) do instrukcji CREATE TABLE.

4. W przypadku indeks GRUPOWANY, można ją zmienić na NONCLUSTERED.

5. Zmień nazwę istniejącej tabeli za pomocą SP_RENAME.

6. Utwórz nową kopię zoptymalizowane pamięci tabeli, uruchamiając skrypt edytowany CREATE TABLE.

7. Skopiuj dane do tabeli zoptymalizowane pamięci za pomocą Wstawianie... WYBIERZ POZYCJĘ * DO:
    
```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Krok 5 (opcjonalnie): Migrowanie procedur składowanych

Funkcja w pamięci można także modyfikować procedura składowana zwiększyć wydajność.


### <a name="considerations-with-natively-compiled-stored-procedures"></a>Zagadnienia dotyczące z oryginalnie skompilowany procedur składowanych

Oryginalnie skompilowany procedura składowana może zawierać następujące opcje na jego klauzula T SQL z:

- NATIVE_COMPILATION

- SCHEMABINDING: co oznacza tabele mające procedura składowana nie można ich definicje kolumny zmieniane w żadnym wpłynie procedura składowana, chyba że upuść procedury składowanej.


Natywne moduł, należy użyć jeden duży [Atomowej bloków konstrukcyjnych](http://msdn.microsoft.com/library/dn452281.aspx) do zarządzania transakcji. Istnieje żadnej roli jawne transakcji rozpocząć lub WYCOFYWANIA transakcji. Jeśli kod wykryje naruszenie reguły biznesowej, go zakończyć Atomowej bloku instrukcją [zgłosić](http://msdn.microsoft.com/library/ee677615.aspx) .


### <a name="typical-create-procedure-for-natively-compiled"></a>Typowe CREATE PROCEDURE dla oryginalnie skompilowany.

Zazwyczaj T-SQL, aby utworzyć oryginalnie skompilowany procedura składowana jest podobny do następującego szablonu:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

- TRANSACTION_ISOLATION_LEVEL MIGAWKI w przypadku wartość najczęściej występującą oryginalnie skompilowany procedury przechowywanej. Podzestaw innych wartości również są obsługiwane:
 - PRZECZYTAJ POWTARZALNYCH
 - MOŻNA


- Wartość języka musi istnieć w widoku sys.languages.


### <a name="how-to-migrate-a-stored-procedure"></a>Jak przeprowadzić migrację procedura składowana

Kroki migracji są:


1. Uzyskać skrypt CREATE PROCEDURE do zwykłego interpretowany procedura składowana.

2. Ponownie zapisać jej nagłówek zgodnie z poprzedniego szablonu.

3. Należy upewnić się, czy procedura składowana kod T SQL używa wszystkich funkcji, które nie są obsługiwane przez ten skompilowany procedur składowanych. Wdrożenie rozwiązania problemu, jeśli to konieczne.
 - Aby uzyskać szczegółowe informacje, zobacz [Zagadnienia migracji dla oryginalnie skompilowany na procedur składowanych](http://msdn.microsoft.com/library/dn296678.aspx).

4. Zmień nazwę starych procedura składowana przy użyciu SP_RENAME. Lub po prostu ZLIKWIDUJ je.

5. Uruchom skrypt edytowany Tworzenie procedury T-SQL.


## <a name="step-6-run-your-workload-in-test"></a>Krok 6: Uruchamianie usługi Obciążenie pracą w badaniu

Uruchom obciążenie pracą w bazie danych programu test jest podobne do obciążenie pracą, uruchamianego w bazie danych produkcji. To okaże się wzmocnienia wydajności osiągnąć korzystanie z funkcji pamięci w tabelach i procedur składowanych.

Główne atrybuty obciążenie pracą:

- Liczba połączeń jednocześnie.

- Stosunek do odczytu/zapisu.


Aby dostosować i uruchomić obciążenie pracą test, warto rozważyć użycie narzędzia przydatne ostress.exe, które przedstawione w [tym miejscu](sql-database-in-memory.md).


Aby zminimalizować opóźnień sieci, testu z tego samego Azure regionu geograficznego, miejsce, w którym istnieje bazy danych.


## <a name="step-7-post-implementation-monitoring"></a>Krok 7: Monitorowanie po wdrożeniu

Należy rozważyć, czy monitorowanie efekty wydajności usługi implementacji w pamięci produkcji:

- [Monitorowanie przechowywania w pamięci](sql-database-in-memory-oltp-monitoring.md).

- [Monitorowanie korzystanie z widoków dynamiczne zarządzanie bazą danych SQL Azure](sql-database-monitoring-with-dmvs.md)


## <a name="related-links"></a>Łącza pokrewne

- [OLTP (Optymalizacja w pamięci) w pamięci](http://msdn.microsoft.com/library/dn133186.aspx)

- [Wprowadzenie do oryginalnie skompilowany procedur składowanych](http://msdn.microsoft.com/library/dn133184.aspx)

- [Advisor optymalizacji pamięci](http://msdn.microsoft.com/library/dn284308.aspx)

