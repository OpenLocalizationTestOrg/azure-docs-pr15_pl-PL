<properties
    pageTitle="Wszystkich tematów dotyczących usługi SQL Data Warehouse | Microsoft Azure"
    description="Tabela wszystkich tematów dotyczących usługi Azure o nazwie magazynu danych SQL, która istnieje w http://azure.microsoft.com/documentation/articles/, tytuł i opis."
    services="sql-data-warehouse"
    documentationCenter=""
    authors="barbkess"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-data-warehouse"
    ms.workload="sql-data-warehouse"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="barbkess"/>


# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Wszystkich tematów dotyczących usługi magazynu danych SQL Azure

Ten temat zawiera listę wszystkich tematów, zastosowany bezpośrednio do usługi **Magazynu danych SQL** Azure. Ta strona sieci Web słów kluczowych można wyszukiwać za pomocą **Klawiszy Ctrl + F**, aby znaleźć tematy odsetki bieżące.




## <a name="new"></a>Nowy

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 1 | [Kopie zapasowe magazynu danych SQL](sql-data-warehouse-backups.md) | Informacje na temat kopie zapasowe wbudowanych bazy danych SQL Data Warehouse, umożliwiające przywracanie magazynu danych SQL Azure punkt przywracania lub innego regionu geograficznego. |


## <a name="updated-articles-sql-data-warehouse"></a>Zaktualizowane artykułów, magazynu danych SQL

W tej sekcji przedstawiono artykułów, które zostały zaktualizowane w ostatnim tam, gdzie aktualizacji nie duży lub znaczące. Dla każdego artykułu zaktualizowane drukowania fragment tekstu dodano promocji cenowych jest wyświetlany. Artykuły zostały zaktualizowane w zakresie data **2016-08-22** do **2016-10-05**.

