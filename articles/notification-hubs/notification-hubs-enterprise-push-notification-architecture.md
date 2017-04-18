<properties
    pageTitle="Powiadomienie o koncentratory — architektura wypychanych przedsiębiorstwa"
    description="Wskazówki na temat korzystania z koncentratorów powiadomienie Azure w środowisku przedsiębiorstwa"
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="enterprise-push-architectural-guidance"></a>Wskazówki architektura wypychanych przedsiębiorstwa

Dzisiaj przedsiębiorstw są stopniowo przenoszony do tworzenia aplikacji dla urządzeń przenośnych w obu użytkownicy końcowi (zewnętrzny) lub dla pracowników (wewnętrzny). Mają istniejących systemów wewnętrznej bazy danych w miejscu komputerów typu mainframe i niektóre aplikacje LoB, które muszą zostać włączone do architektury aplikacji mobilnej. Ten przewodnik będzie porozmawiać na temat jak najlepiej zrobić to zalecenie możliwe rozwiązanie do typowych scenariuszy integracji.

Jest często wymagane do wysyłania powiadomień wypychanych dla użytkowników za pomocą ich aplikacji mobilnej po wystąpieniu zdarzenia miejsc w systemach wewnętrznej bazy danych. Np. kto ma banku bankowych aplikacji na jej iPhone klienta chce otrzymywać powiadomienia, gdy debetowej powyżej pewnego z swojego konta lub scenariusz intranet miejsce, w którym chce otrzymywać powiadomienia o otrzymywać żądania zatwierdzenia pracownika z dziale finansowym, kto ma aplikacji zatwierdzanie budżetu na jego Windows Phone.

Konta bankowego lub przetwarzanie zatwierdzanie prawdopodobnie można wykonywać w niektórych system wewnętrznej bazy danych, który musi zainicjować wypychanych użytkownikowi. Może być wiele takich systemów wewnętrznej bazy danych, które należy utworzyć tego samego rodzaju logiki do wykonania wypychanych, gdy zdarzenie wyzwalane powiadomienie. Złożoność w tym polu znajduje się w integracji kilku systemów wewnętrznej bazy danych razem z systemu pojedynczej wypychanych miejsce, w którym użytkownicy końcowi mogą subskrybowanych różne powiadomienia i może być wiele aplikacji dla urządzeń przenośnych np. w przypadku aplikacji dla urządzeń przenośnych intranet jednej aplikacji mobilnej może miejsce, w którym chcesz otrzymywać powiadomienia z wielu takich systemów wewnętrznej bazy danych. Systemy wewnętrznej bazy danych nie znasz lub wiedzieć wypychanych znaczeń właściwych i technologii, typowych rozwiązań została zazwyczaj wprowadzenie składnik, która sprawdza systemów wewnętrznej bazy danych wszystkie zdarzenia odsetek i jest odpowiedzialny za wysłanie wiadomości wypychanych do klienta.
W tym miejscu będzie porozmawiamy o nawet lepszym rozwiązaniem przy użyciu Bus usługi Azure - tematu-subskrypcji co ograniczy złożoności podczas tworzenia skalowalna rozwiązanie.

Poniżej przedstawiono ogólne architektura rozwiązanie (uogólniony z wielu aplikacji dla urządzeń przenośnych ale również mają zastosowanie, jeśli istnieje tylko jedna aplikacji dla urządzeń przenośnych)

## <a name="architecture"></a>Architektura

![][1]

