<properties 
    pageTitle="Przenoszenie danych z Cassandra przy użyciu danych Factory | Microsoft Azure" 
    description="Informacje na temat przenoszenia danych z bazy danych lokalnych Cassandra przy użyciu Factory danych Azure." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Przenoszenie danych z bazy danych lokalnych Cassandra przy użyciu Factory danych Azure
W tym artykule opisano, jak użyć aktywności kopii w factory Azure danych w celu skopiowania danych z Cassandra lokalnej bazy danych do dowolnego magazynu danych na liście w obszarze kolumny Sink w sekcji [obsługiwane źródła i pochłaniacze](data-factory-data-movement-activities.md#supported-data-stores) . W tym artykule opiera się na artykuł [działania przepływu danych](data-factory-data-movement-activities.md) , w którym przedstawiono ogólne omówienie przenoszenia danych z kopii aktywności i danych obsługiwanych kombinacji magazynu.

Dane factory obecnie obsługuje tylko przenoszenie danych z bazy danych Cassandra [obsługiwane sink magazynów](data-factory-data-movement-activities.md#supported-data-stores), ale nie przenoszenie danych z innych baz danych do bazy danych Cassandra.

## <a name="prerequisites"></a>Wymagania wstępne
W usłudze Azure danych Factory mieć możliwość połączenia z bazą danych Cassandra lokalnego należy zainstalować następujące czynności: 

- Brama zarządzania danymi 2.0 lub nowszy na tym samym komputerze obsługującym bazę danych lub na komputerze oddzielnym, aby uniknąć uczestniczących w zawodach dla zasobów w bazie danych. Brama zarządzania danymi to oprogramowanie, które umożliwia nawiązywanie połączeń lokalnych źródeł danych usług w chmurze w sposób bezpieczny i zarządzanie nimi. Zobacz artykuł [Przenoszenie danych między wdrożeniem lokalnym a chmurą](data-factory-move-data-between-onprem-and-cloud.md) szczegółowe informacje na temat bramy zarządzania danymi.
  
    Po zainstalowaniu bramy automatycznie instaluje sterownik Microsoft Cassandra ODBC Cassandra bazy danych. 

> [AZURE.NOTE] Zobacz [bramy Rozwiązywanie problemów](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) , aby uzyskać porady dotyczące rozwiązywania problemów z połączenia/brama problemy związane z. 

## <a name="copy-data-wizard"></a>Kopiowanie Kreator danych
Najprostszym sposobem utworzenia procesu, który kopiuje dane z bazy danych Cassandra do dowolnego z magazynów sink obsługiwane jest za pomocą Kreatora kopiowania danych. Zobacz [Samouczek: tworzenie potok przy użyciu Kreatora kopiowania](data-factory-copy-data-wizard-tutorial.md) aby szybkie informacje na temat tworzenia potok za pomocą Kreatora kopiowania danych. 

Poniższy przykład zawiera definicje JSON, których można utworzyć potok przy użyciu [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) lub [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) lub [Azure programu PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Jak skopiować dane z bazy danych Cassandra magazynem obiektów Blob platformy Azure są pokazywane. Jednak dane można kopiować do jakichkolwiek pochłaniacze podane [tutaj](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory.   


## <a name="sample-copy-data-from-cassandra-to-blob"></a>Przykład: Kopiowanie danych z Cassandra do obiektów Blob
Próbki kopiuje dane z bazy danych Cassandra do obiektów blob platformy Azure co godzinę. Właściwości JSON używane w tych przykładów opisano w sekcjach poniżej próbki. Dane można kopiować bezpośrednio do pochłaniacze wspomniano w artykule [Działania przepływu danych](data-factory-data-movement-activities.md#supported-data-stores) przy użyciu aktywności kopii w Azure danych Factory. 

- Usługi połączone typu [OnPremisesCassandra](#onpremisescassandra-linked-service-properties).
- Usługi połączone typu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Wprowadzania [zestawu danych](data-factory-create-datasets.md) typu [CassandraTable](#cassandratable-properties).
- Dane wyjściowe [zestawu danych](data-factory-create-datasets.md) typu [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Potok](data-factory-create-pipelines.md) z działaniem kopii w korzystającego z [CassandraSource](#cassandrasource-type-properties) i [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

**Usługa Cassandra połączone**

W tym przykładzie jest używana usługa **Cassandra** połączone. Zobacz sekcję [Cassandra połączone usługi](#onpremisescassandra-linked-service-properties) właściwości obsługiwane przez tę usługę połączone.  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
                "gatewayName": "mygateway"
            }
        }
    }


**Azure Usługa magazynu połączone**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
        "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }

**Zestaw Cassandra wprowadzania danych**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Ustawienie **zewnętrznych** na **wartość true** informuje usługę Factory danych, że zestawu danych jest zewnętrznych do fabryki danych i nie jest tworzone przez działania w factory danych.

**Zestaw danych wyjściowych obiektów Blob platformy Azure**

Dane są zapisywane do nowych obiektów blob co godzinę (częstotliwość: godzina, interwał: 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Planowanej aktywnością Kopiuj**

Proces zawiera działaniem Kopiuj jest skonfigurowany do używania wejściowe i wyjściowe zestawy danych, który jest zaplanowane do uruchomienia co godzinę. W potoku definicji JSON typ **źródła** jest ustawiona na **CassandraSource** i typ **sink** jest ustawiona na **BlobSink**. 

Zobacz [RelationalSource typ właściwości](#cassandrasource-type-properties) na liście właściwości obsługiwane przez RelationalSource. 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"
        
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
            ]   
        }
    }
## <a name="onpremisescassandra-linked-service-properties"></a>Właściwości usługi OnPremisesCassandra połączone

Poniższa tabela zawiera opis specyficzne dla usługi Cassandra połączone elementy JSON.

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- | 
| Typ | Ustaw właściwości Typ: **OnPremisesCassandra** | Tak |
| hosta | Jeden lub więcej adresów IP lub host nazwy serwerów Cassandra.<br/><br/>Określ przecinkami listę adresów IP lub nazwy hostów w danej chwili połączenia na wszystkich serwerach. | Tak | 
| Port | Port TCP używany przez serwer Cassandra, aby odsłuchać dla połączeń klienta. | Nie, wartość domyślna: 9042 |
| authenticationType | Podstawowe i anonimowe | Tak |
| Nazwa użytkownika |Określ nazwę użytkownika dla konta użytkownika. | Tak, jeśli authenticationType jest ustawiona na podstawowy. |
| hasło | Określ hasło do konta użytkownika.  | Tak, jeśli authenticationType jest ustawiona na podstawowy. |
| gatewayName | Nazwa bramy, który jest używany do połączenia z bazą danych Cassandra lokalnego. | Tak |
| encryptedCredential | Poświadczenie szyfrowane przez bramę. | Brak | 

## <a name="cassandratable-properties"></a>Właściwości CassandraTable

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](data-factory-create-datasets.md) . Sekcje, takich jak struktury, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych (Azure SQL, obiektów blob platformy Azure, Azure tabeli itp.).

W sekcji **typeProperties** różni się dla każdego typu zestawu danych i zawiera informacje o lokalizacji danych w magazynie danych. W sekcji typeProperties zestawu danych typu **CassandraTable** ma następujące właściwości

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- |
| keyspace | Nazwa schematu w bazie danych Cassandra lub keyspace. | Tak (Jeśli nie zdefiniowano **kwerendy** dla **CassandraSource** ). |
| tableName | Nazwa tabeli w bazie danych Cassandra. | Tak (Jeśli nie zdefiniowano **kwerendy** dla **CassandraSource** ). |


## <a name="cassandrasource-type-properties"></a>Właściwości typu CassandraSource
Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działania zobacz artykuł [Tworzenie procesy](data-factory-create-pipelines.md) . Właściwości, takie jak nazwa, opis, dane wejściowe i wyjściowe tabel i zasady są dostępne dla wszystkich typów działań. 

Właściwości, które są dostępne w sekcji typeProperties działania z drugiej strony zależne od każdego typu działania. Wykonania kopii różnią się w zależności od rodzaju źródeł i ujść.

Źródło jest typu **CassandraSource**, w sekcji typeProperties są dostępne następujące właściwości:

| Właściwość | Opis | Dopuszczalne wartości | Wymagane |
| -------- | ----------- | -------------- | -------- |
| kwerendy | Użyj kwerendy niestandardowej do odczytywania danych. | Kwerenda SQL-92 lub kwerenda CQL. Zobacz [informacje dotyczące CQL](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Gdy używasz zapytania SQL, określ **nazwę name.table keyspace** do przedstawiania tabelę, którą chcesz wykonać kwerendę. | Nie (jeśli są definiowane tableName i keyspace w zestawie danych).  |
| consistencyLevel | Poziom spójności Określa, ile repliki musi odpowiadać na żądanie odczytu przed zwracanie danych do aplikacji klienckiej. Cassandra sprawdza o określoną liczbę replik danych do spełnienia żądania odczytu. | JEDEN, DWA, TRZY, KWORUM, WSZYSTKIE LOCAL_QUORUM EACH_QUORUM, LOCAL_ONE. Aby uzyskać szczegółowe informacje, zobacz [Konfigurowanie spójności danych](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) . | Wartość nie. Wartość domyślna to. |  


### <a name="type-mapping-for-cassandra"></a>Mapowanie typu Cassandra
Typ Cassandra | Typ podstawie .net
--------------- | ---------------
ASCII | Ciąg 
BIGINT | Int64
OBIEKTÓW BLOB | Bajt]
WARTOŚĆ LOGICZNA | Wartość logiczna
DZIESIĘTNA | Dziesiętna
NACIŚNIJ DWUKROTNIE | Naciśnij dwukrotnie
PRZESTAWNE | Pojedynczy
INET | Ciąg
INT | Int32
TEKST | Ciąg
SYGNATURA CZASOWA | Daty i godziny
TIMEUUID | Identyfikator GUID
UUID | Identyfikator GUID
VARCHAR | Ciąg
VARINT | Dziesiętna

> [AZURE.NOTE]  
> Dla zbioru typy (mapa, ustaw, listy, itd.), zobacz sekcję [pracy z typami zbioru Cassandra przy użyciu wirtualnej tabeli](#work-with-collections-using-virtual-table) . 
> 
> Typy zdefiniowane przez użytkownika nie są obsługiwane.
> 
> Długość długości binarne kolumny i ciąg nie może być większa niż 4000 znaków. 

## <a name="work-with-collections-using-virtual-table"></a>Praca z zbiory przy użyciu tabeli wirtualnej
Azure Factory danych używa wbudowanych sterownik ODBC nawiązywanie i skopiuj dane z bazy danych Cassandra. W tym mapy, konfigurowanie i listy typów kolekcji sterownik ponownie normalizuje danych do odpowiednich tabel wirtualną. W szczególności jeśli tabela zawiera kolumny zbioru, sterownik generuje poniższych tabelach wirtualnych:

-   **Tabeli podstawowej**, która zawiera te same dane jako tabelę rzeczywistą z wyjątkiem kolumn kolekcji. Tabeli podstawowej używa tej samej nazwy jako prawdziwego stołu, która go reprezentuje.
-   **Wirtualna tabeli** dla każdej kolumny zbioru, który rozszerza zagnieżdżonych dane. Wirtualna tabel reprezentujących zbiorów są nazywane przy użyciu nazwy prawdziwego stołu, separator "_vt_" i nazwę kolumny.

Wirtualna tabele odnoszą się do danych w tabeli rzeczywistą Włączanie sterownik uzyskiwać dostęp do danych nieznormalizowany. Zobacz sekcję przykład, aby uzyskać szczegółowe informacje. Masz dostęp do zawartości kolekcji Cassandra przez odpytywanie i dołączanie do wirtualnego tabel.

Można korzystać z [Kreatora kopiowania](data-factory-data-movement-activities.md#data-factory-copy-wizard) intuicyjnie listę tabel w bazie danych Cassandra, łącznie z tabelami wirtualną i wyświetlać podgląd danych wewnątrz. Można również skonstruować kwerendę w Kreatorze kopii i sprawdzanie poprawności, aby wyświetlić wynik.

### <a name="example"></a>Przykład
Na przykład następujące "ExampleTable" jest Cassandra tabelę bazy danych, która zawiera liczby całkowitej kolumny klucza podstawowego o nazwie "pk_int", kolumny tekstu o nazwie wartości, kolumny listy, kolumnie Mapa i kolumnę zestawu (o nazwie "StringSet").

pk_int | Wartość | Lista | Mapa |   StringSet
------ | ----- | ---- | --- | --------
1 | "Przykładowa wartość 1" | ["1", "2", "3"]  | {"S1": "," "S2": "b"} | {"A", "B", "C"}
3 | "Przykładowa wartość 3" | ["100", "101", "102", "105"] | {"S1": "t"} | {"A", "E"}

Sterownik powoduje wygenerowanie wielu tabelach wirtualnych reprezentować tej jednej tabeli. Kolumny klucza obcego w tabelach wirtualnych odwołanie kolumny klucza podstawowego w tabeli rzeczywistą i wskazać, który wiersz prawdziwego stołu odpowiada wiersz tabeli wirtualną. 

Pierwsza tabela wirtualną jest podstawowej tabeli o nazwie "ExampleTable" są wyświetlane w poniższej tabeli. Podstawa tabela zawiera te same dane jako w pierwotnej tabeli bazy danych, z wyjątkiem kolekcji, które pominięty z tej tabeli i rozwinięta w innych tabelach wirtualnych.

pk_int | Wartość
------ | -----
1 | "Przykładowa wartość 1"
3 | "Przykładowa wartość 3"

W poniższej tabeli przedstawiono wirtualnych tabel, które renormalize danych z kolumny listy, mapy i StringSet. Kolumny o nazwach, które kończą się z "_index" lub "_key" wskazują położenie danych w oryginalnej listy lub mapy. Kolumny, których nazwy kończą się "_value" zawierają rozszerzone dane z kolekcji.

#### <a name="table-exampletablevtlist"></a>Tabela "ExampleTable_vt_List":
pk_int | List_index | List_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### <a name="table-exampletablevtmap"></a>Tabela "ExampleTable_vt_Map":
pk_int | Map_key | Map_value
------ | ------- | ---------
1 | S1 | A
1 | S2 | b
3 | S1 | t

#### <a name="table-exampletablevtstringset"></a>Tabela "ExampleTable_vt_StringSet":
pk_int | StringSet_value
------ | ---------------
1 | A
1 | B
1 | C
3 | A
3 | E





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Wydajność i dostosowywanie  
Zobacz [wydajności aktywności kopiowania i dostosowywanie przewodnik](data-factory-copy-activity-performance.md) informacje o kluczowych czynników, które wpływ na wydajność przepływu danych (Kopiuj czynność) w Factory danych Azure i optymalizowanie go na różne sposoby.
