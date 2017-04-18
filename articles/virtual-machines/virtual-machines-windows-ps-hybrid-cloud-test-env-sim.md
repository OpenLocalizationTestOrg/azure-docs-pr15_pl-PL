<properties 
    pageTitle="Symulowany hybrydowego środowiska testowego chmury | Microsoft Azure" 
    description="Tworzenie symulowany hybrydowego środowiska chmury dla IT pro lub testowania rozwoju, przy użyciu dwóch Azure wirtualnych sieci i połączenie VNet do VNet." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Konfigurowanie środowiska chmury symulowany hybrydowego do celów testowych

W tym artykule przeprowadzi Cię przez proces tworzenia symulowany hybrydowego środowiska chmury z Microsoft Azure za pomocą dwóch Azure wirtualnych sieci. Oto konfiguracji wynikowe.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Symuluje środowisku produkcyjnym chmury hybrydowe i składa się z:

- Sieć symulowany i uproszczone lokalnego obsługiwany w Azure wirtualną sieć (testowe wirtualna sieć).
- Symulowany lokalnej infrastrukturze wirtualnej sieci obsługiwany w Azure (TestVNET).
- Połączenie VNet do VNet między dwoma wirtualnych sieci.
- Kontroler pomocniczy domeny w wirtualnej sieci TestVNET.

Zapewnia podstawy i rozpoczynając typowych punktów z której można wykonywać następujące czynności:

- Opracowanie i przetestowanie aplikacji w środowisku chmury symulowany hybrydowego.
- Tworzenie konfiguracji testów komputerów, niektóre w wirtualnej sieci testowe i niektóre w sieci wirtualnych TestVNET celu zasymulowania hybrydowych opartej na chmurze IT obciążenia.

Istnieją cztery główne fazy do konfigurowania tego środowiska hybrydowego chmury test:

1.  Konfigurowanie sieci wirtualnej testowe.
2.  Tworzenie wirtualnych sieci lokalnej krzyżowe.
3.  Utwórz połączenie VPN VNet do VNet.
4.  Konfigurowanie DC2. 

Ta konfiguracja wymaga subskrypcji usługi Azure. Jeśli masz subskrypcję MSDN lub Visual Studio, zobacz [kredytowej Azure miesięczne dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

>[AZURE.NOTE] Maszyn wirtualnych i bram wirtualną sieć platformy Azure spowodować trwających koszcie, gdy działają. Koszt ten jest wystawiona przed usługi MSDN lub płatnych subskrypcji. Brama Azure VPN jest zaimplementowana jako zestaw dwóch Azure maszyn wirtualnych. Aby zminimalizować koszty, środowisku testowym wykonywania i do potrzeb badania i demonstracji tak szybko, jak to możliwe.

## <a name="phase-1-configure-the-testlab-virtual-network"></a>Faza 1: Konfigurowanie wirtualną sieć testowe

Aby skonfigurować komputery DC1, APP1 i KLIENT1 w Azure wirtualną sieć o nazwie testowe, skorzystaj z instrukcji w temacie [testowym konfiguracji bazy](https://technet.microsoft.com/library/mt771177.aspx) . 

Następnie uruchom wiersz Azure programu PowerShell.

> [AZURE.NOTE] Poniższe polecenie ustawia Użyj programu PowerShell Azure 1.0 lub nowszy.

Zaloguj się do swojego konta.

    Login-AzureRMAccount

Uzyskaj nazwę subskrypcji przy użyciu następującego polecenia.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Ustawianie Azure subskrypcji. Za pomocą tej samej subskrypcji, używany do tworzenia konfiguracji podstawowej na etapie 1. Zamień wszystko w obrębie ofert, łącznie z < i > znaki, z prawidłową nazwę.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Następnie dodaj podsieć bramy wirtualną siecią testowe konfiguracji podstawowej, która będzie używana do obsługi Azure bramy.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Następnie należy zażądać publiczny adres IP, aby przypisać do bramy testowe wirtualnej sieci.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Następnie należy utworzyć Centrum.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Należy pamiętać, że nowych bram może potrwać 20 minut lub więcej, aby utworzyć.

Portal Azure na komputerze lokalnym Połącz się DC1 przy użyciu poświadczeń CORP\User1. Aby skonfigurować domenę CORP, tak aby komputerów i użytkowników dla uwierzytelniania za pomocą ich lokalnego kontrolera domeny, Uruchom te polecenia w wierszu polecenia środowiska Windows PowerShell uprawnień administratora na komputerze DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Jest to bieżącej konfiguracji.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)
 
## <a name="phase-2-create-the-testvnet-virtual-network"></a>Faza 2: Tworzenie wirtualnych sieci TestVNET

Najpierw należy utworzyć wirtualnej sieci TestVNET i chronić go z grupy zabezpieczeń sieci.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Następnie należy zażądać publiczny adres IP przydzielenia bramy TestVNET wirtualną sieć i utworzyć Centrum.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Jest to bieżącej konfiguracji.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)
 
