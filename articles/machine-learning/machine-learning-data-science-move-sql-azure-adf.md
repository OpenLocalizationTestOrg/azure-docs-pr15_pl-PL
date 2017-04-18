<properties
    pageTitle="Przenoszenie danych z programu SQL Server w wersji lokalnej do platformy SQL Azure z Factory danych Azure | Azure"
    description="Konfigurowanie potoku ADF, który składa dwa działania migracji danych, które razem przenoszenie danych między bazami danych w wersji lokalnej i w chmurze codziennie."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


# <a name="move-data-from-an-on-premise-sql-server-to-sql-azure-with-azure-data-factory"></a>Przenoszenie danych z programu SQL server w wersji lokalnej do platformy SQL Azure z Factory danych Azure

W tym temacie przedstawiono sposób przenieść dane z bazy danych SQL w wersji lokalnej bazy danych SQL Azure za pośrednictwem magazyn obiektów Blob platformy Azure za pomocą Factory danych Azure (ADF).

Poniższe łącza **menu** do tematów z opisem mogły zjeść tej ostatniej danych w środowiskach docelowej miejsce, w którym dane można przechowywane i przetwarzane podczas nauki danych zespołu.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="intro"></a>Wprowadzenie: Co to jest ADF i kiedy je należy przeprowadzić migrację danych?

Azure Factory danych to usługa integracji danych w pełni zarządzanych opartej na chmurze, która orchestrates i zautomatyzować poruszania się i przekształcania danych. Kluczowych zasad w modelu ADF jest planowana. Potok jest logiczna grupa działań, z których każdy określa akcje do wykonania na danych znajdujących się w zestawy danych. Usługi połączone są używane do definiowania informacji potrzebnych do fabryki danych, aby połączyć się z zasobami danych.

Z ADF istniejącymi usługami przetwarzania danych mogą się składać na części danych, które są bardzo dostępne i zarządzanie nimi w chmurze. Te procesy danych można zaplanować mogły zjeść tej ostatniej, przygotowywanie przekształcenia, analizowanie i publikowanie danych i ADF zarządza i orchestrates złożonych danych i zależności przetwarzania. Rozwiązania można szybko wbudowane i wdrożony w chmurze, łączenie rosnącej liczby lokalnego i źródeł danych w chmurze.

Należy rozważyć użycie ADF:

- gdy dane trzeba zmigrować ciągłe w scenariuszu hybrydowych którego uzyskuje dostęp do zarówno w wersji lokalnej i chmura zasobów 
- gdy dane jest wykonany lub musi można modyfikować lub zostały dodane do niego po migrowane reguł biznesowych. 

