<properties
   pageTitle="Menedżer zasobów obsługiwane usługi | Microsoft Azure"
   description="W tym artykule opisano zasobu do grupy dostawców obsługujących Menedżera zasobów, ich schematów i dostępne wersje interfejsu API i regionów, które mogą obsługiwać zasobów."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="magoedte;tomfitz"/>

# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Menedżer zasobów dostawców, regionów, wersje interfejsu API i schematów

Menedżer zasobów Azure udostępnia nowy sposób wdrażanie i zarządzanie usługami, które składają się z aplikacji. Większość, ale nie wszystkich, usług pomocy technicznej Menedżera zasobów, a niektóre usługi obsługuje tylko częściowo Menedżera zasobów. Ten temat zawiera listę dostawców obsługiwane zasobów dla Menedżera zasobów Azure.

Wdrażając zasobów, należy ustalić tych zasobów pomocy technicznej regiony, które wersje interfejsu API są dostępne dla zasobów. Sekcji [regionów obsługiwane](#supported-regions) pokazano, jak sprawdzić, jakie regiony pracy subskrypcję i zasobów. Sekcji [wersje obsługiwane interfejsu API](#supported-api-versions) pokazano, jak ustalić, które wersje interfejsu API, możesz użyć.

Aby zobaczyć, usług, które są obsługiwane w Azure portal i klasyczny portalu, zobacz [dostępność portal Azure wykresu](https://azure.microsoft.com/features/azure-portal/availability/). Aby wyświetlić przenoszenie zasobów pomocy technicznej usługi, zobacz [Przenoszenie zasobów do nowej grupy zasobów lub innej subskrypcji](resource-group-move-resources.md).

W poniższej tabeli wymieniono, które usług firmy Microsoft do wdrażania pomocy technicznej i zarządzanie Menedżera zasobów i nie. Łącze w kolumnie **Szablony Szybki Start** wysyła kwerendę do repozytorium Azure szablony Szybki Start dla dostawcy określonego zasobu. Szybki Start szablony Dodawanie i aktualizowanie często. Obecność łącze określonej usługi musi oznaczać, że kwerenda zwraca szablonów z repozytorium. Istnieje wiele zasobów innych firm do grupy dostawców obsługujących Menedżera zasobów. Jak wyświetlić wszystkich dostawców zasobu w sekcji [dostawcy zasobów i typów](#resource-providers-and-types) .


## <a name="compute"></a>Obliczenia

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | ------------------------ |-------- | ------ | ------ |
| Partii   | Tak     | [Partia REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) | [2015-12-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json)       |  |
|Kontener | Tak | [Kontener usługi REST](https://msdn.microsoft.com/library/azure/mt711470.aspx) | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) | [Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Usługi Dynamics cyklu | Tak  |      |        |  |
| Zestawy skali | Tak | [Skala Ustaw pozostałe](https://msdn.microsoft.com/library/azure/mt705635.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) | 
| Usługa tkaninie | Tak  | [Usługa tkaninie Rest](https://msdn.microsoft.com/library/azure/dn707692.aspx)  |      | [Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Maszyn wirtualnych | Tak | [POZOSTAŁYCH MASZYN WIRTUALNYCH](https://msdn.microsoft.com/library/azure/mt163647.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Maszyn wirtualnych (klasyczny) | Ograniczone | - | - | - |
| Aplikacja Remote | Brak      | -  | - | - |
| Usług w chmurze (klasyczny) | Ograniczone (patrz poniżej) | - | - | - |

Maszyn wirtualnych (klasyczny) odwołuje się do zasobów wdrożone za pośrednictwem modelu Klasyczny wdrożenia zamiast za pośrednictwem modelu wdrożenia Menedżera zasobów. Zazwyczaj te zasoby nie obsługują operacje Menedżera zasobów, ale niektórych działań, które zostały włączone. Aby uzyskać więcej informacji o tych modeli wdrażania zobacz [wdrożenia opis Menedżera zasobów i wdrażania klasyczny](resource-manager-deployment-model.md). 

Usług w chmurze (klasyczny) może być używany z innymi zasobami klasyczny; Jednak zasoby klasyczny nie korzystać ze wszystkich funkcji Menedżera zasobów i nie są dobrym rozwiązaniem dla przyszłych rozwiązań. Należy rozważyć zmianę infrastruktury aplikacji korzystać z zasobów z nazw Microsoft.Compute, Microsoft.Storage i Microsoft.Network.


## <a name="networking"></a>Sieci

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | -------  | -------- | ------ | ------ |
| Brama aplikacji | Tak | [Brama aplikacji REST](https://msdn.microsoft.com/library/azure/mt684939.aspx)   |        | [applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS     | Tak | [UMIEŚĆ DNS](https://msdn.microsoft.com/library/azure/mt163862.aspx)         | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) | [dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute | Tak  | [ExpressRoute pozostałe](https://msdn.microsoft.com/library/azure/mt586720.aspx)  |       | [expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| Usługa równoważenia obciążenia | Tak  | [Usługi równoważenia obciążenia REST](https://msdn.microsoft.com/library/azure/mt163651.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Menedżer ruchu | Tak | [Umieść Menedżer ruchu](https://msdn.microsoft.com/library/azure/mt163667.aspx) | [2015-11-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json)  | [trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Wirtualnych sieci | Tak| [Wirtualna sieć REST](https://msdn.microsoft.com/en-us/library/azure/mt163650.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| Brama VPN | Tak | [Brama sieci REST](https://msdn.microsoft.com/library/azure/mt163859.aspx) |  | [virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[połączenia](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |


## <a name="storage"></a>Miejsca do magazynowania

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | ------- | -------- | ------ | ------- | ------ |
| Miejsca do magazynowania | Tak  | [Magazyn REST](https://msdn.microsoft.com/library/azure/mt163683.aspx) | [Konto miejsca do magazynowania](resource-manager-template-storage.md) | [Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| StorSimple | Tak  |         |        |  |

## <a name="databases"></a>Bazy danych

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | ------- | -------- | ------ | ------- | ------ |
| DocumentDB | Tak  | [DocumentDB pozostałe](https://msdn.microsoft.com/library/azure/dn781481.aspx) | [2015-04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json)  | [Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Redis pamięci podręcznej | Tak |   | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) | [Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| Baza danych SQL | Tak | [Baza danych SQL REST](https://msdn.microsoft.com/library/azure/mt163571.aspx) | [2014-04-01-Podgląd](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) | [Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| Magazyn danych SQL | Tak |   |      |


## <a name="web--mobile"></a>Web & na telefon komórkowy

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | ------- | -------- | ------ | ------ |
| Interfejs API aplikacji | Tak |   | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Interfejs API aplikacji](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| Interfejs API zarządzania | Tak  | [Interfejs API zarządzania REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) | [2016-07-07](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-07-07/Microsoft.ApiManagement.json)       | [Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) | 
| Moderator zawartości | Tak |   |   |   |
| Funkcja aplikacji | Tak |   |   | [functionApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22functionApp%22&type=Code) |
| Aplikacje warunków logicznych | Tak   | [Interfejsu API usługi REST usługi przepływu pracy](https://msdn.microsoft.com/library/azure/mt643787.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.Logic.json) | [Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Aplikacje dla urządzeń przenośnych | Tak |  | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json)  | [mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code)   |
| Pozyskiwaniu urządzeń przenośnych | Tak  |  [Urządzeń przenośnych zaangażowania REST](https://msdn.microsoft.com/library/azure/mt683754.aspx)        |        | [Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Wyszukiwanie | Tak  | [Wyszukiwanie REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) |  | [Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| Aplikacje sieci Web | Tak |          | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## <a name="analytics"></a>Analizy

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | -------  | -------- | ------ | ------ |
| Wykaz danych | Tak  | [Wykaz danych pozostałych](https://msdn.microsoft.com/library/azure/mt267595.aspx)  | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) |  |
| Factory danych | Tak | [Factory danych pozostałych](https://msdn.microsoft.com/library/azure/dn906738.aspx) |    | [Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Analizy Lake danych | Tak |   |   [2015-10-01-Podgląd](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Magazyn Lake danych | Tak  | [Umieść magazynu Lake danych](https://msdn.microsoft.com/library/azure/mt693424.aspx) | [2015-10-01-Podgląd](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights | Tak | [HDInsights pozostałe](https://msdn.microsoft.com/library/azure/mt622197.aspx) |        | [Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Nauka komputera | Tak | [Maszynowego uczenia REST](https://msdn.microsoft.com/library/azure/mt767538.aspx)        | [2016-05-01-Podgląd](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) |
| Analizy strumieniu | Tak  | [Pary analizy REST](https://msdn.microsoft.com/library/azure/dn835031.aspx)   |        |  |
| Power BI | Tak | [Power BI osadzony REST](https://msdn.microsoft.com/library/azure/mt712303.aspx) | [2016-01-29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) |  |

## <a name="intelligence"></a>Analizy

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | ------- | -------- | ------ | ------ |
| Poznawcze usług | Tak |  | [2016-02-01-Podgląd](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) |   |

## <a name="internet-of-things"></a>Internet rzeczy

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | ------- | -------- | ------ | ------ |
| Centrum zdarzenia | Tak  | [Centrum zdarzeń pozostałych](https://msdn.microsoft.com/library/azure/dn790674.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.EventHub.json) | [Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code)  |
| IoTHubs | Tak | [Centrum IoT REST](https://msdn.microsoft.com/library/azure/mt589014.aspx) | [2016-02-03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) | [Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Powiadomienie o koncentratory | Tak | [Powiadomienie o Centrum REST](https://msdn.microsoft.com/library/azure/dn495827.aspx) | [2015-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) | [Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## <a name="media--cdn"></a>Multimedia i sieci CDN

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | ------- | -------- | ------ | ------ |
| SIECI CDN | Tak | [POZOSTAŁE SIECI CDN](https://msdn.microsoft.com/library/azure/mt634456.aspx)  | [2016-04-02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) | [Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| Usługi multimediów | Tak | [Usługi multimediów REST](https://msdn.microsoft.com/library/azure/hh973617.aspx)  | [2015-10-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) |


## <a name="hybrid-integration"></a>Integracja hybrydowego

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | ------- | -------- | ------ | ------ |
| BizTalk usług | Tak |          | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |  |
| Usługa odzyskiwania | Tak | [Odzyskiwanie witryny REST](https://msdn.microsoft.com/library/azure/mt750497.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) | [Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Usługa Bus | Tak | [Bus usługi REST](https://msdn.microsoft.com/library/azure/mt639375.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.ServiceBus.json)       | [Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## <a name="identity--access-management"></a>Tożsamości i zarządzanie dostępem 

Azure Active Directory działa przy użyciu Menedżera zasobów umożliwiający kontrola dostępu oparta na rolach dla subskrypcji. Aby uzyskać informacje o użyciu kontrola dostępu oparta na rolach i usługi Active Directory, zobacz [Kontrola dostępu oparta na rolach Azure](./active-directory/role-based-access-control-configure.md).

## <a name="developer-services"></a>Usług dla deweloperów 

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | ------- | -------- | ------ | ------ |
| Wnioski aplikacji | Tak  | [Wnioski REST](https://msdn.microsoft.com/library/azure/dn931943.aspx)  | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) | [Microsoft.insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Mapy Bing | Tak  |          |        |  |
| DevTest Labs | Tak |   | [2016-05-15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) | [Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code)  |
| Konto programu Visual Studio | Tak   |          | [2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |  |

## <a name="management-and-security"></a>Zarządzanie i zabezpieczeń

| Usługa | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | ------- | -------- | ------ | ------ |
| Automatyzacji | Tak | [Automatyzacja REST](https://msdn.microsoft.com/library/azure/mt662285.aspx) | [2015-10-31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json)       | [Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Kluczowe magazynu | Tak | [Kluczowe magazynu REST](https://msdn.microsoft.com/library/azure/dn903609.aspx) | [Kluczowe magazynu](resource-manager-template-keyvault.md)<br />[Tajny klucza magazynu](resource-manager-template-keyvault-secret.md)   | [Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Wnioski operacyjne | Tak |          |        |  |
| Harmonogram | Tak  | [Harmonogram REST](https://msdn.microsoft.com/library/azure/mt629143.aspx)     | [2016-03-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-01/Microsoft.Scheduler.json) | [Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| Zabezpieczenia (wersja preview) | Tak | [Zabezpieczenia pozostałych](https://msdn.microsoft.com/library/azure/mt704034.aspx)  |  | [Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code)  |

## <a name="resource-manager"></a>Menedżer zasobów

| Funkcja | Korzystanie z Menedżera zasobów | INTERFEJSU API USŁUGI REST | Schematu | Szablony Szybki Start |
| ------- | ------- | -------------- | -------- | ------ | ------ |
| Autoryzacja | Tak | [Zarządzanie blokady](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[Kontrola dostępu oparta na rolach](https://msdn.microsoft.com/library/azure/dn906885.aspx)  | [Blokada zasobu](resource-manager-template-lock.md)<br />[Przypisania ról](resource-manager-template-role.md)  | [Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| Zasoby | Tak | [Zasoby połączone](https://msdn.microsoft.com/library/azure/mt238499.aspx) | [Łącza do zasobów](resource-manager-template-links.md) | [Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |


## <a name="resource-providers-and-types"></a>Typy i dostawców zasobów

Wdrażając zasobów, często jest potrzebne do pobierania informacji na temat typów i dostawców zasobów. Można pobrać tych informacji za pośrednictwem interfejsu API usługi REST, Azure programu PowerShell lub interfejsu wiersza polecenia Azure.

Aby pracować z dostawcą zasobów, tego dostawcy zasobów musi być zarejestrowany za pomocą konta. Domyślnie wielu dostawców zasobu są automatycznie rejestrowane; jednak może być konieczne ręczne rejestrowanie niektórych dostawców zasobów. W przykładach poniżej pokazano jak sprawdzić stan rejestracji dostawcy zasobów i zarejestrować dostawcy zasobów, w razie potrzeby.

### <a name="portal"></a>Portal

Można łatwo wyświetlić listę dostawców zasoby obsługiwane następujące czynności:

1. Wybierz pozycję **Moje uprawnienia**.

    ![Moje uprawnienia](./media/resource-manager-supported-services/my-permissions.png)

2. Wybierz **Stan dostawcy zasobów**

    ![Stan dostawcy zasobów](./media/resource-manager-supported-services/resource-provider-status.png)

3. Możesz wyświetlić pełną listę dostawców zasobów dla subskrypcji.

    ![Lista dostawców zasobów](./media/resource-manager-supported-services/list-providers.png)

### <a name="rest-api"></a>INTERFEJSU API USŁUGI REST

Do wszystkich zasobów dostępnych dostawców, w tym typy, lokalizacji, wersje interfejsu API i stan rejestracji, użyj operacji [listy wszystkich dostawców zasobów](https://msdn.microsoft.com/library/azure/dn790524.aspx) . Jeśli chcesz zarejestrować dostawcy zasobów, zobacz [rejestrowanie subskrypcji u dostawcy zasobów](https://msdn.microsoft.com/library/azure/dn790548.aspx).

### <a name="powershell"></a>Programu PowerShell

W poniższym przykładzie pokazano sposób uzyskiwania wszystkich dostawców dostępne zasobów.

    Get-AzureRmResourceProvider -ListAvailable
    

W następnym przykładzie pokazano sposób uzyskać typów zasobów z dostawcą danego zasobu.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
    
Wynik jest podobne do:

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...
    
Aby zarejestrować dostawcy zasobów, wprowadź obszaru nazw:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### <a name="azure-cli"></a>Polecenie Azure

W poniższym przykładzie pokazano sposób uzyskiwania wszystkich dostawców dostępne zasobów.

    azure provider list
    
Wynik jest podobne do:

    info:    Executing command provider list
    + Getting registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

Informacje dotyczące dostawcy określonego zasobu można zapisać do pliku przy użyciu następującego polecenia.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Aby zarejestrować dostawcy zasobów, wprowadź obszaru nazw:

    azure provider register -n Microsoft.ServiceBus

## <a name="supported-regions"></a>Obsługiwanych regionów

Wdrażając zasobów, zwykle konieczne Określ region dla zasobów. Menedżer zasobów jest obsługiwany we wszystkich regionach, ale zasoby, które należy rozmieścić mogą nie być obsługiwane we wszystkich regionach. Ponadto mogą wystąpić ograniczenia w subskrypcji usługi uniemożliwiające za pomocą niektórych regionach, które obsługują tego zasobu. Ograniczenia te mogą być związane z podatku problemy dotyczące Twoim kraju lub wynik zasady umieszczany przez administratora subskrypcji, aby użyć tylko w niektórych regionach. 

Aby uzyskać pełną listę wszystkich obsługiwanych regionów w przypadku wszystkich usług Azure zobacz [usług według regionów](https://azure.microsoft.com/regions/#services); jednak lista może zawierać regionów, które nie obsługuje subskrypcji. Można określić regionów dla określonego typu zasobów obsługującego za pośrednictwem portalu, interfejsu API usługi REST, programu PowerShell lub interfejsu wiersza polecenia Azure subskrypcji.

### <a name="portal"></a>Portal

Można wyświetlić obsługiwanych regionów dla typu zasobu przez następujące etapy:

1. Wybierz pozycję **więcej usług** > **Eksploratora zasobów**.

    ![Eksplorator zasobów](./media/resource-manager-supported-services/select-resource-explorer.png)

2. Otwórz węzeł **dostawców** .

    ![Wybierz pozycję dostawców](./media/resource-manager-supported-services/select-providers.png)

3. Wybierz dostawcę zasobów i wyświetlanie obsługiwanych regionów i wersje interfejsu API.

    ![dostawcy widoku](./media/resource-manager-supported-services/view-provider.png)

### <a name="rest-api"></a>INTERFEJSU API USŁUGI REST

Aby odnajdować, regiony są dostępne dla określonego typu zasobów w ramach subskrypcji, użyj operacji [listy wszystkich dostawców zasobów](https://msdn.microsoft.com/library/azure/dn790524.aspx) . 

### <a name="powershell"></a>Programu PowerShell

W poniższym przykładzie pokazano sposób uzyskiwania obsługiwanych regionów witryn sieci web.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
Wynik jest podobne do:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### <a name="azure-cli"></a>Polecenie Azure

W poniższym przykładzie zwracany wszystkie obsługiwane lokalizacje dla każdego typu zasobu.

    azure location list

Możesz również filtrować wyniki lokalizacji z narzędziem JSON, takich jak [jq](https://stedolan.github.io/jq/).

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

Która zwraca:

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## <a name="supported-api-versions"></a>Obsługiwane wersje interfejsu API

Po wdrożeniu szablonu, musisz określić wersję interfejsu API ma być używana do tworzenia każdego zasobu. Wersja API odnosi się do wersji operacje interfejsu API usługi REST, wydawanych przez dostawcę zasobu. Jako dostawca zasobów umożliwia nowych funkcji, udostępnia nową wersję interfejsu API usługi REST. Dlatego wersji interfejsu API określone w szablonie ma wpływ na właściwości, które można określić w szablonie. Na ogół chcesz wybrać najbardziej aktualną wersję interfejsu API, podczas tworzenia szablonów. Dla istniejących szablonów można zdecydować, czy chcesz kontynuować, przy użyciu starszej wersji interfejsu API lub zaktualizować szablon najnowszą wersję, aby można było korzystać z nowych funkcji.

### <a name="portal"></a>Portal

Należy określić obsługiwanych wersji interfejsu API w taki sam sposób określone obsługiwanych regionów (pokazanym wcześniej).

### <a name="rest-api"></a>INTERFEJSU API USŁUGI REST

Aby odkryć, które wersje interfejsu API są dostępne dla typów zasobów, użyj operacji [listy wszystkich dostawców zasobów](https://msdn.microsoft.com/library/azure/dn790524.aspx) . 

### <a name="powershell"></a>Programu PowerShell

W poniższym przykładzie pokazano sposób uzyskiwania dostępnych wersji interfejsu API dla określonego typu zasobów.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
Wynik jest podobne do:
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### <a name="azure-cli"></a>Polecenie Azure

Możesz zapisać informacje (w tym dostępnych wersji interfejsu API) dla dostawcy zasobów do pliku przy użyciu następującego polecenia.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Możesz otworzyć plik i znaleźć elementu **apiVersions**

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje dotyczące tworzenia szablonów Menedżera zasobów, zobacz [Szablony do tworzenia Azure Menedżera zasobów](resource-group-authoring-templates.md).
- Aby dowiedzieć się o wdrażaniu zasobów, zobacz temat [Deploy aplikacji za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy.md).
