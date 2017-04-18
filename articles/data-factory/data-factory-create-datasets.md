<properties 
    pageTitle="Tworzenie zestawów danych w Factory Azure danych | Microsoft Azure" 
    description="Dowiedz się, jak tworzyć zestawy danych w Azure Factory danych z przykłady użycia właściwości, takie jak przesunięcie i anchorDateTime."
    keywords="Tworzenie zestawu danych, przykład zestawu danych, przesunięcie przykład"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="shlo"/>

# <a name="datasets-in-azure-data-factory"></a>Zestawy danych w Factory Azure danych
W tym artykule opisano zestawów danych w Factory danych Azure i zawiera przykłady takich jak przesunięcie, anchorDateTime i przesunięcie i styl baz danych.

Po utworzeniu zestawu danych, tworzysz wskaźnik, aby dane, które mają być przetworzone. Dane są przetwarzane (wejście i wyjście) w działaniu i działania jest zawarty w potoku. Zestaw danych wejściowych reprezentuje dane wejściowe do działania w potoku a zestaw danych wyjściowych wynik działania.

Zestawy danych identyfikowanie danych w różnych magazynów, takich jak tabele, pliki, foldery i dokumenty. Po utworzeniu zestawu danych, możesz go używać z działaniami w potoku. Na przykład zestaw danych może być zestaw wejścia i wyjścia działaniem Kopiuj lub działania HDInsightHive. Azure portal umożliwia wizualne układu potoki i danych dane wejściowe i wyjściowe. Przejrzenie są wyświetlane wszystkie relacje i zależności usługi procesy u własnych źródeł aby zawsze wiedzieć, gdzie pochodzą dane i miejsce, w którym będzie.

W Azure Factory danych można uzyskać dane z zestawu danych za pomocą kopiowania aktywności w potoku.

> [AZURE.NOTE] Jeśli jesteś nowym użytkownikiem Factory danych Azure, zobacz [Wprowadzenie do Azure Factory danych](data-factory-introduction.md) zawiera omówienie dotyczące usługi Azure danych Factory. Zobacz [Tworzenie pierwszej firmie danych](data-factory-build-your-first-pipeline.md) samouczka tworzenie pierwszej firmie danych. Te dwa artykuły opisowe, tło informacji potrzebnych do lepszego zrozumienia w tym artykule. 

## <a name="define-datasets"></a>Definiowanie zestawy danych
Zestawu danych w Azure Factory danych jest definiowana w następujący sposób: 


    {
        "name": "<name of dataset>",
        "properties": {
            "type": "<type of dataset: AzureBlob, AzureSql etc...>",
            "external": <boolean flag to indicate external data. only for input datasets>,
            "linkedServiceName": "<Name of the linked service that refers to a data store.>",
            "structure": [
                {
                    "name": "<Name of the column>",
                    "type": "<Name of the type>"
                }
            ],
            "typeProperties": {
                "<type specific property>": "<value>",
                "<type specific property 2>": "<value 2>",
            },
            "availability": {
                "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
                "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
            },
           "policy": 
            {      
            }
        }
    }

W poniższej tabeli opisano właściwości w JSON powyżej:   

