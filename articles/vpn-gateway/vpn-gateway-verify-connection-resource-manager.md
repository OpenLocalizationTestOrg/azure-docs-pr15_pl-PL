<properties
   pageTitle="Sprawdź połączenie bramy | Microsoft Azure"
   description="W tym artykule pokazano, jak sprawdzić połączenie bramy w modelu wdrożenia Menedżera zasobów"
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
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="verify-a-gateway-connection"></a>Sprawdź połączenie bramy

Na kilka sposobów, można sprawdzić połączenie bramy. W tym artykule zostanie wyświetlona jak sprawdzić stan połączenia bramy Menedżera zasobów za pomocą portalu Azure i przy użyciu programu PowerShell.


## <a name="verify-using-powershell"></a>Sprawdź przy użyciu programu PowerShell

Musisz zainstalować najnowszą wersję programu PowerShell Menedżera zasobów Azure poleceń cmdlet. Aby uzyskać informacje o instalowaniu poleceń cmdlet programu PowerShell zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md). Aby uzyskać więcej informacji na temat używania poleceń cmdlet Menedżera zasobów zobacz [Przy użyciu programu Windows PowerShell przy użyciu Menedżera zasobów](../powershell-azure-resource-manager.md).

### <a name="step-1-log-in-to-your-azure-account"></a>Krok 1: Zaloguj się do konta Azure

1. Otwórz konsoli programu PowerShell z pełnymi uprawnieniami i łączenia się z kontem.

        Login-AzureRmAccount

2. Sprawdzanie subskrypcji dla tego konta.

        Get-AzureRmSubscription 

3. Określ subskrypcję, do której chcesz użyć.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>Krok 2: Sprawdź połączenie


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="verify-using-the-azure-portal"></a>Sprawdź za pomocą portalu Azure

[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)] 


## <a name="next-steps"></a>Następne kroki

- Możesz dodać maszyn wirtualnych do sieci wirtualnej. Instrukcje, zobacz temat [Tworzenie maszyny wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

