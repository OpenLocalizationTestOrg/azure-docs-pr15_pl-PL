<properties
   pageTitle="Potrzeby stanu konfiguracji Azure omówienie | Microsoft Azure"
   description="Omówienie przy użyciu procedury Microsoft Azure rozszerzenia obsługi programu PowerShell potrzeby stan konfiguracji. W tym wymagania wstępne, architektura, poleceń cmdlet."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Wprowadzenie do obsługi rozszerzenia Azure potrzeby stan konfiguracji #

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Agent maszyn wirtualnych Azure i skojarzonych rozszerzeń są częścią usługi infrastruktury programu Microsoft Azure. Rozszerzenia maszyn wirtualnych są składników oprogramowania, które rozszerzenia funkcji maszyn wirtualnych i uprościć różnych operacji zarządzania maszyn wirtualnych. Na przykład z rozszerzeniem VMAccess można zresetować hasło administratora lub rozszerzenia niestandardowego skryptu może służyć do wykonywania skryptu na maszyn wirtualnych.

W tym artykule wprowadzono rozszerzenia programu PowerShell potrzeby stan konfiguracji (DSC) maszyny wirtualne Azure jako część zestawu SDK programu PowerShell Azure. Nowe polecenia cmdlet umożliwia przekazywanie i stosowanie konfiguracji DSC programu PowerShell na maszyn wirtualnych Azure włączone z rozszerzeniem DSC programu PowerShell. DSC programu PowerShell rozszerzenia wywołuje DSC programu PowerShell wprowadzenie Odebrano konfigurację DSC na maszyn wirtualnych. Ta funkcja jest również dostępne za pośrednictwem portalu Azure.

## <a name="prerequisites"></a>Wymagania wstępne ##
**Komputer lokalny** Aby wykonać czynność związaną z rozszerzeniem maszyn wirtualnych Azure, należy za pomocą portalu Azure lub SDK programu PowerShell Azure. 

