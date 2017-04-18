<properties
    pageTitle="Azure urządzenia IoT SDK C - serializatora | Microsoft Azure"
    description="Więcej informacji o używaniu biblioteki serializatora na urządzeniu Azure IoT SDK c"
    services="iot-hub"
    documentationCenter=""
    authors="olivierbloch"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/06/2016"
     ms.author="obloch"/>

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-serializer"></a>Microsoft Azure IoT urządzenia SDK C — więcej informacji na temat serializatora

[Pierwszego artykułu](iot-hub-device-sdk-c-intro.md) z tej serii wprowadzona **urządzenia Azure IoT SDK c**. Następny artykuł udostępniana bardziej szczegółowy opis [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md). W tym artykule wykonuje zapotrzebowania zestawu SDK oferując bardziej szczegółowy opis pozostałej części: biblioteki **serializatora** .

Artykuł wprowadzający opisano sposób używania biblioteki **serializatora** do zdarzenia do wysyłania i odbierania wiadomości z Centrum IoT. W tym artykule Rozszerzamy tej dyskusji, dostarczając dokładniejszy opis modelu danych za pomocą języka makra **serializatora** . Artykuł również zawiera szczegółowe informacje o jak biblioteki serializes wiadomości (i w niektórych przypadkach, jak można kontrolować zachowanie szeregowania). Będzie również opisano niektóre parametry, które można modyfikować określające rozmiar modeli, które można utworzyć.

Na koniec tego artykułu revisits niektóre tematy w poprzednich artykułach, takich jak wiadomości i obsługi właściwości. Jak możemy się dowiedzieć, te funkcje działają w taki sam sposób używania biblioteki **serializatora** , jak w bibliotece **IoTHubClient** .

Wszystkie opisane w tym artykule jest oparty na próbki SDK **serializatora** . Jeśli chcesz wykonać opisane czynności, zobacz **simplesample\_amqp** i **simplesample\_http** aplikacji zawarte w urządzeniu Azure IoT SDK C.

