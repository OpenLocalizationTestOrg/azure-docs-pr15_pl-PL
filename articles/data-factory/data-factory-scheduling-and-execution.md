<properties
    pageTitle="Planowanie i wykonywanie Factory danych | Microsoft Azure"
    description="Dowiedz się, planowania i wykonanie aspektów model aplikacji Azure danych Factory."
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
    ms.date="08/22/2016"
    ms.author="spelluru"/>

# <a name="data-factory-scheduling-and-execution"></a>Planowanie Factory danych i wykonanie
W tym artykule wyjaśniono, planowania i wykonanie aspektów model aplikacji Azure danych Factory. 

## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule założono, aby zrozumieć podstawy Factory danych aplikacji modelu pojęcia, włącznie z aktywności, procesy, usługi połączone i zestawy danych. Aby uzyskać podstawowe pojęcia fabryki danych Azure zobacz następujące artykuły:

- [Wprowadzenie do fabryki danych](data-factory-introduction.md)
- [Procesy](data-factory-create-pipelines.md)
- [Zestawy danych](data-factory-create-datasets.md) 

## <a name="schedule-an-activity"></a>Planowanie działań

Z sekcją harmonogramu aktywności JSON można określić harmonogramu cyklicznego do działania. Na przykład można zaplanować działanie co godzinę w następujący sposób:

    "scheduler": {
        "frequency": "Hour",
        "interval": 1
    },  

![Przykład harmonogram](./media/data-factory-scheduling-and-execution/scheduler-example.png)

Jak pokazano na diagramie, określając rozłożonych w czasie działania tworzy kilka okien tumbling. Tumbling windows są serią stałym rozmiarze siebie, ciągłe interwałów. Okna te logiczne tumbling działania są nazywane *działania systemu windows*.

