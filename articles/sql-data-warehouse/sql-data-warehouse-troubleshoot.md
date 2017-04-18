<properties
   pageTitle="Rozwiązywanie problemów z magazynu danych Azure SQL | Microsoft Azure"
   description="Rozwiązywanie problemów z magazynu danych Azure SQL."
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
   ms.date="08/30/2016"
   ms.author="sonyama;barbkess"/>

# <a name="troubleshooting-azure-sql-data-warehouse"></a>Rozwiązywanie problemów z magazynu danych Azure SQL

W tym temacie przedstawiono niektóre z najczęściej pytania dotyczące rozwiązywania problemów, zadający klientów.

## <a name="connecting"></a>Nawiązywanie połączenia

| Problem                              | Rozdzielczość                                      |
| :----------------------------------| :---------------------------------------------- |
| Niepowodzenie logowania użytkownika "Zarządzanie NT\LOGOWANIE". (Program Microsoft SQL Server, błąd: 18456) | Ten błąd występuje, gdy użytkownik AAD próbuje nawiązać wzorca bazy danych, ale nie ma użytkownika we wzorcu.  Aby rozwiązać ten problem albo określenie magazynu danych SQL chcesz nawiązać połączenie w czasie połączenia lub dodać użytkownika do wzorca bazy danych.  Zobacz artykuł [Omówienie zabezpieczeń][] , aby uzyskać więcej informacji.|
|Serwer kapitału "MyUserName" nie jest mieli dostęp do bazy danych "wzorzec" w bieżącym kontekście zabezpieczeń. Nie można otworzyć bazy danych domyślnych użytkowników. Logowanie nie powiodło się. Niepowodzenie logowania użytkownika "MyUserName". (Program Microsoft SQL Server, błąd: 916) | Ten błąd występuje, gdy użytkownik AAD próbuje nawiązać wzorca bazy danych, ale nie ma użytkownika we wzorcu.  Aby rozwiązać ten problem albo określenie magazynu danych SQL chcesz nawiązać połączenie w czasie połączenia lub dodać użytkownika do wzorca bazy danych.  Zobacz artykuł [Omówienie zabezpieczeń][] , aby uzyskać więcej informacji.|
| Błąd CTAIP                        | Ten błąd może wystąpić podczas logowania została utworzona na wzorca bazy danych SQL server, ale nie w bazie danych SQL Data Warehouse.  Jeśli wystąpi ten błąd, zobacz artykuł [Omówienie zabezpieczeń][] .  W tym artykule wyjaśniono, jak tworzyć tworzenie logowania i użytkownika na wzorzec, a następnie jak utworzyć użytkownika w bazie danych SQL magazynu danych.|
| Zablokowane przez zaporę                |Bazy danych programu SQL Azure są chronione przez serwer i bazę danych zapory poziomu zapewniające tylko znane adresy IP mają dostęp do bazy danych. Zapory są bezpieczne domyślnej, co oznacza, że należy najpierw włączyć i adres IP lub zakres adresów, zanim będzie można połączyć.  Aby skonfigurować zapory dla programu access, wykonaj czynności opisane w [Konfigurowanie zapory dostęp do serwera swój adres IP klienta][] w [instrukcji obsługi][].|
| Nie można połączyć się z narzędzia lub sterownik | Program SQL Data Warehouse zaleca przy użyciu [SSMS][], [SSDT Visual Studio 2015 r][]lub [sqlcmd][] zgromadzonych danych. Aby uzyskać więcej informacji o sterowniki i nawiązywanie połączeń z magazynu danych SQL zobacz artykuły [sterowniki dla magazynu danych SQL Azure][] i [Nawiązywanie połączenia z magazynem danych SQL Azure][] .|


## <a name="tools"></a>Narzędzia

| Problem                              | Rozdzielczość                                      |
| :----------------------------------| :---------------------------------------------- |
| Brak AAD użytkowników programu Visual Studio Eksplorator obiektów | Jest to znany problem.  Aby obejść ten problem należy wyświetlić użytkowników w [sys.database_principals][].  Zobacz [uwierzytelniania do magazynu danych SQL Azure][] , aby dowiedzieć się więcej o korzystaniu z usługi Azure Active Directory z magazynu danych SQL.|

