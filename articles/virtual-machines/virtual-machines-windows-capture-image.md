<properties
    pageTitle="Przechwycić obraz maszyn wirtualnych z uogólniony Azure maszyn wirtualnych | Microsoft Azure"
    description="Dowiedz się, jak przechwycić obraz maszyn wirtualnych z uogólniony maszyn wirtualnych Azure utworzone w modelu wdrożenia Menedżera zasobów"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>

# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Jak przechwycić obraz maszyn wirtualnych z uogólniony maszyny Azure


W tym artykule pokazano, jak utworzyć obraz uogólniony maszyny Azure za pomocą programu PowerShell Azure. Obraz można następnie umożliwia utworzenie innego maszyn wirtualnych. Obraz zawiera dysku systemu operacyjnego i dyski danych, które zostały dołączone do maszyny wirtualnej. Obraz nie zawiera zasobów wirtualnej sieci, więc należy skonfigurować te zasoby, podczas tworzenia nowych maszyn wirtualnych. 


## <a name="prerequisites"></a>Wymagania wstępne

- Użytkownik musi być już [uogólniony maszyn wirtualnych](virtual-machines-windows-generalize-vhd.md). Uogólnianie maszyny powoduje usunięcie wszystkich osobistych informacji o koncie, między innymi i przygotowuje komputer może być używany jako obraz.

- Musisz mieć wersję programu PowerShell Azure 1.0.x lub nowszy zainstalowany. Jeśli jeszcze nie zainstalowano programu PowerShell, przeczytaj, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla kroki instalacji.


## <a name="log-in-to-azure-powershell"></a>Zaloguj się do programu PowerShell Azure

1. Otwórz Azure programu PowerShell i zaloguj się do konta Azure.

    ```powershell
    Login-AzureRmAccount
    ```

    Otwiera okno podręczne dla Ciebie o podanie poświadczeń konta Azure.

2. Uzyskaj subskrypcji identyfikatorów dla subskrypcji dostępne.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Ustawianie poprawne subskrypcji przy użyciu identyfikatora subskrypcji.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Cofnąć maszyn wirtualnych i Ustaw stan uogólniony       

1. Cofnij przydzielanie zasobów maszyn wirtualnych.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```

    *Stan* maszyn wirtualnych w portalu Azure zmienia się z **zatrzymania** **zatrzymania (alokację)**.

2. Ustaw stan maszyny wirtualnej **Generalized**. 

    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```

3. Sprawdź stan maszyn wirtualnych. W sekcji **OSState uogólniony** maszyn wirtualnych powinny mieć **DisplayStatus** ustawiono **uogólniony maszyn wirtualnych**.  

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Tworzenie obrazu 

1. Kopiowanie obrazu maszyn wirtualnych do kontenera magazynu docelowego przy użyciu tego polecenia. Obraz jest tworzony na tym samym koncie miejsca do magazynowania, co oryginalny maszyna wirtualna. `-Path` Parametru zapisuje kopię lokalnie szablon JSON. `-DestinationContainerName` Parametr jest nazwą kontenera, który ma pełnić obrazów. Jeśli kontener nie istnieje, jest tworzony dla Ciebie.

    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```

    Adres URL obrazu można przejść z szablonu plików JSON. Przejdź do **zasobów** > **storageProfile** > **osDisk** > **obraz** > **identyfikatora uri** sekcji pełną ścieżkę obrazu. Adres URL obrazu wygląda: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
    
    Można również sprawdzić identyfikator URI w portalu. Obraz jest kopiowana do kontenera o nazwie **system** na swoim koncie miejsca do magazynowania. 


## <a name="next-steps"></a>Następne kroki

- Teraz można [utworzyć maszyn wirtualnych z obrazu](virtual-machines-windows-create-vm-generalized.md).

