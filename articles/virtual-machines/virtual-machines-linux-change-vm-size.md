<properties
   pageTitle="Sposób zmieniania rozmiaru maszyny Linux | Microsoft Azure"
   description="Jak rozbudowy lub skali maszyny wirtualnej Linux, zmieniając rozmiar pamięci Wirtualnej."
   services="virtual-machines-linux"
   documentationCenter="na"
   authors="mikewasson"
   manager="timlt"
   editor=""
   tags=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="mikewasson"/>


# <a name="how-to-resize-a-linux-vm"></a>Sposób zmieniania rozmiaru maszyny Linux

## <a name="overview"></a>Omówienie 

Po postanowień maszyny wirtualnej (maszyn wirtualnych), można skalować maszyn wirtualnych w górę lub w dół, zmieniając [rozmiar pamięci Wirtualnej][vm-sizes]. W niektórych przypadkach należy najpierw cofnąć maszyn wirtualnych. To może się zdarzyć, jeśli nowy rozmiar nie jest dostępna w klastrze sprzęt obsługujący maszyn wirtualnych.

W tym artykule przedstawiono sposób zmieniania rozmiaru maszyny Linux za pomocą [Interfejsu wiersza polecenia Azure][azure-cli].

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrożenia.


## <a name="resize-a-linux-vm"></a>Zmienianie rozmiaru maszyny Linux 

Aby zmienić rozmiar maszyny, wykonaj następujące czynności.

1. Uruchom następujące polecenie, polecenie. To polecenie wyświetla rozmiary maszyn wirtualnych, które są dostępne w klastrze sprzętu miejsce, w którym znajduje się maszyn wirtualnych.

    ```
    azure vm sizes -g <resource-group> --vm-name <vm-name>
    ```

2. Jeśli wymieniono Zmień rozmiar okna, uruchom następujące polecenie, aby zmienić rozmiar maszyn wirtualnych.

    ```
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    ```

    Maszyn wirtualnych uruchomi się ponownie w trakcie tego procesu. Po uruchomieniu do istniejącego systemu operacyjnego i dyski danych będzie zostać ponownie zamapowane. Dowolnego elementu na dysku tymczasowym zostaną utracone.

    Używanie `--enable-boot-diagnostics` opcja umożliwia [uruchamiania diagnostyki][boot-diagnostics], aby rejestrować błędy związane z uruchamiania.

3. W przeciwnym razie jeśli Zmień rozmiar okna nie są widoczne, uruchom następujące polecenia, aby zwolnić Głosowa, zmienić jego rozmiar i ponownie uruchom maszyn wirtualnych.

    ```
    azure vm deallocate -g <resource-group> <vm-name>
    azure vm set -g <resource-group> --vm-size <new-vm-size> -n <vm-name>  
        --enable-boot-diagnostics --boot-diagnostics-storage-uri
        https://<storage-account-name>.blob.core.windows.net/ 
    azure vm start -g <resource-group> <vm-name>
    ```

   > [AZURE.WARNING] Cofanie przydziału maszyn wirtualnych udostępnia również dynamiczne adresy IP przypisane do maszyn wirtualnych. Nie wpływa na dyskach systemu operacyjnego i danych.
   
## <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkowe skalowalność uruchamianie wielu wystąpień maszyn wirtualnych i skalowania. Aby uzyskać więcej informacji, zobacz [Automatyczne skalowanie maszyn Linux w zestawie maszyn wirtualnych skali][scale-set]. 

<!-- links -->
   
[azure-cli]: ../xplat-cli-install.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[scale-set]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md 
[vm-sizes]: virtual-machines-linux-sizes.md