## <a name="performance"></a>Wydajność

|  Problem                             | Rozdzielczość                                      |
| :----------------------------------| :---------------------------------------------- |
| Rozwiązywanie problemów z wydajnością kwerend  | Jeśli próbujesz Rozwiązywanie problemów z kwerendy, Rozpocznij od [Dowiedz się, jak monitorowanie zapytań][].|
| Wydajność niskiej kwerendy i plany często jest wynikiem ma statystyk   | Najczęstsze przyczyny niskiej wydajności jest brak statystyk dotyczących tabel.  Zobacz [Obsługiwanie tabeli statystyki] [ Statistics] szczegółowe informacje na temat sposobu tworzenia statystyki i dlaczego są krytyczne wydajność.|
| Niska współbieżności / kwerend w kolejce   | Opis [zarządzania obciążenie pracą][] jest ważne, aby zrozumieć, jak Saldo przydzielanie pamięci z współbieżności.|
| Jak wdrażać najważniejsze wskazówki    | Najlepsze umieść Rozpocznij, aby dowiedzieć się, że można też poprawić wydajność kwerend jest artykuł [wskazówkach magazynu danych SQL][] .|
| Jak zwiększyć wydajność skalowania  | Czasami rozwiązanie do zwiększania wydajności jest po prostu dodać więcej obliczyć możliwości kwerend przez [skalowania magazynu danych SQL][].|
| Wydajność kwerend niskiej wyniku indeksu niskiej jakości | Kilka razy kwerendy można spowolnienie ze względu na [columnstore niskiej jakości indeksu][].  Zobacz ten artykuł, aby uzyskać więcej informacji i sposobu [odbudowywania indeksów, aby poprawić jakość części][].|

## <a name="system-management"></a>Zarządzanie systemem

|  Problem                             | Rozdzielczość                                      |
| :----------------------------------| :---------------------------------------------- |
| Msg 40847: Nie można wykonać operacji, ponieważ serwer przekracza dozwolony przydział jednostkę transakcji bazy danych programu 45000. | Zmniejsz [DWU][] bazy danych, którą próbujesz utworzyć lub [zażądać Zwiększ przydziału][].|
| Badanie wykorzystanie miejsca    | Zobacz [rozmiarów tabeli][] w celu zrozumienia wykorzystanie miejsca systemu.|
| Pomoc dotycząca zarządzania tabel          | Zobacz [Omówienie tabeli] [ Overview] artykuł, aby uzyskać pomoc dotyczącą zarządzania tabel.  Ten artykuł zawiera także łącza do bardziej szczegółowych tematów, takich jak [typy danych tabeli][Data types], [rozpowszechniania tabeli][Distribute], [indeksowania tabeli][Index], [podziału tabeli][Partition], [Obsługiwanie tabeli statystyki] [ Statistics] i [tabele tymczasowe][Temporary].|

## <a name="polybase"></a>Polybase

|  Problem                             | Rozdzielczość                                      |
| :----------------------------------| :---------------------------------------------- |
| Błąd UTF-8                        |  PolyBase tylko obsługuje obecnie ładowania plików danych, które zostały zakodowany UTF-8.  Aby uzyskać wskazówki na temat obejścia tego ograniczenia, zobacz [Praca wokół wymagania PolyBase UTF-8][] .|
| Ładowanie nie powiedzie się ze względu na duże wiersze   | Obsługa dużych wierszy nie jest obecnie dostępna dla Polybase.  Oznacza to, że jeśli tabela zawiera NVARCHAR(MAX) VARCHAR(MAX) oraz VARBINARY(MAX), zewnętrzne tabele nie można załadować danych.  Obciążenia dla dużych wierszy jest obecnie obsługiwany tylko przez Azure Factory danych (z BCP), analizy strumieniu Azure, SSIS, BCP lub klasy .NET SQLBulkCopy. Obsługa PolyBase dla dużych wierszy zostanie dodana w przyszłej wersji.|
| Ładowanie BCP tabeli z typem danych MAX kończy się niepowodzeniem | Jest to znany problem, która wymaga VARCHAR(MAX), NVARCHAR(MAX) lub VARBINARY(MAX) znajdować się na końcu tabeli, w niektórych scenariuszach.  Spróbuj przenieść maksymalna liczba kolumn na końcu tabeli.|

