<properties 
   pageTitle="Temat bram wirtualnej sieci ExpressRoute | Microsoft Azure"
   description="Informacje o bram wirtualną sieć dla ExpressRoute."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>Informacje o bram wirtualną sieć dla ExpressRoute


Brama wirtualnej sieci jest używany do wysyłania ruchu sieciowego między Azure wirtualnych sieci i lokalizacji lokalnego. Po skonfigurowaniu połączenia ExpressRoute, należy utworzyć i skonfigurować bramę wirtualną sieć i połączenia wirtualnej sieci bramy.

Podczas tworzenia bramy wirtualną sieć Określ ustawienia kilku. Jedno z ustawień wymagane Określa, czy bramy będą używane do ExpressRoute lub VPN witryny do witryny. W modelu wdrożenia Menedżera zasobów jest ustawienie "-GatewayType".

Gdy ruch sieciowy jest wysyłana na dedykowane połączenie prywatne, użyj typu bramy "ExpressRoute". Jest to także nazywane bramy ExpressRoute. Gdy ruch sieciowy są przesyłane zaszyfrowane przez publicznego Internetu, użyj typu bramy "Vpn". To jest określane jako brama VPN. Witryny do witryny, punktu do witryny i VNet do VNet połączenia wszystkich za pomocą bramy sieci VPN. 

Każda wirtualna sieć może zawierać tylko jeden Brama wirtualnej sieci według typu bramy. Na przykład możesz mieć jeden wirtualną sieć bramy, która używa - GatewayType sieci Vpn oraz takie, które używa - GatewayType ExpressRoute. W tym artykule omówiono Brama wirtualnej sieci ExpressRoute.

## <a name="gwsku"></a>SKU bramy

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Jeśli chcesz uaktualnić uzyskanie bardziej zaawansowanych bramy SKU w większości przypadków służy polecenia cmdlet programu PowerShell "Zmiany rozmiaru AzureRmVirtualNetworkGateway". To będą działać w przypadku uaktualnienia standardowym i o odwróconej wersji produktu. Aby uaktualnić do wersji UltraPerformance, będzie konieczne odtworzenie bramy.

###  <a name="aggthroughput"></a>Szacowane Agreguj przepustowość przez bramę SKU


W poniższej tabeli przedstawiono typy bramy i Szacowana przepustowość agregującą. Ta tabela odnosi się do Menedżera zasobów i modeli wdrażania klasyczny.

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="resources"></a>Polecenia cmdlet programu PowerShell i interfejsów API REST

Dodatkowe zasoby techniczne i wymagania dotyczące składni podczas korzystania z pozostałych interfejsy API i poleceń cmdlet programu PowerShell dla konfiguracji bramy wirtualną sieć zobacz następujące strony:

|**Klasyczny** | **Menedżer zasobów**|
|-----|----|
|[Programu PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[Programu PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[INTERFEJSU API USŁUGI REST](https://msdn.microsoft.com/library/jj154113.aspx)|[INTERFEJSU API USŁUGI REST](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Następne kroki

Zobacz [Omówienie ExpressRoute](expressroute-introduction.md) , aby uzyskać więcej informacji o konfiguracjach dostępne połączenie. 







 
