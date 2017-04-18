<properties 
   pageTitle="Rozwiązywanie problemów z grupami zabezpieczeń sieci — Portal | Microsoft Azure"
   description="Dowiedz się, jak rozwiązywać problemy z grupami zabezpieczeń sieci w modelu wdrożenia Menedżera zasobów Azure za pomocą Azure Portal."
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

# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Rozwiązywanie problemów z grupami zabezpieczeń sieci przy użyciu Azure Portal

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
- [Programu PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Jeśli skonfigurowano grup zabezpieczeń sieci (NSGs) na komputerze wirtualnych (maszyn wirtualnych) i występują problemy z łącznością maszyn wirtualnych, w tym artykule omówiono możliwości diagnostyki NSGs w celu dalszego.

NSGs umożliwiają kontrolowanie typów ruchu tego przepływu i usługi wirtualnych maszyn. NSGs mogą być stosowane do podsieci w wirtualnej sieci Azure (VNet) i/lub interfejsów sieciowych (NIC). Skuteczne reguły stosowane do NIC są zagregowane reguły, które znajdują się w NSGs zastosowane do NIC i podsieć, z którą jest połączony. Reguły w tych NSGs może czasami powodują konflikt i wpływają na łączność sieciowa maszyn wirtualnych.  

Można wyświetlić wszystkie reguły skutecznych zabezpieczeń z usługi NSGs stosowana w swojej maszyn wirtualnych nic. W tym artykule pokazano, jak rozwiązywać problemy z łącznością maszyn wirtualnych za pomocą tych reguł w modelu wdrożenia Azure Menedżera zasobów. Jeśli nie znasz VNet i NSG pojęcia, przeczytaj artykuły omówienie [wirtualnych sieci](virtual-networks-overview.md) i [grupy zabezpieczeń sieci](virtual-networks-nsg.md) .

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Rozwiązywanie problemów z ruch maszyn wirtualnych za pomocą skuteczne reguły zabezpieczeń

Scenariusz, w którym występuje jest przykładem typowych problem z połączeniem:

Maszyny o nazwie *VM1* jest częścią podsieć o nazwie *podsieć1* w VNet o nazwie *WestUS VNet1*. Próba nawiązania połączenia maszyn wirtualnych, przy użyciu RDP przez TCP port 3389 zakończy się niepowodzeniem. NSGs są stosowane na zarówno NIC *VM1 NIC1* , jak i podsieć *podsieć1*. Ruch na TCP port 3389 jest dozwolony w NSG skojarzone z sieciowej *VM1 NIC1*, jednak TCP ping do VM1 na porcie 3389 kończy się niepowodzeniem.

Gdy w tym przykładzie użyto TCP port 3389, następujące czynności może służyć do określenia błędy połączeń przychodzących i wychodzących przez dowolnego portu.

### <a name="view-effective-security-rules-for-a-virtual-machine"></a>Wyświetlanie reguł skutecznych zabezpieczeń dla maszyny wirtualnej

Wykonaj poniższe czynności, aby Rozwiązywanie problemów z NSGs dla maszyny:

Umożliwia wyświetlanie pełnej listy reguły skutecznych zabezpieczeń na NIC z maszyn wirtualnych się. Możesz również dodać, modyfikowanie i usuwanie reguł NSG zarówno NIC, jak i podsieć adresów z karta skuteczne reguły, jeśli masz uprawnienia do wykonania tych operacji.

1. Zaloguj się do portalu Azure pod adresem https://portal.azure.com.
2. Kliknij pozycję **więcej usług**, a następnie kliknij **maszyn wirtualnych** na liście.
3. Zaznacz maszyn wirtualnych rozwiązywać problemy z wyświetlonej listy, a zostanie wyświetlona karta maszyn wirtualnych przy użyciu opcji.
4. Kliknij pozycję **diagnozowanie & Rozwiązywanie problemów** , a następnie wybierz typowych problemów. W tym przykładzie **nie mogę się połączyć z moim maszyn wirtualnych systemu Windows** jest zaznaczone. 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)

5. Czynności są wyświetlane w obszarze problem, jak pokazano na poniższej ilustracji: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)

    Kliknij przycisk *reguły grupy zabezpieczeń skutecznych* na liście zalecanych kroków.

