<properties
    pageTitle="Azure polecenie Monitor próbki szybki start. | Microsoft Azure"
    description="Przykładowe poleceń interfejsu wiersza polecenia dla funkcji Monitora Azure. Azure Monitor jest usługi Microsoft Azure, dzięki czemu można wysyłać alertów, zadzwoń do adresów URL sieci web na podstawie wartości danych telemetrycznych skonfigurowanych usług w chmurze autoScale, maszyn wirtualnych i aplikacje sieci Web."
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
    ms.date="09/08/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor--cross-platform-cli-quick-start-samples"></a>Azure próbki szybki start polecenie między platformami monitora

W tym artykule przedstawiono przykładowy że interfejs wiersza polecenia (polecenie) polecenia ułatwiające dostęp do funkcji Monitora Azure. Azure Monitor pozwala AutoScale usług w chmurze, maszyn wirtualnych i aplikacji sieci Web, jak i do wysyłanie alertów i połączeń adresy URL sieci web na podstawie wartości danych telemetrycznego skonfigurowane.

>[AZURE.NOTE] Monitorowanie Azure jest nową nazwę proponowaną "Azure wniosków" do 25-wrz 2016. Jednak obszarów nazw, a więc poniżej poleceń nadal zawierać "wniosków".


## <a name="prerequisites"></a>Wymagania wstępne

Jeśli jeszcze nie jest już zainstalowany polecenie Azure, zobacz [Instalowanie polecenie Azure](../xplat-cli-install.md). Jeśli znasz polecenie Azure, możesz więcej informacji o nim przy [użyciu interfejsu wiersza polecenia Azure dla komputerów Mac, Linux i systemu Windows przy użyciu Menedżera zasobów Azure](../xplat-cli-azure-resource-manager.md).


W systemie Windows należy zainstalować npm z [Node.js witryny sieci Web](https://nodejs.org/). Po zakończeniu instalacji, używając CMD.exe z uprawnieniami Uruchom jako Administrator, wykonaj następujące czynności z folderu, w którym zainstalowano npm:

```console
npm install azure-cli --global
```

Następnie przejdź do dowolnej-lokalizację folderu ma i wpisz w wierszu polecenia:

```console
azure help
```

## <a name="log-in-to-azure"></a>Zaloguj się do Azure

Pierwszym krokiem jest logowania na konto Azure.

```console
azure login
```

Po uruchomieniu tego polecenia, musisz zalogować się przy użyciu zgodnie z instrukcjami na ekranie. Później Zobacz konta, TenantId i domyślny identyfikator subskrypcji. Wszystkie polecenia działają w ramach subskrypcji domyślne.

Aby wyświetlić szczegółowe informacje o bieżącej subskrypcji, użyj następującego polecenia.

```console
azure account show
```

Aby zmienić kontekst pracy do innej subskrypcji, użyj następującego polecenia.

```console
azure account set "subscription ID or subscription name"
```

Aby użyć poleceń Menedżera zasobów Azure i monitorowanie Azure, musisz być w trybie Azure Menedżera zasobów.

```console
azure config mode arm
```

Aby wyświetlić listę wszystkich obsługiwanych poleceń Azure Monitor, wykonaj następujące czynności.

```console
azure insights
```

## <a name="view-audit-logs-for-a-subscription"></a>Wyświetlanie dzienników inspekcji dla subskrypcji

Aby wyświetlić listę dzienników inspekcji, wykonaj następujące czynności.

```console
azure insights logs list [options]
```

Spróbuj wykonać poniższe czynności, aby wyświetlić wszystkie dostępne opcje.

```console
azure insights logs list -help
```

Oto przykład dzienniki listy, grupa zasobów

```console
azure insights logs list --resourceGroup "myrg1"
```

Przykład listy dzienniki przez wywołującego

```console
azure insights logs list --caller "myname@company.com"
```

Przykład listy dzienniki przez wywołujący na typu zasobu w obrębie daty rozpoczęcia i zakończenia

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Praca z alertów
Informacje w sekcji służy do pracy z alertów.

### <a name="get-alert-rules-in-a-resource-group"></a>Pobieranie reguł alertów w grupie zasobów

```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Utwórz regułę alertu metryczne

```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-a-log-alert-rule"></a>Utwórz regułę alertu dziennika

```console
azure insights alerts rule log set ruleName eastus resourceGroupName someOperationName
```

### <a name="create-webtest-alert-rule"></a>Utwórz regułę alertu webtest

```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Usuwanie reguły alertu

```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Profile dziennika
Skorzystaj z informacji w tej sekcji do pracy z profilami dziennika.

### <a name="get-a-log-profile"></a>Pobierz profil dziennika

```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Dodawanie profilu dziennika bez przechowywania

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Usuwanie profilu dziennika

```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Dodawanie profilu dziennika z zasad przechowywania

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Dodawanie profilu dziennika z zasad przechowywania i EventHub

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Narzędzia diagnostyczne
Skorzystaj z informacji w tej sekcji, aby pracować z ustawień diagnostycznych.

### <a name="get-a-diagnostic-setting"></a>Pobieranie ustawienia diagnostyczne

```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Ustawienie diagnostyczne

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Ustawienie diagnostyczne bez przechowywania

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Autoscale
Praca z ustawieniami autoscale przy użyciu informacji w tej sekcji. Należy zmodyfikować w tych przykładach.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Pobierz autoscale ustawienia dla grupy zasobów

```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Pobierz ustawienia autoscale według nazwy w grupie zasobów

```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Konfigurowanie ustawień auotoscale

```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
