<properties
    pageTitle="Tworzenie Centrum IoT za pomocą interfejsu API usługi REST | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć tworzenie Centrum IoT za pomocą interfejsu API usługi REST."
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

# <a name="tutorial-create-an-iot-hub-using-a-c-program-and-the-rest-api"></a>Samouczek: Tworzenie Centrum IoT programu C# i interfejsu API usługi REST

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Wprowadzenie

Można korzystać z [dostawcy zasobów Centrum IoT interfejsu API usługi REST] [ lnk-rest-api] tworzyć i zarządzać nimi koncentratory Azure IoT programowy. Ten samouczek pokazano, jak utworzyć koncentratora IoT w programie C# za pomocą Centrum IoT dostawcy zasobów interfejsu API usługi REST.

> [AZURE.NOTE] Azure występują dwa modele rozmieszczania służące do tworzenia i pracy z zasobami: [Menedżer zasobów Azure i klasyczny](../resource-manager-deployment-model.md).  W tym artykule opisano, jak przy użyciu modelu wdrożenia Azure Menedżera zasobów.

Aby użyć tego samouczka, są potrzebne następujące elementy:

- Microsoft Visual Studio 2015 r.
- Konto Azure active. <br/>Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.
- [Microsoft Azure PowerShell 1.0] [ lnk-powershell-install] lub nowszym.

[AZURE.INCLUDE [iot-hub-prepare-resource-manager](../../includes/iot-hub-prepare-resource-manager.md)]

## <a name="prepare-your-visual-studio-project"></a>Przygotowywanie projektu programu Visual Studio

1. W programie Visual Studio Utwórz projekt Visual C# systemu Windows za pomocą **Aplikacji konsoli** szablon projektu. Nazwa projektu **CreateIoTHubREST**.

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nad projektem, a następnie kliknij przycisk **Zarządzaj pakietów NuGet**.

3. W Menedżerze pakiet Nuget Sprawdź **obejmują wstępną**i wyszukiwanie **Microsoft.Azure.Management.ResourceManager**. Kliknij przycisk **Zainstaluj**, w **Przejrzyj zmiany** kliknij **przycisk OK**, a następnie kliknij pozycję **akceptuję** aby zaakceptować licencji.

4. W Menedżerze pakiet Nuget Wyszukaj **Microsoft.IdentityModel.Clients.ActiveDirectory**.  Kliknij przycisk **Zainstaluj**, w **Przejrzyj zmiany** kliknij **przycisk OK**, a następnie kliknij pozycję **akceptuję** aby zaakceptować licencji.

6. W plik Program.cs Zastąp istniejące instrukcji **za pomocą** następujących czynności:

    ```
    using System;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    using Microsoft.Rest;
    using System.Linq;
    using System.Threading;
    ```
    
7. W plik Program.cs Dodaj następujące zmienne statyczne zamienianiu wartości symbolu zastępczego. Zanotuj **Identyfikator aplikacji**, **SubscriptionId**, **TenantId**i **hasło** zostały wcześniej w tym samouczku. **Nazwa grupy zasobów** jest nazwą grupy zasobów, których można używać podczas tworzenia Centrum IoT, może to być wstępnie istniejącej grupy zasobów lub nową. **Nazwa centrum IoT** to nazwa Centrum IoT można tworzyć, takich jak **MyIoTHub** (Ta nazwa musi być w globalnie unikatowe, więc powinien zawierać swoje nazwisko lub inicjały). **Nazwa wdrożenia** jest nazwą wdrożenia, takich jak **Deployment_01**.

    ```
    static string applicationId = "{Your ApplicationId}";
    static string subscriptionId = "{Your SubscriptionId}";
    static string tenantId = "{Your TenantId}";
    static string password = "{Your application Password}";
    
    static string rgName = "{Resource group name}";
    static string iotHubName = "{IoT Hub name including your initials}";
    ```

[AZURE.INCLUDE [iot-hub-get-access-token](../../includes/iot-hub-get-access-token.md)]

## <a name="use-the-rest-api-to-create-an-iot-hub"></a>Tworzenie Centrum IoT przy użyciu interfejsu API usługi REST

Za pomocą [Interfejsu API usługi REST Centrum IoT] [ lnk-rest-api] Tworzenie Centrum IoT w swojej grupy zasobów. Za pomocą interfejsu API usługi REST do wprowadzania zmian w istniejących Centrum IoT.

1. Dodać do plik Program.cs metodę następujące czynności:
    
    ```
    static void CreateIoTHub(string token)
    {
        
    }
    ```

2. Dodaj następujący kod do metody **CreateIoTHub** , aby utworzyć obiekt **HttpClient** z token uwierzytelniania w nagłówkach:

    ```
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
    ```

