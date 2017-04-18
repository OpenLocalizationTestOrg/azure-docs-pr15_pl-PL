<properties
   pageTitle="Tworzenie równoważenia obciążenia wewnętrznych, przy użyciu programu PowerShell w modelu Klasyczny wdrożenia | Microsoft Azure"
   description="Dowiedz się, jak utworzyć równoważenia obciążenia wewnętrznych, przy użyciu programu PowerShell w modelu Klasyczny wdrażania"
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
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-using-powershell"></a>Wprowadzenie do tworzenia wewnętrzny usługi równoważenia obciążenia (klasyczny) przy użyciu programu PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](load-balancer-get-started-ilb-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]


[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


## <a name="create-an-internal-load-balancer-set-for-virtual-machines"></a>Tworzenie równoważenia obciążenia wewnętrznych, ustaw dla maszyn wirtualnych

Aby utworzyć zestaw równoważenia obciążenia wewnętrznych i serwerów, które będą wysyłać ruch do niej, należy wykonać następujące czynności:

1. Utwórz wystąpienie wewnętrznych równoważenia obciążenia który będzie punkt końcowy ruch przychodzący być równoważenia na serwerach: Konfigurowanie usługi równoważenia obciążenia obciążenia.

1. Dodaj punkty końcowe odpowiadające maszyn wirtualnych, które będzie otrzymywał ruch przychodzący.

1. Konfigurowanie serwerów, które będą wysyłane na ruch można zastosować równoważenia chcesz wysyłać ruch ich wirtualny adres IP (VIP) wystąpienia równoważenia obciążenia wewnętrznych obciążenia.


### <a name="step-1-create-an-internal-load-balancing-instance"></a>Krok 1: Tworzenie wystąpienia wewnętrznych równoważenia obciążenia

W przypadku istniejącej usługi w chmurze lub wdrożony w obszarze regionalne, wirtualną sieć usługi w chmurze można utworzyć wystąpienia wewnętrznych równoważenia obciążenia z następujących poleceń programu Windows PowerShell:

    $svc="<Cloud Service Name>"
    $ilb="<Name of your ILB instance>"
    $subnet="<Name of the subnet within your virtual network>"
    $IP="<The IPv4 address to use on the subnet-optional>"

    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb –SubnetName $subnet –StaticVNetIPAddress $IP


Należy zauważyć, że tego użyj polecenia cmdlet programu Windows PowerShell [AzureEndpoint Dodaj](https://msdn.microsoft.com/library/dn495300.aspx) używa zestaw parametrów DefaultProbe. Aby uzyskać więcej informacji na zestawach dodatkowy parametr zobacz [Dodawanie AzureEndpoint](https://msdn.microsoft.com/library/dn495300.aspx).

### <a name="step-2-add-endpoints-to-the-internal-load-balancing-instance"></a>Krok 2: Dodawanie punktów końcowych do wewnętrznego równoważenia obciążenia wystąpienia

Oto przykład:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $ilb="ilbset"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -Lbset $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


### <a name="step-3-configure-your-servers-to-send-their-traffic-to-the-new-internal-load-balancing-endpoint"></a>Krok 3: Skonfigurować serwery chcesz wysyłać ruch ich do nowy punkt końcowy wewnętrznych równoważenia obciążenia

Musisz skonfigurować serwery ruch ma być równoważenia umożliwia nowy adres IP (VIP) wystąpienia równoważenia obciążenia wewnętrznych obciążenia. Jest to adres, na którym oczekuje wystąpienie wewnętrznych równoważenia obciążenia. W większości przypadków należy po prostu dodać lub zmodyfikować rekordu DNS dla VIP wystąpienia wewnętrznych równoważenia obciążenia.

Jeśli określono adres IP przy tworzeniu wystąpienia wewnętrznych równoważenia obciążenia, masz już VIP. W przeciwnym razie możesz zobaczyć VIP następujące polecenia:

    $svc="<Cloud Service Name>"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer



Aby użyć tych poleceń, wypełniać wartościami i Usuń < i >. Oto przykład:

    $svc="mytestcloud"
    Get-AzureService -ServiceName $svc | Get-AzureInternalLoadBalancer


Z widoku polecenie Get-AzureInternalLoadBalancer Zanotuj adres IP, a następnie wprowadź niezbędne zmiany na serwerach lub rekordów DNS w celu zapewnienia, że ruch przesłane do VIP.

>[AZURE.NOTE]Platformy Microsoft Azure używa statyczne, publicznie routingu adres IP protokołu IPv4 dla różnych scenariuszach administracyjnych. Adres IP jest 168.63.129.16. Ten adres IP nie powinno zostać zablokowane przez dowolnego zapory, ponieważ może powodować nieoczekiwane działanie.
>W odniesieniu do Azure wewnętrznych równoważenia obciążenia ten adres IP jest używany przez monitorowanie sondy z usługi równoważenia obciążenia do określenia stanu kondycji dla maszyn wirtualnych w zestawie równoważenia obciążenia. Jeśli grupa zabezpieczeń sieci jest używana, aby ograniczyć ruch do Azure maszyn wirtualnych w zestawie wewnętrznie równoważenia obciążenia lub zostanie zastosowany do wirtualnego podsieci, upewnij się, czy regułę zabezpieczeń sieci została dodana do zezwalanie na ruch z 168.63.129.16.


## <a name="example-of-internal-load-balancing"></a>Przykład równoważenia obciążenia wewnętrznych

Krok po kroku przez proces zakończenia zakończenia tworzenia zestawu równoważenia obciążenia dla dwóch przykład konfiguracji, zobacz następujące sekcje.

### <a name="an-internet-facing-multi-tier-application"></a>Internet przeciwległych wielu aplikacji

Chcesz podać usługi równoważenia obciążenia bazy danych dla zestawu serwerów sieci web w Internecie. Obie serwerów są obsługiwane w usłudze w chmurze Azure pojedynczy. Ruch serwer sieci Web na TCP port 1433 musi rozdzielona między dwoma maszyn wirtualnych w warstwie bazy danych. Rysunek 1 pokazuje konfigurację.

![Wewnętrzny zestaw równoważenia obciążenia dla poziomu bazy danych](./media/load-balancer-internal-getstarted/IC736321.png)


Proces konfiguracji składa się z następujących czynności:

- Istniejące usługi w chmurze hostingu maszyn wirtualnych nosi nazwę mytestcloud.

- Dwa serwery istniejącej bazy danych są nazywane Degresywna 1, DB2.

- Serwery sieci Web w warstwie sieci web łączenie się z serwerami bazy danych w warstwie bazy danych za pomocą prywatny adres IP. Innym rozwiązaniem jest za pomocą własnego DNS wirtualnej sieci i ręcznie zarejestrować rekordu A dla zestawu równoważenia obciążenia wewnętrzny.

Następujące polecenia Skonfiguruj nowe wystąpienie wewnętrznych równoważenia obciążenia o nazwie **ILBset** i dodawanie punktów końcowych do maszyn wirtualnych odpowiadające dwa serwery baz danych:

    $svc="mytestcloud"
    $ilb="ilbset"
    Add-AzureInternalLoadBalancer -ServiceName $svc -InternalLoadBalancerName $ilb
    $prot="tcp"
    $locport=1433
    $pubport=1433
    $epname="TCP-1433-1433"
    $lbsetname="lbset"
    $vmname="DB1"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM

    $epname="TCP-1433-1433-2"
    $vmname="DB2"
    Get-AzureVM –ServiceName $svc –Name $vmname | Add-AzureEndpoint -Name $epname -LbSetName $lbsetname -Protocol $prot -LocalPort $locport -PublicPort $pubport –DefaultProbe -InternalLoadBalancerName $ilb | Update-AzureVM


## <a name="remove-an-internal-load-balancing-configuration"></a>Usuwanie konfiguracji wewnętrznych równoważenia obciążenia

Aby usunąć maszyny wirtualnej jako punkt końcowy z wystąpieniem równoważenia obciążenia wewnętrznych, należy użyć następujących poleceń:

    $svc="<Cloud service name>"
    $vmname="<Name of the VM>"
    $epname="<Name of the endpoint>"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Aby użyć tych poleceń, wypełniać wartościami, usuwając < i >.

Oto przykład:

    $svc="mytestcloud"
    $vmname="DB1"
    $epname="TCP-1433-1433"
    Get-AzureVM -ServiceName $svc -Name $vmname | Remove-AzureEndpoint -Name $epname | Update-AzureVM

Aby usunąć wystąpienie równoważenia obciążenia wewnętrznych z usługi w chmurze, należy użyć następujących poleceń:

    $svc="<Cloud service name>"
    Remove-AzureInternalLoadBalancer -ServiceName $svc

Aby użyć tych poleceń, wpisz odpowiednią wartość i Usuń < i >.

Oto przykład:

    $svc="mytestcloud"
    Remove-AzureInternalLoadBalancer -ServiceName $svc



## <a name="additional-information-about-internal-load-balancer-cmdlets"></a>Dodatkowe informacje na temat polecenia cmdlet usługi równoważenia obciążenia wewnętrznych


Aby uzyskać dodatkowe informacje na temat polecenia cmdlet wewnętrznych równoważenia obciążenia, uruchom następujące polecenia w wierszu programu Windows PowerShell:

- Get-help nowe AzureInternalLoadBalancerConfig-pełny

- Get-help Dodawanie AzureInternalLoadBalancer-pełny

- Uzyskiwanie pomocy Get-AzureInternalLoadbalancer — pełny

- Get-help AzureInternalLoadBalancer Usuń-pełny

## <a name="next-steps"></a>Następne kroki

[Konfigurowanie trybu rozkładu równoważenia obciążenia przy użyciu źródła IP koligacji](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)