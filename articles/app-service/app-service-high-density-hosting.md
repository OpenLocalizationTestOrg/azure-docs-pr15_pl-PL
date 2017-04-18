<properties
    pageTitle="Duża gęstość hostingu na Azure aplikacji usługi | Microsoft Azure"
    description="Duża gęstość hostingu na Azure aplikacji usługi"
    authors="btardif"
    manager="wpickett"
    editor=""
    services="app-service\web"
    documentationCenter=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"/>

# <a name="high-density-hosting-on-azure-app-service"></a>Duża gęstość hostingu na Azure aplikacji usługi

Podczas korzystania z aplikacji usługi, aplikacja jest odłączona z możliwości przydzielone do niego przez dwa pojęcia:

- **Aplikacji:** Reprezentuje aplikację i konfigurację runtime. Na przykład zawiera wersję programu .NET, które powinny być pobierane środowisko uruchomieniowe, ustawień aplikacji itp.

- **Plan usług aplikacji:** Określa cechy pojemność, zestaw funkcji dostępny i lokalizacji aplikacji. Na przykład właściwości może być duży komputera (cztery rdzenie), cztery wystąpienia, funkcji Premium wschodniego w USA.

Aplikacji jest zawsze połączony z planu usługi aplikacji, ale plan usług aplikacji zapewnia możliwości jeden lub więcej aplikacji.

W wyniku platformie udostępnia możliwość Izolowanie aplikacji jednego lub wielu aplikacje współużytkowanie zasobów dzięki udostępnieniu plan usług aplikacji.

Jednak wiele aplikacji udostępniający plan usług aplikacji wystąpienie tej aplikacji działa na każde wystąpienie tego plan usług aplikacji.

## <a name="per-app-scaling"></a>Na skalowanie aplikacji
*Na skalowanie aplikacji* jest funkcją, której mogą być włączona na poziomie plan aplikacji usługi, a następnie używane dla aplikacji.

Na aplikacji skalowania skale aplikacji niezależnie od planu usług aplikacji, którym jest obsługiwana. W ten sposób plan usług aplikacji można skonfigurować zapewnienie 10 wystąpieniach, ale aplikacji można ustawić przeskalować tylko 5 ich.

Następujący szablon Menedżera zasobów Azure tworzy plan aplikacji usługi, który skalowania do 10 wystąpieniach i aplikację, który jest skonfigurowany do używania w jednym skalowania aplikacji i Skaluj do wystąpienia tylko 5.

Plan usług aplikacji jest ustawienie właściwości **skalowania poszczególnych witryn** PRAWDA ( `"perSiteScaling": true`). Aplikacja jest ustawianie **wielu pracowników** za pomocą 5 (`"properties": { "numberOfWorkers": "5" }`).

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "5" }
             } ]
         }]
    }


## <a name="recommended-configuration-for-high-density-hosting"></a>Zalecana konfiguracja dla dużej gęstości hostingu

Na skalowanie aplikacji jest funkcję, która jest włączona zarówno w publicznej Azure regionów, jak i w środowisku usługi aplikacji. Zalecana strategia jest jednak za pomocą aplikacji usługi środowiskach zalet funkcji zaawansowanych i pule większe pojemności.  

Wykonaj poniższe czynności, aby skonfigurować dużej gęstości hostingu dla aplikacji:

1. Konfigurowanie środowiska usługi aplikacji i wybierz pozycję roboczych, służącego do tego scenariusza hostingu dużej gęstości.

1. Tworzenie planu jednej aplikacji usługi i skalować je do użycia dostępne możliwości puli pracownika.

1. Plan aplikacji usługi, należy ustawić flagę skalowania poszczególnych witryn na PRAWDA.

1. Do tworzenia nowych witryn i przypisana do tego planu aplikacji usługi z właściwością **numberOfWorkers** ustawioną na wartość **1**. Przy użyciu tej konfiguracji daje gęstości najwyższe możliwe na tej puli pracownika.

1. Liczba pracowników mogą zostać skonfigurowane niezależnie dla każdej witryny, aby udzielić dodatkowe zasoby, stosownie do potrzeb. Na przykład witryny użycia wysoki ustawić **numberOfWorkers** do **3** , aby mieć więcej możliwości przetwarzania dla tej aplikacji, podczas gdy min. użycia witryny ustawić **numberOfWorkers** na **1**.
