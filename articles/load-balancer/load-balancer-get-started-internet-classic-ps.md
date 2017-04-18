<properties
   pageTitle="Wprowadzenie do tworzenia internetowej przeciwległych równoważenia obciążenia w trybie klasycznym przy użyciu programu PowerShell | Microsoft Azure"
   description="Dowiedz się, jak utworzyć internetową przeciwległych równoważenia obciążenia w trybie klasycznym przy użyciu programu PowerShell"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Wprowadzenie do tworzenia dostępnych równoważenia obciążenia (klasyczny) w programie PowerShell przez Internet

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model klasyczny wdrożenia. Można także [dowiedzieć się, jak utworzyć internetową przeciwległych równoważenia obciążenia za pomocą Menedżera zasobów Azure](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## <a name="set-up-load-balancer-using-powershell"></a>Konfigurowanie usługi równoważenia obciążenia przy użyciu programu PowerShell

Aby skonfigurować usługę równoważenia obciążenia przy użyciu programu powershell, wykonaj poniższe czynności:

1. Jeśli po raz pierwszy używasz Azure programu PowerShell, zobacz, [jak instalowanie i konfigurowanie programu PowerShell Azure](../../articles/powershell-install-configure.md) i postępuj zgodnie z instrukcjami maksymalnie end, aby zalogować się do Azure i wybierz subskrypcję.


2. Po utworzeniu maszyny wirtualnej, możesz dodać równoważenia obciążenia maszyn wirtualnych w tym samym usługi w chmurze przy użyciu poleceń cmdlet programu PowerShell.

W poniższym przykładzie spowoduje dodanie zestawu równoważenia obciążenia o nazwie "webfarm" do chmury usługi "mytestcloud" (lub myctestcloud.cloudapp.net), dodawanie punktów końcowych do równoważenia obciążenia do maszyn wirtualnych o nazwie "sieci Web 1" i "sieci Web 2". Usługi równoważenia obciążenia odbiera ruch sieciowy na porcie 80 i Ładowanie sald między maszyn wirtualnych zdefiniowanych przez lokalny punkt końcowy (w tym przypadku port 80) za pomocą protokołu TCP.


### <a name="step-1"></a>Krok 1
Tworzenie punktu końcowego równoważenia obciążenia dla pierwszej maszyn wirtualnych "sieci Web 1"

    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### <a name="step-2"></a>Krok 2

Tworzenie innego punkt końcowy dla drugiego maszyn wirtualnych "sieci Web 2" przy użyciu tej samej nazwy zestawu równoważenia obciążenia

    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Usuwanie maszyny wirtualnej z równoważenia obciążenia

Usuń AzureEndpoint umożliwia usuwanie punktu końcowego maszyn wirtualnych usługi równoważenia obciążenia

    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## <a name="next-steps"></a>Następne kroki

Możesz także [rozpocząć tworzenie równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-classic-ps.md) i konfigurowanie jakiego rodzaju [Tryb rozkładu](load-balancer-distribution-mode.md) zachowanie ruch sieci usługi równoważenia obciążenia especific.

Jeśli aplikacja wymaga do utrzymywania połączenia przy życiu dla serwerów za równoważenia obciążenia, można zrozumieć więcej o [Ustawienia limitu czasu bezczynności TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md). Może pomóc w Aby uzyskać informacje o zachowanie bezczynne połączenia, korzystając z usługi równoważenia obciążenia Azure.

