<properties
    pageTitle="Omówienie skrypt wdrożenia projektu grupa zasobów Azure | Microsoft Azure"
    description="W tym artykule opisano, jak działa skrypt programu PowerShell w programie project wdrożenia Azure grupa zasobów."
    services="visual-studio-online"
    documentationCenter="na"
    authors="tfitzmac"
    manager="timlt"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/26/2016"
    ms.author="tomfitz" />

# <a name="overview-of-the-azure-resource-group-project-deployment-script"></a>Omówienie skrypt wdrożenia projektu Azure grupa zasobów

Grupa zasobów Azure wdrażania projektów pomocy etapu i wdrażanie plików i innych artefaktów Azure. Po utworzeniu projektu wdrażania Azure Menedżera zasobów w programie Visual Studio skrypt programu PowerShell o nazwie **Rozmieszczanie AzureResourceGroup.ps1** jest dodawany do projektu. Ten temat zawiera szczegółowe informacje o działanie tego skryptu i jak ją wykonać w obrębie i poza Visual Studio.

## <a name="what-does-the-script-do"></a>Do czego służy skrypt?

Skrypt AzureResourceGroup.ps1 rozmieszczanie wykonuje dwie czynności, ważnych wdrożenie przepływu pracy.

- Przekazywanie plików ani artefakty potrzebne do wdrożenia szablonu
- Wdrażanie szablonu

Pierwsza część skrypt przekazywanie plików i artefaktów wdrożenia i ostatniego polecenia cmdlet skryptu faktycznie wdrożyć szablonu. Na przykład jeśli maszyny wirtualnej musi być skonfigurowany za pomocą skryptu, skrypt wdrożenia najpierw bezpieczne spowoduje przekazanie skryptu konfiguracji do konta usługi Azure miejsca do magazynowania. Dzięki temu będzie dostępny do Menedżera zasobów Azure konfigurowania maszyny wirtualnej podczas inicjowania obsługi administracyjnej.

Ponieważ nie wszystkie wdrożeń szablon wymagają dodatkowych artefaktów, które należy przekazać, jest obliczane parametrem przełącznik o nazwie *uploadArtifacts* . Jeśli wszelkie artefakty trzeba przekazać, ustaw przełącznik *uploadArtifacts* podczas połączenia skrypt. Należy zauważyć, że plik szablonu głównym i parametrów pliku nie do przekazania. Tylko innych plików, takie jak skrypty konfiguracji zagnieżdżonych szablonach wdrażania, a trzeba przekazać pliki aplikacji.

## <a name="detailed-script-description"></a>Skrypt szczegółowy opis

Oto opis jakie Wybierz sekcje Wykonaj skrypt programu PowerShell Azure rozmieszczanie AzureResourceGroup.ps1.

>[AZURE.NOTE] W takim wersji 1.0 skrypt AzureResourceGroup.ps1 rozmieszczanie.

