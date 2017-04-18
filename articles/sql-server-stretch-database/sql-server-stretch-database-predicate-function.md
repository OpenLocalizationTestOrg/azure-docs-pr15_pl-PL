<properties
    pageTitle="Zaznacz wiersze, aby przeprowadzić migrację przy użyciu funkcji Filtr (baza danych rozciąganie) | Microsoft Azure"
    description="Dowiedz się, jak zaznacz wiersze, aby przeprowadzić migrację przy użyciu funkcji Filtr."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="douglasl"/>

# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Zaznacz wiersze, aby przeprowadzić migrację przy użyciu funkcji Filtr (rozciąganie bazy danych)

Jeśli zimnej dane są przechowywane w osobnej tabeli, można skonfigurować rozciąganie bazy danych do migrowania całej tabeli. Jeśli tabela zawiera zarówno ciepłej i zimnej danych, z drugiej strony, możesz określić funkcję Filtr zaznacz wiersze, aby przeprowadzić migrację. Predykat filtru jest tabelą w tekście\-wartości funkcji. W tym temacie opisano, jak zapisać tablicę w tekście\-wartości funkcji wybierz wiersze do migrowania.

>   [AZURE.NOTE] Jeśli podasz funkcji filtru wykonującego źle migracji danych również wykonuje źle. Rozciąganie bazy danych dotyczy funkcja filter tabeli za pomocą operatora krzyżowe stosowanie.

Jeśli nie określisz funkcji filtru migracji całej tabeli.

Po uruchomieniu bazy danych włączyć rozciąganie kreatora, możesz przeprowadzić migrację całej tabeli lub funkcji prosty filtr można określić w kreatorze. Jeśli chcesz użyć innego typu funkcja filter wybierz wiersze do migracji, wykonaj jedną z następujących czynności:

-   Zamknij kreatora i uruchom instrukcja ALTER TABLE, aby włączyć rozciąganie dla tabeli i określanie funkcji filtru.

-   Uruchom instrukcja ALTER TABLE, aby określić funkcję filtru po zakończeniu kreatora.

Dodawanie funkcji Składnia ALTER TABLE opisano w dalszej części tego tematu.

## <a name="basic-requirements-for-the-filter-function"></a>Podstawowe wymagania dotyczące funkcji filtrowania
W tabeli w tekście\-wartości funkcji wymagane dla predykatu filtru rozciąganie bazy danych wygląda w poniższym przykładzie.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE <predicate>
```
Parametry funkcji muszą być identyfikatory kolumn z tabeli.

Aby zablokować kolumny, które są używane przez funkcję filtru z zostanie usunięty lub zmieniona jest wymagane powiązanie schematu.

### <a name="return-value"></a>Wartość zwracana
Jeśli funkcja zwraca prawidłową\-wyniku, wiersz jest uprawniony do migracji. W przeciwnym razie \- oznacza to, że jeśli funkcja nie zwraca wynik \- wiersz nie kwalifikuje się do migracji.

### <a name="conditions"></a>Warunki
&lt; *Predykat* &gt; mogą zawierać jeden warunek lub wielu warunków z operatorów logicznych AND.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Każdego warunku z kolei może się składać jeden warunek pierwotny lub wielu warunków pierwotny połączone z operatorów logicznych lub.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### <a name="primitive-conditions"></a>Pierwotny warunków
Pierwotny warunek wykonaj jedną z następujących porównania.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Porównanie funkcji parametr wyrażenie stałe. Na przykład `@column1 < 1000`.

    Oto przykładowy sprawdza, czy wartość kolumny *Data* jest &lt; 2016-1-1.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Operator IS NULL lub nie jest wartość zerowa zastosować parametr funkcji.

-   Porównywanie parametr funkcji listy wartości stałych przy użyciu operatora IN.

    Oto przykładowy sprawdza, czy wartość *wysyłek\_stanu* kolumna jest `IN (N'Completed', N'Returned', N'Cancelled')`.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

### <a name="comparison-operators"></a>Operatory porównania
Obsługiwane są następujące operatory porównania.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### <a name="constant-expressions"></a>Stałych wyrażeń
Stałe, używane w funkcji filtru może być dowolne wyrażenie deterministyczne, które może przyjąć podczas definiowania funkcji. Stałych wyrażeń może zawierać następujące czynności.

-   Literałów. Na przykład `N’abc’, 123`.

-   Algebraicznych. Na przykład `123 + 456`.

-   Funkcje deterministyczne. Na przykład `SQRT(900)`.

-   Konwersje deterministyczne korzystające z CAST lub Konwertuj. Na przykład `CONVERT(datetime, '1/1/2016', 101)`.

### <a name="other-expressions"></a>Inne wyrażenia
Jeśli funkcja wyniku spełnia reguły opisanych tutaj po zamianie BETWEEN, a nie między podmiotami z wyrażeń równoważne oraz i lub można użyć BETWEEN i nie BETWEEN operatorów.

Nie można za pomocą podkwerend lub innych niż\-deterministyczne funkcje, takie jak RAND() lub GETDATE().

## <a name="add-a-filter-function-to-a-table"></a>Dodawanie funkcji filtru do tabeli
Dodawanie funkcji filtru do tabeli, uruchamiając instrukcja **ALTER TABLE** oraz określając istniejącej tabeli w tekście\-wartości funkcji jako wartość **filtru\_PREDYKATU** parametru. Na przykład:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
) )
```
Po powiąże funkcję do tabeli jako predykatu, następujące czynności są spełnione.

