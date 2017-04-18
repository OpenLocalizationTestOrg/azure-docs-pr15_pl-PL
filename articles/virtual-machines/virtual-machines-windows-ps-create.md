<properties
    pageTitle="Tworzenie maszyn wirtualnych Azure przy użyciu programu PowerShell | Microsoft Azure"
    description="Przy użyciu programu PowerShell Azure i Menedżera zasobów Azure łatwo tworzyć nowe maszyny z systemem Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Tworzenie maszyn wirtualnych systemu Windows za pomocą Menedżera zasobów i programu PowerShell

W tym artykule pokazano, jak szybko utworzyć Azure Virtual Machine z systemem Windows Server i zasoby niezbędne przy użyciu programu PowerShell i [Menedżera zasobów](../azure-resource-manager/resource-group-overview.md) . 

Wszystkie kroki opisane w tym artykule są wymagane do utworzenia maszyny wirtualnej i wykonywania czynności należy trwa około 30 minut. Zamień wartości parametrów przykład na polecenia nazwy, które miały znaczenie dla środowiska.

## <a name="step-1-install-azure-powershell"></a>Krok 1: Instalowanie programu PowerShell Azure

Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla informacji na temat instalowania najnowszej wersji programu PowerShell Azure, wybierając subskrypcji i logowanie się do swojego konta.
        
## <a name="step-2-create-a-resource-group"></a>Krok 2: Tworzenie grupy zasobów

Wszystkie zasoby musi znajdować się w grupie zasobów, dlatego umożliwia tworzenie tego najpierw.  

1. Pobierz listę dostępnych lokalizacji, w którym można utworzyć zasoby.

    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Ustawianie lokalizacji dla zasobów. To polecenie ustawia lokalizację **centralus**.

    ```powershell
    $location = "centralus"
    ```
    
3. Tworzenie grupy zasobów. To polecenie tworzy grupa zasobów o nazwie **myResourceGroup** w lokalizacji, w której możesz ustawić.

    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```
    
## <a name="step-3-create-a-storage-account"></a>Krok 3: Utwórz konto miejsca do magazynowania

[Konto miejsca do magazynowania](../storage/storage-introduction.md) jest potrzebny do przechowywania wirtualny dysk twardy, który jest używany przez komputer wirtualnych, które są tworzone. Nazwy kont miejsca do magazynowania musi być pomiędzy 3 a 24 znaków i może zawierać liczby i wyłącznie małymi literami.

1. Testowanie nazwę konta magazynu unikatowość. To polecenie testuje nazwę **myStorageAccount**.

    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
    
    Jeśli to polecenie zwraca **wartość PRAWDA**, nazwę proponowana jest unikatowa w Azure. 
    
2. Teraz Utwórz konto miejsca do magazynowania.
    
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```
    
## <a name="step-4-create-a-virtual-network"></a>Krok 4: Tworzenie wirtualnych sieci

Wszystkie maszyn wirtualnych są częścią [wirtualnej sieci](../virtual-network/virtual-networks-overview.md).

1. Utwórz podsieć dla wirtualnej sieci. To polecenie tworzy podsieci o nazwie **mySubnet** z prefiks adresu 10.0.0.0/24.
        
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
    
2. Teraz Utwórz wirtualnej sieci. To polecenie tworzy wirtualną sieć o nazwie **myVnet** za pomocą podsieci, która została utworzona i prefiks adresu **10.0.0.0/16**.

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```
        
## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Krok 5: Tworzenie IP adresu i sieci interfejs publiczny

Aby umożliwić komunikację z maszyny wirtualnej w wirtualnej sieci, konieczne jest [publiczny adres IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) i karty sieciowej.

1. Tworzenie publiczny adres IP. To polecenie tworzy publiczny adres IP o nazwie **myPublicIp** z metodą alokacji **dynamiczne**.
 
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Dynamic
    ```
        
2. Tworzenie interfejsu sieciowego. To polecenie tworzy karty sieciowej o nazwie **myNIC**.

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```
       
## <a name="step-6-create-a-virtual-machine"></a>Krok 6: Tworzenie maszyny wirtualnej

Teraz, gdy masz wszystkie elementy w miejscu, jest czas potrzebny do utworzenia maszyny wirtualnej.

1. Uruchom to polecenie, aby ustawić nazwę konta administratora i hasło dla maszyny wirtualnej.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
    Hasło musi wynosić 12-123 znaków i znak co najmniej jeden małe litery, wielkie litery o jeden znak, jedną liczbę i jeden znak specjalny. 
        
2. Tworzenie obiektu konfiguracji maszyny wirtualnej. To polecenie tworzy obiekt konfiguracji o nazwie **myVmConfig** , która definiuje nazwę maszyn wirtualnych i rozmiar maszyn wirtualnych.

    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
     
    Aby uzyskać listę dostępnych rozmiarów dla maszyny wirtualnej, zobacz [rozmiarów maszyn wirtualnych w Azure](virtual-machines-windows-sizes.md) .
    
3. Konfigurowanie ustawień maszyn wirtualnych systemu operacyjnego. To polecenie ustawia nazwa komputera, typ systemu operacyjnego i poświadczenia konta maszyn wirtualnych.

    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```
    
4. Definiowanie obrazu umożliwia inicjować obsługę maszyn wirtualnych. To polecenie definiuje obraz systemu Windows Server, aby użyć maszyn wirtualnych. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
        
    Aby uzyskać więcej informacji o wybranie obrazów do użycia zobacz [Nawigacja i wybierz pozycję obrazy maszyn wirtualnych systemu Windows Azure za pomocą programu PowerShell lub interfejsu wiersza polecenia](virtual-machines-windows-cli-ps-findimage.md).
        
5. Dodaj interfejs sieciowy, który został utworzony w konfiguracji.

    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
        
6. Definiowanie nazwy i lokalizacji na dysku twardym maszyn wirtualnych. Plik wirtualny dysk twardy jest przechowywany w kontenerze. To polecenie tworzy dysk w kontenerze o nazwie **vhds/WindowsVMosDisk.vhd** na koncie miejsca do magazynowania, utworzone przez Ciebie.

    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
        
7. Dodawanie informacji o dysku system operacyjny konfiguracji maszyn wirtualnych. Zamień wartość **$diskName** nazwę dysku systemu operacyjnego. Utwórz zmienną i Dodaj informacje dotyczące dysku konfiguracji.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
        
8. Na koniec Utwórz maszyny wirtualnej.

    ```
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```
                                  
## <a name="next-steps"></a>Następne kroki

- W przypadku problemów z wdrożenia następnym krokiem jest przeglądać [wdrożeń grupa zasobów Rozwiązywanie problemów z Azure portal](../resource-manager-troubleshoot-deployments-portal.md)
- Dowiedz się, jak zarządzać maszyny wirtualnej utworzone przez Ciebie, przeglądając artykuł [Zarządzanie maszyn wirtualnych za pomocą Menedżera zasobów Azure i programu PowerShell](virtual-machines-windows-ps-manage.md).
- Wykorzystać przy użyciu szablonu do utworzenia maszyny wirtualnej, korzystając z informacji w sekcji [Tworzenie maszyny wirtualnej systemu Windows za pomocą szablonu Menedżera zasobów](virtual-machines-windows-ps-template.md)
