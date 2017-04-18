<properties
    pageTitle="Informacje o obrazach dla maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Informacje na temat używania obrazów w maszyn wirtualnych systemu Windows w Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="about-images-for-windows-virtual-machines"></a>Informacje o obrazach dla maszyn wirtualnych systemu Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-about-images](../../includes/virtual-machines-common-classic-about-images.md)]



## <a name="working-with-images"></a>Praca z obrazami

Modułu programu PowerShell usługi Azure umożliwia zarządzanie obrazów dostępnych do subskrypcji usługi Azure. W przypadku niektórych zadań obraz może zostać korzystać z portalu klasyczny Azure, ale wiersza polecenia oferuje więcej opcji.


-   **Pobierz wszystkie obrazy**:`Get-AzureVMImage`zwraca listę wszystkich obrazów, dostępne w bieżącej subskrypcji: obrazy, a także oferowane przez Azure lub partnerów. Oznacza to, że może zostać wyświetlony dużej listy. Następny przykładach pokazano, jak uzyskać krótszej listy.
-   **Uzyskiwanie rodziny obraz**:`Get-AzureVMImage | select ImageFamily` otrzymuje listę rodzin obrazu, wyświetlając ciągów **ImageFamily** właściwości.
-   **Pobierz wszystkie obrazy w określonym rodziny**:`Get-AzureVMImage | Where-Object {$_.ImageFamily -eq $family}`
-   **Znajdowanie obrazów maszyn wirtualnych**: `Get-AzureVMImage | where {(gm –InputObject $_ -Name DataDiskConfigurations) -ne $null} | Select -Property Label, ImageName` to działa, filtrując właściwość DataDiskConfiguration, która ma zastosowanie tylko do obrazów maszyn wirtualnych. W tym przykładzie filtruje także dane wyjściowe do tylko nazwę etykiety lub obrazu.
-   **Zapisz obraz uogólniony**:`Save-AzureVMImage –ServiceName "myServiceName" –Name "MyVMtoCapture" –OSState "Generalized" –ImageName "MyVmImage" –ImageLabel "This is my generalized image"`
-   **Zapisz obraz specjalnych**:`Save-AzureVMImage –ServiceName "mySvc2" –Name "MyVMToCapture2" –ImageName "myFirstVMImageSP" –OSState "Specialized" -Verbose`
>[Azure.Tip] Parametr OSState jest wymagany, jeśli chcesz utworzyć obraz maszyn wirtualnych zawiera dyski danych, a także dysku systemu operacyjnego. Jeśli nie zostanie użyty parametr, polecenia cmdlet tworzy obraz systemu operacyjnego. Wartość parametru wskazuje, czy obraz jest uogólniony lub specjalistycznego, w zależności od tego, czy przygotowano dysk systemu operacyjnego do ponownego użycia.
-   **Usuwanie obrazu**:`Remove-AzureVMImage –ImageName "MyOldVmImage"`


## <a name="next-steps"></a>Następne kroki

Możesz też [utworzyć za pomocą portalu klasyczny komputera systemu Windows](virtual-machines-windows-classic-tutorial.md)

