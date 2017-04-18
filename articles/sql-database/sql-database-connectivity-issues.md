<properties
    pageTitle="Naprawianie błędu połączenia SQL, przejściowych błąd | Microsoft Azure"
    description="Dowiedz się, jak rozwiązywanie problemów, diagnozowanie i zapobiec błąd połączenia SQL lub przejściowych w bazie danych SQL Azure. "
    keywords="Połączenie SQL, parametry połączenia, problemy z łącznością, przejściowych błąd, błąd połączenia"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="daleche"/>


# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Rozwiązywanie problemów, diagnozowanie i uniknięcia błędów połączenia SQL i przejściowych błędów bazy danych SQL

W tym artykule opisano, jak zapobiec, rozwiązywanie problemów, diagnozowanie i zmniejszeniu błędów połączeń oraz przejściowych błędy, które aplikacji klienckiej napotka przy interakcji z bazy danych SQL Azure. Dowiedz się, jak skonfigurować ponów próbę logicznych, Tworzenie parametrów połączenia i inne ustawienia połączenia.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Przejściowych błędy (błędy przejściowych)

Przejściowych błąd - także przejściowych błędów - ma podstawową przyczyną, która szybko rozwiązać sam. Rzadkie przyczyny błędów przejściowych jest, gdy systemu Azure szybko przenoszony zasoby sprzętowe lepszego równoważenia obciążenia różne obciążenia. Większość z tych zdarzeń zmiany konfiguracji często wykonać w ciągu mniej niż 60 sekund. W tym okresie czasu zmiany konfiguracji może być problemów z łącznością z bazą danych SQL Azure. Nawiązywanie połączenia z bazą danych SQL Azure aplikacji powinno być tworzone oczekiwać tych błędów przejściowych, podejmie wobec nich wdrażając ponów próbę logicznych kodu, zamiast wyświetlanie ich użytkownikom jako błędy aplikacji.

