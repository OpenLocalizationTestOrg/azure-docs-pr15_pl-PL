<properties 
    pageTitle="Kod bufor cykliczny XEvent bazy danych SQL | Microsoft Azure" 
    description="Zawiera przykładowy kod języka Transact-SQL, wykonany łatwe i szybkie przy użyciu celu bufor cykliczny w bazie danych SQL Azure." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Dzwonienie kod docelowej buforu rozszerzone zdarzenia w bazie danych SQL

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Chcesz próbki kompletny kod najłatwiej szybkiego przechwytywania i raport informacje dotyczące zdarzenia rozszerzonego podczas próby. Najprostszym docelowej dla danych zdarzenia rozszerzonego jest [Bufor cykliczny docelowej](http://msdn.microsoft.com/library/ff878182.aspx).


W tym temacie przedstawiono przykładowy kod w języku Transact-SQL, które:


1. Tworzy tabelę z danych z.

2. Tworzy sesję dla zdarzenia istniejącego rozszerzonego, takich jak **sqlserver.sql_statement_starting**.
    - Zdarzenie jest ograniczone do instrukcji SQL, które zawierają określony ciąg aktualizacji: **instrukcji, takie jak "% aktualizacji tabEmployee %"**.
    - Wybiera do wysłania wyniku zdarzenia docelowej typu bufor cykliczny, takich jak **package0.ring_buffer**.

3. Zostanie uruchomiony sesji zdarzeń.

4. Problemy kilka prostych instrukcji SQL UPDATE.

5. Problemy z SQL wybierz, aby pobrać dane wyjściowe wydarzenia z bufora dzwonienia.
    - sprzężone **sys.dm_xe_database_session_targets** i innych widoków dynamiczne zarządzanie (DMVs).

6. Zakończenie sesji zdarzenia.

7. Pomija docelowej bufor cykliczny, aby zwolnić zasoby.

8. Pomija sesji zdarzeń i tabeli pokaz.


## <a name="prerequisites"></a>Wymagania wstępne


- Konto Azure i subskrypcji. Możesz zalogować do [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).


- Dowolnej bazy danych, które można utworzyć tabelę w.
 - Opcjonalnie możesz [utworzyć bazę danych programu **AdventureWorksLT** pokaz](sql-database-get-started.md) w minutach.


- SQL Server Management Studio (ssms.exe), najlepiej najnowszej miesięczny wersji aktualizacji. Możesz pobrać najnowszą ssms.exe z:
 - Temacie [Pobierz program SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Bezpośrednie łącze do pobierania.](http://go.microsoft.com/fwlink/?linkid=616025)


## <a name="code-sample"></a>Przykładowy kod


Niewielkich modyfikacji Poniższy przykładowy kod buforu dzwonienie mogą być uruchamiane w bazy danych SQL Azure lub Microsoft SQL Server. Różnica polega na obecność węzeł "_bazadanych" Nazwa niektórych dynamiczne widoki zarządzania (DMVs) używane w klauzuli FROM w kroku 5. Na przykład:

- _session_targets**_bazadanych**sys.dm_xe
- sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## <a name="ring-buffer-contents"></a>Zawartość buforu dzwonienia


Aby uruchomić przykładowy kod użyliśmy ssms.exe.


Aby wyświetlić wyniki, możemy klikniętego komórkę w obszarze nagłówka kolumny **target_data_XML**.

Następnie w okienku wyników możemy kliknięciu komórki w obszarze nagłówka kolumny **target_data_XML**. Ten kliknij utworzony inną kartę Plik w ssms.exe w którym zawartość komórki wynik był wyświetlany jako XML.


Wynik jest wyświetlana w następujący blok. Wygląd długi, ale jest tylko dwa **<event>** elementy.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Zwolnij zasobów blokowanych przez usługi Bufor cykliczny


Po wykonaniu tych czynności przy użyciu usługi buforu dzwonienia, możesz ją usunąć i wdrożenie jego wydawania **zmieniać** podobnej do następującej:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Definicji sesję zdarzenie jest zaktualizowany, ale nie usunięta. Później można dodać kolejnego wystąpienia bufor cykliczny sesję zdarzeń:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Więcej informacji


Podstawowy temat rozszerzonego zdarzeń w bazie danych SQL Azure jest:


- [Zagadnienia dotyczące zdarzeń rozszerzone w bazie danych SQL](sql-database-xevent-db-diff-from-svr.md), które kontrastujący niektórych aspektów rozszerzone zdarzenia, które różnią się w stosunku do programu Microsoft SQL Server, bazy danych SQL Azure.


Inne tematy przykładowy kod dla zdarzenia rozszerzonego są dostępne w następujących łączy. Należy jednak regularnie sprawdzić każdej próbki, aby sprawdzić, czy próbka jest przeznaczony dla programu Microsoft SQL Server i bazy danych SQL Azure. Następnie można zdecydować, czy drobnych zmian są potrzebne do uruchomienia próbki.


- Przykładowy kod dla bazy danych SQL Azure: [Plik zdarzeń kod docelowej rozszerzone zdarzenia w bazie danych SQL](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
