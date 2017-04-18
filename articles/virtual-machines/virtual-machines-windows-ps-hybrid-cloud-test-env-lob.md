<properties 
    pageTitle="Środowisku testowym aplikacji LOB | Microsoft Azure" 
    description="Dowiedz się, jak utworzyć wiersz oparte na sieci web aplikacji biznesowych w chmurze środowiska hybrydowego dla IT pro lub rozwoju testowania." 
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

# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Konfigurowanie aplikacją LOB oparte na sieci web w chmurze hybrydowych do celów testowych

W tym temacie przeprowadzi Cię przez proces tworzenia symulowany hybrydowego środowiska chmury testowania linii oparte na sieci web aplikacji branżowych (LOB) obsługiwany w programie Microsoft Azure. Oto konfiguracji wynikowe.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Ta konfiguracja składa się z:

- Sieć symulowany lokalnego obsługiwany w Azure (VNet testowe).
- Krzyżowe lokalnej wirtualną sieć obsługiwany w Azure (TestVNET).
- Połączenie VPN VNet do VNet.
- Oparte na sieci web server LOB, program SQL server i kontrolera domeny pomocniczą w wirtualnej sieci TestVNET.

Ta konfiguracja zapewnia podstawy i typowych punkt początkowy, z której można wykonywać następujące czynności:

- Można opracowywać i testowanie aplikacji FIRMOWYCH hostowana na Internetowe usługi informacyjne (IIS) z programu SQL Server 2014 wewnętrznej bazie danych platformy Azure.
- Testowanie obciążenie IT opartej na chmurze symulowany hybrydowego.

Istnieją trzy główne fazy do konfigurowania tego środowiska hybrydowego chmury test:

1.  Konfigurowanie symulowany hybrydowego środowiska chmury.
2.  Skonfiguruj komputer serwera SQL (SQL1).
3.  Konfigurowanie serwera LOB (LOB1).

Obciążenie wymaga subskrypcji usługi Azure. Jeśli masz subskrypcję MSDN lub Visual Studio, zobacz [kredytowej Azure miesięczne dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Na przykład produkcji aplikacji LOB obsługiwany w Azure Zobacz plan architektury **linią aplikacji biznesowych** [schematów](http://msdn.microsoft.com/dn630664)i diagramów architektury oprogramowania firmy Microsoft.

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Faza 1: Konfigurowanie symulowany hybrydowego środowiska chmury

Tworzenie [symulowany hybrydowego środowiska testowego chmury](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md). Ponieważ tej środowisku testowym nie wymaga obecności serwera APP1 podsieci Corpnet, można go zamknąć teraz.

Jest to bieżącej konfiguracji.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)
 
## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Faza 2: Skonfiguruj komputer serwera SQL (SQL1)

Z poziomu portalu Azure uruchom komputer DC2, w razie potrzeby.

Następnie należy utworzyć maszyny wirtualnej SQL1 za pomocą tych poleceń w wierszu polecenia programu PowerShell Azure na komputerze lokalnym. Przed uruchomieniem te polecenia, wypełniać wartościami zmiennych i Usuń < i > znaków.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Nawiązywanie połączenia z SQL1 za pomocą portalu Azure przy użyciu konta administratora lokalnego SQL1.

Następnie skonfiguruj reguły zapory systemu Windows umożliwia testowanie łączności podstawowe i ruch programu SQL Server. W wierszu polecenia środowiska Windows PowerShell uprawnień administratora na SQL1 uruchom następujące polecenia.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Polecenie ping powinno spowodować cztery pomyślnego odpowiedzi z adresu IP 192.168.0.4.

Następnie dodaj jako nowe kreski literę F: dysku dodatkowe dane SQL1.

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

Uruchom następujące polecenia w wierszu polecenia środowiska Windows PowerShell na SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

Następnie dołączyć SQL1 do domeny usługi Active Directory CORP systemu Windows Server za pomocą tych poleceń w wierszu programu Windows PowerShell na SQL1.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Użyj konta CORP\User1, po wyświetleniu monitu o podanie poświadczeń konta domeny dla polecenia **Dodaj komputer** .

Po ponownym uruchomieniu komputera, nawiązywanie połączenia z SQL1 *przy użyciu konta administratora lokalnego SQL1*za pomocą portalu Azure.

Następnie skonfiguruj SQL Server 2014 używać dysku F: dla nowych baz danych i uprawnień konta użytkownika.

1.  Na ekranie startowym wpisz **Zarządzanie serwerem SQL**, a następnie kliknij **SQL Server 2014 Management Studio**.
2.  W oknie dialogowym **połączenie z serwerem**kliknij przycisk **Połącz**.
3.  W okienku drzewa Eksploratora obiektów kliknij prawym przyciskiem myszy **SQL1**, a następnie kliknij polecenie **Właściwości**.
4.  W oknie **Właściwości serwera** kliknij przycisk **Ustawienia bazy danych**.
5.  Zlokalizuj **bazę danych domyślnej lokalizacji** i ustawić te wartości: 
    - W przypadku **danych**wpisz ścieżkę **f:\Data**.
    - W przypadku **dziennika**wpisz ścieżkę **f:\Log**.
    - **Wykonywanie kopii zapasowych**wpisz ścieżkę **f:\Backup**.
    - Uwaga: Tylko nowych baz danych za pomocą następujących lokalizacji.
6.  Kliknij przycisk **OK** , aby zamknąć okno.
7.  W okienku drzewa **Object Explorer** Otwórz **zabezpieczeń**.
8.  Kliknij prawym przyciskiem myszy **logowania** , a następnie kliknij przycisk **Nowe logowanie**.
9.  W polu **Nazwa**wpisz **CORP\User1**.
10. Na stronie **Role serwerów** kliknij pozycję **grupy**, a następnie kliknij **przycisk OK**.
11. Zamknij program Microsoft SQL Server Management Studio.

Jest to bieżącej konfiguracji.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)
 
