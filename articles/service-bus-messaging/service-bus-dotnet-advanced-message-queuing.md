<properties 
    pageTitle="Jak używać AMQP 1.0 z interfejs API Bus usługi .NET | Microsoft Azure" 
    description="Dowiedz się, jak zaawansowane wiadomości Kolejkowanie Protodol (AMQP) 1.0 za pomocą interfejs API Azure .NET usługi Bus." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-amqp-10-with-the-service-bus-net-api"></a>Jak używać AMQP 1.0 przy użyciu interfejsu API usługi Bus .NET

Zaawansowane wiadomości Kolejkowanie Protocol (AMQP) 1.0 jest wydajność, niezawodne i poziomu przewodowy wiadomości protokół, który można tworzyć niezawodne, między platformami aplikacji do obsługi wiadomości.

Pomoc techniczna dla 1.0 AMQP w Bus usługi oznacza, że można używać kolejek i publikowania/subskrypcji brokered funkcje wiadomości z zakresu platform za pomocą protokołu binarnego wydajność. Ponadto można tworzyć aplikacje obejmuje składniki utworzone przy użyciu różnych języków, struktury i systemy operacyjne.

W tym artykule wyjaśniono, jak używać usługi Bus brokered wiadomości funkcji (kolejek i publikowania subskrypcji tematy) z aplikacji .NET przy użyciu interfejsu API usługi Bus .NET. Istnieje [artykuł pomocniczym](service-bus-java-how-to-use-jms-api-amqp.md) , którym wyjaśniono, jak w tym samym użycie standardowego interfejsu API usługi wiadomości Java (JMS). Umożliwia te dwa przewodniki razem więcej informacji na temat obsługi wiadomości między platformami przy użyciu AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Wprowadzenie do usługi Bus