Jeśli program klient używa ADO.NET, program jest informowany o błędzie przejściowych przez rzutowania: **SqlException**. Właściwość **Liczba** można porównać z listą przejściowych błędy w górnej części tematu: [SQL kodów błędów aplikacji klienckich bazy danych SQL](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Połączenia i polecenia

Będzie ponów próbę połączenia SQL lub ustanawiania go ponownie, w zależności od następujących czynności:

* **Wystąpi błąd przejściowych, spróbuj połączenie**: połączenie powinno być ponowione po opóźnienia przez kilka sekund.

* **Wystąpi błąd przejściowych, polecenia zapytania SQL**: polecenie należy nie natychmiast wykonana ponownie. Zamiast tego z opóźnieniem powinny być świeżo ustanowić połączenia. Następnie polecenia mogą być wycofane.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Ponawiania przejściowych błędów


Programy klienckie, które od czasu do czasu wystąpienia błędu przejściowych są bardziej rozbudowany, gdy zawierają ponów próbę logicznych.


Gdy program komunikuje się z bazą danych SQL Azure za pomocą 3 pośredniczącym firmy, zapytanie z dostawcą czy pośredniczącym zawiera ponów próbę logicznych przejściowych błędów.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Zasady ponów próbę


- Jeśli komunikat o błędzie jest przejściowych, należy wykonana ponownie próba otwarcia połączenia.


- Instrukcję SQL SELECT kończy się niepowodzeniem z błędem przejściowych nie powinny bezpośrednio wykonana ponownie.
 - Zamiast tego ustanowienia nowego połączenia, a następnie ponów próbę wybierz.


- Gdy instrukcję SQL UPDATE kończy się niepowodzeniem z błędem przejściowych, nowego połączenia należy ustanowić przed po ponownych próbach aktualizacji.
 - Logika ponawiania należy zagwarantować, że wykonane transakcji całej bazy danych lub że cała transakcja jest przywracany.


#### <a name="other-considerations-for-retry"></a>Inne zagadnienia związane z ponów próbę


- Program wsadowy, który jest automatycznie uruchamiany po godzinach pracy, a które zakończy się przed rano, można przyznać bardzo pacjentów z długo interwałów między jego ponownych prób.


- Program interfejsu użytkownika należy uwzględnić tendencji ludzi po zbyt długo oczekiwania.
 - Jednak rozwiązanie nie może być próbować co kilka sekund, ponieważ tych zasad można zalewu systemie żądania.


#### <a name="interval-increase-between-retries"></a>Zwiększ interwał między kolejnymi próbami



Firma Microsoft zaleca opóźnienie 5 sekund przed do pierwszej ponów próbę. Ponawianie opóźnieniem krótsza ryzyka 5 sekund przeciążając uda się rozpoznać usług w chmurze. Dla każdej kolejnej ponów próbę opóźnienie powinien być zwiększany sposób wykładniczy maksymalnie do 60 sekund.

Dyskusja *blokowania przez okres* dla klientów korzystających z ADO.NET jest dostępna w [SQL Server połączenia buforowanie (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Można także ustawić maksymalna liczba prób, zanim program własny zakończy.


#### <a name="code-samples-with-retry-logic"></a>Przykłady kodu z ponów próbę logicznych


Przykłady kodu z Logika ponawiania w różnych językach programowania, są dostępne na:

- [Biblioteki połączeń dla bazy danych SQL i programu SQL Server](sql-database-libraries.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Testowanie logiki ponów próbę


Aby przetestować logiki ponów próbę, możesz symulować lub powodują wystąpienie błędu, nie można rozwiązać, gdy program jest nadal uruchomiony.


##### <a name="test-by-disconnecting-from-the-network"></a>Testowanie przez odłączenie z sieci


Jednym ze sposobów istnieje możliwość przetestowania logiki ponów próbę jest odłączyć od sieci na komputerze klienckim, gdy jest uruchomiony program. Komunikat o błędzie będzie:

- **SqlException.Number** = 11001
- Komunikat o błędzie: "host jest znany"


Podczas pierwszej próby ponów próbę programu można poprawić błąd pisowni i spróbuj nawiązać.


Ma być praktyczne, można odłączyć komputer z sieci przed rozpoczęciem programu. Następnie program rozpoznaje parametr czas, który powoduje, że program:

1. Dodaj tymczasowo 11001 do swojej listy błędów brać pod uwagę jako zmiennych.
2. Podjąć próbę jej pierwszego połączenia w zwykły sposób.
3. Po otrzymanych jest komunikat o błędzie, Usuń 11001 z listy.
4. Wyświetlanie komunikat informujący użytkownika, aby podłączyć komputer do sieci.
 - Zatrzymaj wskaźnik myszy dalsze wykonanie przy użyciu metody **Console.ReadLine** lub okno dialogowe z przyciskiem OK. Naciśnięciu klawisza Enter po komputer podłączony do sieci.
5. Spróbuj ponownie nawiązać połączenie, oczekiwano sukcesu.


##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Przetestuj błąd pisowni nazwy bazy danych podczas nawiązywania połączenia


Program może celowo Literówka nazwa użytkownika przed pierwszym próba nawiązania połączenia. Komunikat o błędzie będzie:

- **SqlException.Number** = 18456
- Komunikat o błędzie: "Nazwa logowania użytkownika"WRONG_MyUserName"nie powiodło się."


Podczas pierwszej próby ponów próbę programu można poprawić błąd pisowni i spróbuj nawiązać.


Aby wprowadzić praktyczne, program może rozpoznać parametr czas, który powoduje, że program:

1. Dodaj tymczasowo 18456 do swojej listy błędów brać pod uwagę jako zmiennych.
2. Celowo dodać "WRONG_" na nazwę użytkownika.
3. Po otrzymanych jest komunikat o błędzie, Usuń 18456 z listy.
4. Usuwanie "WRONG_" z nazwą użytkownika.
5. Spróbuj ponownie nawiązać połączenie, oczekiwano sukcesu.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>Parametry .NET SqlConnection ponów próbę połączenia


Jeśli program klient nawiązuje połączenie z bazą danych SQL Azure za pomocą programu .NET Framework klasy **System.Data.SqlClient.SqlConnection**, należy użyć programu .NET 4.6.1 lub nowszej, aby korzystać z funkcji ponów próbę jej połączenia. Szczegóły funkcji można znaleźć [w tym miejscu](http://go.microsoft.com/fwlink/?linkid=393996).


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Jeśli konstruujesz [Parametry połączenia](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) dla obiektu **SqlConnection** , należy koordynowanie wartości spośród następujących parametrów:

- ConnectRetryCount &nbsp; &nbsp; *(wartością domyślną jest 1. Zakres jest od 0 do 255.)*
- ConnectRetryInterval &nbsp; &nbsp; *(wartość domyślna to 1 sekundy. Zakres wynosi od 1 do 60.)*
- Limit czasu połączenia &nbsp; &nbsp; *(wartość domyślna to 15 sekundach. Zakres jest od 0 do 2147483647)*


W szczególności następujące PRAWDA równości powinny sprawić, wybrane wartości:

- Limit czasu połączenia = ConnectRetryCount * ConnectionRetryInterval

Na przykład jeśli liczba = 3 i interwał = 10 sekund, limit czasu równy tylko do 29 sekund pozwoli nie dość uzyskać systemu wystarczającą ilość czasu na jej 3 i końcowe ponów próbę na połączenie: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Połączenia i polecenia


Parametry **ConnectRetryCount** i **ConnectRetryInterval** umożliwiają obiektu **SqlConnection** ponawiania operacji połączenia bez informacją lub bothering programu, takich jak sterowanie wróci do programu. Ponowne próby może wystąpić w następujących sytuacjach:

- mySqlConnection.Open metody połączenia
- mySqlConnection.Execute metody połączenia

Istnieje subtlety. Jeśli przejściowych błąd występuje, gdy jest wykonywane *kwerendy* , obiektu **SqlConnection** nie ponowić próbę Połącz i go na pewno nie ponawiania kwerendy. Jednak **SqlConnection** bardzo szybko sprawdzić połączenie przed wysłaniem kwerendy do wykonania. Jeśli szybkie sprawdzenie wykryje problem z połączeniem, **SqlConnection** prób operacji połączenia. Jeśli próba zakończyło się powodzeniem, kwerenda jest wysyłana do wykonania.


#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>ConnectRetryCount powinna być połączone z aplikacji ponów próbę logicznych?

Załóżmy, że aplikacja ma rozbudowany niestandardowego ponów próbę logicznych. Mogą go ponowienia operacji połączenia 4 razy. Jeśli dodasz **ConnectRetryInterval** i **ConnectRetryCount** = 3 do ciągu połączenia, zwiększa licznik do 4 * 3 = 12 ponowne próby. Z dużą liczbą prób nie mogą być planowane.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Połączenia z bazą danych Azure SQL

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Połączenia: Parametry połączenia


Parametry połączenia niezbędne do łączenia się z bazą danych SQL Azure nieznacznie różni się od ciągu do łączenia się z programu Microsoft SQL Server. Parametry połączenia dla bazy danych można kopiować z [Azure Portal](https://portal.azure.com/).


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Połączenia: Adres IP


Należy skonfigurować serwer bazy danych SQL, aby zaakceptować komunikacji z adres IP komputera obsługującego program klient. W tym celu edycji ustawienia zapory za pomocą [Azure Portal](https://portal.azure.com/).


Jeśli zapomnisz skonfigurować adres IP, program zakończy się niepowodzeniem z przydatnego komunikat informujący niezbędne adres IP.


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


Aby uzyskać więcej informacji, zobacz: [jak: Konfigurowanie ustawień zapory w bazie danych SQL](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Połączenie: porty


Zwykle wystarczy upewnić się, że port 1433 jest otwarte dla komunikacji wychodzącej na komputerze obsługującym program klient.


Na przykład kiedy program Klient znajduje się na komputerze z systemem Windows, zapory systemu Windows na hoście umożliwia otwarcie portu 1433:


1. Otwórz Panel sterowania
2. &gt;Wszystkie elementy Panelu sterowania
3. &gt;Zapora systemu Windows
4. &gt;Ustawienia zaawansowane
5. &gt;Reguły ruchu wychodzącego
6. &gt;Akcje
7. &gt;Nowa reguła


Jeśli używany program klient jest hostowana w Azure maszyn wirtualnych (maszyn wirtualnych), należy przeczytać:<br/>[Porty poza 1433 dla ADO.NET 4,5 i wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md).


Aby uzyskać informacje dotyczące cofiguration porty i adresu IP, zobacz: [Zapora bazy danych SQL Azure](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Połączenie: ADO.NET 4.6.1


Jeśli używany program używa klas ADO.NET, takich jak **System.Data.SqlClient.SqlConnection** nawiązywania połączenia z bazą danych SQL Azure, zaleca się użycie .NET Framework w wersji 4.6.1 lub nowszym.


ADO.NET 4.6.1:

- Bazy danych SQL Azure jest większa niezawodność podczas otwierania połączenia przy użyciu metody **SqlConnection.Open** . Metody **Open** zawiera teraz najlepsze mechanizmy ponów próbę nakładu w odpowiedzi na błędy przejściowych, niektórych błędów w okresie limit czasu połączenia.
- Obsługa Pule połączeń. Ta opcja uwzględnia skutecznej weryfikacji, pełniącej obiekt połączenia oferuje program.



Podczas korzystania z obiektu połączenia z puli połączeń, zaleca się, że program tymczasowo zamknąć połączenie, gdy nie bezpośrednio przy użyciu. Ponowne otwieranie połączenia nie jest drogich sposób tworzenia nowego połączenia.


Jeśli używasz ADO.NET 4.0 lub starszy, zaleca się uaktualnienie do najnowszej ADO.NET.

- Od listopada 2015 r. możesz [pobrać ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Narzędzia diagnostyczne

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnostyka: Sprawdzić, czy można połączyć narzędzia


Jeśli używany program kończy się niepowodzeniem nawiązywania połączenia z bazą danych SQL Azure, jedna opcja diagnostyczne jest próby nawiązania połączenia z programem narzędzie. Najlepiej narzędzie chcesz połączyć przy użyciu tej samej biblioteki, używany przez program.


Na dowolnym komputerze z systemem Windows możesz wykonać tych narzędzi:

- SQL Server Management Studio (ssms.exe), który łączy przy użyciu ADO.NET.
- sqlcmd.exe, który łączy przy użyciu [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).


Po połączeniu, należy sprawdzić, czy działa krótki kwerenda SQL SELECT.


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnostyka: Sprawdzanie otwarte porty


Załóżmy, że istnieje podejrzenie niepowodzeniu próby nawiązania połączenia z powodu problemów z portu. Na komputerze można uruchamiać raporty dotyczące konfiguracji portów narzędzie.


W systemie Linux następujących narzędzi mogą być pomocne:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (Zmień wartość przykład do swojego adresu IP).


W systemie Windows [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) narzędzie może być przydatne. Oto przykład wykonanie który proszeni sytuację portu na serwerze bazy danych SQL Azure, a które zostało uruchomione na komputerze przenośnym:


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnostycznego: Błędy dziennika


Przejściowymi problem czasami najlepiej jest zdiagnozowanie wykrywania ogólne deseń nad dni lub tygodni.


Klient może pomóc w diagnostyki przez funkcję rejestrowania wszystkie błędy, które napotkania. Można dostosować wpisy dziennika z danymi błędu, które bazy danych SQL Azure loguje się wewnętrznie.


6 biblioteki przedsiębiorstwa (EntLib60) oferuje klas .NET zarządzane pomagające rejestrowania:

- [5 - tak proste jak objętych wyłączanie dziennika: za pomocą funkcji blokowania rejestrowania aplikacji](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnostyka: Sprawdzenie dzienniki systemu pod kątem błędów


Poniżej przedstawiono niektóre instrukcji Transact-SQL zaznacz tego zapytania dzienniki błędów i inne informacje.


| Kwerendy dziennika | Opis |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` | Widok [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) zawiera informacje o pojedynczych zdarzeń, łącznie z tymi, które mogą powodować błędy przejściowych lub błędów połączenia.<br/><br/>Najlepiej **godzina_rozpoczęcia** lub **end_time** wartości mogą być zgodne z dowiedzieć się, gdy program Klient wystąpienia problemów.<br/><br/>**Porada:** Należy połączyć z **wzorcem** bazy danych, aby uruchomić to. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` | Widok [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) oferuje zagregowane liczby typów zdarzeń dla dodatkowych opcji diagnostycznych.<br/><br/>**Porada:** Należy połączyć z **wzorcem** bazy danych, aby uruchomić to. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnostyka: Wyszukiwanie problem zdarzenia w dzienniku bazy danych SQL


Można wyszukiwać wpisów dotyczących problemu zdarzeń w dzienniku bazy danych SQL Azure. Wypróbuj następującą instrukcję SELECT w języku Transact-SQL **wzorca** bazy danych:


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Kilka zwracanych wierszy z sys.fn_xe_telemetry_blob_target_read_file


Następny jest zwracanych wierszy może wyglądać następująco. Wartości null wyświetlane są często nie zawiera wartości null w innych wierszach.


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Biblioteka Enterprise 6


6 biblioteki przedsiębiorstwa (EntLib60) jest strukturą klas .NET pomagające zaimplementować niezawodne klientów usług w chmurze, z których jedna jest usługa bazy danych SQL Azure. Możesz odnaleźć zarezerwowane dla każdego obszaru, w którym mogą pomóc EntLib60 pierwszy znaleźć tematy:

- [Biblioteka Enterprise 6 — kwiecień 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


Logika ponawiania obsługi błędów przejściowych jest jednego obszaru, w którym mogą pomóc EntLib60:

- [4 - perseverance, tajny wszystkie sukcesy: za pomocą funkcji blokowania aplikacji obsługi błędów przejściowych](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


> [AZURE.NOTE] Kod źródłowy EntLib60 jest dostępna dla publicznej [Pobieranie](http://go.microsoft.com/fwlink/p/?LinkID=290898). Firma Microsoft udostępnia żadnych planów, aby wprowadzić dalsze aktualizacji funkcji lub konserwacji aktualizacje do EntLib.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>Klasy EntLib60 przejściowych błędów i spróbuj ponownie


Następujące klasy EntLib60 są szczególnie przydatne dla ponów próbę logicznych. Wszystkie te znajdują się w lub wchodzi w obszarze nazw **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*W obszarze nazw* *Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling* *:*

- Klasy **RetryPolicy**
 - Metody **ExecuteAction**


- Klasy **ExponentialBackoff**


- Klasy **SqlDatabaseTransientErrorDetectionStrategy**


- Klasy **ReliableSqlConnection**
 - Metoda **ExecuteCommand**


W obszarze nazw **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- Klasy **AlwaysTransientErrorDetectionStrategy**

- Klasy **NeverTransientErrorDetectionStrategy**


Oto łącza do informacji o EntLib60:

- Bezpłatne [książki plik do pobrania: Przewodnik programisty do biblioteki Microsoft Enterprise, wydanie 2](http://www.microsoft.com/download/details.aspx?id=41145)

- Najważniejsze wskazówki: [Ponów próbę ogólne wskazówki](../best-practices-retry-general.md) jest doskonałym szczegółowe omówienie ponów próbę logicznych.

- Pobieranie NuGet [Biblioteka Enterprise — przejściowych obsługi błędów aplikacji blok 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Bloku rejestrowania


- Blokowanie rejestrowanie jest bardzo elastyczne i można konfigurować rozwiązanie, która umożliwia:
 - Tworzenie i przechowywanie wiadomości dziennika w wielu różnych lokalizacji.
 - Kategoryzuj i filtrowanie wiadomości.
 - Zbieranie informacji kontekstowych, będącej wymagania rejestrowania przydatne debugowanie i śledzenie, a także Inspekcja i ogólne.


- Blokowanie rejestrowanie abstracts funkcji rejestrowania docelowego dziennika, tak, aby kod aplikacji jest zgodna, niezależnie od lokalizacji i typu magazynu rejestrowanie docelowej.


Aby uzyskać szczegółowe informacje, zobacz: [5 - jako łatwe jako objęte wyłączanie dziennika: za pomocą funkcji blokowania rejestrowania aplikacji](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>Kod źródłowy metody EntLib60 IsTransient


Następnie z klasy **SqlDatabaseTransientErrorDetectionStrategy** jest kod źródłowy C# metody **IsTransient** . Kod źródłowy zawiera wyjaśnienia, które błędy zostały uznane za przejściowych i warta ponów próbę od kwietnia 2013.

Wiele wierszy **//comment** zostały usunięte z tej kopii, aby wyróżnić czytelności.


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Następne kroki

- Inne typowe bazy danych SQL Azure Rozwiązywanie problemów z połączeniem, można znaleźć [Rozwiązywanie problemów połączenia z bazą danych SQL Azure](sql-database-troubleshoot-common-connection-issues.md).

- [Połączenie z serwerem SQL buforowanie (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*Ponawianie* jest Apache 2.0 licencjonowany ogólnego przeznaczenia ponawianie biblioteki, napisana **Python**, aby uprościć zadanie dodawania zachowanie ponów próbę do niemal każdego elementu.](https://pypi.python.org/pypi/retrying)
