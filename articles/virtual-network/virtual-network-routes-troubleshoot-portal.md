<properties 
   pageTitle="Rozwiązywanie problemów z drogi - portalu | Microsoft Azure"
   description="Dowiedz się, jak rozwiązywać problemy z trasy w modelu wdrożenia Menedżera zasobów Azure za pomocą Azure Portal."
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

# <a name="troubleshoot-routes-using-the-azure-portal"></a>Rozwiązywanie problemów z marszrut przy użyciu Azure Portal

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
- [Programu PowerShell](virtual-network-routes-troubleshoot-powershell.md)

Jeśli występują problemy z łącznością sieciową do lub z usługi Azure maszyn wirtualnych (maszyn wirtualnych), trasy mogą mieć wpływ na usługi przepływu ruchu maszyn wirtualnych. W tym artykule omówiono funkcje diagnostyki przekierowuje pomagające dodatkowo.

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

### <a name="view-effective-routes-for-a-virtual-machine"></a>Wyświetlanie marszrut skutecznych dla maszyny wirtualnej

Aby wyświetlić trasy agregacji, które są stosowane do maszyny, wykonaj następujące czynności:

1. Zaloguj się do portalu Azure pod adresem https://portal.azure.com.
2. Kliknij pozycję **więcej usług**, a następnie kliknij **maszyn wirtualnych** na liście.
3. Zaznacz maszyn wirtualnych rozwiązywać problemy z wyświetlonej listy, a zostanie wyświetlona karta maszyn wirtualnych przy użyciu opcji.
4. Kliknij pozycję **diagnozowanie & Rozwiązywanie problemów** , a następnie wybierz typowych problemów. W tym przykładzie **nie mogę się połączyć z moim maszyn wirtualnych systemu Windows** jest zaznaczone. 

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)

5. Czynności są wyświetlane w obszarze problem, jak pokazano na poniższej ilustracji: 

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Kliknij *skutecznych przekierowuje* na liście zalecanych kroków.

6. Karta **skutecznych przekierowuje** pojawia się, jak pokazano na poniższej ilustracji:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Jeśli do maszyn wirtualnych zawiera tylko jedną NIC, jest zaznaczona domyślnie. Jeśli masz więcej niż jeden NIC, wybierz pozycję NIC, dla którego chcesz wyświetlić skutecznych trasy.

    >[AZURE.NOTE] Jeśli maszyn wirtualnych, skojarzone z NIC nie jest w stanie uruchomienia, skutecznych przekierowuje nie będą wyświetlane. Tylko pierwszych 200 skutecznych trasy są wyświetlane w portalu. Aby uzyskać pełną listę kliknij przycisk **Pobierz**. Można dodatkowo filtrować wyniki z pliku CSV pobrany.

    Zwróć uwagę następujące w wyniku kwerendy:
    - **Źródła**: wskazuje typ trasy. Trasy system są wyświetlane jako *domyślny*, UDRs są wyświetlane jako *użytkownika* i bramy (statyczną lub BGP) są wyświetlane jako *VPNGateway*.
    - **Stan**: stan skutecznych trasę. Możliwe wartości są *aktywne* lub *nieprawidłowe*.
    - **AddressPrefixes**: Określa prefiksu adresu skutecznych trasę w notacji CIDR. 
    - **nextHopType**: wskazuje następnego przeskoku dla danej trasy. Możliwe wartości to *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*lub *wartość Null*. Wartości *Null* dla **nextHopType** w UDR może oznaczać trasa nieprawidłowa. Na przykład w przypadku **nextHopType** *VirtualAppliance* i wirtualnych sieci urządzenia maszyn wirtualnych nie jest w stanie obsługi administracyjnej pracy. Jeśli **nextHopType** jest *VPNGateway* i jest brak obsługi administracyjnej i pracy w danym VNet bramy, trasę może stać się nieprawidłowe.
    
7. Istnieje trasy do VNet *WestUS VNET3* (prefiks 10.10.0.0/16) na liście z *WestUS VNet1* (prefiks 10.9.0.0/16) na obrazie w poprzednim kroku. Na poniższej ilustracji peering łącze jest w stanie *Rozłączono* :
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    Łącze dwukierunkowego zaglądanie jest przerwane, której wyjaśniono dlaczego VM1 nie może połączyć się VM3 w VNet *WestUS VNet3* .

