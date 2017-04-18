<properties
    pageTitle="Tworzenie kopii maszyn wirtualnych usługi Azure Linux | Microsoft Azure"
    description="Dowiedz się, jak utworzyć kopię komputera wirtualnych Azure Linux w modelu wdrożenia Menedżera zasobów"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>

# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure"></a>Tworzenie kopii maszyny wirtualnej Linux uruchomionych Azure


W tym artykule pokazano, jak utworzyć kopię komputera Azure wirtualnych (maszyn wirtualnych) systemem Linux przy użyciu modelu wdrożenia Menedżera zasobów. Najpierw można skopiować systemu operacyjnego i dyski danych do nowego kontenera, a następnie skonfiguruj zasobów sieciowych i tworzenie nowej maszyny wirtualnej.

Możesz również [przekazywać i tworzyć maszyn wirtualnych z obrazu dysku niestandardowe](virtual-machines-linux-upload-vhd.md).


## <a name="before-you-begin"></a>Przed rozpoczęciem

Upewnij się, spełnia następujące wymagania wstępne, przed rozpoczęciem czynności:

- Masz [polecenie Azure] (.. / xplat polecenie install.md) pobranego i zainstalowanego na komputerze. 

- Możesz również wymagane pewne informacje o swojej istniejącej maszyn wirtualnych Linux Azure:

| Informacje o źródle maszyn wirtualnych | Gdzie można go |
|------------|-----------------|
| Nazwa maszyn wirtualnych | `azure vm list` |
| Nazwa grupy zasobów | `azure vm list` |
| Lokalizacja | `azure vm list` |
| Nazwę konta magazynu | `azure storage account list -g <resourceGroup>` |
| Nazwa kontenera | `azure storage container list -a <sourcestorageaccountname>` |
| Nazwa źródłowego pliku wirtualnego dysku twardego maszyn wirtualnych | `azure storage blob list --container <containerName>` |



- Należy zmienić kilka opcji o swojej nowa maszyna wirtualna:   <br> -Nazwa kontenera   <br> Nazwa - maszyn wirtualnych   <br> Rozmiar pamięci Wirtualnej-   <br> Nazwa - vNet   <br> -Nazwa podsieci   <br> Nazwa - IP   <br> Nazwa - NIC
    

## <a name="login-and-set-your-subscription"></a>Logowanie i ustawianie subskrypcji

1. Zaloguj się do interfejsu wiersza polecenia.
        
        azure login

2. Upewnij się, że jesteś w trybie Menedżera zasobów.
    
        azure config mode arm

3. Ustaw poprawne subskrypcji. Możesz użyć "Lista konto azure" Aby wyświetlić wszystkie subskrypcji.

        azure account set <SubscriptionId>



## <a name="stop-the-vm"></a>Zatrzymywanie maszyn wirtualnych 

Zatrzymaj i cofnąć źródła maszyn wirtualnych. Możesz użyć "Lista azure maszyn wirtualnych" w celu uzyskania listy wszystkich maszyny wirtualne w subskrypcji i ich zasobów nazwy grup.
    
        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## <a name="copy-the-vhd"></a>Skopiuj wirtualny dysk twardy


Możesz skopiować wirtualny dysk twardy z magazynu źródła za pomocą pola Lokalizacja docelowa `azure storage blob copy start`. W tym przykładzie chwilę kopiowania wirtualny dysk twardy do tego samego konta miejsca do magazynowania, ale inny kontener.

Aby skopiować wirtualny dysk twardy do drugiego kontenera z tego samego konta miejsca do magazynowania, należy wpisać:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>
        

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Konfigurowanie sieci wirtualnej dla do nowych maszyn wirtualnych

Konfigurowanie wirtualnej sieci i NIC dla Twojej nowej maszyn wirtualnych. 

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## <a name="create-the-new-vm"></a>Tworzenie nowych maszyn wirtualnych 

Teraz można utworzyć maszyn wirtualnych usługi przekazane dysku wirtualnego [przy użyciu szablonu Menedżer zasobów](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) lub za pośrednictwem interfejsu wiersza polecenia, określając identyfikator URI dysku skopiowany, wpisując:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak za pomocą interfejsu wiersza polecenia Azure Zarządzanie nowego komputera wirtualnych, zobacz [poleceń interfejsu Azure wiersza polecenia dla Menedżera zasobów Azure](azure-cli-arm-commands.md).
