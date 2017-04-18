<properties
    pageTitle="Wysyłanie wiadomości chmury do urządzenia z Centrum IoT | Microsoft Azure"
    description="Postępuj zgodnie z tego samouczka, aby dowiedzieć się, jak wysyłać wiadomości chmury do urządzenia za pomocą Centrum IoT Azure C#."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>Samouczek: Jak można wysyłać chmury do urządzenia z Centrum IoT i .net

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Wprowadzenie

Azure koncentratora IoT jest w pełni zarządzanych usług, który ułatwia włączanie i niezawodności komunikacji dwukierunkowego między miliony IoT urządzeń i zakończyć aplikację ponownie. [Wprowadzenie do Centrum IoT] samouczek pokazano, jak tworzyć Centrum IoT, obsługi administracyjnej urządzenia tożsamości w nim i kod symulowany urządzenie, które wysyła wiadomości urządzenia w chmurze.

Ten samouczek opiera się na [Wprowadzenie do Centrum IoT]. Jak przedstawiono do:

- Z Twojej aplikacji chmury wewnętrznej wysyłanie wiadomości chmury do urządzenia do jednego urządzenia za pośrednictwem Centrum IoT.
- Odbieranie wiadomości chmury do urządzenia na urządzeniu.
- Z chmury usługi aplikacji zakończenia, należy zażądać potwierdzenia dostarczenia (*Opinia*) dla wiadomości wysyłane do urządzenia z Centrum IoT.

Więcej informacji na temat wiadomości w chmurze do urządzenia można znaleźć w [IoT Centrum deweloperów przewodnik][IoT Hub Developer Guide - C2D].

Na końcu tego samouczka spowoduje uruchomienie dwóch aplikacji konsoli systemu Windows:

* **SimulatedDevice**, zmodyfikowaną wersję aplikacji utworzony w [Wprowadzenie do Centrum IoT], który łączy się z Centrum IoT i odbiera wiadomości w chmurze do urządzenia.
* **SendCloudToDevice**, które wysyła wiadomość chmury do urządzenia do symulowany urządzenia za pomocą Centrum IoT, a następnie otrzymuje jego potwierdzenia dostarczenia.

> [AZURE.NOTE] Centrum IoT obsługuje SDK dla wielu platformy urządzeń i języki (w tym C, Java i Javascript) SDK urządzenia Azure IoT. Aby uzyskać instrukcje krok po kroku dotyczące nawiązywania połączenia między urządzeniem kod tego samouczka i ogólnie do koncentratora IoT Azure, zobacz [Centrum deweloperów IoT Azure].

Aby użyć tego samouczka, musisz następujące czynności:

+ Program Microsoft Visual Studio 2015 r.

+ Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

## <a name="receive-messages-on-the-simulated-device"></a>Odbieranie wiadomości na tym urządzeniu symulowany

W tej sekcji będzie zmodyfikować aplikację symulacji utworzone w [Wprowadzenie do Centrum IoT] otrzymywać wiadomości w chmurze do urządzenia z poziomu Centrum IoT.

1. W programie Visual Studio w programie project **SimulatedDevice** , Dodaj następujące metody klasy **programu** .

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    `ReceiveAsync` Metoda asynchroniczne zwraca otrzymanej wiadomości w chwili otrzymania przez urządzenie. Zwraca *wartość null* specifiable limit czasu, po (w tym przypadku używana jest wartość domyślna minutę). W takim przypadku kod należy kontynuować oczekiwanie na nowe wiadomości. To jest przyczyna `if (receivedMessage == null) continue` linii.

    Połączenie `CompleteAsync()` powiadamia Centrum IoT pomyślnie przetworzyć wiadomości. Wiadomość można bezpiecznie usunąć z niej urządzenia. Jeśli coś się stało z którego uniemożliwia zakończenie przetwarzania wiadomości aplikację urządzenia koncentratora IoT udostępnia go ponownie. Ważne jest, następnie konieczności logiki przetwarzania wiadomości w aplikacji urządzenia *idempotent*tak, aby otrzymywania tego samego komunikatu wielokrotnie daje taki sam wynik. Aplikacji można również tymczasowo wstrzymać wiadomość, co spowoduje Centrum IoT zachowywania wiadomości w kolejce zużycia przyszłych. Lub aplikacji można odrzucić wiadomość, którą trwale usuwa wiadomość z kolejki. Aby uzyskać więcej informacji na temat cyklu życia wiadomości w chmurze do urządzenia, zobacz [IoT Centrum deweloperów przewodnik][IoT Hub Developer Guide - C2D].

    > [AZURE.NOTE] W przypadku używania protokołu HTTP zamiast MQTT lub AMQP jako transportu `ReceiveAsync` metoda zwraca natychmiast. Obsługiwane wzorzec chmury do urządzenia wiadomości przy użyciu protokołu HTTP jest okresowo podłączone urządzenia, które sprawdzają dla wiadomości rzadko (mniej niż co 25 minut). Wydawania więcej HTTP otrzymuje wyniki w Centrum IoT ograniczania żądania. Aby uzyskać więcej informacji o różnicach między MQTT, AMQP i HTTP pomocy technicznej i ograniczania Centrum IoT, zobacz [IoT Centrum deweloperów przewodnik][IoT Hub Developer Guide - C2D].

2. Dodaj następujące metody w metodzie **główne** tuż przed `Console.ReadLine()` wiersza:

        ReceiveC2dAsync();

