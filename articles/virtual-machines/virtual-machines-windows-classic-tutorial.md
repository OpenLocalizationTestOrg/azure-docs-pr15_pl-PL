<properties
    pageTitle="Tworzenie maszyn wirtualnych w klasycznym portal | Microsoft Azure"
    description="Tworzenie maszyny wirtualnej systemu Windows w portalu klasyczny Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="cynthn"/>

# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Tworzenie wirtualnych komputera z systemem Windows w portalu klasyczny Azure

> [AZURE.SELECTOR]
- [Portal Azure klasyczny](virtual-machines-windows-classic-tutorial.md)
- [Programu PowerShell: Wdrożenie klasycznego](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te czynności przy użyciu modelu wdrożenia Menedżera zasobów](virtual-machines-windows-hero-tutorial.md) za pomocą **Nowy portal Azure**. 

Ten samouczek pokazano, jak utworzyć Azure maszyny wirtualnej (maszyn wirtualnych) z systemem Windows w portalu klasyczny Azure. Jako przykład użyjemy obraz systemu Windows Server, ale jest tylko jeden z wielu obrazów, który oferuje Azure. Należy zauważyć, że opcje obrazu zależą od Twojej subskrypcji. Na przykład obrazów pulpitu systemu Windows mogą być dostępne dla subskrybentów MSDN.

W tej sekcji pokazano, jak korzystać z opcji **Z galerii** w portalu klasyczny Azure tworzenie maszyny wirtualnej. Ta opcja umożliwia więcej opcji konfiguracji niż opcja **Szybkiego tworzenia** . Na przykład jeśli chcesz dołączyć do maszyny wirtualnej do wirtualnej sieci, musisz skorzystać z opcji **Z galerii** .

Można także tworzyć maszyny wirtualne przy użyciu [własnych obrazów](virtual-machines-windows-classic-createupload-vhd.md). Aby dowiedzieć się o to i inne metody, zobacz [różne sposoby tworzenia maszyny wirtualnej systemu Windows](virtual-machines-windows-creation-choices.md).



## <a name="video-walkthrough"></a>Instruktaż wideo

Oto skorzystać z tego samouczka.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>Tworzenia maszyny wirtualnej

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [utworzyć maszyny przy użyciu modelu wdrożenia Menedżera zasobów](virtual-machines-windows-hero-tutorial.md) w nowy portal Azure. 

- Zaloguj się do maszyny wirtualnej. Aby uzyskać instrukcje zobacz [Zaloguj się do maszyny wirtualnej z systemem Windows Server](virtual-machines-windows-classic-connect-logon.md).

- Dołącz dyskiem do przechowywania danych. Można dołączyć zarówno puste dyski i które zawierają dane. Aby uzyskać instrukcje zobacz [Dołączanie dysku danych utworzona za pomocą modelu Klasyczny wdrażania maszyn wirtualnych systemu Windows](virtual-machines-windows-classic-attach-disk.md).
