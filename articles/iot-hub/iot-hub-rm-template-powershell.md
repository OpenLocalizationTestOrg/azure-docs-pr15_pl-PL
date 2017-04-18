<properties
    pageTitle="Tworzenie Centrum IoT przy użyciu szablonu Menedżera zasobów Azure i programu PowerShell | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć tworzenie Centrum IoT przy użyciu programu PowerShell za pomocą Menedżera zasobów Azure szablonów."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/07/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-powershell"></a>Tworzenie Centrum IoT przy użyciu programu PowerShell

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Wprowadzenie

Menedżer zasobów Azure umożliwia tworzenie i zarządzanie nimi koncentratory Azure IoT programowy. Ten samouczek pokazano, jak za pomocą szablonu Menedżera zasobów Azure Tworzenie Centrum IoT przy użyciu programu PowerShell.

> [AZURE.NOTE] Azure występują dwa modele rozmieszczania służące do tworzenia i pracy z zasobami: [Menedżer zasobów Azure i klasyczny](../resource-manager-deployment-model.md).  W tym artykule opisano, jak przy użyciu modelu wdrożenia Azure Menedżera zasobów.

Aby użyć tego samouczka są potrzebne następujące elementy:

- Konto Azure active. <br/>Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] lub nowszym.

> [AZURE.TIP] Artykuł [Za pomocą programu Azure przy użyciu Menedżera zasobów Azure] [ lnk-powershell-arm] znajdziesz więcej informacji na temat Tworzenie zasobów Azure za pomocą skryptów programu PowerShell i szablony Azure Menedżera zasobów. 

## <a name="connect-to-your-azure-subscription"></a>Nawiązywanie połączenia z subskrypcji usługi Azure

W wierszu polecenia programu PowerShell wpisz następujące polecenie, aby zalogować się do usługi Azure subskrypcji:

```
Login-AzureRmAccount
```

Za pomocą następujących poleceń do znalezienia miejsce, w którym można wdrażać koncentratora IoT i obecnie obsługiwanych wersji interfejsu API:

```
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).Locations
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Devices).ResourceTypes | Where-Object ResourceTypeName -eq IoTHubs).ApiVersions
```

Tworzenie grupy zasobów zawierają Twoim Centrum IoT przy użyciu następującego polecenia w jednej z lokalizacji obsługiwanych IoT koncentratora. W tym przykładzie tworzy grupę zasobów o nazwie **MyIoTRG1**:

```
New-AzureRmResourceGroup -Name MyIoTRG1 -Location "East US"
```

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Przesyłanie szablon Azure Menedżera zasobów, aby utworzyć koncentratora IoT

Umożliwia utworzenie Centrum IoT w grupie zasobów szablonu JSON. Za pomocą szablonu Azure Menedżera zasobów do wprowadzania zmian w istniejących Centrum IoT.

1. Używaj edytora tekstu, aby utworzyć szablon Menedżera zasobów Azure o nazwie **template.json** z definicją następujących zasobów do utworzenia nowego koncentratora IoT standardowy. W tym przykładzie dodaje Centrum IoT regionu **Wschodniego Stanów Zjednoczonych** , utworzy dwie grupy dla klientów indywidualnych (**cg1** i **cg2**) punkt końcowy zgodnego Centrum zdarzeń i korzysta z wersji **2016-02-03** interfejsu API. Ten szablon zakłada kopii zapasowych w polu Nazwa centrum IoT jako parametr o nazwie **hubName**. Na liście bieżącej lokalizacji, które obsługują Centrum IoT sprawdzać [Azure Status][lnk-status].

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": {
          "type": "string"
        }
      },
      "resources": [
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs",
        "name": "[parameters('hubName')]",
        "location": "East US",
        "sku": {
          "name": "S1",
          "tier": "Standard",
          "capacity": 1
        },
        "properties": {
          "location": "East US"
        }
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg1')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      },
      {
        "apiVersion": "2016-02-03",
        "type": "Microsoft.Devices/IotHubs/eventhubEndpoints/ConsumerGroups",
        "name": "[concat(parameters('hubName'), '/events/cg2')]",
        "dependsOn": [
          "[concat('Microsoft.Devices/Iothubs/', parameters('hubName'))]"
        ]
      }
      ],
      "outputs": {
        "hubKeys": {
          "value": "[listKeys(resourceId('Microsoft.Devices/IotHubs', parameters('hubName')), '2016-02-03')]",
          "type": "object"
        }
      }
    }
    ```

2. Zapisz plik szablonu Azure Menedżera zasobów na komputerze lokalnym. W tym przykładzie założono, że możesz zapisać w folderze o nazwie **c:\templates**.

3. Uruchom następujące polecenie wdrożenie do nowego koncentratora IoT, przekazując nazwę Twoim Centrum IoT jako parametr. W tym przykładzie nazwą Centrum IoT jest **abcmyiothub** (należy zauważyć, że ta nazwa musi być globalnie unikatowe, powinien zawierać swoje nazwisko lub inicjały):

    ```
    New-AzureRmResourceGroupDeployment -ResourceGroupName MyIoTRG1 -TemplateFile C:\templates\template.json -hubName abcmyiothub
    ```

4. Dane wyjściowe wyświetla klawisze służące do koncentratora IoT utworzony.

5. Możesz sprawdzić aplikacji dodania nowego koncentratora IoT odwiedzić [Azure portal] [ lnk-azure-portal] i wyświetlania listy zasobów lub przy użyciu polecenia cmdlet **Get-AzureRmResource** programu PowerShell.

> [AZURE.NOTE] Ten przykład aplikacja dodaje Centrum IoT standardowe S1 którego konta. Możesz usunąć Centrum IoT przez [Azure portal] [ lnk-azure-portal] lub przy użyciu polecenia cmdlet programu PowerShell **AzureRmResource Usuń** po zakończeniu.

## <a name="next-steps"></a>Następne kroki

Teraz wdrożono koncentratora IoT szablonu Menedżera zasobów Azure za pomocą programu PowerShell, można eksplorować dodatkowo:

- Przeczytaj o możliwości [IoT Centrum zasobów dostawcy interfejsu API usługi REST][lnk-rest-api].
- Przeczytaj [Omówienie Menedżera zasobów Azure] [ lnk-azure-rm-overview] Aby dowiedzieć się więcej o funkcjach Menedżera zasobów Azure.

Aby dowiedzieć się więcej o tworzeniu IoT koncentratora, zobacz następujące artykuły:

- [Wprowadzenie do C SDK][lnk-c-sdk]
- [Centrum IoT SDK][lnk-sdks]

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md
[lnk-powershell-arm]: ../powershell-azure-resource-manager.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
