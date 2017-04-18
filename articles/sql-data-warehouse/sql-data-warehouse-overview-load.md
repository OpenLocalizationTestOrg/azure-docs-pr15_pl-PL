   <properties
   pageTitle="Ładowanie danych do magazynu danych SQL Azure | Microsoft Azure"
   description="Dowiedz się, typowe scenariusze ładowania do magazynu danych SQL danych. Dotyczy to przy użyciu PolyBase, magazyn obiektów blob platformy Azure, plików płaskich i wysyłki dysku. Umożliwia także narzędzi innych firm."
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
   ms.date="07/12/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-into-azure-sql-data-warehouse"></a>Ładowanie danych do magazynu danych SQL Azure

Podsumowanie opcji scenariusz i zalecenia dotyczące ładowania danych do magazynu danych SQL.

Najtrudniejsze część ładowania danych jest zazwyczaj przygotowywanie danych dla Załaduj. Azure upraszcza ładowania przy użyciu magazynem obiektów blob Azure typowych przechowywania danych dla wielu usług, a następnie dodać akompaniament ruchu komunikacji i danych między usług Azure za pomocą Azure danych Factory. Te procesy zostały zintegrowane z technologii PolyBase, która używa znacznym stopniu równoległego przetwarzania (MPP) aby załadować dane równolegle z magazynem obiektów blob Azure do magazynu danych SQL. 

Samouczki, które są ładowane przykładowe bazy danych zobacz [Ładowanie przykładowe bazy danych][].

## <a name="load-from-azure-blob-storage"></a>Ładowanie z magazynu obiektów blob platformy Azure
Najszybszym sposobem importowania danych do magazynu danych SQL ma załadować dane z magazynem obiektów blob Azure za pomocą PolyBase. PolyBase korzysta magazynu danych SQL przetwarzanie znacznym stopniu równoległe (MPP) projektu, aby załadować dane równolegle z magazynem obiektów blob Azure. Aby użyć PolyBase, służy poleceń T SQL lub planowana Azure danych Factory.

### <a name="1-use-polybase-and-t-sql"></a>1. przy użyciu PolyBase i T-SQL

Podsumowanie procesu ładowania:

2. Formatowanie danych jako UTF-8, ponieważ PolyBase nie obsługuje obecnie UTF-16.
2. Przenoszenie danych z magazynem obiektów blob platformy Azure i zapisać go w plikach tekstowych.
3. Konfigurowanie obiektów zewnętrznych w magazynie danych SQL, aby zdefiniować lokalizację i format danych
4. Uruchamianie polecenia T-SQL, aby załadować dane równolegle do nowej tabeli bazy danych.

<!-- 5. Schedule and run a loading job. --> 

Samouczek zobacz [Ładowanie danych z magazynem obiektów blob Azure do magazynu danych SQL (PolyBase)][].

### <a name="2-use-azure-data-factory"></a>2. fabrycznych Azure danych użyj

Dla prostszy sposób za pomocą PolyBase można utworzyć potok Azure Factory danych używaną PolyBase, aby załadować dane z magazynem obiektów blob Azure do magazynu danych SQL. To jest szybkie skonfigurowanie, ponieważ nie należy zdefiniować obiektów T-SQL. Jeśli musisz kwerendy bez importowania danych zewnętrznych za pomocą T-SQL. 

Podsumowanie proces ładowania:

2. Formatowanie danych jako UTF-8, ponieważ PolyBase nie obsługuje obecnie UTF-16.
2. Przenoszenie danych z magazynem obiektów blob platformy Azure i zapisać go w plikach tekstowych.
3. Tworzenie planowana Factory danych Azure, aby mogły zjeść tej ostatniej danych. Za pomocą opcji PolyBase.
4. Planowanie i prowadzenie proces.

Samouczek zobacz [Ładowanie danych z magazynem obiektów blob Azure do magazynu danych SQL (Factory danych Azure)][].


## <a name="load-from-sql-server"></a>Ładowanie z programu SQL Server
Aby załadować dane z programu SQL Server do magazynu danych SQL służy Integration Services (SSIS), transfer plików prostym lub dysków wysyłki do firmy Microsoft. Odczytu było wyświetlić podsumowanie różnych ładowanie procesów i łącza do samouczków.

Planowanie migracji danych z programu SQL Server do magazynu danych SQL, zobacz [Omówienie migracji][]. 

### <a name="use-integration-services-ssis"></a>Za pomocą usług Integration Services (SSIS)
Jeśli już używasz pakietów Integration Services (SSIS) aby załadować do programu SQL Server, możesz zaktualizować pakiety używać programu SQL Server jako źródła i magazynu danych SQL jako miejsca docelowego. To jest szybkie i łatwe w celu i jest dobrym rozwiązaniem, jeśli nie chcesz przeprowadzić migrację procesu ładowania, aby użyć danych już w chmurze. Zależnościami jest obciążenie może być mniejsza niż przy użyciu PolyBase, ponieważ ten SSIS wykonuje Załaduj równolegle.

Podsumowanie procesu ładowania:

1. Sprawdź, czy pakiet usług Integration Services wskaż wystąpienie programu SQL Server dla źródła i baza danych SQL Data Warehouse miejsca docelowego.
2. Migrowanie schematu do magazynu danych SQL, jeśli nie jest już.
3. Zmień mapowanie w pakiety za pomocą typów danych, które są obsługiwane przez program SQL Data Warehouse.
3. Planowanie i prowadzenie pakietu.

Samouczek zobacz [Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (SSIS)][].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Używanie AZCopy (zalecane dla danych < 10 TB)
Jeśli rozmiar danych jest < 10 TB, można wyeksportować dane z programu SQL Server do plików prostych, skopiować pliki z magazynem obiektów blob platformy Azure i załadować dane do magazynu danych SQL za pomocą PolyBase

Podsumowanie procesu ładowania:

1. Narzędzie wiersza polecenia bcp umożliwia eksportowanie danych z programu SQL Server do plików płaskich.
2. Użyj narzędzia wiersza polecenia AZCopy, aby skopiować dane z prostym plików z magazynem obiektów blob platformy Azure.
3. Aby załadować do magazynu danych SQL, należy użyć PolyBase.

Samouczek zobacz [Ładowanie danych z magazynem obiektów blob Azure do magazynu danych SQL (PolyBase)][].

### <a name="use-bcp"></a>Używanie bcp
Jeśli masz niewielkiej ilości danych służy bcp załadować bezpośrednio do magazynu danych SQL Azure.

Podsumowanie procesu ładowania:
1. Narzędzie wiersza polecenia bcp umożliwia eksportowanie danych z programu SQL Server do plików płaskich.
2. Aby załadować dane z prostym plików bezpośrednio do magazynu danych SQL za pomocą bcp.

Samouczek zobacz [Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (bcp)][].


### <a name="use-importexport-recommended-for--10-tb-data"></a>Użyj Importuj/Eksportuj (zalecane dla danych > 10 TB)
Jeśli rozmiar danych jest > 10 TB i chcesz go przenieść Azure, zaleca się użycie naszych dysku wysyłki [Importuj/Eksportuj][]. 

Podsumowanie procesu ładowania
2. Narzędzie wiersza polecenia bcp umożliwia eksportowanie danych z programu SQL Server do prostym pliki na dyskach możliwej.
3. Są dostarczane dysków do firmy Microsoft.
4. Microsoft załadowaniu danych do magazynu danych SQL

## <a name="load-from-hdinsight"></a>Ładowanie z usługi HDInsight
Program SQL Data Warehouse obsługuje ładowania danych z usługi HDInsight za pośrednictwem PolyBase. Proces jest taki sam, jak ładowania danych z magazynem obiektów Blob platformy Azure — nawiązywanie połączenia z usługą hdinsight za, aby załadować dane za pomocą PolyBase. 

### <a name="1-use-polybase-and-t-sql"></a>1. przy użyciu PolyBase i T-SQL

Podsumowanie proces ładowania:

2. Formatowanie danych jako UTF-8, ponieważ PolyBase nie obsługuje obecnie UTF-16.
2. Przenoszenie danych do HDInsight i zapisać go w pliki tekstowe, format ORC lub Parquet.
3. Konfigurowanie obiektów zewnętrznych w magazynie danych SQL, aby zdefiniować lokalizację i format danych.
4. Uruchamianie polecenia T-SQL, aby załadować dane równolegle do nowej tabeli bazy danych.

Samouczek zobacz [Ładowanie danych z magazynem obiektów blob Azure do magazynu danych SQL (PolyBase)][].

## <a name="recommendations"></a>Zalecenia

Wiele partnerów ma ładowania rozwiązań. Aby dowiedzieć się więcej, zobacz listę naszych [partnerów w zakresie rozwiązań][]. 

Jeśli pochodzą dane ze źródła-relacyjnych i chcesz załadować go do magazynu danych SQL należy przekształcić w wierszach i kolumnach przed rozpoczęciem ładowania. Zlogarytmowanych danych nie mają być przechowywane w bazie danych, mogą być przechowywane w plikach tekstowych.

Tworzenie statystyki załadowaniu danych. Magazyn danych SQL Azure czy jeszcze nie pomocy technicznej automatyczne tworzenie lub automatycznej aktualizacji statystyki.  Aby uzyskać najlepszą wydajność z zapytań, ważne jest, aby utworzyć statystyki na wszystkich kolumn wszystkie tabele po załadowaniu pierwszego lub wystąpienia jakichkolwiek znacznych zmian w danych.  Aby uzyskać szczegółowe informacje zobacz [Statystyka][].


## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad rozwoju zobacz [Omówienie tworzenia][].

<!--Image references-->

<!--Article references-->
[Ładowanie danych z magazynem obiektów blob Azure do magazynu danych SQL (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Ładowanie danych z magazynem obiektów blob Azure do magazynu danych SQL (Factory danych Azure)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server to Azure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Ładowanie przykładowe bazy danych]: ./sql-data-warehouse-load-sample-databases.md
[Omówienie migracji]: ./sql-data-warehouse-overview-migrate.md
[rozwiązania partnerów]: ./sql-data-warehouse-partner-business-intelligence.md
[Omówienie tworzenia]: ./sql-data-warehouse-overview-develop.md
[Statystyki]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Importuj/Eksportuj]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
