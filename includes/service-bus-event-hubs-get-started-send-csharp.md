## <a name="send-messages-to-event-hubs"></a>Wysyłanie wiadomości do koncentratorów zdarzenia

W tej sekcji będzie Napisz Aplikacja konsoli w systemie Windows wyśle odbiorcy zdarzeń Twoim Centrum zdarzenia.

1. W programie Visual Studio należy utworzyć nowy projekt Visual C# pulpitu aplikacji przy użyciu szablonu projektów **Console Application** . Nazwa projektu **nadawcy**.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij **Zarządzanie pakietów NuGet rozwiązania**. 

3. Kliknij kartę **Przeglądaj** , a następnie wyszukaj `Microsoft Azure Service Bus`. Upewnij się, że nazwa projektu (**nadawcy**) jest określony w polu **wersje** . Kliknij przycisk **Zainstaluj**i zaakceptuj warunki użytkowania. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Programu Visual Studio do pobrania, instaluje i dodaje odwołanie do [pakietu NuGet biblioteki Bus usługi Azure](https://www.nuget.org/packages/WindowsAzure.ServiceBus).

4. Dodaj następujący `using` instrukcje w górnej części pliku **Plik Program.cs** :

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Dodawanie pola do klasy **programu** , podstawianie wartości symbol zastępczy o nazwie Centrum zdarzenia utworzonego w poprzedniej sekcji oraz parametry połączenia nazw poziom zapisane wcześniej.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. Dodaj następujące metody klasy **programu** :

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    Tej metody można użyć w sposób ciągły wysyła zdarzenia usługi Centrum zdarzeń z opóźnieniem 200 ms.

7. Na koniec dodać kolejne wiersze do metody **główne** :

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
