<properties
    pageTitle="Wysyłanie wiadomości chmury do urządzenia z Centrum IoT | Microsoft Azure"
    description="Postępuj zgodnie z tego samouczka, aby dowiedzieć się, jak wysyłać wiadomości chmury do urządzenia Centrum IoT Azure za pomocą języka Java."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>Samouczek: Jak można wysyłać chmury do urządzenia z Centrum IoT i Java

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Wprowadzenie

Azure koncentratora IoT jest w pełni zarządzanych usług, który ułatwia włączanie i niezawodności komunikacji dwukierunkowego między miliony IoT urządzeń i aplikacji powrotem zakończyć. [Wprowadzenie do Centrum IoT] samouczek pokazano, jak tworzyć Centrum IoT, obsługi administracyjnej urządzenia tożsamości w nim i kod symulowany urządzenie, które wysyła wiadomości urządzenia w chmurze.

Ten samouczek opiera się na [Wprowadzenie do Centrum IoT]. Jak przedstawiono do:

- Z Twojej aplikacji chmury wewnętrznej wysyłanie wiadomości chmury do urządzenia do jednego urządzenia za pośrednictwem Centrum IoT.
- Odbieranie wiadomości chmury do urządzenia na urządzeniu.
- Z chmury usługi aplikacji zakończenia, należy zażądać potwierdzenia dostarczenia (*Opinia*) dla wiadomości wysyłane do urządzenia z Centrum IoT.

Więcej informacji na temat wiadomości w chmurze do urządzenia można znaleźć w [IoT Centrum deweloperów przewodnik][IoT Hub Developer Guide - C2D].

Na końcu tego samouczka możesz uruchomić dwóch aplikacji konsoli Java:

* **symulowany urządzenia**, zmodyfikowaną wersję aplikacji utworzony w [Wprowadzenie do Centrum IoT], który łączy się z Centrum IoT i odbiera wiadomości w chmurze do urządzenia.
* **Wysyłaj c2d**, które wysyła wiadomość chmury do urządzenia do symulowany urządzenia za pomocą Centrum IoT, a następnie otrzymuje jego potwierdzenia dostarczenia.

> [AZURE.NOTE] Centrum IoT obsługuje SDK dla wielu platformy urządzeń i języki (w tym C, Java i Javascript) SDK urządzenia Azure IoT. Aby uzyskać instrukcje krok po kroku dotyczące nawiązywania połączenia między urządzeniem kod tego samouczka i ogólnie do koncentratora IoT Azure, zobacz [Centrum deweloperów IoT Azure].

Aby użyć tego samouczka, są potrzebne następujące elementy:

+ Java SE 8. <br/> [Przygotowywanie środowiska programowania] [ lnk-dev-setup] zawiera opis sposobu instalowania Java ten samouczek w systemach Windows i Linux.

+ Środowiska maven 3.  <br/> [Przygotowywanie środowiska programowania] [ lnk-dev-setup] zawiera opis sposobu instalowania środowiska Maven ten samouczek w systemach Windows i Linux.

+ Konto Azure active. (Jeśli nie masz konta, możesz utworzyć [bezpłatne konto] [ lnk-free-trial] na kilka minut.)

## <a name="receive-messages-on-the-simulated-device"></a>Odbieranie wiadomości na tym urządzeniu symulowany

W tej sekcji można zmodyfikować aplikację symulacji utworzone w [Wprowadzenie do Centrum IoT] otrzymywać wiadomości w chmurze do urządzenia z poziomu Centrum IoT.

1. Przy użyciu edytora tekstu, otwórz plik simulated-device\src\main\java\com\mycompany\app\App.java.

