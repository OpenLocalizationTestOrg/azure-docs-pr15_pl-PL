<properties 
    pageTitle="Jak używać AMQP 1.0 przy użyciu interfejsu API języka Java usługi Bus | Microsoft Azure" 
    description="Jak używać usługi wiadomości Java (JMS) z Bus usługa Azure i zaawansowane wiadomości Kolejkowanie Protodol (AMQP) 1.0." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Jak używać języka Java wiadomości usługi (JMS) interfejsu API usługi Bus i AMQP 1.0

Zaawansowane wiadomości Kolejkowanie Protocol (AMQP) 1.0 jest wydajność, niezawodne i poziomu przewodowy wiadomości protokół, który można tworzyć niezawodne, między platformami aplikacji do obsługi wiadomości.

Pomoc techniczna dla 1.0 AMQP w Bus usługi oznacza, że można używać kolejek i publikowania/subskrypcji brokered funkcje wiadomości z zakresu platform za pomocą protokołu binarnego wydajność. Ponadto można tworzyć aplikacje obejmuje składniki utworzone przy użyciu różnych języków, struktury i systemy operacyjne.

W tym artykule wyjaśniono, jak używać usługi Bus wiadomości funkcje (kolejek i publikowania subskrypcji tematy) z aplikacji Java przy użyciu popularnych Java wiadomości usługi (JMS) interfejsu API standardowy. Istnieje wyjaśniono, jak się tym samym korzystanie z interfejsu API usługi Bus .NET [artykuł pomocniczym](service-bus-dotnet-advanced-message-queuing.md) . Umożliwia te dwa przewodniki razem więcej informacji na temat obsługi wiadomości między platformami przy użyciu AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Wprowadzenie do usługi Bus

