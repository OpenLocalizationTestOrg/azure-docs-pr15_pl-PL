<properties
   pageTitle="Interfejs API Alert pozostałych analizy dziennika"
   description="Dziennik analizy Alert interfejsu API usługi REST umożliwia tworzenie alertów i zarządzanie nimi w pakiecie operacje zarządzania usługi (OMS).  Ten artykuł zawiera szczegóły API i przykłady dotyczące wykonywania różnych operacji."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="log-analytics-alert-rest-api"></a>Dziennik analizy alert interfejsu API usługi REST

Dziennik analizy Alert interfejsu API usługi REST umożliwia tworzenie alertów i zarządzanie nimi w pakiecie operacje zarządzania usługi (OMS).  Ten artykuł zawiera szczegóły API i przykłady dotyczące wykonywania różnych operacji.

Interfejsu API usługi REST dziennika analizy wyszukiwania jest RESTful i można uzyskać do nich dostęp za pośrednictwem interfejsu API usługi REST Menedżera zasobów Azure. W tym dokumencie można znaleźć przykłady miejsce, w którym API jest dostępny z wiersza polecenia programu PowerShell, używając [ARMClient](https://github.com/projectkudu/ARMClient), narzędzie wiersza polecenia Otwórz źródło, ułatwiającym wywoływanie interfejsu API Menedżera zasobów Azure. Używanie programu PowerShell i ARMClient jest jednym z wiele opcji, aby uzyskać dostęp do interfejsu API wyszukiwania analizy dziennika. Korzystając z tych narzędzi można korzystać z RESTful Azure Menedżera zasobów interfejsu API do nawiązywania połączeń z obszarów roboczych usługi OMS i wykonać polecenia wyszukiwania w nich. API będzie wyjściowy wyników wyszukiwania dla Ciebie w formacie JSON, co umożliwia wykorzystanie wyników wyszukiwania na wiele różnych sposobów programowy.

## <a name="prerequisites"></a>Wymagania wstępne
Obecnie alerty można utworzyć tylko z zapisanego wyszukiwania w dzienniku analizy.  Może dotyczyć [Interfejsu API usługi REST wyszukiwania dziennika](log-analytics-log-search-api.md) , aby uzyskać więcej informacji.

## <a name="schedules"></a>Harmonogramów
Zapisane wyszukiwanie może zawierać jeden lub więcej harmonogramów. Harmonogram Określa, jak często wyszukiwania jest uruchamiania oraz przedział czasu, w którym określono kryteria.
Harmonogramy mają właściwości w poniższej tabeli.

| Właściwość  | Opis |
|:--|:--|
| Interwał | Jak często jest wykonywane wyszukiwanie. Mierzony w minutach. |
| QueryTimeSpan | Przedział czasu, dla której zostanie obliczone kryteria. Musi być równa lub większa niż interwał. Mierzony w minutach. |
| Wersja | Używana wersja interfejsu API.  Obecnie to powinno być zawsze ustawione na wartość 1. |

Na przykład można rozważyć kwerendy z interwału 15 minut i przedziału czasu 30 minut. W tym przypadku kwerenda będzie działać co 15 minut, a jeśli kryteria nadal rozpoznawać over PRAWDA czy wywołany alert przedziale 30 minut.

### <a name="retrieving-schedules"></a>Pobieranie harmonogramów
Użyj metody Get pobrać wszystkie harmonogramy dla zapisanego wyszukiwania.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Za pomocą metody Get identyfikator harmonogramu do pobierania określonego harmonogramu zapisanego wyszukiwania.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Oto przykładowe odpowiedzi dla serii rozłożonych w czasie.

    {
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
        "Interval": 15,
        "QueryTimeSpan": 15
    }

### <a name="creating-a-schedule"></a>Tworzenie harmonogramu
Użyj metody położenie przy użyciu Identyfikatora unikatowy harmonogram utworzenie nowego harmonogramu.  Uwaga dwóch harmonogramów nie mogą mieć tej samej nawet jeśli są one skojarzone z różnymi zapisane wyszukiwania.  Po utworzeniu harmonogramu w konsoli usługi OMS identyfikator GUID jest tworzony dla identyfikator harmonogramu.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Edytowanie harmonogramu
Użyj metody położenie przy użyciu istniejącego ID harmonogram dla samej zapisanego wyszukiwania do modyfikowania tego harmonogramu.  Treści żądania muszą zawierać etag harmonogramu.

    $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Usuwanie harmonogramów
Aby usunąć serii rozłożonych w czasie, użyj metody Delete przy użyciu Identyfikatora harmonogramu.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Akcje
Harmonogram można mieć kilka akcji. Akcji mogą definiować jeden lub więcej procesów do wykonania, takie jak wysłanie wiadomości e-mail lub uruchamianie działań aranżacji lub mogą określić próg Określa dopasowania niektóre kryteria, wyniki wyszukiwania.  Niektóre akcje będzie zdefiniować oba, tak aby te procesy są wykonywane po spełnieniu progu.

Wszystkie akcje mają właściwości w poniższej tabeli.  Różne typy alertów mają różne dodatkowe właściwości, które zostały opisane poniżej.

| Właściwość | Opis |
|:--|:--|
| Typ | Typ akcji.  Obecnie możliwe wartości są Alert i Webhook. |
| Nazwa | Nazwa wyświetlana alertu. |
| Wersja | Używana wersja interfejsu API.  Obecnie to powinno być zawsze ustawione na wartość 1. |

### <a name="retrieving-actions"></a>Pobieranie akcji
Użyj metody Get pobrać wszystkie akcje dla serii rozłożonych w czasie.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Za pomocą metody Get identyfikator akcji pobrać określonej akcji dla serii rozłożonych w czasie.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Tworzenie lub edytowanie akcji
Użyj metody położenie identyfikatora akcji, który jest unikatowy w harmonogramie, aby utworzyć nową akcję.  Po utworzeniu akcji w konsoli usługi OMS identyfikator GUID jest identyfikator akcji.

Użyj metody położenie przy użyciu istniejącego ID akcji dla samej zapisanego wyszukiwania do modyfikowania tego harmonogramu.  Treści żądania muszą zawierać etag harmonogramu.

Format żądania związane z tworzeniem nowej akcji zależy od typu akcji, w tych przykładach przedstawiono w poniższych sekcjach.

### <a name="deleting-actions"></a>Usuwanie akcji
Aby usunąć akcji, użyj metody Delete przy użyciu Identyfikatora akcji.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Akcje alertu
Harmonogram powinien mieć tylko jeden akcji alertu.  Alert akcji ma jedną lub więcej z sekcji w poniższej tabeli.  Każdy opisano w dalszej części poniżej.

| Sekcja | Opis |
|:--|:--|
| Progowa. | Kryteria dla uruchomienia akcji. |  
| EmailNotification | Wysyłania poczty e-mail do wielu adresatów. |
| Rozwiązywanie problemu | Rozpoczynanie działań aranżacji w automatyzacji Azure, aby spróbować rozwiązać problem zidentyfikowanych. |

#### <a name="thresholds"></a>Progi
Akcję alertu powinien mieć tylko jeden progowa.  Gdy wyniki wyszukiwania zapisanego pasuje progu w działaniu skojarzone z tym wyszukiwania, są uruchamiane inne procesy w tym działaniu.  Akcja może również zawierać tylko wartości progowej, aby mogą być używane z działaniami innych typów, które nie zawierają wartości progowe.

Progi mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| Operator | Operator porównania progowa. <br> BT = większe niż <br> lt = mniej niż |
| Wartość | Wartość progu. |

Na przykład można rozważyć kwerenda interwał 15 minut, przedziału czasu 30 minut i progu większe niż 10. W tym przypadku kwerenda będzie działać co 15 minut, a alert będzie uruchomiony, jeśli go 10 zdarzenia, które zostały utworzone w danym okresie 30 minut.

Oto przykładowe odpowiedzi na akcję z tylko progu.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Użyj metody położenie przy użyciu Identyfikatora unikatowe akcji do utworzenia nowej akcji progu dla serii rozłożonych w czasie.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Użyj metody położenie przy użyciu istniejącego Identyfikatora akcji do modyfikowania akcję progu dla serii rozłożonych w czasie.  Treści żądania muszą zawierać etag akcji.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="email-notification"></a>Powiadomienia pocztą e-mail
Powiadomienia e-mail wysyłać pocztę do jednego lub wielu adresatów.  Właściwości są umieszczane w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| Adresaci | Lista adresów e-mail. |
| Temat | Temat wiadomości e-mail. |
| Załącznik | Załączniki nie są obecnie obsługiwane, więc zawsze będą miały wartość "Brak". |

Oto przykładowe odpowiedzi na wiadomości e-mail akcji powiadomienie o progu.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Użyj metody położenie przy użyciu Identyfikatora unikatowe akcji do utworzenia nowej akcji poczty e-mail dla serii rozłożonych w czasie.  Poniższy przykład tworzy wiadomości e-mail z powiadomieniem o progu, aby wiadomości e-mail jest wysyłana po wynikach wyszukiwania przekracza progu.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

Użyj metody położenie przy użyciu istniejącego Identyfikatora akcji do modyfikowania działanie poczty e-mail dla serii rozłożonych w czasie.  Treści żądania muszą zawierać etag akcji.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $ emailJson

#### <a name="remediation-actions"></a>Akcje naprawy
Remediations Uruchom działań aranżacji w automatyzacji Azure, która próbuje rozwiązać ten problem, oznaczona alert.  Musisz utworzyć webhook dla działań aranżacji używane w akcji naprawy, a następnie określ identyfikator URI we właściwości WebhookUri.  Po utworzeniu tej akcji za pomocą konsoli usługi OMS nowe webhook jest tworzone automatycznie dla działań aranżacji.

Remediations zawierać właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| RunbookName | Nazwa zestawu działań aranżacji. Nazwa musi być zgodna opublikowanych działań aranżacji na koncie automatyzacji skonfigurowany w rozwiązaniu automatyzacji w obszarze roboczym usługi OMS. |
| WebhookUri | Identyfikator URI webhook.
| Wygaśnięcia | Data wygaśnięcia i godzina webhook.  Jeśli webhook nie ma wygaśnięcia, może to być dowolny prawidłowy przyszłej daty. |

Oto przykładowe odpowiedzi na akcję naprawy progu.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Użyj metody położenie przy użyciu Identyfikatora unikatowe akcji do utworzenia nowej akcji naprawy dla serii rozłożonych w czasie.  Poniższy przykład tworzy korygowania o progu, więc działań aranżacji jest uruchamiany po wynikach wyszukiwania przekracza progu.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Użyj metody położenie przy użyciu istniejącego Identyfikatora akcji do modyfikowania akcję naprawy dla serii rozłożonych w czasie.  Treści żądania muszą zawierać etag akcji.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Przykład
Oto przykład ukończone, aby utworzyć nowy alert e-mail.  Spowoduje to utworzenie nowego harmonogramu wraz z działaniami zawierająca próg i poczty e-mail.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $workspaceId    = "MyWorkspace"
    $searchId       = "51cf0bd9-5c74-6bcb-927e-d1e9080b934e"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule?api-version=2015-03-20 $scheduleJson

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/mythreshold?api-version=2015-03-20 $thresholdJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/$workspaceId/savedSearches/$searchId/schedules/myschedule/actions/myemailaction?api-version=2015-03-20 $emailJson

### <a name="webhook-actions"></a>Akcje Webhook
Webhook akcji Rozpocznij proces nawiązywania połączeń z adresem URL i opcjonalnie podając ładunku do wysłania.  Są podobne do akcji rozwiązywanie problemu z wyjątkiem są przeznaczone do webhooks, który może wywołać procesów innych niż runbooks automatyzacji Azure.  Zapewniają także dodatkowa opcja dostarczania ładunku dostarczane do zdalnego procesu.

Akcje Webhook nie ma wartości progowej, ale zamiast tego powinna zostać dodana do harmonogramu, zawierającego akcję alertu o progu.  Można dodawać wiele akcji Webhook, które zostaną wszystkie uruchomione po spełnieniu progu.

Akcje Webhook zawierać właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| WebhookUri | Temat wiadomości e-mail. |
| CustomPayload | Niestandardowe ładunek były wysyłane do webhook.  Format będzie zależy od tego, co oczekuje webhook. |

Oto przykładowe odpowiedzi na webhook akcji i skojarzone alertów o progu.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

#### <a name="create-or-edit-a-webhook-action"></a>Tworzenie lub edytowanie akcji webhook
Użyj metody położenie przy użyciu Identyfikatora unikatowe akcji do utworzenia nowej akcji webhook dla serii rozłożonych w czasie.  Poniższy przykład tworzy akcji Webhook i alertów o progu tak, aby webhook zostanie wyzwolony w przypadku wyniki wyszukiwania przekracza progu.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Użyj metody położenie przy użyciu istniejącego Identyfikatora akcji do modyfikowania akcję webhook dla serii rozłożonych w czasie.  Treści żądania muszą zawierać etag akcji.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

## <a name="next-steps"></a>Następne kroki

- Za pomocą [Interfejsu API usługi REST do wyszukiwania dziennika](log-analytics-log-search-api.md) w dzienniku analizy.
