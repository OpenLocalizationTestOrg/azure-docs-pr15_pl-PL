<properties
   pageTitle="Zarządzanie współbieżności i obciążenie pracą w magazynie danych SQL | Microsoft Azure"
   description="Opis zarządzania współbieżności i obciążenie pracą w magazynie danych SQL Azure dla opracowania rozwiązań."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Zarządzanie współbieżności i obciążenie pracą w magazynie danych SQL

Do przeprowadzania przewidywalne wydajności w skali, ułatwia magazynu danych Microsoft Azure SQL kontrolujesz poziomy współbieżności i alokacji zasobów, takich jak priorytetów Procesora i pamięci. W tym artykule przedstawiono podstawowe pojęcia zarządzania współbieżności i obciążenie pracą, wyjaśniający, jak obie funkcje zostały wprowadzone i jak możesz sterować ich w magazynie danych. Zarządzanie pracą magazynem danych SQL ma świadczyć pomoc techniczną środowisku wielu użytkowników. Nie jest przeznaczona dla wielu dzierżawy obciążenia.

## <a name="concurrency-limits"></a>Limity współbieżności

Program SQL Data Warehouse umożliwia maksymalnie 1024 równoczesne połączeń. Wszystkie połączenia 1024 może przesyłać kwerendy jednocześnie. Jednak aby zoptymalizować przepustowość, magazynu danych SQL może kolejka niektórych kwerend, aby upewnić się, że każda kwerenda otrzyma udzielanie minimalnego pamięci. Kolejkowanie występuje w czasie wykonywania kwerend. Przez kolejkowanie kwerendy po osiągnięciu limitów współbieżności magazynu danych SQL można zwiększyć ogólnej przepustowości przez zapewnienie, że aktywnej kwerendy uzyskiwanie dostępu do zasobów krytyczną potrzebnej pamięci.  

Limity współbieżności podlegają dwa pojęcia: *równoczesne kwerend* i *gniazda współbieżności*. Kwerendy do wykonania należy wykonać w ramach zarówno limit współbieżności kwerendy i Alokacja przedział współbieżności.

