<properties
   pageTitle="Modyfikowanie prefiksy adresów IP bramy sieci lokalnej i IP bramy | Microsoft Azure"
   description="W tym artykule przeprowadzi Cię przez zmianę prefiksy adresów IP dla swojej sieci lokalnej bramy"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# <a name="modify-local-network-gateway-settings-using-powershell"></a>Modyfikowanie ustawień sieci lokalnej za pomocą programu PowerShell

Czasami zmienić ustawienia Centrum sieci lokalnej AddressPrefix lub GatewayIPAddress. Poniższe instrukcje pomoże Ci modyfikowanie ustawień bramy sieci lokalnej. Można także modyfikować te ustawienia w portalu Azure.

## <a name="before-you-begin"></a>Przed rozpoczęciem
    
Musisz zainstalować najnowszą wersję programu PowerShell Menedżera zasobów Azure poleceń cmdlet. Aby uzyskać więcej informacji o instalowaniu poleceń cmdlet programu PowerShell, zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .

## <a name="to-modify-ip-address-prefixes"></a>Aby zmodyfikować prefiksy adresów IP

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>Aby zmienić adres IP bramy

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Następne kroki

Można sprawdzić połączenie bramy. Zobacz [Weryfikowanie połączenie bramy](vpn-gateway-verify-connection-resource-manager.md).