| &nbsp; | Artykuł | Zaktualizowany tekst, wstawki kodu | Zaktualizowane |
| --: | :-- | :-- | :-- |
| 2 | [Ładowanie danych z magazynem obiektów blob Azure do magazynu danych SQL (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | -— Umożliwia śledzenie bajtów i plików wybierz r.command, s.request_id, r.status, liczba (odrębnych input_name) jako nbr_files, Suma (s.bytes_processed)-1024-1024 jako gb_processed z sys.dm_pdw_exec_requests r sprzężenia wewnętrznego sys.dm_pdw_dms_external_work s na r.request_id = s.request_id gdzie r. Etykieta = "CTAS: cso ładowania. Wymiarprodukt "lub r. Etykieta = "CTAS: cso ładowania. FactOnlineSales GROUP BY r.command, s.request_id, r.status ORDER BY nbr_files desc gb_processed desc;  | 2016-09-07 |
| 3 | [Przywracanie magazynu danych SQL](sql-data-warehouse-restore-database-overview.md) | **Czy można przywrócić magazynu wstrzymanych danych?** Aby przywrócić magazynu danych, która została wstrzymana, musisz najpierw przełączyć go do trybu online. Po powrocie do trybu online magazynu danych masz siedem dni punktów przywracania do wyboru. **Przywracanie zbędne geo regionu** Jeśli korzystasz z zbędne geo przestrzeni dyskowej, można przywrócić magazynu danych do centrum danych iloczynów w innego regionu geograficznego. Po przywróceniu magazynu danych z ostatniego codziennego wykonywania kopii zapasowej. **Przywracanie osi czasu** Bazy danych z dowolnego punktu przywracania można przywrócić w ciągu ostatnich siedmiu dni. Migawki Uruchom co godziny czterech do ośmiu i są dostępne przez siedem dni. W przypadku starszych niż siedem dni migawkę jej wygaśnięciu i punktu przywracania nie jest już dostępna. **Przywracanie kosztów** Dodatkowego miejsca do magazynowania dla magazynu przywracane dane jest wystawiona stawki magazyn Premium Azure. Jeśli zatrzymasz magazynu przywracane dane są naliczane do przechowywania stawki magazyn Premium Azure. Zaletą wstrzymywanie jest, że nie jest opłata | 2016-09-29 |





## <a name="get-started"></a>Rozpoczynanie pracy

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 4 | [Uwierzytelnianie do magazynu danych Azure SQL](sql-data-warehouse-authentication.md) | Azure Active Directory (AAD) i SQL Server uwierzytelnianie do magazynu danych SQL Azure. |
| 5 | [Najważniejsze wskazówki dotyczące magazynu danych SQL Azure](sql-data-warehouse-best-practices.md) | Zalecenia i najważniejsze wskazówki, których warto wiedzieć, jak można opracowywać rozwiązania dla magazynu danych SQL Azure. Te pomoże Ci się pomyślnie. |
| 6 | [Sterowniki magazynu danych Azure SQL](sql-data-warehouse-connection-strings.md) | Parametry połączenia i sterowników dla magazynu danych SQL |
| 7 | [Nawiązywanie połączenia z magazynem danych Azure SQL](sql-data-warehouse-connect-overview.md) | Jak znaleźć serwera nazw i ciąg połączenia dla swojego do magazynu danych SQL Azure |
| 8 | [Analizowanie danych za pomocą nauki maszynowego Azure](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) | Do utworzenia przewidywanych maszynowego uczenia modelu, na podstawie danych przechowywanych w magazynie danych SQL Azure za pomocą nauki maszynowego Azure. |
| 9 | [Kwerenda SQL Azure, Data Warehouse (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) | Kwerenda magazynu danych SQL Azure z wiersza polecenia narzędzia sqlcmd. |
| 10 | [Tworzenie bazy danych SQL magazynu danych przy użyciu języka Transact-SQL (TSQL)](sql-data-warehouse-get-started-create-database-tsql.md) | Dowiedz się, jak utworzyć magazynu danych SQL Azure z TSQL |
| 11 | [Jak utworzyć bilet pomocy technicznej dla magazynu danych SQL](sql-data-warehouse-get-started-create-support-ticket.md) | Jak utworzyć bilet pomocy technicznej w magazynie danych SQL Azure. |
| 12 | [Ładowanie danych za pomocą Factory Azure danych](sql-data-warehouse-get-started-load-with-azure-data-factory.md) | Dowiedz się, jak załadować dane z Factory danych Azure |
| 13 | [Ładowanie danych za pomocą PolyBase w magazynie danych SQL](sql-data-warehouse-get-started-load-with-polybase.md) | Dowiedz się, co to jest PolyBase i jak z niego korzystać dla danych składu scenariuszy. |
| 14 | [Tworzenie magazynu danych Azure SQL](sql-data-warehouse-get-started-provision.md) | Dowiedz się, jak tworzyć magazynu danych SQL Azure w portalu Azure |
| 15 | [Tworzenie magazynu danych SQL przy użyciu programu PowerShell](sql-data-warehouse-get-started-provision-powershell.md) | Tworzenie magazynu danych SQL przy użyciu programu PowerShell |
| 16 | [Wizualizowanie danych za pomocą usługi Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) | Wizualizowanie danych SQL magazynu danych dzięki usłudze Power BI |
| 17 | [Kwerenda Azure SQL Data Warehouse (Visual Studio)](sql-data-warehouse-query-visual-studio.md) | Magazyn danych SQL kwerendy przy użyciu programu Visual Studio. |



## <a name="develop"></a>Można opracowywać

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 18 | [Optymalizacja transakcji dla magazynu danych SQL](sql-data-warehouse-develop-best-practices-transactions.md) | Najważniejsze wskazówki praktyki dotyczące pisania aktualizacje wydajność transakcji w magazynie danych SQL Azure |
| 19 | [Zarządzanie współbieżności i obciążenie pracą w magazynie danych SQL](sql-data-warehouse-develop-concurrency.md) | Opis zarządzania współbieżności i obciążenie pracą w magazynie danych SQL Azure dla opracowania rozwiązań. |
| 20 | [Tworzenie tabeli jako wybierz pozycję (CTAS) w magazynie danych SQL](sql-data-warehouse-develop-ctas.md) | Porady dotyczące kodowania z Utwórz tabelę jako instrukcji select (CTAS) w magazynie danych SQL Azure dla opracowania rozwiązań. |
| 21 | [Dynamiczne SQL w magazynie danych SQL](sql-data-warehouse-develop-dynamic-sql.md) | Porady dotyczące korzystania z dynamicznego kodu SQL w magazynie danych SQL Azure dla opracowania rozwiązań. |
| 22 | [Grupowanie według opcji w magazynie danych SQL](sql-data-warehouse-develop-group-by-options.md) | Porady dotyczące wykonywania grupy opcji w magazynie danych SQL Azure do opracowania rozwiązań. |
| 23 | [Użyj etykiet do dokumentu kwerend w magazynie danych SQL](sql-data-warehouse-develop-label.md) | Porady dotyczące korzystania z etykiety do dokumentu kwerend w magazynie danych SQL Azure dla opracowania rozwiązań. |
| 24 | [Pętle w magazynie danych SQL](sql-data-warehouse-develop-loops.md) | Porady dotyczące pętli języku Transact-SQL i zastąpienie kursory w magazynie danych SQL Azure dla opracowania rozwiązań. |
| 25 | [Procedur składowanych w magazynie danych SQL](sql-data-warehouse-develop-stored-procedures.md) | Porady dotyczące wykonywania procedur składowanych w magazynie danych SQL Azure do opracowania rozwiązań. |
| 26 | [Transakcje w magazynie danych SQL](sql-data-warehouse-develop-transactions.md) | Porady dotyczące wykonywania transakcji w magazynie danych SQL Azure do opracowania rozwiązań. |
| 27 | [Schematy zdefiniowane przez użytkownika w magazynie danych SQL](sql-data-warehouse-develop-user-defined-schemas.md) | Porady dotyczące używania schematy języku Transact-SQL w magazynie danych SQL Azure dla opracowania rozwiązań. |
| 28 | [Przypisywanie zmiennych w magazynie danych SQL](sql-data-warehouse-develop-variable-assignment.md) | Porady dotyczące przypisywanie zmiennych w języku Transact-SQL w magazynie danych SQL Azure dla opracowania rozwiązań. |
| 29 | [Widoki w magazynie danych SQL](sql-data-warehouse-develop-views.md) | Porady dotyczące korzystania z widoków w języku Transact-SQL w magazynie danych SQL Azure dla opracowania rozwiązań. |
| 30 | [Decyzje projektowe i technik kodowania dla magazynu danych SQL](sql-data-warehouse-overview-develop.md) | Pojęcia rozwój, decyzje projektowe, zalecenia i technik kodowania dla magazynu danych SQL. |



## <a name="manage"></a>Zarządzanie

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 31 | [Zarządzanie power obliczeń w magazynie danych SQL Azure (omówienie)](sql-data-warehouse-manage-compute-overview.md) | Skala wydajności się możliwości w magazynie danych SQL Azure. Możliwość skalowania dostosowując DWUs lub wstrzymywanie i wznawianie zasobów obliczeń kosztów. |
| 32 | [Zarządzanie power obliczeń w magazynie danych SQL Azure (Azure portal)](sql-data-warehouse-manage-compute-portal.md) | Azure portalu zadań w celu zarządzania obliczyć power. Skala obliczyć zasobów za pomocą dostosowania DWUs. Lub wstrzymywanie i wznawianie obliczania kosztów zasobów. |
| 33 | [Zarządzanie power obliczeń w magazynie danych SQL Azure (programu PowerShell)](sql-data-warehouse-manage-compute-powershell.md) | Zadania programu PowerShell do zarządzania obliczyć power. Skala obliczyć zasobów za pomocą dostosowania DWUs. Lub wstrzymywanie i wznawianie obliczenia kosztów zasobów. |
| 34 | [Zarządzanie power obliczeń w magazynie danych SQL Azure (RESZTA)](sql-data-warehouse-manage-compute-rest-api.md) | Zadania programu PowerShell do zarządzania obliczyć power. Skala obliczyć zasobów za pomocą dostosowania DWUs. Lub wstrzymywanie i wznawianie obliczenia kosztów zasobów. |
| 35 | [Zarządzanie power obliczeń w magazynie danych SQL Azure (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) | Zadania Transact-SQL (T-SQL) do poza skalowanie wydajności za pomocą dostosowania DWUs. Aby zmniejszyć koszty, skalowania Wstecz w czasie nie Szczyt. |
| 36 | [Monitorowanie z pracą przy użyciu DMVs](sql-data-warehouse-manage-monitor.md) | Dowiedz się, jak można monitorować z pracą przy użyciu DMVs. |
| 37 | [Zarządzanie bazami danych w magazynie danych SQL Azure](sql-data-warehouse-overview-manage.md) | Omówienie Zarządzanie bazami danych SQL magazynu danych. Zawiera narzędzia do zarządzania, DWUs i wydajność skala w nowym oknie, rozwiązywanie problemów z wydajności kwerend, ustanawianie zasad zabezpieczeń dobre i przywracanie bazy danych z uszkodzenie danych lub awaria regionalne. |
| 38 | [Monitorowanie kwerend użytkownika w magazynie danych SQL Azure](sql-data-warehouse-overview-manage-user-queries.md) | Omówienie zagadnienia, najważniejsze wskazówki i zadania do monitorowania kwerend użytkownika w magazynie danych SQL Azure |
| 39 | [Przywracanie magazynu danych SQL](sql-data-warehouse-restore-database-overview.md) | Omówienie opcji przywracania bazy danych do bazy danych w magazynie danych SQL Azure odzyskiwania. |
| 40 | [Przywracanie magazynu danych Azure SQL (Portal)](sql-data-warehouse-restore-database-portal.md) | Azure portalu zadania przywracania magazynu danych SQL Azure. |
| 41 | [Przywracanie magazynu danych Azure SQL (programu PowerShell)](sql-data-warehouse-restore-database-powershell.md) | Zadania programu PowerShell przywracania magazynu danych SQL Azure. |
| 42 | [Przywracanie magazynu danych Azure SQL (interfejsu API usługi REST)](sql-data-warehouse-restore-database-rest-api.md) | Zadania interfejsu API usługi REST przywracania magazynu danych SQL Azure. |



## <a name="tables-and-indexes"></a>Tabele i indeksy

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 43 | [Typy danych dla tabel w magazynie danych SQL](sql-data-warehouse-tables-data-types.md) | Wprowadzenie do typów danych dla tabel magazynu danych SQL Azure. |
| 44 | [Rozpowszechnianie tabel w magazynie danych SQL](sql-data-warehouse-tables-distribute.md) | Wprowadzenie do rozpowszechniania tabel w magazynie danych SQL Azure. |
| 45 | [Indeksowanie tabel w magazynie danych SQL](sql-data-warehouse-tables-index.md) | Wprowadzenie do tabeli indeksowania w magazynie danych SQL Azure. |
| 46 | [Omówienie tabel w magazynie danych SQL](sql-data-warehouse-tables-overview.md) | Wprowadzenie do tabel magazynu danych SQL Azure. |
| 47 | [Tabele podziału w magazynie danych SQL](sql-data-warehouse-tables-partition.md) | Wprowadzenie do tabeli podziału w magazynie danych SQL Azure. |
| 48 | [Zarządzanie statystykę w tabelach w magazynie danych SQL](sql-data-warehouse-tables-statistics.md) | Wprowadzenie do programu statystyk dotyczących tabel w magazynie danych SQL Azure. |
| 49 | [Tabele tymczasowe w magazynie danych SQL](sql-data-warehouse-tables-temporary.md) | Wprowadzenie do tabel tymczasowych w magazynie danych SQL Azure. |



## <a name="integrate"></a>Integracja

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 50 | [Używanie Factory Azure danych z magazynu danych SQL](sql-data-warehouse-integrate-azure-data-factory.md) | Porady dotyczące używania Azure fabrycznych danych (ADF) z magazynu danych SQL Azure dla opracowania rozwiązań. |
| 51 | [Nauka Azure komputera za pomocą magazynu danych SQL](sql-data-warehouse-integrate-azure-machine-learning.md) | Samouczek używanie Azure maszynowego uczenia z magazynu danych SQL Azure dla opracowania rozwiązań. |
| 52 | [Używanie analizy strumieniu Azure z magazynu danych SQL](sql-data-warehouse-integrate-azure-stream-analytics.md) | Porady dotyczące korzystania z analizy strumieniu Azure z magazynu danych SQL Azure dla opracowania rozwiązań. |
| 53 | [Używanie usługi Power BI z magazynu danych SQL](sql-data-warehouse-integrate-power-bi.md) | Porady dotyczące korzystania z usługi Power BI z magazynu danych SQL Azure dla opracowania rozwiązań. |
| 54 | [Korzystać z innych usług z magazynu danych SQL](sql-data-warehouse-overview-integrate.md) | Narzędzia i partnerom rozwiązań, które zostały zintegrowane z magazynu danych SQL.  |



## <a name="load"></a>Ładowanie

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 55 | [Ładowanie danych z magazynem obiektów blob Azure do magazynu danych SQL Azure (Factory danych Azure)](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) | Dowiedz się, jak załadować dane z Factory danych Azure |
| 56 | [Ładowanie danych z magazynem obiektów blob Azure do magazynu danych SQL (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Dowiedz się, jak za pomocą PolyBase załadować dane z magazynem obiektów blob Azure do magazynu danych SQL. Załaduj kilka tabel z danych publicznych w schemacie Contoso detalicznym Data Warehouse. |
| 57 | [Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) | Używa bcp, aby wyeksportować dane z programu SQL Server do prostym plików, AZCopy, aby zaimportować dane z magazynem obiektów blob platformy Azure i PolyBase, aby mogły zjeść tej ostatniej danych do magazynu danych SQL Azure. |
| 58 | [Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (płaskie plików)](sql-data-warehouse-load-from-sql-server-with-bcp.md) | W przypadku małych danych, rozmiar używa bcp eksportowanie danych z programu SQL Server do plików prostych i zaimportować dane bezpośrednio do magazynu danych SQL Azure. |
| 59 | [Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (SSIS)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) | Pokazano, jak utworzyć pakiet SQL Server Integration Services (SSIS) do przenoszenia danych z wielu różnych źródeł danych do magazynu danych SQL. |
| 60 | [Ładowanie danych za pomocą PolyBase w magazynie danych SQL](sql-data-warehouse-load-from-sql-server-with-polybase.md) | Używa bcp, aby wyeksportować dane z programu SQL Server do prostym plików, AZCopy, aby zaimportować dane z magazynem obiektów blob platformy Azure i PolyBase, aby mogły zjeść tej ostatniej danych do magazynu danych SQL Azure. |
| 61 | [Przewodnik dotyczący przy użyciu PolyBase w magazynie danych SQL](sql-data-warehouse-load-polybase-guide.md) | Wskazówki i zalecenia dotyczące korzystania z PolyBase w scenariuszach magazynu danych SQL. |
| 62 | [Ładowanie przykładowych danych do magazynu danych SQL](sql-data-warehouse-load-sample-databases.md) | Ładowanie przykładowych danych do magazynu danych SQL |
| 63 | [Ładowanie danych za pomocą bcp](sql-data-warehouse-load-with-bcp.md) | Dowiedz się, jakie bcp jest i jak używać tej funkcji dla danych składu scenariuszy. |
| 64 | [Ładowanie danych do magazynu danych SQL Azure](sql-data-warehouse-overview-load.md) | Dowiedz się, typowe scenariusze ładowania do magazynu danych SQL danych. Dotyczy to przy użyciu PolyBase, magazyn obiektów blob platformy Azure, plików płaskich i wysyłki dysku. Umożliwia także narzędzi innych firm. |



## <a name="migrate"></a>Migrowanie

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 65 | [Migrowanie kodu SQL do magazynu danych SQL](sql-data-warehouse-migrate-code.md) | Porady dotyczące migracja kodu SQL do magazynu danych SQL Azure dla opracowania rozwiązań. |
| 66 | [Migrowanie danych](sql-data-warehouse-migrate-data.md) | Porady dotyczące migrację danych do magazynu danych SQL Azure dla opracowania rozwiązań. |
| 67 | [Narzędzie migracji magazynu danych (wersja Preview)](sql-data-warehouse-migrate-migration-utility.md) | Migrowanie do magazynu danych SQL. |
| 68 | [Migrowanie schematu do magazynu danych SQL](sql-data-warehouse-migrate-schema.md) | Porady dotyczące migracja schematu do magazynu danych SQL Azure dla opracowania rozwiązań. |
| 69 | [Migrowanie rozwiązania do magazynu danych SQL](sql-data-warehouse-overview-migrate.md) | Wytyczne migracji za rozwiązania do magazynu danych SQL Azure platformy. |



## <a name="partners"></a>Partnerzy

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 70 | [Partnerów analiz biznesowych magazynu danych SQL](sql-data-warehouse-partner-business-intelligence.md) | Listę partnerów analiz biznesowych innych firm przy użyciu rozwiązań, które obsługują magazynu danych SQL. |
| 71 | [Partnerzy integracji danych magazynu danych SQL](sql-data-warehouse-partner-data-integration.md) | Listę partnerów innych firm dla rozwiązań integracji danych, które obsługują magazynu danych SQL Azure. |
| 72 | [Partnerzy zarządzania danymi magazynu danych SQL](sql-data-warehouse-partner-data-management.md) | Wykaz danych innych firm zarządzania partnerom rozwiązania, które obsługują magazynu danych SQL. |



## <a name="reference"></a>Odwołania

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 73 | [Tematy dodatkowe dla magazynu danych SQL](sql-data-warehouse-overview-reference.md) | Zawartość odnośniki dla magazynu danych SQL. |
| 74 | [Polecenia cmdlet programu PowerShell oraz interfejsy API pozostałych dla magazynu danych SQL](sql-data-warehouse-reference-powershell-cmdlets.md) | Znajdź górny poleceń cmdlet programu PowerShell dla magazynu danych SQL Azure w tym jak wstrzymać i wznowić bazy danych. |
| 75 | [Elementy języka](sql-data-warehouse-reference-tsql-language-elements.md) | Lista łączy do materiały dla elementów języka Transact-SQL używane dla magazynu danych SQL. |
| 76 | [Tematy Transact-SQL](sql-data-warehouse-reference-tsql-statements.md) | Łącza do materiały dla tematy języku Transact-SQL używane przez program SQL Data Warehouse. |
| 77 | [Widoki systemowe](sql-data-warehouse-reference-tsql-system-views.md) | Łącza do systemu wyświetla zawartość dla magazynu danych SQL. |



## <a name="security"></a>Zabezpieczenia

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 78 | [Magazyn danych SQL - klientów niższego poziomu obsługę inspekcji i dynamicznych danych maskowanie](sql-data-warehouse-auditing-downlevel-clients.md) | Więcej informacji na temat obsługi klientów niższych poziomów magazynu danych SQL dane inspekcji |
| 79 | [Inspekcja w magazynie danych Azure SQL](sql-data-warehouse-auditing-overview.md) | Rozpoczynanie pracy z inspekcji w magazynie danych SQL Azure |
| 80 | [Rozpoczynanie pracy z przezroczysty danych szyfrowania (TDE) w magazynie danych SQL](sql-data-warehouse-encryption-tde.md) | Szyfrowanie danych przezroczysty (TDE) w magazynie danych SQL |
| 81 | [Rozpoczynanie pracy z szyfrowania danych przezroczysty (TDE)](sql-data-warehouse-encryption-tde-tsql.md) | Szyfrowanie danych przezroczysty (TDE) w magazynie danych SQL (T-SQL) |
| 82 | [Zabezpieczanie bazy danych w magazynie danych SQL](sql-data-warehouse-overview-manage-security.md) | Porady dotyczące Zabezpieczanie bazy danych w magazynie danych SQL Azure dla opracowania rozwiązań. |



## <a name="miscellaneous"></a>Różne

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 83 | [Instalowanie programu Visual Studio 2015 i SSDT dla magazynu danych SQL](sql-data-warehouse-install-visual-studio.md) | Instalowanie programu Visual Studio i narzędzia rozwoju programu SQL Server (SSDT) dla magazynu danych Azure SQL |
| 84 | [Migracja do szczegółów miejsca do magazynowania Premium](sql-data-warehouse-migrate-to-premium-storage.md) | Instrukcje dotyczące migrowania istniejący magazyn danych SQL do magazynowania premium |
| 85 | [Wprowadzenie do wykrywania zagrożenie](sql-data-warehouse-security-threat-detection.md) | Jak rozpocząć pracę z wykrywaniem zagrożenie |
| 86 | [Ograniczenie pojemności magazynu danych SQL](sql-data-warehouse-service-capacity-limits.md) | Maksymalna liczba wartości dla połączenia, baz danych, tabel i kwerend dla magazynu danych SQL. |
| 87 | [Rozwiązywanie problemów z magazynu danych Azure SQL](sql-data-warehouse-troubleshoot.md) | Rozwiązywanie problemów z magazynu danych Azure SQL. |

