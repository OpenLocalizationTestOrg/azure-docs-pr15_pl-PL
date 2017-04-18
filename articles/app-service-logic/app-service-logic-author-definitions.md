<properties 
    pageTitle="Tworzenie definicji aplikacji logika | Microsoft Azure" 
    description="Dowiedz się, jak napisać definicję JSON logiki aplikacji" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="author-logic-app-definitions"></a>Definicje aplikacji logika autora
W tym temacie przedstawiono sposób użycia definicji [Aplikacji logika Azure](app-service-logic-what-are-logic-apps.md) , który jest proste, deklaracyjnych językiem JSON. Jeśli nie zostało to zrobione jeszcze, zapoznaj się z [umożliwiające utworzenie nowej aplikacji logika](app-service-logic-create-a-logic-app.md) najpierw. Można również przeczytać [materiały źródłowe pełny języka definicji w witrynie MSDN](http://aka.ms/logicappsdocs).

## <a name="several-steps-that-repeat-over-a-list"></a>Kilka kroków, które powtarzają listy

Można wykorzystać [Typ foreach](app-service-logic-loops-and-scopes.md) Powtórz na tablicy 10 k elementów i wykonywanie akcji dla każdej z nich.

## <a name="a-failure-handling-step-if-something-goes-wrong"></a>Jeśli wystąpią problemy kroku błąd obsługi

Chcesz mieć możliwość zapisania *kroku korygowania* często — logikę wykonywana, jeśli **i tylko wtedy, gdy**, jednej lub większej liczby połączeń nie powiodła się. W tym przykładzie są możemy pobieranie danych z różnych miejsc, ale jeśli połączenie nie powiedzie się, mają zostać gdzieś publikowanie wiadomości, można śledzić w dół awarii później:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

Umożliwia wykorzystanie `runAfter` właściwości w celu określenia `postToErrorMessageQueue` należy tylko wtedy, gdy `readData` to **nie powiodło się**.  Przyczyną może być również listę możliwych wartości, więc `runAfter` może być `["Succeeded", "Failed"]`.

Na końcu ponieważ mają teraz obsługiwane komunikat o błędzie, firma Microsoft nie jest już oznaczenie Uruchom **nie powiodło się**. Jak widać tutaj, tego uruchomienia jest **powiodło się** chociaż jeden krok nie powiodło się, ponieważ napisane krok do obsługi tego błędu.

## <a name="two-or-more-steps-that-execute-in-parallel"></a>Kroki dwóch (lub więcej), które wykonywanie równolegle

Ma wiele wykonanie akcji równolegle, `runAfter` właściwość muszą być równoważne w czasie rzeczywistym. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

Jak widać w przykładzie powyżej oba `branch1` i `branch2` są ustawione na uruchamianie `readData`. W wyniku obu tych oddziałów zostanie uruchomione równolegle:

![Równolegle](./media/app-service-logic-author-definitions/parallel.png)

Widać, że sygnatura czasowa dla obu gałęzi jest identyczny. 

## <a name="join-two-parallel-branches"></a>Dołączanie do dwóch Gałęzie równoległe

Można dołączać do dwóch akcje, które zostały ustawione na wykonanie równolegle, dodając elementy do `runAfter` właściwość podobne do powyżej.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![Równolegle](./media/app-service-logic-author-definitions/join.png)

## <a name="mapping-items-in-a-list-to-some-different-configuration"></a>Mapowanie elementów na liście kilka różnych konfiguracji

Następny, załóżmy, że chcemy ułatwić całkowicie inną zawartość, w zależności od wartości właściwości. Możemy utworzyć mapę wartości do miejsc docelowych jako parametr:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

W tym przypadku możemy najpierw uzyskać listę artykułów, a następnie drugim krokiem wyszukuje na mapie według kategorii, którą zdefiniowano jako parametr adres URL, który umożliwia otrzymywanie treści z. 

Dwa elementy, które należy zwrócić uwagę tutaj: [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) funkcja służy do sprawdzenia, czy kategoria odpowiada jednej z kategorii znanych zdefiniowane. Drugi, gdy będziemy korzystać z kategorii, firma Microsoft mogą pobierać element planu przy użyciu nawiasów kwadratowych: `parameters[...]`. 

## <a name="working-with-strings"></a>Praca z ciągów

Istnieją różne funkcje, które mogą być używane do manipulowania ciągu. Spójrzmy na przykład gdzie mamy ciąg, który chcemy przekazać do systemu, ale nie możemy pewność, że kodowania znaków będzie obsługiwany poprawnie. Jedną z opcji jest base64 kodowanie tego ciągu. Jednak aby uniknąć użycie w adresu URL chwilę Aby zamienić kilka znaków. 

Chcemy ciąg z zamówienia nazwy, ponieważ nie są używane pierwszych 5 znaków.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

Praca z inside out:

1. Uzyskiwanie [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) nazwy osoby zamawiającej ponownie zwraca to całkowita liczba znaków

2. Odejmowanie 5 (ponieważ chcemy krótszej ciągu)

3. W rzeczywistości prowadzą [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring) . Firma Microsoft rozpoczynają się indeks `5` i przejdź do końca ciągu.

4. Konwertowanie tego podciągiem do [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) ciąg

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)wszystkie `+` znaków z`-`

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)wszystkie `/` znaków z`_`

