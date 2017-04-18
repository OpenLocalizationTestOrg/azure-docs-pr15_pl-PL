<properties 
    pageTitle="Usługi Bus i Python z AMQP 1.0 | Microsoft Azure"
    description="Za pomocą usługi Bus z Python AMQP."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-python-with-amqp-10"></a>Usługa Bus z Python za pomocą AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Python protonów jest powiązanie języka Python protonów-c; oznacza to, że Python protonów jest zaimplementowana jako opakowanie wokół aparat zaimplementowana w C.

## <a name="download-the-proton-client-library"></a>Pobierz protonów Biblioteka klienta

C protonów i jego skojarzony powiązania (w tym Python) można pobrać z [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Pobieranie znajduje się w postaci kodu źródłowego. Aby utworzyć kod, postępuj zgodnie z instrukcjami zawarte w pakiecie pobrany.

Należy zauważyć, że podczas pisania tego dokumentu, obsługa protokołu SSL w C protonów jest dostępna tylko dla systemów operacyjnych Linux. Ponieważ Bus usługa Azure wymaga korzystania z protokołu SSL, C protonów (i powiązań language) tylko można uzyskać dostęp do usługi Bus z Linux w tej chwili. Praca umożliwiające protonów-C, za pomocą protokołu SSL dla systemu Windows jest trwającej tak wyboru Wstecz często aktualizacji.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-python"></a>Praca z kolejek, tematy i subskrypcje z Python Bus usługi

Poniższy kod pokazano, jak na wysyłanie i odbieranie wiadomości z Bus usługi wiadomości jednostki.

### <a name="send-messages-using-proton-python"></a>Wysyłanie wiadomości przy użyciu protonów Python

Poniższy kod przedstawia jak wysłać wiadomość do Bus usługi wiadomości jednostki.

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### <a name="receive-messages-using-proton-python"></a>Odbierać wiadomości przy użyciu protonów Python

Poniższy kod pokazano, jak otrzymywać wiadomości od Bus usługi wiadomości jednostki.

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## <a name="messaging-between-net-and-proton-python"></a>Wiadomości między .NET i Python protonów

### <a name="application-properties"></a>Właściwości aplikacji

#### <a name="proton-python-to-service-bus-net-apis"></a>Python protonów do obsługi Bus interfejsy API .NET

Obsługuje wiadomości Python protonów właściwości aplikacji następujących typów: **int** **długi**, **ruchomości**, **uuid**, **wartość logiczna**, **ciąg**. Poniższy kod Python pokazano, jak ustawić właściwości wiadomości przy użyciu każdego z tych typów właściwości.

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

W interfejsie API usługi .NET Bus właściwości aplikacji wiadomości są przenoszone w kolekcji **Properties** [BrokeredMessage][]. Poniższy kod pokazano, jak odczytywać wiadomości odebranych od klienta Python właściwości aplikacji.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

Poniższa tabela mapowania typów właściwości Python do typów właściwości .NET.

| Typ właściwości Python | Typ właściwości .NET |
|----------------------|--------------------|
| int                  | int                |
| Przestaw                | podwójne             |
| długie                 | Int64              |
| UUID                 | Identyfikator GUID               |
| wartość logiczna                 | wartość logiczna               |
| ciąg               | ciąg             |

#### <a name="service-bus-net-apis-to-proton-python"></a>Usługa Bus interfejsy API .NET do protonów Python

Typ [BrokeredMessage][] obsługuje właściwości aplikacji następujących typów: **bajt**, **sbyte**, **znak**, **Krótki**, **ushort**, **int**, **uint**, **długi**, **ulong**, **ruchomości**, **podwójne**, **dziesiętne**, **wartość logiczna**, **Identyfikator Guid**, **ciąg**, **Identyfikator Uri**, **daty i godziny**, **DateTimeOffset**i **przedziału czasu**. Poniższy kod .NET pokazano, jak ustawić właściwości dla obiektu [BrokeredMessage][] przy użyciu każdego z tych typów właściwości.

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

Poniższy kod Python pokazano, jak odczytywać właściwości aplikacji wiadomości odebranych od klienta usługi Bus .NET.

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

Poniższa tabela mapowania typów właściwości .NET do Python typy właściwości.

| Typ właściwości .NET | Typ właściwości Python | Notatki                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bajt               | int                  | -                                                                                                                                                                     |
| sbyte              | int                  | -                                                                                                                                                                     |
| znak               | znak                 | Python protonów zajęć                                                                                                                                                 |
| krótki              | int                  | -                                                                                                                                                                     |
| ushort             | int                  | -                                                                                                                                                                     |
| int                | int                  | -                                                                                                                                                                     |
| uint               | int                  | -                                                                                                                                                                     |
| długie               | int                  | -                                                                                                                                                                     |
| ulong              | długie                 | Python protonów zajęć                                                                                                                                                 |
| Przestaw              | Przestaw                | -                                                                                                                                                                     |
| podwójne             | Przestaw                | -                                                                                                                                                                     |
| dziesiętna            | Ciąg               | Z protonów dziesiętnych nie jest obecnie obsługiwane.                                                                                                                     |
| wartość logiczna               | wartość logiczna                 | -                                                                                                                                                                     |
| Identyfikator GUID               | UUID                 | Python protonów zajęć                                                                                                                                                 |
| ciąg             | ciąg               | -                                                                                                                                                                     |
| Daty i godziny           | Sygnatura czasowa            | Python protonów zajęć                                                                                                                                                 |
| DateTimeOffset     | DescribedType        | DateTimeOffset.UtcTicks zamapowane typ AMQP:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| Przedziału czasu           | DescribedType        | Timespan.Ticks zamapowane typ AMQP:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| Identyfikator URI                | DescribedType        | Uri.AbsoluteUri zamapowane typ AMQP:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-properties"></a>Właściwości standardowe

W poniższej tabeli przedstawiono mapowanie między właściwości standardowe wiadomości protonów Python i właściwości standardowe wiadomości [BrokeredMessage][] .

#### <a name="proton-python-to-service-bus-net-apis"></a>Python protonów do obsługi Bus interfejsy API .NET

| Protonów Python        | Usługa Bus .NET         | Notatki                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| trwałe              | n/d!                      | Usługa Bus obsługuje tylko trwałe wiadomości.               |
| Priority (priorytet)             | n/d!                      | Usługa Bus obsługuje tylko jednej wiadomości priorytet.      |
| TTL                  | Message.TimeToLive       | Konwersja, Python protonów TTL jest definiowana (w milisekundach). |
| pierwszy\_nabywca      | n/d!                      | -                                                           |
| dostarczenie\_Statystyka      | n/d!                      | -                                                           |
| Identyfikator                   | Message.MessageID        | -                                                           |
| Użytkownik\_identyfikator             | n/d!                      | -                                                           |
| adres              | Message.To               | -                                                           |
| Temat              | Message.Label            | -                                                           |
| odpowiedź\_do            | Message.ReplyTo          | -                                                           |
| korelacji\_identyfikator      | Message.CorrelationID    | -                                                           |
| zawartość\_typu        | Message.ContentType      | -                                                           |
| zawartość\_kodowanie    | n/d!                      | -                                                           |
| wygaśnięcia\_czasu         | n/d!                      | -                                                           |
| Tworzenie\_czasu       | n/d!                      | -                                                           |
| Grupa\_identyfikator            | Message.SessionId        | -                                                           |
| Grupa\_sekwencji      | n/d!                      | -                                                           |
| odpowiedź\_do\_grupy\_identyfikator | Message.ReplyToSessionId | -                                                           |
| Formatowanie               | n/d!                      | -                                                           |

| Usługa Bus .NET        | Protonów                       | Notatki                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| Typ zawartości             | Message.content\_typu        | -                                                           |
| CorrelationId           | Message.Correlation\_identyfikator      | -                                                           |
| EnqueuedTimeUtc         | n/d!                          | -                                                           |
| Etykieta                   | Message.subject              | -                                                           |
| Identyfikator komunikatu               | Message.ID                   | -                                                           |
| Parametr from                 | Message.Reply\_do            | -                                                           |
| ReplyToSessionId        | Message.Reply\_do\_grupy\_identyfikator | -                                                           |
| ScheduledEnqueueTimeUtc | n/d!                          | -                                                           |
| Identyfikator sesji               | Message.Group\_identyfikator            | -                                                           |
| Licznika TimeToLive              | Message.TTL                  | Konwersja, Python protonów TTL jest definiowana (w milisekundach). |
| Aby                      | Message.address              | -                                                           |

## <a name="next-steps"></a>Następne kroki

Chcesz dowiedzieć się więcej? Skorzystaj z poniższych łączy:

- [Omówienie usług Bus AMQP]
- [AMQP w Bus usługi dla systemu Windows Server]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP w Bus usługi dla systemu Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx

[Omówienie usług Bus AMQP]: service-bus-amqp-overview.md
