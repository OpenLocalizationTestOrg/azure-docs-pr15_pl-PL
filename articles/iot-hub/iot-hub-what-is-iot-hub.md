<properties
 pageTitle="Omówienie Centrum IoT Azure | Microsoft Azure"
 description="Omówienie usługi Azure IoT Centrum: co to jest Centrum iot, urządzenia łączności, internet wzorców komunikacji elementów oraz wzorzec pomocy usługi komunikacji"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>

# <a name="what-is-azure-iot-hub"></a>Co to jest Centrum IoT Azure?

Witamy w usłudze Azure Centrum IoT. Ten artykuł zawiera omówienie Centrum IoT Azure i w tym artykule opisano, dlaczego należy przy użyciu tej usługi rozwiązanie programu Internet czynności (IoT). Azure Centrum IoT jest w pełni zarządzane usługa, która pozwala dwukierunkowy i niezawodności komunikacji między miliony IoT urządzeń i wewnętrznej rozwiązanie. Azure Centrum IoT:

- Udostępnia zaufanego urządzenia w chmurze i wiadomości w skali chmury do urządzenia.
- Umożliwia bezpieczna komunikacja przy użyciu poświadczeń zabezpieczeń na urządzenie i kontroli dostępu.
- Udostępnia obszernego monitorowania łączności urządzenia i zdarzeń zarządzania tożsamości urządzenia.
- Zawiera biblioteki urządzenia do najpopularniejszych języków i platformach.

Artykuł [Porównanie IoT Centrum i koncentratory zdarzenia] [ lnk-compare] w tym artykule opisano główne różnice między te dwie usługi i wyróżnienie zalety korzystania z Centrum IoT w rozwiązanie IoT.

![Azure Centrum IoT jako bramy chmury w programie internet rozwiązanie rzeczy][img-architecture]

