<properties
   pageTitle="Wiele adresów IP dla maszyn wirtualnych - programu PowerShell | Microsoft Azure"
   description="Dowiedz się, jak przypisać wiele adresów IP przy użyciu programu PowerShell Azure maszyny wirtualnej."
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
   ms.date="10/05/2016"
   ms.author="jdial;annahar" />


# <a name="assign-multiple-ip-addresses-to-virtual-machines"></a>Przypisywanie wiele adresów IP do maszyn wirtualnych

Azure maszyn wirtualnych (maszyn wirtualnych) mogą zawierać jedną lub więcej interfejsów sieciowych (NIC) dołączone do niego. Wszelkie NIC może mieć co najmniej jeden publiczny czy prywatny adresów IP przypisanych do niego. Jeśli nie znasz adresów IP w Azure, przeczytaj artykuł [adresów IP w Azure](virtual-network-ip-addresses-overview-arm.md) , aby dowiedzieć się więcej na ich temat. W tym artykule wyjaśniono, jak przypisać wiele adresów IP do maszyn wirtualnych w modelu wdrożenia Menedżera zasobów Azure za pomocą programu PowerShell Azure.

Przypisywanie wiele adresów IP do maszyny udostępnia następujące możliwości:

- Obsługa wielu witryn sieci Web lub usług z różnymi adresami IP i certyfikatów SSL na serwerze.
- Służyć jako urządzenia wirtualnej sieci, takich jak Zapora lub Usługa równoważenia obciążenia.
- Możliwość dodawania dowolną prywatnych adresów IP dla każdego sieciowe do puli wewnętrznej równoważenia obciążenia Azure. W przeszłości tylko podstawowy adres IP podstawowego NIC może można dodać do puli wewnętrznej.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

Aby zarejestrować, aby uzyskać podgląd, wysłanie wiadomości e-mail do [Wielu adresy IP](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) przy użyciu swojego Identyfikatora subskrypcji i przeznaczenie.

## <a name="scenario"></a>Scenariusz

W tym artykule zostanie skojarzony trzech konfiguracji adresów IP do karty sieciowej.
Następujące przykładowe konfiguracje zostanie utworzona i przypisana do NIC, zawierające trzy prywatnych adresów IP i jeden publiczny adres IP do niego przypisana:

