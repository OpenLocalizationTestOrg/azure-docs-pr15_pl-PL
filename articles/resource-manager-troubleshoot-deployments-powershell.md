<properties
   pageTitle="Wyświetlanie operacji rozmieszczania przy użyciu programu PowerShell | Microsoft Azure"
   description="Informacje dotyczące używania programu PowerShell Azure do wykrywania problemów z wdrożenia Menedżera zasobów."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-powershell"></a>Wyświetlanie operacji wdrażania przy użyciu programu PowerShell Azure

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [Programu PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Polecenie Azure](resource-manager-troubleshoot-deployments-cli.md)
- [INTERFEJSU API USŁUGI REST](resource-manager-troubleshoot-deployments-rest.md)

Możesz wyświetlić operacji wdrażania przy użyciu programu PowerShell Azure. Może być najbardziej interesuje przeglądanie operacji, gdy masz otrzymujesz komunikat o błędzie podczas wdrażania, w tym artykule omówiono wyświetlanie operacji, które nie powiodło się. PowerShell zawiera polecenia cmdlet umożliwia łatwe znajdowanie błędów i określanie potencjalne rozwiązania.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Aby uniknąć błędów, należy sprawdzania poprawności szablonie i infrastruktury przed wdrożeniem. Możesz także zalogować żądanie dodatkowych i informacji odpowiedzi podczas wdrażania, które mogą być pomocne przy dalszym rozwiązywaniu problemów. Aby dowiedzieć się, sprawdzanie poprawności i rejestrowania informacji i odpowiadania na wezwania, zobacz [Rozmieszczanie grupa zasobów za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy.md).

## <a name="use-deployment-operations-to-troubleshoot"></a>Rozwiązywanie problemów za pomocą operacji rozmieszczania

1. Aby uzyskać ogólny stan wdrożenia, użyj polecenia **Get-AzureRmResourceGroupDeployment** . Można filtrować wyniki tylko wdrożenia, które nie powiodło się.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Która zwraca wdrożeń nie powiodło się w następującym formacie:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Każdego rozmieszczenia zazwyczaj składa się z wielu operacji przy każdorazowym reprezentującą etap procesu wdrażania. Aby odkryć, czym polega problem z wdrożeniu, zazwyczaj należy wyświetlić szczegółowe informacje o operacji rozmieszczania. Można wyświetlić stan operacji z **Get-AzureRmResourceGroupDeploymentOperation**.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Która zwraca operacji wielu z każdego z nich w następującym formacie:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. Aby uzyskać szczegółowe informacje o niepowodzeniu operacji, pobrać właściwości dla operacji ze stanem **nie powiodło się** .

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Która zwraca wszystkie operacje nie powiodło się z każdego z nich w następującym formacie:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Zanotuj identyfikator śledzenia operacji. Użyjesz który w następnym kroku skoncentrować się na określonej operacji.

4. Aby uzyskać komunikat o stanie określonego działania nie powiodło się, użyj następującego polecenia:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Która zwraca:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## <a name="use-audit-logs-to-troubleshoot"></a>Używanie dzienników inspekcji rozwiązywać problemy

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Aby wyświetlić błędy wdrożenia, wykonaj następujące czynności:

1. Aby pobrać Wpisy dziennika, uruchom polecenie **Get-AzureRmLog** . Za pomocą parametrów **Grupa zasobów** i **Stan** zwraca tylko te zdarzenia, które nie powiodło się dla pojedynczej grupy zasobów. Jeśli nie określisz godzinę rozpoczęcia i zakończenia, pozycje ostatniej godziny są zwracane.
Na przykład, aby pobrać niepowodzeniu operacji ostatniej godziny uruchamianie:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    Możesz określić określonego przedziału czasu. W następnym przykładzie opisano działań nie powiodło się ostatniego dnia. 

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Lub można ustawić dokładny czas rozpoczęcia i zakończenia dla akcji nie powiodło się:

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Jeśli to polecenie zwraca zbyt wiele wpisów i właściwości, możesz skoncentrować z inspekcji biznesową pobierając właściwość **Properties** . Firma Microsoft będzie także zawierać parametr **DetailedOutput** wyświetlone komunikaty o błędach.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Która zwraca właściwości wpisy dziennika w następującym formacie:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Załóżmy na podstawie tych wyników, skoncentrować się na drugi element. Możesz uściślić wyniki, sprawdzając komunikat o stanie dla tego wpisu.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Która zwraca:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## <a name="next-steps"></a>Następne kroki

- Aby uzyskać pomoc dotyczącą usuwania błędów danego wdrożenia zobacz [Usuwanie typowych błędów podczas wdrażania zasobów Azure przy użyciu Menedżera zasobów Azure](resource-manager-common-deployment-errors.md).
- Aby uzyskać informacje o za pomocą dzienników inspekcji monitorowanie innych działań, zobacz [Inspekcja operacji przy użyciu Menedżera zasobów](resource-group-audit.md).
- Aby sprawdzić poprawność wdrożenia przed jego wykonaniem, zobacz temat [Deploy grupa zasobów za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy.md).

