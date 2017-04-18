<properties
 pageTitle="Azure IoT wstępnie rozwiązań | Microsoft Azure"
 description="Opis Azure IoT wstępnie rozwiązań i ich architektura z łącza do dodatkowych zasobów."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/09/2016"
 ms.author="dobett"/>

# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Co to są rozwiązania pakietu IoT Azure wstępnie?

Rozwiązania pakietu IoT Azure wstępnie są implementacji typowe wzorce rozwiązanie IoT wdrażane Azure, korzystając ze swojej subskrypcji. Można używać wstępnie rozwiązań:

- Jako punkt początkowy rozwiązanie IoT.
- Aby dowiedzieć się o typowe wzorce w IoT rozwiązanie projektowania i rozwoju.

Każdy wstępnie rozwiązaniem jest wykonane, kompleksowe — wdrożenie, używanego do generowania telemetrycznego symulowany urządzenia.

Oprócz wdrażanie i uruchamianie rozwiązania platformy Azure, można pobrać kodu źródłowego pełną, a następnie dostosowywanie i rozszerzania rozwiązanie do własnych wymagań IoT.

> [AZURE.NOTE] Aby wdrożyć jedno z wstępnie rozwiązań, odwiedź witrynę [Microsoft Azure IoT Suite][lnk-azureiotsuite]. Artykuł [Wprowadzenie do rozwiązania IoT wstępnie] [ lnk-getstarted-preconfigured] znajdziesz więcej informacji na temat wdrażanie i uruchamianie jednego z rozwiązań.

W poniższej tabeli pokazano, jak mapowanie rozwiązania do określonych funkcji IoT:

| Rozwiązanie | Spożyciu danych | Urządzenia tożsamości | Polecenie i formant | Reguły i akcje | Przewidywanych analizy |
|------------------------|-----|-----|-----|-----|-----|
| [Monitorowanie zdalne][lnk-getstarted-preconfigured] | Tak | Tak | Tak | Tak | -   |
| [Przewidywanych konserwacji][lnk-predictive-maintenance] | Tak | Tak | Tak | Tak | Tak |

- *Spożyciu danych*: wnikaniem danych w skali w chmurze.
- *Urządzenia tożsamości*: Zarządzanie unikatowych tożsamości każdego urządzenia połączonego.
- *Polecenie i kontrola*: wysyłanie wiadomości na urządzenie z chmury urządzenie wykonania określonych czynności.
- *Reguły i akcje*: wewnętrznej rozwiązanie stosowane reguły odbywało się na określonych danych urządzenia w chmurze.
- *Przewidywanych analizy*: wewnętrznej rozwiązanie dotyczy analizy danych urządzenia w chmurze do przewidywania, gdy określonych akcji powinna być wykonywana. Na przykład analizowanie telemetrycznego aparat samolotu, aby określić, kiedy przypada konserwacji aparat.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Omówienie rozwiązanie zdalnego monitorowania wstępnie

Firma Microsoft wybrano omówić zdalnego monitorowania wstępnie rozwiązanie w tym artykule, ponieważ go ilustruje wiele typowych elementów projektu, które współużytkują innych rozwiązań.

Na poniższej ilustracji przedstawiono główne elementy zdalnego rozwiązanie monitorowania. W poniższych sekcjach więcej informacji na temat tych elementów.

![Monitorowanie zdalnego wstępnie architektury rozwiązania][img-remote-monitoring-arch]

## <a name="devices"></a>Urządzenia

Po wdrożeniu zdalnego monitorowania rozwiązanie wstępnie cztery symulowany urządzenia są wstępnie ustanawianie w rozwiązanie, które zasymulowania chłodzenia urządzenia. Urządzenia te symulowany mają wbudowane modelu temperatury i wilgotności, który emituje telemetrycznego. Symulowany urządzenia znajdują się ilustrujących przepływ zakończenia do końca danych za pomocą rozwiązanie i zapewnia wygodny źródła telemetrycznego i docelowej do poleceń, jeśli jesteś deweloperem wewnętrznej przy użyciu rozwiązanie jako punktu startowego dla niestandardowej implementacji.

Po pierwszym podłączeniu urządzenia do koncentratora IoT zdalnego monitorowania rozwiązania wstępnie, komunikat informacyjny urządzenia wysyłane do Centrum IoT zawiera listę poleceń, które można odpowiedzieć urządzenia. W zdalnego monitorowania rozwiązanie wstępnie polecenia są: 

- *Polecenie ping urządzenia*: urządzenie odpowiada to polecenie z potwierdzeniem. To jest przydatne w przypadku sprawdzanie, czy urządzenie jest nadal aktywne i słuchanie.
- *Rozpoczynanie telemetrycznego*: powoduje, że urządzenie, aby rozpocząć wysyłanie telemetrycznego.
- *Zatrzymywanie telemetrycznego*: powoduje, że urządzenie, aby zatrzymać wysyłanie telemetrycznego.
- *Zmienianie Ustaw punkt temperatury*: Określa wartości telemetrycznego symulowany temperatury urządzenie wysyła. Jest to przydatne do testowania logiki wewnętrznej.
- *Telemetrycznego diagnostycznych*: Określa urządzenie należy wysłania temperatura zewnętrznych jako telemetrycznego.
- *Zmień stan urządzenia*.: ustawia właściwość metadanych stan urządzenia, które urządzenie raportów. Jest to przydatne do testowania logiki wewnętrznej.