- IPConfig-1: Dynamiczne prywatny adres IP (ustawienie domyślne) i publiczny adres IP adresu z publicznej zasobu adresu IP o nazwie PIP1.
- IPConfig-2: Statycznego adresu IP prywatny i nie publiczny adres IP.
- IPConfig-3: Dynamiczne prywatny adres IP i nie publiczny adres IP.

    ![Tekst alternatywny obrazu](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

W tym scenariuszu założono, że masz grupę zasobów o nazwie *RG1* , w którym jest VNet o nazwie *VNet1* i podsieć o nazwie *podsieć1*. Ponadto zakłada się, czy masz maszyny o nazwie *VM1*skojarzone karty sieciowej o nazwie *VM1 NIC1* i *PIP1*o nazwie publiczny adres IP.

[W tym artykule](./virtual-machines/virtual-machines-windows-ps-create.md ) opisano sposób tworzenia zasobów wymienionych powyżej, w przypadku, gdy nie zostały utworzone przed.

## <a name = "create"></a>Tworzenie maszyn wirtualnych przy użyciu wielu adresów IP

1. Otwórz okno wiersza polecenia programu PowerShell i wykonaj pozostałe kroki w tej sekcji w ramach jednej sesji programu PowerShell. Jeśli nie masz jeszcze programu PowerShell zainstalowana i skonfigurowana, wykonaj czynności opisane w artykule [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .

2. Zmienianie "wartości" następujące $Variables do odpowiedniej Azure [lokalizacji](https://azure.microsoft.com/regions) wirtualna sieć działa w nazwie [Grupa zasobów](../azure-resource-manager/resource-group-overview.md#resource-groups), VNet w grupie zasobów, podsieci, w którym chcesz połączyć NIC, aby i nazwę karty sieciowej. Wykonaj kroki, aby dodać wiele adresów IP do dowolnego NIC dołączone do Głosowa, trzeba.

        $Location = "westcentralus"
        $RgName   = "RG1"
        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"
        $NicName     = "VM1-NIC1"
        $PIP = "PIP"

    Jeśli nie znasz nazwy istniejącej lokalizacji Azure lub grupa zasobów, wpisz następujące polecenia:

        Get-AzureRmLocation      | Format-Table Location
        Get-AzureRmResourceGroup | Format-Table ResourceGroupName

    <a name="subnet"></a>Karta sieciowa musi być połączony z podsieci istniejącą sieć wirtualna Azure (VNet). Trzy składniki: NIC, podsieci i VNet, musi istnieć w ustawieniach regionalnych i [subskrypcji](../azure-glossary-cloud-terminology.md#subscription).  Jeśli nie znasz VNets, przeczytaj artykuł [Omówienie wirtualnych sieci](virtual-networks-overview.md) , aby dowiedzieć się więcej o ich lub przeczytaj artykuł [Tworzenie VNet](virtual-networks-create-vnet-arm-ps.md) , aby dowiedzieć się, jak go utworzyć.

    Jeśli nie znasz nazwy istniejących VNet, wpisz następujące polecenie i Zastąp *VNet1* w poprzednim zmiennej o nazwie VNet:

        Get-AzureRmVirtualNetwork | Format-Table Name

    Jeśli lista, zwracany jest pusty, należy utworzyć VNet. Aby dowiedzieć się, jak to zrobić, przeczytaj artykuł [Tworzenie wirtualnych sieci](virtual-networks-create-vnet-arm-ps.md) .

    Wpisz następujące polecenia, aby uzyskać nazwę istniejącego podsieci w VNet i Zamień *podsieć1* powyżej nazwę podsieci:

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

4. Wpisz następujące polecenie, aby pobrać podsieci i przypisać ją do zmiennej.

        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

5. <a name="ipconfigs"></a>Definiowanie konfiguracji adresów IP, który ma zostać przypisany do karty sieciowej. Każdej konfiguracji mogą mieć jeden statyczną lub dynamiczną prywatny adres IP i jedną skojarzone z adresem statyczną lub dynamiczną publicznej zasobu adresu IP.

    Dodawanie lub usuwanie dowolną liczbę konfiguracji, które należy wykonać w zależności od liczby adresów IP chcesz skojarzyć Sieciowej i ustawienia chcesz skonfigurować.

    **IPConfig-1**

    Zmień wartość *PIP1* na nazwę istniejącej publicznej zasób adresu IP znajdujące się w lokalizacji, w której tworzysz NIC w i nie jest obecnie skojarzony z innej karty sieciowej. Zmienianie *RG1* do nazwy grupy zasobów publicznej zasobu adresu IP istnieje. Zmienianie *IPConfig-1* do nazwy, które chcesz przekazać do pierwszej konfiguracji adresów IP. Wpisz następujące polecenia:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $RgName

        $IpConfigName1 = "IPConfig-1"
        $IPConfig1     = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName1 -Subnet $Subnet -PublicIpAddress $PIP1 -Primary

    Uwaga *-podstawowego* przełączanie. Podczas przypisywania wielu konfiguracji IP NIC jedna konfiguracja musi mieć przypisane jako *podstawowego*. Jeśli nie znasz nazwę istniejącej publicznej zasobu adresu IP, wpisz następujące polecenie: Get-AzureRMPublicIPAddress | Formatowanie tabeli nazwę, lokalizację, adres IP, IpConfiguration

    Jeśli kolumna **IPConfiguration** nie ma wartości w dane wyjściowe zwracane, zasób publiczny adres IP nie jest skojarzony z istniejących NIC i mogą być używane. Jeśli lista jest pusta lub nie ma żadnych dostępnych zasobów adresu publicznych adresów IP, możesz go utworzyć za pomocą polecenia **Nowy AzureRmPublicIPAddress** .

    >[AZURE.NOTE] Publiczne adresy IP mają nominalna opłaty. Aby dowiedzieć się więcej o cenach adres IP, przeczytaj strony [ceny adres IP](https://azure.microsoft.com/pricing/details/ip-addresses) .

    **IPConfig-2**

    Zmienić nazwę, której chcesz przekazać do drugiego konfiguracji IP i zmienić *10.0.0.5* nieużywane prawidłowy adres IP dla podsieci, którego przypisywane NIC do *IPConfig-2* . Wpisz następujące polecenia:

        $IPConfigName2 = "IPConfig-2"
        $IPAddress = 10.0.0.5

        $IPConfig2 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName2 -Subnet $Subnet -PrivateIpAddress $IPAddress

    Wpisz następujące polecenie, jeśli nie wiesz, IP zakresu przypisane do podsieć adresów:

        $VNet.Subnets | Format-Table Name, AddressPrefix

    **IPConfig-3**

    Zmień nazwę, którą chcesz nadać w trzecim konfiguracji adresów IP, a następnie wprowadź następujące polecenia *IPConfig 3* :

        $IPConfigName3 = "IPConfig-3"
        $IPConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet

    >[AZURE.NOTE] Możesz przypisać do 250 prywatny adres IP karty sieciowej. Istnieje ograniczenie liczby publicznych adresów IP, które mogą być używane w ramach subskrypcji. Aby dowiedzieć się więcej, przeczytaj artykuł [ogranicza Azure](../azure-subscription-service-limits.md#networking-limits---azure-resource-manager) .

6. Tworzenie karty Sieciowej przy użyciu konfiguracji adresów IP określonych w poprzednim kroku.

        $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName -Location $Location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3

7. Dołącz NIC, podczas tworzenia maszyny, wykonując czynności opisane w artykule [Tworzenie maszyn wirtualnych](../virtual-machines/virtual-machines-windows-ps-create.md) . Jeśli artykuł tworzy maszyny z systemem Windows Server, czynności są takie same dla Głosowa Linux, oprócz wybrania innego systemu operacyjnego. Wykonaj kroki od 1 do 3 tego artykułu. Pomiń kroki 4 i 5, a następnie wykonaj krok 6 w sekcji Tworzenie artykuł maszyn wirtualnych.

    >[AZURE.WARNING] Krok 6 w sekcji Tworzenie artykuł maszyn wirtualnych zakończy się niepowodzeniem, jeśli zmieniono zmiennej o nazwie $nic na inny w kroku 6 niniejszego artykułu, lub nie zostały wykonane powyższe kroki w tym artykule.

8. Widok prywatne IP dotyczy tego DHCP Azure przypisane do karty Sieciowej i publicznej zasobu adresu IP przypisane do karty Sieciowej, wpisując następujące polecenie:

        $nic.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

9. <a name="os"></a>Ręczne dodawanie wszystkich prywatnych adresów IP (w tym podstawowych) do konfiguracji TCP/IP w systemie operacyjnym. 

**Systemu Windows**

1. W wierszu polecenia wpisz *ipconfig/all*.  Wyświetlać tylko *podstawowy* prywatny adres IP (za pośrednictwem DHCP).
2. Następnie wpisz *ncpa.cpl* w oknie wiersza polecenia. Spowoduje to otwarcie nowego okna.
3. Otwórz okno właściwości połączenie **lokalne**.
4. Kliknij dwukrotnie IP w wersji 4 (IPv4)
5. Zaznacz pole wyboru **Użyj następującego adresu IP** i wprowadź następujące wartości:
    - **Adres IP**: wpisz *podstawowy* adres IP prywatnych
    - **Maska podsieci**: Ustawianie według danej podsieci. Na przykład w przypadku podsieci /24 podsieć następnie maska podsieci jest 255.255.255.0.
    - **Brama domyślna**: pierwszego adresu IP w podsieci. Jeśli danej podsieci jest 10.0.0.0/24, adres IP bramy jest 10.0.0.1.
    - Kliknij opcję **Użyj następujących adresów serwerów DNS** i wprowadź następujące wartości:
        - **Preferowany serwer DNS:** Jeśli nie używasz serwera DNS, wpisz 168.63.129.16.  Jeśli jesteś, wprowadź adres IP serwera DNS.
    - Kliknij przycisk **Zaawansowane** , a następnie dodać dodatkowe adresy IP. Dodaj każdego z pomocniczych adresów IP prywatne opisanych w kroku 8 do karty Sieciowej z tej samej podsieci określone dla podstawowego adresu IP.
    - Kliknij **przycisk OK** , aby zamknąć ustawienia TCP/IP, a następnie **przycisk OK** , aby zamknąć ustawienia karty. Spowoduje to ponownie ustanów połączenie RDP.

6. W wierszu polecenia wpisz *ipconfig/all*. Zostaną wyświetlone wszystkie adresy IP, które zostały dodane a DHCP jest wyłączona.


**Linux (Ubuntu)**

1. Otwieranie okna końcowych.
2. Upewnij się, że jesteś administratora. Jeśli nie jesteś, możesz to zrobić przy użyciu następującego polecenia:

            sudo -i
3. Aktualizowanie pliku konfiguracji interfejsu sieci (przy założeniu "eth0").
    - Zachowaj istniejącej pozycji DHCP. Skonfiguruje podstawowego adresu IP jako on być wcześniejszy.
    - Dodawanie konfiguracji dodatkowe statycznego adresu IP przy użyciu następujących poleceń:

                cd /etc/network/interfaces.d/
                ls

        Powinien zostać wyświetlony pliku .cfg.
4. Otwórz plik: vi *nazwy pliku*.

    Powinny zostać wyświetlone następujące wiersze na końcu pliku:

            auto eth0
            iface eth0 inet dhcp
5. Dodaj następujące wiersze po wierszu znajdują się w tym pliku:

            iface eth0 inet static
            address <your private IP address here>
6. Zapisz plik przy użyciu następującego polecenia:

            :wq
7.  Zresetuj sieciowej przy użyciu następującego polecenia:

            sudo ifdown eth0 && sudo ifup eth0

    >[AZURE.IMPORTANT] Uruchomienie zarówno ifdown, jak i ifup w tym samym wierszu, jeśli za pomocą połączenia zdalnego.
8. Sprawdź, czy adres IP jest dodawany do sieciowej przy użyciu następującego polecenia:

            ip addr list eth0

    Powinien zostać wyświetlony adres IP, dodane w ramach listy.

**Linux (Redhat, CentOS i inne)**

1. Otwieranie okna końcowych.
2. Upewnij się, że jesteś administratora. Jeśli nie jesteś, możesz to zrobić przy użyciu następującego polecenia:

            sudo -i
3. Wprowadź hasło, a następnie postępuj zgodnie instrukcjami zostanie wyświetlony monit. Będąc administratora, przejdź do folderu skryptów sieci przy użyciu następującego polecenia:

            cd /etc/sysconfig/network-scripts
4. Lista plików powiązanych ifcfg przy użyciu następującego polecenia:

            ls ifcfg-*

    Powinien zostać wyświetlony *ifcfg eth0* jako jeden z tych plików.
5. Skopiuj plik *ifcfg eth0* i nadaj mu nazwę *ifcfg-eth0:0* przy użyciu następującego polecenia:

            cp ifcfg-eth0 ifcfg-eth0:0
6. Edytowanie *ifcfg-eth0:0* plik przy użyciu następującego polecenia:

            vi ifcfg-eth1
7. Zmienianie urządzenia do odpowiedniej nazwy w pliku. *eth0:0* w tym przypadku przy użyciu następującego polecenia:

            DEVICE=eth0:0
8. Zmienianie *adres_IP = YourPrivateIPAddress* linię, aby odzwierciedlały adres IP.
9. Zapisz plik przy użyciu następującego polecenia:

            :wq
10. Ponowne uruchomienie usług sieci i upewnij się, że zmiany są pomyślnie, uruchamiając następujące polecenia:

            /etc/init.d/network restart
            Ipconfig

    Powinien zostać wyświetlony adres IP, który został dodany, *eth0:0*na liście zwracane.

## <a name="add"></a>Dodać do istniejącego maszyn wirtualnych adresów IP

Wykonaj poniższe czynności, aby dodać do istniejącego NIC dodatkowych adresów IP:

1. Otwórz okno wiersza polecenia programu PowerShell i wykonaj pozostałe kroki w tej sekcji w ramach jednej sesji programu PowerShell. Jeśli nie masz jeszcze programu PowerShell zainstalowana i skonfigurowana, wykonaj czynności opisane w artykule [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .

2. Zmień nazwę NIC, aby dodać adresy IP i grupa zasobów i lokalizacji Sieciowej istnieje w "wartości" $Variables następujące:

        $NicName     = "RG1-VM1-NIC1"
        $RgName   = "RG1"
        $NicLocation = "westcentralus"

    Jeśli nie znasz nazwy NIC, który chcesz zmienić, wpisz następujące polecenia, następnie zmień wartości poprzedniego zmiennych:

        Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location

3. Utwórz zmienną i ustaw dla istniejących NIC, wpisując następujące polecenie:

        $nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName

4. Pobieranie Identyfikatora podsieci NIC jest połączony wykonując [Krok 3](#subnet) Tworzenie maszyn wirtualnych z wielu sekcji tego artykułu adresy IP.

5. Tworzenie konfiguracji adresów IP, który chcesz dodać do sieci, postępując zgodnie z instrukcjami w [kroku 5](#ipconfigs) Tworzenie maszyn wirtualnych z wielu sekcji tego artykułu adresy IP.

6. Zmienianie *$IPConfigName4* do nazwy konfiguracji adresów IP, który został utworzony w poprzednim kroku. Aby dodać konfigurację, wprowadź następujące polecenie:

        Add-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName4 -NetworkInterface $nic -Subnet $Subnet1

7. Aby ustawić NIC z konfiguracji IP, wpisz następujące polecenie:

        Set-AzureRmNetworkInterface -NetworkInterface $nic

8. Dodaj adresy IP, dodanych do NIC maszyn wirtualnych systemu operacyjnego zgodnie z instrukcjami w [kroku 9](#os) Tworzenie maszyn wirtualnych z wielu sekcji tego artykułu adresy IP.