## <a name="working-with-date-times"></a>Praca z daty i godziny

Daty i godziny może być przydatne, szczególnie w przypadku, gdy próbujesz pobierają dane ze źródła danych, która nie obsługuje naturally **wyzwalaczy**.  Ustalanie, jak długo trwa różne kroki wykonywaną umożliwia także daty i godziny. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{parameters('order').id}"
            }
        },
        "timingWarning": {
            "actions" {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
                },
                "runAfter": {}
            }
            "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
        }
    },
    "outputs": {}
}
```

W tym przykładzie możemy wyodrębnianie `startTime` poprzednim kroku. Następnie jesteśmy wprowadzenie bieżącą godzinę i odjęcie jedną sekundę:[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (można użyć innych jednostek czasu, takich jak `minutes` lub `hours`). Na koniec możemy porównanie tych dwóch wartości. Jeśli pierwszy jest mniejsza niż drugi, a następnie to oznacza więcej niż jednej sekundy upłynął od kolejności po raz pierwszy umieścić. 

Należy także zauważyć, że firma Microsoft służące do formatowania daty formatters ciąg: w ciągu kwerendy używać [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow) uzyskanie RFC1123. Wszystkie daty formatowania [opisano w witrynie MSDN](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow). 

## <a name="using-deployment-time-parameters-for-different-environments"></a>Zgodnie z parametrami czas wdrażania w różnych środowiskach

Są często mają cyklu wdrażania, w którym znajduje się środowisko projektowania, środowisku wzorcowym, a następnie środowisku produkcyjnym. We wszystkich tych może mają tę samą definicję, ale użyj innej bazy danych, na przykład. Analogicznie można użyć tę samą definicję przez wiele różnych regionów wysokiej dostępności, ale chcesz każdego wystąpienia aplikacji logiczny, aby porozmawiać z tego regionu bazy danych. 

Zauważ, że to jest inna niż określone różnych parametrów w *czasie wykonywania*, że należy używać `trigger()` działa zgodnie z wymienionym powyżej. 

Możesz rozpocząć z definicją simplistic bardzo podobny do tego:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

Następnie w rzeczywistych `PUT` żądanie aplikacji logika podasz parametru `uri`. Zwróć uwagę, ile jest już wartością domyślną, które ten parametr jest wymagany w ładunku aplikacji logika:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

W każdym środowisku możesz także podać inną wartość `connection` parametru. 

Można znaleźć w [dokumentacji interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/mt643787.aspx) wszystkie opcje, których masz do tworzenia i zarządzania aplikacjami logiczny. 
