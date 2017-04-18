<properties
   pageTitle="Zarządzanie maszyn wirtualnych przy użyciu programu PowerShell Azure | Microsoft Azure"
   description="Dowiedz się więcej poleceń, których można używać do automatyzowania zadań w środowisku maszyn wirtualnych systemu zarządzania."
   services="virtual-machines-windows"
   documentationCenter="windows"
   authors="singhkays"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

   <tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/12/2016"
   ms.author="kasing"/>

# <a name="manage-your-virtual-machines-by-using-azure-powershell"></a>Zarządzanie maszyn wirtualnych przy użyciu programu PowerShell Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Wiele zadań każdego dnia, aby zarządzać pośrednictwem usługi SMS można zautomatyzować przy użyciu poleceń cmdlet środowiska PowerShell Azure. Ten artykuł zawiera polecenia przykład prostsze zadań i łącza do artykułów, w których są wyświetlane polecenia służące do bardziej złożone zadania.

>[AZURE.NOTE] Jeśli jeszcze nie zainstalowano i jeszcze skonfigurowane Azure programu PowerShell, instrukcje można znaleźć w artykule [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

## <a name="how-to-use-the-example-commands"></a>Jak używać polecenia przykład
Należy zastąpić fragment tekstu w polu polecenia tekst, który jest odpowiedni dla środowiska. < i > symbole wskazują tekst należy zamienić. Gdy zastąpić tekst, Usuń symbole, ale zostawić znaków cudzysłowu w tym miejscu.

## <a name="get-a-vm"></a>Uzyskiwanie maszyny
To jest podstawowe zadania, które będą używane. Go używać do uzyskiwanie informacji na temat maszyny, wykonywania zadań w maszyny lub uzyskaj wyjściowy do przechowywania w zmiennej.

Aby uzyskać informacje o maszyn wirtualnych, uruchomić to polecenie, zamieniając wszystko w cudzysłowie, łącznie z < i > znaków:

     Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

Aby zapisać wynik w zmiennej $vm, uruchom polecenie:

    $vm = Get-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="log-on-to-a-windows-based-vm"></a>Zaloguj się do maszyn wirtualnych systemu Windows

Uruchom następujące polecenia:

>[AZURE.NOTE] Nazwa usługi maszyn wirtualnych i chmura możesz przejść z polecenia **Get-AzureVM** .
>
    $svcName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $localPath = "<drive and folder location to store the downloaded RDP file, example: c:\temp >"
    $localFile = $localPath + "\" + $vmname + ".rdp"
    Get-AzureRemoteDesktopFile -ServiceName $svcName -Name $vmName -LocalPath $localFile -Launch

## <a name="stop-a-vm"></a>Zatrzymywanie maszyny

Uruchom następujące polecenie:

    Stop-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

>[AZURE.IMPORTANT]Ten parametr służy do zachowania wirtualnych IP (VIP) z pakietu usługi w chmurze, w przypadku, gdy jest ostatnim maszyn wirtualnych w tej usłudze w chmurze. <br><br> Jeśli parametr StayProvisioned, możesz nadal zostanie wystawiona dla maszyn wirtualnych.

## <a name="start-a-vm"></a>Rozpoczynanie maszyny

Uruchom następujące polecenie:

    Start-AzureVM -ServiceName "<cloud service name>" -Name "<virtual machine name>"

## <a name="attach-a-data-disk"></a>Dołączanie dysku danych
To zadanie wymaga kilku kroków. Najpierw użyj *** Dodaj-AzureDataDisk *** polecenia cmdlet dodać dysk do obiektu $vm. Następnie możesz użyć polecenia cmdlet **Aktualizacji AzureVM** Aby zaktualizować konfigurację maszyn wirtualnych.

Musisz również zdecydować, czy Dołączanie nowego dysku lub jedną, która zawiera dane. Dla nowego dysku polecenie tworzy plik VHD i dołącza go.

Aby dołączyć nowego dysku, uruchom następujące polecenie:

    Add-AzureDataDisk -CreateNew -DiskSizeInGB 128 -DiskLabel "<main>" -LUN <0> -VM $vm | Update-AzureVM

Aby dołączyć istniejący dysk danych, uruchom następujące polecenie:

    Add-AzureDataDisk -Import -DiskName "<MyExistingDisk>" -LUN <0> | Update-AzureVM

Aby dołączyć dyski danych z istniejącego pliku VHD w magazynie obiektów blob, uruchom następujące polecenie:

    Add-AzureDataDisk -ImportFrom -MediaLocation `
              "<https://mystorage.blob.core.windows.net/mycontainer/MyExistingDisk.vhd>" `
              -DiskLabel "<main>" -LUN <0> |
              Update-AzureVM

## <a name="create-a-windows-based-vm"></a>Tworzenie maszyn wirtualnych systemu Windows

Aby utworzyć nowe maszyny wirtualnej systemu Windows w Azure, należy skorzystać z instrukcji w [Za pomocą programu PowerShell Azure do tworzenia i wstępnie skonfigurować maszyn wirtualnych systemu Windows](virtual-machines-windows-classic-create-powershell.md). Ten temat czynności podczas tworzenia Azure programu PowerShell polecenie zestaw tworzy maszyny opartych na systemie Windows, który można wstępnie zdefiniowany:

- Członkostwo domeny usługi Active Directory.
- W przypadku dysków dodatkowych.
- Jako członek istniejącego równoważenia obciążenia należy ustawić.
- Przy użyciu statycznego adresu IP.