1.  Deklarowanie parametrów wymagane przez Menedżera zasobów Azure wdrożenia projektu. Niektóre parametry mieć wartości domyślnych, które zostały ustawione podczas tworzenia projektu. Można zmienić te wartości domyślne skryptu lub dodać inne wartości, przed wykonaniem skrypt.

    ```
    Param(
      [string] [Parameter(Mandatory=$true)] $ResourceGroupLocation,
      [string] $ResourceGroupName = 'AzureResourceGroup1',
      [switch] $UploadArtifacts,
      [string] $StorageAccountName,
      [string] $StorageAccountResourceGroupName,
      [string] $StorageContainerName = $ResourceGroupName.ToLowerInvariant() + '-stageartifacts',
      [string] $TemplateFile = '..\Templates\azuredeploy.json',
      [string] $TemplateParametersFile = '..\Templates\azuredeploy.parameters.json',
      [string] $ArtifactStagingDirectory = '..\bin\Debug\staging',
      [string] $AzCopyPath = '..\Tools\AzCopy.exe',
      [string] $DSCSourceFolder = '..\DSC'
    )
    ```

  	|Parametr|Opis|
  	|---|---|
  	|$ResourceGroupLocation|Region lub dane lokalizacja Centrum grupa zasobów, takich jak **Zachodnia USA** lub **wschodnioazjatycki**.|
  	|$ResourceGroupName|Nazwa grupy Azure zasobów.|
  	|$UploadArtifacts|Wartość binarna, która wskazuje, czy artefakty trzeba przekazać do Azure z systemu.|
  	|$StorageAccountName|Nazwa konta magazynu platformy Azure miejsce, w którym są przekazywane do artefaktów.|
  	|$StorageAccountResourceGroupName|Nazwa grupy zasobów Azure, zawierający odpowiednie konto miejsca do magazynowania.|
  	|$StorageContainerName|Nazwa kontenera magazynu używany do przekazywania artefakty.|
  	|$TemplateFile|Ścieżka do pliku wdrożenia (`<app name>.json`) w projekcie Azure grupa zasobów.|
  	|$TemplateParametersFile|Ścieżka do pliku parametrów (`<app name>.parameters.json`) w projekcie Azure grupa zasobów.|
  	|$ArtifactStagingDirectory|Ścieżka na komputerze, na którym artefakty lokalnie przekazanych w tym folderze głównym skrypt programu PowerShell. Ta ścieżka może być bezwzględne lub lokalizację skryptu.|
  	|$AzCopyPath|Ścieżka miejsce, w którym narzędzie AzCopy.exe kopiuje jej pliki zip, w tym folderze głównym skrypt programu PowerShell. Ta ścieżka może być bezwzględne lub lokalizację skryptu.|
  	|$DSCSourceFolder|Ścieżka do folderu źródła DSC (potrzeby stan konfiguracji), w tym folderze głównym skrypt programu PowerShell. Ta ścieżka może być bezwzględne lub lokalizację skryptu. Zobacz [Wprowadzenie do rozszerzenia Azure programu PowerShell DSC (potrzeby stan konfiguracji)](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx), jeśli to możliwe, aby uzyskać więcej informacji.|

1.  Sprawdź, czy artefakty należy przekazać do Azure. W przeciwnym razie przejdź do kroku 11. W przeciwnym razie należy wykonać następujące czynności.

1.  Zmienienie dowolny zmienne ścieżki względne ścieżki bezwzględne Na przykład, takich jak zmienić ścieżkę `..\Tools\AzCopy.exe` do `C:\YourFolder\Tools\AzCopy.exe`. Ponadto zainicjować zmiennych *ArtifactsLocationName* i *ArtifactsLocationSasTokenName* wartość null. *ArtifactsLocation* i *SaSToken* mogą być parametrów do szablonu. W przypadku wartości null po odczytu w pliku parametrów, skrypt generuje wartości dla nich.

    Narzędzia Azure umożliwia wartości parametru *_artifactsLocation* i *_artifactsLocationSasToken* w szablonie Zarządzanie artefakty. Jeśli skrypt programu PowerShell umożliwia znalezienie wyrazów parametrów przy użyciu nazw, ale nie mają wartości parametrów, skrypt przekazywanie artefaktów i zwraca odpowiednie wartości dla tych parametrów. Przekazuje następnie do polecenia cmdlet za pośrednictwem `@OptionsParameters`.

  	|Zmienna|Opis|
  	|---|---|
  	|ArtifactsLocationName|Ścieżka do miejsce, w którym znajdują się artefaktów Azure.|
  	|ArtifactsLocationSasTokenName|Nazwa tokenu skojarzeń zabezpieczeń (udostępnione podpis programu Access), używaną przez skrypt do uwierzytelniania Bus usługi. Uzyskać więcej informacji, zobacz [Uwierzytelnianie podpisu dostępu udostępnione za pomocą usługi Bus](service-bus-shared-access-signature-authentication.md) .|

    ```
    if ($UploadArtifacts) {
    # Convert relative paths to absolute paths if needed
    $AzCopyPath = [System.IO.Path]::Combine($PSScriptRoot, $AzCopyPath)
    $ArtifactStagingDirectory = [System.IO.Path]::Combine($PSScriptRoot, $ArtifactStagingDirectory)
    $DSCSourceFolder = [System.IO.Path]::Combine($PSScriptRoot, $DSCSourceFolder)

    Set-Variable ArtifactsLocationName '_artifactsLocation' -Option ReadOnly
    Set-Variable ArtifactsLocationSasTokenName '_artifactsLocationSasToken' -Option ReadOnly

    $OptionalParameters.Add($ArtifactsLocationName, $null)
    $OptionalParameters.Add($ArtifactsLocationSasTokenName, $null)
    ```

