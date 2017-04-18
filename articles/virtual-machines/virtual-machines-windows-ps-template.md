<properties
    pageTitle="Tworzenie maszyn wirtualnych przy użyciu szablonu Menedżera zasobów | Microsoft Azure"
    description="Za pomocą szablonu Menedżera zasobów i programu PowerShell łatwo tworzyć nowe maszyny wirtualnej systemu Windows."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Tworzenie maszyny wirtualnej systemu Windows za pomocą szablonu Menedżera zasobów

W tym artykule przedstawiono szablonu Menedżera zasobów Azure i pokazano, jak wdrożyć go za pomocą programu PowerShell. Szablon wdraża jednym komputerze wirtualnych z systemem Windows Server w nowej wirtualnej sieci z jednej podsieci.

Należy trwa około 20 minut wykonaj kroki opisane w tym artykule.

> [AZURE.IMPORTANT] Jeśli chcesz z maszyn wirtualnych jako część zestawu dostępność, dodaj go do zestawu podczas tworzenia maszyn wirtualnych. Obecnie nie ma umożliwia dodawanie maszyny dostępności po jego utworzeniu.

## <a name="step-1-create-the-template-file"></a>Krok 1: Tworzenie pliku szablonu

Możesz utworzyć własny szablon przy użyciu informacje znajdujące się w [szablonów do tworzenia Azure Menedżera zasobów](../resource-group-authoring-templates.md). Można także wdrożyć szablony, które zostały utworzone przy użyciu [Szablonów Przewodniki Szybki Start Azure](https://azure.microsoft.com/documentation/templates/)za Ciebie.

1. Otwórz Edytor tekstu Ulubione i Dodaj elementu wymaganego schematu oraz element wymagane contentVersion:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Parametry](../resource-group-authoring-templates.md#parameters) nie są zawsze wymagane, ale umożliwiają do wprowadzania wartości po wdrożeniu szablonu. Dodawanie elementu parametry i jej elementów podrzędnych po elemencie contentVersion:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. [Zmienne](../resource-group-authoring-templates.md#variables) może służyć w szablonie, aby określić wartości, które mogą się zmienić często lub wartości, które muszą zostać utworzone z kombinacji wartości parametrów. Dodaj element zmienne po sekcji Parametry:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
        
4. [Zasoby](../resource-group-authoring-templates.md#resources) takie jak maszyna wirtualna, wirtualną sieć i konta miejsca do magazynowania są definiowane dalej w szablonie. Dodawanie sekcji zasoby po sekcji zmiennych:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvn1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
          
    >[AZURE.NOTE] W tym artykule tworzy maszyny wirtualnej wersją systemu operacyjnego Windows Server. Aby dowiedzieć się więcej o wybieraniu innych obrazów, zobacz [Nawigacja i wybierz pozycję Azure maszyn wirtualnych obrazów za pomocą programu Windows PowerShell i polecenie Azure](virtual-machines-linux-cli-ps-findimage.md).  
            
2. Zapisz plik szablonu jako *VirtualMachineTemplate.json*.

## <a name="step-2-create-the-parameters-file"></a>Krok 2: Tworzenie pliku parametrów

Aby określić wartości parametrów zasobów, które zostały zdefiniowane w szablonie, Utwórz plik parametry, zawierającą wartości, które są używane po wdrożeniu szablonu.

1. W edytorze tekstów skopiuj zawartość JSON do nowego pliku o nazwie *Parameters.json*:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Zobacz więcej informacji na temat [wymagań nazwy użytkownika i hasła](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. Zapisz plik parametrów.

## <a name="step-3-install-azure-powershell"></a>Krok 3: Instalowanie programu PowerShell Azure

Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla informacji na temat instalowania najnowszej wersji programu PowerShell Azure, wybierając subskrypcji i logowanie się do swojego konta.

## <a name="step-4-create-a-resource-group"></a>Krok 4: Tworzenie grupy zasobów

[Grupa zasobów](../azure-resource-manager/resource-group-overview.md), należy wdrożyć wszystkich zasobów.

1. Pobierz listę dostępnych lokalizacji, w którym można utworzyć zasoby.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. Zamień wartość **$locName** lokalizację z listy, na przykład **Centralnej USA**. Tworzenie zmiennej.

        $locName = "location name"
        
3. Zamień wartość **$rgName** nazwę nowej grupy zasobów. Tworzenie zmiennej i grup zasobów.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Powinien zostać wyświetlony coś w tym przykładzie:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5-create-the-resources-with-the-template-and-parameters"></a>Krok 5: Tworzenie zasobów przy użyciu szablonu i parametrów

Zamień wartość **$templateFile** ścieżkę i nazwę pliku szablonu. Zamień wartość **$parameterFile** ścieżkę i nazwę pliku parametrów. Utwórz zmienne, a następnie wdrożyć szablonu. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] Można także wdrożyć szablony i parametrów z konta Azure miejsca do magazynowania. Aby uzyskać więcej informacji, zobacz [Przy użyciu programu PowerShell Azure z nośnikami Azure](../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Następne kroki

- W przypadku problemów z wdrożenia następnym krokiem jest przeglądać [wdrożeń grupa zasobów Rozwiązywanie problemów z Azure portal](../resource-manager-troubleshoot-deployments-portal.md)
- Dowiedz się, jak zarządzać maszyny wirtualnej utworzone przez Ciebie, przeglądając artykuł [Zarządzanie maszyn wirtualnych za pomocą Menedżera zasobów Azure i programu PowerShell](virtual-machines-windows-ps-manage.md).