Możesz odnaleźć **urządzenia Azure IoT SDK c** repozytorium GitHub [Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) i wyświetlanie szczegółów interfejsu API [C API odwołania](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-modeling-language"></a>Język modelowania

[Artykuł wprowadzający](iot-hub-device-sdk-c-intro.md) w tej serii wprowadzone **urządzenia Azure IoT SDK c** modelowanie języka za pomocą przykładzie przedstawionym w **simplesample\_amqp** aplikacji:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Jak widać, język modelowania jest oparty na C makra. Zawsze należy zacząć do definicji z **początkowego\_nazw** i zawsze kończą się **zakończenia\_nazw**. Są często nadać nazwę obszaru nazw dla Twojej firmy lub, jak w poniższym przykładzie projektu, którą pracujesz.

Co przechodzi w obszarze nazw są definicje modeli. W tym przypadku jest pojedynczy model dla anemometer. Ponownie modelu może być nazwany nic, ale zwykle jest to urządzenie lub typ danych, który chcesz wymieniać z Centrum IoT.  

Modele zawierają definicji zdarzeń można wchodzącego do koncentratora IoT ( *danych*) oraz otrzymywanych z Centrum IoT ( *Akcje*) wiadomości. Jak widać z przykładu, wydarzenia mają typ i nazwę; akcji ma nazwę i parametry opcjonalne (do każdego typu).

Co nie przedstawiono w tym przykładzie przedstawiono dodatkowe typy danych obsługiwane przez zestaw SDK. Omówiono tego dalej.

> [AZURE.NOTE] Centrum IoT odwołuje się do danych, które urządzenia są wysyłane do go jako *zdarzenia*, gdy język modelowania odwołuje się do niego jako *danych* (zdefiniowane za pomocą **WITH_DATA**). Analogicznie Centrum IoT odwołuje się do danych, które wysyłasz do urządzenia jako *wiadomości*, gdy język modelowania odwołuje się do niego jako *Akcje* (zdefiniowane za pomocą **WITH_ACTION**). Należy pamiętać, że tych terminów mogą być używane zamiennie w tym artykule.

### <a name="supported-data-types"></a>Obsługiwane typy danych

W modelach utworzone za pomocą biblioteki **serializatora** obsługiwane są następujące typy danych:

| Typ                    | Opis                            |
|-------------------------|----------------------------------------|
| podwójne                  | Podwójna precyzja przestawny numer punktu |
| int                     | Liczba całkowita 32-bitowa                         |
| Przestaw                   | Liczba punktu przestawny pojedynczej precyzji |
| długie                    | Liczba całkowita długa                           |
| int8\_t                 | 8-bitową wartość typu integer                          |
| Int16\_t                | Liczba całkowita 16-bitowy                         |
| Int32\_t                | Liczba całkowita 32-bitowa                         |
| Int64\_t                | Liczba całkowita 64-bitowej                         |
| wartość logiczna                    | wartość logiczna                                |
| ASCII\_znak\_ptr        | Ciąg ASCII                           |
| EDM\_daty\_czasu\_przesunięcie | Przesunięcie czasu daty                       |
| EDM\_GUID               | IDENTYFIKATOR GUID                                   |
| EDM\_binarny             | format binarny                                 |
| ZADEKLARUJ\_struktury         | Typ złożonych danych                      |

Zacznijmy od ostatniego typu danych. **DECLARE\_struktury** umożliwia definiowanie złożonymi typami danych, które są grupami innych typów pierwotnych. Te grupy zezwalają na definiowanie modelu, który wygląda następująco:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Nasze model zawiera zdarzenie pojedynczego danych typu **TestType**. **TestType** jest typ złożony, który zawiera kilka członkowie wspólnie wskazujące typy pierwotne obsługiwanych przez **serializator** język modelowania.

Model tak możemy napisać kod wysyłanie danych do koncentratora IoT, który wygląda następująco:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

Firma Microsoft jest zasadniczo przypisywania wartości do każdego członka struktury **Test** i nawiązywania połączeń **SendAsync** wysyłanie **Test** zdarzenie danych do chmury. **SendAsync** jest funkcją Pomocnik wysyłające zdarzenia pojedynczego danych do koncentratora IoT:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Ta funkcja serializes zdarzenie danego danych i wysyła go do Centrum IoT przy użyciu **IoTHubClient\_SendEventAsync**. To jest ten sam kod omówiony w poprzednich artykułach (**SendAsync** obejmuje logikę wygodnych funkcji).

Jeden innych funkcji Pomocnik używane w poprzednim kodzie jest **GetDateTimeOffset**. Ta funkcja przekształca danej chwili w wartość typu **EDM\_daty\_czasu\_przesunięcie**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Po uruchomieniu tego kodu do Centrum IoT są wysyłane następujący komunikat:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Należy zauważyć, że szeregowania JSON, który jest formatem wygenerowane przez bibliotekę **serializatora** . Pamiętaj również zgodność każdego członka seryjnych obiektu JSON członków **TestType** zdefiniowanego w naszym modelu. Wartości również dokładnie odpowiadać używanych w kodzie. Jednak należy zauważyć, że dane binarne kodowane base64: "AQID" jest base64 kodowanie {0x01, 0x02, 0x03}.

W tym przykładzie przedstawiono korzyścią ze stosowania biblioteki **serializatora** — pozwala na wysyłanie JSON w chmurze bez konieczności jawnego zajmowania szeregowania w naszym aplikacji. Wszystkie nasze martwić się o to ustawienie wartości zdarzenia danych naszego modelu, a następnie wywoływania prosty interfejsów API, aby wysłać te zdarzenia w chmurze.

Za pomocą tych informacji firma Microsoft definiować modeli obejmujących zakres obsługiwane typy danych, w tym typów złożonych (Firma Microsoft może nawet dołączyć złożonych typów w innych typów złożonych). Jednak on seryjnych JSON wygenerowane przez powyższy przykład otworzy ważnym. Określa *sposób* wyślemy danych z biblioteką **serializatora** dokładnie tak, jak JSON został utworzony. Konkretnej jest co opisane dalej.

## <a name="more-about-serialization"></a>Więcej informacji na temat szeregowania

Przykład wyniki generowane przez biblioteki **serializator** jest wyróżniana poprzedniej sekcji. W tej sekcji możemy wyjaśniono sposób biblioteki serializes danych i jak można kontrolować to zachowanie przy użyciu szeregowania interfejsów API.

Aby przechodzić między dyskusji w szeregowania, firma Microsoft będzie pracować nad nowy model według termostacie. Najpierw Przyjrzyjmy Wprowadź niektóre tła na scenariusz chcemy adres.

Chcemy modelu termostat, która temperatury i wilgotności. Każdy fragment danych będzie wysyłane do koncentratora IoT inaczej. Domyślnie ingresses termostat zdarzenia temperatury raz na 2 minuty; Zdarzenie wilgotność jest ingressed co 15 minut. W przypadku ingressed albo zdarzenia musi zawierać pokazujący czas był mierzony odpowiednich temperatury lub wilgotność sygnatura czasowa.

Podane w tym scenariuszu, zostanie przedstawiona dwa różne sposoby modelu danych i firma Microsoft będzie wyjaśnić efekt czy modelowania ma seryjnych produkcja.

### <a name="model-1"></a>Model 1

Oto pierwsza wersja modelu, który obsługuje poprzednim scenariuszu:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Uwaga, że model zawiera dwa zdarzenia danych: **temperatury** i **wilgotności**. W odróżnieniu od poprzednich przykładach typ każde zdarzenie jest strukturą zdefiniowane przy użyciu **DECLARE\_struktury**. **TemperatureEvent** zawiera miary temperatury i sygnatura czasowa; **HumidityEvent** zawiera miary wilgotność i sygnatura czasowa. Ten model daje naturalnym narzędziem do modelu danych na potrzeby tego scenariusza opisany powyżej. Gdy wyślemy zdarzenie w chmurze albo wyślemy temperatury i sygnatury czasowej lub parę wilgotność i sygnatury czasowej.

Otrzymasz zdarzenie temperatury w chmurze przy użyciu kodu, takie jak następujące czynności:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Firma Microsoft będzie używać ustalonych wartości dla temperatury i wilgotności w kodzie przykładowych, ale Załóżmy możemy przez próbki odpowiednich czujników na termostat faktycznie jest pobieranie tych wartości.

Powyższy kod używa Pomocnik **GetDateTimeOffset** , który został już wprowadzony. Powodów, które staną się wyczyść później kod jawnie rozdzielający zadanie szeregowania i wysyłanie wydarzenia. Poprzedni kod serializes zdarzenia temperatury do buforu. Następnie **sendMessage** jest funkcją Pomocnik (zawarte w **simplesample\_amqp**) do koncentratora IoT wysyłają zdarzenia:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Ten kod jest podzbiór Pomocnik **SendAsync** opisane w poprzedniej sekcji, więc możemy nie zasoby nad nim ponownie.

Uruchomienie poprzedniego kod, aby wysłać zdarzenia temperatury, ten formularz seryjne wydarzenia są wysyłane do Centrum IoT:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Firma Microsoft wysyłasz temperaturę, która jest typu **TemperatureEvent** i tej struktury zawiera element **temperatury** i **czasu** . Jest to bezpośrednio uwzględniany na seryjne danych.

Podobnie otrzymasz zdarzenie wilgotność kod:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Seryjny formularza, który są wysyłane do Centrum IoT wygląda następująco:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Ponownie jest zgodnie z oczekiwaniami.

W tym modelu można Załóżmy jak dodatkowe zdarzenia można łatwo dodawać. Należy zdefiniować więcej struktury przy użyciu **DECLARE\_struktury**i zawiera odpowiednie zdarzenie za pomocą modelu **z\_danych**.

Teraz Przyjrzyjmy Modyfikowanie modelu, aby uwzględniała tych samych danych, ale o innej strukturze.

### <a name="model-2"></a>Model 2

Należy rozważyć alternatywny modelu przedstawiony powyżej:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

W tym przypadku firma Microsoft zostały usunięte **DECLARE\_struktury** makr i po prostu definiowania elementów danych z naszych scenariuszy przy użyciu prostej typów z języka modelowania.

Tylko na chwilę Przejdźmy Ignoruj zdarzenia **czasu** . Z tego odłogowania Oto kod ingress **temperatury**:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Kod wysyła następujące zdarzenie seryjnych do Centrum IoT:

```
{"Temperature":75}
```

I kod do wysyłania zdarzeń wilgotność jest wyświetlana w następujący sposób:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Kod wysyła to do Centrum IoT:

```
{"Humidity":45}
```

Ile istnieje nadal nie niespodzianki. Teraz Przyjrzyjmy Zmienianie, jak firma Microsoft korzysta z makra SERYJNE.

Makro **SERYJNE** można wykonać wiele zdarzeń danych jako argumenty. Pozwala na razem szeregować zdarzenia **temperatury** i **wilgotności** i wysyłać te Centrum IoT w jednym połączenia:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Może być przypuszczenie, że wynik ten kod jest który zdarzenia dwóch danych są wysyłane do koncentratora IoT:

[

{"Temperatura": 75},

{"Wilgotność": 45}

]

Innymi słowy może oczekiwać, że kod jest taka sama, jak wysyłanie **temperatury** i **wilgotności** oddzielnie. Jest po prostu wygody w celu przekazania obu zdarzeń do **SERYJNE** , w tym samym połączenia. Jednak nie jest wielkość liter. Zamiast tego powyższy kod wysyła to zdarzenie pojedynczego danych do Centrum IoT:

{"Temperatura": 75 "wilgotność": 45}

Może to wyglądać dziwne, ponieważ naszego modelu definiuje **temperatury** i **wilgotności** dwóch *oddzielnych* zdarzeń:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Więcej do punktu, firma Microsoft nie modelu te zdarzenia, gdzie są **temperatury** i **wilgotności** w tej samej strukturze:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Użycie tego modelu, będzie bardziej zrozumiały, jak **temperatury** i **wilgotności** zostanie wysłany w tej samej seryjnych wiadomości. Jednak może nie być wyczyść Dlaczego to działa w ten sposób, jeśli oba zdarzenia danych do **SERYJNE** przy użyciu modelu 2.

To zachowanie jest łatwiejszy do zrozumienia, jeśli wiesz, założeń, które wprowadza biblioteki **serializatora** . Aby zorientować się, to wróć do naszego modelu:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Tego modelu można traktować w kategoriach obiektowych. W tym przypadku mamy już modelowania urządzenia fizycznego (termostacie) i urządzenia zawiera atrybutów, takich jak **temperatury** i **wilgotności**.

Otrzymasz całego stan naszych modelu przy użyciu kodu, takie jak:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Przy założeniu wartości temperatury, wilgotności i czasu są ustawione, czy widać zdarzenie tak wysyłane do Centrum IoT:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Czasami może być tylko chcesz wysłać *niektóre* właściwości modelu w chmurze (jest to zwłaszcza wtedy, gdy model zawiera dużej liczby zdarzenia danych). Warto wysłać tylko niektóre zdarzenia danych, takich jak w naszym przykładzie wcześniejszej:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Spowoduje to wygenerowanie dokładnie tego samego zdarzenia seryjnych tak, jakby była firma Microsoft zdefiniowane **TemperatureEvent** z członkiem **temperatury** i **czasu** tak, jak NAS z modelem 1. W tym przypadku możemy wygenerować dokładnie tego samego zdarzenia seryjnych za pomocą innego modelu (model 2), ponieważ możemy o nazwie **SERYJNE** w inny sposób.

Istotne jest wiele zdarzeń danych w przypadku przekazania w celu **SERYJNE,** założono, że każde zdarzenie jest właściwością w jeden obiekt JSON.

Najlepszym rozwiązaniem zależy od tego, i jak wziąć pod uwagę modelu. Jeśli wysyłasz "zdarzenia" w chmurze i każde zdarzenie zawiera zdefiniowane zestaw właściwości, pierwszą metodę stanowi wiele właściwe rozwiązanie. W takim przypadku można użyć **DECLARE\_struktury** do definiowania struktury każde zdarzenie, a następnie je uwzględnić w modelu przy użyciu **z\_danych** makra. Następnie możesz wysłać każde zdarzenie, możemy jak w powyższym przykładzie pierwszy. W tej metody zdarzenie pojedynczego danych chcesz przekazać tylko do **SERIALIZATORA**.

Jeśli podejmowaniu modelu w sposób obiektowych drugiej metody może być odpowiednio możesz. W tym przypadku elementy zdefiniowane przy użyciu **z\_danych** "właściwości" obiektu. Niezależnie od podzbiór zdarzeń są przekazywane do **SERYJNE** podoba Ci się, w zależności od ilości stan usługi "obiektu" ma zostać wysłany do chmury.

Metoda nether jest nieprawidłowy lub w prawo. Po prostu należy pamiętać o działania biblioteki **serializatora** i wybierz metoda modelowania, który najlepiej pasuje do potrzeb.

## <a name="message-handling"></a>Postępowanie z wiadomościami

Ile w tym artykule tylko został omówiony wysyłania zdarzeń do koncentratora IoT, a nie zaadresowaną odbierania wiadomości. Przyczyną to, że usługa co należy wiedzieć o odbierania wiadomości wiadomości ma przeważnie objęte w [artykule wcześniejszych](iot-hub-device-sdk-c-intro.md). Odwoływanie z tego artykułu, przetwarzania wiadomości zarejestrowanie funkcji zwrotnego wiadomości:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Następnie napisać funkcji zwrotnego, która jest wywoływana po odebraniu wiadomości:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Ta implementacja **IoTHubMessage** połączeń określoną funkcję dla każdej akcji w modelu. Jeśli na przykład modelu definiuje tej akcji:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Należy zdefiniować funkcję z tego podpisu:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** jest następnie wywoływana, gdy wiadomości są wysyłane do urządzenia.

Jeszcze co jeszcze nie możemy wyjaśniono to wygląda seryjne wersję wiadomości. Innymi słowy Jeśli chcesz wysłać wiadomość **SetAirResistance** na urządzeniu, które jak wygląda?

Jeśli wiadomość jest wysyłana do urządzenia, czy Zrób to za pośrednictwem usługi Azure IoT SDK. Nadal trzeba wiedzieć, jak ciąg znaków, aby wysłać do wywołania określonej akcji. Ogólny format wysyłania wiadomości wygląda następująco:

```
{"Name" : "", "Parameters" : "" }
```

Wysyłasz seryjnych obiektu JSON z dwiema właściwościami: **Nazwa** to nazwa akcji (wiadomości), a **parametrów** zawiera parametry działania.

Na przykład do wywołania **SetAirResistance** możesz wysłać tę wiadomość do urządzenia:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

Nazwa akcji muszą dokładnie odpowiadać akcji zdefiniowane w modelu. Nazwy parametrów musi odpowiadać także. Pamiętaj również uwzględnianie wielkości liter. **Nazwa** i **Parametry** są zawsze wielkimi literami. Upewnij się, Uwzględnij wielkość liter nazwa akcji i parametrów w modelu. W tym przykładzie nazwa akcji jest "SetAirResistance", a nie "setairresistance".

W tej sekcji opisano wszystko, co należy wiedzieć, kiedy wydarzeń wysyłanie i odbieranie wiadomości z biblioteką **serializatora** . Przed przejściem, Przejdźmy omówiono niektóre parametry, możesz skonfigurować sterujące, jak duży jest modelu.

## <a name="macro-configuration"></a>Konfiguracja makra

Jeśli używasz biblioteki **serializatora** ważną część zestawu SDK, o których warto pamiętać znajduje się w bibliotece azure-c udostępnione — narzędzie.
Jeśli masz sklonowany repozytorium Azure-iot SDK z GitHub za pomocą opcji — cykliczne, spowoduje znalezienie tej biblioteki narzędzie udostępnionego:

```
.\\c\\azure-c-shared-utility
```

Jeśli nie masz sklonowany biblioteki, możesz znaleźć [w tym miejscu](https://github.com/Azure/azure-c-shared-utility).

W bibliotece udostępnionego narzędzie znajdzie się następującym folderze:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Ten folder zawiera rozwiązania programu Visual Studio o nazwie **makra\_witryny\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

Program w rozwiązaniu generuje **makra\_utils.h** pliku. Istnieje domyślne makro\_plik utils.h dołączony do zestawu SDK. To rozwiązanie pozwala na niektórych parametrów modyfikowanie i ponowne tworzenie pliku nagłówka na podstawie następujących parametrów.

Dwa parametry klucza zajmowanie się są **nArithmetic** i **nMacroParameters** są definiowane w tych dwóch wierszy w makro\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Wartości te są dołączone do zestawu SDK domyślne parametry. Każdy parametr ma następujące znaczenie:

-   nMacroParameters — określa liczbę parametrów w jednym DECLARE może być używana\_definicji makra modelu.

-   nArithmetic — określa całkowitą liczbę członków dozwolone w modelu.

Powodem tych parametrów są ważne jest, ponieważ kontrolować ich rozmiar, jaki może być modelu. Na przykład można rozważyć tej definicji modelu:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Jak już wspomniano wcześniej, **DECLARE\_modelu** jest po prostu makro C. Nazwy modelu i **z\_danych** instrukcji (jeszcze innego makra) są parametry **DECLARE\_modelu**. **nMacroParameters** określa liczbę parametrów mogą być zawarte w **DECLARE\_modelu**. Definiuje skutecznie, ile danych zdarzeń i akcji deklaracji możesz mieć. W związku z domyślny limit 124 oznacza to, który można zdefiniować za pomocą kombinacji około 60 działań i zdarzeń danych modelu. Jeśli spróbujesz przekracza ten limit, otrzymasz błędy kompilatora, które wyglądają podobnie do następującej:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

Parametr **nArithmetic** jest więcej informacji na temat wewnętrzne działanie makra języka niż aplikacja.  Każdy z nich steruje całkowitą liczbę członków, których możesz mieć w modelu, łącznie z **DECLARE_STRUCT** makr. Jeśli zaczniesz są wyświetlane błędy kompilatora, takich jak, następnie należy spróbować zwiększenie **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Jeśli chcesz zmiany tych parametrów, zmodyfikuj wartości w makrze\_utils.tt plików, ponownie skompilować makra\_witryny\_h\_rozwiązanie generator.sln i uruchom program skompilowany. Po wykonaniu tej czynności nowe makro\_wygenerowane i umieszczone w pliku utils.h. \\typowe\\Inc. katalogu.

Aby można było korzystać z nowej wersji makra\_utils.h, usunąć pakiet NuGet **serializatora** rozwiązanie, a w jego miejscu dołączyć projekt programu Visual Studio **serializatora** . Dzięki temu kod skompilować na kod źródłowy biblioteki serializatora. Ta opcja uwzględnia zaktualizowane makra\_utils.h. Jeśli chcesz to zrobić dla **simplesample\_amqp**, zacznij od usunięcie pakietu NuGet w bibliotece serializatora z rozwiązania:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Następnie dodać ten projekt do rozwiązania programu Visual Studio:

> . \\c\\serializatora\\tworzenie\\windows\\serializer.vcxproj

Po wykonaniu tych rozwiązania powinna wyglądać następująco:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Teraz podczas kompilowania rozwiązania zaktualizowane makra\_utils.h znajduje się w swojej binarny.

Zauważ, że te wystarczająco wysoka wartościami rosnącymi może przekroczyć limity kompilatora. Do tego punktu **nMacroParameters** jest główny parametr, z której ma dotyczyć. Specyfikacja C99 Określa, że mogą co najmniej 127 parametrów w definicji makra. Kompilatora Microsoft dokładnie obejmuje użycie (i ma limit 127), więc nie można zwiększyć **nMacroParameters** poza domyślny. Jak inne mogą umożliwiają zrobić (na przykład kompilatora GNU obsługuje wyższą limit).

Pory firma Microsoft zostały omówione, niemal wszystko, co należy wiedzieć o kodzie z biblioteką **serializatora** . Przed zawierania Przejdźmy odwiedzaj niektóre tematy z poprzednich artykułów, które być może zastanawiasz się o.

## <a name="the-lower-level-apis"></a>Interfejsy API niższego poziomu

Przykładowa aplikacja, na którym ograniczony w tym artykule **simplesample\_amqp**. W przykładzie użyto wyższego poziomu (non-"a") interfejsów API do zdarzenia wysyłania i odbierania wiadomości. Użycie te interfejsy API wątek tła uruchamia, która przyjmuje istotnych zdarzeń wysyłania i odbierania wiadomości. Jednak może zawierać interfejsy API (wszystkie) niższego poziomu wyeliminować tego wątku tła i kontrolować jawne przez wysłanie zdarzenia lub odbierać wiadomości z chmury.

Opisane w [artykule poprzedniego](iot-hub-device-sdk-c-iothubclient.md)jest zestaw funkcji składa się z wyższego poziomu interfejsy API:

-   IoTHubClient\_CreateFromConnectionString

-   IoTHubClient\_SendEventAsync

-   IoTHubClient\_SetMessageCallback

-   IoTHubClient\_Destroy

Te interfejsy API przedstawiono w **simplesample\_amqp**.

Istnieje także analogiczny zestaw interfejsów API niższego poziomu.

-   IoTHubClient\_a\_CreateFromConnectionString

-   IoTHubClient\_a\_SendEventAsync

-   IoTHubClient\_a\_SetMessageCallback

-   IoTHubClient\_a\_Destroy

Należy zauważyć, że interfejsy API niższego poziomu działa dokładnie tak samo zgodnie z opisem w poprzednich artykułach. Jeśli chcesz, aby wątek tła do obsługi zdarzeń wysyłanie i odbieranie wiadomości, można użyć pierwszy zestaw interfejsów API. Drugi zestaw interfejsów API jest używane, jeśli chcesz mieć jawne kontrolę nad podczas wysyłania i odbierania danych z Centrum IoT. Skonfigurowany interfejsy API pracy oraz równo z biblioteką **serializatora** .

Przykład użycia interfejsów API niższego poziomu z biblioteką **serializatora** zobacz **simplesample\_http** aplikacji.

## <a name="additional-topics"></a>Dodatkowe tematy

Kilka inne tematy warte tworzenie wzmianki o ponownie należą obsługi, przy użyciu poświadczeń alternatywnego urządzenia i opcje konfiguracji. Są to wszystkich tematów dotyczących usługi zawarte w [artykule poprzedniego](iot-hub-device-sdk-c-iothubclient.md). Główny punkt jest, że tak samo, jak z biblioteką **IoTHubClient** wszystkie te funkcje do pracy w taki sam sposób z biblioteką **serializatora** . Na przykład, jeśli chcesz dołączyć właściwości do wydarzenia z modelu, możesz użyć **IoTHubMessage\_właściwości** i **Map**\_**AddorUpdate**, taki sam sposób, jak opisano powyżej:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Zdarzenia został wygenerowany z biblioteki **serializatora** czy utworzony ręcznie przy użyciu biblioteki **IoTHubClient** nie ma znaczenia.

Dla poświadczeń alternatywnego urządzenia, za pomocą **IoTHubClient\_wszystkie\_tworzenie** działa tylko także jako **IoTHubClient\_CreateFromConnectionString** przyznawania **IOTHUB\_klienta\_obsługiwać**.

Na koniec, jeśli używasz biblioteki **serializatora** można ustawić opcje konfiguracji z **IoTHubClient\_a\_SetOption** tylko tak samo jak w przypadku korzystania z biblioteki **IoTHubClient** .

Funkcja, który jest unikatowy dla biblioteki **serializatora** są inicjowania interfejsów API. Przed rozpoczęciem pracy z biblioteką, musisz wywołać **serializatora\_inicjowania**:

```
serializer_init(NULL);
```

Można to zrobić tylko, zanim zadzwonisz do **IoTHubClient\_CreateFromConnectionString**.

Podobnie, po zakończeniu pracy z biblioteką, ostatniego połączenia należy podjąć jest **serializatora\_deinit**:

```
serializer_deinit();
```

W przeciwnym razie wszystkich innych funkcji wymienionych powyżej działa tak samo w bibliotece **serializatora** tak samo, jak w bibliotece **IoTHubClient** . Aby uzyskać więcej informacji na temat dowolnego z tych tematów zobacz [artykuł poprzedniego](iot-hub-device-sdk-c-iothubclient.md) z tej serii.

## <a name="next-steps"></a>Następne kroki

W tym artykule opisano szczegółowo unikatowe aspekty biblioteki **serializatora** zawarte w **urządzeniu Azure IoT SDK c**. Przy użyciu informacji pod warunkiem, że masz odpowiednie zrozumienie sposobu używania modeli do zdarzenia wysyłanie i odbieranie wiadomości z Centrum IoT.

Zakończenie również serii trzyczęściowe na temat tworzenia aplikacji z **urządzenia Azure IoT SDK c**. Należy to wystarczających informacji, aby nie tylko rozpoczęcie pracy, ale umożliwiają poznanie zasad działania za pośrednictwem interfejsów API. Aby uzyskać dodatkowe informacje istnieje kilka przykładów w SDK nie opisane tutaj. W przeciwnym razie [dokumentacji SDK](https://github.com/Azure/azure-iot-sdks) jest dobrym zasobów, aby uzyskać dodatkowe informacje.


Aby dowiedzieć się więcej o tworzeniu IoT koncentratora, zobacz [SDK Centrum IoT][lnk-sdks].

Aby dodatkowo Poznawanie możliwości Centrum IoT, zobacz:

- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