> [AZURE.NOTE] Aby uzyskać szczegółowe omówienie architektury IoT, zobacz [Microsoft Azure IoT odwołanie architektura][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>Wyzwania urządzenia łączności IoT

Centrum IoT i w bibliotekach urządzenia ułatwiają spełniają problemach dotyczących niezawodne i bezpieczne łączenie urządzenia w celu rozwiązania Wstecz. IoT urządzenia:

- Są często osadzonych systemów nie ludzi operatorem.
- Może być w lokalizacjach zdalnych, gdzie jest drogich fizyczny dostęp.
- Może być tylko dostępne za pośrednictwem wewnętrznej rozwiązanie.
- Może być ograniczone dodatku i przetwarzanie zasobów.
- Może być przerw, działa wolno lub drogich łączności.
- Może być konieczne za pomocą protokołów własnych, niestandardowych lub branżowe aplikacji.
- Można tworzyć, używając duży zbiór popularnych platformy sprzętu i oprogramowania.

Oprócz powyższych wymagań każdego rozwiązania IoT muszą również udostępniać skali, bezpieczeństwa i niezawodności. Wynikowy zestaw wymagania łączności jest słabo i czasochłonne podczas używać tradycyjnych technologii, takich jak kontenery sieci web i brokerów wiadomości.

## <a name="why-use-azure-iot-hub"></a>Dlaczego warto używać Centrum IoT Azure

Azure Centrum IoT adresy problemach łączności urządzenia w następujący sposób:

-   **Uwierzytelnianie na urządzenie i bezpieczne połączenia**. Umożliwia obsługę wszystkie urządzenia za pomocą własnego [klucz zabezpieczeń] [ lnk-devguide-security] aby umożliwić nawiązywanie połączenia z Centrum IoT. [Centrum IoT tożsamości rejestru] [ lnk-devguide-identityregistry] przechowuje tożsamości urządzenia i klawiszy w rozwiązanie. Rozwiązanie wewnętrznej można dodawać pojedyncze urządzenia, aby zezwolić lub odmówić list, aby włączyć pełną kontrolę nad dostępem urządzenia.

-   **Monitorowanie operacje łączności urządzenia**. Możesz odbierać dzienniki operacji szczegółowych informacji o operacji zarządzania tożsamości urządzenia i urządzenia łączności zdarzeń. Ta funkcja monitorowania umożliwia rozwiązania IoT do identyfikowania problemów z łącznością, takich jak urządzenia, które próby nawiązania połączenia przy użyciu poświadczeń problem zbyt częste wysyłanie wiadomości lub odrzucić wszystkie wiadomości w chmurze do urządzenia.

-   **Rozbudowany zestaw bibliotek urządzenia**. [Azure urządzenia IoT SDK] [ lnk-device-sdks] są dostępne i obsługiwane dla różnych języków i platformach — C dla wielu dystrybucji Linux, Windows i systemy operacyjne w czasie rzeczywistym. Azure SDK urządzenia IoT obsługują także zarządzanych językach, takich jak C#, Java i JavaScript.

-   **Protokoły IoT i rozszerzenia**. Jeśli rozwiązanie nie można używać bibliotek urządzenia, Centrum IoT udostępnia publicznej protokół, który umożliwia urządzeń graficznych za pomocą MQTT v3.1.1, HTTP 1.1 lub protokoły AMQP 1.0. Można również rozszerzyć Centrum IoT do obsługi protokołu niestandardowych przy:

    - Tworzenie bramy pola z [Zestawu SDK bramy IoT Azure] [ lnk-gateway-sdk] konwertuje z protokołu niestandardowego do jednej z trzech protokoły zrozumiałe dla Centrum IoT. 
    - Dostosowywanie [bramy protokołu Azure IoT][protocol-gateway], Otwórz źródło składnik, który jest uruchamiany w chmurze.

-   **Skala**. Azure Centrum IoT skale miliony urządzeń jednocześnie i miliony wydarzeń na sekundę.

Następujące korzyści są ogólne do wielu wzorców komunikacji. Centrum IoT umożliwia obecnie wykonania następujących wzorów przekazywane:

-   **Oparte na zdarzeniach spożyciu urządzenia w chmurze.** Centrum IoT niezawodne mogą otrzymywać miliony wydarzeń na sekundę na swoich urządzeniach. Następnie przetwarzania ich na ścieżce dostępu przy użyciu aparatu procesor zdarzenia. Go można również przechowywać je na ścieżce zimnej na potrzeby analizy. Centrum IoT zachowuje dane zdarzenia przez siedem dni, aby zagwarantować niezawodne przetwarzanie i pochłania wartości obciążenia.

-   * *Niezawodne wiadomości w chmurze do urządzenia (lub *polecenia*). ** wewnętrznej rozwiązanie można użyć Centrum IoT do wysyłania wiadomości z gwarancji dostarczenia najmniej — jednocześnie do poszczególnych urządzeń. Każda wiadomość ma poszczególnych ustawienie time to live i wewnętrzna można żądać potwierdzeń dostarczenia jak również wygaśnięcia. Te potwierdzeń upewnij się, pełny wgląd cyklu życia wiadomości chmury do urządzenia. Następnie można implementacji reguł biznesowych, która zawiera operacji, które są uruchamiane na urządzeniach.

-   **Przekazywanie plików i czujnik pamięci podręcznej danych w chmurze.** Urządzenia przekazać pliki do magazynu Azure za pomocą URI skojarzeń zabezpieczeń zarządza Centrum IoT dla Ciebie. Centrum IoT mogą powodować zwrócenie powiadomienia po odebraniu plików w chmurze umożliwiające wewnętrznej ich przetwarzania.

## <a name="gateways"></a>Bramy

Bramy w rozwiązaniu IoT jest zazwyczaj [bramy protokołu] [ lnk-gateway] który został wdrożony w chmurze lub [bramy pola] [ lnk-field-gateway] lokalnie używany przez urządzenia. Brama protokołu wykonuje tłumaczenia protokół, na przykład MQTT do AMQP. Brama pola można uruchamiać analizy na krawędzi, podejmowania decyzji zależne od czasu, aby zmniejszyć opóźnienie, dostępne usługi zarządzania urządzenia, ustawione zabezpieczenia i prywatność ograniczenia oraz także wykonywać tłumaczenia Protocol (protokół). Oba typy bramy działają jako pośrednicy między urządzeniami a Twoim Centrum IoT.

Brama pola różni się od urządzenia routingu prosty ruchu (na przykład urządzenia tłumaczenia adresu sieciowego lub zapory), ponieważ zwykle wykonuje aktywną rolę w zarządzaniu przepływu dostępu i informacji w rozwiązaniu.

Rozwiązanie może zawierać zarówno protokół, jak i pole bramy.

## <a name="how-does-iot-hub-work"></a>Jak działa Centrum IoT?

Wykonuje Azure IoT Centrum [pomocy usługi komunikacji] [ lnk-service-assisted-pattern] wzorca do mediacji interakcji między urządzeniami i sieci wewnętrznej rozwiązanie. Usługi pomocy komunikacji ma na celu ustanowienia w wiarygodny, dwukierunkowy ścieżki komunikacji między system kontroli, takich jak Centrum IoT i urządzeń specjalnych rozmieszczone w niezaufane przestrzeni fizycznej. Wzorzec ustala następujących zasad:

- Wszystkie inne funkcje zabezpieczeń ma pierwszeństwo przed.
- Urządzenia nie akceptuj sieci niechciane informacje. Urządzenie ustala wszystkie połączenia i przekierowuje w sposób tylko ruchu wychodzącego. Urządzenia do odbierania polecenia z wewnętrzna urządzenie musi regularnie zainicjować połączenie, aby sprawdzić, czy są jakieś oczekujące polecenia przetwarzania.
- Urządzenia tylko należy nawiązać połączenie lub ustanowienia trasy do dobrze znanych usług, które są one peered, takich jak IoT Centrum.
- Ścieżka komunikacji między urządzeń i usług lub urządzenia i bramy jest zabezpieczone w warstwie protokołu aplikacji.
- Poziom systemu autoryzacji i uwierzytelniania są oparte na tożsamości na urządzenie. Uczestnicy wprowadzają poświadczenia dostępu i uprawnienia odwoływalny niemal natychmiast.
- Komunikację dwukierunkową dla urządzeń łączących sporadycznie z powodu power lub łączności dotyczy to ułatwiać przytrzymując poleceń i powiadomień urządzenia, aż urządzenie łączy się odbierania ich. Centrum IoT zachowuje kolejek specyficzne dla urządzenia do polecenia, które wysyła.
- Dane ładunku aplikacji jest zabezpieczone oddzielnie dla przewozowe chronionych za pomocą bramy do określonej usługi.

Branżowe urządzeń przenośnych został użyty deseń komunikacji pomocy usługi w większej skali ponad normę do wdrożenia wypychanych powiadomień usług, takich jak [Usługi powiadomień wypychanych systemu Windows][lnk-wns], [Google Cloud wiadomości][lnk-google-messaging], a [Usługa powiadomień Push firmy Apple][lnk-apple-push].

Centrum IoT jest obsługiwana przez osoby ExpressRoute publicznej zaglądanie ścieżki.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak Centrum IoT Azure umożliwia zgodne ze standardami zarządzania urządzeniami IoT umożliwiają zdalnego zarządzania, konfigurowanie i aktualizowanie urządzenia, zobacz [Omówienie Azure IoT Centrum zarządzania urządzeniami][lnk-device-management].

Aby wdrożyć aplikacje klienckie na wielu różnych platform sprzętu urządzenia i systemy operacyjne, możesz użyć urządzenia IoT SDK. Urządzenie IoT SDK zawierać bibliotek, ułatwiające wysyłanie telemetrycznego do koncentratora IoT i odbierania poleceń w chmurze do urządzenia. Gdy używasz SDK, możesz wybrać z różnych protokołów sieciowych można komunikować się z Centrum IoT. Aby dowiedzieć się więcej, zobacz [informacje dotyczące urządzenia SDK][lnk-device-sdks].

Aby rozpocząć pisanie kodu i uruchamianie niektóre przykłady, zobacz [Wprowadzenie do Centrum IoT] [ lnk-get-started] samouczka.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Usługi pomocy komunikacji, wpis w blogu, Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md
