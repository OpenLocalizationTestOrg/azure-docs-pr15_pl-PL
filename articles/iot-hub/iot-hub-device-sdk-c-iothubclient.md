<properties
    pageTitle="Azure urządzenia IoT SDK C - IoTHubClient | Microsoft Azure"
    description="Więcej informacji o używaniu biblioteki IoTHubClient na urządzeniu Azure IoT SDK c"
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

# <a name="microsoft-azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Microsoft Azure IoT urządzenia SDK C — więcej informacji na temat IoTHubClient

[Pierwszego artykułu](iot-hub-device-sdk-c-intro.md) z tej serii wprowadzona **urządzenia Microsoft Azure IoT SDK c**. Tym artykule wyjaśniono, że istnieją dwa warstw architektury SDK. Na podstawie jest biblioteki **IoTHubClient** , która zarządza bezpośrednio komunikowanie się z Centrum IoT. Istnieje także **serializatora** bibliotekę, w której tworzy znajdujący się na świadczenie usług szeregowania. W tym artykule będzie udostępniamy dodatkowe szczegóły w bibliotece **IoTHubClient** .

Artykuł poprzedniego opisano sposób używania biblioteki **IoTHubClient** do wysyłania zdarzeń do koncentratora IoT i odbierania wiadomości. W tym artykule rozszerza tej dyskusji przez wyjaśnieniem dotyczącym sposobu dokładniej zarządzać *podczas* wysyłania i odbierania danych, wprowadzenie do **interfejsów API niższego poziomu**. Firma Microsoft będzie również wyjaśniono, jak dołączyć właściwości dotyczące zdarzeń (i pobieranie ich z wiadomości) przy użyciu właściwości obsługi funkcji w bibliotece **IoTHubClient** . Na koniec będzie firma Microsoft udostępnia dodatkowe objaśnienia różne sposoby obsługi wiadomości odebranych od centrum IoT.

Artykuł kończy się za obejmujące kilka dodatkowych tematów, tym bardziej o poświadczenia urządzenia i jak zmienić zachowanie **IoTHubClient** za pomocą opcji konfiguracji.

Użyjemy próbki SDK **IoTHubClient** opisującą następujące tematy. Jeśli chcesz wykonać opisane czynności, zobacz **iothub\_klienta\_próbki\_http** i **iothub\_klienta\_próbki\_amqp** aplikacji, które znajdują się w urządzeniu Azure IoT SDK dla C. wszystko, co opisano w poniższych sekcjach przedstawiono w tych próbkach.

