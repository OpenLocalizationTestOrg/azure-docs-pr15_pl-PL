<properties
    pageTitle="Omówienie Azure dziennik | Microsoft Azure"
    description="Dowiedz się, co to jest dziennik Azure i jak go używać do zrozumienia zdarzeń występujących w ramach subskrypcji usługi Azure."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="johnkem"/>

# <a name="overview-of-the-azure-activity-log"></a>Omówienie dziennika aktywności Azure
**Dziennik Azure** jest dziennika, który zapewnia wgląd w operacji, które były wykonywane na zasoby w ramach subskrypcji. Dziennik była wcześniej znana jako "Dzienników inspekcji" lub "Dzienniki operacyjne", ponieważ przekazuje kontrolnego sterowania zdarzeń dla subskrypcji. Korzystając z dziennika aktywności, można określić "co, kto i kiedy" dla dowolnej operacji (położenie, WPIS, Usuń) wykonane na zasoby w ramach subskrypcji zapisu. Można także zrozumieć stan operacji i inne odpowiednie właściwości. Dziennik nie zawiera operacji odczytu (Pobierz).

Dziennik różni się od [Dzienników diagnostycznych](./monitoring-overview-of-diagnostic-logs.md), które są wszystkie dzienniki wysyłane przez zasób. Dzienniki te zapewniają dane dotyczące operacji tego zasobu, a nie symulacji tego zasobu.

Zdarzenia można pobrać z dziennika aktywności za pomocą portalu Azure, polecenie, poleceń cmdlet programu PowerShell i interfejsu API usługi REST Monitor Azure.

Wyświetlanie tego [wideo wprowadzające dziennika aktywności](https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz).  

## <a name="what-you-can-do-with-the-activity-log"></a>Co można zrobić z dziennika aktywności
Oto niektóre czynności, które można wykonywać przy użyciu dziennika aktywności:

- Kwerendy, a następnie wyświetlać je w **Azure portal**.
- Kwerendy go za pomocą interfejsu API usługi REST, polecenia Cmdlet programu PowerShell lub interfejsu wiersza polecenia.
- [Tworzenie alertu wiadomości e-mail lub webhook, powodujące wyłączanie zdarzeń dziennika aktywności.](./insights-auditlog-to-webhook-email.md)
- [Zapisanie go **Konta miejsca do magazynowania** inspekcji archiwizacji lub ręcznie](./monitoring-archive-activity-log.md). Można określić czas przechowywania (w dniach) przy użyciu **Profilów dziennika**.
- Analizowanie go w PowerBI przy użyciu [**pakiet PowerBI**](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/).
- [Przesyłanie strumieniowe go do **Centrum zdarzeń** ](./monitoring-stream-activity-logs-event-hubs.md) dla spożyciu przez usługi innych firm lub rozwiązanie niestandardowe analizy, takich jak PowerBI.

## <a name="export-the-activity-log-with-log-profiles"></a>Eksportowanie dziennik z profilami dziennika
**Profil dziennika** określa sposób eksportowania dziennika aktywności. Za pomocą profilu dziennika, można skonfigurować:

- Miejsce, w którym mają być wysyłane dziennik (konto miejsca do magazynowania lub koncentratorów zdarzenia)
- Kategorie zdarzeń (zapis, usuwanie, akcji) powinny zostać wysłane?
- Regiony (lokalizacja) powinny być wyeksportowane
- Jak długo trwa dziennik powinny zostać zachowane na koncie miejsca do magazynowania — przechowywania zero dni oznacza, że zawsze prowadzone dzienniki. W przeciwnym razie wartość może być dowolna liczba dni od 1 do 2147483647. Jeśli określono zasady przechowywania, ale przechowywania dzienników na koncie miejsca do magazynowania jest wyłączony (na przykład, jeśli tylko zostanie wybrana opcja koncentratory zdarzenie lub usługi OMS), zasady przechowywania nie mają wpływu.

Te ustawienia można skonfigurować za pomocą opcji "Eksportuj" w karta dziennik w portalu. Można je również skonfigurowane programowo [przy użyciu interfejsu API usługi REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn931927.aspx), poleceń cmdlet programu PowerShell lub interfejsu wiersza polecenia. Subskrypcja może mieć tylko jeden profil dziennika.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Konfigurowanie profilów dziennika za pomocą portalu Azure
Możesz przesyłać strumieniowo dziennik do koncentratora zdarzenie lub zapisane na koncie miejsca do magazynowania przy użyciu opcji "Eksportuj" w portalu Azure.

