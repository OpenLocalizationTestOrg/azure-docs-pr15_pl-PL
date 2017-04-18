<properties
 pageTitle="Harmonogram uwierzytelniania ruchu wychodzącego"
 description="Harmonogram uwierzytelniania ruchu wychodzącego"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/15/2016"
 ms.author="deli"/>

# <a name="scheduler-outbound-authentication"></a>Harmonogram uwierzytelniania ruchu wychodzącego

Harmonogram zadań może być konieczne wyróżnienia do usług, które wymagają uwierzytelniania. W ten sposób nazywane usługi można określić, jeśli zadanie harmonogramu można uzyskać dostęp do jej zasobów. Niektóre z tych usług zawierać innych usług Azure, Salesforce.com Facebook i bezpiecznych niestandardowych witryn sieci Web.

## <a name="adding-and-removing-authentication"></a>Dodawanie i usuwanie uwierzytelniania

Dodawanie uwierzytelniania do harmonogramu zadania jest proste — Dodaj element podrzędny JSON `authentication` do `request` elementu podczas tworzenia lub aktualizowania zadanie. Hasła przekazywane do usługi Harmonogram zlecenia położenie, poprawki lub wpisu — jako część `authentication` obiekt — nigdy nie są zwracane w odpowiedzi. W odpowiedzi na informacje poufne jest ustawiona na wartość null lub może być token publicznego, reprezentująca uwierzytelnionym jednostki.

Aby usunąć uwierzytelniania, umieszczenie lub poprawkami zadania jawnie, ustawienie `authentication` obiektu null. Nie zobaczą żadnych właściwości uwierzytelniania w odpowiedzi.

Obecnie są tylko obsługiwanych typów uwierzytelniania `ClientCertificate` modelu (dotyczące korzystania z certyfikatów SSL/TLS klienta), `Basic` model (w przypadku uwierzytelniania podstawowego) oraz `ActiveDirectoryOAuth` modelu (dla uwierzytelniania Active Directory OAuth.)

## <a name="request-body-for-clientcertificate-authentication"></a>Treści żądania uwierzytelniania ClientCertificate

Podczas dodawania uwierzytelniania za pomocą `ClientCertificate` modelu, określ następujące dodatkowe elementy w treści wezwania.  

|Element|Opis|
|:---|:---|
|_uwierzytelnianie (element nadrzędny)_|Obiekt uwierzytelniania za pomocą certyfikatu SSL klienta.|
|_Typ_|Wymagane. Typ uwierzytelniania. Certyfikatów SSL klienta, wartość musi być `ClientCertificate`.|
|_PFX_|Wymagane. Zakodowany Base64 zawartość pliku PFX.|
|_hasło_|Wymagane. Hasło, aby uzyskać dostęp do pliku PFX.|


## <a name="response-body-for-clientcertificate-authentication"></a>Treść odpowiedzi uwierzytelniania ClientCertificate

Po przesłaniu żądania z informacjami o uwierzytelniania, odpowiedź zawiera następujące elementy dotyczące uwierzytelniania.

|Element |Opis |
|:--|:--|
|_uwierzytelnianie (element nadrzędny)_ |Obiekt uwierzytelniania za pomocą certyfikatu SSL klienta.|
|_Typ_ |Typ uwierzytelniania. Certyfikaty SSL klienta, wartość jest `ClientCertificate`.|
|_certificateThumbprint_ |Odcisk palca certyfikatu.|
|_certificateSubjectName_ |Nazwa wyróżniająca temat certyfikatu.|
|_certificateExpiration_ |Data wygaśnięcia certyfikatu.|

## <a name="sample-rest-request-for-clientcertificate-authentication"></a>Przykładowe żądanie pozostałych uwierzytelniania ClientCertificate