-   Występuje następnej migracji danych czasu, tylko te wiersze, dla których funkcja zwraca prawidłową\-są migrowane wartości pustej.

-   Kolumny używane przez funkcję są powiązana ze schematem. Nie można zmienić tych kolumn, jak tabela używa funkcji jako jego predykat filtru.

Nie można usunąć tabelę w tekście\-wartości funkcji, ile tabeli jest używanie funkcji jako jego predykat filtru.

>   [AZURE.NOTE] Aby zwiększyć wydajność funkcji filtrowania, utworzyć indeks kolumny używane przez funkcję.

### <a name="passing-column-names-to-the-filter-function"></a>Przekazywanie nazw kolumn do funkcja filter
Po przypisaniu funkcji filtru do tabeli określ nazwy kolumn przekazane do funkcji filtrowania pod nazwą jednej części. Jeśli użytkownik określi nazwę trzyczęściowe podczas przekazywania kolumnie nazwy kolejnych kwerend dla rozciąganie\-enabled tabeli zakończy się niepowodzeniem.

Na przykład jeśli określisz nazwę kolumny trzyczęściowe, jak pokazano w poniższym przykładzie, instrukcji uruchomi się pomyślnie, ale nie powiedzie się kolejnych kwerend dla tabeli.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Zamiast tego Określ funkcję filtru przy użyciu nazwy kolumn z nich części, jak pokazano w poniższym przykładzie.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="addafterwiz"></a>Dodawanie funkcji filtrowania po uruchomieniu kreatora  