6. Karta **Pobieranie reguł skutecznych zabezpieczeń** pojawia się, jak pokazano na poniższej ilustracji:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)

    Zwróć uwagę, sekcjach obrazu:

    - **Zakres:** Skonfigurowano *VM1*, maszyn wirtualnych wybrany w kroku 3.
    - **Sieciowej:** *VM1 NIC1* jest zaznaczone. Maszyny mogą zawierać wiele interfejsów sieciowych (NIC). Każdy NIC mogą zawierać reguły unikatowe skutecznych zabezpieczeń. Podczas rozwiązywania problemów, może być konieczne wyświetlanie reguły skutecznych zabezpieczeń dla każdej karty sieciowej.
    - **Skojarzony NSGs:** NSGs można stosować do karty Sieciowej i podsieć, z którą jest połączony Sieciowej. Na obrazie zastosowano NSG zarówno karty Sieciowej, jak i podsieć, z którą jest połączony. Kliknięcie nazwy NSG bezpośrednio modyfikowania reguły w NSGs.
    - **Karta VM1 nsg:** Lista reguły wyświetlane na obrazie jest dla NSG zastosowane do karty sieciowej. Kilka domyślne reguły są tworzone przez Azure przy każdym utworzeniu NSG. Nie można usunąć domyślnych reguł, ale można je zastąpić z regułami wyższy priorytet. Aby uzyskać więcej informacji o regułach domyślnych, przeczytaj artykuł [Omówienie NSG](virtual-networks-nsg.md#default-rules) .
    - **Kolumna docelowa:** Tekst reguł jest w kolumnie, podczas gdy inne zawierają prefiksów adresów. Tekst jest nazwą znaczników domyślnych zastosowane do tej reguły zabezpieczeń podczas jej tworzenia. Znaczniki są system identyfikatory reprezentujących wielu prefiksów. Wybieranie reguły ze znacznikiem, takich jak *AllowInternetOutBound*Lista prefiksów karta **adresów** .
    - **Pobierz:** Na liście reguł może mieć długość. Możesz pobrać pliku CSV reguł analizy trybu offline, klikając pozycję **Pobierz** i zapisaniu pliku.
    - **AllowRDP** Reguła ruchu przychodzącego: Ta reguła zezwala na połączenia RDP do maszyn wirtualnych.
7. Kliknij kartę **Podsieć1 NSG** , aby wyświetlić efektywnej reguły z NSG stosowane do danej podsieci, jak pokazano na poniższej ilustracji: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)

    Zwróć uwagę, *denyRDP* reguły **przychodzące** . Reguły przychodzące stosowane na podsieci są sprawdzane przed reguł stosowanych w interfejsie sieciowym. Ponieważ reguła Odmów jest stosowana w danej podsieci, żądanie tak, aby nawiązać połączenie TCP 3389 zakończy się niepowodzeniem, ponieważ reguła zezwalania na karcie Sieciowej nigdy nie jest obliczana. 

    Reguła *denyRDP* jest powód, dlaczego połączenie RDP kończy się niepowodzeniem. Usunięcie należy rozwiązać ten problem.

    >[AZURE.NOTE]Jeśli maszyn wirtualnych, skojarzone z NIC nie jest w stanie uruchomienia lub NSGs jeszcze nie zostały zastosowane do karty Sieciowej lub podsieci, są wyświetlane żadne reguły.

8. Aby edytować reguły NSG, kliknij pozycję *Podsieć1 NSG* w sekcji **NSGs skojarzony** .
   Spowoduje to otwarcie karta **NSG podsieć1** . Reguły można edytować bezpośrednio, klikając pozycję na **Reguły przychodzące zabezpieczeń**.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)

9. Po wyłączeniu reguły przychodzące *denyRDP* w **Podsieć1 NSG** i dodawanie reguły *allowRDP* , na liście reguł skutecznych wygląda poniższej ilustracji:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)

    Upewnij się, że TCP port 3389 jest otwarty, Otwieranie połączenia RDP maszyn wirtualnych lub za pomocą narzędzia PsPing. Więcej informacji o PsPing można znaleźć, czytając [PsPing pobieranie strony](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="view-effective-security-rules-for-a-network-interface"></a>Wyświetlanie zasady zabezpieczeń skutecznych karty sieciowej

Jeśli dla określonych NIC dotyczy to usługi ruch maszyn wirtualnych, można wyświetlić pełną listę skuteczne reguły dla NIC z kontekstu interfejsów sieci, wykonując następujące czynności:

1. Zaloguj się do portalu Azure pod adresem https://portal.azure.com.
2. Kliknij pozycję **więcej usług**, a następnie kliknij **interfejsy sieciowe** na liście.
3. Wybierz pozycję karty sieciowej. Na poniższej ilustracji NIC o nazwie *VM1 NIC1* jest zaznaczone.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)

    Należy zauważyć, że **zakres** jest ustawiony na karcie sieciowej zaznaczone. Aby dowiedzieć się więcej o dodatkowe informacje umieszczone, przeczytaj krok 6 **Rozwiązywanie problemów z NSGs dla maszyny** części tego artykułu.

    >[AZURE.NOTE] Jeśli NSG zostanie usunięty z karty sieciowej, podsieci NSG obowiązuje nadal na danym NIC. W tym przypadku dane wyjściowe będą wyświetlane tylko reguły z podsieci NSG. Reguły są wyświetlane tylko, jeśli NIC jest dołączony do maszyny.

4. Reguły można edytować bezpośrednio na NSGs skojarzone z NIC i podsieć. Aby dowiedzieć się, jak to zrobić, przeczytaj krok 8 **Zasady zabezpieczeń skutecznych widoku maszyny wirtualnej** części tego artykułu.

## <a name="view-effective-security-rules-for-a-network-security-group-nsg"></a>Wyświetlanie skutecznych zabezpieczeń zasady grupy zabezpieczeń sieci (NSG)