2. Dodaj następujące klasy **MessageCallback** jako klasę zagnieżdżone wewnątrz klasy **aplikacji** . Metoda **wykonania** jest wywoływana, gdy urządzenie odbiera wiadomości z Centrum IoT. W tym przykładzie urządzenie zawsze powiadamia koncentratora o zakończeniu wiadomości:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. Modyfikowanie **głównym** metodę utworzenia wystąpienia **MessageCallback** i wywołaj metodę **setMessageCallback** , zanim klient zostanie otwarty w następujący sposób:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] Jeśli używasz protokołu HTTP zamiast MQTT lub AMQP jako transportu wystąpienia **DeviceClient** sprawdza wiadomości z Centrum IoT rzadko (mniej niż co 25 minut). Aby uzyskać więcej informacji o różnicach między MQTT, AMQP i HTTP pomocy technicznej i ograniczania Centrum IoT, zobacz [IoT Centrum deweloperów przewodnik][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Wysyłanie wiadomości chmury do urządzenia

W tej sekcji możesz utworzyć aplikację konsoli Java, która wysyła wiadomości w chmurze do urządzenia do aplikacji symulacji. Potrzebujesz identyfikator urządzenia dodanych w samouczku [Wprowadzenie do Centrum IoT] urządzenia. Należy również parametry połączenia z koncentratora IoT, które można uzyskać w [portalu Azure].

1. Tworzenie projektu środowiska Maven o nazwie **Wysyłaj c2d** przy użyciu następującego polecenia w wierszu polecenia. Należy zauważyć, że jest to polecenie jednym, długie:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Wierszu polecenia przejdź do nowego folderu wysyłaj c2d.

3. Przy użyciu edytora tekstu, otwórz plik pom.xml w folderze wysyłaj c2d i dodaj następujące zależności do węzła **zależności** . Dodawanie zależności umożliwia można komunikować się z usługą Centrum IoT za pomocą pakietu **iothub java — Usługa klienta** w aplikacji:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Zapisz i zamknij plik pom.xml.

5. Przy użyciu edytora tekstu, otwórz plik send-c2d-messages\src\main\java\com\mycompany\app\App.java.

6. Dodaj poniższe instrukcje **Importowanie** pliku:

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Dodaj następujące zmienne zajęć poziomie do klasy **aplikacji** , zamieniając **{yourhubconnectionstring}** i **{yourdeviceid}** wartości z uwagę na wspomniane wcześniej:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. Zastąp metodę **głównym** następujący kod, który łączy się z Centrum IoT, wysyła wiadomość do urządzenia i zatrzymuje się na potwierdzenie, że urządzenie odbierane i przetwarzane wiadomości:

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] Sake i uproszczenia tego samouczka nie zaimplementować wszelkie zasady ponów próbę. W kodzie produkcyjnym wykonała zasady ponów próbę (na przykład wykładniczego wycofywania), sugerowanej w artykule w witrynie MSDN [Przejściowych obsługi błędów].

## <a name="run-the-applications"></a>Uruchamianie aplikacji

Teraz możesz przystąpić do uruchamiania aplikacji.

1. W wierszu polecenia — w tym folderze symulowany urządzenie uruchom następujące polecenie, aby rozpocząć wysyłanie telemetrycznego na Twoim Centrum IoT i słuchanie dla chmury do urządzenia wiadomości wysyłanych z Twoim Centrum:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Uruchom aplikację symulacji][img-simulated-device]

2. W wierszu polecenia — w tym folderze wysyłaj c2d uruchom następujące polecenie, aby wysłać wiadomość chmury do urządzenia i poczekaj potwierdzenia opinii:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Uruchom polecenie, aby wysłać wiadomość chmury do urządzenia][img-send-command]

## <a name="next-steps"></a>Następne kroki

W tym samouczku wiesz, jak wysyłać i odbierać wiadomości w chmurze do urządzenia. 

Aby wyświetlić przykłady pełną rozwiązań zakończenia do końca korzystające z Centrum IoT, zobacz [Pakiet IoT Azure].

Aby dowiedzieć się więcej na temat tworzenia rozwiązań z Centrum IoT, zobacz [Przewodnik dewelopera koncentratora IoT].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Wprowadzenie do Centrum IoT]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[IoT Centrum deweloperów przewodnika]: iot-hub-devguide.md
[Centrum deweloperów IoT Azure]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Obsługa przejściowych błędów]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[Pakiet Azure IoT]: https://azure.microsoft.com/documentation/suites/iot-suite/