Jeśli chcesz, należy użyć funkcji, którego nie można tworzyć w kreatorze **Rozciąganie Włączanie bazy danych** , możesz uruchomić instrukcja ALTER TABLE, aby określić funkcji po zakończeniu kreatora. Przed zastosowaniem funkcji, możesz jednak zatrzymać migracji danych, które jest w toku i przywracanie migrowane dane. (Aby uzyskać więcej informacji na temat dlaczego jest to konieczne, zobacz [Zamienianie istniejącej funkcji filtrowania](#replacePredicate).  

1. Odwracanie kierunku migracji i Przesuń do tyłu, które już migracji danych. Nie można anulować tej operacji, po jego uruchomieniu. Również ponoszenia kosztów Azure do przesyłania danych ruchu wychodzącego \(wyjściowego\). Aby uzyskać więcej informacji zobacz [jak Azure ceny działa](https://azure.microsoft.com/pricing/details/data-transfers/).  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Poczekaj, aż migracji zakończyć. Możesz sprawdzić stan w **Rozciąganie Monitor bazy danych** z programu SQL Server Management Studio lub można wysyłać kwerendy w widoku **sys.dm_db_rda_migration_status** . Aby uzyskać więcej informacji, zobacz [Monitor i rozwiązywanie problemów z migracji danych](sql-server-stretch-database-monitor.md) lub [sys.dm_db_rda_migration_status](https://msdn.microsoft.com/library/dn935017.aspx).  

3. Tworzenie funkcji filtru, który chcesz zastosować do tabeli.  

4. Dodawanie funkcji do tabeli, a następnie ponownie uruchom migracji danych Azure.  

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## <a name="filter-rows-by-date"></a>Filtrowanie wierszy według dat
Poniższy przykład migruje wierszy, gdzie kolumny **Data** zawiera wartość wcześniejszych niż 1 stycznia 2016 r.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## <a name="filter-rows-by-the-value-in-a-status-column"></a>Filtrowanie wierszy według wartości w kolumnie Stan
Poniższy przykład migruje wierszy, gdy w kolumnie **Stan** zawiera jedną z określonymi wartościami.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## <a name="filter-rows-by-using-a-sliding-window"></a>Filtrowanie wierszy za pomocą okna przesuwania
Aby filtrować wierszy za pomocą okna przesuwania, pamiętaj o następujące wymagania dotyczące funkcji filtrowania.

-   Funkcja musi być deterministyczna. Dlatego nie można utworzyć funkcję, która automatycznie oblicza ponownie przesuwania okna w miarę upływu czasu.

-   Funkcja użyje powiązanie schematu. W związku z tym nie możesz po prostu zaktualizować funkcji "w miejscu" codziennie, dzwoniąc **ALTER FUNCTION** do przeniesienia przesuwania okna.

Rozpocznij przy użyciu funkcji filtrowania, jak w następującym przykładzie migruje wierszy miejsce, w którym **systemEndTime** kolumna zawiera wartości wcześniejszych niż 1 stycznia 2016 r.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

Funkcja filter zastosować do tabeli.

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Aby zaktualizować przesuwania okna, należy wykonać następujące czynności.

1.  Tworzenie nowej funkcji, która określa nowe okno przesuwania. Poniższy przykład zaznacza dat wcześniejszych niż 2 stycznia 2016 zamiast 1 stycznia 2016 r.

2.  Zamień poprzedniego funkcja filter nową połączeń **ALTER TABLE**, jak pokazano w poniższym przykładzie.

3. Opcjonalnie upuść poprzedniego filtru funkcji, która nie jest już korzystasz z połączeń **UPUŚĆ funkcji**. (Ten krok nie znajduje się w tym przykładzie).

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as the filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## <a name="more-examples-of-valid-filter-functions"></a>Więcej przykładów funkcji prawidłowego filtru

-   Poniższy przykład łączy dwa warunki pierwotny przy użyciu operatorów logicznych AND.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   W poniższym przykładzie użyto kilku warunków i deterministyczne konwersji z Konwertuj.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   W poniższym przykładzie użyto funkcji i operatorów matematycznych.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   W poniższym przykładzie użyto BETWEEN i nie BETWEEN operatorów. Ten zastosowania jest prawidłowy, ponieważ funkcja wyniku spełnia reguły opisanych tutaj po zamianie BETWEEN, a nie między podmiotami z wyrażeń równoważne oraz i lub.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 BETWEEN 0 AND 100
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    Funkcja poprzedniej odpowiada następującą funkcję po zamianie BETWEEN i nie BETWEEN Operatory wyrażeń równoważne oraz i lub.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## <a name="examples-of-filter-functions-that-arent-valid"></a>Przykłady funkcji filtrowania, które nie są poprawnymi

-   Poniższa funkcja jest nieprawidłowy, ponieważ zawiera on prawidłową\-deterministyczne konwersji.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   Poniższa funkcja jest nieprawidłowy, ponieważ zawiera on prawidłową\-połączenia funkcji deterministycznych.

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   Poniższa funkcja jest nieprawidłowy, ponieważ zawiera on podkwerendy.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   Następujące funkcje nie są poprawnymi ponieważ wyrażenia, które operatory algebraicznych lub wbudowanych\-w funkcjach musi być stałą podczas definiowania funkcji. Nie mogą zawierać odwołań do kolumn w algebraicznych lub wywołania funkcji.

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE SQRT(@column1) = 30
    GO
    ```

-   Poniższa funkcja jest nieprawidłowy, ponieważ narusza reguły opisanych tutaj po zamianie BETWEEN operator równoważne wyrażenie i.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    Funkcja poprzedzającego odpowiada następującą funkcję po zamianie BETWEEN operator równoważne wyrażenie i. Ta funkcja jest nieprawidłowy, ponieważ pierwotny warunków można używać tylko operatorów logicznych lub.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## <a name="how-stretch-database-applies-the-filter-function"></a>Jak bazy danych rozciąganie dotyczy funkcja filter
Rozciąganie bazy danych dotyczy funkcja filter tabeli i ustala odpowiedniej wierszy za pomocą operatora krzyżowe stosowanie. Na przykład:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Jeśli funkcja zwraca prawidłową\-wyniku dla wiersza, wiersz jest uprawniony do migracji.

## <a name="replacePredicate"></a>Zamienianie istniejącej funkcji filtrowania
Możesz zastąpić funkcji wcześniej określony filtr, uruchamiając ponownie instrukcja **ALTER TABLE** oraz określając nową wartość **filtru\_PREDYKATU** parametru. Na przykład:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
```
Nowa tabela w tekście\-wartości funkcji ma następujące wymagania.

-   Nowa funkcja musi być unikatowe niż poprzedniego funkcji.

-   Wszystkie operatory istniejące w funkcji stare musi istnieć w nowej funkcji.

-   Nowa funkcja nie może zawierać operatory, których nie istnieje w starej funkcji.

-   Nie można zmienić kolejność argumentów operatora.

-   Tylko stałe wartości, które są częścią `<, <=, >, >=` porównanie mogą być zmieniane w sposób, który sprawia, że funkcja unikatowe.

### <a name="example-of-a-valid-replacement"></a>Przykłady prawidłowych wymiana
Przyjęto, że następującą funkcję zostało bieżącą predykat filtru.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Poniższa funkcja zastępuje prawidłowych ponieważ stałą nową datę (która definiuje później Data progowa) powoduje, że funkcja są unikatowe.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
            AND (@column2 < -50 OR @column2 > 50)
GO
```

### <a name="examples-of-replacements-that-arent-valid"></a>Przykłady poprawnej, które nie są prawidłowe
Poniższa funkcja nie jest prawidłową zamienny ponieważ stałą nową datę (która definiuje wcześniej Data progowa) nie funkcja unikatowe.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Poniższa funkcja nie jest prawidłową zamienny ponieważ jeden z operatorów porównania została usunięta.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -50)
GO
```
Następującą funkcję nie jest prawidłową zamienny ponieważ dodano nowy warunek z operatorów logicznych AND.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
            AND (@column2 <> 0)
GO
```

## <a name="remove-a-filter-function-from-a-table"></a>Usuwanie funkcji filtru z tabeli
Aby przeprowadzić migrację całej tabeli zamiast zaznaczonych wierszy, Usuń istniejących funkcji, ustawiając **Filtr\_PREDYKATU** wartość null. Na przykład:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = NULL,
    MIGRATION_STATE = <desired_migration_state>
) )
```
Po usunięciu funkcja filter wszystkich wierszy w tabeli kwalifikuje się do migracji. W wyniku nie można określić funkcji filtru dla tej samej tabeli później, chyba że możesz przywrócić w chwili zdalnych danych dla tabeli z platformy Azure najpierw. To ograniczenie istnieje w celu uniknięcia sytuacji, w której wiersze, które nie są objęte migracji, po udostępnieniu nowej funkcji filtrowania już zostali przeniesieni do Azure.

## <a name="check-the-filter-function-applied-to-a-table"></a>Sprawdź funkcję filtr zastosowany do tabeli
Aby sprawdzić, funkcja filtr zastosowany do tabeli, otwórz widok wykazu **sys.remote\_danych\_archiwum\_tabele** i sprawdź wartość **filtru\_predykat** kolumny. Jeśli wartość jest pusta, całej tabeli jest uprawniony do archiwizacji. Aby uzyskać więcej informacji zobacz [sys.remote_data_archive_tables (w języku Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="security-notes-for-filter-functions"></a>Uwagi o zabezpieczeniach dla funkcje filtrowania  
Skierowanego na to konto z uprawnieniami db_owner można wykonać następujące czynności.  

-   Tworzenie i stosowanie funkcji zwracających tabelę, która korzysta z dużych ilości zasobów serwera lub czeka przez dłuższy czas, uzyskując odmowa usługi.  

-   Tworzenie i stosowanie funkcji zwracających tabelę, która umożliwia ustalić zawartość tabeli, dla której użytkownik jawnie odmowa dostępu do odczytu.  

## <a name="see-also"></a>Zobacz też

[Instrukcja ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
