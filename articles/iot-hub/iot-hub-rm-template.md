<properties
    pageTitle="Tworzenie Centrum IoT przy użyciu szablonu ARM i C# | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć tworzenie Centrum IoT z programem C# za pomocą Menedżera zasobów Azure szablonów."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-a-c-program-with-an-azure-resource-manager-template"></a>Tworzenie Centrum IoT przy użyciu programu C# za pomocą szablonu Azure Menedżera zasobów

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Wprowadzenie

Menedżer zasobów Azure umożliwia tworzenie i zarządzanie nimi koncentratory Azure IoT programowy. Ten samouczek pokazano, jak utworzyć koncentratora IoT w programie C# przy użyciu szablonu Azure Menedżera zasobów.

> [AZURE.NOTE] Azure występują dwa modele rozmieszczania służące do tworzenia i pracy z zasobami: [Menedżer zasobów Azure i klasyczny](../resource-manager-deployment-model.md).  W tym artykule opisano, jak przy użyciu modelu wdrożenia Azure Menedżera zasobów.

Aby użyć tego samouczka, są potrzebne następujące elementy:

- Microsoft Visual Studio 2015 r.
- Konto Azure active. <br/>Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.
- [Magazyn Azure konta] [ lnk-storage-account] miejsce, w którym można przechowywać pliki szablonu Azure Menedżera zasobów.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] lub nowszym.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Przygotowywanie projektu programu Visual Studio

1. W programie Visual Studio Utwórz projekt Visual C# systemu Windows za pomocą **Aplikacji konsoli** szablon projektu. Nazwa projektu **CreateIoTHub**.

2. W oknie Eksplorator rozwiązań kliknij prawym przyciskiem myszy nad projektem, a następnie kliknij przycisk **Zarządzaj pakietów NuGet**.

3. W Menedżerze pakiet Nuget Sprawdź **obejmują wstępną**i wyszukiwanie **Microsoft.Azure.Management.ResourceManager**. Kliknij przycisk **Zainstaluj**, w **Przejrzyj zmiany** kliknij **przycisk OK**, a następnie kliknij pozycję **akceptuję** aby zaakceptować licencji.

4. W Menedżerze pakiet Nuget Wyszukaj **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Kliknij przycisk **Zainstaluj**, w **Przejrzyj zmiany** kliknij **przycisk OK**, a następnie kliknij pozycję **akceptuję** aby zaakceptować licencji.