Możesz dodać więcej symulowany urządzenia rozwiązanie, które emitować samej telemetrycznego i odpowiadanie na takie same polecenia. 

## <a name="iot-hub"></a>Centrum IoT

W tym rozwiązaniu wstępnie wystąpienie Centrum IoT odpowiada *Chmury bramy* w typowych [architektury rozwiązania IoT][lnk-what-is-azure-iot].

Centrum IoT otrzymuje telemetrycznego z urządzeń w jeden punkt końcowy. Centrum IoT jest również zachowywany określonych punktów końcowych urządzenia miejsce, w którym każdego urządzenia można pobrać polecenia, które są wysyłane do niego.

Centrum IoT udostępnia telemetrycznego Odebrano za pośrednictwem telemetrycznego usługi stronie, przeczytaj punkt końcowy.

## <a name="azure-stream-analytics"></a>Analizy strumieniu Azure

Wstępnie skonfigurowane rozwiązanie używa trzech [Azure analizy strumieniu] [ lnk-asa] zadań (ASA) do filtrowania strumienia telemetrycznego z urządzenia:


- *Zadania DeviceInfo* — wyświetla dane do koncentratora zdarzenia, który przekierowuje urządzenia rejestracji określonej wiadomości, po pierwszym podłączeniu urządzenia lub w odpowiedzi na polecenie **Zmień stan urządzenia** w rejestrze urządzenia rozwiązanie (baza danych DocumentDB). 
- *Zadanie telemetrycznego* — wysyła wszystkie telemetrycznego nieprzetworzonych z magazynem obiektów blob platformy Azure do przechowywania zimnej i oblicza agregacje telemetrycznego, wyświetlane na pulpicie nawigacyjnym rozwiązanie.
- *Zadanie reguły* — umożliwia filtrowanie strumienia telemetrycznego dla wartości, które przekraczają progów dowolną regułę i wyświetla dane do koncentratora zdarzenia. Gdy reguła jest uruchamiana, widok portalu pulpitu nawigacyjnego rozwiązania wyświetla to zdarzenie jako nowy wiersz w tabeli historii, alarmu i uaktywnia zadanie zgodnie z ustawieniami określonymi w widokach reguł i akcji w portalu rozwiązanie.

W tym rozwiązaniu wstępnie zadania ASA częścią na **rozwiązanie IoT wewnętrzna baza danych** w typowych [architektury rozwiązania IoT][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Przetwarzanie zadań

W tym rozwiązaniu wstępnie procesor zdarzeń stanowi część **rozwiązanie IoT wewnętrzna baza danych** w typowych [architektury rozwiązania IoT][lnk-what-is-azure-iot].

Zadania **DeviceInfo** i ASA **reguły** wysyła ich wyniku do koncentratorów zdarzeń dla dostarczania z innymi usługami wewnętrznej. Rozwiązanie używa [EventPocessorHost] [ lnk-event-processor] wystąpienie działa w [WebJob][lnk-web-job]do odczytu wiadomości z tych koncentratory zdarzenia. **EventProcessorHost** używa danych **DeviceInfo** , aby zaktualizować dane urządzenia w bazie danych DocumentDB i używa danych **reguły** w celu wywołania aplikacji logikę i alerty wyświetlane w portalu rozwiązanie aktualizacji.

## <a name="device-identity-registry-and-documentdb"></a>Rejestr tożsamości urządzenia i DocumentDB

Każdy Centrum IoT zawiera [rejestru tożsamości urządzenia] [ lnk-identity-registry] które klucze urządzenia są przechowywane. Centrum IoT korzysta z tych informacji uwierzytelniania urządzeń — urządzenie musi być zarejestrowany i miał prawidłowego klucza można nawiązać koncentratora.

To rozwiązanie przechowuje dodatkowe informacje o urządzeniach, takich jak ich stanu, polecenia, które obsługują i inne metadane. Rozwiązanie używa DocumentDB bazy danych do przechowywania danych określonego rozwiązania urządzenia i portalu rozwiązanie pobiera dane z tej bazy danych DocumentDB do wyświetlania i edytowania.

Rozwiązanie również należy pozostawić dane w rejestrze tożsamości urządzenie synchronizowane z zawartością DocumentDB bazy danych. **EventProcessorHost** używa danych z zadania analizy strumieniu **DeviceInfo** do zarządzania synchronizacji.

## <a name="solution-portal"></a>Portal rozwiązanie

![Rozwiązanie pulpitu nawigacyjnego][img-dashboard]

Portal rozwiązanie jest oparte na sieci web interfejsu użytkownika, który jest używany w chmurze w ramach wstępnie rozwiązania. Umożliwia:

- Wyświetlanie historii telemetrycznego i alarmowe na pulpicie nawigacyjnym.
- Inicjowanie obsługi nowych urządzeniach.
- Zarządzanie i monitorowanie urządzeń.
- Polecenia Wyślij do określonych urządzeń.
- Zarządzaj regułami i akcje.

W tym rozwiązaniu wstępnie portalu rozwiązanie formularze **rozwiązanie IoT wewnętrzna baza danych** i część **przetwarzania i łączności biznesowej** w typowych [architektury rozwiązania IoT][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat architektury rozwiązanie IoT zobacz [usługi Microsoft Azure IoT: Architektura odwołanie][lnk-refarch].

Teraz wiesz, jakie wstępnie rozwiązanie jest, można rozpocząć wdrażając rozwiązanie *zdalne monitorowanie* wstępnie: [Wprowadzenie do rozwiązania wstępnie][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md