W tym artykule założono, że już obszar nazw usługi Bus zawierający kolejki o nazwie "kolejki Kolejka1." W przeciwnym razie mogą możesz również tworzyć nazw i kolejki za pomocą [Azure portal][]. Aby uzyskać więcej informacji o tworzeniu Bus usługi nazw i kolejki zobacz [Wprowadzenie do usługi Bus kolejek](service-bus-dotnet-get-started-with-queues.md#1-create-a-namespace-using-the-Azure-portal).

## <a name="download-the-service-bus-sdk"></a>Pobierz Bus usługi SDK

Obsługa AMQP 1.0 jest dostępna w usługi Bus SDK w wersji 2.1 lub nowszej. Można pobrać najnowszą bitów z NuGet na [http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/).

## <a name="code-net-applications"></a>Kod aplikacji .NET

Domyślnie biblioteka klienta usługi Bus .NET komunikuje się z usługą Bus usługi przy użyciu dedykowane protokołu opartego na protokole SOAP. Używać AMQP 1.0 zamiast domyślny protokół wymaga jawne konfigurację w parametrach połączenia Bus usługi opisane w następnej sekcji. Oprócz tego zmiany kod aplikacji jest zmieniany zasadniczo podczas korzystania z AMQP 1.0.

W bieżącej wersji istnieje kilka funkcji interfejsu API, które nie są obsługiwane w przypadku korzystania z AMQP. Te funkcje nieobsługiwane są widoczne później w sekcji [nieobsługiwane funkcje i ograniczenia](#unsupported-features-and-restrictions). Niektóre zaawansowane ustawienia konfiguracji mieć także inne znaczenie podczas korzystania z AMQP. Żadna z tych ustawień są używane w tym artykule, ale szczegółowe informacje są dostępne w [Omówienie AMQP Bus usługi](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

### <a name="configure-via-appconfig"></a>Konfigurowanie za pośrednictwem App.config

Czy aplikacje używają pliku konfiguracji App.config do przechowywania ustawień rozwiązaniem zalecane jest. W przypadku aplikacji usługi Bus App.config służy do przechowywania Bus usługi **ConnectionString**. Ta aplikacja przykładowa również przechowuje App.config nazwę Bus usługi wiadomości jednostki, która jest używana.

Poniżej przedstawiono przykładowy plik App.config:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
    </appSettings>
</configuration>
```

### <a name="configure-the-service-bus-connection-string"></a>Konfigurowanie usługi Bus parametry połączenia

Wartość ustawienia **Microsoft.ServiceBus.ConnectionString** jest Bus usługi parametry połączenia, które umożliwiają skonfigurowanie połączenia Bus usługi. Format jest w następujący sposób:

```
Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp
```

Miejsce, w którym `[namespace]` i `[SAS key]` pochodzą z [Azure portal][]. Aby uzyskać więcej informacji, zobacz [jak za pomocą usługi Bus kolejek] [].

Przy użyciu AMQP, parametry połączenia jest dołączany z `;TransportType=Amqp`, który wskazuje Biblioteka klienta, aby wprowadzić połączenie Bus usługi przy użyciu AMQP 1.0.

### <a name="configure-the-entity-name"></a>Konfigurowanie nazwy podmiotu

Ta aplikacja przykładowa używa `EntityName` ustawienia w sekcji **appSettings** pliku App.config skonfigurować nazwę kolejki, z której aplikacja wymienia wiadomości.

### <a name="a-simple-net-application-using-a-service-bus-queue"></a>Prosta aplikacji .NET przy użyciu kolejki Bus usługi

Poniższy przykład wysyła i odbiera wiadomości przy użyciu kolejki Bus usługi.

```
// SimpleSenderReceiver.cs
    
using System;
using System.Configuration;
using System.Threading;
using Microsoft.ServiceBus.Messaging;
    
namespace SimpleSenderReceiver
{
    class SimpleSenderReceiver
    {
        private static string connectionString;
        private static string entityName;
        private static Boolean runReceiver = true;
        private MessagingFactory factory;
        private MessageSender sender;
        private MessageReceiver receiver;
        private MessageListener messageListener;
        private Thread listenerThread;
    
        static void Main(string[] args)
        {
            try
            {
                if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                    runReceiver = false;
                    
                string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                string entityNameKey = "EntityName";
                entityName = ConfigurationManager.AppSettings[entityNameKey];
                connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                Console.WriteLine("Press [enter] to send a message. " +
                    "Type 'exit' + [enter] to quit.");
    
                while (true)
                {
                    string s = Console.ReadLine();
                    if (s.ToLower().Equals("exit"))
                    {
                        simpleSenderReceiver.Close();
                        System.Environment.Exit(0);
                    }
                    else
                        simpleSenderReceiver.SendMessage();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Caught exception: " + e.Message);
            }
        }
    
        public SimpleSenderReceiver()
        {
            factory = MessagingFactory.CreateFromConnectionString(connectionString);
            sender = factory.CreateMessageSender(entityName);
    
            if (runReceiver)
            {
                receiver = factory.CreateMessageReceiver(entityName);
                messageListener = new MessageListener(receiver);
                listenerThread = new Thread(messageListener.Listen);
                listenerThread.Start();
            }
        }
    
        public void Close()
        {
            messageListener.RequestStop();
            listenerThread.Join();
            factory.Close();
        }
    
        private void SendMessage()
        {
            BrokeredMessage message = new BrokeredMessage("Test AMQP message from .NET");
            sender.Send(message);
            Console.WriteLine("Sent message with MessageID = " 
                + message.MessageId);
        }

    }
    
    public class MessageListener
    {
        private MessageReceiver messageReceiver;
        public MessageListener(MessageReceiver receiver)
        {
            messageReceiver = receiver;
        }
    
        public void Listen()
        {
            while (!_shouldStop)
            {
                try
                {
                    BrokeredMessage message = 
                        messageReceiver.Receive(new TimeSpan(0, 0, 10));
                    if (message != null)
                    {
                        Console.WriteLine("Received message with MessageID = " + 
                            message.MessageId);
                        message.Complete();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                    break;
                }
            }
        }
    
        public void RequestStop()
        {
            _shouldStop = true;
        }
    
        private volatile bool _shouldStop;
    }
}
```

### <a name="run-the-application"></a>Uruchamianie aplikacji

Uruchamianie aplikacji tworzy dane wyjściowe formularza:

```
> SimpleSenderReceiver.exe
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
Received message with MessageID = f27f79ec124548c196fd0db8544bca49
Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Obsługa wiadomości między JMS i .NET między platformami

W tym temacie pokazano, jak wysyłać wiadomości do Bus usługi przy użyciu .NET, a także jak odbierać wiadomości przy użyciu .NET. Jedną z najważniejszych zalet AMQP 1.0 jest jednak umożliwia aplikacjom utworzona na podstawie składników napisanych w różnych językach z komunikatów wymienianych niezawodne i pełny wierności.

Za pomocą aplikacji .NET przykładowych opisanych powyżej i podobne aplikacji języka Java pobrane z przewodnika pomocniczym, [jak za pomocą języka Java wiadomości usługi (JMS) interfejsu API przy użyciu usługi Bus i AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)jest możliwe do wymiany wiadomości między .NET i Java. 

Aby uzyskać więcej informacji dotyczących szczegółów między platformami wiadomości przy użyciu usługi Bus i AMQP 1.0 zobacz [Omówienie usługi Bus AMQP 1.0](service-bus-amqp-overview.md).

### <a name="jms-to-net"></a>JMS do .NET

Udowodnić JMS .NET wiadomości:

* Uruchom aplikację przykładowe .NET bez żadnych argumentów wiersza polecenia.
* Uruchom aplikację przykładowe Java przy użyciu argumentu wiersza polecenia "sendonly". W tym trybie aplikacji nie będziesz odbierać wiadomości z kolejki tylko wysyła.
* Naciśnij kilka razy klawisz **Enter** , w konsoli aplikacji Java, co spowoduje wiadomości do wysłania.
* Te wiadomości są odbierane przez aplikację .NET.

### <a name="output-from-jms-application"></a>Dane wyjściowe z aplikacji JMS

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

### <a name="output-from-net-application"></a>Dane wyjściowe aplikacji .NET

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

## <a name="net-to-jms"></a>.NET do JMS

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

Następujące funkcje interfejs API Bus usługi .NET nie są obecnie obsługiwane podczas korzystania z AMQP:

 * Transakcje
 * Wysyłanie pocztą docelowy przeniesienia

Aby uzyskać więcej informacji zobacz [nieobsługiwane funkcje i ograniczenia różnice funkcjonalne](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

## <a name="summary"></a>Podsumowanie

W tym artykule pokazano, jak uzyskiwać dostępu do funkcji wiadomości usługi Bus brokered (kolejek i publikowania subskrypcji tematy) z .NET przy użyciu AMQP 1.0 i interfejs API .NET Bus usługi.

Za pomocą usługi Bus AMQP 1.0 z innych języków, takich jak Java, C, Python i PHP. Składniki utworzone za pomocą tych języków wymieniać wiadomości niezawodne i pełny wierności przy użyciu AMQP 1.0 w Bus usługi. Aby uzyskać więcej informacji zobacz [Omówienie AMQP Bus usługi](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Następne kroki

Teraz, gdy przeczytane omówienie Bus usługi i AMQP program .NET, zobacz poniższe łącza, aby uzyskać więcej informacji.

* [Obsługa AMQP 1.0 w Bus usługi Azure](service-bus-amqp-overview.md)
* [Jak używać języka Java wiadomości usługi (JMS) interfejsu API z Bus usługi i AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
* [Jak używać usługi Bus kolejki](service-bus-dotnet-get-started-with-queues.md)
 
[Azure portal]: https://portal.azure.com
