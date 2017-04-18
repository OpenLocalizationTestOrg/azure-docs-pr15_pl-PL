<properties
   pageTitle="Konfigurowanie usługi równoważenia obciążenia dla SQL zawsze na | Microsoft Azure"
   description="Konfigurowanie usługi równoważenia obciążenia do pracy z SQL zawsze na i jak je wykorzystać programu powershell, aby utworzyć równoważenia obciążenia dla implementacji SQL"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-load-balancer-for-sql-always-on"></a>Konfigurowanie usługi równoważenia obciążenia dla SQL zawsze na

Grupy dostępności (AlwaysOn) serwera SQL może zostać uruchomiony z ILB. Grupy dostępności to rozwiązanie najważniejszych programu SQL Server dla wysoka odzyskiwania dostępności i awarii. Dostępność odbiornika grupy umożliwia klienta aplikacji bezproblemowo nawiązywanie połączenia z podstawowego replice, niezależnie od liczby replik w konfiguracji.

Nazwa odbiornika (DNS) są mapowane na adres IP równoważenia obciążenia i równoważenia obciążenia i Azure kieruje ruch przychodzący do serwera podstawowego w zestawie.

Za pomocą ILB pomocy technicznej dla programu SQL Server (AlwaysOn) (odbiornik) punktów końcowych. Teraz kontrolę nad dostępności odbiornika i wybrać adres IP równoważenia obciążenia określonej podsieci w sieci wirtualnych (VNet).

Przy użyciu ILB na odbiornika punkt końcowy serwera SQL (np. serwer = tcp:ListenerName, 1433; bazy danych = DatabaseName) są dostępne tylko dla:

- Usługi i maszyny wirtualne w tej samej sieci wirtualnej
- Usługi i maszyny wirtualne z połączonych sieci lokalnej
- Usługi i maszyny wirtualne z VNets połączone

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Rysunek 1 - SQL (AlwaysOn) skonfigurowano usługi równoważenia obciążenia z Internetu

## <a name="add-internal-load-balancer-to-the-service"></a>Dodawanie wewnętrznych równoważenia obciążenia usługi

1. W poniższym przykładzie możemy skonfiguruje wirtualnej sieci, zawierającą podsieci o nazwie "Podsieci-1":

        Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

2. Dodawanie punktów końcowych równoważenia obciążenia dla ILB na poszczególnych maszyn wirtualnych

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
        DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    W powyższym przykładzie, jest nazywane "sqlsvc1" i "sqlsvc2" uruchomiony 2 maszyn wirtualnych w chmurze usługi "Sqlsvc". Po utworzeniu ILB z przełącznikiem "DirectServerReturn", należy dodać punkty końcowe równoważenia obciążenia ILB umożliwia kod SQL, aby skonfigurować detektory grupy dostępności.

Aby uzyskać więcej informacji na temat SQL (AlwaysOn) zobacz [Konfigurowanie usługi równoważenia obciążenia wewnętrznych dla grupy dostępności (AlwaysOn) platformy Azure](../virtual-machines/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Zobacz też

[Przed rozpoczęciem konfigurowania dostępnych równoważenia obciążenia przez Internet](load-balancer-get-started-internet-arm-ps.md)

[Przed rozpoczęciem konfigurowania równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)
