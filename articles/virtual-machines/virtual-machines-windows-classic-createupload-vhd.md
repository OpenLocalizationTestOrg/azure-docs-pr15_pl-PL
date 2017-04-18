<properties
    pageTitle="Tworzenie i Przekaż obraz maszyn wirtualnych przy użyciu programu Powershell | Microsoft Azure"
    description="Dowiedz się, jak tworzyć i Przekaż obraz uogólniony Windows Server (wirtualnego dysku twardego) przy użyciu modelu Klasyczny wdrożenia i Azure programu Powershell."
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

# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Tworzenie i przekazywanie wirtualnego dysku twardego serwera systemu Windows Azure

W tym artykule pokazano, jak przekazać własny obraz uogólniony maszyn wirtualnych jako wirtualnego dysku twardego (wirtualnego dysku twardego), aby służy do tworzenia maszyn wirtualnych. Aby uzyskać więcej informacji o dyskach i wirtualnych dysków twardych platformy Microsoft Azure zobacz [temat dyski i wirtualnych dysków twardych dla maszyn wirtualnych](virtual-machines-linux-about-disks-vhds.md).


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. Można również [przekazać](virtual-machines-windows-upload-image.md) maszyny wirtualnej przy użyciu modelu Menedżera zasobów. 

## <a name="prerequisites"></a>Wymagania wstępne

W tym artykule założono, że masz:

- **Subskrypcja Azure** — Jeśli nie masz, możesz [otworzyć konto Azure bezpłatnie](/pricing/free-trial/?WT.mc_id=A261C142F).

- **[Microsoft Azure programu PowerShell](../powershell-install-configure.md)** — masz moduł Microsoft Azure programu PowerShell zainstalowaniu i skonfigurowaniu, aby korzystać z subskrypcji. 

- **A . Plik wirtualnego dysku twardego** - obsługiwane systemu operacyjnego przechowywane w pliku VHD i dołączone do maszyny wirtualnej Windows. Sprawdź, role serwerów uruchomionych wirtualny dysk twardy są obsługiwane przez to narzędzie. Aby uzyskać więcej informacji zobacz [Obsługa narzędzia Sysprep dla ról serwera](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.IMPORTANT] VHDX format nie jest obsługiwany w programie Microsoft Azure. Za pomocą Menedżera funkcji Hyper-V lub polecenia [cmdlet wirtualnego dysku twardego Konwertuj](http://technet.microsoft.com/library/hh848454.aspx)Format wirtualnego dysku twardego można konwertować na dysk. Aby uzyskać szczegółowe informacje Zobacz ten [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Krok 1: Przygotowywanie wirtualnego dysku twardego 

Przed przekazaniem wirtualny dysk twardy Azure, musi być uogólniony przy użyciu narzędzia Sysprep. To przygotowuje wirtualny dysk twardy może być używany jako obraz. Aby uzyskać szczegółowe informacje na temat narzędzia Sysprep, zobacz [sposobu użycia Sysprep: wprowadzenie](http://technet.microsoft.com/library/bb457073.aspx). Wykonywanie kopii zapasowej maszyn wirtualnych przed uruchomieniem narzędzia Sysprep.

Z zainstalowanym system operacyjny do maszyny wirtualnej wykonaj poniższe czynności:

1. Zaloguj się do systemu operacyjnego.

2. Otwórz okno wiersza polecenia jako administrator. Zmienianie katalogu **%windir%\system32\sysprep**, a następnie uruchom `sysprep.exe`.

    ![Otwórz okno wiersza polecenia](./media/virtual-machines-windows-classic-createupload-vhd/sysprep_commandprompt.png)

3.  Zostanie wyświetlone okno dialogowe **Narzędzie przygotowywania systemu** .

    ![Uruchom narzędzie Sysprep](./media/virtual-machines-windows-classic-createupload-vhd/sysprepgeneral.png)

4.  **Narzędzie przygotowywania systemu**wybierz **Wprowadź System z pola obsługi (pierwszego uruchomienia)** , a następnie upewnij się, że jest zaznaczona **Generalize** .

5.  W oknie dialogowym **Opcje zamykania**wybierz pozycję **Zamknij**.

6.  Kliknij **przycisk OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Krok 2: Tworzenie konta miejsca do magazynowania i kontenera

Potrzebne konto miejsca do magazynowania w Azure, więc musisz lokalizację, aby przekazać plik VHD. W tym kroku pokazano, jak utworzyć konto, lub uzyskaj informacje, które są potrzebne z istniejącego konta. Zamienianie zmiennych w &lsaquo; nawiasów &rsaquo; na własne informacje.

1. Logowanie

        Add-AzureAccount

1. Ustawianie Azure subskrypcji.

        Select-AzureSubscription -SubscriptionName <SubscriptionName> 

2. Tworzenie nowego konta miejsca do magazynowania. Nazwę konta magazynu powinny być unikatowe, 3-24 znaków. Nazwa może zawierać dowolną kombinację liter i cyfr. Należy określić lokalizację, na przykład "Wschód NAMI"
        
        New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>

3. Nowe konto miejsca do magazynowania Ustaw jako domyślne.
        
        Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>

4. Tworzenie nowego kontenera.

        New-AzureStorageContainer -Name <ContainerName> -Permission Off

 

## <a name="step-3-upload-the-vhd-file"></a>Krok 3: Przekaż plik VHD

[Dodaj AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) umożliwia przekazywanie wirtualnego dysku twardego.

W oknie programu PowerShell Azure użyto w poprzednim kroku, wpisz następujące polecenie i zamienić zmiennych w &lsaquo; nawiasów &rsaquo; na własne informacje.

        Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>


## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Krok 4: Dodawanie obrazu do listy niestandardowych obrazów

Aby dodać obraz do listy niestandardowej obrazów, należy użyć polecenia cmdlet [AzureVMImage Dodaj](https://msdn.microsoft.com/library/mt589167.aspx) .

        Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"


## <a name="next-steps"></a>Następne kroki

Możesz teraz [utworzyć niestandardowe maszyn wirtualnych](virtual-machines-windows-classic-createportal.md) przy użyciu przekazanego obrazu.