| Właściwość | Opis | Wymagane | Domyślne |
| -------- | ----------- | -------- | ------- |
| Nazwa | Nazwa zestawu danych. Zobacz [Azure danych Factory - nazewnictwa reguł](data-factory-naming-rules.md) nazewnictwa reguł. | Tak | BRAK |
| Typ | Typ zestawu danych. Określ jeden z typów obsługiwanych przez Azure Factory danych (na przykład: AzureBlob, AzureSqlTable). <br/><br/>Aby uzyskać szczegółowe informacje, zobacz temat [Type zestawu danych](#Type) . | Tak | BRAK |
| Struktura | Schemat zestawu danych<br/><br/>Aby uzyskać szczegółowe informacje, zobacz [Zestaw danych struktury](#Structure) sekcji. | Wartość nie. | BRAK |
| typeProperties | Właściwości odpowiednio do wybranego typu. [Typ danych](#Type) podano w sekcji szczegółowych informacji na temat obsługiwanych typów i ich właściwości. | Tak | BRAK |
| zewnętrzne | Logiczna flagę, aby określić, czy zestaw danych jest jawnie tworzone przez potok factory danych, czy nie.  | Brak | FAŁSZ | 
| dostępność | Definiuje okna przetwarzania lub skalowania wzór produkcji zestawu danych. <br/><br/>Aby uzyskać szczegółowe informacje, zobacz [Zestaw danych dostępność](#Availability) sekcji. <br/><br/>Aby uzyskać szczegółowe informacje w zestawie danych krojenie modelu, zobacz artykuł [planowania i wykonanie](data-factory-scheduling-and-execution.md) . | Tak | BRAK
| zasady | Określa kryteria lub warunku, który musi spełniać wycinków zestawu danych. <br/><br/>Aby uzyskać szczegółowe informacje, zobacz [Zasady zestawu danych](#Policy) sekcji. | Brak | BRAK |

## <a name="dataset-example"></a>Przykład zestawu danych
W poniższym przykładzie zestawu danych reprezentuje tabeli o nazwie **Moja_tabela** w **bazie danych Azure SQL**. 

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

Zwróć uwagę następujące punkty: 

- Typ jest ustawiony na AzureSqlTable.
- Właściwość typu tableName (specyficzne dla typu AzureSqlTable) jest ustawiona na Moja_tabela.
- linkedServiceName odwołuje się do usługi połączone typu AzureSqlDatabase. Zobacz definicji następujące usługi połączone. 
- częstotliwość dostępność jest ustawiona na dzień i interwału jest równa 1, co oznacza, że wycinek powstaje codziennie.  

AzureSqlLinkedService jest definiowana w następujący sposób:

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }

W powyższym JSON: 

- Typ jest ustawiona na AzureSqlDatabase
- Właściwość Typ connectionString określa informacje dotyczące nawiązywania połączenia z bazą danych Azure SQL.  


Jak widać, powiązanych z określa sposób nawiązywania połączenia z bazą danych programu Azure SQL. Zestaw danych określa, która tabela jest używany jako wyjścia działania w potoku. Sekcja aktywności w [potoku](data-factory-create-pipelines.md) JSON Określa, czy zestaw danych jest używana jako wejściowe i wyjściowe zestawu danych.


> [AZURE.IMPORTANT] O ile zestawu danych jest wyprodukowane przy Factory danych Azure, ma zostać oznaczona jako **zewnętrznych**. To ustawienie dotyczy nakładów pierwszym działaniem w potoku.   

## <a name="Type"></a>Typ zestawu danych
Obsługiwane źródła danych i typy danych są wyrównane. Zobacz Tematy zawarte w artykule [Działania przepływu danych](data-factory-data-movement-activities.md#supported-data-stores) informacji na temat typów i konfiguracji zestawy danych. Na przykład jeśli korzystasz z danych z bazy danych programu Azure SQL, kliknij bazy danych SQL Azure na liście obsługiwanych magazynów, aby wyświetlić szczegółowe informacje.  

## <a name="Structure"></a>Struktura zestawu danych
Sekcja **struktury** definiuje schemat zestawu danych. Zawiera zbiór nazwy i typy danych kolumn.  W poniższym przykładzie zestaw danych zawiera trzy kolumny slicetimestamp, NazwaProjektu i pageviews i są one typu: ciąg, ciąg i dziesiętnych odpowiednio.

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Availability"></a>Dostępność zestawu danych
Sekcji **dostępność** w zestawie danych określa okna przetwarzania (co godzina, dzienną, tygodniową itp.) lub skalowania modelu dla zestawu danych. Zobacz artykuł [planowania i wykonanie](data-factory-scheduling-and-execution.md) więcej informacji na temat zestawu danych modelu odcięć i zależności. 

W poniższej sekcji dostępność Określa, że dane wyjściowe zestawu danych jest wyprodukowane co godzina (albo) wprowadzania zestawu danych jest dostępny co godzinę:

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 1   
    }

W poniższej tabeli opisano właściwości, których można używać w sekcji dostępność: 

| Właściwość | Opis | Wymagane | Domyślne |
| -------- | ----------- | -------- | ------- |
| częstość | Umożliwia określenie jednostki czasu dla produkcji wycinek zestawu danych.<br/><br/>**Częstotliwość obsługiwane**: minuta, godzina, dnia, tygodnia, miesiąca | Tak | BRAK |
| Interwał | Umożliwia określenie współczynnika dla częstotliwości<br/><br/>"Interwał częstotliwości x" Określa, jak często powstają wycinek.<br/><br/>W razie potrzeby zestawie danych, aby zostać podzielona co godzinę, należy ustawić **Częstotliwość** na **godziny**i **Interwał** **1**.<br/><br/>**Uwaga:** Jeśli częstotliwość jest określony jako minuta, zaleca się ustawianie interwału nie mniej niż 15 | Tak | BRAK |
| Styl | Określa, czy wycinek powinien być przedstawiony na początek/koniec okresu.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Jeśli argument częstotliwość jest ustawiona na miesiąc i zestaw stylów w celu EndOfInterval, wycinek jest tworzony ostatniego dnia miesiąca. Jeśli odpowiedni styl jest ustawiona na StartOfInterval, wycinek jest tworzony na pierwszy dzień miesiąca.<br/><br/>Jeśli argument częstotliwość jest ustawiona na dzień i zestaw stylów w celu EndOfInterval, wycinek jest tworzony w ostatniej godziny dnia.<br/><br/>Jeśli argument częstotliwość jest ustawiona na godzinę i zestaw stylów w celu EndOfInterval, wycinek jest tworzony na końcu godzinę. Na przykład dla wycinek okres 1 PM — 2 PM wycinek jest tworzony godzinie 2. | Brak | EndOfInterval |
| anchorDateTime | Określa położenie bezwzględne w momencie używana przez harmonogram do obliczania cięcia zestawu danych. <br/><br/>**Uwaga:** Jeśli AnchorDateTime ma części daty, które są bardziej szczegółowego niż częstotliwość bardziej szczegółowego części są ignorowane. <br/><br/>Na przykład jeśli **Interwał** jest **co godzinę** (częstotliwość: godziny i interwału: 1) i **AnchorDateTime** zawiera **minut i sekund**, a następnie **minuty i sekundy** części AnchorDateTime są ignorowane. | Brak | 0001-01-01 |
| Przesunięcie | Przedziału czasu, w którym są przenoszone początek i koniec wszystkie wycinki zestawu danych. <br/><br/>**Uwaga:** Jeśli określono zarówno anchorDateTime, jak i przesunięcie, wynikiem funkcji jest połączony shift. | Brak | BRAK |

### <a name="offset-example"></a>przykład przesunięcie

Dzienny plasterki rozpoczynających się na 6: 00 zamiast północy domyślne. 

    "availability":
    {
        "frequency": "Day",
        "interval": 1,
        "offset": "06:00:00"
    }

**Częstotliwość** jest ustawiona na **dzień** i **Interwał** jest ustawiony na wartość **1** (raz dziennie): Jeśli chcesz wycinek, aby zostały przedstawione w 6: 00 zamiast w czasie domyślne: 00 jest. Pamiętaj, że teraz jest czasu UTC. 

## <a name="anchordatetime-example"></a>przykład anchorDateTime

**Przykład:** 23 godziny zestawu danych wycinków rozpoczynające się od 2007-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 23, 
        "anchorDateTime":"2007-04-19T08:00:00"  
    }

## <a name="offsetstyle-example"></a>Przykład przesunięcie/stylu

Jeśli potrzebujesz zestawu danych na miesiąc na określoną datę i godzinę (Załóżmy na 3rd każdego miesiąca o 8:00) używanie znaczników **Przesunięcie** umożliwia ustawianie daty i godziny jego powinna działać. 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }


## <a name="Policy"></a>Zasady zestawu danych

Sekcji **zasady** w definicji zestawu danych określa kryteria lub warunku, który musi spełniać wycinków zestawu danych.

### <a name="validation-policies"></a>Zasady sprawdzania poprawności

| Nazwa zasady | Opis | Zastosowane do | Wymagane | Domyślne |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | Sprawdza, czy dane w **Azure blob** spełnia wymagania minimalny rozmiar (w megabajtach). | Obiektów Blob platformy Azure | Brak | BRAK |
|minimumRows | Sprawdza, czy dane w **bazą danych Azure SQL** lub **tabel platformy Azure** zawiera minimalna liczba wierszy. | <ul><li>Baza danych SQL Azure</li><li>Tabel platformy Azure</li></ul> | Brak | BRAK

#### <a name="examples"></a>Przykłady

**minimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### <a name="external-datasets"></a>Zestawy danych zewnętrznych

Zestawy danych zewnętrznych są pokazane, które nie są tworzone przez potok uruchomionego w factory danych. Jeśli zestaw danych został oznaczony jako **zewnętrzne**, można zdefiniować zasady **ExternalData** wpływać na zachowanie dostępności wycinek zestawu danych. 

O ile zestawu danych jest wyprodukowane przy Factory danych Azure, ma zostać oznaczona jako **zewnętrznych**. To ustawienie jest stosowane ogólnie do wprowadzania pierwszym działaniem w potoku chyba że aktywności lub łączenia planowana jest używany. 

| Nazwa | Opis | Wymagane | Wartość domyślna  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | Czas opóźnienia Sprawdź dostępność danych zewnętrznych dla danego wycinek. Na przykład danych ma być dostępny co godzinę, może zostać opóźnione przy użyciu dataDelay Sprawdź dane zewnętrzne jest dostępny i odpowiadające im wycinek jest gotowy.<br/><br/>Dotyczy tylko obecnie niedostępna.  Na przykład jeśli 1:00 PM teraz i ta wartość wynosi 10 minut, sprawdzanie poprawności zostanie uruchomiony godzinie 13:10.<br/><br/>To ustawienie nie wpływa na wycinków w przeszłości (wycinków z czas zakończenia wycinek + dataDelay < teraz) są przetwarzane bez zwłoki.<br/><br/>Czas jest większa niż 23:59, które godzin potrzebne do określonego w formacie day.hours:minutes:seconds. Na przykład aby określić 24 godziny, nie używaj 24:00:00; Użyj zamiast tego 1.00:00:00. Jeśli używasz 24:00:00 jest traktowane jako 24 dni (24.00:00:00). 1 i 4 godziny dni Określ 1:04:00:00. | Brak | 0 |
| retryInterval | Czas oczekiwania między awarii i następnej ponów próbę. Stosuje do chwili obecnej; Jeśli poprzedniego wypróbować zakończonego niepowodzeniem, możemy poczekać to po ostatniej spróbuj. <br/><br/>Jeśli jest to 1:00 pm teraz, możemy rozpocząć podczas pierwszej próby. W przypadku 1 min i operacja nie powiodła się czas trwania, aby ukończyć pierwszą sprawdzania poprawności następną próbą wynosi 1:00 + 1 min (czas trwania) + 1min (interwał ponawiania) = 1:02 pm. <br/><br/>Dla wycinków w przeszłości nie ma żadnego opóźnienia. Próba odbywa się natychmiast. | Brak | 00:01:00 (1 minuta) | 
| retryTimeout | Limit czasu dla każdej próbie ponów próbę.<br/><br/>Jeśli ta właściwość jest ustawiona na 10 minut, sprawdzania poprawności musi być wykonane w ciągu 10 minut. Jeśli trwa dłużej niż 10 minut, aby wykonać sprawdzanie poprawności, ponów próbę przekroczenie limitu czasu.<br/><br/>Jeśli wszystkie próby sprawdzania poprawności przekroczenie limitu czasu, wycinek jest oznaczony jako upłynął limit czasu. | Brak | 00:10:00 (10 minut) |
| maximumRetry | Liczba powtórzeń, aby sprawdzić dostępność danych zewnętrznych. Wartość maksymalna dozwolona wynosi 10. | Brak | 3 | 

## <a name="scoped-datasets"></a>Zestawy zakresu danych
Można tworzyć zestawy danych, które są ograniczone do potok przy użyciu właściwości **zestawy danych** . Te zestawy danych można używać tylko działania w ramach tego procesu, ale nie działań w innych procesy. W poniższym przykładzie zdefiniowano potok z dwóch zestawów danych — Podłączanie InputDataset i Podłączanie pulpitu zdalnego OutputDataset - może być używany w ramach procesu:  

> [AZURE.IMPORTANT] Zestawy zakresu danych są obsługiwane tylko w przypadku jednorazowego procesy (**pipelineMode** ustaw **OneTime**). Aby uzyskać szczegółowe informacje, zobacz [Onetime procesu](data-factory-scheduling-and-execution.md#onetime-pipeline) .

    {
        "name": "CopyPipeline-rdc",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset-rdc"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset-rdc"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1,
                        "style": "StartOfInterval"
                    },
                    "name": "CopyActivity-0"
                }
            ],
            "start": "2016-02-28T00:00:00Z",
            "end": "2016-02-28T00:00:00Z",
            "isPaused": false,
            "pipelineMode": "OneTime",
            "expirationTime": "15.00:00:00",
            "datasets": [
                {
                    "name": "InputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "InputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/input",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": true,
                        "policy": {}
                    }
                },
                {
                    "name": "OutputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "OutputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/output",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": false,
                        "policy": {}
                    }
                }
            ]
        }
    }