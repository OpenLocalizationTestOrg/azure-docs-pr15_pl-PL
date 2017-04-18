<properties
   pageTitle="Obsługa wyjątków aplikacji logika | Microsoft Azure"
   description="Dowiedz się, deseni dla komunikaty o błędach i obsługi z aplikacjami logiki Azure wyjątków"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-error-and-exception-handling"></a>Błąd logiczny aplikacji i obsługi wyjątków

Logika aplikacje zawiera bogatego zestawu narzędzi i desenie, aby zapewnić usługi integracji są niezawodne i podatne na błędy.  Jedną z problemach z dowolnej architekturze integracji zapewnienie, właściwej obsługi przestoje lub problemów systemów zależne.  Logika aplikacji służy do obsługi błędów pierwszej klasie środowisko, które zapewnia narzędzia potrzebne do działania na wyjątki i błędów przepływów pracy.

## <a name="retry-policies"></a>Spróbuj ponownie zasad

Najprostszy typ wyjątku i obsługi błędów jest zasadę ponów próbę.  Ta zasada Określa, czy akcja należy ponowić próbę żądania początkowego Przekroczono limit czasu lub nie powiodło się (każdego wezwania, które spowodowały 429 lub odpowiedź 5xx).  Domyślnie wszystkie akcje ponowienia 4 dodatkowe razy w odstępach 20-sekundowe.  Tak, jeśli otrzymano pierwszego żądania `500 Internal Server Error` odpowiedzi, aparat przepływu pracy można wstrzymać 20 sekund i próba żądanie ponownie.  Jeśli wszystkie ponowne próby odpowiedzi jest nadal wyjątku lub błąd, przepływ pracy będzie kontynuować i oznaczenia stanu akcji jako `Failed`.

