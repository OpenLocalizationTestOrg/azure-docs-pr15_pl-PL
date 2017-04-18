<properties
    pageTitle="Azure próbki szybki start dla programu PowerShell Monitor. | Microsoft Azure"
    description="Aby uzyskać dostęp do funkcji Monitora Azure, takich jak autoscale, alertów, webhooks i przeszukiwanie dzienników aktywności za pomocą programu PowerShell."
    authors="kamathashwin"
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
    ms.date="10/26/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure próbki szybki start dla programu PowerShell monitora

To przykładowe polecenia programu PowerShell ułatwiające dostęp do funkcji Monitora Azure zawiera artykuł. Azure Monitor pozwala AutoScale usług w chmurze, maszyn wirtualnych i aplikacji sieci Web, jak i do wysyłanie alertów i połączeń adresy URL sieci web na podstawie wartości danych telemetrycznego skonfigurowane.

>[AZURE.NOTE] Monitorowanie Azure jest nową nazwę proponowaną "Azure wniosków" do 25-wrz 2016. Jednak obszarów nazw, a więc poniżej poleceń nadal zawierać "wniosków".

## <a name="set-up-powershell"></a>Konfigurowanie programu PowerShell
Jeśli jeszcze tego nie zrobiono, należy skonfigurować programu PowerShell do uruchomienia na komputerze. Aby uzyskać więcej informacji zobacz [jak instalowanie i konfigurowanie programu PowerShell](../powershell-install-configure.md) .

## <a name="examples-in-this-article"></a>Przykłady w tym artykule

W przykładach w artykule przedstawiono, jak można użyć poleceń cmdlet Azure Monitor. Można także przeglądać całą listę poleceń cmdlet środowiska PowerShell Monitor Azure na [polecenia cmdlet Azure Monitor (wniosków)](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).


## <a name="sign-in-and-use-subscriptions"></a>Zaloguj się i używanie subskrypcji

Najpierw zaloguj się do subskrypcji usługi Azure.

```PowerShell
Login-AzureRmAccount
```

W tym celu możesz się zalogować. Po wykonaniu swojego konta, są wyświetlane TenantId i domyślny identyfikator subskrypcji. Azure cmdlet działają w ramach subskrypcji domyślne. Aby wyświetlić listę subskrypcje, do których masz dostęp, użyj następującego polecenia.

```PowerShell
Get-AzureRmSubscription
```

Aby zmienić kontekst pracy do innej subskrypcji, użyj następującego polecenia.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-audit-logs-for-a-subscription"></a>Pobieranie dzienników inspekcji dla subskrypcji
Używanie `Get-AzureRmLog` polecenia cmdlet.  Poniżej przedstawiono kilka typowych przykładów.

Zapoznaj się z tym data/godzina do prezentowania wpisy dziennika:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Uzyskiwanie wpisy dziennika zakresu Data/Godzina:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Pobieranie wpisy dziennika z określonej grupy zasobów:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Pobieranie wpisy dziennika z zakresu Data/Godzina w polu Dostawca określonego zasobu:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Pobierz wszystkie wpisy dziennika z określonym rozmówcy:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

Poniższe polecenie pobiera ostatnio 1000 zdarzenia z dziennika inspekcji:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`obsługuje wiele inne parametry. Zobacz `Get-AzureRmLog` uzyskać więcej informacji.

>[AZURE.NOTE] `Get-AzureRmLog`zapewnia tylko 15 dni historii. Przy użyciu parametru **- MaxEvents** umożliwia kwerendy ostatniego N zdarzeń poza 15 dni. Aby starsze niż 15 dni zdarzenia w programie access należy użyć interfejsu API usługi REST lub SDK (C# próbka przy użyciu zestawu SDK). Jeśli nie zawiera **czas rozpoczęcia**, następnie wartością domyślną jest **zakończenia** minus jedna godzina. Jeśli nie powinno obejmować **zakończenia**, wartością domyślną jest bieżącą godzinę. Zawsze są w czasie UTC.

## <a name="retrieve-alerts-history"></a>Pobieranie historii alertów
Aby wyświetlić wszystkie zdarzenia alert, mogą wysyłać kwerendy Dzienniki Menedżera zasobów Azure za pomocą poniższych przykładach.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Aby wyświetlić historię dla określonych reguł alertów, można użyć `Get-AzureRmAlertHistory` polecenia cmdlet, przekazując identyfikator zasobu reguły alertu.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

`Get-AzureRmAlertHistory` Polecenia cmdlet obsługuje różne parametry. Więcej informacji, zobacz [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## <a name="retrieve-information-on-alert-rules"></a>Pobieranie informacji o regułach alertów
Wszystkie z następujących poleceń działa na grupę zasobów o nazwie "montest".

Wyświetlanie wszystkich właściwości reguły alertu:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Pobierz wszystkie alerty dla grupy zasobów:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Pobierz wszystkie reguły alertów, ustaw dla zasobu docelowego. Na przykład wszystkie reguły alertów ustawiać maszyny.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`obsługuje inne parametry. Aby uzyskać więcej informacji, zobacz [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) .

