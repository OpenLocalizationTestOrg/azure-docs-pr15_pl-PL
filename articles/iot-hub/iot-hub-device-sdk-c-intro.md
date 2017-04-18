<properties
    pageTitle="Za pomocą urządzenia Azure IoT SDK c | Microsoft Azure"
    description="Więcej informacji na temat i rozpoczynanie pracy z przykładowy kod urządzenia Azure IoT SDK dla C."
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

# <a name="introducing-the-azure-iot-device-sdk-for-c"></a>Wprowadzenie urządzenia Azure IoT SDK dla C

**Urządzenie Azure IoT SDK** to zbiór bibliotek pozwala uprościć proces wydarzeń wysyłanie i odbieranie wiadomości z usługi **Azure IoT Centrum** . Dostępne są różne odmiany SDK, kierowanie określonej platformy, ale w tym artykule opisano **urządzenia Azure IoT SDK c**.

Urządzenia Azure IoT SDK C jest napisana ANSI C (C99) maksymalizować przenoszenia. Dzięki temu nadają się do pracy na numer platform i urządzeń, zwłaszcza w przypadku, gdy zminimalizowaniu dysku i pamięci jest priorytet.  

Istnieje szeroki zakres platform, na których przetestowano zestawu SDK (zobacz [Azure certyfikowane dla wykazu urządzenia IoT](https://catalog.azureiotsuite.com/) Aby uzyskać szczegółowe informacje). Mimo że ten artykuł zawiera instruktaży przykładowy kod uruchomione na platformie Windows, należy pamiętać o tym, że kod opisane w tym artykule jest dokładnie taki sam w zakresie platformy obsługiwane.

W tym artykule można będzie można się z wprowadzeniem do architektury urządzenia Azure IoT SDK dla C. Zostanie przedstawiona jak zainicjować biblioteki urządzenia, wysyłania zdarzeń do Centrum IoT, a także otrzymywać wiadomości od niego. Informacje w tym artykule powinny być wystarczające dla rozpocząć korzystanie z zestawu SDK, ale również znajdują się łącza do dodatkowych informacji na temat bibliotek.

## <a name="sdk-architecture"></a>Architektura SDK

Możesz odnaleźć **urządzenia Azure IoT SDK c** repozytorium GitHub [Microsoft Azure IoT SDK](https://github.com/Azure/azure-iot-sdks) i wyświetlanie szczegółów interfejsu API [C API odwołania](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

Najnowszą wersję bibliotek można znaleźć w **głównym** gałęzi tego repozytorium:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

To repozytorium zawiera całej rodziny urządzenia Azure IoT SDK. Ten artykuł jest jednak o urządzeniu Azure IoT SDK *c* znajdującym się w folderze **c** .

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

* Wykonanie podstawowego zestawu SDK można znaleźć w **iothub\_klienta** folder zawierający stosowania najniższej warstwie interfejsu API w zestawie SDK: biblioteki **IoTHubClient** . Biblioteka **IoTHubClient** zawiera interfejsy API implementacji nieprzetworzonych wiadomości do wysyłania wiadomości do koncentratora IoT, a także otrzymywać wiadomości od go. Korzystając z tej biblioteki, odpowiadasz stosowania szeregowania wiadomości (po pewnym czasie przy użyciu próbki serializatora opisane poniżej), ale inne szczegóły komunikowania się z Centrum IoT są obsługiwane dla Ciebie.
* Folder **serializatora** zawiera funkcje Pomocnik oraz przykłady przedstawiający, jak szeregować danych przed wysłaniem do koncentratora IoT Azure za pomocą biblioteki klienta. Należy zauważyć, że wykorzystanie serializatora nie jest obowiązkowe i tylko dostarczana dla wygody. Jeśli używasz biblioteki **serializatora** , możesz uruchomić definiując modelu, który określa zdarzenia, które ma zostać wysłany do Centrum IoT, a także wiadomości, których oczekuje się z niego. Po określeniu modelu jest, zestaw SDK zawiera powierzchni interfejs API umożliwiający Ułatwiona praca z wydarzeń i wiadomości bez konieczności martwić szeregowania szczegóły.
Biblioteki w zależności od innych bibliotek Otwórz źródło, implementujących transportu za pomocą kilku protokołów (MQTT, AMQP).
* Biblioteka **IoTHubClient** zależy od innych bibliotek Otwórz źródło:
   * [Azure C udostępnione narzędzie](https://github.com/Azure/azure-c-shared-utility) biblioteki, która zapewnia często używane funkcje podstawowe zadania (takich jak ciągu znaków, listy manipulowanie, Jo, itp....) wymagane przez kilka powiązanych Azure SDK C
   * Biblioteka [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) , który jest implementacją po stronie klienta AMQP zoptymalizowane dla urządzeń ograniczeń zasobów.
   * Biblioteka [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) ogólnego przeznaczenia biblioteki wprowadzenia w życie protokołu MQTT i zoptymalizowane dla urządzeń ograniczeń zasobów.

Wszystkie te jest łatwiejszy do zrozumienia, na przykład kod. Poniższe sekcje przeprowadził Cię przez kilka przykładowych aplikacji, które znajdują się w zestawie SDK. To powinien podać Ci dobrym sposobem działania różnych możliwości warstw architektury zestawu SDK, jak również wprowadzenie do działania za pośrednictwem interfejsów API.

## <a name="before-running-the-samples"></a>Przed uruchomieniem próbki

Przed uruchomieniem próbki na urządzeniu Azure IoT SDK c musi utworzyć wystąpienie usługi w subskrypcji usługi Azure, jeśli nie masz już jeden oraz wykonywanie zadań 2:
* Przygotowywanie środowiska programowania
* Uzyskaj poświadczenia urządzenia.

Jeśli chcesz utworzyć wystąpienia Centrum IoT Azure w subskrypcji usługi Azure, postępuj zgodnie z instrukcjami [poniżej](https://github.com/Azure/azure-iot-sdks/blob/master/doc/setup_iothub.md).

[Plik readme](https://github.com/Azure/azure-iot-sdks/tree/master/c) dołączonego zestawu SDK zawiera instrukcje dotyczące przygotowywania środowiska programowania i uzyskać poświadczenia urządzenia.
W następujących sekcjach opisano niektóre dodatkowe komentarz w tych instrukcjach.

### <a name="preparing-your-development-environment"></a>Przygotowywanie środowiska programowania

Pakiety znajdują się w przypadku niektórych platform (na przykład NuGet dla systemu Windows lub apt_get Debian i Ubuntu) i próbki za pomocą tych pakietów, gdy są dostępne, instrukcji szczegółowo sposób tworzenia biblioteki i próbki bezpośrednio formularz kod.

Najpierw musisz uzyskać kopię zestawu SDK z GitHub, a następnie skonstruuj źródła. Należy uzyskać kopię źródła od **wzorca** gałęzi [repozytorium GitHub](https://github.com/Azure/azure-iot-sdks).

Po pobraniu kopię źródła, musisz wykonać kroki opisane w artykule SDK ["Przygotowania środowiska programowania"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md).


Oto kilka porad ułatwiających wykonać procedurę opisaną w podręczniku przygotowanie:

-   Po zainstalowaniu narzędzia **CMake** , wybierz opcję, aby dodać **CMake** systemowi ścieżki dla **wszystkich użytkowników** (Dodawanie także Works **bieżącego użytkownika** ):

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)


-   Zanim otworzysz **Deweloper wiersza polecenia VS2015**Zainstaluj narzędzia wiersza polecenia cyfra. Aby zainstalować te narzędzia, wykonaj następujące czynności:

    1. Uruchom program instalacyjny **programu Visual Studio 2015** (lub wybierz pozycję **Microsoft Visual Studio 2015** przed **Programy i funkcje** w Panelu sterowania i wybierz pozycję **Zmień**).
    
    2. Upewnij się, funkcja **Cyfra dla systemu Windows** jest zaznaczona Instalatora pakietu, ale może też chcesz sprawdzić opcję **GitHub rozszerzenie programu Visual Studio** , aby zapewnić integracji IDE:

        ![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

    3. Kończenie pracy Kreatora konfiguracji do zainstalowania narzędzia.

    4. Dodawanie katalogu **bin** cyfra narzędzia do środowiska **ŚCIEŻKA** zmienna. W systemie Windows to wygląda następująco:

        ![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)


Po wykonaniu wszystkich kroków opisanych w stronie ["Przygotowywanie środowiska programowania"](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md) , możesz już przystąpić do opracowania przykładowe aplikacje.

### <a name="obtaining-device-credentials"></a>Uzyskiwanie poświadczeń urządzenia

Teraz, gdy skonfigurowano środowiska programowania następne rozwiązanie do jest zestaw poświadczeń urządzenia.  Urządzenia można było uzyskać dostęp do Centrum IoT musisz najpierw dodać urządzenia w Centrum IoT rejestru urządzenia. Po dodaniu urządzenia zostanie wyświetlony zestaw poświadczeń urządzenia, co będzie potrzebne, aby można było połączyć do koncentratora IoT na tym urządzeniu. Przykładowe aplikacje, które przyjrzymy w następnej sekcji z oczekiwaniami tych poświadczeń w formularzu **Parametry połączenia urządzenia**.

Istnieje kilka narzędzi dostępnych w repozytorium Otwórz źródło SDK ułatwiające zarządzanie Centrum IoT. Jedna jest aplikacji o nazwie urządzenia Eksploratorze drugi jest podstawą node.js krzyżowego platformy polecenie narzędzie o nazwie iothub Eksploratora Windows. Dowiedz się więcej o tych narzędziach [tutaj](https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md).

Jak możemy pośrednictwa uruchomiony próbki w systemie Windows, w tym artykule, użyto narzędzie Explorer urządzenia. Ale może również używać Eksploratora iothub, jeśli wolisz interfejsu wiersza polecenia narzędzia.

Narzędzie [Eksploratora urządzenie](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) używa bibliotek usługi Azure IoT wykonywać różne funkcje na Centrum IoT, łącznie z dodawaniem urządzenia. Dodawanie urządzenia za pomocą Eksploratora urządzenia, otrzymasz odpowiednie parametry połączenia. Potrzebujesz ten ciąg połączenia, aby wprowadzić próbki, które aplikacje są uruchamiane.

W przypadku, gdy nie masz już znanych z procesem, Poniższa procedura opisano, jak za pomocą Eksploratora urządzenia do dodania urządzenia i uzyskiwania parametrów połączenia urządzenia.

Instalator Windows można znaleźć narzędzia Eksploratora urządzenie na [SDK Zwolnij strony](https://github.com/Azure/azure-iot-sdks/releases). Ale można również uruchomić narzędzie bezpośrednio z jego kod otwarcie **[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** w **Visual Studio 2015** i tworzenie rozwiązania.

Po uruchomieniu programu zostanie wyświetlony ten interfejs:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Wprowadź **Parametry połączenia Centrum IoT** w pierwszym polu, a następnie kliknij przycisk **Aktualizuj**. Konfiguruje narzędzie, tak aby może komunikować się z Centrum IoT.

Po skonfigurowaniu Centrum IoT parametry połączenia kliknij kartę **Zarządzanie** :

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Jest to miejsce, w którym będzie zarządzania urządzeniami w Twoim Centrum IoT.

Można utworzyć urządzenie, klikając przycisk **Utwórz** . Okno dialogowe zostanie wyświetlona z zestawu wstępnie wypełnione kluczy (głównego i pomocniczego). Wszystko, co trzeba zrobić to wprowadź **Identyfikator urządzenia** , a następnie kliknij przycisk **Utwórz**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Po utworzeniu urządzenie na liście urządzeń jest aktualizowana wszystkich urządzeń zarejestrowanych, łącznie z tą, która została właśnie utworzona. Kliknięcie prawym przyciskiem myszy nowym urządzeniu, zostanie wyświetlony w tym menu:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Jeśli wybierzesz opcję **Skopiuj parametry połączenia dla wybranego urządzenia** , parametry połączenia dla swojego urządzenia zostanie skopiowana do Schowka. Zachowaj kopię parametry połączenia. Będzie on potrzebny podczas uruchamiania aplikacji przykładowych opisane w następnej sekcji.

Po wykonaniu powyższych czynności, możesz rozpocząć uruchamianie kodu. Obie próbki muszą stałą u góry pliku główne źródło, który pozwala na wprowadź parametry połączenia. Na przykład odpowiednim wierszu z **iothub\_klienta\_próbki\_amqp** aplikacji jest wyświetlana w następujący sposób.

```
static const char* connectionString = "[device connection string]";
```

Jeśli chcesz wykonać opisane czynności, wprowadź parametry połączenia urządzenie, ponownie skompilować rozwiązanie, a będzie można uruchomić przykład.

## <a name="iothubclient"></a>IoTHubClient

W ramach **iothub\_klienta** folder w repozytorium azure-iot SDK jest folder **próbki** , który zawiera aplikację o nazwie **iothub\_klienta\_próbki\_amqp**.

Wersja systemu Windows **iothub\_klienta\_próbki\_ampq** aplikacja zawiera następujące rozwiązania Visual Studio:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

To rozwiązanie zawiera pojedynczego projektu. Warto zauważyć, że istnieją cztery pakietów NuGet zainstalowanych w tym rozwiązaniu:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.uamqp

Zawsze należy pakietu **Microsoft.Azure.C.SharedUtility** podczas pracy z zestawu SDK. Ponieważ w tym przykładzie zależy od AMQP, można także dodać pakiety **Microsoft.Azure.uamqp** i **Microsoft.Azure.IoTHub.AmqpTransport** (dostępne są równoważne pakietów MQTT i HTTP). Ponieważ próbce używa biblioteki **IoTHubClient** , można także dodać pakietu **Microsoft.Azure.IoTHub.IoTHubClient** rozwiązania.

Można znaleźć implementacji przykładowej aplikacji w **iothub\_klienta\_próbki\_amqp.c** plik źródłowy.

Użyjemy tej aplikacji przykładowej kroków co to są wymagane, aby użyć biblioteki **IoTHubClient** .

### <a name="initializing-the-library"></a>Inicjowanie biblioteki

> [AZURE.NOTE] Przed rozpoczęciem pracy z bibliotekami, może być konieczne wykonywanie niektórych inicjowanie określonej platformy. Na przykład jeśli planujesz używać AMQP na Linux należy zainicjować biblioteki OpenSSL. Przykłady w [repozytorium GitHub](https://github.com/Azure/azure-iot-sdks) wywołania funkcji narzędzia **platform_init** , gdy klient zostanie uruchomiony i Wywołaj **platform_deinit** przed zakończeniem. Te funkcje są deklarowane w pliku nagłówek "platform.h". Należy przejrzeć definicje te funkcje dla swojej platformy docelowej w [repozytorium](https://github.com/Azure/azure-iot-sdks) do określenia, czy konieczne jest uwzględnienie kodu inicjowanie platformy w Twoim kliencie.

Aby rozpocząć pracę z bibliotekami najpierw musisz przydzielić uchwytu klienta Centrum IoT:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Należy zauważyć, że firma Microsoft przechodząc kopię naszych parametry połączenia urządzenia do tej funkcji (język, który firma Microsoft uzyskanego z Eksploratora urządzenia). Możemy również wyznaczyć protokołu chcemy za pomocą. W tym przykładzie użyto AMQP, ale MQTT i HTTP są również odpowiednią opcję.

Jeśli używasz prawidłową **IOTHUB\_klienta\_obsługiwać**, możesz rozpocząć wywoływania interfejsów API do zdarzenia wysyłania i odbierania wiadomości z Centrum IoT. Przyjrzymy tego dalej.

### <a name="sending-events"></a>Wysyła zdarzenia

Wysyłania zdarzeń do Centrum IoT wymaga, możesz wykonać następujące czynności:

Najpierw utwórz wiadomość:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

Następnie wyślij wiadomość:

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Zawsze możesz wysłać wiadomość, należy określić odwołanie do funkcji zwrotnego, która jest wywoływana, gdy dane są przesyłane:

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Uwaga połączenie **IoTHubMessage\_Destroy** działać po zakończeniu z tą wiadomością. Musisz wprowadzić to połączenie, aby zwolnić zasoby przydzielone podczas tworzenia wiadomości.

### <a name="receiving-messages"></a>Odbieranie wiadomości

Odbieranie wiadomości jest operacja asynchroniczna. Najpierw należy zarejestrować zwrotnego do wywoływana, gdy urządzenie odbiera wiadomości:

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Ostatni parametr jest void wskaźnik co tylko chcesz. W próbce jest wskaźnik do liczby całkowitej, ale może to być wskaźnik, aby bardziej złożone struktury danych. Dzięki temu funkcja zwrotnego może działać w udostępnionej stan skierowaną do osoby wywołującej tej funkcji.

Kiedy urządzenie odbiera wiadomości, funkcja zarejestrowanych zwrotnego jest wywoływana:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Należy zauważyć, że używasz **IoTHubMessage\_GetByteArray** funkcja pobierania wiadomości, czyli w tym przykładzie ciąg.

### <a name="uninitializing-the-library"></a>Usuwanie zainicjowania biblioteki

Po zakończeniu wydarzeń wysyłanie i odbieranie wiadomości, możesz odinicjowania biblioteki IoT. W tym celu problemu wywołaniu funkcji następujące czynności:

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Spowoduje to zwolnienie zasobów przydzielonych wcześniej przez **IoTHubClient\_CreateFromConnectionString** funkcji.

Jak widać, jest łatwe do zdarzenia wysyłania i odbierania wiadomości z biblioteką **IoTHubClient** . Biblioteka obsługuje szczegóły komunikowania się z Centrum IoT, w tym protokół, który ma być używany (z punktu widzenia projektanta, to jest opcja prostej konfiguracji).

Biblioteka **IoTHubClient** także precyzyjnie kontrolować sposób szeregować zdarzenia, które urządzenie wysyła do Centrum IoT. W niektórych przypadkach jest to korzystne, ale w innych przypadkach jest szczegółów implementacji, z którym chcesz nie dotyczyć. Jeśli jest to wielkość liter, warto rozważyć użycie biblioteki **serializatora** , która będzie opisano w następnej sekcji.

## <a name="serializer"></a>Serializator

Koncepcji biblioteki **serializatora** znajduje się u góry biblioteki **IoTHubClient** w zestawie SDK. Używa biblioteki **IoTHubClient** do podstawowej komunikacji z Centrum IoT, ale dodaje funkcje modelowania, usuwające obciążenia sposób postępowania z szeregowania wiadomości przez dewelopera. Jak działa ta biblioteka najlepsze przedstawiono według przykładu.

W ramach **serializatora** w repozytorium azure-iot SDK jest **próbki** folder zawierający aplikacji o nazwie **simplesample\_amqp**. Wersja systemu Windows w tym przykładzie zawiera następujące rozwiązania Visual Studio:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

Podobnie jak w przypadku poprzedni przykład tego typu zawiera kilka pakietów NuGet:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.IoTHub.Serializer
- Microsoft.Azure.uamqp

Firma Microsoft zobaczył większość z tych w poprzednim przykładzie, ale nowego **Microsoft.Azure.IoTHub.Serializer** . Jest to wymagane, gdy firma Microsoft korzysta z biblioteki **serializatora** .

Można znaleźć stosowania aplikacja przykładowa w **simplesample\_amqp.c** pliku.

Poniższe sekcje przeprowadził Cię do kluczowych fragmentów w tym przykładzie.

### <a name="initializing-the-library"></a>Inicjowanie biblioteki

Aby rozpocząć pracę z biblioteką **serializatora** , należy wywołać inicjowania interfejsów API:

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

Połączenie **serializatora\_inicjowania** funkcja jednorazowego połączeń i służy do inicjowania źródłowych biblioteki. Następnie należy wywołać **IoTHubClient\_CreateFromConnectionString** funkcji, która jest tego samego interfejsu API, tak jak w próbce **IoTHubClient** . To połączenie ustawia ciąg połączenia urządzenia (jest to także miejsce, w którym możesz wybrać protokół ma być używany). Uwaga w tym przykładzie użyto AMQP do transportu, ale można używać protokołu HTTP.

Na koniec połączeń **Tworzenie\_modelu\_wystąpienie** funkcji. Należy zauważyć, że **WeatherStation** jest nazw modelu i **ContosoAnemometer** jest nazwą modelu. Po utworzeniu wystąpienia modelu można go uruchomić zdarzenia wysyłania i odbierania wiadomości. Warto jednak pamiętać, jakie modelu.

### <a name="defining-the-model"></a>Definiowanie modelu

Model w bibliotece **serializatora** Określa zdarzenia, które urządzenie można wysłać do Centrum IoT i wiadomości, nazywane *akcjami* język modelowania, który może odbierać. Definiowanie modelu przy użyciu zestawu makra C jako **simplesample\_amqp** przykładowej aplikacji:

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

**Rozpocząć\_nazw** i **zakończenia\_nazw** makra obu wykonać nazw modelu jako argument. Przewiduje się, że wszystko między te makra jest definicji modele usługi i struktury danych, korzystające z modeli.

W tym przykładzie jest pojedynczy model o nazwie **ContosoAnemometer**. Ten model definiuje dwa zdarzenia, które urządzenia mogą wysyłać do Centrum IoT: **identyfikator urządzenia** i **prędkość wiatru**. Definiuje również trzy akcje (wiadomości), które mogą otrzymywać urządzenia: **TurnFanOn**, **TurnFanOff**i **SetAirResistance**. Każde zdarzenie ma typu i każdej akcji ma nazwę (i opcjonalnie zestaw parametrów).

Zdarzenia i akcje zdefiniowane w modelu Definiowanie powierzchni interfejsu API, która służy do wysyłania zdarzeń do Centrum IoT, a także odpowiadanie na wiadomości wysyłane do urządzenia. Najlepiej jest to rozumieć przez przykładowy.

### <a name="sending-events"></a>Wysyła zdarzenia

Model Określa zdarzenia, które mogą wysyłać do Centrum IoT. W tym przykładzie jednego z dwóch zdarzeń zdefiniowane przy użyciu makra **WITH_DATA** oznacza, że. Na przykład jeśli chcesz wysłać do zdarzenia **prędkość wiatru** do koncentratora IoT, istnieje kilka czynności związane dokonując tak się stało. Pierwszy jest potrzebne do wysłania dane:

```
myWeather->WindSpeed = 15;
```

Model, w którym możemy zdefiniowany wcześniej pozwala na to zrobić przez ustawienie członkiem **struktury**. Następnie możemy szeregować zdarzenia, które będą wysyłać:

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

Kod serializes zdarzeń do bufora (wskazywany przez **miejsce docelowe**). Na koniec w Centrum IoT kod wyślemy zdarzenia:

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

Jest to funkcja Pomocnik w przykładowej aplikacji wysyłające naszych seryjne zdarzeń do koncentratora IoT:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

Ten kod jest bardzo podobne do możemy pokazano w **iothub\_klienta\_próbki\_amqp** aplikacji, w której możemy utworzone wiadomości za pomocą tablicy bajtów i następnie używane **IoTHubClient\_SendEventAsync** je wysłać do Centrum IoT. Po wykonaniu tej możemy po prostu trzeba wolny uchwytu wiadomości i seryjnych bufor danych, które było wcześniej przydzielonych.

Drugiego do ostatniego parametr **IoTHubClient\_SendEventAsync** odwołuje się do funkcji zwrotnego wywoływana, gdy dane są przesyłane pomyślnie. Oto przykład funkcji zwrotnego:

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

Drugi parametr jest wskaźnika w kontekście użytkownika; wskaźnik samej możemy przekazywane do **IoTHubClient\_SendEventAsync**. W tym przypadku kontekst jest prostego licznika, ale można go, trzeba coś.

To wszystko jest wysyłanie zdarzeń. Jedynym elementem w lewo, aby obejmować przedstawiono sposób odbierać wiadomości.

### <a name="receiving-messages"></a>Odbieranie wiadomości

Odbieranie wiadomości działa podobnie do wiadomości sposób pracy w bibliotece **IoTHubClient** . Najpierw zarejestrowaniu funkcji zwrotnego wiadomości:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Następnie możesz zapisywać funkcji zwrotnego, która jest wywoływana po odebraniu wiadomości:

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

Ten kod jest standardowy — jest taki sam dla każdego rozwiązania. Ta funkcja odbiera wiadomości i Trwa obsługę wysyłania go do odpowiedniej funkcji za pośrednictwem połączenia do **wykonywanie\_polecenia**. Funkcja o nazwie w tym momencie zależy od definicji akcje w naszym modelu.

Podczas definiowania akcji w modelu, musisz zaimplementować funkcję, która jest wywoływana, gdy urządzenie otrzyma odpowiedni komunikat. Jeśli na przykład modelu definiuje tej akcji:

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

Należy zauważyć, że nazwa funkcji odpowiada nazwie akcji w modelu i parametry funkcji zgodności parametrów określonych akcji. Pierwszy parametr jest zawsze wymagane i zawiera wskaźnik do wystąpienia naszego modelu.

Kiedy urządzenie odbiera wiadomości, która jest zgodna z tego podpisu, nosi nazwę odpowiednich funkcji. W związku z tym oprócz konieczności dołączania z **IoTHubMessage**kodu standardowy, odbierania wiadomości polega po prostu definiowanie prostych funkcji dla każdej akcji zdefiniowane w modelu.

### <a name="uninitializing-the-library"></a>Usuwanie zainicjowania biblioteki

Po zakończeniu danych wysyłaniem i odbieraniem wiadomości możesz odinicjowania biblioteki IoT:

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Każda z tych trzech funkcji wyrównane trzy funkcje inicjowanie opisany powyżej. Nawiązywania połączeń z tych interfejsów API gwarantuje, że zwolnienia wcześniej przydzielonych zasobów.

## <a name="next-steps"></a>Następne kroki

W tym artykule omówione podstawy korzystania z bibliotek w **urządzeniu Azure IoT SDK c**. Jego opisane wystarczających informacji, aby dowiedzieć się, co zawiera Usługa SDK, jego architektura i jak rozpocząć pracę z próbki systemu Windows. Następny artykuł nadal opis zestawu SDK przez wyjaśnieniem [więcej informacji na temat biblioteki IoTHubClient](iot-hub-device-sdk-c-iothubclient.md).

Aby dowiedzieć się więcej o tworzeniu IoT koncentratora, zobacz [SDK Centrum IoT][lnk-sdks].

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway]


[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