## <a name="differences-from-sql-database"></a>Różnice w stosunku do bazy danych SQL

|  Problem                             | Rozdzielczość                                      |
| :----------------------------------| :---------------------------------------------- |
| Nieobsługiwane funkcje bazy danych SQL  | Zobacz [nieobsługiwane funkcje tabel][].|
| Nieobsługiwane typy danych bazy danych SQL  | Zobacz [nieobsługiwane typy danych][].|
| Usuń i AKTUALIZUJ ograniczenia      | Zobacz [obejścia AKTUALIZUJ][], [Usuń obejścia][] i [Przy użyciu CTAS w celu obejścia nieobsługiwane składni aktualizacji i Usuń][].  |
| Instrukcja korespondencji seryjnej nie jest obsługiwane.   | Zobacz [SCALANIE obejścia][].|
| Procedura składowana ograniczenia       | Zobacz [ograniczenia procedury przechowywane][] do zrozumienia ograniczenia dotyczące procedur składowanych.|
| Funkcji zdefiniowanych przez użytkownika nie obsługują instrukcji SELECT | Jest to bieżące ograniczenie naszych funkcji zdefiniowanych przez użytkownika.  Informacje na temat składni, które obsługujemy, zobacz temat [Tworzenie funkcja][] .   |

## <a name="next-steps"></a>Następne kroki

Jeśli zostały nie można znaleźć rozwiązanie problemu powyżej, poniżej przedstawiono niektóre inne zasoby, możesz wypróbować.

- [Blogów]
- [Funkcja żądania]
- [Klipy wideo]
- [Blogów zespołu KOTA]
- [Tworzenie bilet pomocy technicznej]
- [Forum w witrynie MSDN]
- [Forum przepełnienia stosu]
- [Twitter]

<!--Image references-->

<!--Article references-->
[Omówienie zabezpieczeń]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT Visual Studio 2015 r.]: ./sql-data-warehouse-install-visual-studio.md
[Sterowniki magazynu danych Azure SQL]: ./sql-data-warehouse-connection-strings.md
[Nawiązywanie połączenia z magazynem danych Azure SQL]: ./sql-data-warehouse-connect-overview.md
[Tworzenie bilet pomocy technicznej]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Skalowanie magazynu danych SQL]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[Żądanie zwiększenia przydziału]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change 
[Dowiedz się, jak monitorowanie zapytań]: ./sql-data-warehouse-manage-monitor.md
[Instrukcje obsługi administracyjnej]: ./sql-data-warehouse-get-started-provision.md
[Konfigurowanie dostępu przez zaporę serwera dla swój adres IP klienta]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[Najważniejsze wskazówki dotyczące magazynu danych SQL]: ./sql-data-warehouse-best-practices.md
[Rozmiar tabeli]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Nieobsługiwane funkcje tabel]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Nieobsługiwane typy danych]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Columnstore niskiej jakości indeksu]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Odbudowywanie indeksy, aby poprawić jakość części]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Zarządzanie pracą]: ./sql-data-warehouse-develop-concurrency.md
[Za pomocą CTAS w celu obejścia nieobsługiwane składni aktualizacji i usuwania]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[AKTUALIZOWANIE rozwiązania]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Usuwanie rozwiązania]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[SCALANIE obejścia]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Procedura składowana ograniczenia]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Uwierzytelnianie do magazynu danych Azure SQL]: ./sql-data-warehouse-authentication.md
[Praca wokół wymagania PolyBase UTF-8]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[TWORZENIE FUNKCJI]: https://msdn.microsoft.com/library/mt203952.aspx
[sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogów]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogów zespołu KOTA]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funkcja żądania]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Forum w witrynie MSDN]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Forum przepełnienia stosu]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Klipy wideo]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse

