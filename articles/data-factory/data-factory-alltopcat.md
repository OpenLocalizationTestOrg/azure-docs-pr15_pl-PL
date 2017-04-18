<properties
    pageTitle="Wszystkich tematów dotyczących usługi Factory danych | Microsoft Azure"
    description="Tabela wszystkich tematów dotyczących usługi Azure o nazwie Factory danych, która istnieje w http://azure.microsoft.com/documentation/articles/, tytuł i opis."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="data-factory"
    ms.workload="data-factory"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="spelluru"/>


# <a name="all-topics-for-azure-data-factory-service"></a>Wszystkich tematów dotyczących usługi Azure danych Factory

W tym temacie opisano każdy temat zastosowany bezpośrednio do usługi **Danych Factory** Azure. Ta strona sieci Web słów kluczowych można wyszukiwać za pomocą **Klawiszy Ctrl + F**, aby znaleźć tematy bieżącej zainteresowania.




## <a name="new"></a>Nowy

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 1 | [Przenoszenie danych z Redshift Amazon za pomocą Factory danych Azure](data-factory-amazon-redshift-connector.md) | Informacje na temat przenoszenia danych z Redshift Amazon za pomocą Azure danych Factory. |
| 2 | [Przenoszenie danych z Amazon prosty przestrzeni dyskowej usługi przy użyciu Factory danych Azure](data-factory-amazon-simple-storage-service-connector.md) | Informacje na temat sposobu przenoszenia danych z usługi Magazyn prosty Amazon (S3) przy użyciu Azure danych Factory. |
| 3 | [Azure danych Factory - kreatora kopiowania](data-factory-azure-copy-wizard.md) | Informacje na temat sposobu używania Kreatora kopiowania Azure Factory danych do skopiowania danych z obsługiwanych źródeł danych do pochłaniacze. |
| 4 | [Samouczek: Tworzenie pierwszej firmie Azure danych za pomocą interfejsu API usługi REST Factory danych](data-factory-build-your-first-pipeline-using-rest-api.md) | W tym samouczku możesz utworzyć potok Azure Factory danych przykładowych za pomocą interfejsu API usługi REST Factory danych. |
| 5 | [Samouczek: Tworzenie potok aktywnością kopii za pomocą interfejsu API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md) | W tym samouczku możesz utworzyć potok Azure Factory danych z działaniem kopii za pomocą interfejsu API .NET. |
| 6 | [Samouczek: Tworzenie potok aktywnością kopii za pomocą interfejsu API usługi REST](data-factory-copy-activity-tutorial-using-rest-api.md) | W tym samouczku możesz utworzyć potok Azure Factory danych z działaniem kopii za pomocą interfejsu API usługi REST. |
| 7 | [Kreator kopii Factory danych](data-factory-copy-wizard.md) | Informacje na temat sposobu używania Kreatora kopiowania Factory danych do skopiowania danych z obsługiwanych źródeł danych do pochłaniacze. |
| 8 | [Brama zarządzania danymi](data-factory-data-management-gateway.md) | Konfigurowanie bramy danymi przenoszenie danych między lokalnym a chmurą. Przenoszenie danych za pomocą bramy zarządzania danymi w Azure danych Factory. |
| 9 | [Przenoszenie danych z bazy danych lokalnych Cassandra przy użyciu Factory danych Azure](data-factory-onprem-cassandra-connector.md) | Informacje na temat przenoszenia danych z bazy danych lokalnych Cassandra przy użyciu Factory danych Azure. |
| 10 | [Przenoszenie danych z MongoDB przy użyciu Factory danych Azure](data-factory-on-premises-mongodb-connector.md) | Informacje na temat przenoszenia danych z MongoDB bazy danych przy użyciu Azure danych Factory. |
| 11 | [Przenoszenie danych z usług Salesforce przy użyciu Factory danych Azure](data-factory-salesforce-connector.md) | Informacje na temat przenoszenia danych z usług Salesforce przy użyciu Azure danych Factory. |


## <a name="updated-articles-data-factory"></a>Aktualizowane artykuły Factory danych