Kluczowe element w tym diagramów architektury jest Bus usługi Azure, która udostępnia tematy i subskrypcje programowania modelu (więcej informacji na temat go na [programowania Bus usługi Pub-Sub]). Odbiorca, czyli w tym przypadku urządzeń przenośnych (zwykle [Usługi mobilnej Azure], który rozpocznie naciśnij, aby aplikacje mobilne) wewnętrznej bazy danych nie odbiera wiadomości bezpośrednio z systemów wewnętrznej bazy danych, ale zamiast tego mamy warstwę abstrakcji pośredniej dostarczony przez [Bus usługi Azure] , co umożliwia przenośnych wewnętrznej bazy danych otrzymywać wiadomości z jednego lub kilku systemów wewnętrznej bazy danych. Temat Bus usługi musi być utworzony dla każdego systemu wewnętrznej bazy danych np. konta, HR, Finanse, które są zasadniczo "tematów" który rozpocznie wiadomości do wysłania jako powiadomień wypychanych. Systemy wewnętrznej bazy danych będzie wysyłać wiadomości do tych tematów. Co najmniej jeden tematy subskrybować Mobile wewnętrznej bazie danych podczas tworzenia subskrypcji usługi Bus. Spowoduje to uprawnia przenośnych wewnętrznej bazy danych Aby odbierać powiadomienie z odpowiednich systemu wewnętrznej bazy danych. Urządzeń przenośnych wewnętrznej bazy danych będzie nadal występował odsłuchać wiadomości na ich subskrypcje i zaraz po otrzymaniu wiadomości, zmieni się ponownie i przesyła go jako powiadomienie do jego koncentratora powiadomienie. Powiadomienie o koncentratory następnie ostatecznie dostarcza wiadomość do aplikacji dla urządzeń przenośnych. Dlatego do podsumowania kluczowe składniki, mamy:

1. Systemy wewnętrznej bazy danych (systemy LoB-starsza wersja)
    - Tworzy usługi Bus tematu
    - Służy do wysyłania wiadomości
2. Urządzeń przenośnych wewnętrznej bazy danych
    - Tworzy subskrypcja usługi
    - Odbiera wiadomości (z systemu wewnętrznej bazy danych)
    - Wysyła powiadomienie dla klientów (za pośrednictwem Centrum powiadomienie Azure)
3. Aplikacji mobilnej
    - Odbiera i Wyświetl powiadomienie

###<a name="benefits"></a>Korzyści:

1. Rozdzielenie między odbiorcy (aplikacji i usług mobilnych za pośrednictwem Centrum powiadomienie) i nadawcy (systemy wewnętrznej bazy danych) umożliwia systemów dodatkowe wewnętrznej bazy danych jest zintegrowany z minimalnymi zmiany.
2. Spowoduje to scenariusz wielu aplikacji dla urządzeń przenośnych niemożności otrzymywania zdarzeń z jednego lub kilku systemów wewnętrznej bazy danych.  

## <a name="sample"></a>Przykład:

###<a name="prerequisites"></a>Wymagania wstępne
Należy wykonać następujące samouczki, aby zapoznać się z pojęcia, a także tworzenie typowych & czynności konfiguracyjne:

1. [Programowania Bus usługi Pub-Sub] — to praca z tematy Bus usługi i subskrypcje, szczegółowo omówiono sposób tworzenia nazw zawierają tematy i subskrypcje, jak wysyłać i odbierać wiadomości od nich.
2. [Powiadomienie koncentratory - samouczek uniwersalny systemu Windows] — to wyjaśniono, jak zarejestrować, a następnie otrzymywać powiadomienia za pomocą koncentratorów powiadomienie i konfigurowanie aplikacji ze Sklepu Windows.

###<a name="sample-code"></a>Przykładowy kod

Przykładowy pełny kod jest dostępny w [Powiadomień Centrum próbki]. Są one podzielone na trzy elementy:

