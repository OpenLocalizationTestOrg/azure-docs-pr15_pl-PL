<properties 
    pageTitle="Nowy schemat wersji 2015-08-01-Podgląd" 
    description="Dowiedz się, jak napisać definicji JSON najnowszą wersję aplikacji warunków logicznych" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>
    
# <a name="new-schema-version-2015-08-01-preview"></a>Nowy schemat wersji 2015-08-01-Podgląd

Nowy schemat i interfejsu API wersji dla aplikacji logika ma wiele udoskonaleń, które poprawić niezawodność i ułatwienia użycia logiki aplikacji. Istnieje 4 kluczowych różnic:

1. Typ akcji **APIApp** została zaktualizowana na nowy typ akcji **APIConnection** .
2. **Powtórz** została zmieniona na **Foreach**.
3. Aplikacja interfejsu API **Odbiornik HTTP** nie jest wymagane.
4. Wywoływanie podrzędne przepływy pracy używa nowy schemat.

## <a name="1-moving-to-api-connections"></a>1. przechodzenie do połączenia interfejsu API

Zmiana największych jest, że nie potrzebujesz już do wdrażania aplikacji interfejsu API do subskrypcję Azure za pomocą interfejsu API. Istnieją 2 sposoby używania interfejsów API:
* Zarządzany interfejs API
* Usługi niestandardowe API sieci Web

Każdy z tych przebiega nieco inaczej, ponieważ są różne hostingu modeli i zarządzania nimi. Jedną z zalet modelu jest już nie jest już ograniczona do zasobów, które są wdrożone w grupie zasobów. 

### <a name="managed-apis"></a>Interfejsy API zarządzanych

Istnieje szereg interfejsu API zarządzanych przez firmę Microsoft w Twoim imieniu, takich jak usługi Office 365, usługi Salesforce, Twitter, FTP itd... Niektóre z tych zarządzanego interfejsu API może być używany jako-jest, takich jak Bing przetłumaczyć, a inne wymagają konfiguracji. Ta konfiguracja nosi nazwę *połączenia*.

Na przykład gdy używasz usługi Office 365, należy utworzyć połączenie, która zawiera token logowania usługi Office 365. Ten token są bezpieczne przechowywane i odświeżeniu tak, aby aplikacji logika zawsze można zadzwonić API usługi Office 365. Można też kliknąć aby połączyć się z serwerem bazy danych SQL lub FTP, należy utworzyć połączenia, którego parametry połączenia. 

Wewnątrz definicji te akcje są nazywane `APIConnection`. Oto przykład połączenia, który wymaga usługi Office 365, aby wysłać wiadomość e-mail:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Część danych wejściowych, który jest unikatowy dla połączenia interfejsu API jest `host` obiektu. Ta strona zawiera dwie części: `api` i `connection`.