5. W plik Program.cs Zastąp istniejące instrukcji **za pomocą** następujących czynności:

    ```
    using System;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
    
6. W plik Program.cs Dodaj następujące zmienne statyczne zamienianiu wartości symbolu zastępczego. Zanotuj **Identyfikator aplikacji**, **SubscriptionId**, **TenantId**i **hasło** zostały wcześniej w tym samouczku. **Nazwę konta magazynu platformy Azure swoją** nazwę konta magazynu platformy Azure służy do przechowywania plików szablonu Azure Menedżera zasobów. **Nazwa grupy zasobów** jest nazwą grupy zasobów, których można używać podczas tworzenia Centrum IoT, może to być wstępnie istniejącej grupy zasobów lub nową. **Nazwa wdrożenia** jest nazwą wdrożenia, takich jak **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    static string storageAddress = "https://{Your storage account name}.blob.core.windows.net";
    static string rgName = "{Resource group name}";
    static string deploymentName = "{Deployment name}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="submit-an-azure-resource-manager-template-to-create-an-iot-hub"></a>Przesyłanie szablon Azure Menedżera zasobów, aby utworzyć koncentratora IoT

Plik szablonu i parametr JSON umożliwia utworzenie Centrum IoT w grupie zasobów. Za pomocą szablonu Azure Menedżera zasobów do wprowadzania zmian w istniejących Centrum IoT.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nad projektem, kliknij przycisk **Dodaj**, a następnie kliknij **Nowy element**. Dodawanie pliku JSON o nazwie **template.json** do projektu.

2. Zamień zawartość **template.json** definicję zasobów dodać standardowe Centrum IoT do regionu **Wschodniego USA** (dla bieżącej listy regionów, które obsługują Centrum IoT sprawdzać [Azure Status][lnk-status]):

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

3. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nad projektem, kliknij przycisk **Dodaj**, a następnie kliknij **Nowy element**. Dodawanie pliku JSON o nazwie **parameters.json** do projektu.

4. Zamień zawartość **parameters.json** następujące informacje na temat parametrów takich jak ustawia nazwę dla nowego koncentratora IoT **{swoje inicjały} mynewiothub**. Należy zauważyć, że nazwa Centrum IoT musi być globalnie unikatowe, powinien zawierać swoje nazwisko lub inicjały):

    ```
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "hubName": { "value": "mynewiothub" }
      }
    }
    ```

5. W **Eksploratorze serwera**Podłącz do subskrypcji usługi Azure, a w magazynie Azure konta Tworzenie kontenera o nazwie **szablonów**. W panelu **Właściwości** Ustaw uprawnienia **publicznej dostęp do odczytu** dla kontenera **Szablony** do **obiektów Blob**.

6. W **Eksploratorze serwera**kliknij prawym przyciskiem myszy kontener **Szablony** , a następnie kliknij **Kontener obiektów Blob widoku**. Kliknij przycisk **Przekaż obiektów Blob** , wybierz dwa pliki, **parameters.json** i **templates.json**, a następnie kliknij **Otwórz** przekazywać pliki JSON do kontenera **szablonów** . Adresy URL blob zawierający dane JSON są:

    ```
    https://{Your storage account name}.blob.core.windows.net/templates/parameters.json
    https://{Your storage account name}.blob.core.windows.net/templates/template.json
    ```

7. Dodać do plik Program.cs metodę następujące czynności:
    
    ```
    static void CreateIoTHub(ResourceManagementClient client)
    {
        
    }
    ```

5. Dodaj następujący kod do metody **CreateIoTHub** pliki szablonu i parametrów do Menedżera zasobów Azure:

    ```
    var createResponse = client.Deployments.CreateOrUpdate(
        rgName,
        deploymentName,
        new Deployment()
        {
          Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            TemplateLink = new TemplateLink
            {
              Uri = storageAddress + "/templates/template.json"
            },
            ParametersLink = new ParametersLink
            {
              Uri = storageAddress + "/templates/parameters.json"
            }
          }
        });
    ```

6. Dodaj następujący kod do metody **CreateIoTHub** , która jest wyświetlany stan i klucze dla nowego koncentratora IoT:

    ```
    string state = createResponse.Properties.ProvisioningState;
    Console.WriteLine("Deployment state: {0}", state);

    if (state != "Succeeded")
    {
      Console.WriteLine("Failed to create iothub");
    }
    Console.WriteLine(createResponse.Properties.Outputs);
    ```

## <a name="complete-and-run-the-application"></a>Kończenie i uruchamiania aplikacji

Możesz teraz ukończyć aplikacji przez wywołanie metody **CreateIoTHub** przed budowanie i uruchom go.

1. Dodaj następujący kod do końca metody **główne** :

    ```
    CreateIoTHub(client);
    Console.ReadLine();
    ```
    
2. Kliknij przycisk **Konstruuj** , a następnie **budowania rozwiązań**. Popraw błędy.

3. Kliknij pozycję **Debugowanie** i **Rozpocznij debugowanie** , aby uruchomić aplikację. Może potrwać kilka minut do wdrożenia uruchomić.

4. Możesz sprawdzić aplikacji dodania nowego koncentratora IoT odwiedzić [Azure portal] [ lnk-azure-portal] i wyświetlania listy zasobów lub przy użyciu polecenia cmdlet **Get-AzureRmResource** programu PowerShell.

> [AZURE.NOTE] Ten przykład aplikacja dodaje Centrum IoT standardowe S1 którego konta. Możesz usunąć Centrum IoT przez [Azure portal] [ lnk-azure-portal] lub przy użyciu polecenia cmdlet programu PowerShell **AzureRmResource Usuń** po zakończeniu.

## <a name="next-steps"></a>Następne kroki

Teraz wdrożono koncentratora IoT przy użyciu szablonu Menedżera zasobów Azure za pomocą programu C# można eksplorować dodatkowo:

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
[lnk-storage-account]: ../storage/storage-create-storage-account.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