## <a name="phase-3-configure-the-lob-server-lob1"></a>Faza 3: Konfigurowanie serwera LOB (LOB1)

Najpierw należy utworzyć maszyny wirtualnej LOB1 za pomocą tych poleceń w wierszu polecenia programu PowerShell Azure na komputerze lokalnym.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Następnie nawiązywanie połączenia z LOB1 przy użyciu poświadczeń konta administratora lokalnego LOB1 za pomocą portalu Azure.

Następnie skonfiguruj reguły zapory systemu Windows w celu zezwalanie na ruch testowania łączności podstawowe. W wierszu polecenia środowiska Windows PowerShell uprawnień administratora na LOB1 uruchom następujące polecenia.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Polecenie ping powinno spowodować cztery pomyślnego odpowiedzi z adresu IP 192.168.0.4.

Następnie dołączyć LOB1 do domeny usługi Active Directory CORP za pomocą tych poleceń w wierszu programu Windows PowerShell.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Użyj konta CORP\User1, po wyświetleniu monitu o podanie poświadczeń konta domeny dla polecenia **Dodaj komputer** .

Po ponownym uruchomieniu komputera, nawiązywanie połączenia z LOB1 CORP\User1 konto i hasło za pomocą portalu Azure.

Następnie skonfiguruj LOB1 dla usług IIS i przetestować dostęp z komputera CLIENT1.

1.  Z poziomu Menedżera serwera kliknij pozycję **Dodaj role i funkcje**.
2.  Na stronie **przed rozpoczęciem** kliknij przycisk **Dalej**.
3.  Na stronie **Wybierz typ instalacji** kliknij przycisk **Dalej**.
4.  Na stronie **Wybierz serwer docelowy** kliknij przycisk **Dalej**.
5.  Na stronie **role serwerów** kliknij **Serwer sieci Web (IIS)** na liście **role**.
6.  Po wyświetleniu monitu kliknij pozycję **Dodaj funkcje**, a następnie kliknij przycisk **Dalej**.
7.  Na stronie **Wybierz funkcje** kliknij przycisk **Dalej**.
8.  Na stronie **Sieci Web Server (IIS)** kliknij przycisk **Dalej**.
9.  Na stronie **Wybieranie usługi ról** zaznacz lub wyczyść pola wyboru dla usług, których potrzebujesz do testowania aplikacji LOB, a następnie kliknij **Dalej**.
10. Na stronie **Potwierdzanie opcji instalacji** kliknij przycisk **Zainstaluj**.
11. Poczekaj, aż instalacji składników zostało ukończone, a następnie kliknij przycisk **Zamknij**.
12. Z portalu Azure połączyć się z komputerem KLIENT1 przy użyciu poświadczeń konta CORP\User1, a następnie uruchom program Internet Explorer.
13. Na pasku adresu wpisz **http://lob1/** , a następnie naciśnij klawisz ENTER. Zobacz domyślnej strony sieci web usług IIS 8.

Jest to bieżącej konfiguracji.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)
 
To środowisko jest teraz gotowy do wdrażania aplikacji sieci web na LOB1 i testowanie funkcji KLIENT1 podsieci Corpnet.

## <a name="next-step"></a>Następny krok

- Dodawanie nowej maszyny wirtualnej za pomocą [Azure portal](virtual-machines-windows-hero-tutorial.md).
