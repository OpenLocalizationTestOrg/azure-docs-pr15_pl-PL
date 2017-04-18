<properties
    pageTitle="Analizy dziennika logowania wyszukiwania interfejsu API usługi REST | Microsoft Azure"
    description="Ten przewodnik zawiera podstawowe samouczek opisujący sposób można używać wyszukiwania analizy dziennika interfejsu API usługi REST w pakiecie zarządzania operacje usługi (OMS) i znajdują się przykłady pokazano, jak użyć poleceń."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="log-analytics-log-search-rest-api"></a>Przeszukiwanie dziennika analizy dziennika interfejsu API usługi REST

Ten przewodnik zawiera podstawowe samouczek opisujący sposób możesz użyć interfejsu API usługi REST dziennika analizy wyszukiwania w pakiecie zarządzania operacje usługi (OMS) i znajdują się przykłady pokazano, jak użyć poleceń. Przykłady w tym artykule odwoływać operacyjne wniosków, która jest nazwą poprzednią wersję programu Analytics dziennika.

## <a name="overview-of-the-log-search-rest-api"></a>Omówienie wyszukiwania dziennika interfejsu API usługi REST

Interfejsu API usługi REST dziennika analizy wyszukiwania jest RESTful i można uzyskać do nich dostęp za pomocą interfejsu API Menedżera zasobów Azure. W tym dokumencie można znaleźć przykłady miejsce, w którym API jest dostępna za pośrednictwem [ARMClient](https://github.com/projectkudu/ARMClient), narzędzie wiersza polecenia Otwórz źródło, ułatwiającym wywoływanie interfejsu API Menedżera zasobów Azure. Używanie programu PowerShell i ARMClient jest jednym z wiele opcji, aby uzyskać dostęp do interfejsu API wyszukiwania analizy dziennika. Innym rozwiązaniem jest użycie modułu programu PowerShell usługi Azure dla OperationalInsights, który zawiera polecenia cmdlet, aby uzyskać dostęp do wyszukiwania. Korzystając z tych narzędzi można korzystać z RESTful Azure Menedżera zasobów interfejsu API do nawiązywania połączeń z obszarów roboczych usługi OMS i wykonać polecenia wyszukiwania w nich. API będzie wyjściowy wyników wyszukiwania dla Ciebie w formacie JSON, co umożliwia wykorzystanie wyników wyszukiwania na wiele różnych sposobów programowy.

Menedżer zasobów Azure można używać za pośrednictwem [biblioteki dla środowiska .NET](https://msdn.microsoft.com/library/azure/dn910477.aspx) , a także [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/mt163658.aspx). Przejrzyj połączonych stron sieci web, aby dowiedzieć się więcej.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Podstawowe samouczek interfejsu API usługi REST wyszukiwania analizy dziennika

### <a name="to-use-the-arm-client"></a>Aby używać klienta ARM

1. Zainstaluj [Chocolatey](https://chocolatey.org/), która jest Otwieranie Menedżera pakiet źródła dla systemu Windows. Otwórz okno wiersza polecenia jako administrator, a następnie uruchom następujące polecenie:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

2. Zainstaluj ARMClient, uruchamiając następujące polecenie:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Aby rozpocząć wyszukiwanie proste przy użyciu ARMClient

1. Zaloguj się do konta Microsoft lub zbędna:

    ```
    armclient login
    ```

    Pomyślne logowanie Wyświetla wszystkie subskrypcje powiązanych z danym kontem:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. Pobierz pakiet administracyjny operacje obszarów roboczych:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Pomyślnego połączenia Get czy wyjściowych powiązanych z subskrypcją wszystkich obszarów roboczych:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Utwórz do zmiennej wyszukiwania:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Wyszukaj przy użyciu usługi Nowa zmienna wyszukiwania:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Zaloguj się przykłady odwołań interfejsu API usługi REST wyszukiwania analizy
W poniższych przykładach pokazano, jak korzystając z interfejsu API wyszukiwania.

### <a name="search---actionread"></a>Wyszukiwanie — akcji/przeczytane

**Przykładowy adres Url:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Żądanie:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
W poniższej tabeli opisano właściwości, które są dostępne.

|**Właściwość**|**Opis**|
|---|---|
|Do góry|Maksymalna liczba wyników do zwrócenia.|
|Wyróżnianie|Zawiera parametry przed i po zazwyczaj używany do wyróżnienia zgodne pola|
|wstępne|Dodanie prefiksu składającego określonego ciągu znaków z dopasowanymi polami.|
|wpis|Dołącza ciągu z dopasowanymi polami.|
|kwerendy|Kwerendy wyszukiwania służył do gromadzenia i zwrócić wyniki.|
|Rozpoczynanie|Na początek okna czasu mają być można znaleźć w wynikach.|
|koniec|Na koniec okna czasu mają być można znaleźć w wynikach.|


**Odpowiedź:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Wyszukiwanie / {identyfikator} - akcji/przeczytane

**Żądanie zawartość zapisane wyszukiwanie:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

>[AZURE.NOTE] Jeśli wyniki wyszukiwania zawierają ze stanem "Oczekiwanie", następnie sondowanie uaktualnione wyniki można zrobić za pomocą tego interfejsu API. Po 6 min wyniki wyszukiwania zostaną usunięte z pamięci podręcznej i HTTP zniknęła zostaną zwrócone. Jeśli żądania wyszukiwania początkowy stan "Pomyślnie" od razu, nie zostaną dodane do pamięci podręcznej przyczyną tego interfejsu API zwrócić zniknęła HTTP, jeśli kwerenda. Zawartość wynik HTTP 200 będzie w formacie początkowego żądania wyszukiwania tylko z zaktualizowane wartości.

### <a name="saved-searches---rest-only"></a>Zapisane wyszukiwania — tylko REST

**Żądania listy zapisane wyszukiwania:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Obsługiwane metody: GET umieszczenie usuwanie

Obsługiwane metody kolekcji: pobieranie

W poniższej tabeli opisano właściwości, które są dostępne.

|Właściwość|Opis|
|---|---|
|Identyfikator|Unikatowy identyfikator.|
|Etag|**Wymagane poprawki**. Zaktualizowane przez serwer podczas każdego zapisu. Wartość musi być równa bieżącej wartości przechowywanej lub "*" aktualizacji. zwracany 409 starych nieprawidłowe wartości.|
|Properties.Query|**Wymagane**. Kwerendy wyszukiwania.|
|properties.displayName|**Wymagane**. Nazwa użytkownika zdefiniowane wyświetlania kwerendy. Jeśli w modelu jako zasób Azure będzie znacznik.|
|Properties.category|**Wymagane**. Kategoria kwerendy zdefiniowany przez użytkownika. Jeśli w modelu jako zasób Azure będzie znacznika.|

>[AZURE.NOTE] Interfejs API wyszukiwania analizy dziennika Zwraca obecnie utworzone przez użytkownika zapisane wyszukiwania podczas wysyłane zapisanego wyszukiwania w obszarze roboczym. Interfejs API nie zwróci wyszukiwaniach dostarczony przez rozwiązania w tej chwili. Ta funkcja zostanie dodany w późniejszym terminie.

### <a name="create-saved-searches"></a>Tworzenie zapisane wyszukiwania

**Żądanie:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Usuń zapisane wyszukiwania

**Żądanie:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Aktualizacja zapisane wyszukiwania

 **Żądanie:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metadane — tylko JSON

Oto sposób, aby wyświetlić pola dla wszystkich typów dziennika dla danych zebranych w obszarze roboczym. Na przykład jeśli chcesz, że wiesz, jeśli typ zdarzenia zawiera pole o nazwie komputera, następnie to jeden sposób, aby wyszukać i upewnij się.

**Żądanie dla pól:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Odpowiedź:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

W poniższej tabeli opisano właściwości, które są dostępne.

|**Właściwość**|**Opis**|
|---|---|
|Nazwa|Nazwa pola.|
|displayName|Wyświetlana nazwa pola.|
|Typ|Typ wartości pola.|
|facetable|Kombinacja bieżącego indeksowane, "przechowywane" i "Faseta" właściwości.|
|Wyświetlanie|Bieżący właściwość "wyświetlić". Zwraca wartość true, jeśli pole jest widoczne w wyszukiwaniu.|
|ownerType|Ograniczona, aby tylko tych typów, należące do onboarded IP.|


## <a name="optional-parameters"></a>Parametry opcjonalne
Poniżej opisano parametry opcjonalne, które są dostępne.

### <a name="highlighting"></a>Wyróżnianie

Parametr "Wyróżnianie" jest opcjonalny parametr, który możesz użyć, aby zażądać podsystemu wyszukiwania zawierają zestaw znaczników w odpowiedzi.

Te znaczniki wskazują rozpoczęcia i zakończenia wyróżniony tekst, która jest zgodna z warunkach określonych w kwerendzie wyszukiwania.
Możesz określić znacznikami rozpoczęcia i zakończenia, które będą używane przez funkcję wyszukiwania na zawijanie wyróżniony terminów.

**Przykładowe wyszukiwanie zapytania**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Przykładowy wynik:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Zwróć uwagę, że wynik powyżej zawiera komunikat o błędzie zawierający prefiksem i dołączana.

## <a name="computer-groups"></a>Grupy komputerów

Grupy komputera są specjalne zapisane wyszukiwania, które zwracają zestaw komputerów.  Aby ograniczyć wyniki do komputerów w grupie służy grupę komputerów w innych kwerend.  Grupa komputerów jest zaimplementowana jako zapisane wyszukiwanie ze znacznikiem grupy z wartością komputera.

Oto przykładowe odpowiedzi dla grupy komputerów.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
        }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Pobieranie grup komputerów

Za pomocą metody Get identyfikator grupy do pobierania grupę komputerów.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Tworzenie lub aktualizowanie grupę komputerów

Metoda położenie przy użyciu unikatowego zapisany identyfikator wyszukiwania do tworzenie nowej grupy komputera. Użycie istniejącej identyfikator grupy komputera spowoduje, że jeden zostaną zmodyfikowane. Po utworzeniu grupy komputera w konsoli usługi OMS identyfikator zostanie utworzone z grupy, a nazwa.

Zapytanie użyte definicja grupy musi zwracać zestaw komputerów w grupie działać poprawnie.  Zaleca się zakończenie kwerendy z *| Komputer odrębnych* aby zapewnić poprawne dane są zwracane.

Definicja wyszukiwania muszą zawierać znacznika o nazwie z wartością komputerowi wyszukiwania można zaklasyfikować jako grupę komputerów.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Usuwanie grupy komputerów

Aby usunąć grupę komputerów, użyj metody Delete identyfikatorem grupy.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Następne kroki

- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do tworzenia zapytań przy użyciu pól niestandardowych kryteriów.
