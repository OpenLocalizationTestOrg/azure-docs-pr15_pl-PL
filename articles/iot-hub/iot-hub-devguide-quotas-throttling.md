<properties
 pageTitle="Przewodnik dewelopera — przydziałów i ograniczanie | Microsoft Azure"
 description="Azure IoT Centrum deweloperów przewodniku — opis przydziałów, które dotyczą Centrum IoT i oczekiwane zachowanie ograniczania"
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

# <a name="reference---quotas-and-throttling"></a>Odwołanie — przydziałów i ograniczanie

## <a name="quotas-and-throttling"></a>Przydziałów i ograniczanie

Każdej subskrypcji Azure może zawierać najwyżej 10 IoT koncentratory.

Każdy Centrum IoT ponieważ jest ono inicjowane z określonej liczbie jednostek w określonej wersji (Aby uzyskać więcej informacji, zobacz [Azure IoT Centrum cennik][lnk-pricing]). Jednostka SKU i liczba jednostek określają maksymalną kwotę dzienny wiadomości, które możesz wysłać.

Jednostka SKU określa również ograniczania ograniczenia, które Centrum IoT wymusza na wszystkie operacje.

## <a name="operation-throttles"></a>Operacja ograniczenia

Limity operacji są ograniczenia stawek, które są stosowane w zakresach minuta i mają na celu uniknięcia abuse. Centrum IoT próbuje należy unikać zwracania błędów, o ile to możliwe, ale uruchamiania zwracanie wyjątków, jeśli naruszenia ograniczenia przez dłuższy czas.

Poniżej przedstawiono listę wymuszoną ograniczenia. Wartości odnoszą się do poszczególnych Centrum.

| Ograniczenia | Wartość na Centrum |
| -------- | ------------- |
| Tożsamość rejestru operacji (Tworzenie, pobrać, listy, aktualizowanie i usuwanie) | 5000-min i jednostki (S3) <br/> 100-min i jednostka (S1 i S2). |
| Połączenia urządzenia | 6000-s jednostek (S3) 120-s/jednostka (S2) 12-s jednostek (S1). <br/>Co najmniej 100 na sekundę. <br/> Na przykład dwóch jednostek S1 są 2\*12 = 24/sec, ale co najmniej 100-s przez jednostek. W przypadku dziewięciu S1, masz 108/sec (9\*12) między jednostek. |
| Wysyła urządzenia w chmurze | 6000-s jednostek (S3) 120-s/jednostka (S2) 12-s jednostek (S1). <br/>Co najmniej 100 na sekundę. <br/> Na przykład dwóch jednostek S1 są 2\*12 = 24/sec, ale co najmniej 100-s przez jednostek. W przypadku dziewięciu S1, masz 108/sec (9\*12) między jednostek. |
| Wysyła chmury do urządzenia | 5000-min i jednostki (S3), 100-min i jednostka (S1 i S2). |
| Otrzymuje chmury do urządzenia | 50000-min i jednostki (S3) 1000-min i jednostka (S1 i S2). |
| Operacje przekazywania plików | 5000 przekazywanie plików powiadomienia min i jednostki (S3), 100 plików przekazywania powiadomienia min i jednostki (w przypadku S1 i S2). <br/> 10000 URI skojarzeń zabezpieczeń można się z konta magazynu platformy Azure w tym samym czasie.<br/> 10 skojarzeń zabezpieczeń identyfikatory URI urządzenia można się w tym samym czasie. | 

Jest ważne wskazać, że przepustowość *połączenia urządzenia* decyduje stawki, co nowego połączenia urządzenia można ustalić z koncentratora IoT, a nie maksymalną liczbę urządzeń jednocześnie. Ograniczenia w zależności od liczba jednostek, których zainicjowano obsługę administracyjną dla Centrum.

Na przykład jeśli kupisz całość S1, możesz uzyskać przepustowość połączeń 100 na sekundę. Oznacza to, że aby połączyć 100 000 urządzeń, ma co najmniej 1000 sekund (około 16 minut). Jednak może mieć dowolną liczbę jednocześnie podłączone urządzenia mają urządzeń zarejestrowane w rejestrze tożsamości urządzenia.

Szczegółowe omówienie Centrum IoT ograniczania zachowania, zobacz blogu [Centrum IoT ograniczania i][lnk-throttle-blog].

>[AZURE.NOTE] W dowolnym momencie jest możliwe, aby zwiększyć przydziałów lub ograniczenia przepustowości przez zwiększenie liczby jednostek ustanawianie w Centrum IoT.

>[AZURE.IMPORTANT] Operacje rejestru tożsamości są przeznaczone do użytku wykonywalna zarządzania urządzeniami i inicjowania obsługi administracyjnej scenariuszy. Czytania lub aktualizowanie dużej liczby tożsamości urządzenie jest obsługiwane przez [Importowanie i eksportowanie zadania][lnk-importexport].

## <a name="next-steps"></a>Następne kroki

Inne tematy odwołanie w tym przewodniku Deweloper IoT Centrum obejmują:

- [Centrum IoT punkty końcowe][lnk-devguide-endpoints]
- [Języka kwerend twins, metody i zadania][lnk-devguide-query]
- [IoT MQTT Centrum pomocy technicznej][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md