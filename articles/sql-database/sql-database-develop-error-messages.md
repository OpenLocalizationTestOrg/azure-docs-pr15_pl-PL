<properties
    pageTitle="Kody błędów SQL - błąd połączenia bazy danych | Microsoft Azure"
    description="Informacje na temat kodów błędów SQL aplikacje klienckie bazy danych SQL, takich jak typowych błędów połączenia bazy danych, problemy kopii bazy danych i ogólnych błędów. "
    keywords="Kod błędu SQL, program access sql, błąd połączenia bazy danych sql kody błędów"
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="annemill"/>


# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>Kody błędów SQL dla aplikacji klienckich bazy danych SQL: błąd połączenia i innych problemów z bazy danych


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


Ten artykuł zawiera listę kodów błędów SQL dla aplikacji klienckich bazy danych SQL, w tym błędy połączenia bazy danych, przejściowych błędów (zwanych również przejściowych błędów), błędy zarządzania zasobów, problemy kopii bazy danych, elastyczne puli i inne błędy. Większość kategorie dla bazy danych SQL Azure, a nie mają zastosowania do programu Microsoft SQL Server.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Błędy połączenia bazy danych, błędy przejściowych i innych tymczasowe błędy

Poniższej tabeli opisano kody błędów SQL błędów utraty połączenia i inne błędy przejściowych, które mogą wystąpić, gdy aplikacja próbuje uzyskać dostęp do bazy danych SQL. Aby uzyskać Rozpoczynanie pracy — samouczki dotyczące nawiązywania połączenia z bazą danych SQL Azure, zobacz [Nawiązywanie połączenia z bazą danych SQL Azure](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Najbardziej typowe błędy połączenia bazy danych i przejściowych błędy

Azure infrastruktury ma możliwość dynamiczne zmiany konfiguracji serwerów, gdy intensywnie obciążenia mogą pojawić się w usłudze bazy danych SQL.  To zachowanie dynamiczne może spowodować utratę połączenia z bazą danych SQL program klient. Ten rodzaj błąd jest określana mianem *przejściowych błędów*.

Jeśli używany program klient ma ponów próbę logicznych, może użyć do przywrócenia połączenia po określeniu czasu przejściowych błędów poprawianie sam.  Firma Microsoft zaleca opóźnienie 5 sekund przed do pierwszej ponów próbę. Ponawianie opóźnieniem krótsza ryzyka 5 sekund przeciążając uda się rozpoznać usług w chmurze. Dla każdej kolejnej ponów próbę opóźnienie powinien być zwiększany sposób wykładniczy maksymalnie do 60 sekund.

Błędy przejściowych pojawiają się zazwyczaj w jednym z następujących komunikatów o błędach z programów klienta:

- Baza danych < db_name > na serwerze < Azure_instance > nie jest obecnie dostępna. Spróbuj ponownie później połączenie. Jeśli problem będzie nadal występował, skontaktuj się z obsługą klienta i podaj im identyfikator śledzenia sesji < session_id >

- Baza danych < db_name > na serwerze < Azure_instance > nie jest obecnie dostępna. Spróbuj ponownie później połączenie. Jeśli problem będzie nadal występował, skontaktuj się z obsługą klienta i podaj im identyfikator śledzenia sesji < session_id >. (Program Microsoft SQL Server, błąd: 40613)

- Istniejące połączenie zostało zamknięte przez zdalnego hosta.

- System.Data.Entity.Core.EntityCommandExecutionException: Wystąpił błąd podczas wykonywania polecenia definicji. Zobacz wyjątek wewnętrzny, aby uzyskać szczegółowe informacje. ---> System.Data.SqlClient.SqlException: Wystąpił błąd poziomu transportu podczas odbierania wyników z serwera. (Dostawca: Dostawca sesji błędu: 19 – fizycznego połączenia nie jest używany)

Przykłady kodu ponów próbę logicznych zobacz:

- [Biblioteki połączeń dla bazy danych SQL i programu SQL Server](sql-database-libraries.md) 
- [Akcje, aby naprawić błędy połączenia i przejściowych błędy w bazie danych SQL](sql-database-connectivity-issues.md)

Dyskusja *blokowania przez okres* dla klientów korzystających z ADO.NET jest dostępna w [SQL Server połączenia buforowanie (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Kody błędów przejściowych błędów

Następujące błędy są przejściowych i powinny być ponowna próba przetworzenia w logiki aplikacji 

| Kod błędu | Ważności | Opis |
| ---: | ---: | :--- |
| 4060 | 16 | Nie można otworzyć bazy danych "%. & #x2a; ls" zlecone przez logowania. Błąd logowania. |
|40197|17|Usługa wystąpił błąd podczas przetwarzania żądania. Spróbuj ponownie. Kod błędu %d.<br/><br/>Otrzymasz ten błąd, gdy usługa nie działa z powodu oprogramowania lub sprzętu uaktualnienia, awarie sprzętu lub inne problemy pracy awaryjnej. Kod błędu (%d), osadzone w komunikacie o błędzie 40197 udostępnia dodatkowe informacje dotyczące rodzaju błąd ani przełączenie awaryjne, który wystąpił. Przykładowe kody są osadzone w komunikacie o błędzie 40197 błędów to 40020, 40143, 40166 i 40540.<br/><br/>Ponowne łączenie się z serwerem bazy danych SQL zostanie automatycznie połączyć się prawidłowy kopii bazy danych. Aplikacja należy efektywnej 40197, dziennik błędów kod błędu osadzone (%d) w wiadomości dotyczących rozwiązywania problemów i ponowić próbę połączenia z bazą danych SQL, aż zasoby są dostępne, a połączenie jest ustanowione ponownie.|
|40501|20|Usługa jest obecnie zajęty. Ponawianie żądania po 10 sekund. Identyfikator zdarzenia: %ls. Kod: %d.<br/><br/>*Uwaga:* Aby uzyskać więcej informacji zobacz:<br/>• [Ograniczenia zasobów bazy danych SQL azure](sql-database-resource-limits.md).
|40613|17|Baza danych "%. & #x2a; ls" na serwerze "%. & #x2a; ls" nie jest obecnie dostępny. Spróbuj ponownie później połączenie. Jeśli problem będzie nadal występował, skontaktuj się z obsługą klienta i podaj im identyfikator śledzenia sesji "%. & #x2a; ls".|
|49918|16|Nie może przetworzyć żądania. Mało zasobów żądania.<br/><br/>Usługa jest obecnie zajęty. Spróbuj ponownie później wezwanie. |
|49919|16|Nie można utworzyć procesu lub zaktualizować żądanie. Zbyt wiele Tworzenie lub aktualizowanie operacji w toku dla subskrypcji "% ld".<br/><br/>Usługa jest zajęta przetwarzania wielu Tworzenie lub aktualizowanie żądania dla danej subskrypcji lub serwera. Żądania obecnie są blokowane zoptymalizować zasobów. Kwerendy [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) dla operacji oczekujących. Poczekaj, aż do przeprowadzenia oczekujących Utwórz lub Aktualizuj żądania są wykonane lub usunąć jeden z oczekujące żądania i ponowić żądanie później. |
|49920|16|Nie może przetworzyć żądania. Zbyt wiele operacji w toku dla subskrypcji "% ld".<br/><br/>Usługa jest zajęty, przetwarzanie wielu żądań dla tej subskrypcji. Żądania obecnie są blokowane zoptymalizować zasobów. Kwerendy [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) dla stan operacji. Zaczekaj, aż do czasu żądania zostanie ukończona lub usunąć jeden z oczekujące żądania i ponowić żądanie później. |

## <a name="database-copy-errors"></a>Błędy kopii bazy danych

Następujące błędy mogą wystąpić podczas kopiowania bazy danych w bazie danych SQL Azure. Aby uzyskać więcej informacji zobacz [Kopiowanie bazy danych SQL Azure](sql-database-copy.md).


|Kod błędu|Ważności|Opis|
|---:|---:|:---|
|40635|16|Klienta z adresem IP "%. & #x2a; ls" jest tymczasowo wyłączony.|
|40637|16|Tworzenie kopii bazy danych jest wyłączony.|
|40561|16|Kopiowanie bazy danych nie powiodło się. Źródłowej lub docelowej bazy danych nie istnieje.|
|40562|16|Kopiowanie bazy danych nie powiodło się. Został usunięty źródłowej bazy danych.|
|40563|16|Kopiowanie bazy danych nie powiodło się. Został usunięty docelowej bazy danych.|
|40564|16|Kopiowanie bazy danych nie powiodło się ze względu na błąd wewnętrzny. Usuwanie docelowy bazy danych i spróbuj ponownie.|
|40565|16|Kopiowanie bazy danych nie powiodło się. Nie więcej niż 1 kopie równoczesne bazy danych z tym samym źródłem jest dozwolone. Usuwanie docelowej bazy danych i spróbuj ponownie później.|
|40566|16|Kopiowanie bazy danych nie powiodło się ze względu na błąd wewnętrzny. Usuwanie docelowy bazy danych i spróbuj ponownie.|
|40567|16|Kopiowanie bazy danych nie powiodło się ze względu na błąd wewnętrzny. Usuwanie docelowy bazy danych i spróbuj ponownie.|
|40568|16|Kopiowanie bazy danych nie powiodło się. Źródłowej bazy danych ma stają się niedostępne. Usuwanie docelowy bazy danych i spróbuj ponownie.|
|40569|16|Kopiowanie bazy danych nie powiodło się. Docelowej bazy danych ma stają się niedostępne. Usuwanie docelowy bazy danych i spróbuj ponownie.|
|40570|16|Kopiowanie bazy danych nie powiodło się ze względu na błąd wewnętrzny. Usuwanie docelowej bazy danych i spróbuj ponownie później.|
|40571|16|Kopiowanie bazy danych nie powiodło się ze względu na błąd wewnętrzny. Usuwanie docelowej bazy danych i spróbuj ponownie później.|

## <a name="resource-governance-errors"></a>Błędy zarządzania zasobów

Następujące błędy są powodowanych przez nadmiarowe użycia zasobów podczas pracy z bazą danych SQL Azure. Na przykład:

- Transakcji zostało otwarte przez dłuższy czas.
- Transakcji jest zbyt wiele blokują.
- Aplikacja jest użyciu zbyt dużej ilości pamięci.
- Aplikacja jest zbyt dużo używające `TempDb` miejsca.

Tematy pokrewne:

* Bardziej szczegółowe informacje znajdują się tutaj: [limity zasobów bazy danych SQL Azure](sql-database-resource-limits.md)

|Kod błędu|Ważności|Opis|
|---:|---:|:---|
|10928|20|Identyfikator zasobu: %d. Limit %s dla bazy danych jest %d i osiągnięto. Aby uzyskać więcej informacji zobacz [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>Identyfikator zasobu wskazuje zasób, którego osiągnięto limit. Dla wątków, identyfikator zasobu = 1. Dla sesji, identyfikator zasobu = 2.<br/><br/>*Uwaga:* Aby uzyskać więcej informacji na temat tego błędu i sposobu rozwiązania problemu zobacz:<br/>• [Ograniczenia zasobów bazy danych SQL azure](sql-database-resource-limits.md). |
|10929|20|Identyfikator zasobu: %d. Gwarancji minimalne %s jest %d, maksymalny limit wynosi %d i bieżące użycie bazy danych jest %d. Jednak serwer jest obecnie zbyt zajęty do obsługi żądań większa niż %d dla tej bazy danych. Aby uzyskać więcej informacji zobacz [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). W przeciwnym razie spróbuj ponownie później.<br/><br/>Identyfikator zasobu wskazuje zasób, którego osiągnięto limit. Dla wątków, identyfikator zasobu = 1. Dla sesji, identyfikator zasobu = 2.<br/><br/>*Uwaga:* Aby uzyskać więcej informacji na temat tego błędu i sposobu rozwiązania problemu zobacz:<br/>• [Ograniczenia zasobów bazy danych SQL azure](sql-database-resource-limits.md).|
|40544|20|Baza danych osiągnęła swój limit przydziału. Partycją lub Usuń dane, upuść indeksy lub zapoznaj się z dokumentacją możliwe rozwiązania.|
|40549|16|Ponieważ transakcji długim zakończeniem sesji. Spróbuj skrócenie transakcji.|
|40550|16|Sesja zostało zakończone, ponieważ uzyskał go zbyt wiele blokady. Spróbuj odczytu lub modyfikowanie mniejszej liczby wierszy w jednej transakcji.|
|40551|16|Sesja zostało zakończone ze względu na nadmierne `TEMPDB` zastosowania. Spróbuj zmodyfikowanie kwerendy w celu zmniejszenia wykorzystanie miejsca tabeli tymczasowej.<br/><br/>*Porada:* Jeśli korzystasz z obiektów tymczasowych zaoszczędzić miejsce w `TEMPDB` bazy danych przez upuszczanie obiektów tymczasowych po ich nie są już potrzebne w sesji.|
|40552|16|Ze względu na użycie miejsca dziennika nadmiarowe transakcji zostało zakończone sesji. Spróbuj modyfikowanie mniejszej liczby wierszy w jednej transakcji.<br/><br/>*Porada:* W przypadku wykonywania zbiorczej powoduje wstawienie przy użyciu `bcp.exe` utility lub `System.Data.SqlClient.SqlBulkCopy` klasy, należy spróbować użyć `-b batchsize` lub `BatchSize` skopiowany opcje służące do ograniczania liczby wierszy na serwerze w każdej transakcji. Odbudowywania indeksu z `ALTER INDEX` instrukcji, spróbuj użyć `REBUILD WITH ONLINE = ON` opcji.|
|40553|16|Ze względu na użycie pamięci nadmiarowe zostało zakończone sesji. Spróbuj zmodyfikowanie kwerendy przetwarzania mniejszej liczby wierszy.<br/><br/>*Porada:* Zmniejszenie liczby `ORDER BY` i `GROUP BY` operacje w kodzie języku Transact-SQL zmniejsza wymagania dotyczące pamięci zapytania.|

## <a name="elastic-pool-errors"></a>Błędy puli elastyczne

Następujące błędy odnoszą się do tworzenia i używania pul Elastics.

| Numer_błędu | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX_RESOURCE | Elastyczne puli osiągnięto limit miejsca do magazynowania. Użycie miejsca do magazynowania dla puli elastyczne nie może przekraczać MB (%d). | Elastyczne puli limit miejsca w MB. | Próba zapisywać danych w bazie danych, gdy osiągnięto limit miejsca do magazynowania elastyczne puli. | Należy rozważyć zwiększenie DTUs elastyczne puli, jeśli to możliwe, aby zwiększyć limit miejsca do magazynowania, zmniejszenie Magazyn używany przez pojedyncze bazy danych w puli elastyczne lub usuwanie baz danych z puli elastyczne. |
| 10929 | EX_USER | Gwarancji minimalne %s jest %d, maksymalny limit wynosi %d i bieżące użycie bazy danych jest %d. Jednak serwer jest obecnie zbyt zajęty do obsługi żądań większa niż %d dla tej bazy danych. Zobacz [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) w celu uzyskania pomocy. W przeciwnym razie spróbuj ponownie później. | Min DTU na bazę danych; Maksymalna liczba DTU na bazę danych | Łączna liczba równoczesne pracowników (liczba żądań) przez wszystkie bazy danych w puli elastyczne próbował przekracza limit puli. | Należy rozważyć zwiększenie DTUs elastyczne puli, jeśli to możliwe w celu zwiększenia jego limit pracownik lub usuwanie baz danych z puli elastyczne. |
| 40844 | EX_USER | Baza danych "%ls" na serwerze "%ls" jest baza danych edition "%ls" w puli elastycznych i nie może mieć relacji ciągły Kopiuj. | Nazwa bazy danych, edition bazy danych, nazwa serwera | Polecenie StartDatabaseCopy jest wydawany dla zwykłych db w puli elastyczne. | Wkrótce |
| 40857 | EX_USER | Elastyczne puli nie można odnaleźć serwera: nazwę elastyczne puli, "%ls": "%ls". | Nazwa serwera. Nazwa puli elastyczne | Określonej puli elastyczne nie istnieje w określonej server. | Podaj nazwę prawidłowych puli elastyczne. |
| 40858 | EX_USER | Elastyczne pula '%ls' już istnieje na serwerze: "%ls" | elastyczne puli, nazwa serwera | Określonej puli elastyczne już istnieje w określonym serwerze logiczne. | Podaj nową nazwę puli elastyczne. |
| 40859 | EX_USER | Elastyczne puli nie obsługuje warstwa usług "%ls". | elastyczne puli warstwa usług | Warstwa określonych usług nie jest obsługiwane dla puli elastyczne inicjowania obsługi administracyjnej. | Udostępnianie poprawnej edycji lub pozostaw puste pole, aby użyć domyślnego poziomu usług warstwa usług. |
| 40860 | EX_USER | Elastyczne puli "%ls" i usługa kombinacja cel "%ls" jest nieprawidłowa. | Nazwa puli elastyczną; Nazwa była poziomu usług | Elastyczne puli i usługa cel można wprowadzić jednocześnie tylko wtedy, gdy usługa celem jest określony jako "ElasticPool". | Określ poprawne kombinację puli elastycznych i cel usługi. |
| 40861 | EX_USER | Edition bazy danych "%. *ls nie może być inne niż warstwa usług elastyczne puli, czyli "%.* ls. | Baza danych edition warstwa usług elastyczne puli | Edition bazy danych jest inny niż warstwa usług elastyczne puli. | Określ nie edition bazy danych, który różni się od poziomu usług elastyczne puli.  Zauważ, że edition bazy danych nie musi być określony. |
| 40862 | EX_USER | Elastyczne puli nazwa musi być określony, jeśli określono wskaźniku elastyczne puli usługi. | Brak | Elastyczne puli usługi cel nie jednoznacznie identyfikować puli elastyczne. | Określ nazwę elastyczne puli, jeśli przy użyciu wskaźniku elastyczne puli usługi. |
| 40864 | EX_USER | DTUs elastyczne puli musi mieć co najmniej (%d) DTUs, dla poziomu usług "%. * ls. | DTUs elastyczne puli; elastyczne puli usługi warstwa. | Próba ustawienia DTUs elastyczne puli poniżej minimalnego limitu. | Spróbuj ponownie, ustawienie DTUs dla elastyczne puli co najmniej minimalne ograniczenie. |
| 40865 | EX_USER | DTUs elastyczne puli nie może przekraczać (%d) DTUs, dla poziomu usług "%. * ls. | DTUs elastyczne puli; elastyczne puli usługi warstwa. | Próba ustawienia DTUs elastyczne puli powyżej maksymalnej. | Spróbuj ponownie, ustawienie DTUs elastyczne puli nie przekracza maksymalny limit. |
| 40867 | EX_USER | DTU max na bazę danych musi wynosić co najmniej (%d) dla poziomu usług "%. * ls. | Maksymalna liczba DTU na bazę danych; elastyczne puli warstwa usług | Próba ustawić max DTU na bazę danych, poniżej obsługiwany limit. | Należy rozważyć użycie warstwa usług elastyczne puli obsługującego odpowiednie ustawienie. |
| 40868 | EX_USER | Maksymalna liczba na bazę danych DTU nie może przekraczać (%d) dla poziomu usług "%. * ls. | Maksymalna liczba DTU na bazę danych; elastyczne puli usługi warstwa. | Próba ustawić max DTU na bazę danych poza obsługiwany limit. | Należy rozważyć użycie warstwa usług elastyczne puli obsługującego odpowiednie ustawienie. |
| 40870 | EX_USER | Min DTU na bazę danych nie może przekraczać (%d) dla poziomu usług "%. * ls. | Min DTU na bazę danych; elastyczne puli usługi warstwa. | Próba ustawić min DTU na bazę danych poza obsługiwany limit. | Należy rozważyć użycie warstwa usług elastyczne puli obsługującego odpowiednie ustawienie. |
| 40873 | EX_USER | Liczba baz danych (%d) i min DTU na bazę danych (%d) nie może przekraczać DTUs elastyczne puli (%d). | Liczba baz danych w puli elastyczną; Min DTU na bazę danych; DTUs elastyczne puli. | Próba określić min DTU baz danych w puli elastyczne, którego rozmiar przekracza DTUs elastyczne puli. | Należy rozważyć zwiększenie DTUs elastyczne puli, lub Zmniejsz min DTU na bazę danych lub Zmniejsz liczbę baz danych w puli elastyczne. |
| 40877 | EX_USER | Nie można usunąć puli elastyczne, o ile nie zawiera żadnych baz danych. | Brak | Elastyczne pula zawiera jedną lub więcej baz danych i nie można usunąć. | Usuń baz danych z puli elastyczne aby można było go usunąć. |
| 40881 | EX_USER | Elastyczne puli "%. * ls osiągnięto limit liczby bazy danych.  Limit liczby bazy danych dla puli elastyczne nie może przekraczać (%d) dla puli elastyczną z (%d) DTUs. | Nazwa puli elastyczną; limit liczby bazy danych elastyczne puli, e DTUs puli zasobów. | Próba Tworzenie lub dodawanie bazy danych do puli elastyczne, gdy osiągnięto limit liczby bazy danych elastyczne puli. | Należy rozważyć zwiększenie DTUs elastyczne puli, jeśli to możliwe w celu zwiększenia jego limitu bazy danych lub usuwanie baz danych z puli elastyczne. |
| 40889 | EX_USER | DTUs lub limit miejsca do magazynowania dla puli elastyczne "%. * ls nie można zmniejszyć, od którego nie zapewnia wystarczająco dużo miejsca do magazynowania dla bazy danych. | Nazwa puli elastyczne. | Próba zmniejszyć limit magazynowania wynoszący puli elastyczne pod jej użycie miejsca do magazynowania. | Należy rozważyć użycie miejsca do magazynowania poszczególnych baz danych w puli elastyczne zmniejszania lub usunięcie baz danych z puli, aby zmniejszyć jej DTUs lub limit magazynowania. |
| 40891 | EX_USER | Min DTU na bazę danych (%d) nie może przekraczać max DTU na bazę danych (%d). | Min DTU na bazę danych; DTU max na bazę danych. | Próba ustawić min DTU na bazę danych przekracza maksymalny DTU na bazę danych. | Upewnij się, że min DTU na bazy danych nie przekracza maksymalny DTU na bazę danych. |
| DO OPRACOWANIA PÓŹNIEJ | EX_USER | Rozmiar miejsca do magazynowania dla poszczególnych bazy danych w puli elastyczne nie może przekraczać maksymalny rozmiar zezwala na to "%. * ls usługi puli elastyczne warstwy. | elastyczne puli warstwa usług | Maksymalny rozmiar bazy danych przekracza maksymalny rozmiar zezwala na warstwie elastyczne puli usługi. | Ustaw maksymalny rozmiar bazy danych w ramach maksymalny rozmiar zezwala na warstwie elastyczne puli usługi. |

Tematy pokrewne:

* [Tworzenie puli elastyczne bazy danych (C#)](sql-database-elastic-pool-create-csharp.md) 
* [Zarządzanie puli elastyczne bazy danych (C#)](sql-database-elastic-pool-manage-csharp.md). 
* [Tworzenie puli elastyczne bazy danych (programu PowerShell)](sql-database-elastic-pool-create-powershell.md) 
* [Monitor i zarządzanie nimi puli elastyczne bazy danych (programu PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Błędy ogólne

Następujące błędy nie należą do dowolnej poprzedniej kategorii.

|Kod błędu|Ważności|Opis|
|---:|---:|:---|
|15006|16|<AdministratorLogin>nie jest prawidłową nazwę ponieważ zawiera nieprawidłowe znaki.|
|18452|14|Logowanie nie powiodło się. Podczas logowania jest z niezaufanych domeny i nie można używać w systemie Windows authentication.%. & #x2a; ls (logowania do systemu Windows nie są obsługiwane w tej wersji programu SQL Server).|
|18456|14|Niepowodzenie logowania użytkownika ' %. #x2a;ls'.%. & #x2a % ls. & #x2a; ls (identyfikator logowania użytkownika nie powiodło się "%. & #x2a; ls". Nie można zmienić hasła. Zmienianie hasła podczas logowania nie jest obsługiwane w tej wersji programu SQL Server.)|
|18470|14|Niepowodzenie logowania użytkownika "%. & #x2a; ls". Przyczyna: Konto jest disabled.%. & #x2a; ls|
|40014|16|Wiele baz danych nie można używać w tej samej transakcji.|
|40054|16|Tabele bez grupowany indeksu nie są obsługiwane w tej wersji programu SQL Server. Tworzenie indeksu opartego na grupowany i spróbuj ponownie.|
|40133|15|Operacja nie jest obsługiwana w tej wersji programu SQL Server.|
|40506|16|Określony SID jest nieprawidłowy dla tej wersji programu SQL Server.|
|40507|16|"%. & #x2a; ls nie można wywołać z parametrów w tej wersji programu SQL Server.|
|40508|16|Używanie instrukcji nie jest obsługiwane przełączanie baz danych. Nawiązywanie połączenia z innej bazy danych za pomocą nowe połączenie.|
|40510|16|Instrukcja "%. & #x2a; ls" nie jest obsługiwane w tej wersji programu SQL Server|
|40511|16|Wbudowanych funkcji "%. & #x2a; ls" nie jest obsługiwane w tej wersji programu SQL Server.|
|40512|16|Funkcja przestarzałych "%ls" nie jest obsługiwana w tej wersji programu SQL Server.|
|40513|16|Serwer zmiennych "%. & #x2a; ls" nie jest obsługiwane w tej wersji programu SQL Server.|
|40514|16|"%ls" nie jest obsługiwane w tej wersji programu SQL Server.|
|40515|16|Odwołanie do nazwy bazy danych i/lub serwera w "%. & #x2a; ls" nie jest obsługiwane w tej wersji programu SQL Server.|
|40516|16|Obiekty temp globalne nie są obsługiwane w tej wersji programu SQL Server.|
|40517|16|Opcja słowo kluczowe lub instrukcji "%. & #x2a; ls" nie jest obsługiwane w tej wersji programu SQL Server.|
|40518|16|Polecenie DBCC "%. & #x2a; ls" nie jest obsługiwane w tej wersji programu SQL Server.|
|40520|16|S_MSG zabezpieczanych zajęć "%" nie obsługiwane w tej wersji programu SQL Server.|
|40521|16|S_MSG zabezpieczanych zajęć "%" nieobsługiwane w zakresie serwera w tej wersji programu SQL Server.|
|40522|16|Typ kapitału "%. & #x2a; ls" bazy danych nie jest obsługiwany w tej wersji programu SQL Server.|
|40523|16|Tworzenie "%. & #x2a; ls" niejawne użytkownika nie jest obsługiwane w tej wersji programu SQL Server. Przed użyciem jawnie utworzyć użytkownika.|
|40524|16|Typ danych "%. & #x2a; ls" nie jest obsługiwane w tej wersji programu SQL Server.|
|40525|16|Z "%.ls" nie jest obsługiwane w tej wersji programu SQL Server.|
|40526|16|"%. & #x2a; ls dostawcy wierszy nie jest obsługiwany w tej wersji programu SQL Server.|
|40527|16|Serwerów połączonych nie są obsługiwane w tej wersji programu SQL Server.|
|40528|16|Użytkownicy nie można mapować certyfikaty, asymetrycznym klawiszy lub logowania do systemu Windows w tej wersji programu SQL Server.|
|40529|16|Wbudowanych funkcji "%. & #x2a; ls" w personifikacji kontekstu nie jest obsługiwany w tej wersji programu SQL Server.|
|40532|11|Nie można otworzyć serwera "%. & #x2a; ls" zlecone przez logowania. Błąd logowania.|
|40553|16|Ze względu na użycie pamięci nadmiarowe zostało zakończone sesji. Spróbuj zmodyfikowanie kwerendy przetwarzania mniejszej liczby wierszy.<br/><br/>*Uwaga:* Zmniejszenie liczby `ORDER BY` i `GROUP BY` operacje w kodzie języku Transact-SQL pomaga ograniczyć wymagania dotyczące pamięci zapytania.|
|40604|16|Może nie Utwórz/ZMIEŃ bazę danych, ponieważ zostałaby zasobów serwera.|
|40606|16|Dołączanie baz danych nie jest obsługiwane w tej wersji programu SQL Server.|
|40607|16|Logowania do systemu Windows nie są obsługiwane w tej wersji programu SQL Server.|
|40611|16|Serwery może zawierać najwyżej 128 reguł zapory.|
|40614|16|Początkowy adres IP reguły zapory nie może przekraczać końcowy adres IP.|
|40615|16|Nie można otworzyć serwera "{0}" zlecone przez logowania. Klienta z adresem IP "{1}" nie jest dozwolona na dostęp do serwera.  Aby włączyć dostęp, korzystać z portalu bazy danych SQL lub uruchom sp_set_firewall_rule wzorca bazy danych, aby utworzyć regułę zapory dla tego adresu IP lub zakres adresów.  Może potrwać maksymalnie pięć minut, aby zmiana została uwzględniona.|
|40617|16|Nazwa reguły zapory, który zaczyna się od <rule name> jest zbyt długa. Maksymalna długość wynosi 128.|
|40618|16|Nazwa reguły zapory nie może być puste.|
|40620|16|Identyfikator logowania użytkownika nie powiodło się "%. & #x2a; ls". Nie można zmienić hasła. Zmienianie hasła podczas logowania nie jest obsługiwane w tej wersji programu SQL Server.|
|40627|20|Operacja na serwerze "{0}" i bazy danych "{1}" jest w toku. Poczekaj kilka minut i spróbuj ponownie.|
|40630|16|Nie można sprawdzić poprawności hasła. Hasło nie spełnia wymagań zasad, ponieważ jest zbyt krótki.|
|40631|16|Hasło określony jest zbyt długa. Hasło powinien mieć nie więcej niż 128 znaków.|
|40632|16|Nie można sprawdzić poprawności hasła. Hasło nie spełnia wymagań zasad, ponieważ nie jest wystarczająco złożone.|
|40636|16|Nie można używać nazwy zastrzeżone bazy danych "%. & #x2a; ls" w tej operacji.|
|40638|16|Identyfikator subskrypcji nieprawidłowe < identyfikator subskrypcji >. Subskrypcja nie istnieje.|
|40639|16|Żądanie nie jest zgodny ze schematem: <schema error>.|
|40640|20|Serwer napotkał nieoczekiwany wyjątek.|
|40641|16|We wskazanej lokalizacji jest nieprawidłowy.|
|40642|17|Serwer jest obecnie zbyt zajęty. Spróbuj ponownie później.|
|40643|16|Wartość określonej nagłówka x ms wersji jest nieprawidłowy.|
|40644|14|Nie można zezwolić na dostęp do określonej subskrypcji.|
|40645|16|Nazwa_serwera <servername> nie może być puste ani mieć wartości null. Go można tylko być składa się z małych liter ""-"z", cyfr 0 – 9 i łącznik. Łącznik nie może prowadzić lub przeprowadzenia w nazwie.|
|40646|16|Identyfikator subskrypcji nie może być puste.|
|40647|16|Subskrypcja < identyfikator subskrypcji nie ma nazwa_serwera serwera.|
|40648|17|Zbyt wiele żądań zostały wykonane. Spróbuj ponownie później.|
|40649|16|Podano nieprawidłowy typ zawartości. Tylko aplikacji/xml jest obsługiwana.|
|40650|16|Subskrypcja < identyfikator subskrypcji > nie istnieje lub nie jest gotowy do użycia.|
|40651|16|Nie można utworzyć serwera, ponieważ subskrypcja < identyfikator subskrypcji > zostanie wyłączona.|
|40652|16|Nie można przenosić lub utworzyć serwera. Subskrypcja < identyfikator subskrypcji > spowoduje przekroczenie przydziału serwera.|
|40671|17|Błąd komunikacji między bramy i usługi zarządzania. Spróbuj ponownie później.|
|40852|16|Nie można otworzyć bazy danych "%. *ls na serwerze "%.* ls zlecone przez logowania. Dostęp do bazy danych jest dozwolone tylko przy użyciu parametrów połączenia z włączonymi zabezpieczeniami. Aby uzyskać dostęp do tej bazy danych, należy zmodyfikować usługi parametry połączenia zawierają bezpieczne, na serwerze FQDN -.database.windows "Nazwa serwera".net należy zmodyfikować, aby .database "Nazwa serwera". `secure`. windows.net.|
|45168|16|System platformy SQL Azure jest obciążeniu i umieszcza górną granicę w równoczesne wykonywanie operacji OBSŁUGIWAŁ bazy danych dla pojedynczego serwera (np. Tworzenie bazy danych). Serwer określony w komunikacie o błędzie przekroczył maksymalną liczbę połączeń jednocześnie. Spróbuj ponownie później.|
|45169|16|System SQL azure jest obciążeniu i umieszcza górny limit liczby równoczesne serwer OBSŁUGIWAŁ operacje dla pojedynczej subskrypcji (Tworzenie np server). Subskrypcji określony w komunikacie o błędzie przekroczyła maksymalną liczbę równoległych połączeń, a żądanie zostało odrzucone. Spróbuj ponownie później.|

## <a name="related-links"></a>Łącza pokrewne

- [Ograniczenia ogólne bazy danych Azure SQL i wskazówki](sql-database-general-limitations.md)
- [Limity zasobów bazy danych SQL Azure](sql-database-resource-limits.md)
