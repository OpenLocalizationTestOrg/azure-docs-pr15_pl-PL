<properties
    pageTitle="Rejestrowanie i obsługi błędów w logiczny aplikacji | Microsoft Azure"
    description="Wyświetlanie przypadek użycia rzeczywistych zaawansowane Obsługa błędów i rejestrowania z aplikacjami warunków logicznych"
    keywords=""
    services="logic-apps"
    authors="hedidin"
    manager="anneta"
    editor=""
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="b-hoedid"/>

# <a name="logging-and-error-handling-in-logic-apps"></a>Rejestrowanie i obsługi błędów w logiczny aplikacji

W tym artykule opisano, jak można rozszerzyć aplikacji logika, aby lepiej obsługiwać wyjątek. Przypadek użycia rzeczywistych i naszych odpowiedzi na pytanie "Aplikacje logiczny obsługuje wyjątku i obsługi błędów?"

>[AZURE.NOTE]Bieżącą wersję funkcji logicznych aplikacji usługi aplikacji Microsoft Azure udostępnia standardowy szablon odpowiedzi akcji.
>Ta opcja uwzględnia zarówno wewnętrznych poprawności odpowiedzi i błędy zwrócone przez aplikację interfejsu API.

## <a name="overview-of-the-use-case-and-scenario"></a>Omówienie przypadków użycia i scenariusz

Następujący artykuł jest przypadków użycia, w tym artykule.
Dobrze znane organizacji ochrony zdrowia włączone nam opracowywaniu Azure rozwiązanie, które mogą utworzyć pacjentów portalu przy użyciu programu Microsoft Dynamics CRM Online. One potrzebne do wysłania rekordy kontaktów osobistych między Dynamics CRM Online portal pacjentów i usług Salesforce.  Firma Microsoft zostały monit o używanie standardu [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) dla wszystkich rekordów pacjentów.

Projektu zawiera dwie główne wymagania:  

 -  Metody logowania rekordów wysłanych z portalu Dynamics CRM Online
 -  Można wyświetlić wszystkie błędy, które wystąpiły w ramach przepływu pracy


## <a name="how-we-solved-the-problem"></a>Jak możemy rozwiązać ten problem

>[AZURE.TIP] Możesz wyświetlać wideo wysokiego poziomu projektu [Grupy użytkowników integracji](http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "Integracji grupy użytkowników").

Wybraliśmy [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/ "Azure DocumentDB") jako repozytorium rekordów dziennika i błędów (DocumentDB odwołuje się do rekordów jako dokumenty). Aplikacje logiczny ma standardowy szablon wszystkie odpowiedzi, więc nie mamy utworzyć niestandardowy schemat. Firma Microsoft może utworzyć aplikację programu interfejsu API do **wstawiania** i **kwerendy** rekordów zarówno błędu i dziennika. Firma Microsoft może także zdefiniować schemat dla każdego poziomu aplikacji interfejsu API.  

Wymaganie innego było usunąć rekordy po określonej dacie. DocumentDB ma właściwość o nazwie [czas wygaśnięcia](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "czas wygaśnięcia") (TTL), który umożliwił nam ustaw wartość **czasu wygaśnięcia** dla każdego rekordu lub zbioru. To wyeliminować potrzebna, możesz ręcznie usunąć rekordy DocumentDB.

### <a name="creation-of-the-logic-app"></a>Tworzenie aplikacji warunków logicznych

Pierwszym krokiem jest tworzenie aplikacji logiki i załaduj go w projektancie. W tym przykładzie użyto aplikacji logika nadrzędny podrzędny. Załóżmy, możemy zostały już utworzone nadrzędny i aby utworzyć jedną aplikację logiczny podrzędne.

Ponieważ firma Microsoft mają być rejestrowanie rekord z Dynamics CRM Online, Zacznijmy u góry. Należy użyć wyzwalacza żądania, ponieważ aplikacji logika nadrzędnej wyzwalane ten podrzędny.

