<properties
   pageTitle="Wyświetlanie operacji rozmieszczania z interfejsu API usługi REST | Microsoft Azure"
   description="Informacje dotyczące używania interfejsu API usługi REST Menedżera zasobów Azure do wykrywania problemów z wdrożenia Menedżera zasobów."
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
   ms.date="06/13/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Wyświetlanie operacji wdrażania z interfejsu API usługi REST Menedżera zasobów Azure

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [Programu PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Polecenie Azure](resource-manager-troubleshoot-deployments-cli.md)
- [INTERFEJSU API USŁUGI REST](resource-manager-troubleshoot-deployments-rest.md)

Jeśli otrzymano komunikat o błędzie podczas wdrażania zasobów Azure, można wyświetlić więcej szczegółów dotyczących operacji wdrażania, które zostały wykonane. Interfejsu API usługi REST zawiera operacji, które umożliwiają znajdowanie błędów i określanie potencjalne rozwiązania.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Aby uniknąć błędów, należy sprawdzania poprawności szablonie i infrastruktury przed wdrożeniem. Możesz także zalogować żądanie dodatkowych i informacji odpowiedzi podczas wdrażania, które mogą być pomocne przy dalszym rozwiązywaniu problemów. Aby dowiedzieć się, sprawdzanie poprawności i rejestrowania informacji i odpowiadania na wezwania, zobacz [Rozmieszczanie grupa zasobów za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy-rest.md).

## <a name="troubleshoot-with-rest-api"></a>Rozwiązywanie problemów z interfejsu API usługi REST

1. Wdrażanie zasobów w działaniu [Tworzenie rozmieszczania szablonu](https://msdn.microsoft.com/library/azure/dn790564.aspx) . Aby zachować informacje, które mogą być przydatne podczas debugowania, należy ustawić właściwość **debugSetting** w wezwaniu JSON, aby **requestContent** i/lub **responseContent**. 

        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",      
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }

    Domyślnie wartość **debugSetting** jest ustawiona na **Brak**. Określając wartość **debugSetting** , więc rozważyć typ danych, który jest przesyłany w podczas wdrażania. Przez rejestrowanie informacji na temat żądania lub odpowiedzi może być ujawnienie danych poufnych, które są pobierane za pośrednictwem operacji rozmieszczania. 

2. Uzyskiwanie informacji na temat wdrażania w działaniu [Uzyskiwanie informacji na temat wdrażania szablonu](https://msdn.microsoft.com/library/azure/dn790565.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}

    W odpowiedzi zwróć uwagę w szczególności elementy **provisioningState** , **correlationId** i **błędów** . **CorrelationId** służy do śledzenia powiązane z nimi zdarzenia, a może być pomocne podczas pracy z pomocą techniczną w celu rozwiązania problemu.
    
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }

3. Uzyskiwanie informacji na temat operacji rozmieszczania w działaniu [wszystkie operacje wdrożenia szablon listy](https://msdn.microsoft.com/library/azure/dn790518.aspx) . 

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}

    Odpowiedź będzie zawierać informacje żądania i/lub odpowiedź w oparciu o co określonej w polu właściwości **debugSetting** podczas wdrażania.
    
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }

4. Pobieranie zdarzeń z dzienników inspekcji do wdrożenia w działaniu [listy zdarzeń zarządzania w subskrypcji](https://msdn.microsoft.com/library/azure/dn931934.aspx) .

        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać pomoc dotyczącą usuwania błędów danego wdrożenia zobacz [Usuwanie typowych błędów podczas wdrażania zasobów Azure przy użyciu Menedżera zasobów Azure](resource-manager-common-deployment-errors.md).
- Aby uzyskać informacje o za pomocą dzienników inspekcji monitorowanie innych działań, zobacz [Inspekcja operacji przy użyciu Menedżera zasobów](resource-group-audit.md).
- Aby sprawdzić poprawność wdrożenia przed jego wykonaniem, zobacz temat [Deploy grupa zasobów za pomocą szablonu Azure Menedżera zasobów](resource-group-template-deploy.md).