8. Na poniższej ilustracji przedstawiono trasy po ustaleniu łącze peering dwukierunkowym:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

Więcej scenariuszy rozwiązywania problemów wymuszone tunneling i oceny rozsyłania należy zapoznać się z sekcją [Zagadnienia dotyczące](virtual-network-routes-troubleshoot-portal.md#Considerations) tego artykułu.

### <a name="view-effective-routes-for-a-network-interface"></a>Wyświetlanie marszrut skutecznych dla karty sieciowej

Jeśli dla konkretnego interfejsu sieciowego (NIC) czy dotyczy to natężenia ruchu sieciowego, można bezpośrednio Wyświetl pełną listę przekierowuje skutecznych na Sieciowej. Aby wyświetlić trasy agregacji, które są stosowane do NIC, wykonaj następujące czynności:

1. Zaloguj się do portalu Azure pod adresem https://portal.azure.com.
2. Kliknij pozycję **więcej usług**, a następnie kliknij **interfejsów sieciowych**
3. Wyszukaj liście nazwę karty sieciowej lub wybierz ją z wyświetlonej listy. W tym przykładzie **VM1 NIC1** jest zaznaczone.
4. Wybierz **skutecznych przekierowuje** karta **sieciowa** , jak pokazano na poniższej ilustracji:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    Domyślnie **zakres** sieciowej zaznaczone.

    ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)


### <a name="view-effective-routes-for-a-route-table"></a>Wyświetlanie marszrut skutecznych dla tabeli routingu

Podczas modyfikowania przekierowuje zdefiniowane przez użytkownika (UDRs) w tabeli rozsyłania, może chcesz przejrzeć wpływu przekierowuje dodawanych w szczególności maszyn wirtualnych. Tabela rozsyłania może być skojarzone z dowolną liczbę podsieci. Teraz można wyświetlać wszystkie skutecznych trasy sieciowe stosowane tabeli danej trasy do, bez konieczności przełączania kontekstu z danej trasy karta tabeli.

W tym przykładzie UDR (*UDRoute*) jest określony w tabeli routingu (*UDRouteTable*). Tą drogą wysyła cały ruch internetowy z *podsieć1* w VNet *WestUS VNet1* za pośrednictwem sieci urządzenia wirtualnych (NVA) w *podsieć2* samej VNet. Na poniższej ilustracji pokazano trasę:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Aby wyświetlić agregacji trasy dla tabeli routingu, wykonaj następujące czynności:

1. Zaloguj się do portalu Azure pod adresem https://portal.azure.com.
2. Kliknij pozycję **więcej usług**, a następnie kliknij **tabel routingu**
3. Wyszukiwanie listy do tabeli trasę, którą chcesz wyświetlić agregacji trasy dla i zaznacz go. W tym przykładzie **UDRouteTable** jest zaznaczone. Karta tabeli zaznaczoną trasę pojawia się, jak pokazano na poniższej ilustracji:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)

4. Wybierz **Skutecznych przekierowuje** karta **tabeli routingu** . **Zakres** jest ustawiony na wybranej tabeli rozsyłania.
5. Tabelę routingu można stosować do wielu podsieci. Wybierz pozycję **podsieci** , należy przejrzeć z listy. W tym przykładzie **podsieć1** jest zaznaczone.
6. Wybierz **sieciowej**. Wszystkie nic połączony z zaznaczonej podsieci są widoczne. W tym przykładzie **VM1 NIC1** jest zaznaczone.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

    >[AZURE.NOTE] Jeśli NIC nie jest skojarzone z uruchomionego maszyn wirtualnych, nie efektywnych przekierowuje zostaną wyświetlone.

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
    - Zasady grupy zabezpieczeń sieci (NSG) mogą mieć wpływ na ruch przepływa. Aby uzyskać więcej informacji zobacz artykuł [Rozwiązywanie problemów z grupami zabezpieczeń sieci](virtual-network-nsg-troubleshoot-portal.md) .