> [AZURE.IMPORTANT] Aby użyć tego samouczka, będzie konieczne tworzenie bazy danych DocumentDB i dwoma zbiorów (rejestrowanie i błędy).

### <a name="logic-app-trigger"></a>Logika aplikacji wyzwalacza

Użyto wyzwalacza żądanie jak pokazano w poniższym przykładzie.

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### <a name="steps"></a>Czynności

Trzeba rejestrować źródła (żądanie) rekordu pacjentów z portalu Dynamics CRM Online.

1. Trzeba uzyskać nowy rekord terminu z Dynamics CRM Online.
    Wyzwalacza pochodzące z CRM zapewnia **CRM PatentId**, **Typ rekordu**, **Nowy lub zaktualizowany rekord** (nowe lub zaktualizować wartość logiczna) i **SalesforceId**. **SalesforceId** może być null, ponieważ jest używany tylko do aktualizacji.
    Firma Microsoft otrzymają rekordu CRM za pomocą CRM **PatientID** i **Typ rekordu**.
1. Następnie należy dodać aplikację naszych interfejsu API DocumentDB operacji **InsertLogEntry** , jak pokazano na poniższej ilustracji.


#### <a name="insert-log-entry-designer-view"></a>Wstawianie Widok projektanta wpis dziennika

![Wstaw wpis dziennika](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### <a name="insert-error-entry-designer-view"></a>Wstawianie widoku Projektanta wprowadzania błędu
![Wstaw wpis dziennika](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### <a name="check-for-create-record-failure"></a>Sprawdź, czy utworzyć rekord błąd

![Warunek](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## <a name="logic-app-source-code"></a>Kod źródłowy aplikacji warunków logicznych

>[AZURE.NOTE]  Poniżej przedstawiono przykłady tylko. Ponieważ tego samouczka jest oparty na implementacja obecnie w produkcji, wartość **Węzeł źródła** mogą wyświetlać właściwości, które są związane z planowaniem terminu.

### <a name="logging"></a>Rejestrowanie
Poniższy przykład kodu aplikacji logika pokazano, jak obsługiwać rejestrowanie.

#### <a name="log-entry"></a>Wpis dziennika
To jest kod źródłowy aplikacji logiki do wstawiania pozycji dziennika.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Żądanie dziennika

To jest komunikat żądania dziennika opublikowane w aplikacji interfejsu API.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}"
        }
    }

```


#### <a name="log-response"></a>Zarejestruj odpowiedź

Jest to wiadomość odpowiedzi dziennika z aplikacji interfejsu API.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "\"0400fc2f-0000-0000-0000-575b3ff00000\"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Teraz Przyjrzyjmy się obsługi czynności błędów.


### <a name="error-handling"></a>Obsługa błędów

Poniższy przykładowy kod logiczny aplikacji pokazano, jak można zaimplementować obsługi błędów.

#### <a name="create-error-record"></a>Utwórz rekord błędu

To jest kod źródłowy aplikacji logika tworzenia rekordu błędu.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}          
```

#### <a name="insert-error-into-documentdb--request"></a>Błąd wstawiania do DocumentDB — żądanie

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MasterID_mp__c\":\"\",\"C_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"ONY_ID__c\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-documentdb--response"></a>Błąd wstawiania do DocumentDB — odpowiedzi


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "\"0c00eaac-0000-0000-0000-575b3fdc0000\"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\":\"DO - Degree level is DO\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MterID_mp__c\":\"\",\"Medicis_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"XXXXXXX\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Odpowiedzi komunikat o błędzie usług SalesForce

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="returning-the-response-back-to-the-parent-logic-app"></a>Zwracanie odpowiedzi wrócić do aplikacji logika nadrzędnej

Po uzyskaniu odpowiedzi można przekazać go do aplikacji logika nadrzędnej.

#### <a name="return-success-response-to-the-parent-logic-app"></a>Powodzenia odpowiedź zwrócona do aplikacji logika nadrzędnej

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "   Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-the-parent-logic-app"></a>Zwraca błąd odpowiedzi aplikacji logika nadrzędnej

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="documentdb-repository-and-portal"></a>Repozytorium DocumentDB i portalu

