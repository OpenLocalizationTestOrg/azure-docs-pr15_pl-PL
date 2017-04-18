<properties 
    pageTitle="Usługi Bus i PHP z AMQP 1.0 | Microsoft Azure"
    description="Za pomocą usługi Bus z PHP AMQP."
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

# <a name="using-service-bus-from-php-with-amqp-10"></a>Usługa Bus z PHP za pomocą AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

PHP protonów jest powiązanie języka PHP, aby protonów-C; oznacza to, że protonów PHP jest zaimplementowana jako opakowanie wokół aparat zaimplementowana w C.

## <a name="downloading-the-proton-client-library"></a>Pobieranie biblioteki protonów klienta

C protonów i jego skojarzony powiązania (w tym PHP) można pobrać z [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html). Pobieranie znajduje się w postaci kodu źródłowego. Aby utworzyć kod, postępuj zgodnie z instrukcjami zawartymi w pakiecie pobrany.

> [AZURE.IMPORTANT] Podczas pisania tego dokumentu Obsługa protokołu SSL w C protonów jest dostępna tylko dla systemów operacyjnych Linux. Ponieważ Bus usługa Azure wymaga korzystania z protokołu SSL, C protonów (i powiązań language) tylko można uzyskać dostęp do usługi Bus z Linux w tej chwili. Praca umożliwiające protonów-C, za pomocą protokołu SSL w systemie Windows, jest tak wyboru Wstecz często aktualizacji.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-php"></a>Praca z kolejek, tematy i subskrypcje z PHP Bus usługi

Poniższy kod pokazano, jak na wysyłanie i odbieranie wiadomości z Bus usługi wiadomości jednostki.

### <a name="sending-messages-using-proton-php"></a>Wysyłanie wiadomości za pomocą PHP protonów

Poniższy kod przedstawia jak wysłać wiadomość do Bus usługi wiadomości jednostki.

```
$messenger = new Messenger();
$message = new Message();
$message->address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";

$message->body = "This is a text string";
$messenger->put($message);
$messenger->send();
```

### <a name="receiving-messages-using-proton-php"></a>Odbieranie wiadomości za pomocą PHP protonów

Poniższy kod pokazano, jak otrzymywać wiadomości od Bus usługi wiadomości jednostki.

```
$messenger = new Messenger();
$address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";
$messenger->subscribe($address);

$messenger->start();
$messenger->recv(1);

if($messenger->incoming())
{
   $message = new Message();
   $messenger->get($message);      
}

$messenger->stop();
```

## <a name="messaging-between-net-and-proton-php"></a>Wiadomości między .NET i protonów PHP

### <a name="application-properties"></a>Właściwości aplikacji

#### <a name="protonphp-to-service-bus-net-apis"></a>ProtonPHP do obsługi Bus interfejsy API .NET

Obsługuje wiadomości PHP protonów właściwości aplikacji następujących typów: **Liczba całkowita**, **Podwójna** **logicznych**, **ciąg**i **obiektu**. Poniższy kod PHP pokazano, jak ustawić właściwości wiadomości przy użyciu każdego z tych typów właściwości.

```
$message->properties["TestInt"] = 1;    
$message->properties["TestDouble"] = 1.5;      
$message->properties["TestBoolean"] = False;
$message->properties["TestString"] = "Service Bus";    
$message->properties["TestObject"] = new UUID("1234123412341234");   
```

W interfejsów API .NET Bus usługi właściwości aplikacji wiadomości są przenoszone w kolekcji **Properties** [BrokeredMessage][]. Poniższy kod pokazano, jak odczytywać wiadomości odebranych od klienta PHP właściwości aplikacji.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}if (message.Properties.Keys.Count > 0)
{
foreach (string name in message.Properties.Keys)
{
  Object value = message.Properties[name];
  Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
}
Console.WriteLine();
}
```

Poniższa tabela mapowania typów właściwości PHP do typów właściwości .NET.

| Typ właściwości PHP | Typ właściwości .NET |
|-------------------|--------------------|
| Liczba całkowita           | int                |
| podwójne            | podwójne             |
| wartość logiczna           | wartość logiczna               |
| ciąg            | ciąg             |
| obiekt            | Obiekt             |

#### <a name="service-bus-net-apis-to-php"></a>Usługa Bus interfejsy API .NET do PHP

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
```

Poniższy kod PHP pokazano, jak odczytywać właściwości aplikacji wiadomości odebranych od klienta usługi Bus .NET.

```
if ($message->properties != null)
{
  foreach($message->properties as $key => $value)
  {
    printf("-- %s : %s (%s) \n", $key, $value, gettype($value));                       
  }         
}
```

Poniższa tabela mapowania typów właściwości .NET do typów właściwości PHP.