##<a name="phase-3-create-the-vnet-to-vnet-connection"></a>Faza 3: Tworzenie połączenia VNet do VNet

Najpierw uzyskać losowe, kryptograficznie silne, 32-znakowy klucz wstępny z administratorem sieci lub zabezpieczeń. Ewentualnie skorzystaj z informacji na [Tworzenie ciągu losową dla klucza wstępnego IPsec](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) uzyskanie klucza wstępnego.

Następnie połączenie VPN VNet do VNet, co może zająć trochę czasu, aby zakończyć tworzenie za pomocą tych poleceń.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Po upływie kilku minut można nawiązać połączenia.

Jest to bieżącej konfiguracji.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)
 
## <a name="phase-4-configure-dc2"></a>Faza 4: Konfigurowanie DC2

Najpierw należy utworzyć maszyny wirtualnej dla DC2. Uruchom następujące polecenia w wierszu polecenia programu PowerShell Azure na komputerze lokalnym.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Następnie należy połączyć się nowa maszyna wirtualna DC2 z Azure portal.

Następnie skonfiguruj reguły zapory systemu Windows w celu zezwalanie na ruch do testowania łączności podstawowe. W wierszu polecenia środowiska Windows PowerShell uprawnień administratora na DC2 uruchom następujące polecenia.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

Polecenie ping powinno spowodować cztery pomyślnego odpowiedzi z adresu IP 10.0.0.4. To jest test ruchu za pośrednictwem połączenia VNet do VNet.

Następnie dodaj jako nowe kreski literę F: dysku dodatkowe dane DC2.

1.  W okienku po lewej stronie w Menedżerze serwera kliknij **plików i usługi magazynu**, a następnie kliknij **dysków**.
2.  W okienku Zawartość w grupie **dysków** kliknij **dysk 2** (z **partycją** ustawiono **Nieznany**).
3.  Kliknij pozycję **zadania**, a następnie kliknij pozycję **Nowy**.
4.  W polach przed możesz rozpocząć strona kreatora nowego głośność, kliknij przycisk **Dalej**.
5.  Na stronie Wybieranie serwera i dysku kliknij **dysk 2**, a następnie kliknij przycisk **Dalej**. Po wyświetleniu monitu kliknij **przycisk OK**.
6.  Na stronie Określanie rozmiar strony głośność kliknij przycisk **Dalej**.
7.  Na stronie przypisywanie na stronę litery lub folderu dysk kliknij przycisk **Dalej**.
8.  Na stronie Ustawienia systemu wybierz plik kliknij przycisk **Dalej**.
9.  Na stronie Potwierdzanie opcji kliknij przycisk **Utwórz**.
10. Po zakończeniu kliknij przycisk **Zamknij**.

Następnie skonfiguruj DC2 jako kontrolera domeny replice dla domeny corp.contoso.com. Uruchom te polecenia w wierszu polecenia środowiska Windows PowerShell na DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Należy zauważyć, że jest wyświetlany monit o podanie hasło CORP\User1 i hasła trybu przywracania usług katalogów (DSRM) i ponowne DC2.

Teraz, gdy wirtualnej sieci TestVNET ma własny serwer DNS (DC2), musisz skonfigurować wirtualnej sieci TestVNET do używania tego serwera DNS.

1.  W lewym okienku Azure portal kliknij ikonę wirtualnych sieci, a następnie kliknij **TestVNET**.
2.  Na karcie **Ustawienia** kliknij pozycję **serwery DNS**.
3.  W polu **podstawowy serwer DNS**wpisz **192.168.0.4** , aby zamienić 10.0.0.4.
4.  Kliknij przycisk **Zapisz**.

Jest to bieżącej konfiguracji. 

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)
 
Środowiska chmury symulowany hybrydowego jest teraz gotowy do testowania.

## <a name="next-step"></a>Następny krok

- Konfigurowanie [wiersza aplikacji biznesowych oparte na sieci web,](virtual-machines-windows-ps-hybrid-cloud-test-env-lob.md) w tym środowisku.
