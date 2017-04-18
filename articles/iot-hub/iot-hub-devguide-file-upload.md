<properties
 pageTitle="Przewodnik dewelopera — przekazywanie pliku | Microsoft Azure"
 description="Azure IoT Centrum deweloperów Przewodnik po - przekazywania plików z urządzenia do koncentratora IoT"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="upload-files-from-a-device"></a>Przekazywanie plików z urządzenia

## <a name="overview"></a>Omówienie

Jak wyjaśniono w [Centrum IoT punkty końcowe] [ lnk-endpoints] artykułu, urządzenia można rozpocząć przekazywania plików przez wysłanie powiadomienie za pośrednictwem punktu końcowego skierowaną urządzenia (**/devices/ {identyfikator urządzenia} / pliki**).  Urządzenie powiadamia IoT Centrum przekazywania złożonym, Centrum IoT generuje powiadomienia o przekazywania plików, które może odbierać za pośrednictwem punktu końcowego usługi skierowaną (**/messages/servicebound/filenotifications**) jako wiadomości.

Zamiast pośrednictwa wiadomości za pośrednictwem Centrum IoT, samego Centrum IoT zamiast tego działa jako wysyłającego skojarzone konto Azure miejsca do magazynowania. Urządzenie żądania tokenu miejsca do magazynowania z Centrum IoT, specyficzne dla pliku, który chce przekazać urządzenia. Urządzenie używa identyfikatora URI skojarzeń zabezpieczeń przekazać plik do magazynu, a po zakończeniu pobierania urządzenie powiadomi ukończenia IoT Centrum. Centrum IoT sprawdza, czy plik przekazano i następnie dodaje powiadomienie przekazywania pliku do nowej usługi skierowaną pliku powiadomienie wiadomości punktu końcowego.

Aby możesz przekazać plik do koncentratora IoT z urządzenia, należy skonfigurować Twoim Centrum kojarząc [Magazyn Azure] [ lnk-associate-storage] konta do niego.

Urządzenia mogą następnie [zainicjować przekazywanie] [ lnk-initialize] , a następnie [Powiadom Centrum IoT] [ lnk-notify] po zakończeniu przekazywania. Opcjonalnie urządzeniu powiadamia Centrum IoT zakończeniu pobierania, usługa może wygenerować [wiadomości z powiadomieniem o][lnk-service-notification].

### <a name="when-to-use"></a>Kiedy należy używać

Ta funkcja IoT Centrum gdy trzeba przekazać plik z urządzenia w usłudze wewnętrznej zamiast wysyłania wiadomości za pośrednictwem Centrum IoT.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Kojarzenie konta magazynu platformy Azure z Centrum IoT

Aby użyć funkcji przekazywania plików, musisz najpierw połączyć konta magazynu platformy Azure do koncentratora IoT. Można to zrobić przez [Azure portal][lnk-management-portal], lub programowo za pośrednictwem [dostawcy zasobów Centrum IoT interfejsy API pozostałych][lnk-resource-provider-apis]. Po konto Azure miejsca do magazynowania jest skojarzony z Twoim Centrum IoT, usługę zwraca URI skojarzeń zabezpieczeń do urządzenia, gdy urządzenie inicjuje żądanie Przekaż plik.

> [AZURE.NOTE] [Azure IoT Centrum SDK] [ lnk-sdks] automatycznie obsługiwać pobieranie identyfikatora URI skojarzeń zabezpieczeń, przekazywanie pliku i powiadamianie o IoT Centrum przekazywania złożonym.

## <a name="initialize-a-file-upload"></a>Inicjowanie przekazywania pliku

Centrum IoT ma punktu końcowego specjalnie dla urządzenia, aby zażądać identyfikatora URI skojarzenia zabezpieczeń do przechowywania, aby przekazać plik. Urządzenie inicjuje procesu przekazywania plików przez wysłanie wpisu do koncentratora IoT u `{iot hub}.azure-devices.net/devices/{deviceId}/files` z następujących treści JSON:

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

Centrum IoT zwraca następujące dane, które urządzenie korzysta przekazać plik:

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Przestarzałe: zainicjowanie przekazywania pliku z GET

> [AZURE.NOTE] W tej sekcji opisano funkcje przestarzałych sposobu uzyskania identyfikatora URI skojarzenia zabezpieczeń z Centrum IoT. Użyj metody POST opisany powyżej.

