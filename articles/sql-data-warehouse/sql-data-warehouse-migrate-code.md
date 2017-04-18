<properties
   pageTitle="Migrowanie kodu SQL do magazynu danych SQL | Microsoft Azure"
   description="Porady dotyczące migracja kodu SQL do magazynu danych SQL Azure dla opracowania rozwiązań."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/02/2016"
   ms.author="lodipalm;barbkess;sonyama;jrj"/>

# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>Migrowanie kodu SQL do magazynu danych SQL

Podczas migracji kodu z innej bazy danych do magazynu danych SQL, najprawdopodobniej będzie konieczne wprowadzanie zmian w kodzie. Jak są one przeznaczone do pracy w sposób rozłożone niektóre funkcje SQL Data Warehouse znacznie zwiększyć wydajność. Jednak aby zachować wydajność i skali, niektóre funkcje również nie są dostępne.

## <a name="common-t-sql-limitations"></a>Typowe ograniczenia T-SQL

Poniżej przedstawiono najczęściej używanych funkcji, które nie są obsługiwane w magazynie danych SQL Azure. Łącza prowadzą do obejścia dla funkcji nieobsługiwanych:

- [Sprzężenia ANSI na aktualizacje][]
- [Sprzężenia ANSI na usuwa][]
- [Instrukcja korespondencji seryjnej][]
- sprzężenia między bazami danych
- [kursory][]
- [WYBIERZ POZYCJĘ. DO][]
- [WSTAWIANIE. WYKONYWALNA][]
- Klauzula OUTPUT
- Funkcje zdefiniowane przez użytkownika w tekście
- funkcje wielu instrukcji
- [Typowe wyrażenia tabeli](#Common-table-expressions)
- [cykliczne typowych wyrażenia tabeli (CTE)] (#Recursive-common-table-expressions-(CTE)
- Funkcje CLR i procedury
- Funkcja $partition
- zmienne tabeli
- Tabela wartości parametrów
- Transakcje rozproszone
- Zatwierdź / wycofywania pracy
- Zapisywanie transakcji
- wykonanie konteksty (EXECUTE AS)
- [Klauzula z funkcją rollup group by / modułu / zestawy opcje grupowania][]
- [Liczba poziomów zagnieżdżenia poza 8][]
- [aktualizowanie za pomocą widoków][]
- [Korzystanie z zaznacz dla zmiennych przydziałów][]
- [Typ danych nie MAX dynamiczne ciągów SQL][]

Na szczęście większość ograniczenia te mogą wykonywać wokół. Objaśnienia znajdują się w artykułach odpowiednich zmianach powyżej.

## <a name="supported-cte-features"></a>Obsługiwane funkcje CTE

Typowe wyrażenia tabeli (wyrażeń CTE) jest częściowo obsługiwane w magazynie danych SQL.  Obecnie obsługiwane są następujące funkcje CTE:

- CTE można określić w instrukcji SELECT.
- CTE można określić w instrukcji CREATE VIEW.
- CTE można określić w instrukcji tworzenia tabeli jako wybierz (CTAS).
- CTE można określić w instrukcji Tworzenie zdalnego tabeli jako wybierz (CRTAS).
- CTE można określić w instrukcji Tworzenie zewnętrznych tabeli jako wybierz (CETAS).
- Tabeli zdalnej odwoływać się ze CTE.
- Tabeli zewnętrznej można odwoływać się ze CTE.
- Wiele definicji zapytania CTE można zdefiniować w CTE.

## <a name="cte-limitations"></a>Ograniczenia CTE

Typowe wyrażenia tabeli mają pewne ograniczenia z magazynu danych SQL, w tym:

- CTE należy przestrzegać pojedynczej instrukcji SELECT. Usuń INSERT, UPDATE i instrukcje korespondencji seryjnej nie są obsługiwane.
- Wspólne wyrażenie tabeli, zawierająca odwołania do siebie (cykliczne typowych wyrażenie tabeli) nie jest obsługiwane (patrz poniżej).
- Określanie więcej niż jedna ZAWIERAJĄCA klauzulę w CTE nie jest dozwolone. Na przykład jeśli CTE_query_definition zawiera podkwerendy, tym podkwerendy nie może zawierać zagnieżdżonej ZAWIERAJĄCA klauzulę, która definiuje CTE innego.
- Klauzula ORDER BY nie można używać w CTE_query_definition, z wyjątkiem, gdy określono klauzulę TOP.
- Gdy CTE jest używana w instrukcji, który jest częścią serii, należy przestrzegać instrukcji przed nim średnikami.
- Użycie w instrukcjach Autor sp_prepare wyrażeń CTE będzie działać tak samo jak inne instrukcji SELECT w PDW. Jednak jeśli wyrażeń CTE zostaną użyte jako część CETAS Autor sp_prepare, zachowanie można odłożyć z programu SQL Server i innych instrukcji PDW ze względu na sposób powiązania jest zaimplementowana dla sp_prepare. Wybierz czy odwołania CTE używa niewłaściwej kolumnie, która nie istnieje w CTE sp_prepare przejdzie bez wykrycia błędów, ale komunikat o błędzie będzie wyjątek podczas sp_execute zamiast tego.

## <a name="recursive-ctes"></a>Wyrażeń CTE cykliczne

Cyklicznych wyrażeń CTE nie są obsługiwane w magazynie danych SQL.  Migraion: cykliczne CTE może być nieco ukończone i proces zalecane jest aby podzielić na wiele kroków. Zwykle można skorzystać z pętli i wypełnianie tymczasowej tabeli jako iteracyjne kwerendy pośrednie cykliczne. Po tabeli tymczasowej jest wypełniana możesz następnie zwrócenie danych przez zestaw pojedynczy wynik. Podobne podejście został użyty do rozwiązania `GROUP BY WITH CUBE` w artykule [Klauzula z funkcją rollup group by / modułu / zestawy opcje grupowania][] .

## <a name="unsupported-system-functions"></a>Funkcje nieobsługiwany system

Istnieją również niektóre funkcje systemowe, które nie są obsługiwane. Główny te, które zwykle mogą okazać się używanych w danych składu należą:

- NEWSEQUENTIALID()
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE() —

Niektóre z tych problemów można pracować wokół.

## <a name="rowcount-workaround"></a>@@ROWCOUNTObejście

Aby obejść Brak obsługi @@ROWCOUNT, tworzenie pobierania ostatniej liczba wierszy z sys.dm_pdw_request_steps, a następnie wykonaj procedury przechowywanej `EXEC LastRowCount` po instrukcji DML.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Następne kroki
Aby uzyskać pełną listę wszystkich obsługiwanych instrukcji T SQL zobacz [Tematy języku Transact-SQL][].

<!--Image references-->

<!--Article references-->
[Sprzężenia ANSI na aktualizacje]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Sprzężenia ANSI na usuwa]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[Instrukcja korespondencji seryjnej]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[WSTAWIANIE. WYKONYWALNA]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Tematy Transact-SQL]: ./sql-data-warehouse-reference-tsql-statements.md

[kursorów]: ./sql-data-warehouse-develop-loops.md
[WYBIERZ POZYCJĘ. DO]: ./sql-data-warehouse-develop-ctas.md#selectinto
[Klauzula z funkcją rollup group by / modułu / zestawy opcje grupowania]: ./sql-data-warehouse-develop-group-by-options.md
[Liczba poziomów zagnieżdżenia poza 8]: ./sql-data-warehouse-develop-transactions.md
[aktualizowanie za pomocą widoków]: ./sql-data-warehouse-develop-views.md
[Korzystanie z zaznacz dla zmiennych przydziałów]: ./sql-data-warehouse-develop-variable-assignment.md
[Typ danych nie MAX dynamiczne ciągów SQL]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