## <a name="create-alert-rules"></a>Tworzenie reguł alertów
Możesz użyć `Add-AlertRule` polecenia cmdlet, aby tworzyć, aktualizować i wyłączyć regułę alertu.

Można tworzyć wiadomości e-mail i webhook właściwości za pomocą `New-AzureRmAlertRuleEmail` i `New-AzureRmAlertRuleWebhook`odpowiednio. Polecenia cmdlet alertu przypisywanie je jako akcje do właściwości **Akcje** reguły alertu.

Następnej sekcji zawiera próbki, w którym pokazano, jak utworzyć regułę alertu z różnych parametrów.

### <a name="alert-rule-on-a-metric"></a>Reguła alertu na metryki
W poniższej tabeli opisano wartości parametrów i umożliwia tworzenie alertu przy użyciu metryki.


|Parametr|wartość|
|---|---|
|Nazwa|  simpletestdiskwrite|
|Lokalizacja ta reguła alertu|   Wschodniej Stany Zjednoczone|
|Grupa zasobów| montest|
|TargetResourceId|  /Subscriptions/S1/resourceGroups/montest/Providers/Microsoft.COMPUTE/virtualMachines/testconfig|
|MetricName alertu, który jest tworzony|   \Disk \PhysicalDisk (_Total) / s. Zobacz `Get-MetricDefinitions` polecenia cmdlet poniżej o sposób pobierania dokładne nazwy metryczne|
|operator|  Większe|
|Wartość progowa (liczba/s w dla tej metryki)|    1|
|Rozmiar_okna (format gg)|  00:05:00|
|Agregator (statystyczny metryki, w tym przypadku używający średnia liczba)|  Średnia|
|niestandardowe wiadomości e-mail (ciąg tablica)|'foo@example.com','bar@example.com'|
|Wysyłanie wiadomości e-mail do właścicieli, uczestników i czytniki|    -SendToServiceOwners|

Utworzenie wiadomości E-mail

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Utworzenie Webhook

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Utwórz regułę alertu na metryki % Procesora na klasyczny maszyn wirtualnych

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Pobieranie reguły alertu

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Polecenia cmdlet alert Dodaj umożliwia również aktualizowanie reguły jeśli regułę alertu już istnieje dla danej właściwości. Aby wyłączyć regułę alertu, należy dodać parametr **- DisableRule**.

### <a name="alert-on-audit-log-event"></a>Alert na zdarzeń dziennika inspekcji

>[AZURE.NOTE] Ta funkcja jest nadal w podglądzie.

W tym scenariuszu będzie wysyłanie wiadomości e-mail, gdy w subskrypcji w grupie zasobów *abhingrgtest123*zostanie pomyślnie uruchomiona witryny sieci Web.

Konfigurowanie reguły wiadomości e-mail

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Konfigurowanie reguły webhook

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Utwórz regułę dla zdarzenia

```PowerShell
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

Pobieranie reguły alertu

```PowerShell
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