1. Przejdź do karta **Dziennik** przy użyciu menu po lewej stronie portalu.

    ![Przejdź do dziennika aktywności w portalu](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Kliknij przycisk **Eksportuj** w górnej części karta.

    ![Przycisk Eksportuj w portalu](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. W kartę, która jest wyświetlana można wybrać:  
    - regiony, dla których chcesz wyeksportować zdarzenia
    - Konto miejsca do magazynowania, do którego chcesz zapisywać zdarzenia
    - Liczba dni, które mają zostać zachowane te zdarzenia w magazynie. Wartość 0 dni, które zawsze zachowuje pliki dziennika.
    - Namespace Bus usługę, w którym chcesz koncentratora wydarzenie ma zostać utworzony przesyłania strumieniowego te zdarzenia.

    ![Karta dziennik eksportu](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Kliknij przycisk **Zapisz** , aby zapisać ustawienia. Ustawienia są natychmiast zastosowane do swojej subskrypcji.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Konfigurowanie profilów dziennika przy użyciu poleceń cmdlet środowiska PowerShell Azure
#### <a name="get-existing-log-profile"></a>Uzyskaj profil dziennika
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Dodawanie profilu dziennika
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Właściwość         | Wymagane | Opis   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nazwa             | Tak      | Nazwa profilu dziennika.                                 |
| StorageAccountId | Brak       | Identyfikator zasobu konta miejsca do magazynowania, do którego powinny być zapisywane dziennika aktywności.                         |
| serviceBusRuleId | Brak       | Identyfikator reguły Bus usługi nazw Bus usługi chcesz mieć koncentratory zdarzenia utworzonego w. Jest ciągiem o następującym formacie: `{service bus resource ID}/authorizationrules/{key name}`. |
| Lokalizacje        | Tak      | Rozdzielaną średnikami listę regionów, dla których chcesz zbierania zdarzeń dziennika aktywności.              |
| RetentionInDays  | Tak      | Liczba dni dla zdarzenia, które powinny zostać zachowane, od 1 do 2147483647. Wartość zero przechowuje dzienniki czas nieokreślony (zawsze). |
| Kategorie       | Brak       | Rozdzielaną średnikami listę kategorii zdarzeń, które powinny być pobierane. Możliwe wartości to zapisu, usuwania i akcji.                                 |

#### <a name="remove-a-log-profile"></a>Usuwanie profilu dziennika
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Konfigurowanie profilów dziennika przy użyciu interfejsu wiersza polecenia dla innych Platform Azure
#### <a name="get-existing-log-profile"></a>Uzyskaj profil dziennika
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
`name` Właściwości powinny być nazwę profilu dziennika.

#### <a name="add-a-log-profile"></a>Dodawanie profilu dziennika
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Właściwość         | Wymagane | Opis   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nazwa             | Tak      | Nazwa profilu dziennika.                                 |
| storageId        | Brak       | Identyfikator zasobu konta miejsca do magazynowania, do którego powinny być zapisywane dziennika aktywności.                         |
| serviceBusRuleId | Brak       | Identyfikator reguły Bus usługi nazw Bus usługi chcesz mieć koncentratory zdarzenia utworzonego w. Jest ciągiem o następującym formacie: `{service bus resource ID}/authorizationrules/{key name}`. |
| lokalizacje        | Tak      | Rozdzielaną średnikami listę regionów, dla których chcesz zbierania zdarzeń dziennika aktywności.              |
| retentionInDays  | Tak      | Liczba dni dla zdarzenia, które powinny zostać zachowane, od 1 do 2147483647. Wartość zero przechowuje dzienniki czas nieokreślony (zawsze).     |
| kategorie       | Brak       | Rozdzielaną średnikami listę kategorii zdarzeń, które powinny być pobierane. Możliwe wartości to zapisu, usuwania i akcji.                                 |

#### <a name="remove-a-log-profile"></a>Usuwanie profilu dziennika
```
azure insights logprofile delete --name my_log_profile
```

## <a name="event-schema"></a>Schematu zdarzeń
Każde zdarzenie w dzienniku aktywności obejmuje obiektów blob JSON, podobnie jak w tym przykładzie:

```
{
  "value": [ {
    "authorization": {
      "action": "microsoft.support/supporttickets/write",
      "role": "Subscription Admin",
      "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
    },
    "caller": "admin@contoso.com",
    "channels": "Operation",
    "claims": {
      "aud": "https://management.core.windows.net/",
      "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
      "iat": "1421876371",
      "nbf": "1421876371",
      "exp": "1421880271",
      "ver": "1.0",
      "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
      "puid": "20030000801A118C",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
      "name": "John Smith",
      "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
      "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.microsoft.com/claims/authnclassreference": "1"
    },
    "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "description": "",
    "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
    "eventName": {
      "value": "EndRequest",
      "localizedValue": "End request"
    },
    "eventSource": {
      "value": "Microsoft.Resources",
      "localizedValue": "Microsoft Resources"
    },
    "httpRequest": {
      "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
      "clientIpAddress": "192.168.35.115",
      "method": "PUT"
    },
    "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
    "level": "Informational",
    "resourceGroupName": "MSSupportGroup",
    "resourceProviderName": {
      "value": "microsoft.support",
      "localizedValue": "microsoft.support"
    },
    "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
    "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "operationName": {
      "value": "microsoft.support/supporttickets/write",
      "localizedValue": "microsoft.support/supporttickets/write"
    },
    "properties": {
      "statusCode": "Created"
    },
    "status": {
      "value": "Succeeded",
      "localizedValue": "Succeeded"
    },
    "subStatus": {
      "value": "Created",
      "localizedValue": "Created (HTTP Status Code: 201)"
    },
    "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
    "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
    "subscriptionId": "s1"
  } ],
"nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```

| Nazwa elementu         | Opis             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autoryzacja        | Obiektów blob RBAC właściwości zdarzenia. Zazwyczaj zawiera właściwości "działania", "roli" i "zakres".|
| wywołujący               | Adres e-mail użytkownika, który przeprowadził operacji, oświadczeń głównej nazwy użytkownika lub oświadczeniu SPN, na podstawie dostępności.|
| kanały             | Jedną z następujących wartości: "Administrator", "Operacja"|
| correlationId        | Zazwyczaj GUID w postaci ciągu. Zdarzenia, które mają correlationId należą do tego samego działania uber.   |
| Opis          | Tekst statyczny opis zdarzenia.                              |
| eventDataId          | Unikatowy identyfikator zdarzenia.    |
| źródła zdarzeń          | Nazwa usługi Azure lub infrastruktury, który wygenerował to zdarzenie.    |
| httpRequest          | Obiektów blob opisem żądania Http. Zazwyczaj zawiera "clientRequestId", "clientIpAddress" i "metoda" (metoda HTTP. For example, umieść).                            |
| poziom                | Poziom zdarzenia. Jedną z następujących wartości: "Krytyczne", "Błąd", "Ostrzeżenia", "Informacyjne" i "Pełne"  |
| resourceGroupName    | Nazwa grupy zasobów dla zasobu ryzyko.               |
| resourceProviderName | Nazwa dostawcy zasobów dla zasobu ryzyko             |
| resourceUri          | Identyfikator zasobu ryzyko zasobu.                               |
| operationId          | Identyfikator GUID współużytkowany zdarzenia, które odpowiadają jednej operacji.         |
| operationName        | Nazwa operacji.  |
| właściwości           | Zestaw `<Key, Value>` par (Słownik) przedstawiający szczegóły zdarzenia.                                |
| Stan               | Ciąg opisujący stan operacji. Niektóre typowe wartości są: pracę w toku, powiodło się, nie powiodło się aktywne, rozwiązany.                                |
| podstanu            | Zazwyczaj kod stanu HTTP odpowiedniego odpoczynku połączeń, ale można także dodać inne ciągi opisujące podstanu, takie jak te wartości typowych: OK (kod stanu HTTP: 200), utworzony (kod stanu HTTP: 201), akceptowane (kod stanu HTTP: 202), nie zawartości (kod stanu HTTP: 204), nieprawidłowe żądanie (kod stanu HTTP: 400), nie można odnaleźć (kod stanu HTTP: 404), konflikt (kod stanu HTTP : 409), wewnętrzny błąd serwera (kod stanu HTTP: 500), Usługa niedostępna (kod stanu HTTP: 503), limit czasu bramy (kod stanu HTTP: 504). |
| eventTimestamp       | Sygnatura czasowa, gdy to zdarzenie zostało wygenerowane przez usługę Azure przetwarzania żądania odpowiednie zdarzenie.     |
| submissionTimestamp  | Sygnatura czasowa podczas wydarzenia stał się dostępny dla kwerend.             |
| subscriptionId       | Identyfikator subskrypcji Azure.  |
| nextLink             | Token kontynuacji do pobierania następnego zestawu wyników, gdy są one podzielony na wiele odpowiedzi. Zazwyczaj potrzebne, gdy istnieje więcej niż 200 rekordów.     |

## <a name="next-steps"></a>Następne kroki
- [Dowiedz się więcej o dziennik (dawniej dzienników inspekcji)](../resource-group-audit.md)
- [Strumień Azure dziennik do koncentratorów zdarzenia](./monitoring-stream-activity-logs-event-hubs.md)