Możesz odnaleźć **urządzenia Azure IoT SDK c** repozytorium GitHub [Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) i wyświetlanie szczegółów interfejsu API [C API odwołania](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

## <a name="the-lower-level-apis"></a>Interfejsy API niższego poziomu

Artykuł poprzedniego opisano podstawowe operacje **IotHubClient** w kontekście **iothub\_klienta\_próbki\_amqp** aplikacji. Na przykład wyjaśniono jak zainicjować biblioteki przy użyciu następującego kodu.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

On również opisany wysyłania zdarzeń przy użyciu wywołanie tej funkcji.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Artykuł również opisem sposobu odbierania wiadomości przez proces rejestracji funkcji zwrotnego.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Artykuł również pokazano, jak w celu zwolnienia zasobów za pomocą następującego kodu.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Istnieją jednak funkcje pomocniczym dla każdego z tych interfejsów API:

-   IoTHubClient\_a\_CreateFromConnectionString

-   IoTHubClient\_a\_SendEventAsync

-   IoTHubClient\_a\_SetMessageCallback

-   IoTHubClient\_a\_Destroy


Te funkcje wszystkich nazwa może zawierać "A" interfejsu API. Oprócz, parametry każda z tych funkcji są identyczne do swoich odpowiedników nie wszystkie. Jednak zachowanie te funkcje różni się w jednym ze sposobów ważne.

Gdy połączenie telefoniczne **IoTHubClient\_CreateFromConnectionString**, źródłowych bibliotek utworzyć nowego wątku, która działa w tle. Ten wątek wysyła zdarzenia i odbiera wiadomości z Centrum IoT. Nie takich wątku jest tworzona podczas pracy z interfejsów API "a". Tworzenie wątku tło jest wygody przez dewelopera. Nie musisz martwić jawnie zdarzenia wysyłania i odbierania wiadomości z Centrum IoT — wykonywane automatycznie w tle. Natomiast "A" interfejsy API pozwalają jawne kontrolę komunikowanie się z Centrum IoT, jeśli jest potrzebna.

Aby lepiej zrozumieć to, Spójrzmy na przykład:

Gdy połączenie telefoniczne **IoTHubClient\_SendEventAsync**, faktycznie wykonywanej jest wprowadzenie wydarzenia buforu. Wątku tła utworzone podczas połączenia **IoTHubClient\_CreateFromConnectionString** stale monitoruje bufor i wysyła dane, które zawiera do Centrum IoT. W tym samym czasie, że głównym wątku wykonuje innych prac dzieje się w tle.

Podobnie, po zarejestrowaniu funkcji zwrotnego dla wiadomości przy użyciu **IoTHubClient\_SetMessageCallback**, w przypadku jeśli SDK ma wątku tła wywołania funkcji wywołania zwrotnego w przypadku wiadomości przychodzących, niezależnie od wątku głównego.

"Wszystkie" interfejsy API nie utworzyć wątku tła. Zamiast tego nowego interfejsu API usługi musi zostać wywołany jawnie wysyłanie i odbieranie danych z Centrum IoT. To przedstawiono w poniższym przykładzie.

**Iothub\_klienta\_próbki\_http** aplikacji, w której znajduje się w zestawie SDK zaprezentowano interfejsy API niższego poziomu. W tym przykładzie wyślemy zdarzeń do koncentratora IoT z kodem, takie jak następujące czynności:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

Pierwsze trzy wiersze Tworzenie wiadomości i ostatni wiersz wysyła zdarzenia. Jednak jak już wspomniano, "wysłanie" zdarzenie oznacza, że dane jest po prostu umieścić buforu. Nic nie jest przesyłane w sieci, gdy nazywamy **IoTHubClient\_a\_SendEventAsync**. W celu faktycznie ingress danych do koncentratora IoT, musisz wywołać **IoTHubClient\_a\_DoWork**w tym przykładzie:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Kod (z **iothub\_klienta\_próbki\_http** aplikacji) wielokrotnie wywołuje **IoTHubClient\_a\_DoWork**. Zawsze **IoTHubClient\_a\_DoWork** zostanie wywołana, wysyła niektóre zdarzenia z bufora do koncentratora IoT i jego pobiera wiadomość w kolejce są wysyłane do urządzenia. Ostatnim przypadku oznacza, że jeśli firma Microsoft zarejestrowany takiej funkcji dla wiadomości, następnie wywołanie zwrotne jest wywoływana (przy założeniu, że wszystkie wiadomości są umieszczone w kolejce). Firma Microsoft może zarejestrowanych funkcji zwrotnego z kodem, takie jak:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

Przyczyny który **IoTHubClient\_a\_DoWork** jest często nazywane w pętli jest zawsze nazywany, wysyła *niektóre* buforowane zdarzenia do koncentratora IoT i pobiera *następnej* wiadomości umieszczone w kolejce urządzenia. Każdego połączenia nie jest gwarantowana wysłać wszystkie buforowane zdarzenia lub pobranie wszystkich wiadomości z kolejki. Jeśli chcesz wysłać wszystkie zdarzenia w buforu, a następnie kontynuuj na inne przetwarzanie możesz zastąpić pętlę kodu, takie jak następujące czynności:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

To kod wywołuje **IoTHubClient\_a\_DoWork** aż wszystkie pozycje buforu zostały wysłane do Centrum IoT. Należy zauważyć, że to również oznacza otrzymano wszystkich wiadomości w kolejce. Część przyczynę to, że sprawdzanie wiadomości "wszystkie" nie jest jako deterministyczny akcji. Co się stanie, jeśli pobierasz "wszystkie", wiadomości, ale to kolejnego jest wysyłany do urządzenia zaraz po? Lepszym sposobem zajęcie się z zaprogramowanych przekroczenia limitu czasu. Na przykład funkcja zwrotnego wiadomości można zresetować czasomierza za każdym razem, gdy jest wywoływana. Następnie można napisać logiczny kontynuować przetwarzanie, jeśli na przykład wiadomości nie zostały odebrane w ciągu ostatnich *X* sekund.

Po już ukończona ingressing zdarzeń i odbierania wiadomości, pamiętaj połączenie odpowiednich funkcji oczyszczania zasobów.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Zasadniczo jest tylko jeden zestaw interfejsów API do wysyłania i odbierania danych przy użyciu wątku tła i inny zestaw interfejsów API, które ma to samo bez wątku tła. Wiele deweloperów może być lepsze non - wszystkie interfejsów API, ale za pośrednictwem interfejsów API niższego poziomu są przydatne, gdy projektanta chce jawne kontrolę nad transmisji sieci. Na przykład niektóre urządzenia zbieranie danych na czas i zdarzeń ingress tylko w określonych odstępach czasu (na przykład raz na godzinę lub raz dziennie). Interfejsy API niższego poziomu zapewnianie możliwości jawnie kontrolki, podczas wysyłania i odbierania danych z Centrum IoT. Inne osoby będą po prostu woli uproszczenia, które zapewniają interfejsy API niższego poziomu. Wszystko, co się dzieje w głównym wątku zamiast niektórych dzieje pracy w tle.

Niezależnie od tego modelu można wybrać, pamiętaj zachować spójność API, którego używasz. Jeśli zaczniesz, dzwoniąc **IoTHubClient\_a\_CreateFromConnectionString**, należy korzystać jedynie odpowiednich interfejsów API niższego poziomu starań monitowania:

-   IoTHubClient\_a\_SendEventAsync

-   IoTHubClient\_a\_SetMessageCallback

-   IoTHubClient\_a\_Destroy

-   IoTHubClient\_a\_DoWork

PRAWDA jest również odwrotny. Rozpoczyna się **IoTHubClient\_CreateFromConnectionString**, następnie przy użyciu innych niż - wszystkie interfejsów API dla dowolnego dodatkowych czynności.

Na urządzeniu Azure IoT SDK c, zobacz **iothub\_klienta\_próbki\_http** aplikacji pełny przykład interfejsów API niższego poziomu. **Iothub\_klienta\_próbki\_amqp** aplikacji można odwoływać się na przykład pełny non - wszystkie interfejsów API.

## <a name="property-handling"></a>Obsługa właściwości

Pory firma Microsoft zostały opisane wysyłaniu danych, firma Microsoft została zostały odnosi do treści wiadomości. Na przykład można rozważyć kod:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

W tym przykładzie wysyła wiadomość do Centrum IoT z tekstem "Witaj świecie". Centrum IoT umożliwia również właściwości załączany do każdej wiadomości. Właściwości są pary nazwa wartość, które może zostać dołączony do wiadomości. Na przykład możemy zmodyfikować poprzedniego kod, aby dołączyć właściwość do wiadomości:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Firma Microsoft Rozpocznij, łącząc **IoTHubMessage\_właściwości** i przekazanie go uchwytu wiadomości. Co możemy wrócić jest **mapy\_obsługiwać** odwołanie, które pozwala na zacząć dodawać właściwości. Te ostatnie jest realizowane, dzwoniąc **mapy\_AddOrUpdate**, który pobiera odwołanie do mapy\_UCHWYT, nazwa właściwości i wartości właściwości. W tym interfejsie API można dodać dowolną liczbę właściwości, jak firma Microsoft.

Przeczytaniu wydarzenia z **Koncentratorów zdarzenia**odbiorcy można wyliczyć właściwości i pobrać odpowiadające im wartości. Na przykład w .NET to samo osiągnąć po zalogowaniu się do [kolekcji Properties obiektu funkcji EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

W poprzednim przykładzie firma Microsoft jest dołączanie właściwości na zdarzenie wyślemy do koncentratora IoT. Właściwości również może zostać dołączony do wiadomości odebranych od centrum IoT. Jeśli chcemy pobrać właściwości z wiadomości, firma Microsoft korzysta z następującego kodu w naszym funkcji zwrotnego wiadomości:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

Połączenie **IoTHubMessage\_właściwości** zwraca **Mapa\_obsługiwać** odwołania. Firma Microsoft następnie przekazać odwołujące się do **mapy\_GetInternals** celu uzyskania odwołania do tablicy pary nazwa wartość (jak liczba właściwości). W tym momencie jest proste przedmiotem wyliczanie właściwości, aby uzyskać dostęp do wartości, które będą.

Nie należy używać właściwości w aplikacji. Jednak jeśli musisz je ustawia informacje o zdarzeniach lub pobrane z wiadomości biblioteki **IoTHubClient** ułatwia.

## <a name="message-handling"></a>Postępowanie z wiadomościami

Jak wspomniano wcześniej, po nadejściu wiadomości z Centrum IoT biblioteki **IoTHubClient** odpowiada przez funkcję zarejestrowanych zwrotnego. Istnieje parametru zwrotnego tej funkcji, która wymaga dodatkowe objaśnienia. Oto fragment funkcję zwrotnego **iothub\_klienta\_próbki\_http** przykładowej aplikacji:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Należy zauważyć, że typ zwracanej wartości jest **IOTHUBMESSAGE\_DYSPOZYCJI\_wynik** i w tym przypadku zwróconych **IOTHUBMESSAGE\_ZAAKCEPTOWANE**. Istnieją inne wartości, które można zwróconych z tej funkcji zmieniają się zachowaniem biblioteki **IoTHubClient** zwrotnego wiadomości. Poniżej przedstawiono opcje.

-   **IOTHUBMESSAGE\_ZAAKCEPTOWANE** — wiadomość została przetworzona pomyślnie. Biblioteka **IoTHubClient** nie wywoła funkcja zwrotnego ponownie z tego samego komunikatu.

-   **IOTHUBMESSAGE\_ODRZUCONE** — wiadomość nie została przetworzona i jest chęć zrobić w przyszłości. Biblioteka **IoTHubClient** nie powinna wywołać funkcja zwrotnego ponownie z tego samego komunikatu.

-   **IOTHUBMESSAGE\_ABANDONED** — wiadomość nie została przetworzona pomyślnie, ale biblioteki **IoTHubClient** powinno wywołać funkcja zwrotnego ponownie z tego samego komunikatu.

Dla pierwszych dwóch kody zwrotne biblioteki **IoTHubClient** wysyła wiadomość do koncentratora IoT informujący, że zostaną usunięte z kolejki urządzenia i nie dostarczone ponownie wiadomość. Efekt netto jest taki sam (wiadomość zostanie usunięty z kolejki urządzenia), ale nadal jest rejestrowany tego, czy wiadomość została zaakceptowania lub odrzucenia.  Rejestrowanie to rozróżnienie przydaje się do nadawców wiadomości, kto może wykrywać opinii i Dowiedz się, jeśli urządzenie jest zaakceptowane lub odrzucone konkretną wiadomość.

W ostatnim przypadku wiadomość jest wysyłana także do Centrum IoT, ale wskazuje, że należy przed przeniesieniem wiadomości. Zwykle będzie opuszczenie wiadomości, jeśli wystąpienia błędu niektórych, ale chcesz spróbować ponownie przetwarzania wiadomości. Natomiast odrzucanie wiadomości jest odpowiedni, w przypadku wystąpienia błędu odzyskać (lub możesz po prostu okazało się, że nie chcesz, aby przetworzyć wiadomości).

W każdym przypadku należy pamiętać różnych kodów zwrotu tak, aby może wywołać zachowanie, które ma być z biblioteki **IoTHubClient** .

## <a name="alternate-device-credentials"></a>Poświadczenia alternatywnego urządzenia

Jak wyjaśniono wcześniej, najpierw należy zrobić, jeśli praca z biblioteką **IoTHubClient** jest uzyskanie **IOTHUB\_klienta\_obsługiwać** z połączenia, takie jak następujące:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Argumenty **IoTHubClient\_CreateFromConnectionString** są parametry połączenia naszych urządzenia i parametr, który wskazuje, firma Microsoft korzysta z komunikowanie się z Centrum IoT Protocol (protokół). Parametry połączenia ma format, który wygląda następująco:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Istnieją cztery elementy informacji w tym ciągu: Nazwa centrum IoT, Centrum IoT sufiks, identyfikator urządzenia i klawisz dostępu udostępnione. W pełni kwalifikowaną nazwę domeny (FQDN) można uzyskać z Centrum IoT podczas tworzenia wystąpienia Centrum IoT w portalu Azure — umożliwia nazwy Centrum IoT (pierwsza część nazwy FQDN) i sufiksu Centrum IoT (pozostała część nazwy FQDN). Uzyskaj identyfikator urządzenia i kod dostępu udostępnione po zarejestrowaniu urządzenia z Centrum IoT (zgodnie z opisem w [poprzedniej artykuł](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** umożliwia jeden sposób, aby zainicjować biblioteki. Jeśli wolisz, możesz utworzyć nowy **IOTHUB\_klienta\_obsługiwać** przy użyciu tych parametrów poszczególnych zamiast parametry połączenia. Można to osiągnąć przy użyciu następującego kodu:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Obejmuje to samo jako **IoTHubClient\_CreateFromConnectionString**.

Może wydawać się oczywiste, czy chcesz używać **IoTHubClient\_CreateFromConnectionString** zamiast tej metody generowanie bardziej szczegółowych inicjowania. Należy pamiętać, że po zarejestrowaniu urządzenia w Centrum IoT jest wyświetlany jest identyfikator urządzenia i klucza urządzenia (nie parametry połączenia). Narzędzie **Menedżer urządzeń** SDK wprowadzone w [artykule poprzedniego](iot-hub-device-sdk-c-intro.md) użyto bibliotek **usługi Azure IoT SDK** , aby utworzyć parametry połączenia z identyfikator urządzenia, klucza urządzenia i Centrum IoT nazwa hosta. Sposób nawiązywania połączeń z **IoTHubClient\_a\_tworzenie** mogą być lepiej, ponieważ zapisuje krok generowania parametry połączenia. Użyj metody jest wygodne.

## <a name="configuration-options"></a>Opcje konfiguracji

Pory wszystkich opisanych o sposobie działania biblioteki **IoTHubClient** odzwierciedla zachowanie domyślne. Istnieje jednak kilka opcji, które można ustawić, aby zmienić działanie biblioteki. W tym celu używanie **IoTHubClient\_a\_SetOption** interfejsu API. Należy rozważyć, czy w tym przykładzie:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Istnieje kilka opcji, które są często używane:

-   **SetBatching** (wartość logiczna) — Jeśli **PRAWDA**, a następnie dane wysyłane do Centrum IoT są wysyłane partiami. Jeśli **wartość false**, a następnie wiadomości są wysyłane pojedynczo. Wartość domyślna to **false**. Zauważ, że opcja **SetBatching** dotyczy tylko protokołu HTTP, a nie protokołów MQTT lub AMQP.

-   **Limit czasu** (niepodpisany int) — ta wartość jest reprezentowane (w milisekundach). Jeśli wysłanie żądania HTTP lub otrzymujesz odpowiedź trwa dłużej niż teraz, a następnie limit czasu połączenia.

Opcja łączenia we wsady jest ważna. Domyślnie zdarzeń ingresses biblioteki indywidualnie (pojedyncze zdarzenie jest niezależnie od przekazać do **IoTHubClient\_a\_SendEventAsync**). Jeśli opcja łączenia we wsady jest **prawdziwe**, bibliotece zbiera tyle zdarzenia, jak go z bufora (maksymalnie maksymalny rozmiar wiadomości którego Centrum IoT będzie pasować).  Partii zdarzeń zostanie wysłany do koncentratora IoT jednego połączenia HTTP (pojedynczych zdarzeń są połączone w tablicy JSON). Włączanie tworzeniu partii zwykle powoduje zwiększenie wydajności ogólnej, ponieważ w przypadku zmniejszenia round-trips sieci. Zmniejsza znacząco przepustowości, ponieważ wysyłasz jeden z zestawów nagłówków HTTP z partii zdarzenia, a nie zestaw nagłówków dla każdego poszczególne zdarzenia. Jeśli nie masz jakieś specjalne powody, w przeciwnym razie, zwykle należy włączyć tworzeniu partii.

## <a name="next-steps"></a>Następne kroki

W tym artykule opisano szczegółowo zachowanie biblioteki **IoTHubClient** w sekcji **urządzenie Azure IoT SDK c**. Przy użyciu tych informacji powinien mieć dobrze zapoznać się z możliwościami biblioteki **IoTHubClient** . [Następny artykuł](iot-hub-device-sdk-c-serializer.md) zawiera podobne szczegółów biblioteki **serializatora** .

Aby dowiedzieć się więcej o tworzeniu IoT koncentratora, zobacz [SDK Centrum IoT][lnk-sdks].

Aby dodatkowo Poznawanie możliwości Centrum IoT, zobacz:

- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