`Add-AlertRule` Polecenia cmdlet umożliwia różne inne parametry. Więcej informacji, zobacz [Dodawanie AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Uzyskiwanie listy dostępne wskaźniki alertów
Możesz użyć `Get-AzureRmMetricDefinition` polecenia cmdlet, aby wyświetlić listę wszystkich wskaźników dla określonego zasobu.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Poniższy przykład generuje tabelę z metryki nazwę oraz jednostką go.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Pełną listę dostępnych opcji dla `Get-AzureRmMetricDefinition` jest dostępna w [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).


## <a name="create-and-manage-autoscale-settings"></a>Tworzenie i zarządzanie ustawieniami AutoScale
Zasób, na przykład aplikacji sieci Web, maszyn wirtualnych, usługa w chmurze i Ustawianie skali maszyn wirtualnych może mieć tylko jedno ustawienie autoscale odpowiednio skonfigurowany.
Każde ustawienie autoscale można jednak profili. Na przykład jedną dla profilu na podstawie skali i drugi dla serii rozłożonych w czasie na podstawie profilu. Każdego profilu można mieć wiele reguł skonfigurowane na nim. Aby uzyskać więcej informacji o Autoscale zobacz [jak Autoscale aplikacji](../cloud-services/cloud-services-how-to-scale.md).

Poniżej przedstawiono czynności, które użyjemy:

1. Tworzenie reguły.
2. Tworzenie profilów mapowanie reguły, utworzone wcześniej do profilów.
3. Opcjonalnie: Tworzenie powiadomienia o autoscale konfigurując właściwości webhook i poczty e-mail.
4. Tworzenie ustawienie autoscale przy użyciu nazwy zasobu docelowej dzięki mapowaniu profilów i powiadomień, które zostały utworzone w poprzednich krokach.

W poniższych przykładach pokazano, jak można utworzyć ustawienie Autoscale Skala maszyn wirtualnych, ustaw dla systemu Windows za pomocą metryki wykorzystania Procesora.

Najpierw należy utworzyć regułę, aby skala w nowym oknie, z większa liczba wystąpienie.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```     

Następnie utwórz regułę skali przy, zmniejszenie liczby wystąpienie.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Następnie utwórz profil zasady.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Tworzenie właściwości webhook.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Tworzenie właściwości powiadomienie ustawienie autoscale, takich jak wiadomości e-mail i webhook, utworzony wcześniej.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Na koniec Utwórz ustawienie autoscale, aby dodać profil, który został utworzony powyżej.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Aby uzyskać więcej informacji na temat zarządzania ustawieniami Autoscale zobacz [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Historia Autoscale
W poniższym przykładzie pokazano sposób wyświetlenia ostatnich autoscale i zdarzeń alertów. Umożliwia wyświetlanie historii Autoscale wyszukiwania dziennika inspekcji.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Możesz użyć `Get-AzureRmAutoScaleHistory` polecenia cmdlet, aby pobrać AutoScale historii.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Aby uzyskać więcej informacji zobacz [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Wyświetlanie szczegółów ustawienia autoscale
Możesz użyć `Get-Autoscalesetting` polecenia cmdlet, aby pobrać więcej informacji na temat konfigurowania autoscale.

W poniższym przykładzie pokazano szczegółowe informacje o wszystkich ustawień autoscale w grupie zasobów "myrg1".

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

W poniższym przykładzie pokazano szczegółowe informacje o wszystkich ustawień autoscale w grupie zasobów "myrg1" i specjalnie ustawienie autoscale o nazwie "MyScaleVMSSSetting".

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Usuwanie ustawienia autoscale
Możesz użyć `Remove-Autoscalesetting` polecenia cmdlet, aby usunąć ustawienie autoscale.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-audit-logs"></a>Zarządzanie profilami dziennika dzienników inspekcji

Można utworzyć *profilu dziennika* eksportowanie danych z dzienników inspekcji konta miejsca do magazynowania i przechowywanie danych można skonfigurować dla niego. Opcjonalnie możesz również przesyłać strumieniowo dane do usługi Centrum zdarzenia. Należy zauważyć, że ta funkcja jest obecnie w wersji Preview i można utworzyć tylko jeden profil dziennika na subskrypcję. Następujące polecenia cmdlet z bieżącej subskrypcji umożliwia tworzenie i zarządzanie profilami dziennika. Możesz również wybrać danej subskrypcji. Mimo że programu PowerShell domyślnie bieżącej subskrypcji, możesz zmienić tego za pomocą `Set-AzureRmContext`. Możesz skonfigurować dzienników inspekcji na przesyłanie danych do dowolnego miejsca do magazynowania konta lub Centrum wydarzenie w tej subskrypcji. Dane są zapisywane jako pliki obiektów blob w formacie JSON.

### <a name="get-a-log-profile"></a>Pobierz profil dziennika
Aby uzyskiwanie zdalnego dostępu do istniejącego profilu dziennika, użyj `Get-AzureRmLogProfile` polecenia cmdlet.

### <a name="add-a-log-profile-without-data-retention"></a>Dodawanie profilu dziennika bez przechowywania danych

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Usuwanie profilu dziennika

```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Dodawanie profilu dziennika z przechowywanie danych

Właściwość **- RetentionInDays** można określić liczbę dni, jako dodatnią liczbą całkowitą, w którym dane są zachowywane.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Dodawanie profilu dziennika z zasad przechowywania i EventHub
Oprócz routingu danych do konta miejsca do magazynowania, możesz również przesyłać strumieniowo go do koncentratora zdarzenia. Pamiętaj, że w tej wersji preview i przechowywania Konfiguracja konta jest obowiązkowe, ale konfiguracja Centrum zdarzenie jest opcjonalna.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Konfigurowanie dzienników diagnostycznych
Wiele usług Azure udostępnia dodatkowe dzienniki i telemetrycznego, w tym grupy zabezpieczeń sieci Azure, urządzenia do równoważenia obciążenia oprogramowania, klucza magazynu usługi wyszukiwania Azure, i logiki aplikacji i mogą być skonfigurowane na zapisywanie danych na swoim koncie magazyn Azure. Tej operacji można wykonać tylko na poziomie zasobów, a następnie konta miejsca do magazynowania powinny znajdować się w tym samym regionie jako zasobu docelowego, miejsce, w którym skonfigurowano ustawienie diagnostyki.

### <a name="get-diagnostic-setting"></a>Pobieranie ustawienia diagnostyczne

```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Ustawienie diagnostyczne

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Ustawienie diagnostyczne bez przechowywania

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Ustawienie diagnostyczne z zasad przechowywania

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Ustawienie diagnostyczne z przechowywania dla kategorii określonych log

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```