Podczas modyfikowania reguły NSG, może chcesz przejrzeć wpływu reguł dodawany na określony maszyn wirtualnych. Zobaczysz pełną listę reguły skutecznych zabezpieczeń dla wszystkich sieciowe stosowane do danej NSG bez konieczności przełączania kontekstu z danym karta NSG. Aby rozwiązać skuteczne reguły w NSG, wykonaj następujące czynności:

1. Zaloguj się do portalu Azure pod adresem https://portal.azure.com.
2. Kliknij pozycję **więcej usług**, a następnie kliknij pozycję **grupy zabezpieczeń sieci** na liście.
3. Wybierz pozycję NSG. Na poniższej ilustracji NSG o nazwie VM1 nsg została wybrana.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)

    Zwróć uwagę następujące sekcje poprzedniego obrazu:

    - **Zakres:** Ustaw NSG zaznaczone.
    - **Maszyn wirtualnych:** NSG jest stosowana do podsieci, są stosowane do wszystkich interfejsów sieciowych dołączone do wszystkich maszyny wirtualne podłączony do podsieci. Ta lista zawiera wszystkie maszyny wirtualne ten NSG zostanie zastosowany do. Można wybrać dowolny maszyn wirtualnych z listy.

    >[AZURE.NOTE] Jeśli NSG dotyczy tylko puste podsieci, maszyny wirtualne nie są wyświetlane. Jeśli NSG zostanie zastosowany do NIC, który nie jest skojarzony z maszyny, te nic również nie będzie wymieniony. 
    - **Interfejs sieciowy:** Maszyny mogą zawierać wiele interfejsów sieciowych. Możesz wybrać karty sieciowej dołączone do wybranego maszyn wirtualnych.
    - **AssociatedNSGs:** W dowolnej chwili NIC może zawierać maksymalnie dwa NSGs skutecznych jedną zastosowane do karty Sieciowej, a drugi do podsieci. Mimo że zakres jest wybrany VM1 nsg, jeśli NIC ma skutecznych podsieci NSG, wynik zostanie wyświetlona obu NSGs.
4. Reguły można edytować bezpośrednio na NSGs skojarzony z NIC lub podsieci. Aby dowiedzieć się, jak to zrobić, przeczytaj krok 8 **Zasady zabezpieczeń skutecznych widoku maszyny wirtualnej** części tego artykułu.

Aby dowiedzieć się więcej o dodatkowe informacje umieszczone, przeczytaj krok 6 **Zasady zabezpieczeń skutecznych widoku maszyny wirtualnej** części tego artykułu.

>[AZURE.NOTE] Chociaż podsieci i NIC można mieć tylko raz NSG stosowane do ich, NSG można skojarzyć z kart i wielu podsieci.

## <a name="considerations"></a>Zagadnienia dotyczące

Podczas rozwiązywania problemów z łącznością, rozważ następujące wskazówki:

- Domyślne reguły NSG zostaną zablokować dostęp ruchu przychodzącego z Internetu i tylko zezwolić VNet ruch przychodzący. Zasady powinny zostać jawnie dodane umożliwiają dostęp ruchu przychodzącego z Internetu, zależnie od potrzeb.
- W przypadku reguł zabezpieczeń NSG powodujące łączność sieciowa maszyn wirtualnych awarię problemu może być:
    - Oprogramowaniem zapory uruchomionym w systemie operacyjnym maszyn wirtualnych
    - Trasy skonfigurowane dla urządzeń wirtualna lub lokalnego. Ruch internetowy mogą być przekierowywane do lokalnego za pośrednictwem wymuszone tunelowania. Połączenie RDP-SSH z Internetu do swojego maszyn wirtualnych może nie działać z tych ustawień, w zależności od tego, jak sprzętu sieciowego lokalnego obsługuje ruch. Przeczytaj artykuł [Przekierowuje rozwiązywania problemów](virtual-network-routes-troubleshoot-powershell.md) , aby dowiedzieć się, jak diagnozowanie problemów rozsyłania, które mogą utrudnić przepływu ruchu i maszyn wirtualnych. 
- Jeśli masz peered VNets, domyślnie, znacznik VIRTUAL_NETWORK automatycznie zostaną rozszerzone mają zawierać prefiksy dla peered VNets. Tych prefiksów można wyświetlać na liście **ExpandedAddressPrefix** rozwiązywać problemy związane z VNet zaglądanie łączności. 
- Reguły skutecznych zabezpieczeń są wyświetlane tylko jeśli jest NSG skojarzone z maszyn NIC i podsieci. 
- Jeśli istnieją nie NSGs skojarzone z karty Sieciowej lub podsieci i masz publiczny adres IP przypisanej do swojego maszyn wirtualnych, wszystkie porty będzie otwarty ruch przychodzący i wychodzący dostęp. Jeśli maszyn wirtualnych ma publiczny adres IP, zdecydowanie zalecane jest stosowanie NSGs do karty Sieciowej lub podsieci.