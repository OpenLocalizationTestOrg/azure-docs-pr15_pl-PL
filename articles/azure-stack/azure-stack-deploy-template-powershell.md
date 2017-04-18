<properties
    pageTitle="Wdrażanie szablonów z programem PowerShell w stos Azure | Microsoft Azure"
    description="Dowiedz się, jak wdrożyć maszyny wirtualnej przy użyciu szablonu Menedżera zasobów i programu PowerShell."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Wdrażanie szablonów w stos Azure za pomocą programu PowerShell

Wdrażanie szablonów Azure Menedżera zasobów, aby Zapewnić stosu Azure za pomocą programu PowerShell.  Menedżer zasobów szablony wdrażania i obsługi administracyjnej wszystkich zasobów dla aplikacji w jednym, skoordynowanego operacji.

## <a name="run-azurerm-powershell-cmdlets"></a>Uruchamianie polecenia cmdlet programu AzureRM PowerShell

W tym przykładzie, możesz uruchomić skrypt wdrożenia maszyny wirtualnej Zapewnić stosem Azure za pomocą szablonu Menedżera zasobów.  Przed kontynuowaniem upewnij się, że masz [zainstalowaniu i skonfigurowaniu programu PowerShell](azure-stack-connect-powershell.md)  

Używane w tym szablonie przykład wirtualny dysk twardy jest domyślny obraz marketplace (Centrum WindowsServer-2012-R2 — danych).

1.  Przejdź do <http://aka.ms/AzureStackGitHub>wyszukać szablon **101-proste — systemu windows — maszyn wirtualnych** , a następnie zapisanie go w następującej lokalizacji: c:\\szablonów\\azuredeploy-101-proste — windows-vm.json.

2.  W programie PowerShell Uruchom następujący skrypt wdrożenia. Zamień *nazwę użytkownika* i *hasło* nazwę użytkownika i hasło. W kolejnych zastosowań zwiększać wartość parametru *$myNum* zapobiec zastępowaniu wdrożenia.

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  Otwórz portal Azure stos, kliknij przycisk **Przeglądaj**, kliknij pozycję **maszyn wirtualnych**i poszukaj nowa maszyna wirtualna (*myDeployment001*).

## <a name="video-example-hybrid-virtual-machine-deployment"></a>Przykładowy klip wideo: wdrożenie hybrydowe maszyn wirtualnych

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>Następne kroki

[Wdrażanie szablonów z programu Visual Studio](azure-stack-deploy-template-visual-studio.md)
