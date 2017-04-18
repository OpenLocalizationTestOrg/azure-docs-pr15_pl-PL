<properties
    pageTitle="Wszystkich tematów dotyczących usługi bazy danych SQL | Microsoft Azure"
    description="Tabela wszystkich tematów dotyczących usługi Azure nazwę bazy danych SQL, która istnieje w http://azure.microsoft.com/documentation/articles/, tytuł i opis."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="genemi"/>


# <a name="all-topics-for-azure-sql-database-service"></a>Wszystkich tematów dotyczących usługi bazy danych SQL Azure

Ten temat zawiera listę wszystkich tematów, zastosowany bezpośrednio do usługi **Bazy danych SQL** Azure. Ta strona sieci Web słów kluczowych można wyszukiwać za pomocą **Klawiszy Ctrl + F**, aby znaleźć tematy bieżącego zainteresowania.




## <a name="new"></a>Nowy

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 1 | [Daxko/CSI używane Azure przyspieszyć cyklu opracowywania i zwiększyć jego działu pomocy technicznej i wydajność](sql-database-implementation-daxko.md) | Więcej informacji na temat jak Daxko/CSI używa bazy danych SQL, aby przyspieszyć cyklu opracowywania i zwiększyć jej działu pomocy technicznej i wydajność |
| 2 | [Azure udostępnia globalne GEP i usprawnienie](sql-database-implementation-gep.md) | Więcej informacji na temat jak GEP używa bazy danych SQL do osiągnięcia globalnych klientów i uzyskać większą wydajność |
| 3 | [Z Azure SnelStart szybko została rozszerzona jego usług firm wynosi 1000 nowych Azure SQL baz danych na miesiąc](sql-database-implementation-snelstart.md) | Dowiedz się więcej o SnelStart używaniu bazy danych SQL szybko rozwinięte jego usług firm wynosi 1000 nowych Azure SQL baz danych na miesiąc |
| 4 | [Umbraco używa bazy danych SQL Azure szybko świadczenia skali usługi i tysiące dzierżaw w chmurze](sql-database-implementation-umbraco.md) | Więcej informacji na temat usługi skali tysiące dzierżaw w chmurze i jak Umbraco używane bazy danych SQL szybko inicjowania obsługi |
| 5 | [Zarządzanie bazy danych Azure SQL za pomocą programu PowerShell](sql-database-manage-powershell.md) | Zarządzanie bazą danych SQL Azure przy użyciu programu PowerShell. |
| 6 | [Obsługa SSMS Azure AD MFA z bazy danych SQL i magazynu danych SQL](sql-database-ssms-mfa-authentication.md) | Za pomocą uwierzytelniania uwzględnić wielu SSMS dla bazy danych SQL i magazynu danych SQL. |
| 7 | [Wyjaśnieniem jednostki transakcji bazy danych (DTUs) i elastycznym jednostki transakcji bazy danych (eDTUs)](sql-database-what-is-a-dtu.md) | Opis jakie bazy danych SQL Azure jest jednostkę transakcji. |


## <a name="updated-articles-sql-database"></a>Aktualizowane artykuły bazy danych SQL

W tej sekcji przedstawiono artykułów, które zostały zaktualizowane w ostatnim tam, gdzie aktualizacji nie duży lub znaczące. Dla każdego artykułu zaktualizowane drukowania fragment tekstu dodano promocji cenowych jest wyświetlany. Artykuły zostały zaktualizowane w zakresie data **2016-08-22** do **2016-10-05**.

