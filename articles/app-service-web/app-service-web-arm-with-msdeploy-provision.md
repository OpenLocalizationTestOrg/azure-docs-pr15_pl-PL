<properties
    pageTitle="Wdrażanie aplikacji sieci web MSDeploy za pomocą certyfikatu ssl i nazwa hosta"
    description="Wdrażanie aplikacji sieci web przy użyciu MSDeploy i skonfigurowanie niestandardowych hostname i certyfikat SSL przy użyciu szablonu Azure Menedżera zasobów"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Wdrażanie aplikacji sieci web przy użyciu MSDeploy, niestandardowe hostname i certyfikat SSL

Ten przewodnik opisano tworzenie kompleksowe — wdrożenie dla aplikacji sieci Web programu Azure, używanie MSDeploy, a także dodając niestandardowe nazwa hosta i certyfikatu SSL do szablonu ARM.

Aby uzyskać więcej informacji na temat tworzenia szablonów zobacz [Tworzenie szablonów Menedżera zasobów Azure](../resource-group-authoring-templates.md).

###<a name="create-sample-application"></a>Tworzenie aplikacji przykładowej

Wdrażania aplikacji sieci web ASP.NET. Pierwszym krokiem jest utworzenie aplikacji sieci web prostych (lub można użyć istniejącego — w takim przypadku możesz pominąć ten krok).

Otwórz program Visual Studio 2015 i wybierz pozycję Plik > Nowy projekt. W wyświetlonym oknie dialogowym Wybierz sieci Web > aplikacji sieci Web programu ASP.NET. W obszarze Szablony wybierz sieci Web i wybierz szablon MVC. Wybierz _Typ uwierzytelniania zmiany_ _Bez_uwierzytelniania. To jest po prostu nawiązać przykładowej aplikacji możliwie najprostsze.

W tym momencie należy podstawowe aplikacji sieci web programu ASP.Net gotowe do użycia w ramach procesu wdrażania.

###<a name="create-msdeploy-package"></a>Tworzenie pakietu MSDeploy

Następnym krokiem jest utworzenie pakietu wdrażania aplikacji sieci web Azure. Aby to zrobić, zapisywanie projektu, a następnie uruchom następujące polecenie w wierszu polecenia:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Spowoduje to utworzenie pakietu ZIP w folderze tabelę PackageLocation. Aplikacja jest teraz gotowy do wdrożenia, której można obecnie tworzyć się szablonu Menedżera zasobów Azure to zrobić.

###<a name="create-arm-template"></a>Tworzenie szablonu ARM
Najpierw Zacznijmy szablon podstawowy ARM, który powoduje utworzenie aplikacji sieci web i hostingu plan (należy zauważyć, że parametry i zmienne nie są wyświetlane w celu jego skrócenia).

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Następny będzie konieczne modyfikowanie Przełącz zagnieżdżonych zasób MSDeploy zasobu aplikacji sieci web. Dzięki temu będzie można do odwołania pakiet utworzony wcześniej i określić Menedżera zasobów Azure za pomocą MSDeploy wdrażanie pakietu Azure aplikacji sieci Web. Poniżej przedstawiono zasób Microsoft.Web/sites z zagnieżdżonych zasobów MSDeploy:

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Teraz można zauważyć, że zasobów MSDeploy ma właściwość **packageUri** , która jest definiowana w następujący sposób:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

Ten **packageUri** ma identyfikator uri konta miejsca do magazynowania, który wskazuje na koncie miejsca do magazynowania przekazywania będzie pocztowy pakiet do. Menedżer zasobów Azure będzie używać [Udostępnionych podpisów dostęp](../storage/storage-dotnet-shared-access-signature-part-1.md) do rozwijane pakietu lokalnie z konta miejsca do magazynowania przy umieszczaniu szablonu. Ten proces będzie zautomatyzowany za pomocą programu PowerShell skrypt, który będzie przekazać pakiet i połączeń utworzyć klucze interfejsu API zarządzania Azure wymagane i przekazać te do szablonu jako parametrów (*_artifactsLocation* i *_artifactsLocationSasToken*). Należy określić parametry w folderze i nazwę pliku pakietu jest przekazywany w kontenerze miejsca do magazynowania.

Następnie należy dodać w inny zasób zagnieżdżonych skonfigurować wiązania hostname, aby korzystać z domeny niestandardowej. Będzie należy najpierw upewnij się, że jesteś właścicielem nazwa hosta i skonfigurować ją do weryfikowane przez Azure właścicielem jej — zobacz [Konfigurowanie niestandardowej nazwy domeny w usłudze Azure aplikacji](web-sites-custom-domain-name.md). Po wykonaniu tej operacji można dodać do szablonu w sekcji Microsoft.Web/sites zasobów następujące czynności:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Na koniec należy dodać najwyższego poziomu zasób Microsoft.Web/certificates. Tego zasobu będzie zawierał certyfikatu SSL i wystąpią na tym samym poziomie jako aplikacji sieci web i hostingu planu.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

Konieczne będzie mieć certyfikat SSL w celu skonfigurowania tego zasobu. Po umieszczeniu nieprawidłowy certyfikat należy wyodrębnić bajtów pfx jako ciąg base64. Aby wyodrębnić to jedna z opcji jest Użyj następującego polecenia programu PowerShell:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Użytkownik może następnie przekazać jako parametr, do szablonu wdrożenia ARM.

W tym momencie szablon ARM jest gotowy.

###<a name="deploy-template"></a>Wdrażanie szablonu

Ostateczne kroki mają element to wszystko razem do wdrożenia pełnego zakończenia do końca. Aby ułatwić wdrożenie można wykorzystać skrypt programu PowerShell **AzureResourceGroup.ps1 rozmieszczanie** , dodawany podczas tworzenia grupy zasobów Azure projektu w programie Visual Studio ułatwiające przesyłanie wszelkie artefakty wymagane w szablonie. Wymaga utworzył konto miejsca do magazynowania, który ma być używany wyprzedzeniem. W tym przykładzie utworzony konto udostępnionego miejsca do magazynowania dla package.zip do przekazania. Skrypt będzie używana AzCopy, aby przekazać pakiet do konta miejsca do magazynowania. Przejście w lokalizacji folderu artefaktu i skrypt zostanie automatycznie przekazywać wszystkie pliki w tym katalogu do kontenera nazwanych magazynu. Po wywołaniu rozmieszczanie AzureResourceGroup.ps1 musisz następnie zaktualizuj powiązań protokołu SSL do zamapować hostname niestandardowych przy użyciu certyfikatu SSL.

Następujące programu PowerShell zawiera pełne rozmieszczenie nawiązywania połączeń z rozmieszczanie-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

Na tym etapie aplikacja powinna został wdrożony i powinno być możliwe przejść do niego za pośrednictwem https://www.yourcustomdomain.com