1.  Ta sekcja sprawdza, czy <app name>. plik parameters.json (nazywane "pliku parametrów") ma nadrzędnym **parametrów** nazwanych (w `else` bloku). W przeciwnym razie ma nie nadrzędnym. Tych formatów jest do przyjęcia.
    
    ```
    if ($JsonParameters -eq $null) {
            $JsonParameters = $JsonContent
        }
        else {
            $JsonParameters = $JsonContent.parameters
        }
    ```

1.  Iterację Kolekcja parametrów JSON. Jeśli wartość parametru została przypisana do *_artifactsLocation* lub *_artifactsLocationSasToken*, a następnie ustaw zmienną *$OptionalParameters* tych wartości. Skrypt zapobiega niezamierzonemu zastąpieniu podane wartości parametrów.

    ```
    $JsonParameters | Get-Member -Type NoteProperty | ForEach-Object {
        $ParameterValue = $JsonParameters | Select-Object -ExpandProperty $_.Name

        if ($_.Name -eq $ArtifactsLocationName -or $_.Name -eq $ArtifactsLocationSasTokenName) {
            $OptionalParameters[$_.Name] = $ParameterValue.value
        }
    }
    ```

1.  Uzyskaj klucz konta miejsca do magazynowania i kontekst dla zasobu konta magazynu służy do przechowywania artefakty do wdrożenia.

    ```
    $StorageAccountKey = (Get-AzureRMStorageAccountKey -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Key1

    $StorageAccountContext = (Get-AzureRmStorageAccount -ResourceGroupName $StorageAccountResourceGroupName -Name $StorageAccountName).Context
    ```

1.  Jeśli używasz programu PowerShell DSC Konfigurowanie maszyny wirtualnej, rozszerzenie DSC wymaga artefakty znajdować się w pliku zip pojedynczy. Tak Utwórz plik archiwum zip konfiguracji DSC. Aby to zrobić, sprawdź, czy istnieje $DSCSourceFolder. Jeśli konfiguracja DSC istnieje, usuń go, a następnie utwórz nowy plik skompresowany o nazwie dsc.zip.

    ```
    # Create DSC configuration archive
    if (Test-Path $DSCSourceFolder) {
    Add-Type -Assembly System.IO.Compression.FileSystem
        $ArchiveFile = Join-Path $ArtifactStagingDirectory "dsc.zip"
        Remove-Item -Path $ArchiveFile -ErrorAction SilentlyContinue
        [System.IO.Compression.ZipFile]::CreateFromDirectory($DSCSourceFolder, $ArchiveFile)
    }
    ```

1.  Jeśli żadna ścieżka dla artefaktów Azure znajduje się w pliku parametrów, Ustaw ścieżkę skrypt programu PowerShell używany podczas przekazywania artefakty. Aby to zrobić, Utwórz ścieżkę przy użyciu kombinacji ścieżka punktu końcowego konta przestrzeni dyskowej oraz nazwę kontenera magazynu. Następnie zaktualizuj następującą ścieżkę nowego pliku parametrów.

    ```
    # Generate the value for artifacts location if it is not provided in the parameter file
    $ArtifactsLocation = $OptionalParameters[$ArtifactsLocationName]
    if ($ArtifactsLocation -eq $null) {
        $ArtifactsLocation = $StorageAccountContext.BlobEndPoint + $StorageContainerName
        $OptionalParameters[$ArtifactsLocationName] = $ArtifactsLocation
    }
    ```