| &nbsp; | Artykuł | Zaktualizowany tekst, wstawki kodu | Zaktualizowane |
| --: | :-- | :-- | :-- |
| 8 | [Zarządzanie bazy danych Azure SQL za pomocą programu PowerShell](sql-database-command-line-tools.md) | Tworzenie grupy zasobów dla naszych bazy danych SQL i Azure zasoby pokrewne przy użyciu polecenia cmdlet New-AzureRmResourceGroup (https://msdn.microsoft.com/library/azure/mt759837.aspx). *$resourceGroupName = "resourcegroup1" $resourceGroupLocation = "northcentralus" nowy AzureRmResourceGroup-nazwa $resourceGroupName-lokalizacji $resourceGroupLocation* Aby uzyskać więcej informacji, zobacz Używanie Azure przy użyciu Menedżera zasobów Azure (.. / programu powershell-azure zasobów manager.md). Aby przykładowy skrypt Zobacz Tworzenie bazy danych SQL skrypt programu PowerShell (sql-bazy danych — get pracę powershell.md create-a-sql-database-powershell-script). **Tworzenie bazy danych programu SQL server** Tworzenie bazy danych programu SQL server przy użyciu polecenia cmdlet New-AzureRmSqlServer (https://msdn.microsoft.com/library/azure/mt603715.aspx). Zamień *serwer1* nazwa serwera. Nazwy serwerów musi być unikatowa na wszystkich serwerach bazy danych SQL Azure. Jeśli nazwa serwera nie jest już zajęty, zostanie wyświetlony komunikat o błędzie. To polecenie może potrwać kilka minut. Resou | 2016-09-14 |
| 9 | [Kopiowanie bazy danych programu Azure SQL za pomocą programu PowerShell](sql-database-copy-powershell.md) |   SQL źródło bazy danych (istniejącej bazy danych do skopiowania) — $sourceDbName = "Degresywna 1" $sourceDbServerName = "serwer1" $sourceDbResourceGroupName = kopii bazy danych SQL "rg1" (nowy baza ma zostać utworzony) — $copyDbName = "db1_copy" $copyDbServerName = "Serwer2" $copyDbResourceGroupName = "rg2" kopii bazy danych na tym samym serwerze — nowy AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - nazwa_serwera $sourceDbServerName - DatabaseName $sourceDbName - CopyDatabaseName $copyDbName skopiować bazę danych na innym serwerze — nowy AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - nazwa_serwera $sourceDbServerName - DatabaseName $sourceDbName - CopyResourceGroupName $copyDbResourceGroupName - CopyServerName $copyDbServerName - CopyDatabaseName $copyDbName skopiuj bazy danych do puli elastyczne bazy danych — $poolName = $ New AzureRmSqlDatabaseCopy - ResourceGroupName "pool1" sourceDbResourceGroupName - nazwa_serwera $sourceDbServerName - DatabaseName $sourceDbName - CopyReso | 2016-09-08 |
| 10 | [Tworzenie puli elastyczne bazy danych przy użyciu języka C#](sql-database-elastic-pool-create-csharp.md) |   Console.WriteLine ("serwer Zapora...");  FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule (_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);  Console.WriteLine ("zaporę serwera:" + fwr. FirewallRule.Id);  Console.WriteLine ("elastyczne puli...");  ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool (_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);  Console.WriteLine ("elastyczne puli:" + epr. ElasticPool.Id);  Console.WriteLine("Database...");  DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase (_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);  Console.WriteLine ("bazy danych:" + dbr. Database.Id);  Console.WriteLine ("naciśnij dowolny klawisz, aby kontynuować...");  Console.ReadKey();  } statyczne CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupNa grupa zasobów | 2016-09-14 |
| 11 | [Skrypt programu PowerShell do identyfikowania baz danych odpowiednie dla puli elastyczne bazy danych](sql-database-elastic-pool-database-assessment-powershell.md) | Kandydatów **dla elastyczne puli bazy danych, możemy wykluczenie wzorca i żadnej bazy danych, które już znajdują się w puli. Możesz dodać więcej baz danych do listy wykluczonych poniżej stosownie do potrzeb** $ListOfDBs = Get AzureRmSqlDatabase - nazwa_serwera $servername. Split('.') 0 - ResourceGroupName $ResourceGroupName / Where-Object {$_. DatabaseName - notin ("główny") - i $_. CurrentServiceLevelObjectiveName - notin ("ElasticPool")- i $_. CurrentServiceObjectiveName - notin ("ElasticPool")} $outputConnectionString = "źródła danych = $outputServerName; zabezpieczenia zintegrowane = FAŁSZ; początkowego wykazu = $outputdatabaseName; Identyfikator użytkownika = $outputDBUsername; Hasło = $outputDBpassword "$destinationTableName ="resource_stats_output" **Tworzenie tabeli w bazie danych dane wyjściowe dla zbioru metryki** $sql =" Jeśli NOT EXISTS (wybierz * z sys.objects object_id gdzie = OBJECT_ID(N'$($destinationTableName)'), a następnie wpisz w (N'U ")) $($destinationTableName) początkowego Utwórz tabelę (nazwa_bazy_danych varchar(128), slo varchar(20), end_time Data/Godzina, avg_cpu ruchomości, średnia_ | 2016-09-29 |
| 12 | [Baza danych SQL samouczek: tworzenie bazy danych SQL w minutach przy użyciu Azure portal](sql-database-get-started.md) |   ! Nowej bazy danych sql 1 (. / media/sql-database-get-started/sql-database-new-database-1.png) 3. Kliknij opcję **Bazy danych SQL** , aby otworzyć karta bazy danych SQL. Zawartość ta karta zmienia się w zależności od liczby subskrypcji i z istniejących obiektów (takich jak istniejące serwery).  ! Nowej bazy danych sql 2 (. / media/sql-database-get-started/sql-database-new-database-2.png) 4. W polu tekstowym **Nazwa bazy danych** Podaj nazwę dla pierwszej bazy danych — takie jak "Moje bazy danych". Zielony znacznik wyboru wskazuje, czy podano prawidłową nazwę.  ! Nowej bazy danych sql 3 (. / media/sql-database-get-started/sql-database-new-database-3.png) 5. Jeśli masz wiele subskrypcji, wybierz subskrypcję. 6. W obszarze **Grupa zasobów**kliknij przycisk **Utwórz nowy** i podaj nazwę dla pierwszej grupy zasobów — takie jak "Grupa Mój zasobów". Zielony znacznik wyboru wskazuje, czy podano prawidłową nazwę.  ! Nowej bazy danych sql 4 (. / media/sql-database-get-started/sql-database-new-database-4.png) 7. W obszarze **Wybierz źródło* | 2016-09-08 |
| 13 | [Spróbuj bazy danych SQL: Używanie C# do utworzenia bazy danych SQL z biblioteką bazy danych SQL dla środowiska .NET](sql-database-get-started-csharp.md) | **Tworzenie głównej dostęp do zasobów usługi** Poniższy skrypt programu PowerShell tworzy aplikację Active Directory (AD) i wystawcy usługi, jaka jest potrzebna do uwierzytelnienia naszych aplikacji C. Skrypt wyświetla wartości, które administrator powinien dla powyższego przykładu C. Aby uzyskać szczegółowe informacje, zobacz za pomocą Azure tworzenie wystawcy usługi dostępu do zasobów (.. / zasobów — grupy — służą do uwierzytelniania — usługa principal.md).  Zaloguj się do Azure.  Dodawanie AzureRmAccount Jeśli masz wiele subskrypcji, Usuń komentarze, a następnie ustaw subskrypcję, którą chcesz pracować z.  $subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" zestawu AzureRmContext - SubscriptionId $subscriptionId podać te wartości z nowej aplikacji AAD.  $appName jest nazwą wyświetlaną aplikacji, musi być unikatowa w katalogu.  $uri nie musi być rzeczywistą identyfikatora uri.  $secret jest utworzone hasło.  $appName = "{-Nazwa aplikacji}" $uri = "http://{app-name}" $secret = "{hasło aplikacji}" Tworzenie aplikacji AAD $azureAdApplication = AzureRmADApp nowy | 2016-09-23 |
| 14 | [Zarządzanie bazami danych programu SQL Azure za pomocą portalu Azure](sql-database-manage-portal.md) | Aby wyświetlić lub zaktualizować ustawienia bazy danych, kliknij odpowiednie ustawienie na karta bazy danych SQL:! Ustawienia bazy danych SQL (. / media/sql-database-manage-portal/settings.png) **jak znaleźć nazwy serwera w pełni kwalifikowana bazy danych SQL?** Aby wyświetlić nazwy serwera baz danych, kliknij pozycję **Przegląd** na karta **bazy danych SQL** i zanotuj nazwę serwera:! Ustawienia bazy danych SQL (. / media/sql-database-manage-portal/server-name.png) **jak zarządzać reguły zapory do kontrolowania dostępu programu SQL server i bazy danych?** Aby wyświetlić, tworzenie lub aktualizowanie reguł zapory, kliknij przycisk **Ustaw zaporę serwera** na karta **bazy danych SQL** . Aby uzyskać szczegółowe informacje Zobacz Konfigurowanie reguły zapory na poziomie serwera bazy danych SQL Azure za pomocą portalu Azure (sql-bazy danych — Konfigurowanie zapory settings.md). ! reguły zapory (. / media/sql-database-manage-portal/sql-database-firewall.png) **jak zmienić moje SQL bazy danych usługi warstwa poziomu?** Aby zaktualizować poziom warstwa lub wydajności usługi z bazą danych SQL, kliknij pozycję ** poziomu ceny (s | 2016-09-20 |





## <a name="get-started"></a>Rozpoczynanie pracy

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 15 | [Tworzy aplikacje wielu dzierżawy z bazy danych Azure SQL za pomocą izolacji i wydajności](sql-database-build-multi-tenant-apps.md) | Dowiedz się, jak baza danych SQL tworzy aplikacje wielu dzierżawy |
| 16 | [Nawiązywanie połączenia z bazą danych SQL z programu SQL Server Management Studio, a następnie wykonaj przykładowa kwerenda T-SQL](sql-database-connect-query-ssms.md) | Dowiedz się, jak nawiązać połączenia z bazą danych SQL Azure za pomocą programu SQL Server Management Studio (SSMS). Następnie uruchom przykładowa kwerenda przy użyciu języka Transact-SQL (T-SQL). |
| 17 | [Nawiązywanie połączenia z bazą danych SQL przy użyciu .NET (C#)](sql-database-develop-dotnet-simple.md) | Użyj kod przykładowy w tym szybkie rozpoczęcie tworzenia nowoczesny aplikacji z C# i przez zaawansowanych relacyjnej bazy danych w chmurze z bazy danych SQL Azure. |
| 18 | [Utwórz nową pulę elastyczne bazy danych za pomocą portalu Azure](sql-database-elastic-pool-create-portal.md) | Jak dodać puli skalowalna elastyczne bazy danych konfiguracji bazy danych SQL, aby ułatwić Administracja i zasobów udostępnianie wiele baz danych. |
| 19 | [Eksplorowanie samouczki bazy danych Azure SQL](sql-database-explore-tutorials.md) | Więcej informacji na temat funkcji bazy danych SQL i możliwości |
| 20 | [Baza danych SQL samouczek: tworzenie bazy danych SQL w minutach przy użyciu Azure portal](sql-database-get-started.md) | Dowiedz się, jak skonfigurować bazy danych programu SQL server logicznych, reguły zapory serwera bazy danych SQL i przykładowe dane. Ponadto Dowiedz się, jak połączyć się z narzędziami klienta, konfigurowanie użytkowników i konfigurowanie reguły zapory bazy danych. |
| 21 | [Baza danych SQL Azure zabezpiecza i chroni](sql-database-helps-secures-and-protects.md) | Dowiedz się, jak baza danych SQL ułatwia bezpiecznego i ochrona |
| 22 | [Baza danych SQL Azure dowiaduje się &amp; przystosowuje do Twoich](sql-database-learn-and-adapt.md) | Dowiedz się, jak baza danych SQL uczy się i przystosowuje do Twoich |
| 23 | [Wybierz pozycję chmury opcja programu SQL Server: bazy danych SQL Azure (PaaS) lub SQL Server na Azure maszyny wirtualne (IaaS)](sql-database-paas-vs-sql-server-iaas.md) | Dowiedz się więcej opcji programu SQL Server chmury pasuje do Twojej aplikacji: bazy danych SQL Azure (PaaS) lub SQL Server w chmurze na maszyn wirtualnych Azure. |
| 24 | [Azure skale bazy danych SQL w czasie rzeczywistym](sql-database-scale-on-the-fly.md) | Dowiedz się, jak baza danych SQL skale w czasie rzeczywistym |
| 25 | [Eksplorowanie Szybkie uruchamianie rozwiązanie bazy danych Azure SQL](sql-database-solution-quick-starts.md) | Informacje o rozwiązaniach bazy danych Azure SQL |
| 26 | [Baza danych SQL Azure działa w środowisku usługi](sql-database-works-in-your-environment.md) | Dowiedz się, jak baza danych SQL pomaga, zabezpiecza i chroni |



## <a name="build-an-app"></a>Tworzenie aplikacji

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 27 | [Rozwiązywanie problemów, diagnozowanie i uniknięcia błędów połączenia SQL i przejściowych błędów bazy danych SQL](sql-database-connectivity-issues.md) | Dowiedz się, jak rozwiązywanie problemów, diagnozowanie i zapobiec błąd połączenia SQL lub przejściowych w bazie danych SQL Azure.  |
| 28 | [Nawiązywanie połączenia z bazą danych SQL przy użyciu programu Visual Studio](sql-database-connect-query.md) | Napisać program w języku C# do kwerendy, a następnie nawiązywanie połączenia z bazą danych SQL. Informacje o adresy IP, parametry połączenia, bezpiecznego logowania i bezpłatnego programu Visual Studio. |
| 29 | [Porty inne niż 1433 ADO.NET 4,5 i wersji 12 bazy danych SQL](sql-database-develop-direct-route-ports-adonet-v12.md) | Czasami połączeń klientów z ADO.NET wersji 12 bazy danych SQL Azure pominąć serwer proxy i pracować bezpośrednio na bazę danych. Porty inne niż 1433 stają się ważne. |
| 30 | [Kody błędów SQL dla aplikacji klienckich bazy danych SQL: błąd połączenia i innych problemów z bazy danych](sql-database-develop-error-messages.md) | Informacje na temat kodów błędów SQL aplikacje klienckie bazy danych SQL, takich jak typowych błędów połączenia bazy danych, problemy kopii bazy danych i ogólnych błędów.  |
| 31 | [Nawiązywanie połączenia z bazą danych SQL za pomocą PHP w systemie Windows](sql-database-develop-php-simple.md) | Przedstawia przykładowego programu PHP, który łączy do bazy danych SQL Azure z klientem systemu Windows, a znajdują się łącza do składniki odpowiednie oprogramowanie wymagane przez klienta. |
| 32 | [Spróbuj bazy danych SQL: Używanie C# do utworzenia bazy danych SQL z biblioteką bazy danych SQL dla środowiska .NET](sql-database-get-started-csharp.md) | Spróbuj bazy danych SQL dotyczącej opracowywania aplikacji SQL i C# i tworzenie bazy danych SQL Azure z używania biblioteki bazy danych SQL dla środowiska .NET C#. |
| 33 | [Wprowadzenie do funkcji JSON w bazie danych SQL Azure](sql-database-json-features.md) | Baza danych SQL Azure umożliwia analizy, kwerendy i formatowanie danych w notacji notacji obiektu JavaScript (JSON). |
| 34 | [Rozwiązywania problemów z połączeniem z bazą danych SQL Azure](sql-database-troubleshoot-common-connection-issues.md) | Procedura identyfikowanie i rozwiązywanie typowych błędów połączenia bazy danych SQL Azure. |
| 35 | [Błąd "Bazy danych na serwerze nie jest obecnie niedostępna" podczas nawiązywania połączenia z bazą danych sql](sql-database-troubleshoot-connection.md) | Rozwiązywanie problemów z bazy danych na serwer nie jest obecnie dostępny błąd podczas Aplikacja nawiązuje połączenie z bazą danych SQL. |



## <a name="develop"></a>Można opracowywać

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 36 | [Uzyskiwanie żądane wartości uwierzytelniania aplikacji, aby uzyskać dostęp do bazy danych SQL z kodu](sql-database-client-id-keys.md) | Tworzenie wystawcy usługi z kodu dostępu do bazy danych SQL. |
| 37 | [Nawiązywanie połączenia z bazą danych SQL przy użyciu języka Java JDBC w systemie Windows](sql-database-develop-java-simple.md) | Przedstawia przykładowy kod języka Java, który umożliwia nawiązywanie połączenia z bazą danych SQL Azure. W przykładzie zastosowano JDBC i działa na komputerze klienckim systemu Windows. |
| 38 | [Nawiązywanie połączenia z bazą danych SQL przy użyciu Node.js](sql-database-develop-nodejs-simple.md) | Przedstawia Node.js przykładowy kod, który umożliwia nawiązywanie połączenia z bazą danych SQL Azure. |
| 39 | [Omówienie tworzenia bazy danych SQL](sql-database-develop-overview.md) | Więcej informacji na temat bibliotek dostępne łączności i najważniejsze wskazówki dotyczące aplikacji nawiązywanie połączenia z bazą danych SQL. |
| 40 | [Nawiązywanie połączenia z bazą danych SQL przy użyciu Python](sql-database-develop-python-simple.md) | Przedstawia Python przykładowy kod, który umożliwia nawiązywanie połączenia z bazą danych SQL Azure. |
| 41 | [Nawiązywanie połączenia z bazą danych SQL przy użyciu dopiskiem](sql-database-develop-ruby-simple.md) | Nadaj próbki kodu dopiskiem fonetycznym uruchomienia nawiązywania połączenia z bazą danych SQL Azure. |
| 42 | [Wprowadzenie do programu czasowy tabel w bazie danych Azure SQL](sql-database-temporal-tables.md) | Dowiedz się, jak rozpocząć pracę z przy użyciu czasowy tabel w bazie danych SQL Azure. |



## <a name="performance-and-scale"></a>Wydajność i Skala

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 43 | [Advisor bazy danych SQL](sql-database-advisor.md) | Advisor bazy danych SQL Azure zawiera zalecenia dotyczące istniejącej bazy danych programu SQL, który można zwiększyć wydajność bieżącej kwerendy. |
| 44 | [Za pomocą portalu Azure Advisor bazy danych SQL](sql-database-advisor-portal.md) | Advisor bazy danych SQL Azure Azure portal umożliwia przeglądanie i wdrażanie zaleceń istniejące bazy danych SQL, który można zwiększyć wydajność bieżącej kwerendy. |
| 45 | [Omówienie testu bazy danych SQL Azure](sql-database-benchmark-overview.md) | W tym temacie opisano testu bazy danych SQL Azure używane w pomiaru wydajności bazy danych SQL Azure. |
| 46 | [Kiedy ma być użyty puli elastyczne bazy danych?](sql-database-elastic-pool-guidance.md) | Pula elastyczne bazy danych jest to zbiór dostępne zasoby, które są wspólne grupy elastyczne baz danych. Niniejszy dokument zawiera wskazówki w celu oceny przydatności dla grupy baz danych za pomocą puli elastyczne bazy danych. |
| 47 | [Wprowadzenie do narzędzia nieelastyczne bazy danych](sql-database-elastic-scale-get-started.md) | Podstawowe informacje dotyczące funkcji narzędzia elastyczne bazy danych bazy danych SQL Azure, w tym łatwe do uruchamiania aplikacji przykładowych. |
| 48 | [Baza danych SQL — często zadawane pytania](sql-database-faq.md) | Odpowiedzi na typowe pytania klientów zapytaj o baz danych w chmurze i bazy danych SQL Azure, system zarządzania relacyjnymi bazami danych firmy Microsoft (RDBMS) i bazy danych jako usługa w chmurze. |
| 49 | [Wprowadzenie do programu w pamięci (wersja Preview) w bazie danych SQL](sql-database-in-memory.md) | Technologie SQL w pamięci znacznie zwiększyć wydajność transakcji i analizy obciążeń pracą. Dowiedz się, jak korzystać z tych technologii. |
| 50 | [Używanie w pamięci OLTP (wersja preview) aby poprawić wydajność aplikacji w bazie danych SQL](sql-database-in-memory-oltp-migration.md) | Przyjąć OLTP w pamięci, aby zwiększyć wydajność transakcji w istniejącej bazy danych SQL. |
| 51 | [Monitorowanie magazynowania OLTP w pamięci](sql-database-in-memory-oltp-monitoring.md) | Szacowanie i monitorowanie miejsca do magazynowania w pamięci XTP korzystać, zdolności; Naprawianie błędu zdolności 41823 |
| 52 | [Działania w sklepie kwerendy w bazie danych Azure SQL](sql-database-operate-query-store.md) | Dowiedz się, jak działają w sklepie kwerendy w bazie danych SQL Azure |
| 53 | [Wgląd wydajności bazy danych SQL](sql-database-performance.md) | Baza danych SQL Azure udostępnia narzędzia wydajności ułatwiające określanie obszarów, które można zwiększyć wydajność bieżącej kwerendy. |
| 54 | [Wydajność dla pojedynczego baz danych i baza danych SQL Azure](sql-database-performance-guidance.md) | W tym artykule mogą ułatwić podjęcie warstwa usług, które służących do aplikacji. Zaleca również sposoby dostosować aplikację, aby uzyskać najbardziej z bazy danych SQL Azure. |
| 55 | [Wgląd wydajności kwerendy bazy danych Azure SQL](sql-database-query-performance.md) | Monitorowanie wydajności zapytania identyfikuje większości używające Procesora kwerend dla bazy danych SQL Azure. |
| 56 | [Zmienianie usługi warstwa i wydajności poziomu (poziomu cen) z bazą danych SQL](sql-database-scale-up.md) | Zmiana poziomu usług i poziom wydajności bazy danych programu Azure SQL pokazano, jak skalowanie bazy danych SQL w górę lub w dół. Zmienianie poziomu cen bazy danych programu Azure SQL. |
| 57 | [Monitorowanie wydajności bazy danych w bazie danych SQL Azure](sql-database-single-database-monitor.md) | Więcej informacji na temat opcji monitorowania bazy danych przy użyciu narzędzia Azure i dynamiczne zarządzanie widoków. |
| 58 | [Porady dotyczące dostosowywania wydajności bazy danych SQL](sql-database-troubleshoot-performance.md) | Porady dotyczące Dostosowywanie w bazie danych SQL Azure za pomocą obliczeń i poprawy wydajności. |
| 59 | [Jak używać tworzeniu partii, aby zwiększyć wydajność aplikacji bazy danych SQL](sql-database-use-batching-to-improve-performance.md) | Temat dostarcza dowodów tej operacji łączenia we wsady bazy danych znacznie imroves szybkość i skalowalność aplikacji bazy danych SQL Azure. Mimo że te łączenia we wsady techniki pracy dla każdej bazy danych programu SQL Server, artykuł dotyczy Azure. |



## <a name="tools-for-scaling-out"></a>Narzędzia do Skalowanie zewnętrzne

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 60 | [Projektowanie wzorców multitenant aplikacji władz akredytacji bezpieczeństwa i bazy danych SQL Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md) | W tym artykule omówiono wymagania i typowe wzorce Architektura danych multitenant bazy danych, które aplikacje w środowisku chmury potrzebne brać pod uwagę i różnych kompromisów skojarzone z tych deseni. Go też wyjaśniono, jak bazy danych SQL Azure z pul elastycznych i elastyczne narzędzia pomóc rozwiązania tych wymagań w sposób nie kompromisów. |
| 61 | [Migrowanie istniejącej bazy danych w celu skala w nowym oknie](sql-database-elastic-convert-to-use-elastic-tools.md) | Konwertowanie sharded baz danych, tworząc shard menedżera map za pomocą narzędzi elastyczne bazy danych |
| 62 | [Tworzenie bazy danych w chmurze skalowalna](sql-database-elastic-database-client-library.md) | Tworzenie skalowalna .NET bazy danych aplikacji z biblioteki klienta elastyczne bazy danych |
| 63 | [Liczniki wydajności dla menedżera map shard](sql-database-elastic-database-perf-counters.md) | ShardMapManager klas i danych zależne routingu liczniki wydajności |
| 64 | [Dodawanie shard za pomocą narzędzia elastyczne bazy danych](sql-database-elastic-scale-add-a-shard.md) | Umożliwia dodawanie nowych odłamki do shard elastyczne API skali zestawu. |
| 65 | [Wdrażanie usługi korespondencji seryjnej podziału](sql-database-elastic-scale-configure-deploy-split-and-merge.md) | Dzielenie i scalanie przy użyciu narzędzia elastyczne bazy danych |
| 66 | [Dane zależne routingu](sql-database-elastic-scale-data-dependent-routing.md) | Jak używać klasy ShardMapManager w aplikacjach .NET dla danych zależne od routingu, funkcja elastyczne bazy danych do bazy danych SQL Azure |
| 67 | [Narzędzia elastyczne bazy danych — często zadawane pytania](sql-database-elastic-scale-faq.md) | Często zadawane pytania dotyczące skali elastyczne bazy danych Azure SQL. |
| 68 | [Elastyczne słownik Narzędzia bazy danych](sql-database-elastic-scale-glossary.md) | Wyjaśnienie pojęcia dotyczące narzędzia elastyczne bazy danych |
| 69 | [Skalowanie zewnętrzne z bazy danych SQL Azure](sql-database-elastic-scale-introduction.md) | Oprogramowanie jako deweloperów usługi (władz akredytacji bezpieczeństwa) można łatwo tworzyć elastyczne, skalowalna baz danych w chmurze za pomocą tych narzędzi |
| 70 | [Poświadczenia używane na dostęp do biblioteki klienta elastyczne bazy danych](sql-database-elastic-scale-manage-credentials.md) | Jak ustawić odpowiedni poziom poświadczeń, administrator, aby tylko do odczytu, w przypadku aplikacji elastyczną bazy danych |
| 71 | [Kwerenda shard wielu elementów](sql-database-elastic-scale-multishard-querying.md) | Uruchamianie kwerend przez odłamki przy użyciu Biblioteka klienta elastyczne bazy danych. |
| 72 | [Przenoszenie danych między bazami danych w chmurze skalowanej](sql-database-elastic-scale-overview-split-and-merge.md) | Wyjaśniono, jak do manipulowania odłamki i przenoszenie danych za pośrednictwem usługi siebie za pomocą elastyczne API bazy danych. |
| 73 | [Możliwość skalowania baz danych z Menedżera map shard](sql-database-elastic-scale-shard-map-management.md) | Jak używać ShardMapManager, Biblioteka klienta elastyczne bazy danych |
| 74 | [Konfiguracja zabezpieczeń korespondencji seryjnej podziału](sql-database-elastic-scale-split-merge-security-configuration.md) | Konfigurowanie x409 certyfikaty szyfrowania |
| 75 | [Uaktualnienie aplikacji przy użyciu najnowszych biblioteki klienta elastyczne bazy danych](sql-database-elastic-scale-upgrade-client-library.md) | Uaktualnianie aplikacji i bibliotek przy użyciu Nuget |
| 76 | [Elastyczne bibliotece klienta bazy danych przy użyciu struktury obiektu](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) | Skorzystać z biblioteki klienta elastyczne bazy danych i struktury jednostki kodowania baz danych |
| 77 | [Biblioteka klienta elastyczne bazy danych za pomocą Dapper](sql-database-elastic-scale-working-with-dapper.md) | Biblioteka klienta elastyczne bazy danych za pomocą Dapper. |
| 78 | [Dzierżawy wielu aplikacji za pomocą narzędzia bazy danych elastycznych i zabezpieczenia na poziomie wiersza](sql-database-elastic-tools-multi-tenant-row-level-security.md) | Dowiedz się, jak za pomocą narzędzia bazy danych elastyczne razem z zabezpieczeń na poziomie wiersza tworzenia aplikacji z poziomu wysoce skalowalna danych w bazie danych SQL Azure, obsługującego odłamki wielu dzierżawy. |



## <a name="elastic-jobs"></a>Elastyczne zadania

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 79 | [Tworzenie i zarządzanie nimi skalować się bazy danych programu SQL Azure za pomocą elastyczne zadania (wersja preview)](sql-database-elastic-jobs-create-and-manage.md) | Przeprowadź przez tworzenie i Zarządzanie zadaniem elastyczne bazy danych. |
| 80 | [Wprowadzenie do zadań elastyczne baz danych](sql-database-elastic-jobs-getting-started.md) | jak używać zadań elastyczne baz danych |
| 81 | [Zarządzanie bazami danych w chmurze skalowanej](sql-database-elastic-jobs-overview.md) | Przedstawia usługa zadania elastyczne bazy danych |
| 82 | [Tworzenie i zarządzanie zadaniami elastyczne bazy danych SQL Database, przy użyciu programu PowerShell (wersja preview)](sql-database-elastic-jobs-powershell.md) | Programu PowerShell służące do zarządzania pul bazy danych SQL Azure |
| 83 | [Instalowanie Omówienie zadań elastyczne bazy danych](sql-database-elastic-jobs-service-installation.md) | Szczegółową instalacji funkcji elastyczne zadania. |
| 84 | [Odinstaluj składniki zadania elastyczne bazy danych](sql-database-elastic-jobs-uninstall.md) | Jak odinstalować narzędzie zadania elastyczne bazy danych |



## <a name="elastic-pools"></a>Elastyczne pul

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 85 | [Co to jest Azure puli elastyczne?](sql-database-elastic-pool.md) | Zarządzanie setki lub tysiące bazy danych za pomocą puli. Cena jednego zestawu pomiary wydajności można rozłożoną w puli. Przenieś bazach danych lub pomniejszyć będzie. |
| 86 | [Tworzenie puli elastyczne bazy danych przy użyciu języka C#](sql-database-elastic-pool-create-csharp.md) | Tworzenie puli skalowalna elastyczne bazy danych w bazie danych SQL Azure, więc można współużytkowanie zasobów między wiele baz danych za pomocą technik projektowania bazy danych C#. |
| 87 | [Utwórz nową pulę elastyczne bazy danych przy użyciu programu PowerShell](sql-database-elastic-pool-create-powershell.md) | Dowiedz się, jak używać programu PowerShell do bazy danych SQL Azure w nowym oknie Skala zasobów, tworząc puli skalowalna elastyczne bazy danych do zarządzania wiele baz danych. |
| 88 | [Skrypt programu PowerShell do identyfikowania baz danych odpowiednie dla puli elastyczne bazy danych](sql-database-elastic-pool-database-assessment-powershell.md) | Pula elastyczne bazy danych jest to zbiór dostępne zasoby, które są wspólne grupy elastyczne baz danych. Niniejszy dokument zawiera skrypt programu Powershell ułatwiające oceny przydatności dla grupy baz danych za pomocą puli elastyczne bazy danych. |
| 89 | [Monitorowanie i zarządzanie nimi pula elastyczne bazy danych z C#](sql-database-elastic-pool-manage-csharp.md) | Zarządzanie puli bazy danych SQL Azure elastyczne bazy danych przy użyciu technik projektowania bazy danych C#. |
| o 90 stopni. | [Monitorowanie i zarządzanie nimi puli elastyczne bazy danych za pomocą portalu Azure](sql-database-elastic-pool-manage-portal.md) | Dowiedz się, jak za pomocą Azure portal i analizy wbudowanych SQL Database zarządzanie, monitorowanie i prawo rozmiar puli skalowalna elastyczne bazy danych optymalizacji wydajności baz danych i zarządzać nimi, koszt. |
| 91 | [Monitorowanie i zarządzanie nimi puli elastyczne bazy danych przy użyciu programu PowerShell](sql-database-elastic-pool-manage-powershell.md) | Dowiedz się, jak zarządzanie puli elastyczne bazy danych przy użyciu programu PowerShell. |
| 92 | [Monitorowanie i zarządzanie nimi puli elastyczne bazy danych przy użyciu języka Transact-SQL](sql-database-elastic-pool-manage-tsql.md) | Umożliwia tworzenie bazy danych programu Azure SQL w puli elastyczne T-SQL. Lub przenoszenie datbase i pule przy użyciu T-SQL. |
| 93 | [Rozliczenia puli elastyczne bazy danych i informacje o cenach](sql-database-elastic-pool-price.md) | Informacje o cenach specyficzne dla pul elastyczne bazy danych. |



## <a name="elastic-query"></a>Elastyczne kwerendy

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 94 | [Raportowanie w chmurze skalowanej baz danych (wersja preview)](sql-database-elastic-query-getting-started.md) | jak używać innej kwerendy bazy danych bazy danych |
| 95 | [Wprowadzenie do kwerend między bazami danych (podziału pionowego) (wersja preview)](sql-database-elastic-query-getting-started-vertical.md) | jak używać zapytania elastyczne bazy danych przy użyciu pionowo na partycje baz danych |
| 96 | [Raportowania w chmurze skalowanej baz danych (wersja preview)](sql-database-elastic-query-horizontal-partitioning.md) | jak skonfigurować elastyczne kwerend na poziomie partycje |
| 97 | [Azure baza danych SQL elastyczne baza danych kwerendy Przegląd (wersja preview)](sql-database-elastic-query-overview.md) | Omówienie funkcji elastyczne kwerendy |
| 98 | [Kwerenda wykonywana w chmurze baz danych z różnych schematów (wersja preview)](sql-database-elastic-query-vertical-partitioning.md) | jak skonfigurować kwerendy między bazami danych w pionie partycje |



## <a name="elastic-transaction"></a>Elastyczne transakcji

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 99 | [Transakcje rozproszone w chmurze baz danych](sql-database-elastic-transactions-overview.md) | Przegląd transakcji elastyczne bazy danych z bazy danych Azure SQL |



## <a name="backup-and-recovery"></a>I przywracania kopii zapasowych

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 100 | [Kopie zapasowe bazy danych SQL](sql-database-automated-backups.md) | Dowiedz się więcej o bazy danych SQL wbudowanych kopie zapasowe bazy danych, które umożliwiają użycia bazy danych SQL Azure z powrotem do poprzedniego punktu w czasie lub kopiowanie bazy danych do nowej bazy danych w regionu geograficznego (maksymalnie 35 dni). |
| 101 | [Omówienie ciągłości z bazy danych SQL Azure](sql-database-business-continuity.md) | Dowiedz się, jak bazy danych SQL Azure obsługuje chmury ciągłości i odzyskiwanie bazy danych i pomaga zachować uruchomione aplikacje w chmurze krytyczne. |
| 102 | [Jak przywrócić jednej tabeli z kopii zapasowej bazy danych SQL Azure](sql-database-cloud-migrate-restore-single-table-azure-backup.md) | Dowiedz się, jak przywrócić jednej tabeli z kopii zapasowej bazy danych SQL Azure. |
| 103 | [Projektowanie aplikacji chmury awarii przy użyciu aktywnej replikacji Geo w bazie danych SQL](sql-database-designing-cloud-solutions-for-disaster-recovery.md) | Dowiedz się, jak zaprojektować chmury awarii odzyskiwania rozwiązań biznesowych ciągłości planowania przy użyciu replikacji geo dla aplikacji kopii zapasowych danych z bazy danych SQL Azure. |
| 104 | [Przywracanie bazy danych SQL Azure ani przełączenie awaryjne pomocniczym](sql-database-disaster-recovery.md) | Dowiedz się, jak odzyskać bazy danych z awarii regionalnego centrum danych lub błąd Azure SQL Replikacja bazy danych Active Geo- i możliwości, przywracanie Geo. |
| 105 | [Wykonywanie awarii odzyskiwania zwijania i rozwijania szczegółów](sql-database-disaster-recovery-drills.md) | Dowiedz się, orientacji i najważniejsze wskazówki dotyczące korzystania z bazy danych SQL Azure przeprowadzić ćwiczenia odzyskiwanie danych, które mogą pomóc zachować Twoja misja krytycznych aplikacji biznesowych mechanizm błędów i awarii. |
| 106 | [Strategii odzyskiwania danych dla aplikacji za pomocą puli elastyczne bazy danych SQL](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) | Dowiedz się, jak zaprojektować rozwiązania cloud awarii, wybierając pozycję deseniu prawo pracy awaryjnej. |
| 107 | [Rozwiązywanie problemów z mapą shard za pomocą klasy RecoveryManager](sql-database-elastic-database-recovery-manager.md) | Rozwiązywanie problemów z mapami shard za pomocą klasy RecoveryManager |
| 108 | [Importowanie pliku PLECAK tworzenie bazy danych programu Azure SQL](sql-database-import.md) | Tworzenie bazy danych programu Azure SQL, importując istniejący plik PLECAK. |
| 109 | [Przywracanie bazy danych SQL Azure do poprzedniego punktu w czasie z Azure Portal](sql-database-point-in-time-restore-portal.md) | Przywracanie bazy danych SQL Azure do poprzedniego punktu w czasie. |
| 110 | [Przywracanie bazy danych SQL Azure do poprzedniego punktu w czasie przy użyciu programu PowerShell](sql-database-point-in-time-restore-powershell.md) | Przywracanie bazy danych SQL Azure do poprzedniego punktu w czasie |
| 112 | [Odzyskiwanie bazy danych programu Azure SQL za pomocą kopie zapasowe automatycznego bazy danych](sql-database-recovery-using-backups.md) | Informacje na temat przywracania w chwili, umożliwiająca przywrócenie bazy danych SQL Azure do poprzedniego punktu w czasie (maksymalnie 35 dni). |
| 113 | [Przywracanie usuniętego bazy danych Azure SQL za pomocą portalu Azure](sql-database-restore-deleted-database-portal.md) | Przywracanie usuniętego bazy danych Azure SQL (Azure Portal). |
| 114 | [Przywracanie usuniętego bazy danych SQL Azure za pomocą programu PowerShell](sql-database-restore-deleted-database-powershell.md) | Przywracanie usuniętego Azure SQL Database (programu PowerShell). |
| 115 | [Przywracanie bazy danych do poprzedniego punktu w czasie, przywracanie usuniętych bazy danych lub odzyskiwanie z awaria centrum danych](sql-database-troubleshoot-backup-and-restore.md) | Dowiedz się, jak odzyskać bazy danych w chmurze z błędów i awarii przy użyciu kopii zapasowych i replik w bazie danych SQL Azure. |



## <a name="migrate"></a>Migrowanie

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 116 | [Eksportowanie bazy danych programu SQL Server do pliku PLECAK przy użyciu SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Migracja bazy danych dla systemu Microsoft Azure SQL Database eksport bazy danych, eksportowanie pliku PLECAK, sqlpackage |
| 117 | [Eksportowanie bazy danych programu SQL Server do pliku PLECAK przy użyciu programu SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Migracja bazy danych dla systemu Microsoft Azure SQL Database eksport bazy danych, Eksportuj Kreator eksportu danych aplikacji poziomu PLECAK pliku |
| 118 | [Importowanie do bazy danych SQL z pliku PLECAK przy użyciu SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) | Microsoft Azure SQL Database migracja bazy danych, importowanie bazy danych, importowanie pliku PLECAK, sqlpackage |
| 119 | [Importowanie z PLECAK przy użyciu SSMS bazą danych SQL](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Wdrażanie bazy danych Microsoft Azure SQL Database, migracja bazy danych, importowanie bazy danych, eksportowanie bazy danych, Kreator migracji |
| 120 | [Migrowanie bazy danych programu SQL Server do bazy danych SQL za pomocą Kreatora bazy danych Microsoft Azure wdrażanie bazy danych](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) | Microsoft Azure SQL Database migracja bazy danych, Kreator bazy danych Microsoft Azure |
| 121 | [Migrowanie bazy danych programu SQL Server do bazy danych SQL Azure za pomocą transakcji replikacji](sql-database-cloud-migrate-compatible-using-transactional-replication.md) | Microsoft Azure SQL Database migracja bazy danych, importowanie bazy danych, transakcji replikacji |
| 122 | [Określanie przy użyciu SqlPackage.exe zgodności bazy danych SQL](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) | Microsoft Azure SQL Database, migracja bazy danych, bazy danych SQL w celu zapewnienia zgodności SqlPackage |
| 123 | [Określanie bazy danych SQL zgodności przed migracją do bazy danych SQL Azure za pomocą programu SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md) | Microsoft Azure SQL Database, migracja bazy danych, bazy danych SQL w celu zapewnienia zgodności eksportowanie danych warstwy aplikacji Kreator |
| 124 | [Problemy ze zgodnością bazy danych programu SQL Server rozwiązać problem przed migracją do bazy danych SQL Azure za pomocą Kreatora migracji SQL Azure](sql-database-cloud-migrate-fix-compatibility-issues.md) | Microsoft Azure SQL Database, migracja bazy danych, informacje o zgodności, Kreator migracji SQL Azure |
| 125 | [Migrowanie bazy danych programu SQL Server bazą danych SQL Azure za pomocą programu SQL Server Data Tools for Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) | Microsoft Azure SQL Database, migracja bazy danych, informacje o zgodności, Kreator migracji SQL Azure, SSDT |
| 126 | [Rozwiązywanie problemów ze zgodnością bazy danych programu SQL Server przy użyciu programu SQL Server Management Studio przed migracją do bazy danych SQL](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) | Microsoft Azure SQL Database, migracja bazy danych, informacje o zgodności, Kreator migracji SQL Azure |
| 127 | [Archiwum bazy danych programu Azure SQL w pliku PLECAK przy użyciu programu PowerShell](sql-database-export-powershell.md) | Archiwum bazy danych programu Azure SQL w pliku PLECAK przy użyciu programu PowerShell |
| 128 | [Importowanie pliku PLECAK tworzenie bazy danych programu Azure SQL za pomocą programu PowerShell](sql-database-import-powershell.md) | Importowanie pliku PLECAK tworzenie bazy danych programu Azure SQL za pomocą programu PowerShell |
| 129 | [Ładowanie danych z pliku CSV do magazynu danych SQL Azure (płaskie plików)](sql-database-load-from-csv-with-bcp.md) | W przypadku małych danych, rozmiar używa bcp do importowania danych do bazy danych SQL Azure. |
| 130 | [Przenoszenie baz danych między serwerami między subskrypcje i się Azure](sql-database-troubleshoot-moving-data.md) | Szybkie kroki, aby skopiować, przenieść, a następnie przeprowadzić migrację danych i baz danych w bazie danych SQL Azure. |



## <a name="move-data-in-and-out"></a>Przenoszenie danych i pomniejszanie

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 131 | [Migracja bazy danych programu SQL Server do bazy danych SQL w chmurze](sql-database-cloud-migrate.md) | Dowiedz się, jak lokalnego programu SQL Server migracja bazy danych do bazy danych SQL Azure w chmurze. Aby sprawdzić zgodność przed migracją bazy danych za pomocą narzędzia do migracji bazy danych. |
| 132 | [Kopiowanie bazy danych programu Azure SQL](sql-database-copy.md) | Utwórz kopię bazy danych programu Azure SQL |
| 133 | [Archiwizowanie bazy danych programu Azure SQL plik PLECAK przy użyciu Azure Portal](sql-database-export.md) | Archiwizowanie bazy danych programu Azure SQL plik PLECAK przy użyciu Azure Portal |



## <a name="security"></a>Zabezpieczenia

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 134 | [Łączenie z bazą danych SQL lub magazynu danych SQL za pomocą uwierzytelniania usługi Azure Active Directory](sql-database-aad-authentication.md) | Dowiedz się, jak nawiązać połączenia z bazą danych SQL przy użyciu uwierzytelnianie usługi Azure Active Directory. |
| 135 | [Zawsze szyfrowane: Ochrony ważnych danych w bazie danych SQL i przechowywanie kluczy szyfrowania w magazynie certyfikatów systemu Windows](sql-database-always-encrypted.md) | Ochrona ważnych danych w bazie danych SQL w minutach. |
| 136 | [Zawsze szyfrowane: Ochrony ważnych danych w bazie danych SQL i przechowywanie kluczy szyfrowania w Azure klucza magazynu](sql-database-always-encrypted-azure-key-vault.md) | Ochrona ważnych danych w bazie danych SQL w minutach. |
| 137 | [Baza danych SQL — Obsługa klientów niższych poziomów i IP punktu końcowego zmiany dotyczące inspekcji](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) | Informacje o Obsługa klientów niższych poziomów bazy danych SQL i IP zmiany punkt końcowy dla inspekcja. |
| 138 | [Konfigurowanie reguły zapory poziomie serwera bazy danych SQL Azure za pomocą programu PowerShell](sql-database-configure-firewall-settings-powershell.md) | Dowiedz się, jak skonfigurować zaporę dla adresów IP, które uzyskiwać dostęp do bazy danych programu SQL Azure. |
| 139 | [Skonfiguruje reguły zapory poziomie serwera bazy danych SQL Azure za pomocą interfejsu API usługi REST](sql-database-configure-firewall-settings-rest.md) | Dowiedz się, jak skonfigurować zaporę dla adresów IP, które uzyskiwać dostęp do bazy danych programu SQL Azure. |
| 140 | [Skonfiguruje reguły zapory serwera i poziomu bazy danych bazy danych SQL Azure za pomocą T-SQL](sql-database-configure-firewall-settings-tsql.md) | Dowiedz się, jak skonfigurować zaporę dla adresów IP, które uzyskiwać dostęp do bazy danych programu SQL Azure. |
| 141 | [Wprowadzenie do programu SQL bazy danych dynamicznych danych maskowanie (Azure Portal)](sql-database-dynamic-data-masking-get-started.md) | Jak rozpocząć pracę z SQL bazy danych dynamiczne dane maskowanie w Azure Portal |
| 142 | [Wprowadzenie do programu SQL bazy danych dynamicznych danych maskowanie (klasyczny Portal Azure)](sql-database-dynamic-data-masking-get-started-portal.md) | Jak rozpocząć pracę z SQL bazy danych dynamicznych danych maskowanie w portalu klasyczny Azure |
| 143 | [Skonfiguruje reguły zapory bazy danych SQL Azure \- — omówienie](sql-database-firewall-configure.md) | Dowiedz się, jak skonfigurować zaporę bazy danych SQL z reguły zapory serwera poziomie bazy danych i zarządzanie dostępem. |
| 144 | [Baza danych SQL samouczek: Tworzenie konta użytkowników w celu dostępu i zarządzanie bazą danych bazy danych SQL](sql-database-get-started-security.md) | Dowiedz się, jak utworzyć konta użytkowników dostęp do i zarządzanie bazą danych. |
| 145 | [Uwierzytelnianie bazy danych SQL i autoryzacji: udzielanie dostępu](sql-database-manage-logins.md) | Więcej informacji na temat zarządzania zabezpieczeń bazy danych SQL, w szczególności, jak zarządzać zabezpieczeń programu access i logowania bazy danych za pomocą konta głównym poziomie serwera. |
| 146 | [Wytyczne dotyczące zabezpieczeń bazy danych SQL Azure i ograniczenia](sql-database-security-guidelines.md) | Informacje na temat zasady Microsoft Azure SQL Database i ograniczenia związane z zabezpieczeniami. |
| 147 | [Wprowadzenie do wykrywania zagrożenie bazy danych SQL](sql-database-threat-detection-get-started.md) | Jak rozpocząć pracę z wykrywanie zagrożenie bazy danych SQL w Azure Portal |
| 148 | [Sposób wykonywania typowych zadań administracyjnych, takich jak zresetować hasło administratora w bazie danych SQL Azure](sql-database-troubleshoot-permissions.md) | W tym artykule opisano sposób wykonywania typowych zadań administracyjnych w bazie danych SQL. Na przykład Resetowanie hasła administratora, udzielanie i usuwanie dostępu. |



## <a name="manage-your-database"></a>Zarządzanie bazą danych

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 149 | [Rozpoczynanie pracy z inspekcji bazy danych SQL](sql-database-auditing-get-started.md) | Rozpoczynanie pracy z inspekcji bazy danych SQL |
| 150 | [Konfigurowanie reguły zapory na poziomie serwera bazy danych SQL Azure za pomocą portalu Azure](sql-database-configure-firewall-settings.md) | Dowiedz się, jak skonfigurować zaporę adresów IP dostępu do serwera Azure SQL. |
| 151 | [Kopiowanie bazy danych SQL Azure za pomocą portalu Azure](sql-database-copy-portal.md) | Utwórz kopię bazy danych programu Azure SQL |
| 152 | [Zarządzanie bazami danych programu SQL Azure za pomocą automatyzacji Azure](sql-database-manage-automation.md) | Dowiedz się, jak usługa Azure automatyzacji umożliwia zarządzanie bazami danych Azure SQL w skali. |
| 153 | [Omówienie: narzędzia do zarządzania dla bazy danych SQL](sql-database-manage-overview.md) | Porównuje narzędzia i opcje służące do zarządzania bazy danych SQL Azure |
| 154 | [Monitorowanie korzystanie z widoków dynamiczne zarządzanie bazą danych SQL Azure](sql-database-monitoring-with-dmvs.md) | Dowiedz się, jak wykrywanie i diagnozowanie typowych problemów z wydajnością przy użyciu widoków dynamiczne zarządzanie monitorowanie Microsoft Azure SQL Database. |
| 155 | [Zabezpieczanie bazy danych SQL](sql-database-security.md) | Informacje dotyczące zabezpieczeń bazy danych SQL Azure i SQL Server, w tym różnice w chmurze i SQL Server lokalnego w przypadku uwierzytelniania, autoryzacji, zabezpieczenia połączeń, szyfrowania i zgodność. |



## <a name="extended-events"></a>Rozszerzone zdarzenia

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 156 | [Zdarzenia plik docelowy kod rozszerzone zdarzenia w bazie danych SQL](sql-database-xevent-code-event-file.md) | Przykład dwuetapowa kod demonstrujący obiekt docelowy plik zdarzeń rozszerzonego zdarzenia bazy danych SQL Azure zapewnia programu PowerShell i Transact-SQL. Azure magazyn jest wymaganym w ramach tego scenariusza. |
| 157 | [Dzwonienie kod docelowej buforu rozszerzone zdarzenia w bazie danych SQL](sql-database-xevent-code-ring-buffer.md) | Zawiera przykładowy kod języka Transact-SQL, wykonany łatwe i szybkie przy użyciu celu bufor cykliczny w bazie danych SQL Azure. |
| 158 | [Rozszerzone zdarzenia w bazie danych SQL](sql-database-xevent-db-diff-from-svr.md) | W tym artykule opisano rozszerzonego zdarzenia (XEvents) w bazie danych SQL Azure, a także jak sesji zdarzeń się nieco różnić od sesji zdarzeń w programie Microsoft SQL Server. |



## <a name="geo-replication"></a>Geo replikacji

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 159 | [Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure Portal Azure](sql-database-geo-replication-failover-portal.md) | Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure za pomocą portalu Azure |
| 160 | [Inicjowanie planowanego lub niezaplanowane trybie awaryjnym dla bazy danych SQL Azure za pomocą programu PowerShell](sql-database-geo-replication-failover-powershell.md) | Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure za pomocą programu PowerShell |
| 161 | [Inicjowanie planowanego lub niezaplanowane trybie awaryjnym dla bazy danych SQL Azure za pomocą języka Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure za pomocą języka Transact-SQL |
| 162 | [Omówienie: SQL bazy danych Active Geo replikacji](sql-database-geo-replication-overview.md) | Replikacja Geo Active umożliwia konfigurowanie 4 replik bazy danych w każdym z Azure centrach danych. |
| 163 | [Konfigurowanie Geo Replikacja bazy danych SQL Azure Portal Azure](sql-database-geo-replication-portal.md) | Konfigurowanie Geo Replikacja bazy danych SQL Azure za pomocą portalu Azure |
| 164 | [Konfigurowanie replikacji Geo dla bazy danych Azure SQL za pomocą programu PowerShell](sql-database-geo-replication-powershell.md) | Konfigurowanie aktywnej replikacji Geo dla bazy danych SQL Azure za pomocą programu PowerShell |
| 165 | [Jak zarządzać zabezpieczeń bazy danych SQL Azure po awarii](sql-database-geo-replication-security-config.md) | W tym temacie opisano zagadnienia dotyczące zabezpieczeń związanych z zarządzaniem zabezpieczeń po Przywracanie bazy danych lub w trybie awaryjnym. |
| 166 | [Konfigurowanie replikacji Geo dla bazy danych Azure SQL za pomocą języka Transact-SQL](sql-database-geo-replication-transact-sql.md) | Konfigurowanie Geo Replikacja bazy danych SQL Azure za pomocą języka Transact-SQL |
| 167 | [Geo-Przywracanie bazy danych SQL Azure z kopii zapasowej zbędne geo przy użyciu Azure Portal](sql-database-geo-restore-portal.md) | Przywracanie Geo bazy danych SQL Azure z kopii zapasowej geo zbędne (Azure Portal). |
| 168 | [Przywracanie bazy danych SQL Azure z kopii zapasowej zbędne geo przy użyciu programu PowerShell](sql-database-geo-restore-powershell.md) | Przywracanie bazy danych SQL Azure do nowego serwera z kopii zapasowej zbędne geo |



## <a name="upgrade"></a>Uaktualnianie

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 169 | [Ulepszone funkcje wydajności kwerend z zgodności 130 poziom w bazie danych SQL Azure](sql-database-compatibility-level-query-performance-130.md) | Czynności i narzędzia umożliwiające określenie poziomu zgodności, który jest zalecane dla bazy danych programu Microsoft SQL Server lub bazy danych SQL Azure |
| 170 | [Uaktualnianie przewijane aplikacje w chmurze za pomocą SQL Replikacja bazy danych Active Geo — Zarządzanie](sql-database-manage-application-rolling-upgrade.md) | Dowiedz się, jak używać geo Replikacja bazy danych SQL Azure do pomocy technicznej online uaktualnienia aplikacji chmury. |
| 171 | [Uaktualnianie do wersji 12 bazy danych SQL Azure za pomocą portalu Azure](sql-database-upgrade-server-portal.md) | Wyjaśniono, jak przeprowadzić uaktualnienie do wersji 12 bazy danych SQL Azure wraz ze uaktualnianie baz danych sieci Web i małych firm oraz serwer V11 Migrowanie bazy danych bezpośrednio w puli elastyczne bazy danych przy użyciu Azure portal uaktualnić. |
| 172 | [Uaktualnianie do wersji 12 bazy danych SQL Azure za pomocą programu PowerShell](sql-database-upgrade-server-powershell.md) | Wyjaśniono, jak przeprowadzić uaktualnienie do wersji 12 bazy danych SQL Azure wraz ze uaktualnianie baz danych sieci Web i małych firm oraz uaktualnić serwer V11 Migrowanie bazy danych bezpośrednio w puli elastyczne bazy danych przy użyciu programu PowerShell. |
| 173 | [Planowanie i przygotowywanie przeprowadzić uaktualnienie do wersji 12 bazy danych SQL](sql-database-v12-plan-prepare-upgrade.md) | W tym artykule opisano przygotowań oraz ograniczenia biorących udział w uaktualnianiu do wersji w wersji 12 z bazy danych SQL Azure. |
| 174 | [Co nowego w wersji 12 bazy danych SQL](sql-database-v12-whats-new.md) | W tym artykule opisano, dlaczego korzyść systemów biznesowych, które używają bazy danych SQL Azure w chmurze przez uaktualnienie do wersji w wersji 12 jest teraz. |
| 175 | [Witryny sieci Web i Business Edition zachód słońca — często zadawane pytania](sql-database-web-business-sunset-faq.md) | Dowiedz się, po bazy danych sieci Web SQL Azure i małych firm zostanie wycofana i informacje na temat funkcji i funkcji nowe warstwy usługi. |



## <a name="tools"></a>Narzędzia

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 176 | [Zarządzanie bazy danych Azure SQL za pomocą programu PowerShell](sql-database-command-line-tools.md) | Zarządzanie bazą danych SQL Azure przy użyciu programu PowerShell. |
| 177 | [Baza danych SQL samouczek: łączenie programu Excel z bazy danych programu Azure SQL i tworzenie raportów](sql-database-connect-excel.md) | Dowiedz się, jak nawiązywania połączenia z bazą danych Azure SQL w chmurze programu Microsoft Excel. Importowanie danych do programu Excel, raportowanie i danych badań. |
| 178 | [Tworzenie bazy danych SQL i wykonywać typowe zadania konfiguracji bazy danych przy użyciu poleceń cmdlet programu PowerShell](sql-database-get-started-powershell.md) | Dowiedz się teraz w celu utworzenia bazy danych SQL przy użyciu programu PowerShell. Typowe zadania konfiguracji bazy danych można zarządzać za pomocą poleceń cmdlet środowiska PowerShell. |
| 179 | [Wprowadzenie do synchronizacji danych Azure SQL (wersja Preview)](sql-database-get-started-sql-data-sync.md) | Ten samouczek ułatwia rozpoczęcie pracy z synchronizacją danych SQL Azure (wersja Preview). |
| 180 | [Zarządzanie bazą danych SQL Azure za pomocą programu SQL Server Management Studio](sql-database-manage-azure-ssms.md) | Dowiedz się, jak za pomocą programu SQL Server Management Studio Zarządzanie bazą danych SQL serwerami i bazami danych. |
| 181 | [Zarządzanie bazami danych programu SQL Azure za pomocą portalu Azure](sql-database-manage-portal.md) | Dowiedz się, jak zarządzać relacyjnej bazy danych w chmurze za pomocą portalu Azure za pomocą Azure Portal. |
| 182 | [Zmienianie usługi warstwy i wydajności poziomu (poziomu ceny) bazy danych SQL za pomocą programu PowerShell](sql-database-scale-up-powershell.md) | Zmiana poziomu usług i poziom wydajności bazy danych programu Azure SQL pokazano, jak przeskalować bazy danych SQL w górę lub w dół przy użyciu programu PowerShell. Zmienianie warstwie cennik bazy danych Azure SQL za pomocą programu PowerShell. |
| 183 | [Nawiązywanie połączenia z bazą danych SQL za pomocą programu SQL Server Management Studio w Azure RemoteApp](sql-database-ssms-remoteapp.md) | Użyj tego samouczka, aby dowiedzieć się, jak używać programu SQL Server Management Studio w Azure RemoteApp dotyczące zabezpieczeń i wydajności podczas nawiązywania połączenia z bazą danych SQL |



## <a name="reference"></a>Odwołania

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 184 | [Biblioteki połączeń dla bazy danych SQL i programu SQL Server](sql-database-libraries.md) | Wyświetla numer minimalna wersja dla każdego sterownika, który programów klienckich służy do łączenia się z bazą danych SQL Azure lub programu Microsoft SQL Server. Aby uzyskać informacje o sterowniki wydawanych przez społeczność, a nie przez firmę Microsoft wersji znajduje się łącze. |
| 185 | [Limity zasobów bazy danych SQL Azure](sql-database-resource-limits.md) | Na tej stronie opisano niektóre typowe ograniczenia zasobów dla bazy danych SQL Azure. |
| 186 | [Różnice bazy danych SQL języku Transact-SQL Azure](sql-database-transact-sql-information.md) | Instrukcje języka Transact-SQL, mniej niż w pełni obsługiwane w bazie danych SQL Azure |



## <a name="miscellaneous"></a>Różne

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 187 | [Kopiowanie bazy danych programu Azure SQL za pomocą programu PowerShell](sql-database-copy-powershell.md) | Utwórz kopię bazy danych programu Azure SQL za pomocą programu PowerShell |
| 188 | [Kopiowanie bazy danych programu Azure SQL za pomocą języka Transact-SQL](sql-database-copy-transact-sql.md) | Utwórz kopię bazy danych programu Azure SQL za pomocą języka Transact-SQL |
| 189 | [Ograniczenia ogólne bazy danych Azure SQL i wskazówki](sql-database-general-limitations.md) | Na tej stronie opisano pewne ograniczenia ogólne dla bazy danych SQL Azure, a także obszarów współdziałania i obsługa techniczna. |
| 190 | [Zalecenia dotyczące warstwa cennik bazy danych SQL](sql-database-service-tier-advisor.md) | Podczas zmieniania ceny poziomów w portalu Azure ceny zalecenia warstwa są pod warunkiem, że zalecane warstwie, która najlepiej nadaje się do uruchamiania obciążenie pracą istniejącej bazy danych SQL Azure w. Cennik poziomów opisuje poziom warstwa i wydajności usługi z bazą danych SQL. |


&nbsp;

<hr/>

<a name="extras_append"></a>

## <a name="extras"></a>Dodatki

<a name="extra_related_articles"></a>

### <a name="related-articles-from-other-azure-services"></a>Artykuły pokrewne z innych usług Azure

- [Wykonywanie kopii zapasowej aplikacji usługi Azure aplikacji platformy Azure](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### <a name="documentation-tools"></a>Dokumentacja narzędzia

- [Aktualizacje podawane dla bazy danych SQL Azure](http://azure.microsoft.com/updates/?service=sql-database)

- [Przeszukiwanie wszystkich typów dokumentacji Microsoft Azure filtrami](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### <a name="learning-path-graphics"></a>Grafika ścieżki szkoleniowe

- [Grafika ścieżki szkoleniowe dla wszystkich usług Microsoft Azure](http://azure.microsoft.com/documentation/learning-paths/)

- [Ścieżka nauki: Dowiedz się, baza danych SQL Azure](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)

- [Ścieżka nauki: Skala elastyczne bazy danych SQL](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)


<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->


