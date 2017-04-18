<properties 
    pageTitle="Factory danych — dziennik zmian interfejsu API .NET | Microsoft Azure" 
    description="W tym artykule opisano najnowsze zmiany, funkcja dodatki, poprawki itp. w określonej wersji programu .NET interfejs API umożliwiający Factory danych Azure." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="spelluru"/>

# <a name="azure-data-factory---net-api-change-log"></a>Azure Factory danych — dziennik zmian interfejsu API .NET 
Ten artykuł zawiera informacje o zmiany do Azure danych Factory SDK w określonej wersji. Można znaleźć najnowszy pakiet NuGet Factory danych Azure [tutaj](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## <a name="version-4110"></a>Wersja 4.11.0
Funkcja dodatki:

- Dodano powiązanych z następujących typów:
    - [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
    - [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
    - [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- Dodano następujące typy danych: 
    - [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
    - [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- Dodano następujących typów źródeł kopii:
    - [MongoDbSource](https://msdn.microsoft.com/en-US/library/mt765123.aspx)

## <a name="version-4100"></a>Wersja 4.10.0
- Następujące właściwości opcjonalne zostały dodane do TextFormat:
    - [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
    - [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
    - [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- Dodano powiązanych z następujących typów:
    - [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
    - [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- Dodano następujące typy danych:
    - [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- Dodano następujących typów źródeł kopii:
    - [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- Dodaj właściwość [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) do AzureMLBatchExecutionActivity 
    - Włącz przekazywanie wielu sieci web usługi wejść do doświadczenia Azure maszynowego uczenia


## <a name="version-491"></a>Wersja 4.9.1

### <a name="bug-fix"></a>Poprawka błędu

- Oznaczanie jako przestarzałego uwierzytelniania opartego na WebApi dla [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Wersja 4.9.0

### <a name="feature-additions"></a>Funkcja dodatki

- Dodawanie właściwości [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) i [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) do CopyActivity. Aby uzyskać szczegółowe informacje na temat funkcji, zobacz [etapowa kopia](data-factory-copy-activity-performance.md#staged-copy) .


### <a name="bug-fix"></a>Poprawka błędu

- Przedstaw przeciążeń [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) metodę, która ma wystąpienie [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) .
- Oznaczanie [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) i [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) jako opcjonalnej CopySink.

## <a name="version-480"></a>Wersja 4.8.0

### <a name="feature-additions"></a>Funkcja dodatki
- Następujące właściwości opcjonalne zostały dodane do typu działania kopii umożliwiające dostosowywanie wydajności kopiowania:
    - [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
    - [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Wersja 4.7.0

### <a name="feature-additions"></a>Funkcja dodatki
* Dodany nowy typ StorageFormat typu [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) , aby skopiować pliki w wierszu zoptymalizowanej tabelarycznego (ORC).
* Dodawanie właściwości [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) i PolyBaseSettings do SqlDWSink.
    * Umożliwia korzystanie z PolyBase, aby skopiować dane do magazynu danych SQL.

## <a name="version-461"></a>Wersja 4.6.1

### <a name="bug-fixes"></a>Poprawki
* Rozwiązuje żądania HTTP listą działania systemu windows.
    * Usuwa nazwy grupy zasobów i nazwy factory danych ładunku wezwanie.

## <a name="version-460"></a>Wersja 4.6.0

### <a name="feature-additions"></a>Funkcja dodatki

- Następujące właściwości zostały dodane do [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx):
    - [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
    - [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
    - [Zestawy danych](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- Następujące właściwości zostały dodane do [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx):
    - [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- Dodano nowe [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) typu [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) Aby zdefiniować zestawy danych, których dane są zapisane w formacie JSON. 

## <a name="version-450"></a>Wersja 4.5.0

### <a name="feature-additions"></a>Funkcja dodatki
* Dodano [operacji na liście w oknie aktywności](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
    * Dodano metody do pobierania windows aktywności filtrami na podstawie typów jednostki (czyli fabryki danych, zestawy danych, procesy i działania).
* Dodano powiązanych z następujących typów: 
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Dodano następujące typy danych: 
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Dodano następujących typów źródeł kopii:  
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Wersja 4.4.0

### <a name="feature-additions"></a>Funkcja dodatki

- Następującego typu powiązanych z zostało dodane jako źródła danych i pochłaniacze kopii działań:
    - [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Zobacz [Usługa połączonych Azure magazynu skojarzeń zabezpieczeń](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) dla informacji koncepcyjnych i przykłady. 

## <a name="version-430"></a>Wersja 4.3.0

### <a name="feature-additions"></a>Funkcja dodatki

- Następujące był typy powiązanych z zostały dodane jako źródła danych dla kopii działania:
    - [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Ogólne informacje i przykłady, zobacz [Przenoszenie danych z plików HDFS przy użyciu danych Factory](data-factory-hdfs-connector.md) . 
    - [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Ogólne informacje i przykłady, zobacz [Przenoszenie z ODBC dane są przechowywane przy użyciu Azure danych Factory](data-factory-odbc-connector.md) . 

## <a name="version-420"></a>Wersja 4.2.0

### <a name="feature-additions"></a>Funkcja dodatki

- Dodano następujące nowego typu działania: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Aby uzyskać szczegółowe informacje o działaniu zobacz [modeli aktualizowanie ML Azure za pomocą działań zasobu aktualizacji](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity).
- Nowe opcjonalne właściwości [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) został dodany do [klasy AzureMLLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx). 
- Właściwości [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) i [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) zostały dodane do klasy [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) . 
- Zezwalaj konfiguracji limity czasu dla połączeń klientów z usługą Factory danych. 


## <a name="version-410"></a>Wersja 4.1.0

### <a name="feature-additions"></a>Funkcja dodatki
* Dodano powiązanych z następujących typów: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Dodano następujące typy działań: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Dodano następujące typy danych: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Dodano następujących typów źródeł i sink wykonania kopii:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## <a name="version-401"></a>Wersja 4.0.1

### <a name="breaking-changes"></a>Przerywanie zmian
Zmieniono następujące klasy. Nowe nazwy sprzed oryginalnych nazw klas 4.0.0 Zwolnij. 
 
Imię i nazwisko 4.0.0 | Imię i nazwisko 4.0.1
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## <a name="version-400"></a>Wersja 4.0.0

### <a name="breaking-changes"></a>Przerywanie zmian



- Zmieniono klas i interfejsów.

| Stara nazwa | Nowa nazwa |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Tabela | [Zestaw danych](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- Metody **listy** zwracać stronicowania wyniki teraz. Jeśli odpowiedź zawiera właściwość **NextLink** niepustych, z aplikacją kliencką należy kontynuować pobieranie Następna strona, aż wszystkie strony są zwracane.  Oto przykład: 

        PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
        var pipelines = new List<Pipeline>(response.Pipelines);
    
        string nextLink = response.NextLink;
        while (string.IsNullOrEmpty(response.NextLink))
        {
            PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
            pipelines.AddRange(nextResponse.Pipelines);
    
            nextLink = nextResponse.NextLink;
        }
    
- **Lista** planowana interfejsu API zwraca tylko podsumowanie potok zamiast pełne szczegóły. Na przykład działania w podsumowaniu planowana zawierać tylko nazwa i typ.

### <a name="feature-additions"></a>Funkcja dodatki
- Klasa [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) obsługuje dwie nowe właściwości **SliceIdentifierColumnName** i **SqlWriterCleanupScript**w celu obsługi kopiowania idempotent do magazynu danych SQL Azure. Zapoznaj się z artykułem [Magazynu danych SQL Azure](data-factory-azure-sql-data-warehouse-connector.md) w szczególności [mechanizmu 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) i [2 mechanizmu](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) sekcji, aby uzyskać szczegółowe informacje o tych właściwości.

- Teraz obsługiwać jest uruchomiony procedura składowana bazy danych SQL Azure i magazynu danych SQL Azure źródłach jako część działania Kopiuj. Klasy [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) i [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) mają następujące właściwości: **SqlReaderStoredProcedureName** i **StoredProcedureParameters**. Zobacz artykuły [Bazy danych SQL Azure](data-factory-azure-sql-connector.md#sqlsource) i [Magazynu danych SQL Azure](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) w Azure.com szczegółowe informacje na temat tych właściwości.  