| Typ właściwości .NET | Typ właściwości PHP | Notatki                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| bajt               | Liczba całkowita           | -                                                                                                                                                                     |
| sbyte              | Liczba całkowita           | -                                                                                                                                                                     |
| znak               | Znak              | PHP protonów zajęć                                                                                                                                                    |
| krótki              | Liczba całkowita           | -                                                                                                                                                                     |
| ushort             | Liczba całkowita           | -                                                                                                                                                                     |
| int                | Liczba całkowita           | -                                                                                                                                                                     |
| uint               | Liczba całkowita           | -                                                                                                                                                                     |
| długie               | Liczba całkowita           | -                                                                                                                                                                     |
| ulong              | Liczba całkowita           | -                                                                                                                                                                     |
| Przestaw              | podwójne            | -                                                                                                                                                                     |
| podwójne             | podwójne            | -                                                                                                                                                                     |
| dziesiętna            | ciąg            | Z protonów dziesiętnych nie jest obecnie obsługiwane.                                                                                                                     |
| wartość logiczna               | wartość logiczna           | -                                                                                                                                                                     |
| Identyfikator GUID               | UUID              | PHP protonów zajęć                                                                                                                                                    |
| ciąg             | ciąg            | -                                                                                                                                                                     |
| Daty i godziny           | Liczba całkowita           | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | DateTimeOffset.UtcTicks zamapowane typ AMQP:<type name="datetime-offset" class=restricted source="long"><descriptor name="com.microsoft:datetime-offset" /></type> |
| Przedziału czasu           | DescribedType     | Timespan.Ticks zamapowane typ AMQP:<type name="timespan" class=restricted source="long"><descriptor name="com.microsoft:timespan" /></type>                        |
| Identyfikator URI                | DescribedType     | Uri.AbsoluteUri zamapowane typ AMQP:<type name="uri" class=restricted source="string"><descriptor name="com.microsoft:uri" /></type>                               |

### <a name="standard-properties"></a>Właściwości standardowe

W poniższej tabeli przedstawiono mapowanie między właściwości standardowe wiadomości protonów PHP i właściwości standardowe wiadomości [BrokeredMessage][] .

| Protonów PHP           | Usługa Bus .NET         | Notatki                                                    |
|----------------------|--------------------------|----------------------------------------------------------|
| Trwałe              | n/d!                      | Usługa Bus obsługuje tylko trwałe wiadomości.          |
| Priority (priorytet)             | n/d!                      | Usługa Bus obsługuje tylko jednej wiadomości priorytet. |
| TTL                  | Message.TimeToLive       | Konwersja, PHP protonów TTL jest definiowana (w milisekundach).   |
| pierwszy\_nabywca      | -                          | -                                                          |
| dostarczenie\_Statystyka      | -                          | -                                                          |
| Identyfikator                   | Message.Id               | -                                                          |
| Użytkownik\_identyfikator             | -                          | -                                                          |
| Adres              | Message.To               | -                                                          |
| Temat              | Message.Label            | -                                                          |
| odpowiedź\_do            | Message.ReplyTo          | -                                                          |
| korelacji\_identyfikator      | Message.CorrelationId    | -                                                          |
| zawartość\_typu        | Message.ContentType      | -                                                          |
| zawartość\_kodowanie    | n/d!                      | -                                                          |
| wygaśnięcia\_czasu         | Message.ExpiresAtUTC     | -                                                          |
| Tworzenie\_czasu       | n/d!                      | -                                                          |
| Grupa\_identyfikator            | Message.SessionId        | -                                                          |
| Grupa\_sekwencji      | -                          | -                                                          |
| odpowiedź\_do\_grupy\_identyfikator | Message.ReplyToSessionId | -                                                          |
| Formatowanie               | n/d!                      | -

#### <a name="service-bus-net-apis-to-proton-php"></a>Usługa Bus interfejsy API .NET do protonów PHP

| Usługa Bus .NET        | Protonów PHP                                             | Notatki                                                  |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------|
| Typ zawartości             | Wiadomości -\>zawartości\_typu                                | -                                                        |
| CorrelationId           | Wiadomości -\>korelacji\_identyfikator                              | -                                                        |
| EnqueuedTimeUtc         | Wiadomości -\>adnotacje [x i-został umieszczony w kolejce time]             | -                                                        |
| Etykieta                   | Wiadomości -\>tematu                                      | -                                                        |
| Identyfikator komunikatu               | Wiadomości -\>identyfikator                                           | -                                                        |
| Parametr from                 | Wiadomości -\>odpowiedź\_do                                    | -                                                        |
| ReplyToSessionId        | Wiadomości -\>odpowiedź\_do\_grupy\_identyfikator                         | -                                                        |
| ScheduledEnqueueTimeUtc | Wiadomości -\>adnotacje ["x i zaplanowane-kolejkowania wywołań zwrotnych time"] | -                                                        |
| Identyfikator sesji               | Wiadomości -\>grupy\_identyfikator                                    | -                                                        |
| Licznika TimeToLive              | Wiadomości -\>ttl                                          | Konwersja, PHP protonów TTL jest definiowana (w milisekundach). |
| Aby                      | Wiadomości -\>adresu                                      | -                                                        |

## <a name="next-steps"></a>Następne kroki

Chcesz dowiedzieć się więcej? Skorzystaj z poniższych łączy:

- [Omówienie usług Bus AMQP]
- [AMQP w Bus usługi dla systemu Windows Server]


[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP w Bus usługi dla systemu Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
[Omówienie usług Bus AMQP]: service-bus-amqp-overview.md