Centrum IoT występują dwa pozostałe punkty końcowe do obsługi przekazywania pliku, jeden-do-Uzyskiwanie identyfikatora URI skojarzeń zabezpieczeń dla miejsca do magazynowania, a drugi do powiadamiania Centrum IoT przekazywanie zostało ukończone. Urządzenie inicjuje procesu przekazywania plików przez wysłanie GET do koncentratora IoT u `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. Centrum zwraca identyfikator URI skojarzeń zabezpieczeń określonego pliku do przekazania i identyfikator korelacji ma być używany po zakończeniu przekazywania.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Powiadom IoT Centrum przekazywania pliku wykonanych

Urządzenie jest odpowiedzialny za przekazywanie pliku do miejsca do magazynowania przy użyciu Azure SDK miejsca do magazynowania. Po zakończeniu przekazywania urządzenie wysyła WPIS do koncentratora IoT u `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` z następujących treści JSON:

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Wartość `isSuccess` jest logiczna przedstawiający czy plik został prawidłowo załadowany. Kod stanu `statusCode` jest stan przekazywania pliku do miejsca do magazynowania oraz `statusDescription` odpowiada `statusCode`.

## <a name="reference-topics"></a>Odwołanie tematy:

W poniższych tematach odwołanie dostarczać więcej informacji na temat przekazywania plików z urządzenia.

## <a name="file-upload-notifications"></a>Powiadomienia o przekazywania plików

Gdy urządzeniu przekazywanie pliku i powiadamia IoT Centrum przekazywania ukończenia, usługę opcjonalnie generuje powiadomienie, które zawiera nazwy i miejsca do magazynowania lokalizacji pliku.

Zgodnie z opisem w [punkty końcowe][lnk-endpoints], Centrum IoT zapewnia powiadomienia o przekazanie pliku za pośrednictwem punktu końcowego usługi skierowaną (**/messages/servicebound/fileuploadnotifications**) jako wiadomości. Znaczenie Odbierz dla powiadomienia o przekazywania plików są takie same jak w przypadku wiadomości w chmurze do urządzenia i mieć samego [cyklu wiadomości][lnk-lifecycle]. Każda wiadomość pobrane z punkt końcowy powiadomienie przekazywania pliku jest rekordu JSON za pomocą następujących właściwości:

| Właściwość | Opis |
| -------- | ----------- |
| EnqueuedTimeUtc | Sygnatura czasowa wskazująca utworzenia powiadomienie. |
| Identyfikator urządzenia | **Identyfikator urządzenia** z urządzeniem, które przekazać plik. |
| BlobUri | Identyfikator URI przekazywanego pliku. |
| BlobName | Nazwa przekazywanego pliku. |
| LastUpdatedTime | Sygnatura czasowa wskazująca, kiedy plik ostatniej aktualizacji. |
| BlobSizeInBytes | Rozmiar przekazywanego pliku. |

**Przykład**. To jest przykład treści wiadomości z powiadomieniem Przekaż plik.

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Opcje konfiguracji powiadomienia przekazywania plików

Każdy koncentratora IoT udostępnia następujące opcje konfiguracji powiadomienia o przekazywania plików:

| Właściwość | Opis | Zakres i domyślne |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | Określa, czy powiadomienia o przekazywania plików są zapisywane w punkt końcowy powiadomienia pliku. | Wartość logiczna. Domyślne: PRAWDA. |
| **fileNotifications.ttlAsIso8601** | Domyślny czas TTL dla powiadomienia o przekazywania plików. | ISO_8601 interwał maksymalnie 48 H (minimalne 1 minuta). Wartość domyślna: 1 godzina. |
| **fileNotifications.lockDuration** | Czas trwania blokady kolejki powiadomienia przekazywania plików. | 5 do 300 sekund (minimalne 5 sekund). Wartość domyślna: 60 sekund. |
| **fileNotifications.maxDeliveryCount** | Liczba dostarczaniem maksymalny pliku Przekaż kolejce powiadomień. | 1 do 100. Domyślne: 100. |

## <a name="additional-reference-material"></a>Materiały dodatkowe informacje

Inne tematy odwołanie w podręczniku Deweloper obejmują:

- [Punkty końcowe Centrum IoT] [ lnk-endpoints] w tym artykule opisano różne punkty końcowe, które każdego centrum IoT udostępnia obsługę operacji obsługi i zarządzania.
- [Ograniczanie i przydziałami] [ lnk-quotas] w tym artykule opisano przydziałów, które mają zastosowanie do usługi Centrum IoT i ograniczania zachowanie się spodziewać podczas korzystania z usługi.
- [Centrum IoT urządzeń i usług SDK] [ lnk-sdks] listy język różne SDK możesz Użyj podczas opracowywania zarówno urządzenia i usługi aplikacji współpracujących z Centrum IoT.
- [Kwerendy języka dla zadań, metody i twins] [ lnk-query] w tym artykule opisano język kwerend, którego można używać do pobierania informacji z Centrum IoT o twins urządzenia, metody i zadania.
- [Obsługa IoT koncentratora MQTT] [ lnk-devguide-mqtt] znajdziesz więcej informacji na temat IoT Centrum obsługi protokołu MQTT.

## <a name="next-steps"></a>Następne kroki

Teraz znasz jak przekazywać pliki na urządzeniach za pomocą Centrum IoT, może Cię w następujących tematach przewodnik dewelopera:

- [Zarządzanie tożsamościami urządzenia w Centrum IoT][lnk-devguide-identities]
- [Kontrolowanie dostępu do Centrum IoT][lnk-devguide-security]
- [Synchronizowanie stanu i konfiguracji za pomocą urządzenia twins][lnk-devguide-device-twins]
- [Wywoływanie metody bezpośrednio na urządzeniu][lnk-devguide-directmethods]
- [Planowanie zadań na wielu urządzeniach][lnk-devguide-jobs]

Jeśli chcesz wypróbować pewne pojęcia opisane w tym artykule, może być w następujących samouczek koncentratora IoT:

- [Jak przekazywać pliki z urządzeń z chmurą przy użyciu Centrum IoT][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
