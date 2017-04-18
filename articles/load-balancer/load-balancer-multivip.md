<properties
   pageTitle="Wiele służącą do usługi w chmurze"
   description="Omówienie multiVIP i jak ustawić wiele służącą dotyczące usługi w chmurze"
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

# <a name="configure-multiple-vips-for-a-cloud-service"></a>Konfigurowanie wielu służącą do usługi w chmurze

Przy użyciu adresu IP dostarczony przez Azure dostępne usług w chmurze Azure publicznie w Internecie. Ten publiczny adres IP jest nazywany VIP (wirtualnych adresów IP), ponieważ jest połączony z modułem równoważenia obciążenia Azure, a nie maszyny wirtualnej (maszyn wirtualnych) wystąpienia w usłudze w chmurze. Masz dostęp do każdego wystąpienia maszyn wirtualnych w ramach usługi w chmurze przy użyciu jednego VIP.

Istnieją jednak scenariusze, w których może być konieczne więcej niż jedna VIP jako wpisu wskaż samej usługi w chmurze. Na przykład usługa w chmurze może obsługiwać wiele witryn sieci Web, które wymagają łączności SSL przy użyciu domyślny port 443, jak każdej witryny jest obsługiwany dla innego klienta lub dzierżawa. W tym scenariuszu musisz różnych publiczny przeciwległych adres IP dla każdej witryny sieci Web. Poniższy diagram przedstawia typowe wielu dzierżawy hostingu potrzebuje wielu certyfikatów SSL w tym samym porcie publicznej.

![Scenariusz wielokrotne VIP SSL](./media/load-balancer-multivip/Figure1.png)

W powyższym przykładzie wszystkie adresy VIP używać tego samego portu publicznej (443) i ruch jest przekierowywana do jednej lub równoważenia obciążenia więcej maszyny wirtualne na porcie prywatne unikatowe dla wewnętrzny adres IP hostingu wszystkich witryn sieci Web usługi w chmurze.

>[AZURE.NOTE] Sytuacji wymagających wielu służącą obsługuje wiele detektory grupy dostępności (AlwaysOn SQL) na tym samym zestawie maszyn wirtualnych.

Służącą są dynamiczne domyślnie, co oznacza, że rzeczywisty adres IP przypisany do usługi w chmurze mogą się zmieniać w czasie. Aby zapobiec który, można zarezerwować VIP tej usługi. Aby dowiedzieć się więcej na temat służącą zastrzeżone, zobacz [Zastrzeżone publiczny adres IP](../virtual-network/virtual-networks-reserved-public-ip.md).

>[AZURE.NOTE] Zobacz [ceny adres IP](https://azure.microsoft.com/pricing/details/ip-addresses/) informacji na temat ceny służącą i zastrzeżone adresy IP.

Można sprawdzanie służącą używane przez usługi cloud za pomocą programu PowerShell oraz dodawanie i usuwanie służącą, skojarzyć VIP do punktu końcowego i konfigurowanie równoważenia na określonym VIP.

## <a name="limitations"></a>Ograniczenia

W tej chwili VIP wielokrotne funkcje są ograniczone do następujących scenariuszy:

- **Tylko IaaS**. Wielokrotne VIP można włączyć tylko dla usługami w chmurze, które zawierają maszyny wirtualne. Nie można użyć wielu VIP scenariusze PaaS z wystąpieniami roli.
- **Tylko programu PowerShell**. Wielokrotne VIP może zarządzać tylko przy użyciu programu PowerShell.

Ograniczenia te są tymczasowe i może się zmienić w dowolnym momencie. Upewnij się ponownie strony, aby zweryfikować przyszłe zmiany.


## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Jak dodać VIP do usługi w chmurze

Aby dodać VIP usługi, uruchom następujące polecenia programu PowerShell:

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

To polecenie wyświetla wynik podobny do poniższego przykładu:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>Jak usunąć VIP z usługi w chmurze

Aby usunąć VIP dodane do usługi w powyższym przykładzie, uruchom następujące polecenia programu PowerShell:

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] VIP można usuwać tylko, jeśli został skojarzonego z nim punktów końcowych.

## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Jak pobrać VIP informacji z usługi w chmurze

Aby pobrać służącą skojarzone z usługi w chmurze, uruchom następujący skrypt programu PowerShell:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Skrypt zostaną wyświetlone wyniki podobne do poniższego przykładu:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

W tym przykładzie usługa w chmurze ma 3 służącą:

- **Vip1** jest VIP domyślne, wiesz, że ponieważ wartość IsDnsProgrammedName jest ustawiona na PRAWDA.
- **Vip2** i **Vip3** nie są używane jako mają adresy IP. Tylko będzie można używać, jeśli powiążesz punktu końcowego do VIP.

>[AZURE.NOTE] Twoja subskrypcja będzie tylko obciążony dodatkowe służącą po skojarzone z punktem końcowym. Aby uzyskać więcej informacji dotyczących ceny zobacz [ceny adres IP](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Jak skojarzyć VIP do punktu końcowego

Aby skojarzyć VIP na do punktu końcowego usługi w chmurze, uruchom następujące polecenia programu PowerShell:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

Polecenie tworzy punktu końcowego połączone z VIP o nazwie *Vip2* w port *80*i łączy do maszyn wirtualnych o nazwie *myVM1* w usłudze w chmurze o nazwie *Moja_usługa* na porcie *8080*za pomocą *protokołu TCP* .

Aby sprawdzić konfigurację, uruchom następujące polecenia programu PowerShell:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Wynik będzie podobny do następującego przykładu:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Jak włączyć równoważenia na określonym VIP

Możesz skojarzyć VIP jednego z wielu maszyn wirtualnych do celów równoważenia obciążenia. Na przykład masz o nazwie *Moja_usługa*usługi w chmurze, a dwa maszyn wirtualnych o nazwie *myVM1* i *myVM2*. A usługa w chmurze ma wiele służącą, jeden z nich o nazwie *Vip2*. Jeśli chcesz upewnić się, że cały ruch do portu *81* na *Vip2* jest zbilansowane między *myVM1* i *myVM2* na porcie *8181*, uruchom następujący skrypt programu PowerShell:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

Można także zaktualizować usługi równoważenia obciążenia, aby użyć innego VIP. Ustaw VIP o nazwie Vip1 za pomocą równoważenia obciążenia zmienić na przykład po uruchomieniu polecenia programu PowerShell poniżej:

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## <a name="next-steps"></a>Następne kroki

[Dziennik analizy równoważenia obciążenia Azure](load-balancer-monitor-log.md)

[Omówienie równoważenia obciążenia przeciwległych Internet](load-balancer-internet-overview.md)

[Rozpoczynanie pracy w Internecie przeciwległych równoważenia obciążenia](load-balancer-get-started-internet-arm-ps.md)

[Omówienie wirtualnych sieci](../virtual-network/virtual-networks-overview.md)

[Interfejsy API pozostałych zastrzeżony adres IP](https://msdn.microsoft.com/library/azure/dn722420.aspx)