Ten przewodnik założono, że już obszar nazw usługi Bus zawierający kolejki o nazwie **kolejki Kolejka1**. W przeciwnym razie następnie możesz [utworzyć nazw i kolejki](service-bus-create-namespace-portal.md) za pomocą [Azure portal](https://portal.azure.com). Aby uzyskać więcej informacji na temat sposobu tworzenia nazw Bus usługi i kolejek zobacz [jak za pomocą usługi Bus kolejek](service-bus-dotnet-get-started-with-queues.md).

> [AZURE.NOTE] Podzielony na partycje kolejki i tematy obsługują także AMQP. Aby uzyskać więcej informacji zobacz [Partitioned wiadomości podmioty](service-bus-partitioning.md) i [AMQP 1.0 obsługę usługi Bus partycją kolejki i tematów](service-bus-partitioned-queues-and-topics-amqp-overview.md).

## <a name="downloading-the-amqp-10-jms-client-library"></a>Pobieranie biblioteki klienta AMQP 1.0 JMS

Aby uzyskać informacje o tym, gdzie pobrać najnowszą wersję Apache Qpid JMS AMQP 1.0 Biblioteka klienta, odwiedź stronę [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Należy dodać następujące cztery pliki SŁOIK z archiwum rozkładu Apache Qpid JMS AMQP 1.0 do ścieżki Java podczas tworzenia i uruchamianie aplikacji JMS za pomocą usługi Bus:

- geronimo jms\_1.1\_1.0.jar Specyfikacja
- qpid-amqp-1-0-Client-[Version].JAR
- qpid-amqp-1-0-Client-jms-[Version].JAR
- qpid-amqp-1-0-Common-[Version].JAR

## <a name="coding-java-applications"></a>Kodowania aplikacji Java

### <a name="java-naming-and-directory-interface-jndi"></a>Java nadawaniu nazw i katalogu Interface (JNDI)

JMS użyto Java nadawaniu nazw i katalogu Interface (JNDI), aby utworzyć odstęp między nazw logicznych i fizycznych. Dwa typy obiektów JMS rozpoznawania przy użyciu JNDI: ConnectionFactory i docelowej. JNDI korzysta z modelu dostawcy, w którym możesz podłączyć różnych usług katalogów obsługę obowiązków rozpoznawanie nazw. 1.0 AMQP JMS Qpid Apache biblioteki zawiera proste właściwości format opartych na plikach dostawcy JNDI skonfigurowaną przy użyciu właściwości pliku z następujących czynności:

```
# servicebus.properties - sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>Konfigurowanie ConnectionFactory

Wpis można definiować **ConnectionFactory** przez dostawcę JNDI w Qpid właściwości pliku jest następujący format:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Gdzie **[jndi_name]** i **[ConnectionURL]** mają następujące znaczenie:

- **[jndi_name]**: Nazwa logiczna ConnectionFactory. Jest to nazwa, którego będzie można rozwiązać w aplikacji Java przy użyciu metody JNDI IntialContext.lookup().
- **[ConnectionURL]**: adres URL, który udostępnia biblioteki JMS informacje wymagane do brokera AMQP.

Format **ConnectionURL** jest w następujący sposób:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Gdzie **[nazw]**, **[SASPolicyName]** i **[SASPolicyKey]** mają następujące znaczenie:

- **[nazw]**: Bus usługi nazw.
- **[SASPolicyName]**: Nazwa zasady podpis dostępu udostępnione kolejki.
- **[SASPolicyKey]**: klucz zasad podpisu dostępu udostępnione kolejki.

> [AZURE.NOTE] Należy kodowanie adresu URL hasło ręcznie. Przydatne narzędzie kodowanie adresów URL jest dostępna w [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

#### <a name="configure-destinations"></a>Konfigurowanie miejsc docelowych

Wpis używane do określania miejsca docelowego przez dostawcę JNDI w Qpid właściwości pliku jest następujący format:

```
queue.[jndi_name] = [physical_name]
```

lub

```
topic.[jndi_name] = [physical_name]
```

Gdzie **[jndi\_nazwę]** i **[fizycznie\_nazwę]** mają następujące znaczenie:

- **[jndi_name]**: Nazwa logiczna elementu docelowego. Jest to nazwa, którego będzie można rozwiązać w aplikacji Java przy użyciu metody JNDI IntialContext.lookup().
- **[physical_name]**: nazwę podmiotu Bus usługi, do której aplikacja wysłania lub odebrania wiadomości.

> [AZURE.NOTE] Po otrzymaniu z subskrypcji usługi Bus tematu, nazwy fizycznej określonego w JNDI powinna być nazwą tematu. Nazwa subskrypcji znajduje się po utworzeniu trwałego subskrypcji w kodzie aplikacji JMS. [Usługa Bus AMQP 1.0 przewodnik programisty](service-bus-amqp-dotnet.md) zawiera szczegółowe informacje na temat pracy z subskrypcją tematu Bus usługi z JMS.

### <a name="write-the-jms-application"></a>Napisz aplikację JMS

Nie ma żadnych specjalnych interfejsy API ani opcje potrzebne podczas korzystania z usługi Bus JMS. Istnieją jednak kilka ograniczeń, których będzie można później. Zgodnie z dowolną aplikacją JMS po pierwsze wymagane jest Konfiguracja środowiska JNDI, aby można było rozwiązać **ConnectionFactory** i miejsc docelowych.

#### <a name="configure-the-jndi-initialcontext"></a>Konfigurowanie JNDI InitialContext

Środowisko JNDI skonfigurowano przekazując skrótów informacje o konfiguracji do konstruktora klasy javax.naming.InitialContext. Dwa wymagane elementy w skrótów to nazwa klasy początkowej Factory kontekstu i adres URL dostawcy. Poniższy kod pokazano, jak skonfigurować środowisko JNDI do korzystania z Qpid właściwości pliku na podstawie dostawcy JNDI właściwości pliku o nazwie **servicebus.properties**.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Prosta aplikacja JMS przy użyciu kolejki Bus usługi

Następujący przykład program wysyła JMS TextMessages do kolejki Bus usługi o nazwie logiczne JNDI kolejki i odbiera wiadomości ponownie.

```
// SimpleSenderReceiver.java
    
import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;
    
public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();
    
    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);
    
        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");
    
        // Create Connection
        connection = cf.createConnection();
    
        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);
    
        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }
    
    public static void main(String[] args) {
        try {
    
            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }
    
            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }
    
    public void close() throws JMSException {
        connection.close();
    }
    
    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}   
