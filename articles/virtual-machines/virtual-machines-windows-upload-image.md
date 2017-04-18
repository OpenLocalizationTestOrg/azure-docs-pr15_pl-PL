<properties
    pageTitle="Przekazywanie Windows wirtualnego dysku twardego dla Menedżera zasobów | Microsoft Azure"
    description="Dowiedz się przekazać Windows maszyny wirtualnej wirtualnego dysku twardego ze źródeł lokalnych Azure, przy użyciu modelu wdrożenia Menedżera zasobów. Możesz przekazać dysku z albo uogólniony ani specjalistyczne maszyn wirtualnych."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>

# <a name="upload-a-windows-vhd-from-an-on-premises-vm-to-azure"></a>Przekazywanie wirtualnego dysku twardego z lokalnego maszyn wirtualnych Azure systemu Windows 


W tym artykule pokazano, jak tworzyć i przekazywać Windows wirtualnego dysku twardego (VHD) może być używany podczas tworzenia maszyn wirtualnych Azure. Możesz przekazać wirtualnego dysku twardego z uogólniony maszyn wirtualnych lub specjalistyczne maszyn wirtualnych. 

Aby uzyskać więcej informacji o dyskach i wirtualnych dysków twardych platformy Azure zobacz [informacje o dysków i wirtualnych dysków twardych dla maszyn wirtualnych](virtual-machines-linux-about-disks-vhds.md).


## <a name="prepare-the-vm"></a>Przygotowywanie maszyn wirtualnych 

Możesz przekazać zarówno ogólnych i specjalnych wirtualnych dysków twardych Azure. Każdego typu wymaga przygotowywanie maszyn wirtualnych przed rozpoczęciem.

- **Uogólniony wirtualnego dysku twardego** — uniwersalny wirtualnego dysku twardego miał wszystkie informacje osobiste konto usunąć za pomocą narzędzia Sysprep. Jeśli zamierzasz użyć wirtualnego dysku twardego jako obraz, aby utworzyć nowy maszyny wirtualne z, należy:
    - [Przygotowywanie wirtualnego dysku twardego systemu Windows do przekazania Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). 
    - [Generalize maszyny wirtualnej za pomocą programu Sysprep](virtual-machines-windows-generalize-vhd.md). 

- **Specjalistyczne wirtualnego dysku twardego** — specjalistyczne wirtualny dysk twardy przechowuje konta użytkowników, aplikacji i innych danych o stanie ze swojego oryginalnego maszyn wirtualnych. Jeśli zamierzasz użyć wirtualnego dysku twardego jako-można utworzyć nowy maszyn wirtualnych, upewnij się, wykonywane są następujące czynności. 
    - [Przygotowywanie wirtualnego dysku twardego systemu Windows do przekazania Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md). **Nie** generalize maszyn wirtualnych, za pomocą programu Sysprep.
    - Usuń gościa, za pomocą narzędzia wirtualizacji i czynników, które są zainstalowane na maszyn wirtualnych (to znaczy VMware narzędzia).
    - Upewnij się, że skonfigurowano maszyn wirtualnych aby uwzględniał jego adres IP i ustawienia DNS za pośrednictwem DHCP. Dzięki temu, że serwer uzyskuje adres IP w VNet podczas jego uruchamiania. 

## <a name="log-in-to-azure"></a>Zaloguj się do Azure

Jeśli nie masz jeszcze programu PowerShell wersji 1.4 lub powyżej zainstalowanych, przeczytaj [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

1. Otwórz Azure programu PowerShell i zaloguj się do konta Azure. Otwiera okno podręczne dla Ciebie o podanie poświadczeń konta Azure.

    ```powershell
    Login-AzureRmAccount
    ```


2. Uzyskaj subskrypcji identyfikatorów dla subskrypcji dostępne.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Ustawianie poprawne subskrypcji przy użyciu identyfikatora subskrypcji. Zamienianie `<subscriptionID>` o identyfikatorze poprawne subskrypcji.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Uzyskiwanie konta miejsca do magazynowania

Wymagane jest konto miejsca do magazynowania w Azure do przechowywania przekazanego obrazu maszyn wirtualnych. Możesz używać istniejącego konta miejsca do magazynowania lub utworzyć nową. 

Aby wyświetlić konta dostępnego magazynu, wpisz:

```powershell
Get-AzureRmStorageAccount
```

Jeśli chcesz używać istniejącego konta miejsca do magazynowania, przejdź do sekcji [Przekaż obraz maszyn wirtualnych](#upload-the-vm-vhd-to-your-storage-account) .

Jeśli musisz utworzyć konto miejsca do magazynowania, wykonaj następujące czynności:

1. Potrzebujesz nazwę grupy zasobów, gdzie należy utworzone konto miejsca do magazynowania. Aby dowiedzieć się, wszystkie grupy zasobów, które są w ramach subskrypcji, należy wpisać:

    ```powershell
    Get-AzureRmResourceGroup
    ```

Aby utworzyć grupę zasobów o nazwie **myResourceGroup** w regionie **Zachód Stanów Zjednoczonych** , należy wpisać:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Utwórz konto miejsca do magazynowania o nazwie **mystorageaccount** w tej grupie zasobów za pomocą polecenia cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) :

    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" -SkuName "Standard_LRS" -Kind "Storage"
    ```
            
    Prawidłowe wartości - SkuName są następujące:

    - **Standard_LRS** - lokalnie zbędne miejsca do magazynowania. 
    - **Standard_ZRS** — magazyn zbędne strefy.
    - **Standard_GRS** - Geo zbędne przestrzeni dyskowej. 
    - **Standard_RAGRS** - magazynowania zbędne geo dostęp do odczytu. 
    - **Premium_LRS** - Premium lokalnie zbędne przestrzeni dyskowej. 



## <a name="upload-the-vhd-to-your-storage-account"></a>Przekazywanie wirtualny dysk twardy do swojego konta miejsca do magazynowania

Aby przekazać obraz do kontenera na koncie miejsca do magazynowania, należy użyć polecenia cmdlet [AzureRmVhd Dodaj](https://msdn.microsoft.com/library/mt603554.aspx) . W tym przykładzie przekazywania pliku **myVHD.vhd** z `"C:\Users\Public\Documents\Virtual hard disks\"` konta o nazwie **mystorageaccount** w grupie zasobów **myResourceGroup** miejsca do magazynowania. Plik zostanie umieszczony w kontenerze o nazwie **mycontainer** i nową nazwę pliku zostaną **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Jeśli kończy się pomyślnie, zostanie wyświetlony odpowiedź, która wygląda tak:

```
  C:\> Add-AzureRmVhd -ResourceGroupName myResourceGroup -Destination https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
  MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
  MD5 hash calculation is completed.
  Elapsed time for the operation: 00:03:35
  Creating new page blob of size 53687091712...
  Elapsed time for upload: 01:12:49

  LocalFilePath           DestinationUri
  -------------           --------------
  C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

W zależności od połączenie sieciowe i rozmiar pliku wirtualnego dysku twardego to polecenie może potrwać do wykonania


## <a name="next-steps"></a>Następne kroki

- [Tworzenie maszyn wirtualnych w Azure z uniwersalny wirtualnego dysku twardego](virtual-machines-windows-create-vm-generalized.md)
- [Tworzenie maszyn wirtualnych w Azure z specjalistyczne wirtualnego dysku twardego](virtual-machines-windows-create-vm-specialized.md) dołączając je jako dysk systemu operacyjnego, podczas tworzenia nowych maszyn wirtualnych.


