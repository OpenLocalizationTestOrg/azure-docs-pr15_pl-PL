<properties
   pageTitle="Wdrażanie maszyny wirtualne NIC wielu przy użyciu programu PowerShell w modelu Klasyczny wdrożenia | Microsoft Azure"
   description="Dowiedz się, jak wdrożyć maszyny wirtualne NIC wielu przy użyciu programu PowerShell w modelu Klasyczny wdrażania"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-classic-using-powershell"></a>Wdrażanie wielu maszyny wirtualne NIC (klasyczny) przy użyciu programu PowerShell

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Można tworzyć wirtualnych maszyn platformy Azure i dołączyć wiele interfejsów sieciowych (NIC) do każdego z pośrednictwem usługi SMS. Kart włączyć odstęp typów ruchu przez nic. Na przykład jeden NIC może komunikować się z Internetem, podczas drugiego komunikuje się tylko z zasobów wewnętrznych nie jest połączony z Internetem. Osobne ruch sieciowy wielu kart jest wymagana dla wielu urządzeń wirtualnych sieci, takich jak dostarczanie aplikacji i rozwiązań optymalizacji WAN.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](virtual-network-deploy-multinic-arm-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Obecnie nie mogą mieć maszyny wirtualne z jednym NIC i maszyny wirtualne z kart w tym samym usługi w chmurze. Dlatego konieczne do wprowadzenia serwerem wewnętrznym usługi w chmurze innego niż i wszystkie inne składniki w scenariuszu. Poniższe czynności za pomocą usługi w chmurze o nazwie *IaaSStory* głównym zasoby i *Wewnętrznej bazy danych IaaSStory* dla serwerów wewnętrznej.

## <a name="prerequisites"></a>Wymagania wstępne

Przed wdrożeniem serwerem wewnętrznym, należy wdrożyć usług w chmurze głównym z niezbędne zasoby dla tego scenariusza. Co najmniej potrzebne do utworzenia wirtualnej sieci z podsieć dla wewnętrznej bazy danych. Odwiedź stronę [Tworzenie wirtualnej sieci przy użyciu programu PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md) , aby dowiedzieć się, jak wdrożyć wirtualnej sieci.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Wdrażanie wewnętrznej maszyny wirtualne

Maszyny wirtualne wewnętrznej bazy danych zależy od utworzenia zasobów wymienionych poniżej.

- **Podsieci wewnętrznej bazy danych**. Serwer bazy danych będą częścią osobną podsieć, aby oddzielnie różnic ruch. Skrypt poniżej oczekuje danej podsieci istnieć w vnet o nazwie *WTestVnet*.
- **Konto miejsca do magazynowania dla dyski danych**. Lepszą wydajność dyski danych na serwerze bazy danych będzie używany stanie stałym technologii dysk (SSD), która wymaga konta programu premium miejsca do magazynowania. Upewnij się, lokalizację Azure Wdroż obsługuje magazynowania premium.
- **Ustawianie dostępności**. Wszystkie serwery mają bazy danych zostanie dodany do jednej dostępność ustawić, aby upewnić się, że co najmniej jeden z pośrednictwem SMS jest uruchomiony i uruchamiania podczas konserwacji.

### <a name="step-1---start-your-script"></a>Krok 1 — Uruchom skrypt

Możesz pobrać pełny skrypt programu PowerShell używane [w tym miejscu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Wykonaj poniższe czynności, aby zmienić skrypt do pracy w środowisku.

1. Zmienianie wartości na podstawie do istniejącej grupy zasobów wdrożona powyżej w [wymagania wstępne dotyczące](#Prerequisites)poniżej zmiennych.

        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"

2. Zmienianie wartości poniżej zmiennych na podstawie wartości, których chcesz używać dla wdrożenia wewnętrznej bazy danych.

        $backendCSName         = "IaaSStory-Backend"
        $prmStorageAccountName = "iaasstoryprmstorage"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $diskSize              = 127
        $vmNamePrefix          = "DB"
        $dataDiskSuffix        = "datadisk"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Krok 2 — Tworzenie niezbędnych zasobów dla swojego maszyny wirtualne

Potrzebne do utworzenia nowej usługi w chmurze, a konto miejsca do magazynowania dla dyski danych dla wszystkich maszyny wirtualne. Należy określić dla maszyny wirtualne obrazu i lokalnego konta administratora. Aby utworzyć te zasoby, wykonaj następujące czynności.

1. Tworzenie nowej usługi w chmurze.

        New-AzureService -ServiceName $backendCSName -Location $location

2. Tworzenie nowego konta miejsca do magazynowania premium.

        New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
            -Location $location `
            -Type Premium_LRS

3. Ustawianie konta miejsca do magazynowania, utworzony w poprzednim przykładzie jako konta miejsca do magazynowania dla subskrypcji.

        $subscription = Get-AzureSubscription `
            | where {$_.IsCurrent -eq $true}  
        Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
            -CurrentStorageAccountName $prmStorageAccountName

4. Wybierz obraz dla maszyn wirtualnych.

        $image = Get-AzureVMImage `
            | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
            | sort PublishedDate -Descending `
            | select -ExpandProperty ImageName -First 1

5. Ustaw poświadczenia konta lokalnego administratora.

        $cred = Get-Credential -Message "Enter username and password for local admin account"

### <a name="step-3---create-vms"></a>Krok 3 — Tworzenie maszyny wirtualne

Należy możliwie jak najwięcej maszyny wirtualne, a tworzenie niezbędne nic i maszyny wirtualne w pętli za pomocą pętli. Aby utworzyć nic i maszyny wirtualne, wykonaj następujące czynności.

1. Rozpoczynanie `for` pętli Powtarzaj polecenia służące do tworzenia maszyn wirtualnych i dwóch nic tyle razy, stosownie do potrzeb, na podstawie wartości z `$numberOfVMs` zmiennej.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Tworzenie `VMConfig` obiektu określającą obrazu, rozmiar i dostępność ustawieniem maszyn wirtualnych.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureVMConfig -Name $vmName `
                            -ImageName $image `
                            -InstanceSize $vmSize `
                            -AvailabilitySetName $avSetName  

3. Inicjowanie obsługi maszyn wirtualnych jako maszyn wirtualnych systemu Windows.

            Add-AzureProvisioningConfig -VM $vmConfig -Windows `
                -AdminUsername $cred.UserName `
                -Password $cred.Password

4. Ustawianie domyślnego NIC i przypisz mu statyczny adres IP.

            Set-AzureSubnet -SubnetNames $backendSubnetName -VM $vmConfig
            Set-AzureStaticVNetIP -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig

5. Dodawanie drugiej karty Sieciowej dla każdego maszyn wirtualnych.

            Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
                -SubnetName $backendSubnetName `
                -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
                -VM $vmConfig

6. Tworzenie na dyskach danych dla każdego maszyn wirtualnych.

            $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk1Name `
                -LUN 0       

            $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk2Name `
                -LUN 1

7. Tworzenie poszczególnych maszyn wirtualnych i zakończenia pętli.

            New-AzureVM -VM $vmConfig `
                -ServiceName $backendCSName `
                -Location $location `
                -VNetName $vnetName
        }

### <a name="step-4---run-the-script"></a>Krok 4 — uruchamianie skryptu

Teraz, gdy pobranego i zmienić skrypt, w zależności od potrzeb runt on skrypt tworzenie wewnętrzna maszyny wirtualne bazy danych przy użyciu kart.

1. Zapisz skrypt i uruchom go z poziomu wiersza polecenia **programu PowerShell** lub **Środowiska PowerShell ISE**. Zostanie wyświetlona początkowej wynik, jak pokazano poniżej.

        OperationDescription    OperationId                          OperationStatus
        --------------------    -----------                          ---------------
        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      

        WARNING: No deployment found in service: 'IaaSStory-Backend'.

2. Wypełnij informacje potrzebne w wierszu poświadczeń, a następnie kliknij **przycisk OK**. Będą wyświetlane poniżej dane.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
