<properties
   pageTitle="Eksplorowanie samouczki bazy danych Azure SQL | Microsoft Azure"
   description="Więcej informacji na temat funkcji bazy danych SQL i możliwości"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="08/24/2016"
   ms.author="carlrab"/>
   
# <a name="explore-azure-sql-database-tutorials"></a>Eksplorowanie samouczki bazy danych Azure SQL

Poniższe łącza prowadzą do przeglądu każdego z obszarów funkcji wymienionych i prosty samouczek krok uruchomienia dla każdego obszaru. Zakres rozwiązań szybkich rozpoczynają się demonstrujących użycie bazy danych SQL w pełnym rozwiązaniem według rzeczywistych scenariuszy zobacz [Azure SQL bazy danych rozwiązanie szybkiego uruchamiania](sql-database-solution-quick-starts.md).

## <a name="using-sql-server-management-studio"></a>Za pomocą programu SQL Server Management Studio

W następujące samouczki dowiesz informacji na temat używania programu SQL Server Management Studio administrowania i kwerendy bazy danych SQL Azure.


> [AZURE.IMPORTANT] Zalecane jest zawsze używać najnowszej wersji programu Management Studio do pozostawać aktualizacje Microsoft Azure i baza danych SQL. [Aktualizowanie programu SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


| Samouczek  | Opis  |
|---|---|---|
| [Nawiązywanie połączenia z bazą danych SQL Azure za pomocą identyfikatora logowania głównym poziomie serwera](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| W tym samouczku dowiesz się, jak nawiązywania połączenia z bazą danych SQL Azure za pomocą identyfikatora logowania głównym poziomie serwera.|
| [Nawiązywanie połączenia z bazą danych SQL Azure jako użytkownik](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | W tym samouczku dowiesz się, jak utworzyć połączenie z bazą danych programu Azure SQL za pomocą konta użytkownika poziom bazy danych.|
||||

## <a name="elastic-pools"></a>Elastyczne pul

W następujące samouczki będą informacje o zarządzanie cele wydajności dla wielu baz danych zawierających bardzo zróżnicowanych i nieprzewidywalny upodobania za pomocą [pul elastyczne](sql-database-elastic-pool.md) .

| Samouczek  | Opis  |
|---|---|---|
| [Tworzenie puli elastyczne](sql-database-elastic-pool-create-portal.md) | W tym samouczku dowiesz się, jak tworzenie puli skalowalna baz danych programu SQL Azure. |
| [Monitorowanie elastyczne bazy danych](sql-database-elastic-pool-manage-portal.md#elastic-database-monitoring) | W tym samouczku dowiesz się, jak monitorowanie poszczególnych elastyczne bazy danych dla potencjalne problemy. |
| [Dodawanie alertu do puli zasobów](sql-database-elastic-pool-manage-portal.md#add-an-alert-to-a-pool-resource) | W tym samouczku dowiesz się, jak dodać reguły do zasobów, które powodują wysyłanie wiadomości e-mail do osoby lub alert ciągów do punktów końcowych adresu URL podczas zasobu trafienia progu wykorzystania skonfigurowanego. |
| [Przenoszenie bazy danych do puli elastyczne](sql-database-elastic-pool-manage-portal.md#move-a-database-into-an-elastic-pool) | W tym samouczku dowiesz się, jak przenieść bazę danych do puli nieelastyczne. |
| [Przenoszenie bazy danych z elastycznych puli](sql-database-elastic-pool-manage-portal.md#move-a-database-out-of-an-elastic-pool) | W tym samouczku dowiesz się, jak przenieść bazę danych z elastycznych puli. |
| [Zmienianie ustawień wydajności pula](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) | W tym samouczku dowiesz się, jak dostosować limity wydajności i miejsca do magazynowania dla zestawu. |
||||

## <a name="elastic-database-jobs"></a>Zadania nieelastyczne bazy danych

Następujące samouczki poznasz przy użyciu [zadań nieelastyczne bazy danych](sql-database-elastic-jobs-overview.md).

| Samouczek  | Opis  |
|---|---|---|
| [Wprowadzenie do narzędzia nieelastyczne bazy danych](sql-database-elastic-scale-get-started.md) | W tym samouczku dowiesz się, jak używać funkcji narzędzia elastyczne bazy danych za pomocą prostych aplikacji sharded. |
| [Wprowadzenie do zadań elastyczne bazy danych SQL Azure](sql-database-elastic-jobs-getting-started.md)  | W tym samouczku dowiesz się, jak do jak tworzyć i zarządzać nimi zadań Zarządzanie grupą z powiązanych baz danych.  | 
||||

## <a name="elastic-queries"></a>Elastyczne kwerendy

Następujące samouczki poznasz wykonywania [kwerend elastyczne](sql-database-elastic-query-overview.md). 

| Samouczek  | Opis  |
|---|---|---|
| [Badanie na poziomie podzieloną bazą danych (sharded))](sql-database-elastic-query-getting-started.md) | W tym samouczku dowiesz się, jak tworzyć raporty z wszystkich baz danych w poziomie podzieloną (sharded) bazą danych przy użyciu [kwerendy elastyczne](sql-database-elastic-query-overview.md) |
| [Kwerenda w pionie podzieloną bazą danych)](sql-database-elastic-query-getting-started-vertical.md#create-database-objects) | W tym samouczku dowiesz się, jak tworzyć raporty z wszystkich baz danych w pionie bazy danych przy użyciu [kwerendy elastyczne](sql-database-elastic-query-overview.md) |
| [Migrowanie istniejącej bazy danych do skali w nowym oknie](sql-database-elastic-convert-to-use-elastic-tools.md)| W tym samouczku dowiesz się, poziomie przeskalować bazy danych (shard) Azure SQL. |
||||

## <a name="performance-optimization"></a>Optymalizacja wydajności

Następujące samouczki poznasz optymalizacji [wydajności jednej bazy danych](sql-database-performance-guidance.md). Dotyczące optymalizowania wydajności wiele baz danych, zobacz [nieelastyczne pul](#elastic-pools).

| Samouczek  | Opis  |
|---|---|---|
| [Zmiana poziomu usług warstwowych i wydajności bazy danych](sql-database-scale-up.md#change-the-service-tier-and-performance-level-of-your-database) | W tym samouczku dowiesz się, jak do rozbudowy lub przeskalować wydajność bazy danych programu Azure SQL za pomocą warstwy usługi. |
| [Informacje wydajności kwerendy na Advisor bazy danych SQL](sql-database-performance.md#performance-overview) | W tym samouczku dowiesz się, jak za otwieranie i używanie SQL bazy danych Advisor kwerendy wydajności wglądu.|
| [Zalecenia dotyczące wydajności Advisor bazy danych SQL](sql-database-advisor.md#viewing-recommendations) | W tym samouczku dowiesz się, jak wyświetlać i stosowanie zaleceń Advisor bazy danych SQL. |
| [Przejrzyj górny Procesora przez inne kwerendy](sql-database-query-performance.md#review-top-cpu-consuming-queries)| W tym samouczku dowiesz się, jak za pomocą SQL bazy danych Advisor kwerendy wydajności wglądu Przejrzyj górny Procesora przez inne kwerendy.|
| [Wyświetlanie szczegółów pojedynczej kwerendy](sql-database-query-performance.md#viewing-individual-query-details)| W tym samouczku dowiesz się, jak aby wyświetlić szczegółowe informacje dotyczące poszczególnych kwerendy wydajności za pomocą SQL bazy danych Advisor kwerendy wydajności wglądu.|
||||

## <a name="sql-database-migration-and-archive"></a>Migracja bazy danych SQL i archiwizacji 

Następujące samouczki poznasz [migrację istniejącej bazy danych programu SQL Server do bazy danych SQL Azure](sql-database-cloud-migrate.md).

| Samouczek  | Opis  |
|---|---|---|
| [Wykrywanie problemy ze zgodnością przy użyciu programu SQL Server Data Tools for Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | W tym samouczku dowiesz się, jak do określenia zgodności bazy danych SQL Azure za pomocą programu SQL Server Data Tools for Visual Studio. |
| [Rozwiązywanie problemów ze zgodnością przy użyciu programu SQL Server Data Tools for Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt#fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | W tym samouczku dowiesz się, jak za pomocą programu SQL Server Data Tools for Visual Studio rozwiązać problemy ze zgodnością bazy danych SQL Azure. |
| [Określanie przy użyciu SqlPackage.exe zgodności bazy danych SQL](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md#using-sqlpackageexe) | W tym samouczku dowiesz się, jak za pomocą narzędzia wiersza polecenia SQLPackage.exe ustalić zgodności bazy danych SQL Azure.|
| [Określanie przy użyciu SSMS zgodności bazy danych SQL](sql-database-cloud-migrate-determine-compatibility-ssms.md#using-sql-server-management-studio) |W tym samouczku dowiesz się, jak za pomocą programu SQL Server Management Studio ustalić zgodności bazy danych SQL Azure.|
| [Migrowanie bazy danych programu SQL Server do bazy danych SQL za pomocą Kreatora bazy danych Microsoft Azure wdrażanie bazy danych](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md#use-the-deploy-database-to-microsoft-azure-database-wizard) | W tym samouczku dowiesz się, jak przeprowadzić migrację zgodne bazy danych programu SQL Server do bazy danych SQL Azure za pomocą bazy danych Kreator bazy danych Microsoft Azure wdrażanie w programie SQL Server Management Studio.
| [Eksportowanie bazy danych programu SQL Server do pliku PLECAK przy użyciu SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | W tym samouczku dowiesz się, jak eksportować zgodne bazy danych programu SQL Server w pliku PLECAK Kreator eksportu danych warstwy aplikacji w programie SQL Server Management Studio.|
| [Eksportowanie bazy danych programu SQL Server do pliku PLECAK przy użyciu SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | W tym samouczku dowiesz się, jak eksportować zgodne bazy danych programu SQL Server w pliku PLECAK przy użyciu narzędzia wiersza polecenia SQLPackage.exe.|
| [Importowanie pliku PLECAK do bazy danych SQL Azure za pomocą SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | W tym samouczku dowiesz się, jak importować bazy danych do bazy danych SQL Azure z pliku PLECAK Kreator eksportu danych warstwy aplikacji w programie SQL Server Management Studio. |
| [Importowanie pliku PLECAK do bazy danych SQL Azure za pomocą SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage) | W tym samouczku dowiesz się, jak importować bazy danych do bazy danych SQL Azure z pliku PLECAK przy użyciu narzędzia wiersza polecenia SQLPackage. |
| [Importowanie pliku PLECAK do bazy danych SQL Azure za pomocą portalu Azure](sql-database-import.md) | W tym samouczku dowiesz się, jak importować bazy danych do bazy danych SQL Azure z pliku PLECAK, który jest przechowywany w obiektów blob platformy Azure za pomocą portalu Azure.|
| [Importowanie pliku PLECAK do bazy danych SQL Azure za pomocą programu PowerShell](sql-database-import-powershell.md) | W tym samouczku dowiesz się, jak importować bazy danych do bazy danych SQL Azure z pliku PLECAK przy użyciu programu PowerShell.|
| [Archiwum bazy danych programu Azure SQL za pomocą portalu Azure](sql-database-export.md#export-your-database) | W tym samouczku dowiesz się, jak archiwizowania bazy danych programu Azure SQL plik PLECAK za pomocą portalu Azure. |
| [Archiwizowanie bazy danych programu Azure SQL za pomocą programu PowerShell](sql-database-export-powershell.md) | W tym samouczku dowiesz się, jak archiwizowania bazy danych programu Azure SQL plik PLECAK przy użyciu programu PowerShell. |
| [Kopiowanie bazy danych programu Azure SQL za pomocą portalu Azure](sql-database-copy.md#copy-your-sql-database) | W tym samouczku dowiesz się, jak skopiować bazy danych programu Azure SQL za pomocą portalu Azure. |
| [Kopiowanie bazy danych programu Azure SQL za pomocą programu PowerShell](sql-database-copy-powershell#copy-your-sql-database) | W tym samouczku dowiesz się, jak skopiować bazy danych programu Azure SQL za pomocą programu PowerShell. |
| [Kopiowanie bazy danych programu Azure SQL za pomocą języka Transact-SQL](sql-database-copy-transact-sql.md#copy-your-sql-database) | W tym samouczku dowiesz się, jak skopiować bazy danych programu Azure SQL za pomocą języka Transact-SQL. |
||||

##<a name="develop"></a>Można opracowywać

W następujące samouczki dowiesz się o [Projektowaniu bazy danych SQL](sql-database-develop-overview.md) i korzystania z [bibliotek łączności](sql-database-libraries.md).

| Samouczek  | Opis  |
|---|---|---|
| [Nawiązywanie połączenia z bazą danych SQL przy użyciu .NET (C#)](sql-database-develop-dotnet-simple.md) | W tym samouczku dowiesz się, jak nawiązywania połączenia z bazą danych Azure SQL za pomocą C#. |
| [Nawiązywanie połączenia z bazą danych SQL przy użyciu języka Java](sql-database-develop-java-simple.md) | W tym samouczku dowiesz się, jak nawiązywania połączenia z bazą danych Azure SQL za pomocą języka Java. |
| [Nawiązywanie połączenia z bazą danych SQL przy użyciu Node.js](sql-database-develop-nodejs-simple.md) | W tym samouczku dowiesz się, jak nawiązywania połączenia z bazą danych Azure SQL za pomocą Node.js. |
| [Nawiązywanie połączenia z bazą danych SQL za pomocą PHP](sql-database-develop-php-simple.md) | W tym samouczku dowiesz się, jak nawiązywania połączenia z bazą danych Azure SQL za pomocą PHP. |
| [Nawiązywanie połączenia z bazą danych SQL przy użyciu Python](sql-database-develop-python-simple.md) | W tym samouczku dowiesz się, jak nawiązywania połączenia z bazą danych Azure SQL za pomocą Python. |
| [Nawiązywanie połączenia z bazą danych SQL przy użyciu dopiskiem](sql-database-develop-ruby-simple.md) | W tym samouczku dowiesz się, jak nawiązywania połączenia z bazą danych Azure SQL za pomocą dopiskiem. |
||||
 
## <a name="database-access"></a>Dostęp do bazy danych

W następujące samouczki poznasz [Tworzenie i zarządzanie nimi logowania i użytkowników](sql-database-manage-logins.md).

| Samouczek  | Opis  |
|---|---|---|
| [Tworzenie reguły zapory na poziomie serwera bazy danych SQL Azure za pomocą portalu Azure](sql-database-configure-firewall-settings.md)  | W tym samouczku dowiesz się, jak skonfigurować zaporę poziomie serwera bazy danych SQL za pomocą portalu Azure.  |
| [Tworzenie reguły zapory poziom bazy danych przy użyciu języka Transact-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules) | W tym samouczku dowiesz się, jak tworzyć reguły zapory poziom bazy danych przy użyciu języka Transact-SQL.|
| [Zarządzanie regułami zaporę na poziomie serwera za pomocą języka Transact-SQL](sql-database-configure-firewall-settings-tsql.md#manage-server-level-firewall-rules-through-transact-sql) | W tym samouczku dowiesz się, jak zarządzać Zapora poziomie serwera bazy danych SQL Azure za pomocą języka Transact-SQL.|
| [Zarządzanie regułami zapory poziomie serwera przy użyciu programu PowerShell](sql-database-configure-firewall-settings-powershell.md#manage-firewall-rules-using-powershell) | W tym samouczku dowiesz się, jak zarządzać Zapora poziomie serwera bazy danych SQL Azure za pomocą programu PowerShell.|
| [Zarządzanie regułami zaporę na poziomie serwera za pomocą interfejsu API usługi REST](sql-database-configure-firewall-settings-rest.md#manage-firewall-rules-using-the-service-management-rest-api) | W tym samouczku dowiesz się, jak zarządzać Zapora poziomie serwera bazy danych SQL Azure za pomocą interfejsu API ZRESETOWAĆ.|
| [Nawiązywanie połączenia z bazą danych SQL Azure za pomocą identyfikatora logowania głównym poziomie serwera](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| W tym samouczku dowiesz się, jak nawiązywania połączenia z bazą danych SQL Azure za pomocą identyfikatora logowania głównym poziomie serwera.|
| [Udzielanie dostępu do bazy danych do logowania] (sql-database-manage-logins.md#granting-database-access-to-a-login() | W tym samouczku dowiesz się, jak do udzielania dostępu do bazy danych na poziomie serwera logowania.|
| [Utwórz nowego użytkownika bazy danych przy użyciu SSMS](sql-database-get-started-security.md#create-new-database-user-using-ssms) | W tym samouczku dowiesz się, jak utworzyć nowy użytkownik bazy danych w istniejącej bazy danych przy użyciu SSMS.|
| [Udzielanie uprawnień db_owner użytkownika w usłudze nowej bazy danych](sql-database-get-started-security.md#grant-new-database-user-dbowner-permissions) | W tym samouczku dowiesz się, jak do udzielania uprawnień db_owner istniejącej bazy danych.|
| [Nawiązywanie połączenia z bazą danych SQL Azure jako użytkownik](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | W tym samouczku dowiesz się, jak nawiązywania połączenia z bazą danych Azure SQL za pomocą konta użytkownika poziom bazy danych.|
||||


## <a name="data-security"></a>Bezpieczeństwo danych

Następujące samouczki poznasz [zabezpieczania danych bazy danych SQL Azure](sql-database-security.md). 

| Samouczek  | Opis  |
|---|---|---|
| [Włącz wykrywanie zagrożenie dla bazy danych przy użyciu Azure portal](sql-database-threat-detection-get-started.md#set-up-threat-detection-for-your-database) | W tym samouczku dowiesz się, jak skonfigurować wykrywania zagrożenie w portalu Azure dla bazy danych.|
| [Zabezpieczanie danych poufnych uisng zawsze szyfrowane](sql-database-always-encrypted-azure-key-vault.md) | W tym samouczku Użyj Kreatora zawsze szyfrowane do zabezpieczenia ważnych danych w bazie danych programu Azure SQL.|
| [Zabezpieczanie senstive danych przy użyciu szyfrowania danych przezroczystości](https://msdn.microsoft.com/library/dn948096.aspx)| W tym samouczku dowiesz się, jak szyfrowania przezroczysty danych do zabezpieczania danych senstive.|
| [Szyfrowanie kolumny danych](https://msdn.microsoft.com/library/ms179331.aspx)| W tym samouczku dowiesz się, jak szyfrowanie kolumny danych przy użyciu języka Transact-SQL.|
| [Konfigurowanie maskowanie dynamiczne dane](sql-database-dynamic-data-masking-get-started.md#set-up-dynamic-data-masking-for-your-database-using-the-azure-portal)  | W tym samouczku dowiesz się, jak skonfigurować maskowanie dynamiczne dane w bazie danych Azure SQL. |
||||

## <a name="business-continuity-and-query-scale-out"></a>Zapewnianie ciągłości i kwerendy skala w nowym oknie

W następujące samouczki będą informacje o korzystaniu [Geo Przywróć i replikacja Geo Active](sql-database-business-continuity.md) do reccover z błędy, ciągłości i kwerendy skala w nowym oknie.

| Samouczek  | Opis  |
|---|---|---|
| [Przywracanie bazy danych SQL Azure do poprzedniego punktu w czasie z Azure Portal](sql-database-point-in-time-restore-portal.md)| W tym samouczku dowiesz się, jak przywrócić bazy danych do wcześniejszego stanu za pomocą portalu Azure.|
| [Przywracanie bazy danych SQL Azure do poprzedniego punktu w czasie przy użyciu programu PowerShell](sql-database-point-in-time-restore-powershell.md) | W tym samouczku dowiesz się, jak przywrócić bazy danych do wcześniejszego stanu przy użyciu programu PowerShell|
| [Przywracanie usuniętego bazy danych Azure SQL za pomocą portalu Azure](sql-database-restore-deleted-database-portal.md) | W tym samouczku dowiesz się, jak przywrócić usunięty bazy danych za pomocą portalu Azure.|
| [Przywracanie usuniętego bazy danych Azure SQL za pomocą programu PowerShell](sql-database-restore-deleted-database-powershell.md) | W tym samouczku dowiesz się, jak przywrócić usunięty bazy danych przy użyciu programu PowerShell.|
| [Konfigurowanie Geo Replikacja bazy danych SQL Azure za pomocą portalu Azure](sql-database-geo-replication-portal.md)| W tym samouczku dowiesz się, jak przed skonfigurowaniem aktywnej replikacji Geo przy użyciu Azure portal.|
| [Konfigurowanie Geo Replikacja bazy danych SQL Azure za pomocą programu PowerShell](sql-database-geo-replication-powershell.md)| W tym samouczku możesz dowiedzieć się, jak skonfigurować aktywnej replikacji Geo przy użyciu programu PowerShell.|
| [Konfigurowanie Geo Replikacja bazy danych SQL Azure za pomocą języka Transact-SQL](sql-database-geo-replication-transact-sql.md)| W tym samouczku możesz dowiedzieć się, jak skonfigurować aktywnej replikacji Geo przy użyciu języka Transact-SQL.|
| [Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure za pomocą portalu Azure](sql-database-geo-replication-failover-portal.md) | W tym samouczku dowiesz się, jak przełączanie awaryjne do replice pomocniczej replikowane geo za pomocą portalu Azure.|
| [Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure za pomocą programu PowerShell](sql-database-geo-replication-failover-powershell.md) | W tym samouczku dowiesz się, jak przełączanie awaryjne do replice pomocniczej replikowane geo przy użyciu programu PowerShell.|
| [Inicjowanie planowanego lub niezaplanowane trybie awaryjnym bazy danych SQL Azure za pomocą języka Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | W tym samouczku dowiesz się, jak przełączanie awaryjne do replice pomocniczej replikowane geo przy użyciu języka Transact-SQL.|
||||

## <a name="data-sync"></a>Synchronizacja danych

W tym samouczku dowiesz się o [Synchronizacji danych](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

| Samouczek  | Opis  |
|---|---|---| 
| [Wprowadzenie do synchronizacji danych Azure SQL (wersja Preview)](sql-database-get-started-sql-data-sync.md)  | W tym samouczku Dowiedz się podstawy synchronizacja danych SQL Azure za pomocą portalu klasyczny Azure. |
||||

## <a name="next-steps"></a>Następne kroki

[Eksplorowanie Szybkie uruchamianie rozwiązanie bazy danych Azure SQL](sql-database-solution-quick-starts.md)