```

### <a name="run-the-application"></a>Uruchamianie aplikacji

Uruchamianie aplikacji tworzy dane wyjściowe formularza:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Obsługa wiadomości między JMS i .NET między platformami

Ten przewodnik pokazano, jak wysyłać i odbierać wiadomości przy użyciu usługi Bus przy użyciu JMS. Jedną z najważniejszych zalet AMQP 1.0 jest jednak umożliwia aplikacjom utworzona na podstawie składników napisanych w różnych językach z komunikatów wymienianych niezawodne i pełny wierności.

Za pomocą aplikacji JMS przykładowej opisanych powyżej i podobne aplikacji .NET pobierany ze przewodnik pomocniczym, [jak za pomocą AMQP 1.0 przy użyciu interfejsu API .NET programu .NET usługi Bus](service-bus-dotnet-advanced-message-queuing.md), możesz wymieniać wiadomości między .NET i Java. 

Aby uzyskać więcej informacji dotyczących szczegółów między platformami wiadomości przy użyciu usługi Bus i AMQP 1.0 zobacz [Usługa Bus AMQP 1.0 Podręcznik dewelopera](service-bus-amqp-dotnet.md).

### <a name="jms-to-net"></a>JMS do .NET

Udowodnić JMS .NET wiadomości:

* Uruchom aplikację przykładowe .NET bez żadnych argumentów wiersza polecenia.
* Uruchom aplikację przykładowe Java przy użyciu argumentu wiersza polecenia "sendonly". W tym trybie aplikacji nie będziesz odbierać wiadomości z kolejki tylko wysyła.
* Naciśnij kilka razy klawisz **Enter** , w konsoli aplikacji Java, co spowoduje wiadomości do wysłania.
* Te wiadomości są odbierane przez aplikację .NET.

#### <a name="output-from-jms-application"></a>Dane wyjściowe z aplikacji JMS

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Dane wyjściowe aplikacji .NET

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET do JMS

Udowodnić .NET JMS wiadomości:

* Uruchom aplikację przykładowe .NET przy użyciu argumentu wiersza polecenia "sendonly". W tym trybie aplikacji nie będziesz odbierać wiadomości z kolejki tylko wysyła.
* Uruchom aplikację przykładowe Java bez żadnych argumentów wiersza polecenia.
* Naciśnij kilka razy klawisz **Enter** , w konsoli aplikacji .NET spowoduje, że aby została wysłana.
* Te wiadomości są odbierane przez aplikację języka Java.

#### <a name="output-from-net-application"></a>Dane wyjściowe aplikacji .NET

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Dane wyjściowe z aplikacji JMS

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Nieobsługiwane funkcje i ograniczenia

Gdy przy użyciu JMS przez AMQP 1.0 z Bus usług, takich jak obowiązują następujące ograniczenia:

* Tylko jeden **MessageProducer** lub **MessageConsumer** jest dozwolone **sesji**. Jeśli chcesz utworzyć wiele **MessageProducers** lub **MessageConsumers** w aplikacji, Utwórz dedykowane **sesji** dla każdego z nich.
* Subskrypcje nietrwałe tematu nie są obecnie obsługiwane.
* **MessageSelectors** nie są obecnie obsługiwane.
* Tymczasowe miejsc docelowych, to znaczy **TemporaryQueue**, **TemporaryTopic** nie są obecnie obsługiwane, razem z **QueueRequestor** i **TopicRequestor** interfejsów API, które z nich korzystać.
* Sesje transakcji i transakcje rozproszone nie są obsługiwane.

## <a name="summary"></a>Podsumowanie

Ten przewodnik pokazano, jak używać usługi Bus brokered wiadomości funkcji (kolejek i publikowania subskrypcji tematy) z języka Java za pomocą popularnych API JMS i AMQP 1.0.

Za pomocą usługi Bus AMQP 1.0 z innych języków, w tym .NET, C, Python i PHP. Składniki utworzone za pomocą tych różnych języków można wymieniać wiadomości w wiarygodny sposób i na pełny wierności przy użyciu AMQP 1.0 obsługi w Bus usług. Aby uzyskać więcej informacji zobacz [Usługa Bus AMQP 1.0 Podręcznik dewelopera](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Następne kroki

* [Obsługa AMQP 1.0 w Bus usługa Azure](service-bus-amqp-overview.md)
* [Jak używać AMQP 1.0 przy użyciu interfejsu API usługi Bus .NET](service-bus-dotnet-advanced-message-queuing.md)
* [Usługi Bus AMQP 1.0 Podręcznik dewelopera](service-bus-amqp-dotnet.md)
* [Jak używać usługi Bus kolejki](service-bus-dotnet-get-started-with-queues.md)
* [Centrum deweloperów języka Java](/develop/java/).



 