> [AZURE.NOTE] Sake i uproszczenia tego samouczka nie zaimplementować wszelkie zasady ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład wykładniczego wycofywania), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów].

## <a name="send-a-cloud-to-device-message"></a>Wysyłanie wiadomości chmury do urządzenia

W tej sekcji będzie napisz aplikację konsoli systemu Windows, która wysyła wiadomości w chmurze do urządzenia do aplikacji symulacji.

1. W bieżącym rozwiązaniu Visual Studio należy utworzyć nowy projekt Visual C# pulpitu aplikacji przy użyciu **Aplikacji konsoli** szablon projektu. Nazwa projektu **SendCloudToDevice**.

    ![Nowy projekt w programie Visual Studio][20]

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij **Zarządzanie pakietów NuGet... rozwiązania**. 

    Spowoduje to otwarcie okna **Zarządzanie pakietów NuGet** .

3. Wyszukiwanie `Microsoft Azure Devices`, kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania. 

    Pliki do pobrania, instaluje i dodaje odwołanie do [IoT Azure - pakiet NuGet SDK usługi].

4. Dodaj następujący `using` instrukcji u góry pliku **Plik Program.cs** :

        using Microsoft.Azure.Devices;

5. Dodawanie pola do klasy **programu** . Podstawiania wartości symbol zastępczy przy użyciu parametrów połączenia koncentratora IoT z [Wprowadzenie do Centrum IoT]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. Dodaj następujące metody klasy **programu** :

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    Ta metoda wysyła nową wiadomość w chmurze do urządzenia do urządzenia z Identyfikatorem, `myFirstDevice`. W związku z tym zmienić ten parametr w przypadku zmodyfikowano z używaną w [Wprowadzenie do Centrum IoT].

7. Na koniec dodać kolejne wiersze do metody **główne** :

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. W programie Visual Studio, kliknij prawym przyciskiem myszy rozwiązanie i wybierz polecenie **Ustaw uruchamiania projektów...**. Zaznacz **wiele projektów uruchamiania**, a następnie wybierz pozycję **Rozpoczęcie** akcji dla **ProcessDeviceToCloudMessages**, **SimulatedDevice**i **SendCloudToDevice**.

9.  Naciśnij klawisz **F5**. Nazwa powinna rozpoczynać się wszystkie trzy aplikacje. Zaznacz **SendCloudToDevice** systemu windows, a następnie naciśnij klawisz **Enter**. Powinien zostać wyświetlony komunikat za pomocą aplikacji symulowany.

    ![Aplikacja odbierania wiadomości][21]

## <a name="receive-delivery-feedback"></a>Pozwala uzyskiwać opinie dostarczenia
Istnieje możliwość do żądania potwierdzenia dostarczenia (lub wygaśnięcia) z Centrum IoT dla każdej wiadomości w chmurze do urządzenia. Dzięki temu chmury wewnętrznej łatwo poinformować logiczny ponów próbę lub rekompensaty. Aby uzyskać więcej informacji o opinię w chmurze do urządzenia, zobacz [IoT Centrum deweloperów przewodnik][IoT Hub Developer Guide - C2D].

W tej sekcji należy zmodyfikować aplikację **SendCloudToDevice** , aby zażądać opinii i odbierać go z Centrum IoT.

1. W programie Visual Studio w programie project **SendCloudToDevice** , Dodaj następujące metody klasy **programu** .
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    Należy zauważyć, że wzór Odbierz ten sam używany do odbierania wiadomości w chmurze do urządzenia z aplikacji urządzenia.

2. Dodaj następujące metody w metodzie **główne** zaraz po `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` wiersza:

        ReceiveFeedbackAsync();

3. Aby zażądać opinii na dostarczenie wiadomości chmury do urządzenia, musisz określić właściwości w metodzie **SendCloudToDeviceMessageAsync** . Dodaj następujący wiersz, bezpośrednio po `var commandMessage = new Message(...);` wiersza:

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  Uruchamianie aplikacji, naciskając klawisz **F5**. Powinny być widoczne wszystkie trzy aplikacje start. Zaznacz **SendCloudToDevice** systemu windows, a następnie naciśnij klawisz **Enter**. Powinien być widoczny jest komunikat symulowany aplikacji, a po kilku sekundach opinii odbierane przez aplikację **SendCloudToDevice** wiadomości.

    ![Aplikacja odbierania wiadomości][22]

> [AZURE.NOTE] Sake i uproszczenia tego samouczka nie zaimplementować wszelkie zasady ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład wykładniczego wycofywania), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów].

## <a name="next-steps"></a>Następne kroki

W tym samouczku wiesz, jak wysyłać i odbierać wiadomości w chmurze do urządzenia. 

Aby wyświetlić przykłady pełną rozwiązań zakończenia do końca korzystające z Centrum IoT, zobacz [Pakiet IoT Azure].

Aby dowiedzieć się więcej na temat tworzenia rozwiązań z Centrum IoT, zobacz [Przewodnik dewelopera Centrum IoT].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT — pakiet NuGet SDK usługi]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Obsługa przejściowych błędów]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[Centrum IoT przewodnik dewelopera]: iot-hub-devguide.md
[Wprowadzenie do Centrum IoT]: iot-hub-csharp-csharp-getstarted.md
[Centrum deweloperów IoT Azure]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Pakiet Azure IoT]: https://azure.microsoft.com/documentation/suites/iot-suite/