```
PUT https://management.azure.com/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-clientcertificate-authentication"></a>Przykładowe pozostałych odpowiedź uwierzytelniania ClientCertificate

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/1fe0abdf-581e-4dfe-9ec7-e5cb8e7b205e/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="request-body-for-basic-authentication"></a>Treść żądania dla uwierzytelniania podstawowego

Podczas dodawania uwierzytelniania za pomocą `Basic` modelu, określ następujące dodatkowe elementy w treści wezwania.

|Element|Opis|
|:--|:--|
|_uwierzytelnianie (element nadrzędny)_ |Obiekt uwierzytelniania dla uwierzytelniania podstawowego.|
|_Typ_ |Wymagane. Typ uwierzytelniania. W przypadku uwierzytelniania podstawowego wartość musi być `Basic`.|
|_Nazwa użytkownika_ |Wymagane. Nazwa użytkownika do uwierzytelnienia.|
|_hasło_ |Wymagane. Hasło do uwierzytelniania.|

## <a name="response-body-for-basic-authentication"></a>Treść odpowiedzi dla uwierzytelniania podstawowego

Po przesłaniu żądania z informacjami o uwierzytelniania, odpowiedź zawiera następujące elementy dotyczące uwierzytelniania.

|Element|Opis|
|:--|:--|
|_uwierzytelnianie (element nadrzędny)_ |Obiekt uwierzytelniania dla uwierzytelniania podstawowego.|
|_Typ_ |Typ uwierzytelniania. W przypadku uwierzytelniania podstawowego wartość jest `Basic`.|
|_Nazwa użytkownika_ |Nazwa użytkownika uwierzytelnionego.|

## <a name="sample-rest-request-for-basic-authentication"></a>Przykładowe żądanie pozostałych do uwierzytelniania podstawowego

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-basic-authentication"></a>Przykładowe pozostałych odpowiedzi dla uwierzytelniania podstawowego

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="request-body-for-activedirectoryoauth-authentication"></a>Treści żądania uwierzytelniania ActiveDirectoryOAuth

Podczas dodawania uwierzytelniania za pomocą `ActiveDirectoryOAuth` modelu, określ następujące dodatkowe elementy w treści wezwania.

|Element |Opis |
|:--|:--|
|_uwierzytelnianie (element nadrzędny)_ |Obiekt uwierzytelniania dla przy użyciu funkcji uwierzytelniania ActiveDirectoryOAuth.|
|_Typ_ |Wymagane. Typ uwierzytelniania. W przypadku uwierzytelniania ActiveDirectoryOAuth wartość musi być `ActiveDirectoryOAuth`.|
|_dzierżawy_ |Wymagane. Identyfikator dzierżawy dla dzierżawy Azure AD.|
|_grupy odbiorców_ |Wymagane. To jest ustawiona na https://management.core.windows.net/.|
|_clientId_ |Wymagane. Podaj identyfikator klienta na podstawie Azure AD.|
|_tajny_ |Wymagane. Tajny klienta, który żąda tokenu.|

### <a name="determining-your-tenant-identifier"></a>Określanie identyfikatora dzierżawy

Możesz znaleźć identyfikator dzierżawy dla dzierżawy Azure AD, uruchamiając `Get-AzureAccount` w programie PowerShell Azure.

## <a name="response-body-for-activedirectoryoauth-authentication"></a>Treść odpowiedzi uwierzytelniania ActiveDirectoryOAuth

Po przesłaniu żądania z informacjami o uwierzytelniania, odpowiedź zawiera następujące elementy dotyczące uwierzytelniania.

|Element |Opis |
|:--|:--|
|_uwierzytelnianie (element nadrzędny)_ |Obiekt uwierzytelniania dla przy użyciu funkcji uwierzytelniania ActiveDirectoryOAuth.|
|_Typ_ |Typ uwierzytelniania. W przypadku uwierzytelniania ActiveDirectoryOAuth wartość jest `ActiveDirectoryOAuth`.|
|_dzierżawy_ |Identyfikator dzierżawy dla dzierżawy Azure AD. |
|_grupy odbiorców_ |To jest ustawiona na https://management.core.windows.net/.|
|_clientId_ |Identyfikator klienta na podstawie Azure AD.|

## <a name="sample-rest-request-for-activedirectoryoauth-authentication"></a>Przykładowe żądanie pozostałych uwierzytelniania ActiveDirectoryOAuth

```
PUT https://management.azure.com/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
  }
}
```

## <a name="sample-rest-response-for-activedirectoryoauth-authentication"></a>Przykładowe pozostałych odpowiedź uwierzytelniania ActiveDirectoryOAuth

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{  
   "id":"/subscriptions/1d908808-e491-4fe5-b97e-29886e18efd4/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "tenant":"microsoft.onmicrosoft.com",
               "audience":"https://management.core.windows.net/",
               "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type":"ActiveDirectoryOAuth"
            }
         },
         "type":"http"
      },
      "recurrence":{  
         "frequency":"minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"enabled",
      "status":{  
         "lastExecutionTime":"2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime":"2016-03-16T19:11:00Z",
         "executionCount":5,
         "failureCount":5,
         "faultedCount":1
      }
   }
}
```

## <a name="see-also"></a>Zobacz też


 [Co to jest harmonogram?](scheduler-intro.md)

 [Azure pojęcia harmonogram, terminologia i hierarchia jednostki](scheduler-concepts-terms.md)

 [Wprowadzenie do korzystania z harmonogramu w portalu Azure](scheduler-get-started-portal.md)

 [Plany i rozliczenia w harmonogramie Azure](scheduler-plans-billing.md)

 [Azure odwołanie interfejsu API usługi REST harmonogramu](https://msdn.microsoft.com/library/mt629143)

 [Dokumentacja dotycząca Azure poleceń cmdlet środowiska PowerShell harmonogramu](scheduler-powershell-reference.md)

 [Azure Harmonogram dostępności i niezawodności](scheduler-high-availability-reliability.md)

 [Azure limity harmonogram, wartości domyślne i kody błędów](scheduler-limits-defaults-errors.md)