ADF umożliwia planowanie i monitorowanie zadań przy użyciu prostej skryptów JSON, które zarządzają przepływu danych w regularnych odstępach czasu. ADF zawiera również inne funkcje, takie jak obsługa złożonych operacji. Aby uzyskać więcej informacji o ADF zobacz dokumentację w [Azure Factory danych (ADF)](https://azure.microsoft.com/services/data-factory/).


## <a name="scenario"></a>Scenariusz

Firma Microsoft Skonfiguruj potok ADF, który składa dwa działania migracji danych. Wspólna ich przenoszenie danych codziennie między bazy danych SQL w wersji lokalnej i bazy danych SQL Azure w chmurze. Są dwa działania:

* Kopiowanie danych z bazy danych programu SQL Server w wersji lokalnej do konta usługi Magazyn obiektów Blob platformy Azure
* Skopiuj dane z magazynem obiektów Blob platformy Azure konta z bazą danych SQL Azure.

>[AZURE.NOTE] Procedura pokazane poniżej zostały dostosowane z bardziej szczegółowe samouczka dostarczonymi przez zespół ADF: [Przenoszenie danych między lokalnych źródeł i chmura z bramy zarządzania danymi](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) odwołania w odpowiednich sekcjach ten temat znajdują się w stosownych przypadkach.


## <a name="prereqs"></a>Wymagania wstępne
Tego samouczka przyjęto założenie, że masz:

* **Azure subskrypcji**. Jeśli nie masz subskrypcji, można Załóż [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
* **Konto Azure miejsca do magazynowania**. Korzystasz z konta Azure miejsca do magazynowania do przechowywania danych w tym samouczku. Jeśli nie masz konta usługi Azure miejsca do magazynowania, zobacz artykuł [Tworzenie konta miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account) . Po utworzeniu konta miejsca do magazynowania, musisz uzyskać klucz konta używanego do uzyskiwania dostępu do przechowywania. Zobacz [Wyświetlanie, Kopiuj i klawisze dostępu wyniku miejsca do magazynowania](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Dostęp do **bazy danych Azure SQL**. Jeśli musisz skonfigurować bazy danych SQL Azure, tpoic [Wprowadzenie do programu Microsoft Azure SQL Database](../sql-database/sql-database-get-started.md) zawiera informacje na temat obsługi administracyjnej nowego wystąpienia bazy danych SQL Azure.
* Zainstalowane i skonfigurowane **Azure programu PowerShell** lokalnie. Aby uzyskać instrukcje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

> [AZURE.NOTE] Ta procedura używa [Azure portal](https://portal.azure.com/).


##<a name="upload-data"></a>Przekazywanie danych do programu SQL Server w wersji lokalnej

Firma Microsoft korzysta z [taksówki Warszawa zestawu danych](http://chriswhong.com/open-data/foil_nyc_taxi/) w celu zademonstrowania procesu migracji. Taksówki Warszawa zestawu danych jest dostępna, które zostały wymienione w tym wpisie Azure blob miejsca do magazynowania [Danych taksówki Warszawa](http://www.andresmh.com/nyctaxitrips/). Dane mają dwa pliki, plik trip_data.csv, który zawiera szczegóły dotyczące podróży, i plik trip_far.csv, który zawiera szczegóły opłat spłacona w odniesieniu do każdej podróży. Próbki i opis te pliki znajdują się w [Warszawa taksówki podróży zestawu danych w polu Opis](machine-learning-data-science-process-sql-walkthrough.md#dataset).


Możesz dostosować procedury przedstawione zbiór danych użytkownika lub zgodnie z opisem przy użyciu zestawu danych taksówki Warszawa, postępuj zgodnie z instrukcjami. Aby przekazać taksówki Warszawa zestawu danych do bazy danych programu SQL Server w wersji lokalnej, wykonaj procedurę opisaną w [Zbiorcze importowanie danych do bazy danych programu SQL Server](machine-learning-data-science-process-sql-walkthrough.md#dbload). Poniższe instrukcje dotyczą programu SQL Server na maszyn wirtualnych Azure, ale jest taka sama procedury dotyczące przekazywania do lokalnego programu SQL Server.


##<a name="create-adf"></a>Tworzenie Factory Azure danych

Instrukcje dotyczące tworzenia nowych Factory danych Azure i grupa zasobów w [Azure portal](https://portal.azure.com/) znajdują się [Tworzenie Factory danych Azure](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#step-1-creating-the-data-factory). Nazwa nowego wystąpienia ADF *adfdsp* i nazwa utworzona grupa zasobów *adfdsprg*.


## <a name="install-and-configure-up-the-data-management-gateway"></a>Instalowanie i konfigurowanie bramy zarządzania danymi w górę

Aby włączyć usługi procesy w fabrycznych Azure danych do pracy z programem SQL Server w wersji lokalnej, musisz dodać go jako usługi połączone do fabryki danych. Aby utworzyć powiązanych z lokalnego serwera SQL Server, należy:

- Pobierz i zainstaluj bramy zarządzania danymi firmy Microsoft do komputera lokalnego. 
- Skonfiguruj usługę połączonych dla lokalnego źródła danych, aby używać bramy. 

Brama zarządzania danymi serializes i deserializes danych źródłowych i Zlew na komputerze, w której jest ona hostowana.

Aby uzyskać instrukcje dotyczące konfiguracji i szczegółowych informacji na temat bramy zarządzania danymi zobacz [Przenoszenie danych między lokalnych źródeł i chmura z bramy zarządzania danymi](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)


## <a name="adflinkedservices"></a>Tworzenie połączonych usług do łączenia się z zasobami danych

Usługi połączone określa informacje potrzebne do Azure fabryki danych, aby połączyć się z zasobem danych. Procedura krok po kroku dotyczące tworzenia usługi połączone jest dostępna w [Tworzenie połączonych usług](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-2-create-linked-services).

Firma Microsoft udostępnia zasoby trzy w tym scenariuszu, dla którego połączone usługi są potrzebne.

1. [Usługi połączone dla lokalnego programu SQL Server](#adf-linked-service-onprem-sql)
2. [Połączone usługi Magazyn obiektów Blob platformy Azure](#adf-linked-service-blob-store)
3. [Powiązanych z bazy danych Azure SQL](#adf-linked-service-azure-sql)


###<a name="adf-linked-service-onprem-sql"></a>Powiązanych z bazy danych programu SQL Server w wersji lokalnej

Aby utworzyć powiązanych z lokalnego serwera SQL:

- Kliknij pozycję **Magazyn danych** w strona początkowa ADF na klasyczny Portal Azure 
- Wybierz pozycję **SQL** i wprowadź poświadczenia *nazwy użytkownika* i *hasła* dla lokalnego programu SQL Server. Należy wprowadzić wartość nazwa_serwera jako w **pełni kwalifikowaną nazwę wystąpienia nazwa_serwera ukośnik odwrotny (servername\instancename)**. Nazwa powiązanych z *adfonpremsql*.

###<a name="adf-linked-service-blob-store"></a>Usługi połączone dla obiektów Blob

Aby utworzyć powiązanych z magazynem obiektów Blob platformy Azure konta:

- Kliknij pozycję **Magazyn danych** w strona początkowa ADF na klasyczny Portal Azure
- Wybieranie **Konta magazynu platformy Azure** 
- Wprowadź nazwę klucza i kontener konta magazyn obiektów Blob platformy Azure. Nazwa powiązanych z *adfds*.

###<a name="adf-linked-service-azure-sql"></a>Powiązanych z bazy danych Azure SQL

Aby utworzyć usługę połączony z bazą danych SQL Azure:

- Kliknij pozycję **Magazyn danych** w strona początkowa ADF na klasyczny Portal Azure
- Wybierz pozycję **Azure SQL** i wprowadź poświadczenia *nazwy użytkownika* i *hasła* bazy danych SQL Azure. *Nazwa użytkownika* musi być określony jako *user@servername*.   


##<a name="adf-tables"></a>Definiowanie i tworzenie tabel, aby określić, jak uzyskać dostęp do danych

Tworzenie tabel, które Określ struktury, lokalizacja oraz dostępności zestawy danych z poniższych procedur na podstawie skryptów. Pliki JSON są używane do tabel. Aby uzyskać więcej informacji na temat struktury te pliki Zobacz [zestawy danych](../data-factory/data-factory-create-datasets.md).

> [AZURE.NOTE]  Należy wykonać `Add-AzureAccount` polecenia cmdlet przed wykonaniem polecenia cmdlet [AzureDataFactoryTable nowy](https://msdn.microsoft.com/library/azure/dn835096.aspx) , aby potwierdzić, że prawo Azure subskrypcji został wybrany do wykonywania polecenia. Dokumentacji tego polecenia cmdlet zobacz [Dodawanie AzureAccount](https://msdn.microsoft.com/library/azure/dn790372.aspx).

Definicje oparte na JSON w tabelach za pomocą następujących nazw:

* **Nazwa tabeli** w programie SQL server w wersji lokalnej jest *nyctaxi_data*
* **Nazwa kontenera** na koncie magazyn obiektów Blob platformy Azure to *containername*  

Trzy definicji tabel są potrzebne do tego procesu ADF:

1. [Tabela programu SQL w wersji lokalnej](#adf-table-onprem-sql)
2. [Tabela obiektów blob](#adf-table-blob-store)
3. [SQL Azure tabeli](#adf-table-azure-sql)

> [AZURE.NOTE]  Poniższe procedury za pomocą programu PowerShell Azure zdefiniować i utworzyć działania ADF. Jednak tych zadań można także wykonywać za pomocą portalu Azure. Aby uzyskać szczegółowe informacje zobacz [Tworzenie dane wejściowe i wyjściowe zestawy danych](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-3-create-input-and-output-datasets).

###<a name="adf-table-onprem-sql"></a>Tabela programu SQL w wersji lokalnej

Definicja tabeli dla programu SQL Server w wersji lokalnej jest określony w następującym pliku JSON:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

Nazwy kolumn nie zostaną uwzględnione w tym miejscu. Możesz wybrać częściowej według nazw kolumn, dołączając je tutaj (w przypadku wyboru szczegóły w temacie [dokumentacji ADF](../data-factory/data-factory-data-movement-activities.md ) .

Skopiować definicję JSON tabeli w pliku o nazwie *onpremtabledef.json* pliku, a następnie zapisz go w znanej lokalizacji (w tym miejscu przyjmuje się, że *C:\temp\onpremtabledef.json*). Tworzenie tabeli w ADF przy użyciu następującego polecenia cmdlet programu PowerShell Azure:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


###<a name="adf-table-blob-store"></a>Tabela obiektów blob
Definicja tabeli dla lokalizację danych wyjściowych obiektów blob znajduje się w następujących (map zasysanego danych z lokalnego do obiektów blob platformy Azure):

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

Skopiować definicję JSON tabeli w pliku o nazwie *bloboutputtabledef.json* pliku, a następnie zapisz go w znanej lokalizacji (w tym miejscu przyjmuje się, że *C:\temp\bloboutputtabledef.json*). Tworzenie tabeli w ADF przy użyciu następującego polecenia cmdlet programu PowerShell Azure:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

###<a name="adf-table-azure-sq"></a>SQL Azure tabeli
Definicja tabeli platformy SQL Azure wyjściowy jest poniżej (ten schemat mapy dane pochodzące z obiektów blob):

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

Skopiować definicję JSON tabeli w pliku o nazwie *AzureSqlTable.json* pliku, a następnie zapisz go w znanej lokalizacji (w tym miejscu przyjmuje się, że *C:\temp\AzureSqlTable.json*). Tworzenie tabeli w ADF przy użyciu następującego polecenia cmdlet programu PowerShell Azure:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


##<a name="adf-pipeline"></a>Definiowanie i tworzenie proces

Określ działania, które należą do nich i Utwórz proces z poniższych procedur na podstawie skryptów. Aby zdefiniować właściwości planowana zostanie użyty plik JSON.

* Skrypt przyjęto założenie, że **planowanej nazwa** jest *AMLDSProcessPipeline*.
* Ponadto należy zauważyć, że firma Microsoft częstotliwości proces można wykonać w dziennym i używania domyślny czas wykonywania zadania (00 jest UTC).

> [AZURE.NOTE]Poniższe procedury przy użyciu Azure do definiowania i tworzenia potoku ADF. Ale to zadanie można także wykonywać za pomocą portalu Azure. Aby uzyskać szczegółowe informacje zobacz [Tworzenie i uruchamianie potok](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-4-create-and-run-a-pipeline).

Używanie definicji tabel opisane wcześniej, definicję planowana Podajnik wygląda następująco:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premise SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premise SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",             
                            }           
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Skopiuj tej definicji JSON potoku do pliku o nazwie *pipelinedef.json* pliku, a następnie zapisz go w znanej lokalizacji (w tym miejscu przyjmuje się, że *C:\temp\pipelinedef.json*). Utwórz proces w ADF z następującego polecenia cmdlet programu PowerShell Azure:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Potwierdź widzą planowana na ADF w portalu klasyczny Azure wyświetlany następujący (po kliknięciu przycisku diagram)

![](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)


##<a name="adf-pipeline-start"></a>Uruchom proces
Proces może zostać uruchomiony przy użyciu następującego polecenia:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

Wartości parametrów *startdate* i *enddate* muszą być zastąpiona rzeczywistymi datami, między którymi ma proces, aby uruchomić.

Gdy proces wykonuje, należy wyświetlić dane są wyświetlane w kontenerze zaznaczone dla obiektów blob, jednego pliku na dzień.

Należy zauważyć, że nie masz firma Microsoft osobie funkcjonalność ADF danych potoku stopniowo. Aby uzyskać więcej informacji na temat tego i innych możliwości oferowane przez ADF zapoznaj się z [dokumentacją ADF](https://azure.microsoft.com/services/data-factory/).
