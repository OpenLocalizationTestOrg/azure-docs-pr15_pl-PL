<properties
    pageTitle="Azure Centrum IoT wprowadzenie C# | Microsoft Azure"
    description="Samouczek wprowadzający Azure koncentratora IoT za pomocą C# wprowadzenie. Za pomocą Centrum IoT Azure i C# SDK IoT Microsoft Azure do wdrożenia rozwiązanie programu Internet rzeczy."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>Wprowadzenie do Centrum IoT Azure dla środowiska .NET

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na końcu tego samouczka masz trzy aplikacji konsoli systemu Windows:

* **CreateDeviceIdentity**, co spowoduje utworzenie urządzenia tożsamości i skojarzony klucz nawiązywania połączenia między urządzeniem symulowany.
* **ReadDeviceToCloudMessages**, wyświetlającego telemetrycznego wysyłane przez symulowany urządzenia.
* **SimulatedDevice**, który łączy się z Centrum IoT z urządzenia tożsamości utworzony wcześniej i wysyła wiadomości telemetrycznego co sekundę przy użyciu protokołu AMQP.

> [AZURE.NOTE] Uzyskać informacji na temat różnych SDK, których można użyć do utworzenia obu uruchamiania aplikacji na urządzenia i usługi rozwiązanie wewnętrznej, zobacz [SDK Centrum IoT][lnk-hub-sdks].

Aby użyć tego samouczka, są potrzebne następujące elementy:

+ Microsoft Visual Studio 2015 r.

+ Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Teraz utworzono Twoim Centrum IoT, a masz połączenie i hostname ciąg, który należy wykonać podczas przerabiania tego samouczka.

## <a name="create-a-device-identity"></a>Tworzenie urządzenia tożsamości

W tej sekcji możesz utworzyć aplikację konsoli systemu Windows, która tworzy urządzenia tożsamości w rejestrze tożsamości w Twoim Centrum IoT. Urządzenie nie może nawiązać połączenia Centrum IoT, o ile nie zawiera on wpis w rejestrze tożsamości urządzenia. Aby uzyskać więcej informacji, zobacz sekcję "Urządzenia tożsamości rejestru" [IoT Centrum deweloperów przewodnik][lnk-devguide-identity]. Podczas uruchamiania tej aplikacji konsoli generuje unikatowy identyfikator i klucz, który urządzenia służy do identyfikacji podczas wysyłania wiadomości urządzenia w chmurze do koncentratora IoT.

1. W programie Visual Studio Dodaj projekt Visual C# klasyczny komputerze z systemem Windows z bieżącym rozwiązaniem przy użyciu szablonu projektów **Console Application** . Upewnij się, że .NET Framework w wersji jest 4.5.1 lub nowszy. Nazwa projektu **CreateDeviceIdentity**.

    ![Nowy projekt Visual C# klasyczny komputerze z systemem Windows][10]

2. W oknie Eksplorator rozwiązań projektu **CreateDeviceIdentity** kliknij prawym przyciskiem myszy, a następnie kliknij **Zarządzanie pakietów Nuget**.

3. W oknie **Menedżer pakietów Nuget** wybierz pozycję **Przeglądaj**, wyszukaj **microsoft.azure.devices**, wybierz pozycję **Zainstaluj** , aby zainstalować pakiet **Microsoft.Azure.Devices** i zaakceptuj warunki użytkowania. Ta procedura — pliki do pobrania, instaluje i dodaje odwołanie do [Microsoft Azure IoT usługi SDK] [ lnk-nuget-service-sdk] Nuget pakietu i jego zależności.

    ![Okno Menedżera pakietu Nuget][11]

4. Dodaj następujący `using` instrukcji u góry pliku **Plik Program.cs** :

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. Dodawanie pola do klasy **programu** . Zamień wartości symbolu zastępczego parametry połączenia dla Centrum IoT, utworzoną w poprzedniej sekcji.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Dodaj następujące metody klasy **programu** :

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Ta metoda powoduje urządzenia tożsamości z identyfikator **myFirstDevice**. (Jeśli ta nazwa urządzenia już istnieje w rejestrze, kod po prostu pobiera istniejących informacji o urządzeniu.) Aplikacja zostanie wyświetlony klucz podstawowy dla tej tożsamości. Aby nawiązać połączenie z Centrum IoT za pomocą tego klucza w symulowany urządzenia.

7. Na koniec dodać kolejne wiersze do metody **główne** :

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Uruchomienia tej aplikacji i zanotuj klucz urządzenia.

    ![Klucza urządzenia wygenerowane przez aplikację][12]

> [AZURE.NOTE] Centrum IoT rejestru tożsamości są przechowywane tożsamości urządzenia umożliwiające bezpieczny dostęp do Centrum. Przechowuje identyfikatory urządzeń i klawisze służące do pracy jako poświadczeń zabezpieczeń i flagi włączone/wyłączone, którą można wyłączyć dostęp dla każdego urządzenia. Jeśli aplikacja wymaga do przechowywania inne metadane specyficzne dla urządzenia, używaj magazynu specyficzne dla aplikacji. Aby uzyskać więcej informacji, zobacz [Przewodnik dewelopera Centrum IoT][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Odbieranie wiadomości urządzenia w chmurze

W tej sekcji możesz utworzyć aplikację konsoli systemu Windows, który odczytuje urządzenia w chmurze wiadomości z Centrum IoT. Centrum IoT udostępnia [Koncentratory zdarzenia Azure][lnk-event-hubs-overview]-zgodne punktu końcowego, aby możliwe było czytać wiadomości urządzenia w chmurze. Aby zachować proste czynności, ten samouczek tworzy czytnika podstawowe, który nie jest odpowiedni dla wdrożenia wysokiej wydajności. Aby uzyskać sposobu przetwarzania wiadomości urządzenia w chmurze w skali, zobacz [wiadomości urządzenia w chmurze proces] [ lnk-process-d2c-tutorial] samouczka. Uzyskać dalsze informacje dotyczące celu przetwarzania wiadomości z koncentratorów zdarzenia, zobacz [Wprowadzenie koncentratory zdarzenia] [ lnk-eventhubs-tutorial] samouczka. (Ten samouczek ma zastosowanie do punktów końcowych zgodnego Centrum IoT Centrum zdarzeń).

> [AZURE.NOTE] Punkt końcowy zgodnego z programem zdarzenia Centrum zawsze czytania wiadomości urządzenia w chmurze za pośrednictwem protokołu AMQP.

1. W programie Visual Studio Dodaj projekt Visual C# klasyczny komputerze z systemem Windows z bieżącym rozwiązaniem przy użyciu **Aplikacji konsoli** szablon projektu. Upewnij się, że .NET Framework w wersji jest 4.5.1 lub nowszy. Nazwa projektu **ReadDeviceToCloudMessages**.

    ![Nowy projekt Visual C# klasyczny komputerze z systemem Windows][10]

2. W oknie Eksplorator rozwiązań kliknij prawym przyciskiem myszy **ReadDeviceToCloudMessages** projektu, a następnie kliknij **Zarządzanie pakietów Nuget**.

3. W oknie **Menedżer pakietów Nuget** wyszukiwanie **WindowsAzure.ServiceBus**, wybierz pozycję **Zainstaluj**i zaakceptuj warunki użytkowania. Ta procedura — pliki do pobrania, instaluje i dodaje odwołanie do [Bus usługi Azure][lnk-servicebus-nuget], z jego zależności. Ten pakiet umożliwia nawiązywanie połączenia z punktu końcowego zdarzenia zgodnego z Centrum na Twoim Centrum IoT aplikacji.

4. Dodaj następujący `using` instrukcji u góry pliku **Plik Program.cs** :

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. Dodawanie pola do klasy **programu** . Zamień wartości symbolu zastępczego parametry połączenia dla Centrum IoT, utworzony w sekcji "Tworzenie Centrum IoT".

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. Dodaj następujące metody klasy **programu** :

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    Ta metoda używa wystąpienie **EventHubReceiver** do odbierania wiadomości z wszystkich IoT Centrum urządzenia do chmury otrzymywać partycje. Zwróć uwagę, jak przekazać `DateTime.Now` parametru w przypadku tworzenia obiektu **EventHubReceiver** , dzięki czemu tylko odbiera wiadomości wysyłane po jego uruchomieniu. Ten filtr jest używany w środowisku testowym, dzięki czemu można zobaczyć bieżący zestaw wiadomości. W środowisku produkcyjnym kodzie upewnij się, że przetwarza wszystkie wiadomości. Aby uzyskać więcej informacji, zobacz [sposób przetwarzania wiadomości urządzenia w chmurze Centrum IoT] [ lnk-process-d2c-tutorial] samouczka.

7. Na koniec dodać kolejne wiersze do metody **główne** :

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Tworzenie aplikacji symulacji

W tej sekcji możesz utworzyć aplikację konsoli systemu Windows, która symuluje z urządzenia, które wysyła wiadomości urządzenia w chmurze do koncentratora IoT.

1. W programie Visual Studio Dodaj projekt Visual C# klasyczny komputerze z systemem Windows z bieżącym rozwiązaniem przy użyciu **Aplikacji konsoli** szablon projektu. Upewnij się, że .NET Framework w wersji jest 4.5.1 lub nowszy. Nazwa projektu **SimulatedDevice**.

    ![Nowy projekt Visual C# klasyczny komputerze z systemem Windows][10]

2. W oknie Eksplorator rozwiązań kliknij prawym przyciskiem myszy **SimulatedDevice** projektu, a następnie kliknij **Zarządzanie pakietów Nuget**.

3. W oknie **Menedżer pakietów Nuget** wybierz pozycję **Przeglądaj**, wyszukaj **Microsoft.Azure.Devices.Client**, wybierz pozycję **Zainstaluj** , aby zainstalować pakiet **Microsoft.Azure.Devices.Client** i zaakceptuj warunki użytkowania. Tę procedurę do pobrania, instaluje i dodaje odwołanie do [IoT Azure — pakiet urządzenia SDK Nuget] [ lnk-device-nuget] i jego zależności.

4. Dodaj następujący `using` instrukcji u góry pliku **Plik Program.cs** :

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. Dodawanie pola do klasy **programu** . Podstawianie wartości symbolu zastępczego przy użyciu hostname Centrum IoT pobierane w sekcji "Tworzenie Centrum IoT" i klucza urządzenia pobierane w sekcji "Tworzenie urządzenia tożsamości".

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. Dodaj następujące metody klasy **programu** :

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Ta metoda wysyła nową wiadomość urządzenia w chmurze co sekundę. Wiadomość zawiera obiekt seryjnych JSON z identyfikator urządzenia i losowo wygenerowanym numerem w celu zasymulowania czujnika prędkości wiatru.

7. Na koniec dodać kolejne wiersze do metody **główne** :

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  Domyślnie metoda **Create** tworzy wystąpienie **DeviceClient** , która korzysta z protokołu AMQP można komunikować się z Centrum IoT. Aby użyć protokołu HTTP, za pomocą Zastępowanie metody **tworzenia** , który umożliwia określenie protokołu. Jeśli używasz protokołu HTTP, należy również dodać pakietu Nuget **Microsoft.AspNet.WebApi.Client** do projektu do uwzględnienia nazw **System.Net.Http.Formatting** .

Ten samouczek przedstawia kroki w celu utworzenia Centrum IoT urządzenia klienta. Można również użyć [Połączenia usługi Azure IoT Centrum] [ lnk-connected-service] rozszerzenie programu Visual Studio, można dodawać kod niezbędne do aplikacji klienckiej urządzenia.


> [AZURE.NOTE] Aby zachować proste czynności, ten samouczek nie powoduje żadnych zasad ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład wykładniczego wycofywania), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów][lnk-transient-faults].

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1.  W programie Visual Studio, w oknie Eksplorator rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij **uruchamiania zestawu projektów**. Zaznacz **wiele projektów uruchamiania**, a następnie wybierz **Start** , jako akcji dla projektów zarówno **ReadDeviceToCloudMessages** , jak i **SimulatedDevice** .

    ![Właściwości projektu uruchamiania][41]

2.  Naciśnij klawisz **F5** , aby uruchomić obu uruchomione aplikacje. Dane wyjściowe z konsoli z aplikacji **SimulatedDevice** zawiera wiadomości, które symulowany urządzenie wysyła do Twoim Centrum IoT. Dane wyjściowe z konsoli z aplikacji **ReadDeviceToCloudMessages** zawiera wiadomości, które Twoim Centrum IoT odbiera.

    ![Wyjścia konsoli z aplikacji][42]

3. Fragment **zastosowania** w [Azure portal] [ lnk-portal] wyświetlana liczba wiadomości wysłane do niego:

    ![Azure portalu kafelków zastosowania][43]


## <a name="next-steps"></a>Następne kroki

W tym samouczku skonfigurowane koncentratora IoT w portalu Azure, a następnie tworzone urządzenia tożsamości w rejestrze tożsamości koncentratora. Ta tożsamość urządzenia umożliwia włączyć aplikację symulacji wysyłania wiadomości urządzenia w chmurze do koncentratora. Utworzono aplikację, która zawiera wiadomości odebranych przez Centrum. 

Aby kontynuować, wprowadzenie do Centrum IoT i Eksplorowanie innych sytuacjach IoT, zobacz:

- [Łączenie urządzenia][lnk-connect-device]
- [Wprowadzenie do zarządzania urządzeniami][lnk-device-management]
- [Wprowadzenie do programu IoT SDK bramy][lnk-gateway-SDK]

Aby dowiedzieć się, jak rozszerzyć rozwiązania IoT i przetwarzania wiadomości urządzenia w chmurze w skali, zobacz [wiadomości urządzenia w chmurze proces] [ lnk-process-d2c-tutorial] samouczka.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