Można skonfigurować zasady ponów próbę w **danych wejściowych** dla określonych akcji.  Zasady ponów próbę można skonfigurować wypróbować interwały maksymalnie 4 godzin na 1 godzinę.  Szczegółowe informacje o właściwościach wprowadzania można [znaleźć w witrynie MSDN][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Jeśli chcesz utworzyć tę czynność HTTP, aby ponowić próbę 4 godziny i poczekaj 10 minut między każdej próbie wymaga definicję następujące czynności:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Więcej informacji o obsługiwanych składni, można wyświetlić w [sekcji zasady ponów próbę w witrynie MSDN][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>Właściwość RunAfter do efektywnej błędy

Każdej akcji aplikacji logiki oświadcza, które akcje należy wykonać, zanim uruchomi akcję.  To można traktować jako kolejnością kroków w przepływie pracy.  Ta kolejność nosi nazwę `runAfter` właściwość w definicji akcji.  Istnieje obiekt, który w tym artykule opisano, które akcje i stany akcji czy wykonywanie akcji.  Domyślnie wszystkie akcje dodane przez projektanta są ustawione na wartość `runAfter` poprzedni krok, jeśli został poprzedni krok `Succeeded`.  Jednak można dostosować tę wartość na uruchomienie akcji po poprzedniej akcji `Failed`, `Skipped`, lub możliwe zestawu tych wartości.  Jeśli chcesz dodać element do wyznaczonego tematu Bus usługi po określonej akcji `Insert_Row` nie powiedzie się, można użyć następujących `runAfter` konfiguracji:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Powiadomienie o `runAfter` właściwość jest ustawiona na Jeśli `Insert_Row` akcja jest `Failed`.  Aby uruchomić akcję, jeśli jest stan akcji `Succeeded`, `Failed`, lub `Skipped` będzie składni:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

>[AZURE.TIP] Akcje, które są uruchamiane po poprzedniej akcji nie powiodło i zakończone pomyślnie, zostaną oznaczone jako `Succeeded`.  To zachowanie oznacza, że jeśli zostanie pomyślnie efektywnej wszystkie błędy w przepływie pracy Uruchom sam jest oznaczony jako `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Zakresy i wyniki oceny akcje

Podobnie jak można uruchamiać po poszczególnych akcji, można też akcje grupy razem wewnątrz [zakresu](app-service-logic-loops-and-scopes.md) -, które działają jak logiczna grupa akcje.  Zakresy są przydatne, zarówno w organizowaniu czynności aplikacji logicznych, jak i do wykonywania agregacji ocen stanu zakresu.  Po zakończeniu wszystkich działań w zakresie, sam zakres otrzymają stan.  Ustalić stanu zakres z zachowaniem kryteriów jako Uruchom — w przypadku ostateczne działania w gałęzi wykonanie `Failed` lub `Aborted` jej stan to `Failed`.

Możesz `runAfter` został oznaczony jako zakres `Failed` uruchomienie określonych akcji dla wszystkie błędy, które wystąpiły w zakresie.  Po zakres kończy się niepowodzeniem, umożliwia tworzenie jednej akcji do efektywnej błędy, jeśli nie *Wszystkie* akcje w zakresie.

### <a name="getting-the-context-of-failures-with-results"></a>Wprowadzenie kontekście błędy wyników

Przechwytywanie błędy z zakresu jest bardzo przydatne, ale możesz także kontekst, aby dowiedzieć się, które dokładnie akcje nie powiodło się i ewentualne błędy lub kody stanu, które zostały zwrócone.  `@result()` Funkcji przepływu pracy zawiera kontekstu w wyniku wszystkie akcje w ramach zakresu.

`@result()`przyjmuje jeden parametr, nazwa zakresu, a następnie zwraca tablicę wyników wszystkich akcji z tego zakresu.  Przykładami tych obiektów akcji są takie same atrybuty jak `@actions()` wyświetla obiektu, w tym akcji czas rozpoczęcia, czas zakończenia akcji, stan akcji, wartości wejściowych akcji, korelacji akcji identyfikatorów i akcji.  Można łatwo skojarzyć `@result()` działać z `runAfter` do wysłania kontekście akcji, które nie powiodło się w zakresie.

Jeśli chcesz wykonać czynności *dla każdej* akcji w zakresie którego `Failed`, można skojarzyć `@result()` o działaniu **[Tablicy filtrów](../connectors/connectors-native-query.md)** i pętli **[ForEach](app-service-logic-loops-and-scopes.md)** .  Umożliwia filtrowanie tablicę wyników, aby akcje, które nie powiodło się.  Tablica wynik filtrowania, można wykonywać akcję dla każdej awarii przy użyciu pętli **ForEach** .  Oto przykład poniżej, a po nim szczegółowy opis.  W tym przykładzie wyśle żądania HTTP POST z treści odpowiedzi akcji, które nie powiodło się w zakresie `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Poniżej opisano, co się dzieje uzyskać szczegółowy opis:

1. Działania **Tablicy filtrów** w celu filtrowania `@result('My_Scope')` Aby wyświetlić wynik wszystkie akcje w ramach`My_Scope`
1. Warunek **Tablicy filtrów** jest dowolną `@result()` elementu z Stan równa się `Failed`.  Spowoduje to filtrowanie tablicy wszystkie wyniki akcji z `My_Scope` tablicy wyników akcji nie powiodło się.
1. Wykonywanie akcji **dla każdej z nich** na **Tablicy filtrowane** wyjściowe.  To zostanie wykonana akcja *dla każdej z nich* nie powiodło się akcji wynik możemy filtrowane powyżej.
    - Jeśli wystąpiło pojedyncza czynność w zakresie, który nie powiodło się, akcje w `foreach` można uruchomić tylko raz.  Wielu akcji nie powiodło się spowoduje akcji na błąd.
1. Wyślij HTTP POST na `foreach` element treść odpowiedzi lub `@item()['outputs']['body']`.  `@result()` Kształt element jest taka sama jak `@actions()` kształt, a następnie można przeanalizować tak samo.
1. Zawiera również dwa niestandardowe nagłówki o nazwie akcji nie powiodło się `@item()['name']` i nie powiodło się uruchamianie klienta identyfikator śledzenia `@item()['clientTrackingId']`.

Odwołanie, Oto przykład pojedynczego `@result()` elementu.  Widać `name`, `body`, i `clientTrackingId` właściwości przeanalizować w powyższym przykładzie.  Należy zauważyć, która poza `foreach`, `@result()` zwraca tablicę tych obiektów.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Powyższe wyrażenia umożliwia wykonywanie różnych obsługi desenie wyjątków.  Możesz wykonać jedynym wyjątkiem obsługi akcji poza zakresem akceptującym całą tablicę filtrowanych błędów i Usuń `foreach`.  Można także dodać inne przydatnych właściwości `@result()` odpowiedzi, jak pokazano powyżej.

## <a name="azure-diagnostics-and-telemetry"></a>Narzędzia diagnostyczne Azure i telemetrycznego

Desenie powyżej są doskonały sposób na obsługę błędów i wyjątki w serii, ale można także identyfikowanie i odpowiadanie na błędy niezależnie od Uruchom się.  [Diagnostyka Azure](app-service-logic-monitor-your-logic-apps.md) umożliwia łatwe wysyłanie wszystkie zdarzenia przepływu pracy (w tym wszystkie stany Uruchom i akcji) do konta magazynu platformy Azure lub koncentratora zdarzenia Azure.  Monitorowanie dzienników i wskaźniki lub publikować do dowolnego narzędzia monitorowania, preferowane, ma być obliczona statusy uruchamianie.  Jedna opcja potencjalne jest przesyłać strumieniowo wszystkie wydarzenia za pośrednictwem Centrum zdarzeń Azure do [Analizy strumieniu](https://azure.microsoft.com/services/stream-analytics/).  W analizy strumieniu można zapisywać live kwerendy poza dowolnego różnic w odniesieniu, średnie i niepowodzenia z dzienniki diagnostyczne.  Analizy strumieniu można łatwo wyjściowy z innymi źródłami danych, takich jak kolejki, tematy SQL, DocumentDB i Power BI.

## <a name="next-steps"></a>Następne kroki
- [Zobacz jak jeden klient wbudowany obsługi z aplikacjami logiczny niezawodne błędów](app-service-logic-scenario-error-and-exception-handling.md)
- [Znajdź więcej aplikacji logiki przykładów i scenariusze](app-service-logic-examples-and-scenarios.md)
- [Dowiedz się, jak utworzyć zautomatyzowanych wdrożeń logiki aplikacji](app-service-logic-create-deploy-template.md)
- [Projektowanie i wdrażanie aplikacji logika z programu Visual Studio](app-service-logic-deploy-from-vs.md)


<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9