W oknie aktualnie aktywności można korzystać z interwał skojarzone z okna aktywności [WindowStart](data-factory-functions-variables.md#data-factory-system-variables) i [WindowEnd](data-factory-functions-variables.md#data-factory-system-variables) zmienne systemu w działaniu JSON. Za pomocą tych czynników dla różnych celów w swojej aktywności JSON. Na przykład można je zaznaczyć dane wejściowe i wyjściowe zestawy danych, reprezentująca godziny serii danych.

Właściwość **Harmonogram** obsługuje samej właściwości jako właściwości **dostępność** w zestawie danych. Aby uzyskać szczegółowe informacje, zobacz [Dostępność zestawu danych](data-factory-create-datasets.md#Availability) . Przykłady: Planowanie z przesunięciem określonym czasie lub ustawianie trybu do wyrównywania przetwarzanie na początku lub końcu interwał w oknie czynność.

Możesz określić **Harmonogram** właściwości dla działania, ale ta właściwość jest **Opcjonalna**. Jeśli użytkownik określi, że właściwość, musi być zgodna cadence, zadanej w definicji zestawu danych dane wyjściowe. Dane wyjściowe zestawu danych jest obecnie, co dyski harmonogram, należy utworzyć zestaw danych wyjściowych, nawet jeśli działania generuje żadnego wyniku. Jeśli działanie nie trwa wszelkie wprowadzone dane, możesz pominąć tworzenie zestawu danych wejściowych.

## <a name="time-series-datasets-and-data-slices"></a>Przedziały zestawy danych i danych serii czasu

Czas serii danych jest ciągłej sekwencji punktów danych, które zwykle składa się z następujących po sobie pomiarów w przedziale czasu. Typowe czasu serie danych przykładami czujnik danych i danych telemetrycznych aplikacji.

Z Factory danych można przetwarzać uruchomieniu serii danych w sposób wsadowej z działaniem. Zazwyczaj jest cykliczne cadence szybkości danych wejściowych przychodzi i wyjściowe potrzeb dane mają zostać przedstawione. Ten cadence w modelu, określając **dostępność** w zestawie danych w następujący sposób:

    "availability": {
      "frequency": "Hour",
      "interval": 1
    },

Każdej jednostki danych zużyte i tworzone przez Uruchom aktywności jest określana mianem wycinek danych. Na poniższym diagramie przedstawiono przykład działania z jednego zestawu danych wejściowych i jedną wyjściowych zestawu danych. Te zestawy danych o **dostępności** ustawiona na co godzinę częstotliwości.

![Harmonogram dostępności](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Na powyższym diagramie przedstawiono godzinowe fragmentów dane wejściowe i wyjściowe zestawu danych. Na diagramie pokazano trzy wycinki wprowadzania danych, które są gotowe do przetwarzania. Działanie AM 10-11 jest w toku produkcji wycinek dane wyjściowe AM 10-11.

Masz dostęp do przedział czasu skojarzone z bieżącym wycinek wyprodukowane w zestawie danych JSON przy użyciu zmiennych [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) i [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables).

Obecnie Factory danych wymaga, że z harmonogramem określonym w działaniu dokładnie zgodne z harmonogramem określonym w **dostępność** zestawu danych dane wyjściowe. W związku z tym **WindowStart**, **WindowEnd**, **SliceStart**i **SliceEnd** zawsze mapowane na tym samym czasie i wycinek pojedynczy wynik.

Aby uzyskać więcej informacji na różne właściwości dostępnych w sekcji dostępność zobacz [Tworzenie zestawów danych](data-factory-create-datasets.md).

## <a name="move-data-from-sql-database-to-blob-storage"></a>Przenoszenie danych z bazy danych SQL z magazynem obiektów Blob

Załóżmy kwestie ze sobą i umieścić w działaniu, tworząc potok kopiuje dane z tabeli bazy danych SQL Azure z magazynem obiektów Blob platformy Azure co godzinę.

**Danych wejściowych: Zestaw danych bazy danych SQL Azure**

    {
        "name": "AzureSqlInput",
        "properties": {
            "published": false,
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


**Częstotliwość** jest ustawiona na **godzinę** i **interwału** jest ustawiona na **1** w sekcji dostępność.

**Wyjście: Zestaw danych magazyn obiektów Blob platformy Azure**

    {
        "name": "AzureBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
                "format": {
                    "type": "TextFormat"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Częstotliwość** jest ustawiona na **godzinę** i **interwału** jest ustawiona na **1** w sekcji dostępność.



**Aktywność: Aktywność Kopiuj**

    {
        "name": "SamplePipeline",
        "properties": {
            "description": "copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 100000,
                            "writeBatchTimeout": "00:05:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureSQLInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            ],
            "start": "2015-01-01T08:00:00Z",
            "end": "2015-01-01T11:00:00Z"
        }
    }


Pokazano harmonogram aktywności i sekcje dostępność zestawu danych Ustaw godzinowe częstotliwości. Pokazano, jak można użyć **WindowStart** i **WindowEnd** , aby zaznaczyć odpowiednie dane do działania uruchomić i skopiować go do obiektów blob z odpowiednich **ścieżkafolderu**. **Ścieżkafolderu** jest sparametryzowane ma osobny folder dla każdej godziny.

Trzy wycinki pomiędzy 8 — 11 wykonywania danych w bazie danych SQL Azure jest następująca:

![Przykładowe dane wejściowe](./media/data-factory-scheduling-and-execution/sample-input-data.png)

Po wdrożeniu jej proces, obiektów blob platformy Azure znajduje się w następujący sposób:

-   Plik mypath i 2015-1-1-8-danych. &lt;Guid&gt;txt z danymi

            10002345,334,2,2015-01-01 08:24:00.3130000
            10002345,347,15,2015-01-01 08:24:00.6570000
            10991568,2,7,2015-01-01 08:56:34.5300000

    > [AZURE.NOTE] &lt;Identyfikator GUID&gt; jest zastępowany rzeczywisty guid. Przykład nazwy pliku: Data.bcde1348-7620-4f93-bb89-0eed3455890b.txt
-   Plik mypath i 2015-1-1-9-danych. &lt;Guid&gt;txt z danymi:

            10002345,334,1,2015-01-01 09:13:00.3900000
            24379245,569,23,2015-01-01 09:25:00.3130000
            16777799,21,115,2015-01-01 09:47:34.3130000
-   Plik mypath i 2015-1-1-10-danych. &lt;Guid&gt;txt bez danych.


## <a name="active-period-for-pipeline"></a>Aktywnego okresu dla procesu

[Tworzenie procesy](data-factory-create-pipelines.md) wprowadzona pojęcie aktywnego okresu dla potok określonych przez ustawienie właściwości **Początek** i **koniec** .

Można ustawić datę rozpoczęcia dla aktywnego okresu i planowana w przeszłości. Factory danych oblicza wszystkie wycinki danych (wypełnienia wstecz) w przeszłości i automatycznie rozpocznie przetwarzanie je.

## <a name="parallel-processing-of-data-slices"></a>Przetwarzanie równoległe wycinków danych
Możesz skonfigurować fragmenty wypełnione wstecz danych mają być wykonywane w równolegle przez ustawienie właściwości **współbieżności** w sekcji zasady działania JSON. Aby uzyskać więcej informacji na temat tej właściwości zobacz [Tworzenie procesy](data-factory-create-pipelines.md).

## <a name="rerun-a-failed-data-slice"></a>Uruchom ponownie wycinek danych nie powiodło się 
Można monitorować wykonanie wycinków w sposób zaawansowanej, wizualne. Aby uzyskać szczegółowe informacje, zobacz [Monitorowanie i zarządzanie nimi za pomocą karty portal Azure procesy](data-factory-monitor-manage-pipelines.md) lub [aplikacji monitorowania i zarządzania](data-factory-monitor-manage-app.md) .

Należy rozważyć, czy poniższy przykład, który zawiera dwa działania. Activity1 tworzy zestaw danych serii czasu z wycinków jako wynik używanej jako danych wejściowych przez Activity2 da zestawu wyjściowego czasu serii danych.

![Wycinek nie powiodło się](./media/data-factory-scheduling-and-execution/failed-slice.png)

Diagram wskazuje, że poza trzy ostatnie wycinki, wystąpił błąd produkcji wycinek AM 9 – 10 dla Dataset2. Dane Factory automatyczne śledzenie zależności dla zestawu danych serii czasu. W wyniku go nie można uruchomić działania uruchamianie dla wycinek podrzędne 9-10 AM.

Narzędzia monitorowania i zarządzania Factory danych umożliwiają przechodzenie do dzienników diagnostycznych dla zakończonego niepowodzeniem wycinek można łatwo dostępne głównej przyczyny problemu i rozwiązywanie problemu. Po problem został rozwiązany, można łatwo rozpocząć aktywności Uruchom, aby utworzyć wycinek nie powiodło się. Aby uzyskać więcej informacji na temat ponownie i opis stan przejścia dla fragmenty danych zobacz [monitorowania i zarządzania procesy przy użyciu karty portal Azure](data-factory-monitor-manage-pipelines.md) lub [aplikacja do monitorowania i zarządzania](data-factory-monitor-manage-app.md).

Po ponownym wycinek 9-10 AM dla **Dataset2**Factory danych uruchamia Uruchom dla wycinek zależne AM 9-10 na ostateczne zestawu danych.

![Uruchom ponownie wycinek nie powiodło się](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="run-activities-in-a-sequence"></a>Uruchamianie działań w sekwencji
Dwa działania (uruchomiony aktywności jeden po drugim) można rowerowy przez ustawienie zestawu danych wynik działania jednego zestawu danych wejściowych inne działania. Działania mogą być w tym samym potoku lub w różnych procesy. Druga aktywności wykonuje tylko podczas pierwszego zakończy się pomyślnie.

Na przykład można rozważyć następujące sprawy:

1.  Potok P1 ma A1 aktywności, wymaga wprowadzania zestawu danych zewnętrznych D1, która daje wynik zestawu danych D2.
2.  Potok P2 ma A2 aktywności, wymaga wprowadzania danych z zestawu danych D2, która daje wynik zestawu danych D3.

W tym scenariuszu działania A1 i A2 znajdują się w różnych procesy. Działanie A1 jest uruchamiane po danych zewnętrznych jest dostępny i osiągnięciu częstotliwość planowaną dostępność. Działanie A2 jest uruchamiane po zaplanowanym wycinków z D2 staną się dostępne i osiągnięciu częstotliwość planowaną dostępność. W przypadku błędu w jednym z wycinków w zestawie danych D2, A2 nie jest uruchamiany dla ten wycinek, dopóki nie będzie dostępne.

Widok diagramu będzie wyglądało Poniższy diagram:

![Tworzenie łańcucha działania na dwie części](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Jak wspomniano wcześniej, w tym samym potoku może być działań. Widok diagramu z obu działań w tym samym potoku będzie wyglądało Poniższy diagram:

![Tworzenie łańcuchów działania w tym samym potoku](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

### <a name="copy-sequentially"></a>Kopiowanie kolejno
Istnieje możliwość przeprowadzić wielu operacji kopiowania jeden po drugim w sposób sekwencyjne zamówiono. Na przykład może być dwa działania kopii w potoku (CopyActivity1 i CopyActivity2) z następujących zestawów danych dane wyjściowe danych wejściowych:   

CopyActivity1

Danych wejściowych: zestaw danych. Wynik: Dataset2.

CopyActivity2

Danych wejściowych: Dataset2.  Wynik: Dataset3.

CopyActivity2 można uruchomić tylko wtedy, gdy pomyślnym uruchomieniu CopyActivity1 i Dataset2 jest dostępna.

Oto przykładowy planowana JSON:

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob1ToBlob2",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset3"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob2ToBlob3",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2016-08-25T01:00:00Z",
            "end": "2016-08-25T01:00:00Z",
            "isPaused": false
        }
    }

Zwróć uwagę, że w tym przykładzie zestaw danych wynik działania pierwszej kopii (Dataset2) jest określony jako danych wejściowych dla drugiego działania. W związku z tym drugi aktywności działa tylko wtedy, gdy zestaw danych dane wyjściowe z pierwszym działaniem jest gotowa do wysłania.  

W przykładzie CopyActivity2 może zawierać różne wprowadzenie danych, takich jak Dataset3, ale określić Dataset2 jako dane wejściowe do CopyActivity2, dopóki nie zakończy się CopyActivity1 działania nie będą uruchamiane. Na przykład:

CopyActivity1

Danych wejściowych: Dataset1. Wynik: Dataset2.

CopyActivity2

Wejścia: Dataset3, Dataset2. Wynik: Dataset4.

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlobToBlob",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset3"
                        },
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset4"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob3ToBlob4",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2017-04-25T01:00:00Z",
            "end": "2017-04-25T01:00:00Z",
            "isPaused": false
        }
    }


Zwróć uwagę, że w tym przykładzie dwa zestawy danych wejściowych podano drugiego wykonania kopii. Gdy określono wielu obrazów wejściowych, tylko pierwszy wprowadzania zestawu danych służy do kopiowania danych, ale inne zestawy danych są używane jako zależności. CopyActivity2 zaczyna się tylko wtedy, gdy są spełnione następujące warunki:

- Pomyślnie CopyActivity1 i Dataset2 jest dostępna. Ten zestaw danych nie jest używana podczas kopiowania danych do Dataset4. Tylko działa jako zależność planowania dla CopyActivity2.   
- Dataset3 jest dostępna. Tego zestawu danych reprezentuje dane, które są kopiowane do miejsca docelowego.  



## <a name="model-datasets-with-different-frequencies"></a>Zestawy modelu danych z różnych częstotliwości

W próbkach częstotliwości wejściowe i wyjściowe zestawy danych i okno Harmonogram aktywności są takie same. Kilka scenariuszy wymagać do generowania danych wyjściowych z częstotliwością niż częstotliwości jeden lub więcej danych wejściowych. Factory danych obsługuje modelowania scenariuszom.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Przykład 1: Warzywa dzienny raport dane wyjściowe wprowadzania danych, który jest dostępny co godzinę

Rozważmy scenariusz, w którym mają wprowadzania danych oparte na miarach z czujników dostępne co godzinę w magazynie obiektów Blob platformy Azure. Chcesz warzywa dzienny raport agregacji danych statystycznych, takich jak średnia, maksimum i minimum w danym dniu z [danych Factory gałęzi aktywności](data-factory-hive-activity.md).

Poniżej opisano, jak można modelować w tym scenariuszu z Factory danych:

**Zestaw wprowadzania danych**

Co godzinę pliki wejściowe nie są wyświetlane w folderze dla danego dnia. Dostępność danych wejściowych jest ustawiona na **godzinę** (częstotliwość: godzina, interwał: 1).

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Dane wyjściowe zestawu danych**

Jeden plik docelowy zostanie utworzona codziennie w danym dniu folderu. Dostępność danych wyjściowych jest ustawiona na **dzień** (częstotliwość: dzień i interwału: 1).


    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Aktywność: aktywność gałęzi w potoku**

Skrypt gałęzi otrzyma odpowiednie informacje *daty i godziny* jako parametrów używające zmiennej **WindowStart** , jak pokazano w poniższej wstawki kodu. Skrypt gałęzi używa tej zmiennej, aby załadować dane z odpowiednim folderze w danym dniu i uruchamianie agregacji do generowania danych wyjściowych.

        {  
            "name":"SamplePipeline",
            "properties":{  
            "start":"2015-01-01T08:00:00",
            "end":"2015-01-01T11:00:00",
            "description":"hive activity",
            "activities": [
                {
                    "name": "SampleHiveActivity",
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adftutorial\\hivequery.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                            "Month": "$$Text.Format('{0:%M}',WindowStart)",
                            "Day": "$$Text.Format('{0:%d}',WindowStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },          
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 2,
                        "timeout": "01:00:00"
                    }
                 }
             ]
           }
        }

Na poniższym diagramie przedstawiono scenariusz z punktu widzenia zależności danych.

![Zależności danych](./media/data-factory-scheduling-and-execution/data-dependency.png)

Wycinek wynik dla każdego dnia w zależności od 24 godzinowe wycinków z zestawu danych wejściowych. Dane Factory oblicza zależności automatycznie z nazwami danych wejściowych wycinki reprezentujące wartości w tym samym czasie jako wycinek dane wyjściowe, które należy opracować. Jeśli dowolny z 24 wycinków wprowadzania nie jest dostępny, Factory danych oczekuje wprowadzania wycinek będzie gotowa przed rozpoczęciem aktywności dzienny, uruchom.


### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Przykład 2: Określ współzależności z wyrażeń i funkcji Factory danych

Przypatrzmy innym scenariuszu. Załóżmy, że masz działaniem gałęzi przetwarzającym dwa zestawy danych wejściowych. Jeden z nich ma nowe dane codziennie, ale jeden z nich otrzymuje nowe dane co tydzień. Załóżmy, że chcesz zrobić sprzężenia w dwóch danych wejściowych, a da wynik każdego dnia.

Proste podejście w których Factory danych automatycznie ilustracji się po prawej stronie wprowadzania wycinków przetwarzania ustawiając do pliku, który wycinek danych czasu okresu nie działa.

Należy określić, że dla każdej czynności uruchom Factory danych należy użyć ostatni tydzień wycinek tygodniowy zestaw danych wejściowych. Przy użyciu funkcji Factory danych Azure jak pokazano w poniższej wstawki kodu to zachowanie.

**Input1: Obiektów blob platformy Azure**

Pierwszy dane wejściowe są obiektów blob platformy Azure aktualizowany codziennie.

    {
      "name": "AzureBlobInputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Input2: Obiektów blob platformy Azure**

Input2 jest obiektów blob platformy Azure aktualizowany co tydzień.

    {
      "name": "AzureBlobInputWeekly",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 7
        }
      }
    }

**Wynik: Obiektów blob platformy Azure**

Jeden plik docelowy jest tworzony codziennie w folderze w danym dniu. Dostępność danych wyjściowych jest ustawiona na **dzień** (częstotliwość: dzień, interwał: 1).

    {
      "name": "AzureBlobOutputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Aktywność: aktywność gałęzi w potoku**

Działanie gałęzi trwa dwóch danych wejściowych i tworzy wycinek wynik każdego dnia. Możesz określić wycinka wynik każdego dnia do zależą od wprowadzania wycinek w poprzednim tygodniu tygodniowy wejścia w następujący sposób.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"hive activity",
        "activities": [
          {
            "name": "SampleHiveActivity",
            "inputs": [
              {
                "name": "AzureBlobInputDaily"
              },
              {
                "name": "AzureBlobInputWeekly",
                "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
                "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutputDaily"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
              "scriptPath": "adftutorial\\hivequery.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                "Month": "$$Text.Format('{0:%M}',WindowStart)",
                "Day": "$$Text.Format('{0:%d}',WindowStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            },          
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 2,  
              "timeout": "01:00:00"
            }
           }
         ]
       }
    }


## <a name="data-factory-functions-and-system-variables"></a>Funkcje Factory danych i zmienne systemowe   

Zobacz [Funkcje Factory danych i zmienne systemu](data-factory-functions-variables.md) listę funkcji i zmiennych systemu, obsługiwanych Factory danych.

## <a name="data-dependency-deep-dive"></a>Dive głębokości współzależności danych

Aby wygenerować wycinek zestawu danych, uruchom aktywności, Factory danych użyto następujących *modelu współzależność* , aby ustalić relacje między zestawy danych zużyte i tworzone przez działanie.

Zakres czasu wprowadzania zestawy danych wymagane do wygenerowania wycinek zestawu danych dane wyjściowe nosi nazwę *współzależności okresu*.

Uruchom aktywności generuje wycinek zestawu danych tylko wtedy, gdy fragmenty danych w zestawach wprowadzania danych w okresie współzależność są dostępne. Innymi słowy Uruchom wszystkie wycinki wprowadzania obejmujący okres współzależności muszą być w stanie **gotowy** działania da wynik wycinek zestawu danych.

Aby wygenerować wycinek zestawu danych [**start**, **zakończenia**], funkcja musi zamapować wycinek zestawu danych na okresie jej zależności. Ta funkcja jest zasadniczo formułę, która konwertuje początek i koniec wycinek zestawu danych na początek i koniec okresu zależności. Bardziej formalnego:

    DatasetSlice = [start, end]
    DependecyPeriod = [f(start, end), g(start, end)]

**F** i **g** mapowanie funkcje obliczające początek i koniec okresu współzależność dla każdej czynności wprowadzania.

Jak widać w próbkach, okresu zależności jest taki sam jak termin wycinek, który jest. W tych przypadkach Factory danych automatycznie oblicza wprowadzania wycinki, które wykraczają w okresie zależności.  

Na przykład w próbce agregacji, której codziennie powstaje dane wyjściowe i danych wejściowych jest dostępny co godzinę, okres wycinek danych jest 24 godziny. Factory danych umożliwia znalezienie wyrazów, odpowiednie dane wejściowe godzinowe plasterki dla tego okresu i sprawia, że wycinek wynik jest zależne od wprowadzania wycinek.

Możesz także podać mapowanie okres współzależności, jak pokazano w próbce, gdzie jest jednego z wejść tygodniowym i wycinek dane wyjściowe powstaje codziennie.

## <a name="data-dependency-and-validation"></a>Zależności danych i sprawdzanie poprawności

Zestaw danych może zawierać zdefiniowanych zasad sprawdzania poprawności określająca sposób dane wygenerowane przez wykonanie wycinek poprawność można sprawdzić przed jest teraz gotowy do zużycie. Aby uzyskać szczegółowe informacje, zobacz [Tworzenie zestawów danych](data-factory-create-datasets.md) .

W takich przypadkach po zakończeniu wykonywania wycinek stan wycinek wyjścia będzie zmieniać **Trwa oczekiwanie** podstanu **sprawdzania poprawności**. Po zatwierdzeniu wycinki stan wycinek zmieni się na **Gotowe**.

Jeśli wycinek wyprodukowane, ale niepomyślnego sprawdzania poprawności, zostanie uruchomiony aktywności dla wycinki podrzędne, które zależą od tego wycinek nie są przetwarzane.

[Monitor i zarządzanie nimi procesy](data-factory-monitor-manage-pipelines.md) obejmuje różne stany danych wycinki Factory danych.

## <a name="external-data"></a>Dane zewnętrzne

Zestaw danych może zostać oznaczona jako zewnętrzne (jak pokazano w poniższej wstawek JSON), co oznacza, że nie został wygenerowany z Factory danych. W takim przypadku zasad zestawu danych ma dodatkowy zestaw parametrów opisem sprawdzania poprawności i spróbuj ponownie zasad dla zestawu danych. Zobacz [Tworzenie procesy](data-factory-create-pipelines.md) opis wszystkich właściwości.

Podobnie jak zestawy danych, które są tworzone przez Factory danych, fragmenty danych dla danych zewnętrznych, muszą być gotowe w celu przetworzenia wycinków zależne.

    {
        "name": "AzureSqlInput",
        "properties":
        {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties":
            {
                "tableName": "MyTable"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1     
            },
            "external": true,
            "policy":
            {
                "externalData":
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }  
        }
    }


## <a name="onetime-pipeline"></a>Jednorazowe planowana
Możesz tworzyć i planowanie potok, aby uruchomić okresowo (na przykład: co godzinę lub codziennie) w ciągu godziny rozpoczęcia i zakończenia określ w definicji procesu. Aby uzyskać szczegółowe informacje, zobacz [Planowanie działań](#scheduling-and-execution) . Możesz również utworzyć potok jest wykonywane tylko raz. W tym celu ustawieniu właściwości **pipelineMode** w definicji planowana do **jednorazowe** jak pokazano w poniższym przykładzie JSON. Wartość domyślna tej właściwości jest **Zaplanowane**.

    {
        "name": "CopyPipeline",
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
                            "name": "InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ]
                    "name": "CopyActivity-0"
                }
            ]
            "pipelineMode": "OneTime"
        }
    }

Pamiętaj o następujących kwestiach:

- Nie określono godzin **Początek** i **koniec** procesu.
- **Dostępność** wejściowe i wyjściowe zestawy danych jest określony (**Częstotliwość** i **Interwał**), nawet jeśli nie korzysta z danych Factory wartości.  
- Widok diagramu nie pokazuje jednorazowego procesy. To zachowanie jest zgodne z projektem.
- Nie można zaktualizować jednorazowego procesy. Można klonowanie potok jednorazowego, zmienić jego nazwę, właściwości i wdrożyć go, aby utworzyć nowe.