1. **EnterprisePushBackendSystem**

    . Ten projekt pakietu Nuget *WindowsAzure.ServiceBus* i jest oparty na [usługę Bus Pub-Sub programowania].

    b. Jest to proste C# konsoli aplikacja celu zasymulowania systemu LoB, która inicjuje wiadomości mają być dostarczane do aplikacji dla urządzeń przenośnych.

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c. `CreateTopic`Służy do tworzenia w temacie Bus usługi miejsce, w którym będzie wysyłać wiadomości.

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d. `SendMessage`Służy do wysyłania wiadomości do tego tematu Bus usługi. Tutaj firma Microsoft po prostu wysyłają zestawu losowe wiadomości z tematem okresowo w celu próbki. Zwykle będzie systemu wewnętrznej bazy danych, który będzie wysyłać wiadomości, po wystąpieniu zdarzenia.

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    . Ten projekt używa pakiety *WindowsAzure.ServiceBus* i *Microsoft.Web.WebJobs.Publish* Nuget i jest oparty na [usługę Bus Pub-Sub programowania].

    b. Jest to innej C# konsoli aplikacji firma Microsoft będzie działał jako [Azure WebJob] , ponieważ ma na uruchamianie ciągłe, aby odsłuchać dla wiadomości z systemów LoB/wewnętrznej bazy danych. Są to część z urządzeń przenośnych wewnętrznej bazy danych.

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c. `CreateSubscription`Służy do tworzenia subskrypcji usługi Bus temat miejsce, w którym system wewnętrznej bazy danych będzie wysyłać wiadomości. W zależności od scenariusza biznesowego ten składnik utworzy jedną lub więcej subskrypcji do odpowiednich tematów (np. niektóre może odbierać wiadomości z systemu HR, niektóre z systemu Finanse itd.)

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d. ReceiveMessageAndSendNotification jest używany do czytania wiadomości z tematu, korzystając ze swojej subskrypcji i jeśli Czytaj zakończyło się pomyślnie były wysyłane do aplikacji mobilnej za pomocą koncentratorów powiadomienie Azure następnie jednostki powiadomienie (w scenariuszu Przykładowe powiadomienie natywnych wyskakującego systemu Windows).

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    e. Publikowania to jako **WebJob**, kliknij prawym przyciskiem myszy rozwiązania w programie Visual Studio, a następnie wybierz pozycję **Publikuj jako WebJob**

    ![][2]

    f. Wybierz profil publikowania i utworzyć nową witrynę Azure, jeśli nie istnieje już której będzie przechowywana ten WebJob i po utworzeniu witryny sieci Web, a następnie **Publikuj**.

    ![][3]

    g. Skonfiguruj zadanie ma być "Uruchamiania stale", tak aby po zalogowaniu się do [Portalu klasyczny Azure] powinna być widoczna podobną do następujących czynności:

    ![][4]


3. **EnterprisePushMobileApp**

    . Jest to aplikacja ze Sklepu Windows, którą otrzymasz wyskakującego powiadomienia z WebJob działającego jako część usługi przenośnych wewnętrznej bazy danych i go wyświetlić. To jest oparty na [Powiadomienie koncentratory - samouczek uniwersalny systemu Windows].  

    b. Upewnij się, że aplikacja jest włączona powiadomień wyskakującego.

    c. Upewnij się, że następujące powiadomienie koncentratory kodu rejestracji jest wywoływana w aplikacji uruchamiania (po zamiana *HubName* i *DefaultListenSharedAccessSignature*:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Uruchamianie przykładowych:

1. Zapewnia usługi WebJob jest uruchomiony pomyślnie i według harmonogramu "Uruchamiania stale".
2. Uruchom **EnterprisePushMobileApp** , która rozpocznie się aplikacji ze Sklepu Windows.
3. Uruchom aplikację konsoli **EnterprisePushBackendSystem** , która będzie symulować LoB wewnętrznej bazy danych i rozpocznie się wysyłania wiadomości i powinien zostać wyświetlony wyskakującego powiadomienia o wyświetlaniu podobnej do następującej:

    ![][5]

4. Wiadomości pierwotnie zostały wysłane do tematów Bus usługi, które zostało monitorowane w przypadku subskrypcji usługi Bus w swojej pracy w sieci Web. Gdy wiadomość została odebrana, powiadomienie została utworzona i wysyłane do aplikacji dla urządzeń przenośnych. Możesz przejrzeć dzienniki WebJob, aby potwierdzić przetwarzania po przejściu do łącza dzienniki w [Klasycznym Portal Azure] dla zadania w sieci Web:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Powiadomienie o Centrum próbki]: https://github.com/Azure/azure-notificationhubs-samples
[Usługa mobilna Azure]: http://azure.microsoft.com/documentation/services/mobile-services/
[Usługa Azure Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Usługa Bus Pub-Sub programowania]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Powiadomienie o koncentratory - samouczek uniwersalny systemu Windows]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Portal Azure klasyczny]: https://manage.windowsazure.com/
