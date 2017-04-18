<properties
   pageTitle="Typy węzłów tkaninie usługi i zestawów skali maszyn wirtualnych | Microsoft Azure"
   description="W tym artykule opisano, jak typy węzłów tkaninie usługi dotyczą zestawów skali maszyn wirtualnych i jak do zdalnej połączyć się z wystąpieniem Ustawianie skali maszyn wirtualnych lub węzła."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Relacja między typy węzłów tkaninie usługi i zestawy skali maszyn wirtualnych

Zestawy skali maszyn wirtualnych są obliczenia Azure zasób, który umożliwia wdrażanie i zarządzanie zbiorem maszyn wirtualnych jako zestaw. Każdego typu węzeł, która jest zdefiniowana w klastrze tkaninie usługi jest skonfigurowana jako osobne zestaw maszyn wirtualnych Skala. Każdego typu węzła można następnie skalować w górę lub w dół niezależnie od tego, mają różne zestawy otwarte porty i nie może mieć inną pojemność metryki.

Poniższy zrzut ekranu przedstawia klastrze występują dwa typy węzłów: serwera sieci Web i wewnętrznej bazy danych.  Dla każdego typu węzeł jest pięć węzły.

![Zrzut ekranu przedstawiający klastrze występują dwa typy węzłów][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>Mapowanie Ustawianie skali maszyn wirtualnych wystąpień węzły

Jak widać powyżej, ustawianie skali maszyn wirtualnych wystąpień Rozpocznij z wystąpienia 0, a następnie przejść w górę. Numerowanie jest widoczny w polu nazwy. Na przykład BackEnd_0 to wystąpienie 0 zestawu skali maszyn wirtualnych wewnętrznej bazy danych. Tego określonego maszyn wirtualnych skali zestawu ma pięciu wystąpień o nazwie BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 i BackEnd_4.

Po rozbudowy Ustawianie skali maszyn wirtualnych zostanie utworzone nowe wystąpienie. Nową nazwę wystąpienia maszyn wirtualnych Skala Ustaw będzie zazwyczaj nazwa zestawu skali maszyn wirtualnych + numer następnej. W naszym przykładzie jest BackEnd_5.


## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Mapowanie maszyn wirtualnych Skala Ustaw urządzenia do równoważenia obciążenia każdego węzła typu/m Skala Ustaw

Jeśli wdrożeniu klaster z portalu lub korzystają z Poniższy przykładowy szablon Menedżera zasobów możemy pod warunkiem, gdy zostanie wyświetlona lista wszystkich zasobów w obszarze Grupa zasobów zostanie wyświetlona równoważenia obciążenia dla każdego typu Ustawianie skali maszyn wirtualnych lub węzeł.

Nazwa będzie podobny do: **kg -&lt;Nazwa Typ węzła&gt;**. Na przykład kg sfcluster4doc-0, jak pokazano w tym zrzut ekranu:


![Zasoby][Resources]


## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Zdalne połączyć się z wystąpieniem Ustawianie skali maszyn wirtualnych lub węzła
Każdego typu węzeł, która jest zdefiniowana w klastrze jest skonfigurowana jako osobne zestaw maszyn wirtualnych Skala.  Oznacza to, że można skalować typy węzłów w górę lub w dół niezależne i może się z różnych wersji produktu maszyn wirtualnych. W odróżnieniu od jednego wystąpienia maszyny wirtualne Ustawianie skali maszyn wirtualnych wystąpień nie są wirtualny adres IP we własnym zakresie. Dlatego może być nieco trudne po szukasz adres IP i portu, który umożliwia zdalnym połączeniu do konkretnego wystąpienia.

Wykonaj poniższe kroki mogą wykonać, aby je także wykryć.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Krok 1: Dowiedz się, wirtualny adres IP typ węzła, a następnie reguły przychodzące translatora adresów Sieciowych RDP

Aby można było uzyskiwać, który, potrzebne do uzyskania wartości reguły przychodzące translatora adresów Sieciowych zdefiniowane w ramach definicji zasobów **Microsoft.Network/loadBalancers**.

W portalu przejdź do karta równoważenia obciążenia, a następnie **Ustawienia**.

![LBBlade][LBBlade]


W obszarze **Ustawienia**kliknij **Reguły przychodzące translatora adresów Sieciowych**. Ten teraz przedstawiono adres IP i portu, który umożliwia zdalnym nawiązywanie połączenia z pierwsze wystąpienie Ustawianie skali maszyn wirtualnych. Poniżej ekranu jest **104.42.106.156** i **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Krok 2: Sprawdzanie portu, który służy do zdalnego nawiązywanie połączenia z określonego wystąpienia Ustawianie skali maszyn wirtualnych-węzeł

Wcześniej w tym dokumencie można zawsze mówię o jak ustawić skali maszyn wirtualnych wystąpień mapowanie węzły. Użyjemy który ustalanie dokładnie portu.

Porty są przydzielane rosnąco wystąpienia Ustawianie skali maszyn wirtualnych. Dlatego w naszym przykładzie dla typu węzła FrontEnd portów dla każdego z pięciu wystąpień są następujące. Teraz musisz wykonać mapowanie samego wystąpienia Ustawianie skali maszyn wirtualnych.

|**Wystąpienia zestawu skali maszyn wirtualnych**|**Port**|
|-----------------------|--------------------------|
|FrontEnd_0|3389|
|FrontEnd_1|3390|
|FrontEnd_2|3391|
|FrontEnd_3|3392|
|FrontEnd_4|3393|
|FrontEnd_5|3394|


### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>Krok 3: Połączenie zdalne do konkretnego wystąpienia Ustawianie skali maszyn wirtualnych

Zrzut ekranu poniżej I Użyj Podłączanie pulpitu zdalnego nawiązywania połączenia z FrontEnd_1:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>Jak zmienić RDP zakres wartości

### <a name="before-cluster-deployment"></a>Przed wdrożeniem klaster

Podczas konfigurowania klaster przy użyciu szablonu Menedżera zasobów, można określić zakres w **inboundNatPools**.

Przejdź do definicji zasobów dla **Microsoft.Network/loadBalancers**. W obszarze, który możesz znaleźć opis **inboundNatPools**.  Zamienianie wartości *frontendPortRangeStart* i *frontendPortRangeEnd* .

![InboundNatPools][InboundNatPools]


### <a name="after-cluster-deployment"></a>Po wdrożeniu klaster
To jest nieco bardziej skomplikowane i może spowodować naliczenie maszyny wirtualne, wprowadzenie wykonywane. Konieczne będzie teraz ustawić nowych wartości przy użyciu programu PowerShell Azure. Upewnij się, czy na komputerze zainstalowano Azure PowerShell 1.0 lub nowsza. Jeśli jeszcze tego nie tego wcześniej, mogę zdecydowanie zgłaszać postępuj zgodnie z instrukcjami podanymi w [sposób instalowania i konfigurowania środowiska PowerShell Azure.](../powershell-install-configure.md)

Zaloguj się do swojego konta Azure. Jeśli tego polecenia programu PowerShell jakiegoś powodu nie powiedzie się, należy sprawdzić, czy masz Azure programu PowerShell poprawnie zainstalowany.

```
Login-AzureRmAccount
```

Uruchom poniższe czynności, aby uzyskać szczegółowe informacje na temat usługi równoważenia obciążenia i wyświetlić wartości dla opisu **inboundNatPools**:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Wartości, które chcesz teraz ustaw *frontendPortRangeEnd* i *frontendPortRangeStart* .

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Następne kroki

- [Omówienie funkcji "Wdrażanie z dowolnego miejsca" i porównanie z zarządzaniem Azure klastrów](service-fabric-deploy-anywhere.md)
- [Klaster zabezpieczeń](service-fabric-cluster-security.md)
- [Usługa tkaninie SDK oraz wprowadzenie](service-fabric-get-started.md)


<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
