<properties 
   pageTitle="Przyspieszona sieć w maszyny wirtualnej - programu PowerShell | Microsoft Azure"
   description="Dowiedz się, jak skonfigurować przyspieszona Networking Azure maszyn wirtualnych przy użyciu programu PowerShell."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
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
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Przyspieszona sieci dla maszyny wirtualnej

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-accelerated-networking-portal.md)
- [Programu PowerShell](virtual-network-accelerated-networking-powershell.md)

Przyspieszona sieci umożliwia jednym głównym we/wy wirtualizacji (SR IOV) maszyn wirtualnych (Głosowa), znacznie poprawia wydajności sieci. Ta ścieżka wysokiej wydajności pomija hosta z ścieżki danych, zmniejszenie opóźnienia, zakłócenia i użycie Procesora przez najbardziej wymagających obciążeń sieci na obsługiwane typy maszyn wirtualnych. W tym artykule wyjaśniono, jak za pomocą programu PowerShell Azure Konfigurowanie przyspieszona sieć w modelu wdrożenia Azure Menedżera zasobów. Można także tworzyć maszyny z przyspieszona sieci przy użyciu Azure Portal. Aby dowiedzieć się, jak to zrobić, kliknij pole Azure Portal na początku tego artykułu.

Na poniższej ilustracji przedstawiono komunikacji między dwoma maszyn wirtualnych (maszyn wirtualnych) z i bez przyspieszona sieci:

![Porównanie](./media/virtual-network-accelerated-networking-powershell/image1.png)