- Równoczesne kwerendy są kwerendy, wykonywanie w tym samym czasie. Program SQL Data Warehouse obsługuje do 32 równoczesne kwerendy na większych rozmiarów DWU.
- Gniazda współbieżności są przydzielane według DWU. Każdy 100 DWU zawiera 4 gniazda współbieżności. Na przykład DW100 przydziela 4 gniazda współbieżności i DW1000 przydziela 40. Każda kwerenda korzysta z jednego lub więcej współbieżności gniazda, zależy od [zasobów klasy](#resource-classes) kwerendy. Kwerendy uruchamianie klasy zasobu smallrc korzystanie ze jeden przedział współbieżności. Kwerendy uruchamianie wyższej klasy zasobu korzystanie ze kolejnych gniazd współbieżności.

W poniższej tabeli opisano limity dotyczące zarówno równoczesne zapytań, jak i gniazda współbieżności w różnych rozmiarach DWU.

### <a name="concurrency-limits"></a>Limity współbieżności

|  DWU   | Maksymalna liczba kwerend równoczesne  | Gniazd współbieżności przydzielonych |
| :----  | :---------------------: | :-------------------------: |
| DW100  |            4            |                4            |
| DW200  |            8            |                8            |
| DW300  |           12            |               12            |
| DW400  |           16            |               16            |
| DW500  |           20            |               20            |
| DW600  |           24            |               24            |
| DW1000 |           32            |               40            |
| DW1200 |           32            |               48            |
| DW1500 |           32            |               60            |
| DW2000 |           32            |               80            |
| DW3000 |           32            |              120            |
| DW6000 |           32            |              240            |

Jeśli jest spełniony jeden z tych progów, nowych kwerend są w kolejce i są stosowane na zasadzie first-out pierwszego w.  Kończy się kwerendy i liczba kwerend i gniazda przypada poniżej granic, kolejce kwerendy są dodawane. 

> [AZURE.NOTE]  *Wybierz* kwerendy wykonywanie wyłącznie na dynamiczne widoki zarządzania (DMVs) lub widoków wykazu nie podlegają wymienionych limitów współbieżności. Można monitorować system bez względu na to liczba kwerend wykonywanie na nim.

## <a name="resource-classes"></a>Klasy zasobów

Zasób klas pomocy kontrolowanie przydzielanie pamięci i cyklów Procesora dostarczonych do kwerendy. Cztery klasy zasobu można przypisać do użytkownika w formularzu *role bazy danych*. Klasy zasobów cztery są **smallrc**, **mediumrc**, **largerc**i **xlargerc**. Użytkownicy w smallrc podano mniejszej ilości pamięci i korzystać z wyższymi współbieżności. Z drugiej strony Użytkownicy przypisani do xlargerc podano dużej ilości pamięci i w związku z tym mniej kwerendy można uruchomić jednocześnie.

Domyślnie każdy użytkownik jest członkiem klasy małych zasobów, smallrc. Procedura `sp_addrolemember` jest umożliwiają zwiększenie klasy zasobu i `sp_droprolemember` umożliwia zmniejszenie klasy zasobu. Na przykład tego polecenia zwiększy klasy zasobu loaduser do largerc:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```

Zalecane jest trwale przypisywanie użytkowników do klasy zasobów, zamiast zmieniać ich klas zasobów. Na przykład obciążenia do tabel grupowany columnstore Utwórz indeksy wyższą jakość, gdy przydzielone więcej pamięci. W celu zapewnienia, że obciążenia mają dostęp do pamięci wyższą, Utwórz użytkownika specjalnie dla ładowania danych i trwale Przypisz tego użytkownika wyższymi klasy zasobu.

Istnieje kilka typów kwerend, które nie korzystają z większym przydzielanie pamięci. System będzie ignorowanie ich alokacji zasobów zajęć i uruchamiać te zapytania w klasie małych zasobu zamiast tego. Jeśli te kwerendy są zawsze uruchamiane w klasie małych zasobów, można uruchamiać po współbieżności gniazda są w obszarze ciśnienia i nie zajmowanie gniazda więcej niż jest potrzebna. Aby uzyskać więcej informacji, zobacz [Wyjątki klasy zasobu](#query-exceptions-to-concurrency-limits) .

Kilka więcej informacji na temat klasy zasobu:

- Aby zmienić klasy zasobu użytkownika wymagane jest uprawnienie *ALTER roli* .  
- Mimo że można dodać użytkownika do jednej lub większej liczby wyższych klas zasobów, użytkowników będzie miał atrybuty najwyższe klasy zasobu, do którego są przydzielone. Jeśli użytkownik jest przypisany do mediumrc i largerc, wyższej klasy zasobu (largerc) jest klasy zasobu, która będzie uznane.  
- Nie można zmienić klasy zasobów administratora systemu.

Na przykład szczegółowe zobacz [Zmienianie użytkownika zasobu klasy przykład](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Przydzielanie pamięci

Istnieje wad i zalet do wzrostu klasy zasobów użytkownika. Zwiększanie klasy zasobów dla użytkownika będzie udostępnienia ich kwerendy więcej pamięci, co może oznaczać, że szybsze wykonywanie zapytań.  Jednak wyższych klas zasobu również zmniejszyć liczbę równoczesne kwerend, które można uruchamiać. Jest to zależność między przydzielanie dużej ilości pamięci do pojedynczej kwerendy lub zezwolenia na innych kwerend, które muszą również przydziału pamięci, aby uruchomić jednocześnie. Jeśli jeden użytkownik podano wysoka przydziałów pamięci dla zapytania, inni użytkownicy nie będą mieć dostępu do tego samego pamięci, aby uruchomić kwerendę.

Poniższa tabela mapy pamięć przydzielona do każdego dystrybucji przez klasę DWU i zasobów.

### <a name="memory-allocations-per-distribution-mb"></a>Przydziału pamięci na dystrybucyjnych (MB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |   100   |    100   |    200  |    400   |
| DW200  |   100   |    200   |    400  |    800   |
| DW300  |   100   |    200   |    400  |    800   |
| DW400  |   100   |    400   |    800  |  1 600   |
| DW500  |   100   |    400   |    800  |  1 600   |
| DW600  |   100   |    400   |    800  |  1 600   |
| DW1000 |   100   |    800   |  1 600  |  3 200   |
| DW1200 |   100   |    800   |  1 600  |  3 200   |
| DW1500 |   100   |    800   |  1 600  |  3 200   |
| DW2000 |   100   |  1 600   |  3 200  |  6,400   |
| DW3000 |   100   |  1 600   |  3 200  |  6,400   |
| DW6000 |   100   |  3 200   |  6,400  |  12,800  |

Z powyższej tabeli można zobaczyć, że kwerendy uruchomionych DW2000 klasy zasobu xlargerc czy mają dostęp do 6400 MB pamięci w każdej 60 rozłożone baz danych.  W magazynie danych SQL istnieje 60 dystrybucji. W związku z tym aby obliczyć alokację pamięć zapytania w klasie danego zasobu, powyższe wartości powinny być pomnożone przez 60.

### <a name="memory-allocations-system-wide-gb"></a>Pamięć alokacje systemowe (GB)

|  DWU   | smallrc | mediumrc | largerc | xlargerc |
| :----- | :-----: | :------: | :-----: | :------: |
| DW100  |    6    |    6     |    12   |    23    |
| DW200  |    6    |    12    |    23   |    47    |
| DW300  |    6    |    12    |    23   |    47    |
| DW400  |    6    |    23    |    47   |    94    |
| DW500  |    6    |    23    |    47   |    94    |
| DW600  |    6    |    23    |    47   |    94    |
| DW1000 |    6    |    47    |    94   |   188    |
| DW1200 |    6    |    47    |    94   |   188    |
| DW1500 |    6    |    47    |    94   |   188    |
| DW2000 |    6    |    94    |   188   |   375    |
| DW3000 |    6    |    94    |   188   |   375    |
| DW6000 |    6    |   188    |   375   |   750    |

Z tej tabeli przydziału pamięci systemowe widać, że kwerendy uruchomionych DW2000 xlargerc klasy zasobu przydzielono sumę 375 GB pamięci (rozkład 6400 MB * 60 / 1024 do przekonwertowania na GB) w całości magazynu danych SQL.

## <a name="concurrency-slot-consumption"></a>Zużycie przedział współbieżności

Program SQL Data Warehouse udziela większej ilości pamięci do kwerendy uruchamianie wyższych klas zasobów. Pamięć jest stały zasobów.  Dlatego więcej pamięci przydzielone dla kwerendy, mniejszej liczby zapytań równoczesne można wykonywać. Poniższa tabela powtórnie wszystkich poprzednich pojęć w jednym widoku, który zawiera numer współbieżności dostępnych przez DWU i gniazda zużywanej przez każdej klasy zasobu.

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Alokacja i zużycie czasu na start lub współbieżności

|  DWU   | Maksymalna liczba kwerend równoczesne  | Gniazd współbieżności przydzielonych | Używane przez smallrc gniazda |  Używane przez mediumrc gniazda |  Używane przez largerc gniazda |  Używane przez xlargerc gniazda |
| :----  | :---------------------: | :-------------------------: | :-----: | :------: | :-----: | :------: |
| DW100  |            4            |                4            |    1    |     1    |    2    |    4     |
| DW200  |            8            |                8            |    1    |     2    |    4    |    8     |
| DW300  |           12            |               12            |    1    |     2    |    4    |    8     |
| DW400  |           16            |               16            |    1    |     4    |    8    |   16     |
| DW500  |           20            |               20            |    1    |     4    |    8    |   16     |
| DW600  |           24            |               24            |    1    |     4    |    8    |   16     |
| DW1000 |           32            |               40            |    1    |     8    |   16    |   32     |
| DW1200 |           32            |               48            |    1    |     8    |   16    |   32     |
| DW1500 |           32            |               60            |    1    |     8    |   16    |   32     |
| DW2000 |           32            |               80            |    1    |    16    |   32    |   64     |
| DW3000 |           32            |              120            |    1    |    16    |   32    |   64     |
| DW6000 |           32            |              240            |    1    |    32    |   64    |  128     |


W tej tabeli wyświetlanie z systemem SQL Data Warehouse DW1000 przydziela maksymalnie 32 równoczesne kwerend i sumy 40 czasu na start lub współbieżności. Wszyscy użytkownicy korzystający z w smallrc, 32 kwerend równoczesne czy dozwolone, ponieważ każda kwerenda będzie zajmować 1 przedział współbieżności. Jeśli wszyscy użytkownicy na DW1000 działały w mediumrc, każda kwerenda zostałaby przydzielona 800 MB na rozkład przydzielania pamięć 47 GB dla kwerendy i współbieżności może być ograniczone tylko do 5 użytkowników (40 gniazda współbieżności / 8 gniazd na użytkownika mediumrc).

## <a name="query-importance"></a>Znaczenie kwerendy

Magazyn danych SQL zawiera klasy zasobów przy użyciu grup obciążenie pracą. Istnieje łącznie ośmiu grup obciążenie pracą, wpływających na wielu różnych rozmiarach DWU zachowanie klas zasobów. W przypadku dowolnej DWU magazynu danych SQL używa tylko cztery grupy osiem obciążenie pracą. Ma sens, ponieważ każda grupa obciążenie pracą jest przypisany do jednej z czterech klas zasobu: smallrc, mediumrc, largerc, lub xlargerc. Znaczenie opis grup obciążenie pracą to, że niektóre z tych grup obciążenie pracą ustawiono wyższą *ważność*. Znaczenie jest używane na potrzeby Procesora planowania. Zapytań za pomocą Wysoka ważność otrzymają trzykrotnie więcej cykli Procesora niż średnia ważności. Dlatego mapowania przedział współbieżności także określić priorytet CPU. Kwerendy powoduje zużycie 16 lub więcej gniazd, działają jako bardzo ważnej.

W poniższej tabeli przedstawiono mapowania znaczenie dla każdej grupy obciążenie pracą.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Obciążenie pracą mapowania grup do wolnych miejsc współbieżności i ważność

| Grupy obciążenie pracą | Mapowanie przedział współbieżności | MB / dystrybucji | Mapowanie ważność |
| :-------------- | :----------------------: | :---------------: | :----------------- |
| SloDWGroupC00   |            1             |         100       |       Średnia       |
| SloDWGroupC01   |            2             |         200       |       Średnia       |
| SloDWGroupC02   |            4             |         400       |       Średnia       |
| SloDWGroupC03   |            8             |         800       |       Średnia       |
| SloDWGroupC04   |           16             |       1 600       |       Duży         |
| SloDWGroupC05   |           32             |       3 200       |       Duży         |
| SloDWGroupC06   |           64             |       6,400       |       Duży         |
| SloDWGroupC07   |          128             |      12,800       |       Duży         |

**Alokacja i zużycie czasu na start lub współbieżności** wykresu można zobaczyć, że DW500 używa 1, 4, 8 lub 16 współbieżności gniazda dla smallrc, mediumrc, largerc i xlargerc, odpowiednio. Możesz wyszukać te wartości na wykresie znaleźć znaczenie dla każdej grupy zasobów.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Mapowanie DW500 zasobów klas ważność

| Klasy zasobów | Grupa obciążenie pracą | Gniazda współbieżności używane | MB / dystrybucji | Znaczenie |
| :------------- | :------------- | :--------------------: | :---------------: | :--------- |
| smallrc        | SloDWGroupC00  |           1            |         100       |   Średnia   |
| mediumrc       | SloDWGroupC02  |           4            |         400       |   Średnia   |
| largerc        | SloDWGroupC03  |           8            |         800       |   Średnia   |
| xlargerc       | SloDWGroupC04  |          16            |        1 600      |   Duży     |


Aby przyjrzeć się różnice w alokacją zasobów pamięci szczegółowo z perspektywy Gubernator zasobów lub do analizy historycznej i active zastosowania grup obciążenie pracą podczas rozwiązywania problemów, można użyć następującej kwerendy DMV.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                      AS node_type
    ,pn.pdw_node_id                 AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                  AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                     AS group_max_dop
    ,wg.effective_max_dop               AS group_effective_max_dop
    ,wg.total_request_count             AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON  wg.pdw_node_id  = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,   group_request_max_memory_grant_pcnt
,   group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Zapytania, które przestrzegać ograniczeń współbieżności

Większość kwerend podlegają klasy zasobu. Te kwerendy musi nadania obu jednocześnie kwerendy i współbieżności przedział progi. Użytkownik nie można wykluczyć kwerendy z modelem współbieżności przedział.

Aby powtórzyć, poniższe instrukcje przestrzegać klasy zasobu:

- WYBIERZ POZYCJĘ WSTAW
- AKTUALIZACJA
- USUWANIE
- Wybierz pozycję (w przypadku kwerend tabele użytkowników)
- INSTRUKCJA ALTER ODBUDOWYWANIA INDEKSU
- ZMIEŃ ROZMIESZCZENIE INDEKSU
- INSTRUKCJA ALTER ODBUDUJ TABELI
- TWORZENIE INDEKSU
- TWORZENIE INDEKSU COLUMNSTORE GRUPOWANY
- TWORZENIE TABELI JAKO WYBIERZ POZYCJĘ (CTAS)
- Ładowanie danych
- Dane przepływu przeprowadzonych przez usługi przepływu danych (systemie)

## <a name="query-exceptions-to-concurrency-limits"></a>Wyjątki kwerendy limitów współbieżności

Niektóre kwerendy nie obsługują funkcji nakładania klasy zasobu, do którego przydzielono użytkownika. Następujące wyjątki od ograniczeń współbieżności po stają się zasobów pamięci wymaganych dla określonego polecenia brakuje, często to polecenie jest operacja metadanych. Celem tych wyjątków jest unikać większe przydziału pamięci w przypadku kwerend, które nie będzie trzeba je. W tych przypadkach domyślnej klasy małych zasobów (smallrc) jest zawsze używany niezależnie od klasy faktycznymi zasobami przypisana do użytkownika. Na przykład `CREATE LOGIN` zawsze będzie działać w smallrc. Zasoby wymagane do realizacji tej operacji są bardzo niskie, więc nie zrozumiałe uwzględnienie zapytanie w modelu przedział współbieżności.  Te kwerendy również nie są ograniczone limit współbieżności 32 użytkowników, nieograniczoną liczbę te kwerendy można uruchamiać w górę do limitu sesji 1024 sesji.

Poniższe instrukcje nie obsługują funkcji nakładania klasy zasobu:

- Tworzenie lub usuwanie tabeli
- INSTRUKCJA ALTER TABLE... PRZEŁĄCZANIE, dzielenie lub SCALANIE PARTITION
- INSTRUKCJA ALTER WYŁĄCZANIE INDEKSU
- USUWANIE INDEKSU
- Tworzenie, AKTUALIZOWANIE lub DROP STATISTICS
- OBCINANIE TABELI
- INSTRUKCJA ALTER AUTORYZACJI
- TWORZENIE LOGOWANIA
- Tworzenie, ALTER lub DROP USER
- Tworzenie, ZMIEŃ lub UPUSZCZAĆ procedury
- Tworzenie lub usuwanie WIDOKU
- WSTAWIANIE WARTOŚCI
- Wybierz z widoków systemu i DMVs
- WYJAŚNIĆ
- DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

## <a name="change-a-user-resource-class-example"></a>Zmienianie przykład klasy zasobów użytkownika

1. **Tworzenie logowania:** Otwórz połączenie do **wzorca** bazy danych na serwerze SQL obsługującym bazy danych SQL magazynu danych, a następnie wykonaj następujące polecenia.

    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```

    > [AZURE.NOTE] Jest dobrym pomysłem jest utworzenie użytkownika w główną bazę danych użytkowników magazynu danych SQL Azure. Tworzenie użytkownika we wzorcu umożliwia użytkownikowi zalogować się za pomocą narzędzi, takich jak SSMS bez określenia nazwy bazy danych.  Umożliwia także wyświetlić za pomocą Eksploratora obiektów wszystkie bazy danych w programie SQL server.  Aby uzyskać więcej informacji na temat tworzenia i zarządzanie użytkownikami zobacz [bezpiecznego bazy danych w magazynie danych SQL][].

2. **Użytkownikowi tworzenie magazynu danych SQL:** Otwórz połączenie z bazą danych **SQL magazynu danych** , a następnie wykonaj następujące polecenie.

    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```

3. **Udzielanie uprawnień:** W przykładzie poniżej zezwolenie `CONTROL` w bazie danych **SQL magazynu danych** . `CONTROL`w bazie danych poziom odpowiada db_owner w programie SQL Server.

    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```

4. **Zwiększanie klasy zasobu:** Aby dodać użytkownika do roli wyższą zarządzania obciążenie pracą, należy użyć następującej kwerendy.

    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```

5. **Zmniejsz klasy zasobu:** Aby usunąć użytkownika z roli zarządzania obciążenie pracą, należy użyć następującej kwerendy.

    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```

    > [AZURE.NOTE] Nie jest możliwe usuwanie użytkownika ze smallrc.

## <a name="queued-query-detection-and-other-dmvs"></a>Wykrywanie kolejce kwerendy i inne DMVs

Możesz użyć `sys.dm_pdw_exec_requests` DMV do identyfikowania kwerendy oczekujących w kolejce współbieżności. Oczekiwanie na przedział współbieżności kwerendy będą mieć stan **zawieszone**.

```sql
SELECT   r.[request_id]              AS Request_ID
        ,r.[status]              AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]              AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Role związane z zarządzaniem obciążenie pracą można wyświetlić w programie `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Poniższe zapytanie pokazuje każdy użytkownik jest przypisany do roli.

```sql
SELECT   r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id     = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id   = m.principal_id
WHERE   r.name IN ('mediumrc','largerc', 'xlargerc');
```

Program SQL Data Warehouse występują następujące typy czekać:

- **LocalQueriesConcurrencyResourceType**: kwerend, które znajdują się poza framework przedział współbieżności. DMV kwerendy i system, takich jak funkcje `SELECT @@VERSION` przedstawiono przykłady lokalne kwerendy.
- **UserConcurrencyResourceType**: kwerend, które znajdują się w ramach przedział współbieżności. Kwerend dla użytkowników końcowych tabele przedstawiają przykłady korzystające ten typ zasobu.
- **DmsConcurrencyResourceType**: czeka powstałe w wyniku operacji przepływu danych.
- **BackupConcurrencyResourceType**: ten oczekiwanie wskazuje, że baza danych jest teraz kopię zapasową. Maksymalna wartość dla tego typu zasobu to 1. Jeśli wiele kopii zapasowych zażądano w tym samym czasie, inne osoby będą kolejki.

`sys.dm_pdw_waits` DMV można używać do wyświetlania zasobów, których oczekuje żądanie.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,   SESSION_ID()            AS Current_session
,   s.[status]          AS Session_status
,   s.[login_name]
,   s.[query_count]
,   s.[client_id]
,   s.[sql_spid]
,   r.[command]         AS Request_command
,   r.[label]
,   r.[status]          AS Request_status
,   r.[submit_time]
,   r.[start_time]
,   r.[end_compile_time]
,   r.[end_time]
,   DATEDIFF(ms,r.[submit_time],r.[start_time])     AS Request_queue_time_ms
,   DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,   DATEDIFF(ms,r.[end_compile_time],r.[end_time])      AS Request_execution_time_ms
,   r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE   w.[session_id] <> SESSION_ID();
```

`sys.dm_pdw_resource_waits` DMV pokazuje tylko czeka zasobów zużywanej przez danej kwerendy. Czas oczekiwania zasobów środków tylko czas oczekiwania dla zasobów do zrealizowania, zamiast czas oczekiwania sygnału, czyli czas potrzebny źródłowych serwerów SQL planowania kwerendy na Procesora.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE   [session_id] <> SESSION_ID();
```

`sys.dm_pdw_wait_stats` DMV może być używany do analizy historycznej trendu czeka.

```sql
SELECT  w.[pdw_node_id]
,       w.[wait_name]
,       w.[max_wait_time]
,       w.[request_count]
,       w.[signal_time]
,       w.[completed_count]
,       w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o zarządzaniu użytkownikami bazy danych i zabezpieczeń zobacz [bezpiecznego bazy danych w magazynie danych SQL][]. Aby uzyskać więcej informacji o zasobie jak większe klas można poprawić jakość indeks grupowany columnstore, zobacz [Rebuilding indeksy, aby poprawić jakość części].

<!--Image references-->

<!--Article references-->
[Zabezpieczanie bazy danych w magazynie danych SQL]: ./sql-data-warehouse-overview-manage-security.md
[Odbudowywania indeksów, aby poprawić jakość części]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Zabezpieczanie bazy danych w magazynie danych SQL]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->