3. Dodaj następujący kod do metody **CreateIoTHub** celu opisania Centrum IoT do tworzenia i generowanie postać JSON (dla bieżącej listy lokalizacji, które obsługują Centrum IoT sprawdzać [Azure Status][lnk-status]):

    ```
    var description = new
    {
      name = iotHubName,
      location = "East US",
      sku = new
      {
        name = "S1",
        tier = "Standard",
        capacity = 1
      }
    };
    
    var json = JsonConvert.SerializeObject(description, Formatting.Indented);
    ```

4. Dodaj następujący kod do metody **CreateIoTHub** przesłaniu żądania pozostałych Azure, Sprawdź odpowiedź i pobrać adresu URL można monitorować stan zadania wdrażania:

    ```
    var content = new StringContent(JsonConvert.SerializeObject(description), Encoding.UTF8, "application/json");
    var requestUri = string.Format("https://management.azure.com/subscriptions/{0}/resourcegroups/{1}/providers/Microsoft.devices/IotHubs/{2}?api-version=2016-02-03", subscriptionId, rgName, iotHubName);
    var result = client.PutAsync(requestUri, content).Result;
      
    if (!result.IsSuccessStatusCode)
    {
      Console.WriteLine("Failed {0}", result.Content.ReadAsStringAsync().Result);
      return;
    }
    
    var asyncStatusUri = result.Headers.GetValues("Azure-AsyncOperation").First();
    ```

5. Dodaj następujący kod do końca metody **CreateIoTHub** do korzystania z adresu **asyncStatusUri** pobrane w poprzednim kroku czekać do wdrożenia do wykonania:

    ```
    string body;
    do
    {
      Thread.Sleep(10000);
      HttpResponseMessage deploymentstatus = client.GetAsync(asyncStatusUri).Result;
      body = deploymentstatus.Content.ReadAsStringAsync().Result;
    } while (body == "{\"status\":\"Running\"}");
    ```

6. Dodaj następujący kod do końca metody **CreateIoTHub** do pobierania kluczy Centrum IoT została utworzona i wydrukować je do konsoli:

    ```
    var listKeysUri = string.Format("https://management.azure.com/subscriptions/{0}/resourceGroups/{1}/providers/Microsoft.Devices/IotHubs/{2}/IoTHubKeys/listkeys?api-version=2015-08-15-preview", subscriptionId, rgName, iotHubName);
    var keysresults = client.PostAsync(listKeysUri, null).Result;
    
    Console.WriteLine("Keys: {0}", keysresults.Content.ReadAsStringAsync().Result);
    ```
    
## <a name="complete-and-run-the-application"></a>Kończenie i uruchamiania aplikacji

Możesz teraz ukończyć aplikacji przez wywołanie metody **CreateIoTHub** przed budowanie i uruchom go.

1. Dodaj następujący kod do końca metody **główne** :

    ```
    CreateIoTHub(token.AccessToken);
    Console.ReadLine();
    ```
    
2. Kliknij przycisk **Konstruuj** , a następnie **budowania rozwiązań**. Popraw błędy.

3. Kliknij pozycję **Debugowanie** i **Rozpocznij debugowanie** , aby uruchomić aplikację. Może potrwać kilka minut do wdrożenia uruchomić.

4. Możesz sprawdzić aplikacji dodania nowego koncentratora IoT odwiedzić [Azure portal] [ lnk-azure-portal] i wyświetlania listy zasobów lub przy użyciu polecenia cmdlet **Get-AzureRmResource** programu PowerShell.

> [AZURE.NOTE] Ten przykład aplikacja dodaje Centrum IoT standardowe S1 którego konta. Po zakończeniu, możesz usunąć Centrum IoT przez [Azure portal] [ lnk-azure-portal] lub przy użyciu polecenia cmdlet programu PowerShell **AzureRmResource Usuń** po zakończeniu.

## <a name="next-steps"></a>Następne kroki

Teraz wdrożono koncentratora IoT za pomocą interfejsu API usługi REST, można eksplorować dodatkowo:

- Przeczytaj o możliwości [IoT Centrum zasobów dostawcy interfejsu API usługi REST][lnk-rest-api].
- Przeczytaj [Omówienie Menedżera zasobów Azure] [ lnk-azure-rm-overview] Aby dowiedzieć się więcej o funkcjach Menedżera zasobów Azure.

Aby dowiedzieć się więcej o tworzeniu IoT koncentratora, zobacz następujące artykuły:

- [Wprowadzenie do C SDK][lnk-c-sdk]
- [Centrum IoT SDK][lnk-sdks]

Aby dodatkowo Poznawanie możliwości Centrum IoT, zobacz:

- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-powershell-install]: ../powershell-install-configure.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-azure-rm-overview]: ../azure-resource-manager/resource-group-overview.md

[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