`api` Zawiera środowisko uruchomieniowe, gdzie który zarządzany interfejs API adres URL jest obsługiwana. Można wyświetlić wszystkie dostępne zarządzanych interfejsów API dla Ciebie, dzwoniąc `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Użycie interfejsu API, może być lub może nie być zdefiniowane **Parametry połączenia** . Jeśli nie ma **połączenia** jest wymagane. Jeśli tak, następnie należy utworzyć połączenie. Po utworzeniu to połączenie będzie mieć nazwę, możesz wybrać, a następnie odwołać się w `connection` obiekt wewnątrz `host` obiektu. Aby utworzyć połączenie w grupie zasobów, zadzwoń:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Jednostki następujące czynności:


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>Wdrożenie zarządzanych interfejsy API w szablonie Menedżera zasobów Azure

Możesz utworzyć pełne zastosowanie w szablonie ARM, dopóki nie wymaga interakcyjnego logowania. Jeśli wymaga logowania, można skonfigurować wszystko za pomocą szablonu ARM, ale nadal będą musieli odwiedź portal Aby autoryzować połączenia. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Widać, w tym przykładzie, że połączenia są tylko normalny zasoby, które są przechowywane w swojej grupy zasobów. Odwołują managedAPIs dla Ciebie dostępne w ramach subskrypcji.

### <a name="your-custom-web-apis"></a>Usługi niestandardowe interfejsy API sieci Web

Jeśli używasz własnego interfejsu API (w szczególności nie zarządzany przez firmę Microsoft z nich), należy użyć wbudowanej akcji **HTTP** ich połączenie. W celu obsługi idealne rozwiązanie należy uwidaczniają punkt końcowy swagger dla swojego interfejsu API. Spowoduje to włączenie projektanta aplikacji logiki do renderowania wejścia i wyjścia dla interfejsu API usługi. Bez swagger projektanta tylko będzie pokazanie wejścia i wyjścia jako nieprzezroczysty obiekty JSON.

Oto przykład z nowymi `metadata.apiDefinitionUrl` właściwości:
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

Jeśli obsługuje interfejs API usługi sieci Web w **Aplikacji usługi** , a następnie go będą automatycznie wyświetlane na liście akcji dostępnych w projektancie. Jeśli nie, konieczne będzie Wklej adres URL bezpośrednio. Punkt końcowy swagger musi nieuwierzytelniony było można używać wewnątrz projektanta aplikacji logiczny (chociaż może być bezpieczna API z niezależnie od metody są obsługiwane w Swagger).

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>Używanie już wdrożonej aplikacji interfejsu API z 2015-08-01-Podgląd

Jeśli wcześniej wdrożono aplikację interfejsu API, można go wywołać przy użyciu akcji **HTTP** .

Na przykład jeśli używasz usługi Dropbox do listy plików, masz mniej więcej tak w Twojej wersji definicja schematu **2014-12-01-podglądu** :

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Można utworzyć HTTP akcje takie jak poniżej (część parametrów pozostaje definicji aplikacji logika bez zmian):

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Instruktaż tych właściwości po kolei:

| Właściwości akcji |  Opis |
| --------------- | -----------  |
| `type` | `Http`Zamiast`APIapp` |
| `metadata.apiDefinitionUrl` | Jeśli chcesz za pomocą tej akcji w Projektancie aplikacji logika chcesz uwzględnić punkt końcowy metadanych. To jest tworzona z:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | To jest tworzona z:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Zawsze`POST` |
| `inputs.body` | Identyczne parametry aplikacji interfejsu api | 
| `inputs.authentication` | Identyczny z interfejsu api uwierzytelniania aplikacji |

Ta metoda powinna działać dla wszystkich akcji aplikacji interfejsu API. Jednak pamiętaj o Pamiętaj, że te aplikacje poprzedniego interfejsu API nie są już obsługiwane i należy przenieść do jednego z dwóch pozostałych opcji powyżej (zarządzanego interfejsu API lub hostingu niestandardowy interfejs API sieci Web).

## <a name="2-repeat-renamed-to-foreach"></a>2. zmieniona na Foreach powtarzanie

Poprzedniej wersji schematu Otrzymaliśmy wiele opinii klientów tego **Powtórz** została skomplikowana i nie został poprawnie przechwycić naprawdę została dla każdej pętli. Dzięki temu możemy zmieniono go do **Foreach**. Na przykład:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Teraz powinny być zapisane jako:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Wcześniej funkcję `@repeatItem()` został użyty do bieżącego elementu jest powtórzyć przez odwołanie. To został uproszczony nieco `@item()`. 

### <a name="referencing-the-outputs-of-the-foreach"></a>Odwoływanie się do wydajność Foreach
Aby jeszcze bardziej uprościć wyjściowe akcji **Foreach** nie będą zawijane w obiekcie o nazwie **repeatItems**. Oznacza to, były wydajność Powtórz powyższe:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Teraz będzie:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Przy odwoływaniu się do tych wyników, aby przejść do treści akcji wymaga zrobić:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

Teraz możesz wykonać zamiast tego:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

Wprowadzone zmiany funkcji `@repeatItem()`, `@repeatBody()` i `@repeatOutputs()` zostaną usunięte.

## <a name="3-native-http-listener"></a>3. natywnych odbiornik HTTP 
Możliwości odbiornik HTTP są teraz wbudowane, więc nie trzeba wdrożyć aplikację interfejsu API odbiornika HTTP. Przeczytaj o [Pełne szczegóły dotyczące tutaj wprowadź punkt końcowy aplikacji logika żądanie](app-service-logic-http-endpoint.md). 

Wprowadzone zmiany, funkcja `@accessKeys()` jest usuwana i zamieniono `@listCallbackURL()` funkcji na potrzeby uzyskiwania punkt końcowy (w razie potrzeby). Ponadto teraz należy zdefiniować co najmniej jeden wyzwalacz w aplikacji logika teraz. Jeśli chcesz `/run` przepływ pracy, musisz dysponować jedną z `manual`, `apiConnectionWebhook` lub `httpWebhook` wyzwalaczy. 

## <a name="4-calling-child-workflows"></a>4. podrzędne przepływy pracy dzwonisz

Wcześniej dzwoniąc podrzędne przepływy pracy wymagane, przechodząc do danego przepływu pracy, wprowadzenie token dostępu i wklejania w definicji aplikacji logiczny, którą chcesz nawiązać połączenie dzieckiem. W nowej wersji schematu aparat aplikacje logiczny automatycznie generuje skojarzeń zabezpieczeń w czasie wykonywania podrzędnego przepływu pracy, co oznacza, że nie masz wklejanie wszelkie hasła do definicji.  Oto przykład:

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

Druga poprawy jakości jest firma Microsoft będzie pokazywania podrzędne przepływy pracy pełny dostęp do przychodzące żądanie. Oznacza to, że można przekazać parametry w sekcji *kwerend* i w obiekcie *nagłówki* i że można zdefiniować w pełni cały.

Na koniec są wymagane zmiany podrzędny przepływ pracy. Przed użytkownik może po prostu wymagają podrzędny przepływ pracy bezpośrednio; Teraz musisz określić punkt końcowy wyzwalacza w ramach przepływu pracy dla nadrzędnej nawiązać połączenie. Zazwyczaj oznacza to, będzie dodać wyzwalacz typu **ręcznego** , a następnie użyj który w definicji nadrzędnej. Należy zauważyć, że `host` właściwość specjalnie ma `triggerName`, ponieważ wyzwalacza, która jest zawsze należy określić.

## <a name="other-changes"></a>Inne zmiany

### <a name="new-queries-property"></a>Nowa właściwość kwerendy
Wszystkie typy akcji obsługuje teraz nowe dane wejściowe o nazwie **zapytania**. Może to być strukturalnych obiektu zamiast konieczności zebrania ciąg ręcznie.

### <a name="parse-function-renamed"></a>Funkcja Parse(), której nazwę zmieniono
Jak możemy będziesz szybko dodawać więcej typów zawartości, `parse()` funkcja została zmieniona na `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Wkrótce: interfejsy API integracji przedsiębiorstwa
W tym momencie firma Microsoft nie jeszcze udało wersje interfejsów API integracji przedsiębiorstwa, które są dostępne (na przykład AS2). Te będzie można już wkrótce jak to opisano w [Przewodnik](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). W międzyczasie możesz użyć API BizTalk istniejących wdrożonym, przy użyciu akcji HTTP objętych powyżej w "przy użyciu już wdrożonej aplikacji interfejsu API."
