<properties 
   pageTitle="Rozwiązywanie problemów z drogi - programu PowerShell | Microsoft Azure"
   description="Dowiedz się, jak rozwiązywać problemy z trasy w modelu wdrożenia Menedżera zasobów Azure przy użyciu programu PowerShell Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-routes-using-azure-powershell"></a>Rozwiązywanie problemów z marszrut przy użyciu programu PowerShell Azure

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
- [Programu PowerShell](virtual-network-routes-troubleshoot-powershell.md)

Jeśli występują problemy z łącznością sieciową do lub z usługi Azure maszyn wirtualnych (maszyn wirtualnych), trasy mogą mieć wpływ na usługi przepływu ruchu maszyn wirtualnych. W tym artykule omówiono funkcje diagnostyki przekierowuje pomagające dalszej.

Trasa tabele są skojarzone z podsieci i obowiązują na wszystkich interfejsach sieciowych (NIC) w tej podsieci. Następujące typy trasy mogą być stosowane do każdego interfejsu sieciowego:

- **System przekierowuje:** Domyślnie każdy podsieć utworzone w wirtualnej sieci Azure (VNet) ma system rozsyłania tabel, które umożliwiają ruchu VNet lokalnego, ruchu lokalnego za pośrednictwem sieci VPN bram i ruchu internetowego. Istnieją również przekierowuje systemu dla peered VNets.
- **Przekierowuje BGP:** Propagowane do interfejsów sieciowych za pośrednictwem połączenia VPN witryny do witryny lub ExpressRoute. Dowiedz się więcej o BGP routingu, czytając artykuły [Omówienie ExpressRoute](../expressroute/expressroute-introduction.md) i [BGP z bramy sieci VPN](../vpn-gateway/vpn-gateway-bgp-overview.md) .
- **Przekierowuje zdefiniowane przez użytkownika (UDR):** Jeśli korzystasz z urządzenia wirtualnej sieci lub są wymuszone tunneling ruch do sieci lokalnej za pośrednictwem sieci VPN witryny do witryny, może być zdefiniowane przez użytkownika ścieżki (UDRs) skojarzone z tabeli routingu podsieci. Jeśli nie znasz UDRs, przeczytaj artykuł [przekierowuje zdefiniowane przez użytkownika](virtual-networks-udr-overview.md#user-defined-routes) .

Z różnych sposobów, które można stosować do karty sieciowej może być trudne do określenia agregacji trasy są skuteczne. Pomagające łączność sieciowa maszyn wirtualnych, można przeglądać wszystkie skutecznych trasy dla karty sieciowej w modelu wdrożenia Azure Menedżera zasobów.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Rozwiązywanie problemów z ruch maszyn wirtualnych za pomocą skutecznych trasy

W tym artykule używa następującym scenariuszu jako przykład, aby przedstawić jak rozwiązywać skutecznych trasy karty sieciowej:

Maszyny (*VM1*) połączony z VNet (*VNet1*, prefiks: 10.9.0.0/16) nie może połączyć się VM(VM3) w nowo peered VNet (*VNet3*, 10.10.0.0/16 prefiks). Nie UDRs lub BGP kieruje zastosowane do VM1 NIC1 sieciowej podłączony do maszyn wirtualnych, są stosowane tylko system trasy.

W tym artykule wyjaśniono, jak ustalić przyczynę błąd połączenia przy użyciu przekierowuje wykorzystania możliwości w modelu wdrażania zarządzania zasobami Azure.
Gdy w przykładzie użyto tylko system trasy, te same kroki może służyć do określenia błędy połączeń przychodzących i wychodzących na dowolnym typie rozsyłania.

>[AZURE.NOTE] Jeśli do maszyn wirtualnych zawiera więcej niż jeden NIC dołączone, sprawdź skutecznych trasy dla każdej nic diagnozowanie problemów z łącznością sieci do i z maszyny.

### <a name="view-effective-routes-for-a-virtual-machine"></a>Wyświetlanie marszrut skutecznej dla maszyny wirtualnej

Aby wyświetlić trasy agregacji, które są stosowane do maszyny, wykonaj następujące czynności:

### <a name="view-effective-routes-for-a-network-interface"></a>Wyświetlanie marszrut skutecznych dla karty sieciowej

Aby wyświetlić trasy agregacji, które są stosowane do karty sieciowej, wykonaj następujące czynności:

1.  Rozpoczynanie sesji programu PowerShell Azure i logowania Azure. Jeśli nie znasz programu PowerShell Azure, przeczytaj artykuł [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .

2.  Poniższe polecenie zwraca wszystkie trasy zastosowane do karty sieciowej o nazwie *VM1 NIC1* w grupie zasobów *RG1*.

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Jeśli nie znasz nazwę karty sieciowej, wpisz następujące polecenie, aby pobrać nazwy wszystkich interfejsów sieci w group.* zasobów

        Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name

    Następujący wynik będzie podobny do wyników dla każdej trasy zastosowane do podsieć, z którą jest połączony NIC:

        Name :
        State : Active
        AddressPrefix : {10.9.0.0/16}
        NextHopType : VNetLocal
        NextHopIpAddress : {}

        Name :
        State : Active
        AddressPrefix : {0.0.0.0/16}
        NextHopType : Internet
        NextHopIpAddress : {}

    Zwróć uwagę następujące w wyniku kwerendy:
    - **Nazwa**: Nazwa skutecznych trasę może być puste, chyba że jawnie określony, marszrut zdefiniowanych przez użytkownika. 
    - **Stan**: stan skutecznych trasę. Możliwe wartości są "Aktywna" lub "Nieprawidłowe"
    - **AddressPrefixes**: Określa prefiksu adresu trasa skutecznej w notacji CIDR. 
    - **nextHopType**: wskazuje następnego przeskoku dla danej trasy. Możliwe wartości to *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*lub *wartość Null*. Wartości *Null* dla **nextHopType** w UDR może oznaczać trasa nieprawidłowa. Na przykład w przypadku **nextHopType** *VirtualAppliance* i wirtualnych sieci urządzenia maszyn wirtualnych nie jest w stanie obsługi administracyjnej pracy. Jeśli **nextHopType** jest *VPNGateway* i jest brak obsługi administracyjnej i pracy w danym VNet bramy, trasę może stać się nieprawidłowe.
    - **NextHopIpAddress**: Określa adres IP następnego przeskoku skutecznych trasę.
    
    Poniższe polecenie zwraca trasy ułatwia przeglądanie tabeli:

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table

    Następujący wynik jest niektóre dane wyjściowe odebranych w scenariuszu opisane wcześniej:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {0.0.0.0/0} Internet {}
    

3. Istnieje żadna trasa znajdującą się w VNet *WestUS VNet3* (prefiks 10.10.0.0/16)* * z *WestUS VNet1* (prefiks 10.9.0.0/16) w wyniku kwerendy w poprzednim kroku. Jak pokazano na poniższej ilustracji, łącze peering VNet z VNet *WestUS VNet3* jest w stanie *Rozłączono* .
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Łącze dwukierunkowego zaglądanie jest przerwane, której wyjaśniono dlaczego VM1 nie może połączyć się VM3 w VNet *WestUS VNet3* . Ponownie skonfigurować dwukierunkowego łącza peering VNet *WestUS VNet1* i VNets *WestUS VNet3* . Dane wyjściowe zwracane po poprawnie łącze peering VNet następująco:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
        
    Po ustaleniu ten problem, możesz dodać, usunąć, lub zmienić trasy i kierowanie tabel. Wpisz następujące polecenie, aby wyświetlić listę poleceń używanych w tym celu:

        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Zagadnienia dotyczące

Kilka rzeczy, które należy pamiętać podczas przeglądania listy przekierowuje zwrócone:

- Routing jest oparty na Najdłuższy prefiks dopasowania (LPM) między UDRs, BGP i system. Jeśli istnieje więcej niż jedna trasa z tym samym dopasowanie LPM, trasę zaznaczona jest oparte na jego pochodzenie w następującej kolejności:
    - Trasa zdefiniowane przez użytkownika
    - Trasa BGP
    - Trasa system (ustawienie domyślne)

    Trasy skutecznych widać tylko efektywnych ścieżek, które są oparte na wszystkie trasy dostępnego dopasowanie LPM. Pokazując, jak trasy są faktyczną obliczane dla danego NIC, ułatwia znacznie rozwiązywać problemy z określonej trasy, które mogą mieć wpływ na łączność z Twojej maszyn wirtualnych.

- Jeśli masz UDRs i wysyłają ruch do sieci urządzenia wirtualnych (NVA) z *VirtualAppliance* jako **nextHopType**, zapewnienia, że przekazywanie adresów IP jest włączone NVA odbieraniem danych lub pakiety są usuwane. 
- Jeśli jest włączone tunelowanie wymuszony, cały ruch wychodzący Internet będą kierowane do lokalnego. RDP-SSH z Internetu do swojego maszyn wirtualnych może nie działać z tych ustawień, w zależności od tego, jak w lokalnym obsługuje ruch. 
  Wymuszone tunneling można włączyć:
    - Jeśli przy użyciu sieci VPN witryny do witryny, ustawiając trasę zdefiniowane przez użytkownika (UDR) z nextHopType jako brama VPN
    - Jeśli trasa domyślna jest ogłaszane przez BGP
- Dla VNet ruchu zaglądanie do działa prawidłowo trasę system z **nextHopType** *VNetPeering* , musi istnieć zakresu prefiks peered VNet. Jeśli taka trasa nie istnieje, a łącze peering VNet wygląda OK:
    - Poczekaj chwilę i spróbuj ponownie, jeśli jest nowo utworzonego łącza peering. Od czasu do czasu trwa dłużej propagowanie trasy do wszystkich interfejsów sieciowych podsieci.
    - Zasady grupy zabezpieczeń sieci (NSG) mogą mieć wpływ na ruch przepływa. Aby uzyskać więcej informacji zobacz artykuł [Rozwiązywanie problemów z grupami zabezpieczeń sieci](virtual-network-nsg-troubleshoot-powershell.md) .