**Agent gościa** Maszyn wirtualnych Azure, skonfigurowanego przez konfigurację DSC musi być OS obsługującego albo Windows Management Framework (WMF) 4.0 i 5.0. Pełna lista obsługiwanych wersji systemu operacyjnego można znaleźć w [Historii wersji rozszerzenia DSC](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Terminy i pojęcia ##
Ten przewodnik przyjęto założenie, znajomości następujące pojęcia:

Konfiguracja — dokumentacja konfiguracji DSC. 

Węzeł — cel konfiguracji DSC. W tym dokumencie "węzeł" zawsze dotyczy maszyn wirtualnych Azure.

Konfiguracja danych — .psd1 plik zawierający środowiska dane dotyczące konfiguracji

## <a name="architectural-overview"></a>Omówienie architektury ##

Rozszerzenie Azure DSC używa framework agenta maszyn wirtualnych Azure przeprowadzania, przyjąć i raportowanie informacji dotyczących konfiguracji DSC uruchomionych maszyny wirtualne Azure. Rozszerzenie DSC oczekuje plik zip zawierający co najmniej dokumentu konfiguracji i ustawianie parametrów podana za pośrednictwem SDK programu PowerShell Azure lub Azure portal.

Rozszerzenie nazywa się po raz pierwszy, jest uruchomiony proces instalacji. Ten proces instaluje wersję struktury zarządzania Windows (WMF), z pomocą następujących reguł:

1. System operacyjny maszyn wirtualnych Azure w przypadku systemu Windows Server 2016, jest przyjmowana żadnej akcji. Windows Server 2016 ma już najnowszą wersję programu PowerShell zainstalowany.
2. Jeśli `wmfVersion` określono właściwości, ta wersja WMF jest zainstalowany, o ile nie jest zgodna z maszyn wirtualnych systemu operacyjnego.
3. Jeśli nie `wmfVersion` określono właściwość, jest zainstalowana najnowsza wersja dotyczy wersja WMF.

Instalacja WMF wymaga ponownego uruchomienia. Po ponownym uruchomieniu, rozszerzenia do pobrania pliku zip, określonego w `modulesUrl` właściwości. W przypadku tej lokalizacji w magazynie obiektów blob platformy Azure, token skojarzeń zabezpieczeń można określić w `sasToken` właściwość dostępu do tego pliku. Po pobraniu zip i rozpakowane, funkcja konfiguracji zdefiniowana w `configurationFunction` jest Uruchom, aby utworzyć plik MOF. Następnie uruchamia rozszerzenie `Start-DscConfiguration -Force` w wygenerowanym pliku MOF. Rozszerzenie przechwytuje dane wyjściowe i zapisuje ponownie do kanału stanu Azure. Od tej chwili NAJMN.wsp.WIEL DSC obsługuje monitorowania i korekta w zwykły sposób. 

## <a name="powershell-cmdlets"></a>Polecenia cmdlet programu PowerShell ##

Polecenia cmdlet programu PowerShell umożliwia ARM lub ASM pakowanie, publikowanie i monitorowanie DSC rozszerzenia wdrożenia. Następujące polecenia cmdlet na liście to moduły ASM, ale "Azure" można zastąpić "AzureRm", aby użyć modelu ARM. Na przykład `Publish-AzureVMDscConfiguration` używa ASM, gdzie `Publish-AzureRmVMDscConfiguration` używa ARM. 

`Publish-AzureVMDscConfiguration`przejście w pliku konfiguracji, wyszukuje zasoby zależne DSC i tworzy plik zip zawierający konfiguracji i zasoby DSC wymagane wprowadzenie konfiguracji. Można również utworzyć pakiet lokalnie, przy użyciu `-ConfigurationArchivePath` parametru. W przeciwnym razie publikuje plik zip z magazynem obiektów blob platformy Azure i zabezpieczyć przy użyciu tokenu skojarzeń zabezpieczeń.

Pliku zip utworzonego za pomocą tego polecenia cmdlet ma skryptu konfiguracji .ps1 w katalogu głównym folderu archiwum. Folder modułu umieszczane w folderze archiwum ma zasobów. 

`Set-AzureVMDscExtension`Wstawia go ustawienia wymagane przez rozszerzenia DSC programu PowerShell do obiektu konfiguracji maszyn wirtualnych, który można zastosować do maszyn wirtualnych Azure z `Update-AzureVM`.

`Get-AzureVMDscExtension`pobiera stan rozszerzenia DSC określonego maszyn wirtualnych. 

`Get-AzureVMDscExtensionStatus`pobiera stan konfiguracji DSC wprowadzone przez obsługi rozszerzenia DSC. Ta akcja mogą być wykonywane na jednym maszyn wirtualnych lub grupie maszyny wirtualne.

`Remove-AzureVMDscExtension`Usuwa obsługi rozszerzenia z danym komputerze wirtualnych. To polecenie cmdlet **Usuń konfiguracji,** odinstalować WMF lub zmienianie zastosowane ustawienia komputera wirtualnych. Powoduje tylko usunięcie obsługi rozszerzenia. 

**Kluczowych różnic w poleceń cmdlet ASM i rąk**

- Polecenia cmdlet ARM są synchroniczne. Polecenia cmdlet ASM są asynchroniczne.
- ResourceGroupName, VMName ArchiveStorageAccountName, wersji i lokalizacji są wszystkie nowe wymagane parametry.
- ArchiveResourceGroupName jest nowe opcjonalne parametru ARM. Ten parametr można określić, kiedy konta magazynu należy do grupy zasobów inny niż ten, której jest tworzona maszyny wirtualnej.
- ConfigurationArchive jest nazywany ArchiveBlobName w imieniu
- ContainerName nosi nazwę ArchiveContainerName w imieniu
- StorageEndpointSuffix jest nazywany ArchiveStorageEndpointSuffix w imieniu
- Przełączanie AutoUpdate został dodany do ARM, aby włączyć automatyczne aktualizowanie obsługi rozszerzenia do najnowszej wersji jako i jest dostępna. Zauważ, ten parametr może spowodować ponowne uruchomienie na maszyn wirtualnych po zwolnieniu nowa wersja WMF. 


## <a name="azure-portal-functionality"></a>Funkcje portal Azure ##
Przejdź do klasycznej maszyn wirtualnych. W obszarze Ustawienia -> Ogólne kliknij pozycję "Rozszerzenia". Nowe okienko zostanie utworzona. Kliknij pozycję "Dodaj" i wybierz DSC programu PowerShell.

Portal usługi musi danych wejściowych.
**Moduły konfiguracji lub skrypt**: to pole jest wymagane. Wymaga pliku .ps1 zawierającego skrypt konfiguracji lub pliku zip za pomocą skryptu konfiguracji .ps1 w katalogu, a wszystkie zasoby zależne w module folderami zip. Czy można utworzyć z `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` polecenia cmdlet zawarte w zestawie SDK programu PowerShell Azure. Plik zip jest przekazane do usługi Magazyn obiektów blob użytkownika zabezpieczone tokenu skojarzeń zabezpieczeń. 

**Plik PSD1 danych konfiguracji**: to pole jest opcjonalne. Jeśli konfiguracja wymaga pliku danych konfiguracji w .psd1, to pole umożliwia go zaznaczyć, a następnie przekaż go do użytkownika magazyn obiektów blob, gdy jest ono zabezpieczone tokenu skojarzeń zabezpieczeń. 
 
**Nazwa konfiguracji Module-Qualified**: .ps1 pliki mogą mieć wiele funkcji konfiguracji. Wprowadź nazwę skryptu .ps1 konfiguracji, a po nim "\' i nazwę funkcji konfiguracji. Na przykład aby skrypt .ps1 ma nazwę "configuration.ps1", a konfiguracja jest "IisInstall", należy wprowadzić:`configuration.ps1\IisInstall`

**Argumenty konfiguracji**: Jeśli funkcja konfiguracji przyjmuje argumenty, wprowadź je w tym miejscu w formacie `argumentName1=value1,argumentName2=value2`. Należy zauważyć, że ten format jest inny format niż sposób konfiguracji argumenty są akceptowane za pomocą poleceń cmdlet środowiska PowerShell lub szablony Menedżera zasobów. 

## <a name="getting-started"></a>Wprowadzenie ##

Rozszerzenie Azure DSC trwa w dokumentach konfiguracji DSC i enacts je na maszyny wirtualne Azure. Prosty przykład konfiguracji są uwzględniane. Zapisz go lokalnie jako "IisInstall.ps1":

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

Następujące czynności skrypt IisInstall.ps1 umieszczony na określony maszyn wirtualnych, wykonywanie konfiguracji i ponownie utworzenie raportu o stanie.
 
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "demo.ps1.zip" -StorageContext $storageContext -ConfigurationName "runScript" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```

## <a name="logging"></a>Rejestrowanie ##

Dzienniki znajdują się w:

C:\WindowsAzure\Logs\Plugins\Microsoft.PowerShell.DSC\[numer wersji]

## <a name="next-steps"></a>Następne kroki ##

Aby uzyskać więcej informacji na temat programu PowerShell DSC, [odwiedź witrynę Centrum dokumentacji programu PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

Sprawdź [Menedżera zasobów Azure szablon z rozszerzeniem DSC](virtual-machines-windows-extensions-dsc-template.md
). 

Aby znaleźć dodatkowe funkcje można zarządzać przy użyciu programu PowerShell DSC, [Przejdź w galerii programu PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) dla większej ilości zasobów DSC.

Dla szczegółowych informacji na temat przekazywania poufnych parametrów w konfiguracji zobacz [Zarządzanie poświadczenia bezpieczne z DSC obsługi rozszerzenia](virtual-machines-windows-extensions-dsc-credentials.md).
