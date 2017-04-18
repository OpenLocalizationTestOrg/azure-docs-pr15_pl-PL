<properties
   pageTitle="Wyświetlanie operacji rozmieszczania z polecenie Azure | Microsoft Azure"
   description="Informacje dotyczące używania polecenie Azure do wykrywania problemów z wdrożenia Menedżera zasobów."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-cli"></a>Wyświetlanie operacji wdrażania z polecenie Azure

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [Programu PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Polecenie Azure](resource-manager-troubleshoot-deployments-cli.md)
- [INTERFEJSU API USŁUGI REST](resource-manager-troubleshoot-deployments-rest.md)

Jeśli otrzymano komunikat o błędzie podczas wdrażania zasobów Azure, można wyświetlić więcej szczegółów dotyczących operacji wdrażania, które zostały wykonane. Polecenie Azure zawiera polecenia, które umożliwiają znajdowanie błędów i określanie potencjalne rozwiązania.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Aby uniknąć błędów, należy sprawdzania poprawności szablonie i infrastruktury przed wdrożeniem. Możesz także zalogować żądanie dodatkowych i informacji odpowiedzi podczas wdrażania, które mogą być pomocne przy dalszym rozwiązywaniu problemów. Aby dowiedzieć się, sprawdzanie poprawności i rejestrowania informacji i odpowiadania na wezwania, zobacz [Rozmieszczanie grupa zasobów za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy-cli.md).

## <a name="use-audit-logs-to-troubleshoot"></a>Używanie dzienników inspekcji rozwiązywać problemy

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Aby wyświetlić błędy wdrożenia, wykonaj następujące czynności:

1. Aby wyświetlić dzienników inspekcji, uruchom polecenie **Pokaż dziennik azure grupy** . Możesz umieścić **— ostatniego wdrożenia** opcję, aby pobrać tylko dziennik wdrożenia ostatnio.

        azure group log show ExampleGroup --last-deployment

2. Polecenie **Pokaż dziennik grupy azure** zwraca dużą ilość informacji. Do rozwiązywania problemów, zazwyczaj chcesz skupić się na operacji, które nie powiodło się. Poniższy skrypt używa opcji **— json** i narzędzie JSON [jq](https://stedolan.github.io/jq/) do wyszukiwania w dzienniku błędów wdrożenia.

        azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'
        
        {
        "claims": {
          "aud": "https://management.core.windows.net/",
          "iss": "https://sts.windows.net/<guid>/",
          "iat": "1442510510",
          "nbf": "1442510510",
          "exp": "1442514410",
          "ver": "1.0",
          "http://schemas.microsoft.com/identity/claims/tenantid": "{guid}",
          "http://schemas.microsoft.com/identity/claims/objectidentifier": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
          "puid": "XXXXXXXXXXXXXXXX",
          "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",
          "altsecid": "1:example.com:XXXXXXXXXXX",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string="">",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
          "name": "Tom FitzMacken",
          "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
          "groups": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
          "wids": "{guid}",
          "appid": "{guid}",
          "appidacr": "0",
          "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
          "http://schemas.microsoft.com/claims/authnclassreference": "1",
          "ipaddr": "000.000.000.000"
        },
        "properties": {
          "statusCode": "Conflict",
          "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
            mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
            \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
        },
        ...

    Możesz sprawdzić, czy **Właściwości** zawiera informacje w formacie json o niepowodzeniu operacji.

    Możesz użyć **— pełne** i opcje **— vv** , aby wyświetlić dodatkowe informacje z dzienników.  Używanie **— pełne** opcję, aby wyświetlić kroki operacje obsłużone na `stdout`. Historii żądania wykonania użyj opcji **- vv** . Wiadomości często udostępniają najważniejszych wskazówek dotyczących przyczyny wszystkie błędy.

3. Aby skoncentrować się na komunikat o stanie dla wpisów nie powiodło się, użyj następującego polecenia:

        azure group log show ExampleGroup --json | jq -r ".[] | select(.status.value == \"Failed\") | .properties.statusMessage"


## <a name="use-deployment-operations-to-troubleshoot"></a>Rozwiązywanie problemów za pomocą operacji rozmieszczania

1. Uzyskaj ogólny stan wdrożenia przy użyciu polecenia **Pokaż wdrożenia azure grupy** . W poniższym przykładzie Wdrażanie nie powiodło się.

        azure group deployment show --resource-group ExampleGroup --name ExampleDeployment
        
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : ExampleDeployment
        data:    ResourceGroupName  : ExampleGroup
        data:    ProvisioningState  : Failed
        data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
        data:    Mode               : Incremental
        data:    Name             Type    Value
        data:    ---------------  ------  ------------
        data:    siteName         String  ExampleSite
        data:    hostingPlanName  String  ExamplePlan
        data:    siteLocation     String  West US
        data:    sku              String  Free
        data:    workerSize       String  0
        info:    group deployment show command OK

2. Aby wyświetlić wiadomość nie powiodło się operacji wdrożenia, należy użyć:

        azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json  | jq ".[] | select(.properties.provisioningState == \"Failed\") | .properties.statusMessage.Message"


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać pomoc dotyczącą usuwania błędów danego wdrożenia zobacz [Usuwanie typowych błędów podczas wdrażania zasobów Azure przy użyciu Menedżera zasobów Azure](resource-manager-common-deployment-errors.md).
- Aby uzyskać informacje o za pomocą dzienników inspekcji monitorowanie innych działań, zobacz [Inspekcja operacji przy użyciu Menedżera zasobów](resource-group-audit.md).
- Aby sprawdzić poprawność wdrożenia przed jego wykonaniem, zobacz temat [Deploy grupa zasobów za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy.md).
