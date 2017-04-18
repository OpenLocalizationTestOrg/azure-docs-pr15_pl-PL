<properties
   pageTitle="Wdrażanie maszyn wirtualnych przy użyciu statycznego publiczny adres IP przy użyciu szablonu w Menedżerze zasobów | Microsoft Azure"
   description="Dowiedz się, jak wdrożyć maszyny wirtualne przy użyciu statycznego publiczny adres IP przy użyciu szablonu w Menedżerze zasobów"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-a-template"></a>Wdrażanie maszyn wirtualnych przy użyciu statycznego publiczny adres IP przy użyciu szablonu

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]model klasyczny wdrożenia.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-resources-in-a-template-file"></a>Publiczne IP zasobów w pliku szablonu

Można wyświetlić i pobrać [Przykładowy szablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

W sekcji poniżej przedstawiono definicji publicznej zasób IP, w oparciu o tego scenariusza powyżej.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[variables('webVMSetting').pipName]",
        "location": "[variables('location')]",
        "properties": {
          "publicIPAllocationMethod": "Static"
        },
        "tags": {
          "displayName": "PublicIPAddress - Web"
        }
      },

Zwróć uwagę, właściwość **publicIPAllocationMethod** , która jest ustawiona na *statyczne*. Ta właściwość może być *dynamiczne* (wartość domyślna) lub *statyczny*. Ustawienie statyczne gwarancje, które nigdy nie zmienia publiczny adres IP przydzielone.

W sekcji poniżej przedstawiono skojarzenia publiczny adres IP przy użyciu karty sieciowej.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[variables('webVMSetting').nicName]",
        "location": "[variables('location')]",
        "tags": {
          "displayName": "NetworkInterface - Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
          "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
          "ipConfigurations": [
            {
              "name": "ipconfig1",
              "properties": {
                "privateIPAllocationMethod": "Static",
                "privateIPAddress": "[variables('webVMSetting').ipAddress]",
                "publicIPAddress": {
                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
                },
                "subnet": {
                  "id": "[variables('frontEndSubnetRef')]"
                }
              }
            }
          ]
        }
      },

Zwróć uwagę, właściwość **publicIPAddress** wskazująca **identyfikator** zasobu o nazwie **variables('webVMSetting').pipName**. Jest to nazwa publicznej zasobu adresów IP, jak pokazano powyżej.

Na koniec powyżej sieciowej znajduje się we właściwości **networkProfile** maszyn wirtualnych, tworzenia.

      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Wdrażanie szablonu za pomocą kliknięcia do wdrożenia

Poniższy przykładowy szablon dostępny w publicznej repozytorium używa parametru plik zawierający domyślne wartości umożliwia generowanie scenariuszy opisanych powyżej. Aby wdrożyć tego szablonu, za pomocą kliknięcia, kliknij polecenie **Rozmieść Azure** w pliku Readme.md szablonu [maszyn wirtualnych przy użyciu statycznego PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) . Zamienianie wartości domyślne parametrów w razie potrzeby i wprowadź wartości parametrów puste.  Postępuj zgodnie z instrukcjami w portalu w celu utworzenia maszyny wirtualnej przy użyciu statycznego publiczny adres IP.

## <a name="deploy-the-template-by-using-powershell"></a>Wdrażanie szablon przy użyciu programu PowerShell

Aby wdrożyć szablon, który został pobrany przy użyciu programu PowerShell, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz Azure programu PowerShell, zobacz, [jak instalowanie i konfigurowanie programu PowerShell Azure](../powershell-install-configure.md) i postępuj zgodnie z instrukcjami kroki od 1 do 3.

2. W konsoli programu PowerShell Uruchom polecenie cmdlet **AzureRmResourceGroup nowy** , aby utworzyć nową grupę zasobów, jeśli to konieczne. Jeśli masz już utworzony grupa zasobów, przejdź do kroku 3.

        New-AzureRmResourceGroup -Name PIPTEST -Location westus

    Oczekiwany wynik:

        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. W konsoli programu PowerShell Uruchom polecenie cmdlet **New-AzureRmResourceGroupDeployment** wdrożyć szablonu.

        New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
            -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json

    Oczekiwany wynik:

        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : <Deployment date> <Deployment time>
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0

        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         

        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Wdrażanie szablonu za pomocą interfejsu wiersza polecenia Azure

Aby wdrożyć szablon przy użyciu interfejsu wiersza polecenia Azure, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz polecenie Azure, wykonaj czynności opisane w artykule [zainstalować i skonfigurować polecenie Azure](../xplat-cli-install.md) , a następnie czynności, aby podłączyć polecenie do subskrypcji w sekcji "Użyj azure logowania do uwierzytelnienia interakcyjnie" artykuł [Nawiązywanie połączenia z subskrypcji usługi Azure z interfejsu wiersza polecenia Azure (polecenie Azure)](../xplat-cli-connect.md) .
2. Uruchom polecenie **Konfiguracja azure tryb** , aby przełączyć się do trybu Menedżera zasobów, tak jak pokazano poniżej.

        azure config mode arm

    Oto oczekiwany wynik polecenia powyżej:

        info:    New mode is arm

3. Otwórz [plik parametr](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), zaznacz jego zawartość i zapisanie go w pliku na komputerze. W tym przykładzie parametry są zapisywane w pliku o nazwie *parameters.json*. Zmień wartości parametrów w pliku, jeśli to konieczne, ale co najmniej zalecane jest, zmień wartość parametru adminPassword na unikatowe, złożone hasło.

4. Uruchom polecenie cmdlet **Tworzenie grupy azure wdrożenia** wdrażania nowych VNet przy użyciu szablonu i parametr pliki pobrane i modyfikować powyżej. W tym poleceniu zastąp <path> ścieżką został zapisany plik. 

        azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json

    Oczekiwany wynik (listami wartości parametru używane):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/<Subscription ID>/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
