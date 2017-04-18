
<properties
    pageTitle="Lista portów i adresy URL do listy sprawdzonej dla Azure RemoteApp wdrożony w wirtualnej sieci klienta | Microsoft Azure"
    description="Dowiedz się, które porty i adresy URL musisz skonfigurować dla komunikacji przy użyciu funkcji RemoteApp Azure."
    services="remoteapp"
    documentationCenter=""
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="elizapo" />



# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Listy portów i adresy URL, aby zezwolić na dostęp dla Azure RemoteApp wdrożony w klienta wirtualnej sieci 

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Poniższe informacje dotyczą Azure RemoteApp w chmurze lub hybrydowymi połączeniami zbiorze wdrażając go w wirtualnej sieci (VNET). Aby uzyskać więcej informacji na wirtualnych sieci przeczytaj [Omówienie wirtualnych sieci](../virtual-network/virtual-networks-overview.md). Jeśli użytkownik utworzył grupy zabezpieczeń sieci (NSG) ograniczanie ruch do Twojej zasobów wirtualnej sieci, które wybrano Azure RemoteApp, upewnij się, że następujące czynności są dostępne i dozwolone za pomocą zasad zabezpieczeń w wirtualnej sieci. Aby uzyskać więcej informacji dotyczących grup zabezpieczeń sieci przeczytaj [Co to jest grupa zabezpieczeń sieci? (NSG)](../virtual-network/virtual-networks-nsg.md).

##  <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure podsieci RemoteApp musi mieć dostęp do tych punktów końcowych i adresy URL: 
*   *. servicebus.windows.net
*    *. servicebus.net
*    https://*.RemoteApp.windwsazure.com  
*    https://www.RemoteApp.windowsazure.com 
*    https://*RemoteApp.windowsazure.com  
*    https://*.Core.Windows.NET  
*    Ruchu wychodzącego: TCP: port TCP 443,: 10101 10175 
*    Opcjonalne — UDP: 10201 10275  
 
## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp klienci muszą mieć dostęp do tych punktów końcowych i adresy URL: 

Klienci I oznacza komputery stacjonarne, urządzeń itp tej osoby nawiązywać połączenie z aplikacjami wdrożony w zbiorze Azure RemoteApp.

-  https://telemetry.RemoteApp.windowsazure.com  
-  https://*.RemoteApp.windowsazure.com (opcjonalne porty UDP są dla tego adresu) 
-  https://login.Windows.NET  
-  https://login.microsoftonline.com  
-  https://www.RemoteApp.windowsazure.com 
-  https://*.Core.Windows.NET  
-  Ruchu wychodzącego: TCP: 443  
-  Opcjonalnie - UDP: 3391 