1.  Narzędzie **AzCopy** (zawarte w folderze **Narzędzia** projektu wdrożenia grupa zasobów Azure) aby skopiować wszystkie pliki z ścieżki lokalnej upuszczania miejsca do magazynowania do konta usługi Azure magazyn online. Jeśli ten krok nie powiedzie się, Zamknij skrypt, ponieważ wdrażanie prawdopodobnie nie powiodła się bez wymagane artefakty.

    ```
    # Use AzCopy to copy files from the local storage drop path to the storage account container
    & $AzCopyPath """$ArtifactStagingDirectory""", $ArtifactsLocation, "/DestKey:$StorageAccountKey", "/S", "/Y", "/Z:$env:LocalAppData\Microsoft\Azure\AzCopy\$ResourceGroupName"
    if ($LASTEXITCODE -ne 0) { return }
    ```

1.  Jeśli token skojarzeń zabezpieczeń dla lokalizacji artefakty nie jest dostępne w pliku parametrów, utwórz ją o podanie tymczasowy dostęp tylko do odczytu w kontenerze magazyn online. Następnie należy przekazać token skojarzeń zabezpieczeń było wiersz_polecenia jako "optionalParameter". Zauważ, że wszystkie parametry przekazywane wiersz_polecenia ma pierwszeństwo wartości podane w pliku parametrów.

    ```
    # Generate the value for artifacts location SAS token if it is not provided in the parameter file
    $ArtifactsLocationSasToken = $OptionalParameters[$ArtifactsLocationSasTokenName]
    if ($ArtifactsLocationSasToken -eq $null) {
       # Create a SAS token for the storage container - this gives temporary read-only access to the container
       $ArtifactsLocationSasToken = New-AzureStorageContainerSASToken -Container $StorageContainerName -Context $StorageAccountContext -Permission r -ExpiryTime (Get-Date).AddHours(4)
       $ArtifactsLocationSasToken = ConvertTo-SecureString $ArtifactsLocationSasToken -AsPlainText -Force
       $OptionalParameters[$ArtifactsLocationSasTokenName] = $ArtifactsLocationSasToken
    }
    ```

1.  Tworzenie grupy zasobów, jeśli jeszcze nie istnieje i zaznacz plik szablonu i parametrów uniemożliwiających wdrożenia pomyślną błędy sprawdzania poprawności.

    ```
    # Create or update the resource group using the specified template file and template parameters file
    New-AzureRMResourceGroup -Name $ResourceGroupName -Location $ResourceGroupLocation -Verbose -Force -ErrorAction Stop

    Test-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterFile $TemplateParametersFile @OptionalParameters -ErrorAction Stop
    ```

