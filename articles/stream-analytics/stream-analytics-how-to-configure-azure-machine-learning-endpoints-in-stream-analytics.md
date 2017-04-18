<properties 
    pageTitle="Jak skonfigurować Azure maszynowego uczenia punkty końcowe w analizy strumieniu | Microsoft Azure" 
    description="Funkcje zdefiniowane przez użytkownika w języku maszynowym do analizy strumieniu"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"
/>

# <a name="machine-learning-integration-in-stream-analytics"></a>Integracja nauki w analizy strumieniu komputera

Analizy strumieniu obsługuje funkcje zdefiniowane przez użytkownika, które wyróżnienia do nauki maszynowego Azure punktów końcowych. Interfejsu API usługi REST obsługę tej funkcji jest szczegółowe w [bibliotece interfejsu API usługi REST analizy strumieniu](https://msdn.microsoft.com/library/azure/dn835031.aspx). Ten artykuł zawiera dodatkowe informacje potrzebne do pomyślnego zakończenia tę funkcję w analizy strumieniu. Samouczek także zostały opublikowane i jest dostępna [w tym miejscu](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Omówienie: Azure maszynowego uczenia terminologia

Microsoft Azure maszynowego uczenia jest dostępne narzędzie wspólnej, przeciągnij i upuść używanych do tworzenia, testowanie i wdrażanie rozwiązań przewidywanych analizy warunkowej danych. To narzędzie nosi nazwę *Azure maszynowego uczenia Studio*. Studio umożliwia interakcję z komputera szkoleniowe i łatwe tworzenie, testowanie i przejść w projekcie. Te zasoby i definicje są poniżej.

- **Obszar roboczy**: *obszar roboczy* jest kontener inne zasoby szkoleniowe maszynowego razem w kontenerze zarządzania i kontroli.
- **Doświadczenia**: *doświadczeń* są tworzone przez ekspertów naukowych danych, aby korzystać zestawy danych i przeszkolenie modelu nauki komputera.
- **Punkt końcowy**: *punkty końcowe* są obiekt nauki maszynowego Azure umożliwia wybranie funkcji jako danych wejściowych, model nauki określonego komputera i zwrócić uzyskał wynik.
- **Webservice wyników**: *wyników webservice* to zbiór punkty końcowe, jak powyżej.

Każdy punkt końcowy ma interfejsy API wsadowe i wykonanie synchroniczne. Analizy strumieniu używa wykonanie synchroniczne. Określonej usługi nosi nazwę [Usługi żądanie/odpowiedź](../machine-learning/machine-learning-consume-web-services.md#request-response-service-rrs) AzureML studio.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Maszynowego uczenia zasobów potrzebnych do analizy strumieniu zadań

Na potrzeby analizy strumieniu przetwarzanie zadania, punkt końcowy żądanie/odpowiedź, [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md#get-an-azure-machine-learning-authorization-key)i definicji swagger są wszystkie niezbędne do pomyślnego wykonania. Analizy strumieniu zawiera dodatkowe punktu końcowego, tworzy adres url punktu końcowego swagger, wyszukuje interfejsie i zwraca domyślną definicją UDF do użytkownika.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Konfigurowanie analizy strumieniu i maszynowego uczenia UDF za pośrednictwem interfejsu API usługi REST

Za pomocą interfejsów API pozostałych można skonfigurować zadania połączenie funkcji języka komputera Azure. Dostępne są następujące czynności:

1. Utwórz zadanie analizy strumieniu
2. Definiowanie danych wejściowych
3. Definiowanie wynik
4. Tworzenie funkcji zdefiniowanych przez użytkownika (UDF)
5. Pisanie przekształcenia analizy strumieniu wywołującego UDF
6. Rozpoczynanie zadania

## <a name="creating-a-udf-with-basic-properties"></a>Tworzenie UDF za pomocą podstawowe właściwości

Na przykład następujący kod tworzy skalarne UDF, o nazwie *newudf* , która wiąże punktu końcowego Azure maszynowego uczenia. Należy zauważyć, że punkt *końcowy* (usługa Identifier) można znaleźć na stronie pomocy interfejsu API dla wybranej usługi i *apiKey* można znaleźć na stronie głównej usługi.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Przykład treść żądania:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Punkt końcowy RetrieveDefaultDefinition połączenia dla domyślnych UDF

Utworzone szkielet UDF pełną definicji UDF jest potrzebny. Punkt końcowy RetreiveDefaultDefinition ułatwia uzyskiwanie domyślnej definicji dla skalarnej powiązanej z punktu końcowego Azure maszynowego uczenia. Poniżej ładunku wymaga uzyskiwanie domyślnej definicji UDF dla skalarnej powiązanej z punktu końcowego Azure maszynowego uczenia. Punkt końcowy rzeczywisty go nie określić, jak zostały już dostarczone podczas żądania położenie. Analizy strumieniu połączeń punkt końcowy dostarczony z żądaniem, jeśli jest dostępna bezpośrednio. W przeciwnym razie używa ten, który został pierwotnie przez odwołanie. W tym miejscu ma UDF jeden ciąg parametru (zdanie) i zwraca pojedynczy wyjścia typu ciąg, co oznacza etykietę "upodobania" zdanie to.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Przykład treść żądania:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

Przykładowe dane wyjściowe tego będzie wyglądać na przykład poniżej.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-the-response"></a>Poprawka UDF z odpowiedzią 

Teraz UDF musi poprawkami z poprzedniej odpowiedzi, tak jak pokazano poniżej.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

Treść żądania (wyjście z RetrieveDefaultDefinition):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-to-call-the-udf"></a>Implementowanie transformacja analizy strumieniu połączenie UDF

Teraz kwerendy UDF (w tym miejscu o nazwie scoreTweet) dla każdej wprowadzania zdarzenia i pisanie odpowiedzi na to zdarzenie do wyników.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)
