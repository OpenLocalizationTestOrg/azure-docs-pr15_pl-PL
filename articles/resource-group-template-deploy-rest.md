<properties
   pageTitle="Wdrażanie zasobów za pomocą interfejsu API usługi REST i szablonu | Microsoft Azure"
   description="Wdrażanie zasobów Azure za pomocą Menedżera zasobów Azure i interfejsu API usługi REST Menedżera zasobów. Zasoby są definiowane w szablonie Menedżera zasobów."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Wdrażanie zasobów z szablonami Menedżera zasobów i interfejsu API usługi REST Menedżera zasobów

> [AZURE.SELECTOR]
- [Programu PowerShell](resource-group-template-deploy.md)
- [Polecenie Azure](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [INTERFEJSU API USŁUGI REST](resource-group-template-deploy-rest.md)

W tym artykule wyjaśniono, jak za pomocą interfejsu API usługi REST Menedżera zasobów z szablonami Menedżera zasobów wdrażanie zasobów Azure.  

> [AZURE.TIP] Aby uzyskać pomoc dotyczącą debugowania komunikat o błędzie podczas wdrażania zobacz:
>
> - [Wyświetlanie operacji wdrażania z interfejsu API usługi REST](resource-manager-troubleshoot-deployments-rest.md) , aby informacje o wprowadzenie informacji, które ułatwia rozwiązywanie problemów z błędu
> - [Rozwiązywanie typowych błędów podczas wdrażania zasobów Azure przy użyciu Menedżera zasobów Azure](resource-manager-common-deployment-errors.md) , aby dowiedzieć się, jak rozwiązać typowe błędy wdrażania

Szablon może być plik lokalny lub zewnętrznego pliku, który jest dostępny za pomocą identyfikatora URI. Gdy szablon znajduje się na koncie miejsca do magazynowania, można ograniczyć dostęp do szablonu i udostępniać token podpisu (SA) udostępnienia podczas wdrażania.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Rozmieszczanie za pomocą interfejsu API usługi REST
1. Ustawianie [typowych parametrów i nagłówki](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), w tym tokenów uwierzytelniania.
2. Jeśli nie masz istniejącej grupy zasobów, należy utworzyć grupy zasobów. Podaj swój identyfikator subskrypcji, nazwę nowej grupy zasobów i lokalizacji, w której potrzebne do tego rozwiązania. Aby uzyskać więcej informacji zobacz [Tworzenie grupy zasobów](https://msdn.microsoft.com/library/azure/dn790525.aspx).

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. Sprawdź poprawność wdrożenia przed wykonaniem go uruchomić operację [sprawdzania poprawności rozmieszczania szablonu](https://msdn.microsoft.com/library/azure/dn790547.aspx) . Podczas testowania wdrożenia, należy podać parametry dokładnie tak jak podczas wykonywania wdrożenia (jak pokazano w następnym kroku).

3. Tworzenie wdrożeniu. Podaj swój identyfikator subskrypcji nazwę grupy zasobów, aby wdrożyć, nazwę rozmieszczenia i łącza do szablonu. Aby uzyskać informacje o pliku szablonu zobacz [plik parametru](#parameter-file). Aby uzyskać więcej informacji na temat interfejsu API usługi REST, aby utworzyć grupę zasobów zobacz [Tworzenie rozmieszczania szablonu](https://msdn.microsoft.com/library/azure/dn790564.aspx). Zwróć uwagę, że **Tryb** jest ustawiony na **przyrostowe**. Aby uruchomić pełną wdrożenia, ustaw **Tryb** **wykonane**. Uważaj, aby podczas korzystania z trybu wykonane, zgodnie z przypadkowego usunięcia zasoby, które nie są w szablonie.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
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
              }
            }
          }
   
      Jeśli chcesz rejestrować zawartości odpowiedzi, żądania i/lub zawartość, zawierać **debugSetting** w wezwaniu.

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      Można ustawić konta miejsca do magazynowania za pomocą udostępniania tokenu podpisu (SA). Aby uzyskać więcej informacji zobacz [Delegowanie dostępu przy użyciu podpisu dostępu udostępnione](https://msdn.microsoft.com/library/ee395415.aspx).

4. Pobieranie stanu rozmieszczania szablonu. Aby uzyskać więcej informacji zobacz [Uzyskiwanie informacji na temat wdrażania szablonu](https://msdn.microsoft.com/library/azure/dn790565.aspx).

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Następne kroki
- Na przykład wdrażania zasobów za pośrednictwem biblioteki .NET klienta zobacz [zasoby rozmieszczanie przy użyciu bibliotek .NET i szablonu](virtual-machines/virtual-machines-windows-csharp-template.md).
- Aby określić parametry w szablonie, zobacz [Narzędzia do tworzenia szablonów](resource-group-authoring-templates.md#parameters).
- Aby uzyskać wskazówki na temat wdrażania rozwiązania w różnych środowiskach zobacz [rozwoju i środowiskach testowych platformy Microsoft Azure](solution-dev-test-environments.md).
- Aby uzyskać szczegółowe informacje o korzystaniu z odwołaniem KeyVault do przekazania bezpiecznego wartości zobacz [przekazania bezpiecznego wartości podczas wdrażania](resource-manager-keyvault-parameter.md).
