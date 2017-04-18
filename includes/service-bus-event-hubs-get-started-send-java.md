## <a name="send-messages-to-event-hubs"></a>Wysyłanie wiadomości do koncentratorów zdarzenia

Biblioteka klienta Java dla koncentratorów zdarzenie jest dostępny do użytku w projektach środowiska Maven z [Centralnym repozytorium środowiska Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)i można odwoływać się przy użyciu następującej deklaracji współzależności wewnątrz pliku projektu środowiska Maven:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Dla różnych typów środowiskach kompilacji można jawnie uzyskania najnowszych wydaną SŁOIK plików, z [Centralnym repozytorium środowiska Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) lub [punktu dystrybucji wersji na GitHub](https://github.com/Azure/azure-event-hubs/releases).  

Programu publisher zdarzenia prostego pakiet *com.microsoft.azure.eventhubs* klasy klienta koncentratory zdarzeń i zaimportować pakiet *com.microsoft.azure.servicebus* klasy narzędzia, takie jak typowych wyjątki, które zostały udostępnione klient poczty e-mail Bus usługi Azure. 

Dla następujących próbki należy najpierw utworzyć nowy projekt środowiska Maven dla aplikacji konsoli/powłoki w środowiska programowania Java Ulubione. Zostanie wywołana klasę ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Zamień nazw i nazw Centrum zdarzeń wartości używany podczas tworzenia Centrum zdarzenia.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Następnie utwórz zdarzenie pojedynczej przez zmianę ciągu do jego kodowanie UTF-8 bajtów. Firma Microsoft Utwórz nowe wystąpienie klienta koncentratory zdarzenia z parametrów połączenia i wysłać wiadomość.   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
