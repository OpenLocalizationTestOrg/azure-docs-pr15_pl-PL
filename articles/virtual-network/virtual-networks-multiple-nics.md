<properties
   pageTitle="Tworzenie maszyn wirtualnych przy użyciu kart"
   description="Dowiedz się, jak utworzyć i skonfigurować maszyny wirtualne z kart"
   services="virtual-network, virtual-machines"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management,azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="create-a-vm-with-multiple-nics"></a>Tworzenie maszyn wirtualnych przy użyciu kart

Można tworzyć wirtualnych maszyn platformy Azure i dołączyć wiele interfejsów sieciowych (NIC) do każdego z pośrednictwem usługi SMS. Wielokrotne NIC jest wymagane dla wielu urządzeń wirtualnych sieci, takich jak dostarczanie aplikacji i rozwiązań optymalizacji WAN. Wielokrotne NIC udostępnia więcej funkcji zarządzania ruchu sieciowego, włączając izolacji ruchu między wierzch zakończyć NIC i z powrotem end karty lub odstęp ruchu płaszczyzny danych z ruchu płaszczyzny zarządzania.

![Wielokrotne NIC dla maszyn wirtualnych](./media/virtual-networks-multiple-nics/IC757773.png)

Powyższym rysunku pokazano maszyn wirtualnych z trzech nic każdy połączony z innej podsieci.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]

- Z Internetu VIP (klasyczny wdrożeń) jest obsługiwana tylko na karcie NIC. "wartość domyślna" Istnieje tylko jedna VIP adres IP domyślne karty sieciowej.
- W tej chwili adresów IP publicznej poziom wystąpienia (LPIP) (klasyczny wdrożeń) nie są obsługiwane dla wielu NIC maszyny wirtualne.
- Kolejność nic z wewnątrz maszyn wirtualnych będzie losowe i może również zmienić między aktualizacjami Azure infrastruktury. Jednak adresów IP, a odpowiadające im ethernet MAC dotyczy będzie pozostają bez zmian. Załóżmy na przykład, że **Eth1** ma adres IP 10.1.0.100 i adres MAC 00-0D-3A-B0-39-0D; Po aktualizacji infrastruktury Azure i uruchom ponownie komputer może być zmieniane **Eth2**, ale IP i MAC sparowania pozostaną niezmienione. Jeśli ponowne uruchomienie jest zainicjowany przez klienta, kolejności NIC pozostaną niezmienione.
- Adres dla każdej karty Sieciowej w poszczególnych maszyn wirtualnych musi znajdować się w podsieci, kart na jednym maszyn wirtualnych każdego można przypisywać adresy, które znajdują się w tej samej podsieci.
- Rozmiar pamięci Wirtualnej określa liczbę kart NIC, którą można utworzyć dla maszyny. Wykaz [Systemu Windows Server](../virtual-machines/virtual-machines-windows-sizes.md) i [Linux](../virtual-machines/virtual-machines-linux-sizes.md) maszyn wirtualnych rozmiarów artykułów, aby określić, ile nic obsługuje każdego rozmiar pamięci Wirtualnej. 

## <a name="network-security-groups-nsgs"></a>Grupy zabezpieczeń sieci (NSGs)
We wdrożeniu Menedżera zasobów dowolnej karty Sieciowej w maszyny może być skojarzone z sieci zabezpieczeń grupy (NSG), łącznie z dowolnej nic na maszyn wirtualnych, zawierającą kart włączone. Jeśli NIC ma przypisany adres w miejsce, w którym jest skojarzony z NSG podsieci podsieci, następnie reguły w danej podsieci NSG też zastosowane do tej karty sieciowej. Oprócz kojarzenia podsieci z NSGs, można także skojarzyć NIC z NSG.

Jeśli podsieć jest skojarzony z NSG i NIC w tej podsieci pojedynczo skojarzone z NSG, skojarzone reguły NSG są stosowane w **kolejności przepływu** zgodnie z kierunkiem ruchu były przekazywane do lub z karty Sieciowej:

- **Ruch przychodzący** , których miejscem docelowym jest w danym NIC przepływa najpierw przez podsieci, powodujące reguły NSG danej podsieci, przed przekazaniem do karty Sieciowej, a następnie powodujące reguły NSG karty Sieciowej.
- **Ruch wychodzący** źródłem jest w danym NIC będzie przepływał najpierw się od NIC, powodujące reguły NSG karty Sieciowej, przed przechodzące przez podsieci, a następnie powodujące danej podsieci NSG reguły.

Dowiedz się więcej o [Grup zabezpieczeń sieci](virtual-networks-nsg.md) i jak są stosowane oparte na skojarzenia z podsieci, maszyny wirtualne i nic...

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>Jak skonfigurować wiele maszyn wirtualnych NIC w klasycznym wdrożeniu

Poniższe instrukcje pomoże Ci utworzyć wiele maszyn wirtualnych NIC zawierające nic 3: domyślne NIC i dwie dodatkowe karty sieciowe. Kroki konfiguracji utworzy maszyny skonfigurowanym według poniżej fragment pliku konfiguracji usługi:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


Potrzebujesz następujące wymagania wstępne przed próby uruchomienia polecenia programu PowerShell w przykładzie.

- Subskrypcję usługi Azure.
- Skonfigurowaną wirtualnej sieci. Aby uzyskać więcej informacji na temat VNets, zobacz [Omówienie wirtualnych sieci](virtual-networks-overview.md) .
- Najnowszą wersję programu Azure PowerShell pobranego i zainstalowanego. Dowiedz się, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

Aby utworzyć maszyny z kart, wykonaj następujące czynności:

1. Wybierz obraz maszyn wirtualnych z galerii obrazów maszyn wirtualnych Azure. Należy zauważyć, że obrazy często zmieniają i są dostępne według regionów. Obraz określony w poniższym przykładzie może zmienić lub może nie może być w danym regionie, dlatego należy określić obrazu, które są potrzebne.

        $image = Get-AzureVMImage `
            -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"

1. Utworzenie konfiguracji maszyn wirtualnych.

        $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
            -Image $image.ImageName –AvailabilitySetName "MyAVSet"

1. Utwórz identyfikator logowania administratora domyślne.

        Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
            -Password "<YourAdminPassword>"

1. Dodać dodatkowe nic do konfiguracji maszyn wirtualnych.

        Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
            -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
        Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
            -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm

1. Określ podsieci i adres IP dla domyślnych karty sieciowej.

        Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
        Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm

1. Tworzenie maszyn wirtualnych w wirtualnej sieci.

        New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm

>[AZURE.NOTE] VNet, określone w tym miejscu, musi już istnieć (wymieniony w wymagania wstępne). W poniższym przykładzie określa, wirtualną sieć o nazwie **MultiNIC VNet**.

## <a name="limitations"></a>Ograniczenia

Podczas korzystania z funkcji NIC wielokrotne stosuje się następujące ograniczenia:

- Maszyny wirtualne NIC wielokrotne muszą zostać utworzone w Azure wirtualnych sieci (VNets). Nie można skonfigurować maszyny wirtualne nie VNet z wieloma nic.
- Wszystkie maszyny wirtualne w zestawie dostępność trzeba użyć wielu NIC lub pojedynczej karty sieciowej. Nie może być maszyny wirtualne NIC wielokrotne i pojedynczy NIC maszyny wirtualne w zestawie dostępności. Same reguły są stosowane do maszyny wirtualne w usłudze w chmurze.
- Maszyn wirtualnych z jednego NIC nie można skonfigurować przy użyciu wielu nic (i odwrotnie) po wdrożeniu, bez usunięcie i ponowne utworzenie.


## <a name="secondary-nics-access-to-other-subnets"></a>Awaryjny nic dostęp do innych podsieci

Domyślnie pomocniczej nic nie zostanie skonfigurowany z bramą domyślną ze względu na które będzie ograniczony ruch na pomocniczym nic się w tej samej podsieci. Jeśli użytkownicy mają Włącz pomocniczą nic porozmawiać spoza własnych podsieci, musisz dodać wpis w tabeli routingu, aby skonfigurować bramy w sposób opisany poniżej.

>[AZURE.NOTE] Maszyny wirtualne utworzonych przed lipca 2015 r. napotkać brama domyślna jest skonfigurowana dla wszystkich kart sieciowych. Brama domyślna pomocniczej nic nie zostaną usunięte, dopóki nie zostaną ponownie uruchomić te maszyny wirtualne. W systemach operacyjnych, korzystające z modelu routingu hosta słabych, takich jak Linux łączność z Internetem rozbicie użycie ruch ingress i wyjściowego nic innego.

### <a name="configure-windows-vms"></a>Konfigurowanie maszyny wirtualne systemu Windows

Załóżmy, że masz maszyn wirtualnych systemu Windows z dwiema nic w następujący sposób:

- Podstawowy adres NIC IP: 192.168.1.4
- Pomocniczego adresu NIC IP: 192.168.2.5

Ten maszyn wirtualnych w tabeli rozsyłania IP protokołu IPv4 będzie miała następującą postać:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Zwróć uwagę, że trasy domyślnej (0.0.0.0) jest dostępna tylko dla podstawowego karty sieciowej. Nie można uzyskać dostęp do zasobów spoza podsieci dla pomocniczej NIC, jak pokazano poniżej:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

Aby dodać trasę domyślną na pomocniczym Sieciowej, wykonaj następujące czynności:

1. W wierszu polecenia uruchom poniższe polecenie, aby zidentyfikować numer indeksu dla pomocniczej NIC:

        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================

2. Zwróć uwagę, drugiej pozycji w tabeli z indeksem 27 (w tym przykładzie).
3. W wierszu polecenia Uruchom polecenie **Dodaj trasę** , tak jak pokazano poniżej. W tym przykładzie są określanie 192.168.2.1 jako brama domyślna dla pomocniczej NIC:

        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27

4. Aby przetestować łączność, wróć do wiersza polecenia i spróbujesz ping innej podsieci z pomocniczym NIC jako wyświetlana eh w poniższym przykładzie:

        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128

5. Możesz również sprawdzić tabeli routingu sprawdzić trasę nowo dodany, tak jak pokazano poniżej:

        C:\Users\Administrator>route print

        ...

        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Konfigurowanie maszyny wirtualne Linux

Maszyny wirtualne Linux ponieważ domyślne zachowanie używane słabych hosta routingu, zalecamy że pomocniczej sieciowe są ograniczone do przepływu ruchu tylko w tej samej podsieci. Jednak jeśli niektórych scenariuszach żądanie łączności spoza podsieci, użytkownicy włączyć zasady podstawie kierowaniu do upewnić, że ruch ingress i wyjściowym używa samej karty sieciowej.

## <a name="next-steps"></a>Następne kroki

- Wdrażanie [MultiNIC maszyny wirtualne w scenariuszu aplikacji poziomu 2 we wdrożeniu Menedżera zasobów](virtual-network-deploy-multinic-arm-template.md).
- Wdrażanie [MultiNIC maszyny wirtualne w scenariuszu aplikacji poziomu 2 w klasycznym wdrożeniu](virtual-network-deploy-multinic-classic-ps.md).
