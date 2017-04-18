<properties
    pageTitle="Azure monitorowania Instruktaż interfejsu API pozostałych | Microsoft Azure"
    description="Jak uwierzytelnienia żądania i za pomocą interfejsu API usługi REST monitorowania Azure."
    authors="mcollier, rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="mcollier"/>

# <a name="azure-monitoring-rest-api-walkthrough"></a>Monitorowanie pozostałych Instruktaż interfejsu API platformy Azure
W tym artykule pokazano, jak do uwierzytelniania, więc kodu można użyć [Microsoft Azure Monitor pozostałych API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

Interfejs API monitora Azure umożliwia programowy pobieranie definicji metryczne dostępnych domyślnych (typ metryki, takich jak czas Procesora, żądania itp.) i dokładności i wartości metryki. Po pobraniu, dane można zapisywać w sklepie osobnych danych, takich jak bazy danych SQL Azure, DocumentDB lub Lake danych Azure. Stamtąd dodatkowe analizy mogą być wykonywane zgodnie z potrzebami.

Oprócz pracy z różnych punktach dane, jak w tym artykule przedstawiono, Monitor API umożliwia listy reguł alertów, wyświetlanie dzienników aktywności i wiele innych. Aby uzyskać pełną listę dostępnych operacji zobacz [Microsoft Azure Monitor pozostałych API Reference](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Uwierzytelnianie żądania Azure Monitor

Pierwszym krokiem jest uwierzytelnianie wezwanie.

Wszystkie zadania wykonywane z interfejsu API Monitor Azure za pomocą Menedżera zasobów Azure model uwierzytelniania. W związku z tym wszystkie żądania muszą zostać uwierzytelnione z usługą Azure Active Directory (Azure AD). Jeden ze sposobów uwierzytelniania aplikacji klienckiej jest można tworzyć Azure AD wystawcy usługi i pobierać token uwierzytelniania (JWT). Poniższy przykładowy skrypt pokazuje, tworzenie głównej za pomocą programu PowerShell usługi Azure AD. Aby uzyskać bardziej szczegółowe omówienie zapoznaj się z dokumentacją przy [użyciu programu PowerShell Azure utworzyć usługi kapitału dostępu do zasobów](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password—powershell). Użytkownik może również [utworzyć zasady usług za pośrednictwem portalu Azure](../resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Kwerendy Azure Monitor API, z aplikacją kliencką należy używać wystawcy usługi poprzednio utworzone do uwierzytelniania. W poniższym przykładzie skrypt programu PowerShell pokazano jeden z nich, które ułatwiają rozpoczęcie token uwierzytelniania JWT za pomocą [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL). JWT token są przekazywane jako część parametru autoryzacji HTTP w żądania do interfejsu API usługi REST Monitor Azure.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.windows.net/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)
 
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values 
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Po zakończeniu kroku ustawienia uwierzytelniania kwerend następnie można wykonać przed interfejsu API usługi REST Monitor Azure. Istnieją dwa zapytania pomocne:

1. Lista metryczne definicji dla zasobu

2. Pobieranie wartości metryki


## <a name="retrieve-metric-definitions"></a>Pobieranie definicji metryczne
>[AZURE.NOTE] Aby pobrać metryczne definicji za pomocą interfejsu API usługi REST Monitor Azure, należy użyć "2016-03-01" jako wersja API.

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Dla aplikacji logika Azure metryczne definicje pojawią się podobne do następujących zrzut ekranu:

![ALT "JSON widok definicji metryczne odpowiedzi".](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Aby uzyskać więcej informacji zobacz dokumentację [listy metryczne definicje dla zasobu w interfejsu API usługi REST Monitor Azure](https://msdn.microsoft.com/library/azure/mt743621.aspx) .

## <a name="retrieve-metric-values"></a>Pobieranie wartości metryki
Po dostępne definicje metryczne są znane, następnie jest możliwość pobierania powiązanych wartości metryki. Używanie nazw metryczne "wartość" (nie "localizedValue") dla żądań filtrowania (na przykład, pobrać punktów danych metryczne "CpuTime" i "Żąda"). Jeśli określono żadnych filtrów, jest zwracana Metryka domyślnej.

>[AZURE.NOTE] Aby pobrać wartości metryki za pomocą interfejsu API usługi REST Monitor Azure, należy użyć "2016-06-01" jako wersja API.

**Metoda**: pobieranie

**Żądanie URI**: https://management.azure.com/subscriptions/_{identyfikator subskrypcji}_/resourceGroups/_{ruch Nazwa zasobu grupy}_/providers/_{ruch nazw zasobów dostawcy}_/_{Typ zasobu}_//providers/microsoft.insights/metrics?$filter=_{Nazwa zasobu}__{filtra}_i wersja api =_{apiVersion}_

Na przykład aby pobrać punktów danych metryczne RunsSucceeded zakresu danej chwili i na ziarno czasu, 1 godzina, żądania będzie się następująco:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Wynik pojawią się podobny do następującego zrzut ekranu:

![ALT "Odpowiedź JSON przedstawiający wartość metryki Średni czas odpowiedzi"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Aby pobrać wiele punktów danych lub agregacji, dodać nazwy definicji metryczne i typy agregacji do filtru, jak pokazano w poniższym przykładzie:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>Za pomocą ARMClient
Zamiast przy użyciu programu PowerShell (jak pokazano powyżej), jest użycie [ARMClient](https://github.com/projectkudu/ARMClient) na komputerze z systemem Windows. ARMClient automatycznie obsługuje uwierzytelnianie Azure AD (i tokenu JWT). Wykorzystanie ARMClient do pobierania danych metryki w konspekcie, następujące czynności:

1. Zainstaluj [Chocolatey](https://chocolatey.org/) i [ARMClient](https://github.com/projectkudu/ARMClient).

2. W oknie końcowych wpisz _armclient.exe logowania_. Monit o zalogowanie się Azure.

3. Typ _armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01_

4. Typ _armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01_


![ALT "Używanie ARMClient do pracy z monitorowania Azure interfejsu API usługi REST"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)


## <a name="retrieve-the-resource-id"></a>Pobieranie identyfikator zasobu
Za pomocą interfejsu API usługi REST naprawdę może pomóc zrozumienie dostępne definicje metryczne, dokładności i powiązanych wartości. Informacje te są przydatne podczas korzystania z [Biblioteki zarządzania Azure](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Poprzedzającego kodu identyfikator zasobu, aby użyć jest pełną ścieżkę do żądanego zasobu Azure. Na przykład wykonać kwerendę aplikacji sieci Web programu Azure, identyfikator zasobu jest następujący:

*/Subscriptions/{Subscription-ID}/resourceGroups/{Resource-group-name}/providers/Microsoft.Web/Sites/{Site-Name}/*

Poniższa lista zawiera kilka przykładów formatów identyfikator zasobów dla różnych zasobów Azure:

* **Centrum IoT** - /subscriptions/_{identyfikator subskrypcji}_/resourceGroups/_{ruch Nazwa zasobu grupy}_/providers/Microsoft.Devices/IotHubs/_{iot Centrum nazwa}_

* **Elastyczne puli SQL** - /subscriptions/_{identyfikator subskrypcji}_/resourceGroups/_{ruch Nazwa zasobu grupy}_/providers/Microsoft.Sql/servers/_{puli db}_/elasticpools/_{sql puli nazwa}_

* **Baza danych SQL (w wersji 12)** - /subscriptions/_{identyfikator subskrypcji}_/resourceGroups/_{ruch Nazwa zasobu grupy}_/providers/Microsoft.Sql/servers/_{Nazwa serwera}_/databases/_{nazwy bazy danych}_

* **Usługa Bus** - /subscriptions/_{identyfikator subskrypcji}_/resourceGroups/_{ruch Nazwa zasobu grupy}_/providers/Microsoft.ServiceBus/_{nazw}_/_{servicebus nazwa}_

* **Zestawy skali maszyn wirtualnych** - /subscriptions/_{identyfikator subskrypcji}_/resourceGroups/_{ruch Nazwa zasobu grupy}_/providers/Microsoft.Compute/virtualMachineScaleSets/_{maszyn wirtualnych nazwa}_

* **Maszyny wirtualne** - /subscriptions/_{identyfikator subskrypcji}_/resourceGroups/_{ruch Nazwa zasobu grupy}_/providers/Microsoft.Compute/virtualMachines/_{maszyn wirtualnych nazwa}_

* **Koncentratory zdarzenia** - /subscriptions/_{identyfikator subskrypcji}_/resourceGroups/_{ruch Nazwa zasobu grupy}_/providers/Microsoft.EventHub/namespaces/_{przestrzeń nazw eventhub}_


Istnieje kilka alternatywnych rozwiązań do pobierania identyfikator zasobu, również za pomocą Eksploratora zasobów Azure, wyświetlanie żądanego zasobu w portalu Azure i przy użyciu programu PowerShell lub interfejsu wiersza polecenia Azure.

### <a name="azure-resource-explorer"></a>Eksplorator Azure zasobów
Aby znaleźć identyfikator zasobów dla żądanego zasobu, jeden ze sposobów pomocne jest narzędzie [Azure Eksploratora zasobów](https://resources.azure.com) . Przejdź do żądanego zasobu, a następnie spójrz na identyfikator pokazano, jak następujące zrzut ekranu:

![ALT "Azure zasobów Eksploratorze"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure portal
Identyfikator zasobu można uzyskać również z poziomu portalu Azure. Aby to zrobić, przejdź do odpowiedniej zasobu, a następnie wybierz właściwości. Identyfikator zasobu w karta właściwości są wyświetlane w następujących zrzut ekranu:

![ALT "identyfikator zasobu wyświetlana w karta właściwości w portalu Azure"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure programu PowerShell
Identyfikator zasobu mogą być pobierane przy użyciu Azure programu PowerShell oraz poleceń cmdlet. Na przykład aby uzyskać identyfikator zasobów dla aplikacji sieci Web programu Azure, należy wykonać polecenia cmdlet Get-AzureRmWebApp, tak jak w następujących zrzut ekranu:

![ALT "identyfikator zasobu uzyskanych za pośrednictwem programu PowerShell"](./media\monitoring-rest-api-walkthrough\resourceid_powershell.png)

### <a name="azure-cli"></a>Polecenie Azure
Aby pobrać identyfikator zasobu przy użyciu interfejsu wiersza polecenia Azure, należy wykonać polecenia "Pokaż azure aplikacji sieci Web" Określanie "— json' opcja, jak pokazano w poniższej zrzut ekranu:

![ALT "identyfikator zasobu uzyskanych za pośrednictwem programu PowerShell"](./media\monitoring-rest-api-walkthrough\resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Pobieranie danych dziennika aktywności
Oprócz pracy z definicji metryczne i powiązanych wartości, użytkownik może również pobrać dodatkowe wniosków interesujące związane z zasobami Azure. Na przykład jest możliwe dane [dziennika aktywności](https://msdn.microsoft.com/library/azure/dn931934.aspx) kwerendy. Poniższy przykład przedstawia przy użyciu interfejsu API usługi REST Monitor Azure zbadać aktywności dziennika danych w ramach określonego zakresu dat dla subskrypcji usługi Azure:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Następne kroki
* Przejrzyj [monitorowania — omówienie](monitoring-overview.md).
* Wyświetlanie [metryki obsługiwane przy użyciu monitora Azure](monitoring-supported-metrics.md).
* Przejrzyj [Platformy Microsoft Azure Monitor pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Przejrzyj [Biblioteka zarządzania Azure](https://msdn.microsoft.com/library/azure/mt417623.aspx).