W tej sekcji przedstawiono artykułów, które zostały zaktualizowane w ostatnim tam, gdzie aktualizacji nie duży lub znaczące. Dla każdego artykułu zaktualizowane drukowania fragment tekstu dodano promocji cenowych jest wyświetlany. Artykuły zostały zaktualizowane w zakresie data **2016-08-22** do **2016-10-05**.

| &nbsp; | Artykuł | Zaktualizowany tekst, wstawki kodu | Zaktualizowane |
| --: | :-- | :-- | :-- |
| 12 | [Azure Factory danych — dziennik zmian interfejsu API .NET](data-factory-api-change-log.md) | Ten artykuł zawiera informacje o zmiany do Azure danych Factory SDK w określonej wersji. Znajdują się najnowszy pakiet NuGet Azure danych Factory poniżej (https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) **wersji 4.11.0** funkcji Dodatki: / dodano następujące typy usługi połączone: - OnPremisesMongoDbLinkedService (https://msdn.microsoft.com/library/mt765129.aspx) - AmazonRedshiftLinkedService (https://msdn.microsoft.com/library/mt765121.aspx) - AwsAccessKeyLinkedService (https://msdn.microsoft.com/library/mt765144.aspx) / dodano następujące typy danych: AmazonS3Dataset - MongoDbCollectionDataset (https://msdn.microsoft.com/library/mt765145.aspx) - (https://msdn.microsoft.com/library/mt765112.aspx) / dodano następujących typów źródeł Kopiuj :-MongoDbSource (https://msdn.microsoft.com/en-US/library/mt765123.aspx) **wersji 4.10.0** / następujące właściwości opcjonalne zostały dodane do TextFormat:-nart | 2016-09-22 |
| 13 | [Przenoszenie danych do i z obiektów Blob platformy Azure za pomocą Factory danych Azure](data-factory-azure-blob-connector.md) |  / copyBehavior / pozwala definiować zachowanie kopii, gdy źródłem jest BlobSource lub systemu plików.  - **PreserveHierarchy:** zachowuje hierarchię pliku w folderze docelowym. Względne ścieżki pliku źródłowego do folderu źródła jest identyczny z ścieżkę względną plik docelowy do folderu docelowego. br /... br /. **FlattenHierarchy:** wszystkie pliki z folderu źródłowego są pierwszego poziomu folderu docelowego. Pliki docelowe mieć nazwę generowana automatycznie. .br... br /. **MergeFiles: (ustawienie domyślne)** scala wszystkie pliki z folderu źródłowego do jednego pliku. Jeśli określono nazwy pliku i Blob, nazwy pliku scalonych będzie o określonej nazwie; w przeciwnym razie będzie nazwy pliku generowane automatycznie.  -Nie / **BlobSource** obsługuje także te dwie właściwości zgodności z poprzednimi wersjami. / **treatEmptyAsNull**: Określa, czy traktowania wartości null lub pusty ciąg jako wartość null. / **skipHeaderLineCount** — umożliwia określenie liczby wierszy muszą zostać pominięte. Dotyczy tylko podczas wprowadzania danych korzysta z TextFormat. Podobnie **BlobSink** obsługuje tą | 2016-09-28 |
| 14 | [Tworzenie przewidywanych procesy przy użyciu Azure maszynowego uczenia działań](data-factory-azure-ml-batch-execution-activity.md) | **Usługa sieci Web wymaga wielu wartości wejściowych** Jeśli usługa sieci web ma wiele wartości wejściowych, użyj właściwości **webServiceInputs** zamiast **WebServiceInputActivity**. Zestawy danych, do których odwołuje się **webServiceInputs** muszą także być ujęte w aktywności **danych wejściowych**. W swojej doświadczeniu Azure ML wprowadzania usługi sieci web i porty wyjścia i parametry globalne mają nazw domyślnych ("input1", "input2"), które można dostosować. Nazwy, które dla webServiceInputs, webServiceOutputs i globalParameters ustawienia musi dokładnie odpowiadać imiona i nazwiska w doświadczenia. Na stronie Pomoc wykonanie partii punkt końcowy Azure ML zweryfikować oczekiwanych mapowania, możesz wyświetlić ładunku żądania próbki.    {"Nazwa": "PredictivePipeline", "właściwości": {"opis": "Użyj modelu AzureML", "działania": {"Nazwa": "MLActivity", "typ": "AzureMLBatchExecution", "opis": "Analiza przewidywania partii wprowadzania", "wejść": {"Nazwa": "inputDataset1"}, {"Nazwa": "inputDatase | 2016-09-13 |
| 15 | [Kopiowanie wydajność działania i dostosowywania przewodnika](data-factory-copy-activity-performance.md) | 1. **Ustanów planu bazowego**. Podczas fazy opracowywania przetestować planowaną za pomocą kopiowania aktywności przed próbki przedstawiciela danych. Za pomocą Factory danych krojenie modelu (danych factory planowania i execution.md time-series-datasets-and-data-slices) aby ograniczyć ilość danych, z którymi pracujesz.  Zbieranie czas wykonywania i parametrów przy użyciu **monitorowania i zarządzania aplikacji**. Wybierz **Monitor i Zarządzaj** na stronie głównej Factory danych. W widoku drzewa wybierz **zestaw danych dane wyjściowe**. Na liście **Aktywności Windows** wybierz pozycję aktywności kopii Uruchom. **Windows aktywności** Wyświetla czas trwania działania kopii i rozmiar danych, które są kopiowane. Przepustowość znajduje się w **Eksploratorze okna aktywności**. Aby dowiedzieć się więcej o tej aplikacji, zobacz monitorowanie i zarządzanie procesy Factory danych Azure za pomocą monitorowania i zarządzania aplikacji (danych factory-monitor — Zarządzanie — app.md).  ! Uruchamianie szczegóły (./media/data-factory-copy-activity-performance/mmapp-activity-run-details.pn aktywności | 2016-09-27 |
| 16 | [Samouczek: Tworzenie potok aktywnością kopii przy użyciu programu Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) |   Zwróć uwagę następujące punkty:-zestawu danych **typu** jest ustawiona na **AzureBlob**.     - **linkedServiceName** jest ustawiona na **AzureStorageLinkedService**. Ta usługa połączony został utworzony w kroku 2.     - **ścieżkafolderu** jest ustawiona na kontenerze **adftutorial** . Można również określić nazwy obiektów blob w folderze przy użyciu właściwości **Nazwa pliku** . Ponieważ nie określisz nazwę obiektu blob, dane z wszystkich obiektów blob w kontenerze jest traktowana jako danych wejściowych.    -formatu **Typ** jest ustawiona na **TextFormat** - są dwa pola tekstowe plik ΓÇô **Imię** i **nazwisko** ΓÇô oddzielone znakiem przecinkami (**columnDelimiter**) — **dostępność** jest ustawiona na **co godzinę** (**Częstotliwość** jest ustawiona na **godzinę** i **interwału** jest ustawiona na **1**). Dlatego Factory danych wyszukuje dane wejściowe co godzinę w folderze głównym kontenera obiektów blob (**adftutorial**) określonej.  Jeśli nie określisz **nazwę pliku** dla zestawu danych **wejściowych** , wszystkie pliki i blob z folderu wprowadzania (**ścieżkafolderu**) są consid | 2016-09-29 |
| 17 | [Tworzenie, monitorowanie i zarządzanie fabryki Azure danych przy użyciu zestawu SDK .NET Factory danych](data-factory-create-data-factories-programmatically.md) | Zanotuj identyfikator aplikacji i hasło (tajny klienta) i używać go w Instruktaż. **Uzyskiwanie Azure subskrypcji i dzierżawy identyfikatory** Jeśli nie masz najnowszą wersję programu PowerShell zainstalowane na komputerze, postępuj zgodnie z instrukcjami jak zainstalować i skonfigurować Azure programu PowerShell (.. / programu powershell — Zainstaluj configure.md) artykuł, aby go zainstalować. 1. Uruchom Azure programu PowerShell i wpisz następujące polecenie 2. Uruchom następujące polecenie, a następnie wprowadź nazwę użytkownika i hasło, którego używasz, aby zalogować się do portalu Azure.         AzureRmAccount logowania, jeśli masz tylko jedną subskrypcję Azure skojarzone z tym kontem, nie musisz wykonywać następnych dwóch krokach. 3. Uruchom następujące polecenie, aby wyświetlić wszystkie subskrypcje dla tego konta.       Get-AzureRmSubscription 4. Uruchom następujące polecenie, aby Wybierz subskrypcję, którą chcesz pracować z. Zamień **NameOfAzureSubscription** nazwę subskrypcji Azure.       Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription-AzureRmCo zestawu | 2016-09-14 |
| 18 | [Procesy i działania w Factory Azure danych](data-factory-create-pipelines.md) |       "Uruchom": "2016-07-12T00:00:00Z", "koniec": "2016-07-13T00:00:00Z"}} Zwróć uwagę następujące punkty: / w sekcji działania jest tylko jedno działanie, którego **Typ** jest ustawiona na **kopii**. I wejście działania jest ustawiona na **InputDataset** , a wynik działania jest ustawiona na **OutputDataset**. I w sekcji **typeProperties** **BlobSource** jest określony jako typ źródła i **SqlSink** jest określony jako typ sink. Aby uzyskać pełne instrukcje tworzenia tego procesu, zobacz samouczek: kopiowanie danych z magazynem obiektów Blob do bazy danych SQL (data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). **Przykładowe przekształcenie potoku** W następujących potoku próbki istnieje jedno działanie typu **HDInsightHive** w sekcji **działań** . W tym przykładzie aktywności gałąź HDInsight (danych factory gałęzi activity.md) przy użyciu danych z magazynem obiektów Blob platformy Azure, uruchamiając plik skryptu gałęzi w klastrze Azure HDInsight Hadoop.  {"Nazwa": "TransformPipeline", "p | 2016-09-27 |
| 19 | [Przekształcanie danych w Factory danych Azure](data-factory-data-transformation-activities.md) | Factory danych obsługuje następujące działania przekształcania danych dodane do rurociągi (danych factory — tworzenie pipelines.md) albo pojedynczo lub powiązane z innej działalności. .  AZURE. Uwaga Aby skorzystać z instrukcji krok po kroku, zobacz Tworzenie potok z artykułem przekształcenie (data-factory-build-your-first-pipeline.md) gałęzi. **Gałąź HDInsight aktywności** Działanie gałąź HDInsight w potoku Factory danych wykonuje kwerendy gałęzi samodzielnie lub klaster HDInsight systemu Windows i Linux oraz na żądanie. Zobacz artykuł (danych factory gałęzi activity.md) aktywności gałęzi szczegółowe informacje na temat tego działania. **Świnka HDInsight aktywności** Działanie świnka HDInsight w potoku Factory danych wykonuje kwerendy świnka samodzielnie lub klaster HDInsight systemu Windows i Linux oraz na żądanie. Zobacz artykuł (danych factory świnka activity.md) świnka aktywności szczegółowe informacje na temat tego działania. **Usługa HDInsight MapReduce aktywności** Działanie HDInsight MapReduce w potoku Factory danych wykonuje programy MapReduce na y | 2016-09-26 |
| 20 | [Planowanie Factory danych i wykonanie](data-factory-scheduling-and-execution.md) | CopyActivity2 można uruchomić tylko wtedy, gdy pomyślnym uruchomieniu CopyActivity1 i Dataset2 jest dostępna. Oto przykładowy planowana JSON: {"Nazwa": "ChainActivities", "właściwości": {"opis": "Uruchom działania w sekwencji", "działania": {"typ": "Kopiuj", "typeProperties": {"źródłowy": {"typ": "BlobSource"}, "sink": {"typ": "BlobSink", "copyBehavior": "PreserveHierarchy", "writeBatchSize": 0; "writeBatchTimeout": "00: 00:00"}}, "wartości wejściowych": {"Nazwa": "Dataset1"}, "Wyświetla": {"Nazwa": "Dataset2"}, "zasady": {"Przekroczono limit czasu": "01: 00:00"}, "harmonogram": {"częstotliwość": "Godz.", "zakres": 1}, "Nazwa": "CopyFromBlob1ToBlob2", "opis": "Kopiowanie danych z obiektów blob do innego"}, {"typ": "Kopiuj", "typeProperties": {"źródłowy": {"typ": "BlobSource"}, "sink" : {"typ": "BlobSink", "writeBatchSize": 0; "writeBatchTimeout": "00: 00:00"}}, "w | 2016-09-28 |





## <a name="tutorials"></a>Samouczki

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 21 | [Samouczek: Tworzenie pierwszej planowaną przetworzyć danych przy użyciu klaster Hadoop](data-factory-build-your-first-pipeline.md) | Ten samouczek Factory danych Azure pokazano, jak tworzyć i planowanie factory danych, który przetwarza dane za pomocą skryptu gałęzi w klastrze Hadoop. |
| 22 | [Samouczek: Tworzenie pierwszej firmie Azure danych przy użyciu szablonu Azure Menedżera zasobów](data-factory-build-your-first-pipeline-using-arm.md) | W tym samouczku możesz utworzyć potok Azure Factory danych przykładowych przy użyciu szablonu Azure Menedżera zasobów. |
| 23 | [Samouczek: Tworzenie pierwszej firmie Azure danych za pomocą portalu Azure](data-factory-build-your-first-pipeline-using-editor.md) | W tym samouczku możesz utworzyć potok Azure Factory danych przykładowych przy użyciu edytora Factory danych w portalu Azure. |
| 24 | [Samouczek: Tworzenie pierwszej firmie Azure danych przy użyciu programu PowerShell Azure](data-factory-build-your-first-pipeline-using-powershell.md) | W tym samouczku możesz utworzyć potok Azure Factory danych przykładowych przy użyciu programu PowerShell Azure. |
| 25 | [Samouczek: Tworzenie imienia Azure danych factory przy użyciu programu Microsoft Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) | W tym samouczku możesz utworzyć potok Azure Factory danych przykładowych przy użyciu programu Visual Studio. |
| 26 | [Samouczek: Tworzenie potok aktywnością kopiowanie za pomocą portalu Azure](data-factory-copy-activity-tutorial-using-azure-portal.md) | W tym samouczku możesz utworzyć potok Factory danych Azure z działaniem kopii przy użyciu edytora Factory danych w portalu Azure. |
| 27 | [Samouczek: Tworzenie potok aktywnością kopii przy użyciu programu PowerShell Azure](data-factory-copy-activity-tutorial-using-powershell.md) | W tym samouczku możesz utworzyć potok Factory danych Azure z działaniem kopii przy użyciu programu PowerShell Azure. |
| 28 | [Samouczek: Tworzenie potok aktywnością kopii przy użyciu programu Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) | W tym samouczku możesz utworzyć potok Factory danych Azure z działaniem kopii przy użyciu programu Visual Studio. |
| 29 | [Skopiuj dane z magazynem obiektów Blob SQL bazy danych przy użyciu danych Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | Ten samouczek pokazano, jak za pomocą kopiowania aktywności w potoku Factory danych Azure skopiować dane z magazynem obiektów Blob do bazy danych SQL. |
| 30 | [Samouczek: Tworzenie potok aktywnością kopii za pomocą Kreatora kopiowania Factory danych](data-factory-copy-data-wizard-tutorial.md) | W tym samouczku tworzenia potoku Azure Factory danych z działaniem kopii za pomocą Kreatora kopiowania obsługiwanych przez Factory danych |



## <a name="data-movement"></a>Przenoszenie danych

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 31 | [Przenoszenie danych do i z obiektów Blob platformy Azure za pomocą Factory danych Azure](data-factory-azure-blob-connector.md) | Dowiedz się, jak skopiuj dane obiektów blob platformy Azure Factory danych. Za pomocą próby: jak skopiować dane do i z magazynem obiektów Blob platformy Azure i bazy danych SQL Azure. |
| 32 | [Przenoszenie danych do i z magazynu Lake danych Azure za pomocą Factory danych Azure](data-factory-azure-datalake-connector.md) | Dowiedz się, jak przenieść dane z magazynu Lake danych Azure za pomocą Factory danych Azure |
| 33 | [Przenoszenie danych do i z DocumentDB przy użyciu Factory danych Azure](data-factory-azure-documentdb-connector.md) | Dowiedz się, jak przenieść dane do/z kolekcji Azure DocumentDB za pomocą Factory danych Azure |
| 34 | [Przenoszenie danych do i z bazy danych SQL Azure za pomocą Factory danych Azure](data-factory-azure-sql-connector.md) | Dowiedz się, jak przenieść dane z bazy danych SQL Azure za pomocą Azure danych Factory. |
| 35 | [Przenoszenie danych do i z magazynu danych SQL Azure za pomocą Factory danych Azure](data-factory-azure-sql-data-warehouse-connector.md) | Dowiedz się, jak przenieść dane z magazynu danych SQL Azure za pomocą Factory danych Azure |
| 36 | [Przenoszenie danych do i z tabeli Azure za pomocą Factory danych Azure](data-factory-azure-table-connector.md) | Dowiedz się, jak przenieść dane z magazynie tabel platformy Azure za pomocą Factory danych Azure. |
| 37 | [Kopiowanie wydajność działania i dostosowywania przewodnika](data-factory-copy-activity-performance.md) | Informacje na temat kluczy czynniki, które wpływ na wydajność przepływu danych w Factory danych Azure, korzystając z kopii aktywności. |
| 38 | [Przenoszenie danych za pomocą polecenia Kopiuj aktywności](data-factory-data-movement-activities.md) | Więcej informacji na temat przenoszenia danych w procesy Factory danych: migracji danych między sklepy chmurze oraz między magazynu lokalnego i magazynu w chmurze. Za pomocą kopiowania aktywności. |
| 39 | [Informacje o wersji dla bramy zarządzania danymi](data-factory-gateway-release-notes.md) | Informacje o wersji tory bramy zarządzania danymi |
| 40 | [Przenoszenie danych z HDFS lokalnego przy użyciu Factory danych Azure](data-factory-hdfs-connector.md) | Informacje na temat przenoszenia danych z HDFS lokalnego przy użyciu Azure danych Factory. |
| 41 | [Monitorowanie i zarządzanie nimi procesy Factory danych Azure za pomocą nowych monitorowania i zarządzania aplikacji](data-factory-monitor-manage-app.md) | Dowiedz się, jak używać monitorowania i zarządzania aplikacji do monitorowania i zarządzania danymi Azure fabryki i procesy. |
| 42 | [Przenoszenie danych między lokalnych źródeł i chmura z bramy zarządzania danymi](data-factory-move-data-between-onprem-and-cloud.md) | Konfigurowanie bramy danymi przenoszenie danych między lokalnym a chmurą. Przenoszenie danych za pomocą bramy zarządzania danymi w Azure danych Factory. |
| 43 | [Przenoszenie danych z OData źródła przy użyciu Factory danych Azure](data-factory-odata-connector.md) | Informacje na temat przenieść dane ze źródeł OData przy użyciu Azure danych Factory. |
| 44 | [Przenoszenie danych ODBC z magazynów przy użyciu Factory danych Azure](data-factory-odbc-connector.md) | Informacje na temat przenieść dane z baz danych ODBC za pomocą Azure danych Factory. |
| 45 | [Przenoszenie danych z DB2 przy użyciu Factory danych Azure](data-factory-onprem-db2-connector.md) | Dowiedz się, jak przenieść dane z DB2 bazy danych przy użyciu Factory danych Azure |
| 46 | [Przenoszenie danych do i z lokalnym systemem plików przy użyciu Factory danych Azure](data-factory-onprem-file-system-connector.md) | Dowiedz się, jak przenieść dane do i z lokalnym systemem plików przy użyciu Azure danych Factory. |
| 47 | [Przenoszenie danych z MySQL przy użyciu Factory danych Azure](data-factory-onprem-mysql-connector.md) | Informacje na temat Przenoszenie danych z bazy danych MySQL przy użyciu Azure danych Factory. |
| 48 | [Przenoszenie danych z Oracle lokalnego przy użyciu Factory danych Azure](data-factory-onprem-oracle-connector.md) | Dowiedz się, jak przenieść dane z bazy danych programu Oracle, która jest lokalnego przy użyciu Factory danych Azure. |
| 49 | [Przenoszenie danych z przy użyciu Azure Factory danych PostgreSQL](data-factory-onprem-postgresql-connector.md) | Informacje na temat Przenoszenie danych z bazy danych PostgreSQL za pomocą Azure danych Factory. |
| 50 | [Przenoszenie danych z programu Sybase przy użyciu Factory danych Azure](data-factory-onprem-sybase-connector.md) | Informacje na temat przenoszenia danych z bazy danych programu Sybase przy użyciu Azure danych Factory. |
| 51 | [Przenoszenie danych z przy użyciu Azure Factory danych Teradata](data-factory-onprem-teradata-connector.md) | Dowiedz się więcej o Teradata łącznik usługi Factory danych, który umożliwia przenoszenie danych z bazy danych programu Teradata |
| 52 | [Przenoszenie danych do i z programu SQL Server lokalnego lub IaaS (maszyn wirtualnych Azure) przy użyciu Factory danych Azure](data-factory-sqlserver-connector.md) | Informacje na temat Przenoszenie danych z bazy danych programu SQL Server, z lokalną lub maszyn wirtualnych Azure za pomocą Azure danych Factory. |
| 53 | [Przenoszenie danych z tabeli źródło w sieci Web przy użyciu Factory danych Azure](data-factory-web-table-connector.md) | Informacje na temat przenoszenia danych z lokalnego tabeli na stronie sieci Web przy użyciu Azure danych Factory. |



## <a name="data-transformation"></a>Przekształcenie danych

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 54 | [Tworzenie przewidywanych procesy przy użyciu Azure maszynowego uczenia działań](data-factory-azure-ml-batch-execution-activity.md) | W tym artykule opisano sposób tworzenia tworzenie przewidywanych procesy przy użyciu Factory danych Azure i nauka maszynowego Azure |
| 55 | [Obliczanie usługi połączone](data-factory-compute-linked-services.md) | Informacje o enviornments obliczeń, w której można w procesy Azure Factory danych do procesu przekształcania danych. |
| 56 | [Przetwarzanie dużych zestawów danych przy użyciu Factory danych i partii](data-factory-data-processing-using-batch.md) | W tym artykule opisano sposób przetwarzania dużej ilości danych w potoku Azure Factory danych przy użyciu funkcji przetwarzanie równoległe partii Azure. |
| 57 | [Przekształcanie danych w Factory danych Azure](data-factory-data-transformation-activities.md) | Dowiedz się, jak Przekształcanie lub proces danych w Azure Factory danych przy użyciu Hadoop, nauki komputera lub Azure danych Lake analizy. |
| 58 | [Hadoop strumienia aktywności](data-factory-hadoop-streaming-activity.md) | Dowiedz się, jak za pomocą aktywności Streaming Hadoop w factory Azure danych do uruchamiania programów Hadoop Streaming na na żądanie/swój własny klaster HDInsight. |
| 59 | [Gałąź aktywności](data-factory-hive-activity.md) | Dowiedz się, jak za pomocą aktywności gałęzi w factory Azure danych do uruchomienia kwerendy gałęzi na żądanie/swój własny klaster HDInsight. |
| 60 | [Wywołaj programy MapReduce z fabryki danych](data-factory-map-reduce.md) | Dowiedz się, jak przetwarzanie danych, uruchamiając programy MapReduce w klastrze Azure HDInsight z fabryki Azure danych. |
| 61 | [Świnka aktywności](data-factory-pig-activity.md) | Dowiedz się, jak za pomocą aktywności świnka w factory Azure danych na uruchamianie skryptów świnka na żądanie/swój własny klaster HDInsight. |
| 62 | [Wywołaj programy Spark z fabryki danych](data-factory-spark.md) | Dowiedz się, jak wywołać programy Spark z fabryki Azure danych przy użyciu aktywności MapReduce. |
| 63 | [Działania procedury przechowywane programu SQL Server](data-factory-stored-proc-activity.md) | Dowiedz się, jak za pomocą aktywności przechowywane procedury programu SQL Server do wywołania procedura składowana bazy danych SQL Azure lub magazynu danych SQL Azure z poziomu procesu Factory danych. |
| 64 | [Używanie niestandardowe działania w potoku Factory danych Azure](data-factory-use-custom-activities.md) | Dowiedz się, jak tworzyć niestandardowe działania i używać ich w potoku Azure danych Factory. |
| 65 | [Uruchamianie skryptu U SQL podczas analizy Lake danych Azure z fabryki danych Azure](data-factory-usql-activity.md) | Dowiedz się, jak przetwarzanie danych, uruchamiając skrypty U SQL Azure danych Lake analizy do uruchamiania usługi. |



## <a name="samples"></a>Przykłady

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 66 | [Azure danych Factory - próbki](data-factory-samples.md) | Szczegółowe informacje na temat próbki dostarczane z usługą Azure danych Factory. |



## <a name="use-cases"></a>Przypadków użycia

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 67 | [Analizy przypadków](data-factory-customer-case-studies.md) | Dowiedz się, jak niektóre z klientami był używany Azure danych Factory. |
| 68 | [Używanie liter - profilowania odbiorców](data-factory-customer-profiling-usecase.md) | Dowiedz się, jak Azure Factory danych służy do tworzenia opartych na danych przepływu pracy (planowana) do profilu gier klientów. |
| 69 | [Używanie liter — zalecenia produktu](data-factory-product-reco-usecase.md) | Informacje na temat przypadków użycia wykonywane przy użyciu Factory danych Azure wraz z innymi usługami. |



## <a name="monitor-and-manage"></a>Monitorowanie i zarządzanie nimi

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 70 | [Monitorowanie i zarządzanie nimi procesy Factory danych Azure](data-factory-monitor-manage-pipelines.md) | Dowiedz się, jak można monitorować i zarządzać nimi fabryki Azure danych, procesy, które zostały utworzone przy użyciu Azure Portal i Azure programu PowerShell. |



## <a name="sdk"></a>ZESTAW SDK

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 71 | [Azure Factory danych — dziennik zmian interfejsu API .NET](data-factory-api-change-log.md) | W tym artykule opisano najnowsze zmiany, funkcja dodatki, poprawki itp. w określonej wersji programu .NET interfejs API umożliwiający Factory danych Azure. |
| 72 | [Tworzenie, monitorowanie i zarządzanie fabryki Azure danych przy użyciu zestawu SDK .NET Factory danych](data-factory-create-data-factories-programmatically.md) | Dowiedz się, jak programowo tworzenie, monitorowanie i zarządzanie fabryki Azure danych przy użyciu zestawu SDK Factory danych. |
| 73 | [Dokumentacja dewelopera Factory Azure danych](data-factory-sdks.md) | Więcej informacji na temat sposobów tworzenia, monitorowanie i zarządzanie fabryki Azure danych |



## <a name="miscellaneous"></a>Różne

| &nbsp; | Tytuł | Opis |
| --: | :-- | :-- |
| 74 | [Factory Azure danych — często zadawane pytania](data-factory-faq.md) | Często zadawane pytania dotyczące Azure danych Factory. |
| 75 | [Azure danych Factory - funkcje i zmienne systemowe](data-factory-functions-variables.md) | Lista funkcji Factory danych Azure i zmienne systemowe |
| 76 | [Azure danych Factory - nazewnictwa reguł](data-factory-naming-rules.md) | W tym artykule opisano reguł nazewnictwa dla obiektów Factory danych. |
| 77 | [Rozwiązywanie problemów Factory danych](data-factory-troubleshoot.md) | Dowiedz się, jak rozwiązywać problemy z używaniem Azure danych Factory. |

