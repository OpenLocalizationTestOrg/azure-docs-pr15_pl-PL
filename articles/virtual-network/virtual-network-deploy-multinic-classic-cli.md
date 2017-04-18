<properties
   pageTitle="Wdrażanie maszyny wirtualne NIC wielu przy użyciu interfejsu wiersza polecenia Azure w modelu Klasyczny wdrożenia | Microsoft Azure"
   description="Dowiedz się, jak wdrożyć maszyny wirtualne NIC wielu przy użyciu interfejsu wiersza polecenia Azure w modelu Klasyczny wdrażania"
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

#<a name="deploy-multi-nic-vms-classic-using-the-azure-cli"></a>Wdrażanie wielu NIC maszyny wirtualne (klasyczny) za pomocą interfejsu wiersza polecenia Azure

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Można tworzyć wirtualnych maszyn platformy Azure i dołączyć wiele interfejsów sieciowych (NIC) do każdego z pośrednictwem usługi SMS. Kart włączyć odstęp typów ruchu przez nic. Na przykład jeden NIC może komunikować się z Internetem, podczas drugiego komunikuje się tylko z zasobów wewnętrznych nie jest połączony z Internetem. Osobne ruch sieciowy wielu kart jest wymagana dla wielu urządzeń wirtualnych sieci, takich jak dostarczanie aplikacji i rozwiązań optymalizacji WAN.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](virtual-network-deploy-multinic-arm-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Obecnie nie mogą mieć maszyny wirtualne z jednym NIC i maszyny wirtualne z kart w tym samym usługi w chmurze. Dlatego konieczne do wprowadzenia serwerem wewnętrznym usługi w chmurze innego niż i wszystkie inne składniki w scenariuszu. Poniższe czynności za pomocą usługi w chmurze o nazwie *IaaSStory* głównym zasoby i *Wewnętrznej bazy danych IaaSStory* dla serwerów wewnętrznej.

## <a name="prerequisites"></a>Wymagania wstępne

Przed wdrożeniem serwerem wewnętrznym, należy wdrożyć usług w chmurze głównym z niezbędne zasoby dla tego scenariusza. Co najmniej potrzebne do utworzenia wirtualnej sieci z podsieć dla wewnętrznej bazy danych. Odwiedź stronę [Tworzenie wirtualnej sieci przy użyciu interfejsu wiersza polecenia Azure](virtual-networks-create-vnet-classic-cli.md) , aby dowiedzieć się, jak wdrożyć wirtualnej sieci.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Wdrażanie wewnętrznej maszyny wirtualne

Maszyny wirtualne wewnętrznej bazy danych zależy od utworzenia zasobów wymienionych poniżej.

- **Konto miejsca do magazynowania dla dyski danych**. Lepszą wydajność dyski danych na serwerze bazy danych będzie używany stanie stałym technologii dysk (SSD), która wymaga konta programu premium miejsca do magazynowania. Upewnij się, lokalizację Azure Wdroż obsługuje magazynowania premium.
- **Nic**. Każdy maszyn wirtualnych ma dwa nic, jedną dla dostępu do bazy danych, a drugi do zarządzania.
- **Ustawianie dostępności**. Wszystkie serwery mają bazy danych zostanie dodany do jednej dostępność ustawić, aby upewnić się, że co najmniej jeden z pośrednictwem SMS jest uruchomiony i uruchamiania podczas konserwacji.

### <a name="step-1---start-your-script"></a>Krok 1 — Uruchom skrypt

Możesz pobrać skryptu pełny imprezie używane [w tym miejscu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Wykonaj poniższe czynności, aby zmienić skrypt do pracy w środowisku.

1. Zmienianie wartości poniżej zmiennych na podstawie do istniejącej grupy zasobów wdrożona powyżej w [wymagania wstępne](#Prerequisites).

        location="useast2"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"

2. Zmienianie wartości poniżej zmiennych na podstawie wartości, których chcesz używać dla wdrożenia wewnętrznej bazy danych.

        backendCSName="IaaSStory-Backend"
        prmStorageAccountName="iaasstoryprmstorage"
        image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskPrefix="db"
        dataDiskName="datadisk"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Krok 2 — Tworzenie niezbędnych zasobów dla swojego maszyny wirtualne

1. Tworzenie nowej usługi w chmurze dla wszystkich maszyny wirtualne wewnętrznej bazy danych. Zwróć uwagę, stosowania `$backendCSName` zmiennych dla grupy Nazwa zasobu i `$location` dla regionu Azure.

        azure service create --serviceName $backendCSName \
            --location $location

2. Utwórz konto premium miejsca do magazynowania dla dysków systemu operacyjnego i danych ma być używany przez Twój maszyny wirtualne.

        azure storage account create $prmStorageAccountName \
            --location $location \
            --type PLRS

### <a name="step-3---create-vms-with-multiple-nics"></a>Krok 3 — Tworzenie maszyny wirtualne przy użyciu kart

1. Rozpoczynanie pętli, aby utworzyć wiele maszyny wirtualne na podstawie `numberOfVMs` zmiennych.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Dla każdego Głosowa Określ nazwę oraz adres IP każdego z dwóch sieciowe.

            nic1Name=$vmNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x

            nic2Name=$vmNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x

4. Tworzenie maszyn wirtualnych. Zwróć uwagę, użycie `--nic-config` parametru zawierający listę wszystkich nic z nazwą, podsieci i adres IP.

            azure vm create $backendCSName $image $username $password \
                --connect $backendCSName \
                --vm-name $vmNamePrefix$suffixNumber \
                --vm-size $vmSize \
                --availability-set $avSetName \
                --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
                --virtual-network-name $vnetName \
                --subnet-names $backendSubnetName \
                --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::

5. Dla każdego Głosowa Utwórz dwa dyski danych.

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
        done

### <a name="step-4---run-the-script"></a>Krok 4 — uruchamianie skryptu

Teraz, gdy pobranego i zmienić skrypt, w zależności od potrzeb, uruchom skrypt w celu utworzenia wewnętrzna maszyny wirtualne bazy danych przy użyciu kart.

1. Zapisz skrypt i uruchom go z usługi terminalowe **urodzinową** . Zostanie wyświetlona początkowej wynik, jak pokazano poniżej.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Po kilku minutach zakończy się wykonywanie, a zobaczysz pozostałą część wynik, jak pokazano poniżej.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
