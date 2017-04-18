<properties
    pageTitle="Wdrażanie maszynowego uczenia obszaru roboczego przy użyciu szablonu Menedżera zasobów Azure | Microsoft Azure"
    description="Jak wdrożyć szkoleniowe komputera Azure za pomocą Menedżera zasobów Azure szablonu obszaru roboczego"
    services="machine-learning"
    documentationCenter=""
    authors="ahgyger"
    manager="haining"
    editor="garye"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="ahgyger"/>
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Wdrażanie maszynowego uczenia obszaru roboczego przy użyciu Azure Menedżera zasobów

## <a name="introduction"></a>Wprowadzenie
Korzystanie z Menedżera zasobów Azure szablonu rozmieszczenia zapisuje czas, umożliwiając skalowalna umożliwia wdrażanie powiązanych elementów ze sprawdzaniem poprawności i spróbuj ponownie mechanizmu. Aby skonfigurować Azure maszynowego uczenia w obszarach roboczych, na przykład, musisz najpierw skonfigurować konto Azure miejsca do magazynowania, a następnie wdrażać obszaru roboczego. Załóżmy tych czynności ręcznie setki obszarów roboczych. Łatwiejsze alternatywny jest wdrażanie obszaru roboczego Azure maszynowego uczenia i jego zależności przy użyciu szablonu Azure Menedżera zasobów. W tym artykule przejście przez ten proces krok po kroku. Aby uzyskać doskonałe Omówienie Menedżera zasobów Azure zobacz [Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Procedura: tworzenie obszaru roboczego nauki komputera
Firma Microsoft będzie utworzyć grupę Azure zasobów, a następnie wdrożyć nowe konto Azure miejsca do magazynowania i nowego Azure maszynowego uczenia obszaru roboczego przy użyciu szablonu Menedżera zasobów. Po zakończeniu rozmieszczania firma Microsoft zostanie wydrukowana ważne informacje o obszarach roboczych, które zostały utworzone (klucz podstawowy, workspaceID i adres URL do obszaru roboczego).

### <a name="create-an-azure-resource-manager-template"></a>Tworzenie szablonu Azure Menedżera zasobów
Obszar roboczy nauki komputera wymaga konta Azure miejsca do magazynowania do przechowywania połączone z nim zestawu danych.
Następujący szablon używa nazwę grupy zasobów, aby wygenerować nazwę konta magazynu i nazwę obszaru roboczego.  Użyto także nazwę konta magazynu jako właściwości podczas tworzenia obszaru roboczego.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Zapisz ten szablon jako plik mlworkspace.json w obszarze c:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Wdrażanie grupa zasobów, na podstawie szablonu
* Otwieranie programu PowerShell
* Instalowanie modułów dla Menedżera zasobów Azure i zarządzanie usługą Azure  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Poniższe czynności, Pobierz i zainstaluj moduły konieczne wykonaj pozostałe kroki. To tylko musi zostać wykonane raz w środowisku, w którym są wykonywane polecenia programu PowerShell.   

* Uwierzytelnianie Azure  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
W tym kroku należy powtórzyć dla każdej sesji. Po uwierzytelnieniu, powinny być wyświetlane informacje o Twojej subskrypcji.

![Konto Azure][1]

Teraz gdy mamy już dostęp do Azure, możemy utworzyć grupy zasobów.

* Tworzenie grupy zasobów

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Upewnij się, że grupa zasobów jest poprawnie obsługi administracyjnej. **ProvisioningState** powinny być "powiodło się."
Aby wygenerować nazwę konta magazynu w szablonie zostanie użyta nazwa grupy zasobów. Nazwę konta magazynu musi należeć do przedziału od 3 do 24 znaków i używanie tylko małe litery i cyfry.

![Grupa zasobów][2]

* Za pomocą wdrożenia grupa zasobów, wdrożenie nowego obszaru roboczego nauki komputera.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Po zakończeniu rozmieszczania jest proste właściwości dostępu do obszaru roboczego, który zostanie wdrożony. Na przykład można korzystać z podstawowego tokenu klucza.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Innym sposobem pobrać tokenów istniejącego obszaru roboczego jest za pomocą polecenia Wywołaj AzureRmResourceAction. Można na przykład listy głównego i pomocniczego tokeny wszystkie obszary robocze.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Po ponieważ jest ono inicjowane obszaru roboczego, można też zautomatyzować wiele zadań Azure maszynowego uczenia Studio za pomocą [Modułu programu PowerShell Azure szkoleniowe na komputerze](http://aka.ms/amlps).

## <a name="next-steps"></a>Następne kroki 
* Dowiedz się więcej na temat [Tworzenia szablonów Menedżera zasobów Azure](../resource-group-authoring-templates.md). 
* Masz przyjrzeć się [Repozytorium szablonów Szybki Start Azure](https://github.com/Azure/azure-quickstart-templates). 
* Obejrzyj ten klip wideo o [Azure Menedżera zasobów](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 
 
<!--Image references-->
[1]: ../media/machine-learning-deploy-with-resource-manager-template/azuresubscription.png
[2]: ../media/machine-learning-deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