Nasze rozwiązanie dodane dodatkowe funkcje z [DocumentDB](https://azure.microsoft.com/services/documentdb).

### <a name="error-management-portal"></a>Portal zarządzania błędu

Aby wyświetlić błędy, można utworzyć aplikacji sieci web programu MVC, aby wyświetlić rekordy błędu z DocumentDB. **Lista**, **Szczegóły**, **Edytowanie**i **Usuwanie** operacji znajdują się w bieżącej wersji.

> [AZURE.NOTE]Edytowanie operacji: DocumentDB czy Zamień całego dokumentu.
> Rekordy wyświetlone na **liście** widoki **szczegółów** są tylko próbki. Nie są one rzeczywisty termin pacjentów rekordów.

Poniżej przedstawiono przykłady naszych szczegóły aplikacji MVC utworzone za pomocą metody opisane wcześniej.

#### <a name="error-management-list"></a>Lista zarządzania błędów

![Lista błędów](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Widok szczegółów zarządzania błędu

![Informacje o błędzie](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Portal zarządzania dziennika

Aby wyświetlić pliki dziennika, możemy również utworzony MVC aplikacji sieci web.  Poniżej przedstawiono przykłady naszych szczegóły aplikacji MVC utworzone za pomocą metody opisane wcześniej.

#### <a name="sample-log-detail-view"></a>Przykładowy widok szczegółów dziennika

![Widok szczegółów dziennika](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>Interfejs API szczegóły aplikacji

#### <a name="logic-apps-exception-management-api"></a>Zarządzanie wyjątkami aplikacje logiczny interfejs API

Nasze Otwórz źródło aplikacje logiczny wyjątku zarządzania interfejsu API aplikacji udostępnia następujące funkcje.

Istnieją dwa kontrolery:

- **ErrorController** wstawia błąd rekord (dokument) w zbiorze DocumentDB.
- **LogController** Wstawia rekordu dziennika (dokument) w zbiorze DocumentDB.

> [AZURE.TIP] Oba kontrolery `async Task<dynamic>` operacji. Dzięki temu operacje, aby rozwiązać w czasie wykonywania, więc możemy utworzyć schemat DocumentDB w treści tej operacji.

Każdy dokument w DocumentDB musi mieć unikatowy identyfikator. Użyto `PatientId` i Dodawanie sygnatury czasowej, które są konwertowane na wartość sygnatury czasowej systemu Unix (double). Firma Microsoft obciąć go, aby usunąć wartość ułamkową.

Możesz wyświetlić kodu źródłowego naszych kontrolera błąd interfejsu API, [z GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs).

Nazywamy API za pomocą aplikacji logiczny przy użyciu następującej składni.

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Wyrażenie w poprzednim przykładzie kodu jest sprawdzanie stanu *Create_NewPatientRecord* **nie powiodło się**.

## <a name="summary"></a>Podsumowanie

- Można łatwo zaimplementować rejestrowania i obsługi błędów w aplikacji logicznych.
- Za pomocą DocumentDB jako repozytorium rekordów dziennika i błędów (dokumenty).
- MVC umożliwia tworzenie portalu mają być wyświetlane rekordy dziennika i błędów.

### <a name="source-code"></a>Kod źródłowy
Kod źródłowy do zarządzania wyjątkami aplikacje logiki aplikacji interfejsu API jest dostępna w tym [repozytorium GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "API zarządzania wyjątku aplikacji logicznych").


## <a name="next-steps"></a>Następne kroki
- [Wyświetlanie więcej przykładów logika aplikacje i scenariusze](app-service-logic-examples-and-scenarios.md)
- [Więcej informacji na temat aplikacji logika narzędzia monitorowania](app-service-logic-monitor-your-logic-apps.md)
- [Tworzenie szablonu automatycznego wdrażania aplikacji warunków logicznych](app-service-logic-create-deploy-template.md)