Bez przyspieszona sieci wszystkich ruchu sieciowego i maszyn wirtualnych muszą przejść hosta i przełącznika wirtualną. Wirtualny przełącznik zawiera wszystkie wymuszania zasad, takich jak grupy zabezpieczeń sieci, listy kontroli dostępu, izolacji i innych usług sieci z obsługą ruchu sieciowego. Aby dowiedzieć się więcej, przeczytaj artykuł [wirtualizacji sieci funkcji Hyper-V i wirtualny przełącznik](https://technet.microsoft.com/library/jj945275.aspx) .

Przyspieszona sieciowych ruch sieciowy nadejścia karty sieciowej (NIC) i następnie przekazywana maszyn wirtualnych. Wszystkie zasady sieci stosowanych wirtualny przełącznik bez przyspieszona sieć są przekazywać i sprzętu. Stosowanie zasad sprzętu umożliwia NIC do przodu ruchu sieciowego bezpośrednio do Głosowa, pomijanie hosta i wirtualny przełącznik przy zachowaniu zasady, które on zastosowany w polu host.

Zalety przyspieszona sieć są stosowane tylko do maszyn wirtualnych, który jest włączone. Aby uzyskać najlepsze wyniki jest idealnym rozwiązaniem włączyć tę funkcję, na co najmniej dwa maszyny wirtualne połączony z tym samym VNet.  Podczas komunikacji za pośrednictwem VNets lub łączących lokalnego, ta funkcja ma minimalny wpływ na ogólną opóźnienie.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Zalety

- **Mniejsze opóźnienia / wyższą pakietów na sekundę (pps):** Odebranie wirtualny przełącznik ścieżki danych usuwa razem, gdy poświęcają pakietów hosta przetwarzanie zasad i zwiększanie liczby pakietów, które mogą być przetwarzane wewnątrz maszyn wirtualnych.
- **Zmniejszenia zakłócenia:** Przetwarzanie wirtualny przełącznik zależy od ilości zasady, które należy zastosować i obciążenie Procesora robi przetwarzania. Odciążanie wymuszania zasad sprzętu usuwa tego zmienności dostarczania pakietów bezpośrednio do Głosowa, usuwając hosta maszyn wirtualnych komunikacji wszystkich przerwania oprogramowania i przełączniki kontekstu.
- **Zmniejszyła się procesora:** Pomijanie wirtualny przełącznik na hoście prowadzi do mniej procesora przetwarzania ruchu sieciowego.

## <a name="limitations"></a>Ograniczenia

Następujące ograniczenia istnieją podczas korzystania z tej funkcji:
 
- **Sieci tworzenia interfejsu:** Przyspieszona sieci można włączyć tylko dla nowego interfejsu sieciowego.  Nie można włączyć dla istniejącego interfejsu sieci.
- **Tworzenie maszyn wirtualnych:** Karty sieciowej z włączoną obsługą przyspieszona sieci tylko może zostać dołączony do maszyny po utworzeniu maszyn wirtualnych. Interfejs sieciowy nie zostaną dołączone do istniejącego maszyn wirtualnych.
- **Regionów:** Oferowane w tylko regionów zachód centralnej nam oraz Azure Europe Zachód. Ustawianie regionów zostaną rozszerzone w przyszłości.
- **System operacyjny obsługiwane:** Microsoft Windows Server 2012 R2 i Windows Server 2016 Technical Preview 5. Wkrótce zostanie dodany Linux i systemu Windows Server 2012 pomocy technicznej.
- **Rozmiar pamięci Wirtualnej:** Standard_D15_v2 i Standard_DS15_v2 są obsługiwane tylko rozmiarów wystąpienie maszyn wirtualnych. Aby uzyskać więcej informacji zobacz artykuł [rozmiarów maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-sizes.md) . Zestaw obsługiwane rozmiary wystąpienie maszyn wirtualnych zostaną rozszerzone w przyszłości.

Zmiany te ograniczenia zostanie ogłoszone za pośrednictwem strony [Azure wirtualna sieć — aktualizacje](https://azure.microsoft.com/updates/accelerated-networking-in-preview) .

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Tworzenie maszyn wirtualnych systemu Windows z sieci przyspieszona

1. Otwórz okno wiersza polecenia programu PowerShell i wykonaj pozostałe kroki w tej sekcji w ramach jednej sesji programu PowerShell. Jeśli nie masz jeszcze programu PowerShell zainstalowana i skonfigurowana, wykonaj czynności opisane w artykule [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .
2. Aby zarejestrować, aby uzyskać podgląd, wysyłanie wiadomości e-mail [Przyspieszona subskrypcje sieci](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) przy użyciu swojego Identyfikatora subskrypcji i przeznaczenie. Nie wykonaj pozostałe kroki do momentu, gdy otrzymasz wiadomość e-mail z powiadomieniem, zostały już zaakceptowane do podglądu.
3. Zarejestruj się funkcja przy użyciu swojej subskrypcji, wpisując następujące polecenia:

        Register-AzureRmProviderFeature -FeatureName AllowAcceleratedNetworkingFeature -ProviderNamespace Microsoft.Network
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network

4. Zamień *westcentralus* nazwę innej lokalizacji obsługiwane przez tę funkcję wymienionych w sekcji [ograniczenia](#limitations) tego artykułu (Jeśli to konieczne). Wpisz następujące polecenie, aby ustawić zmiennej lokalizacji:

        $locName = "westcentralus"

5. Zamień *RG1* nazwę grupy zasobów, która będzie zawierać nowy interfejs sieci i wpisz następujące polecenia, aby go utworzyć:

        $rgName = "RG1"
        New-AzureRmResourceGroup -Name $rgName -Location $locName

6. Zmienianie *VM1 NIC1* ma być nazwa interfejsu sieciowego, a następnie wprowadź następujące polecenie:

        $NICName = "VM1-NIC1"

7. Interfejs sieciowy musi być połączony z podsieci istniejącą sieć wirtualna Azure (VNet) w tej samej lokalizacji i [subskrypcji](../azure-glossary-cloud-terminology.md#subscription) jako interfejsu sieciowego. Dowiedz się więcej o VNets, czytając artykuł [Omówienie wirtualnych sieci](virtual-networks-overview.md) , jeśli nie wiesz, jak z nimi. Aby utworzyć VNet, wykonaj czynności opisane w artykule [Tworzenie VNet](virtual-networks-create-vnet-arm-ps.md) . Zmienianie "wartości" następujące $Variables do nazwy VNet i podsieci, które chcesz połączyć sieciowej do.

        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"

    Jeśli nie znasz nazwy istniejących VNet w lokalizacji wybranej w kroku 3, wpisz następujące polecenia:
        
        $VirtualNetworks = Get-AzureRmVirtualNetwork
        $VirtualNetworks | Where-Object {$_.Location -eq $locName} | Format-Table Name, Location
        
    Jeśli lista, zwracany jest pusty, należy utworzyć VNet w wybranej lokalizacji. Aby utworzyć VNet, wykonaj czynności opisane w artykule [Tworzenie wirtualnych sieci](virtual-networks-create-vnet-arm-ps.md) .

    Aby uzyskać nazwę podsieci w VNet, wpisz następujące polecenia i Zamień *podsieć1* powyżej nazwę podsieci:
        
        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

8. Wpisz następujące polecenia, aby pobrać VNet i podsieci i przypisanie ich do zmiennych.

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $rgName
        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

9. Identyfikowanie istniejącej publicznej zasób adres IP, który można skojarzyć z interfejsu sieciowego, pozwalający łączyć do niego przez Internet. Jeśli nie chcesz uzyskać dostęp do maszyn wirtualnych z interfejsem sieci w Internecie, możesz pominąć ten krok. Bez publiczny adres IP możesz połączyć się maszyn wirtualnych z innego maszyn wirtualnych połączony z tym samym VNet. 

    Zmienić nazwę istniejącej publicznej zasób adresu IP znajdujące się w lokalizacji tworzysz interfejsu sieciowego w i nie jest obecnie skojarzony z innego sieciowej *PIP1* . W razie potrzeby zmień $rgName nazwę grupy zasobów publicznej zasobu adresu IP istnieje w i wpisz następujące polecenie:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $rgName

    Jeśli nie znasz nazwę istniejącej publicznej zasobu adresu IP, wpisz następujące polecenia:

        $PublicIPAddresses = Get-AzureRmPublicIPAddress
        $PublicIPAddresses | Where-Object { $_.Location -eq $locName } |Format-Table Name, Location, IPAddress, IpConfiguration

    Jeśli kolumna **IPConfiguration** nie ma wartości w dane wyjściowe zwracane, zasób publiczny adres IP nie jest skojarzony z istniejącego interfejsu sieci i mogą być używane. Jeśli lista jest pusta lub nie ma żadnych dostępnych zasobów adresu publicznych adresów IP, możesz go utworzyć za pomocą polecenia Nowy AzureRmPublicIPAddress.

    >[AZURE.NOTE] Publiczne adresy IP mają nominalna opłaty. Dowiedz się więcej o cenach, czytając strony [ceny adres IP](https://azure.microsoft.com/pricing/details/ip-addresses) .
10. Jeśli nie chcesz dodać publicznej zasobu adresu IP do interfejsu, Usuń *- PublicIPAddress $PIP1* na końcu polecenia znajdujący się. Utworzenie interfejsu sieci z sieci przyspieszona, wpisując następujące polecenie:

        $nic = New-AzureRmNetworkInterface -Location $locName -Name $NICName -ResourceGroupName $rgName -Subnet $Subnet -EnableAcceleratedNetworking -PublicIpAddress $PIP1 

11. Przypisz interfejsu sieciowego do maszyny podczas tworzenia maszyn wirtualnych, wykonując instrukcje opisane w krokach 3 i 6 artykułu [Tworzenie maszyn wirtualnych](../virtual-machines/virtual-machines-windows-ps-create.md) . W kroku 6-2 zastąpić *Standard_A1* rozmiarów maszyn wirtualnych wymienionych w sekcji [ograniczenia](#limitations) tego artykułu.

    >[AZURE.NOTE] Jeśli zmienisz *nazwę* $locName, $rgName lub $nic zmiennych w tym artykule krok 6 w sekcji Tworzenie artykuł maszyn wirtualnych nie powiedzie się. Można jednak zmieniać *wartości* zmiennych.

12. Po utworzeniu maszyn wirtualnych pobrać [sterownik przyspieszona sieci](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), nawiązywanie połączenia z maszyn wirtualnych i uruchom Instalatora sterownika wewnątrz maszyn wirtualnych.

13. Kliknij prawym przyciskiem myszy przycisk systemu Windows, a następnie kliknij pozycję **Menedżer urządzeń**. Sprawdź, czy **Karta Ethernet funkcja wirtualna Mellanox ConnectX-3** jest wyświetlana w obszarze opcji **sieci** po rozwinięciu, jak pokazano na poniższej ilustracji:

    ![Menedżer zadań](./media/virtual-network-accelerated-networking-powershell/image2.png)