1. Na koniec wdrożyć szablonu. Kod ten tworzy unikatową nazwę wdrożenie przy użyciu sygnatura czasowa.

    ```
    New-AzureRMResourceGroupDeployment -Name ((Get-ChildItem $TemplateFile).BaseName + '-' + ((Get-Date).ToUniversalTime()).ToString('MMdd-HHmm')) `
        -ResourceGroupName $ResourceGroupName `
        -TemplateFile $TemplateFile `
        -TemplateParameterFile $TemplateParametersFile `
        @OptionalParameters `
        -Force -Verbose
    ```

## <a name="deploy-the-resource-group"></a>Wdrażanie grupa zasobów

### <a name="to-deploy-the-resource-group-in-visual-studio"></a>Aby wdrożyć grupa zasobów w programie Visual Studio

1. W menu skrótów projektu Azure grupa zasobów, wybierz pozycję **Deploy** > **Nowego wdrożenia**.

    ![][0]

1. W oknie dialogowym **Rozmieszczanie do grupy zasobów** albo wybierz pozycję istniejącej grupy zasobów w polu listy rozwijanej umożliwia Wdroż lub wybranie ** &lt;Utwórz nowy... &gt;** Aby utworzyć nową grupę zasobów.

    ![][1]

1. Jeśli zostanie wyświetlony monit, wprowadź nazwę grupy zasobów i lokalizację w oknie dialogowym **Tworzenie grupa zasobów** , a następnie wybierz przycisk **Utwórz** .

    ![][2]

1. Wybierz przycisk **Edytuj parametry** , aby wyświetlić okno dialogowe **Edytuj parametry** , a następnie wprowadź wszelkie brakujące wartości parametru.

    ![][3]

    >[AZURE.NOTE] Parametry wymagane na potrzeby wartości, to okno dialogowe pojawia się automatycznie po wdrożeniu.

    ![][4]

1. Po zakończeniu wprowadzania wartości parametru, wybierz przycisk **Zapisz** , a następnie wybierz przycisk **Deploy** .

    Skrypt rozmieszczania (rozmieszczania AzureResourceGroup.ps1) jest uruchamiany i szablonu, oraz wszelkie artefakty wdraża Azure.

### <a name="to-deploy-the-resource-group-by-using-powershell"></a>Aby wdrożyć grupa zasobów przy użyciu programu PowerShell

Jeśli chcesz uruchomić skrypt bez użycia polecenia wdrożenia programu Visual Studio i elementy interfejsu użytkownika, w menu skrótów dla skrypt, wybierz pozycję **Otwórz za pomocą programu PowerShell ISE**.

![][5]


## <a name="command-deployment-examples"></a>Polecenie wdrożenie przykładów

### <a name="deploy-using-default-values"></a>Wdrażanie przy użyciu wartości domyślnych

W tym przykładzie pokazano sposób uruchamianie skryptu za pomocą domyślne wartości parametru. (Ponieważ parametr lokalizacja nie ma wartość domyślną, musisz podać jedną.)

`.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus`

### <a name="deploy-overriding-the-default-values"></a>Wdrażanie zastępowanie wartości domyślne

W tym przykładzie pokazano sposób uruchomić skrypt, aby wdrożyć szablon i parametrów pliki, które różnią się od wartości domyślne.

```
.\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation eastus –TemplateFile ..\templates\AnotherTemplate.json –TemplateParametersFile ..\templates\AnotherTemplate.parameters.json
```

### <a name="deploy-using-uploadartifacts-for-staging"></a>Wdrażanie przy użyciu UploadArtifacts dla tymczasowego

W tym przykładzie pokazano sposób uruchomić skrypt, aby przekazać artefakty z folderu wersji i wdrażanie szablonów inne niż domyślne.

```
.\Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory ..\bin\release\staging
```

W tym przykładzie pokazano sposób uruchomić skrypt w zadaniu Azure programu PowerShell w Visual Studio Online.

```
$(Build.StagingDirectory)/AzureResourceGroup1/Scripts/Deploy-AzureResourceGroup.ps1 -StorageAccountName 'mystorage' -StorageAccountResourceGroupName 'Default-Storage-EastUS' -ResourceGroupName 'myResourceGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\windowsvirtualmachine.json' -TemplateParametersFile '..\templates\windowsvirtualmachine.parameters.json' -UploadArtifacts -ArtifactStagingDirectory $(Build.StagingDirectory)
```

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o Menedżera zasobów Azure, czytając [Omówienie Menedżera zasobów Azure](azure-resource-manager/resource-group-overview.md).

Aby uzyskać więcej przykładów Praca z projektami Azure grupa zasobów, zobacz [Rozmieszczanie Azure zasobów i zarządzanie nimi](https://github.com/Microsoft/HealthClinic.biz/wiki/Deploy-and-manage-Azure-resources) z Połącz [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [Pokaz](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Aby uzyskać więcej Przewodniki Szybki Start z pokaz HealthClinic.biz zobacz [Azure Deweloper narzędzia Przewodniki Szybki Start](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

[0]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy1c.png
[1]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy2bc.png
[2]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy3bc.png
[3]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy4bc.png
[4]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy5c.png
[5]: ./media/vs-azure-tools-resource-groups-how-script-works/deploy6c.png
