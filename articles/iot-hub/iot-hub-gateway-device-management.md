<properties
 pageTitle="Włączanie zarządzanymi urządzeniami za bramy IoT | Microsoft Azure"
 description="Temat wskazówki za pomocą bramy IoT utworzony przy użyciu zestawu SDK bramy IoT wraz z urządzeń zarządzane przez Centrum IoT."
 services="iot-hub"
 documentationCenter=""
 authors="chipalost"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="cstreet"/>
 
# <a name="enable-managed-devices-behind-an-iot-gateway"></a>Włączanie zarządzanymi urządzeniami za bramy IoT

## <a name="basic-device-isolation"></a>Izolacji urządzenia podstawowego

Organizacje często używane bram IoT zwiększenie bezpieczeństwa ogólnego ich rozwiązań IoT. Niektóre urządzenia trzeba będzie wysłać danych w chmurze, ale nie są w stanie ochrony przed zagrożeniami w Internecie. Te urządzenia z zewnętrznych wątków można zabezpieczyć Pozwól komunikowanie się za pomocą światem przez bramę.

Brama znajduje się na obramowaniu między bezpiecznego środowiska i otwórz internet. Urządzenia porozmawiać z bramy i bramy przekazuje wiadomości wzdłuż do miejsca docelowego poprawne chmury. Brama jest zaostrzony przed wątków zewnętrznych, blokuje żądania nieautoryzowanego umożliwia autoryzowanych ruchu przychodzącego i przesyłania dalej ruchu przychodzącego do właściwego urządzenia.

![][1]

Można także umieścić urządzenia, które można chronić się za brama dla dodatkowej warstwy zabezpieczeń. W tym scenariuszu tylko potrzebne do zaktualizowania bramy poprawkami przed najnowszą luk, zamiast aktualizacji systemu operacyjnego na wszystkich urządzeniach z systemem operacyjnym.

## <a name="isolation-plus-intelligence"></a>Izolacji oraz analizy

Zaostrzony router jest wystarczające bramy w celu wyodrębnienia po prostu urządzenia. Jednak rozwiązań IoT często wymagają, że brama zapewnia analizy więcej niż po prostu izolowanie urządzenia. Można na przykład, aby zarządzać urządzeniami z chmury. Jesteś w stanie użyj [LWM2M](https://github.com/OpenMobileAlliance/OMA_LwM2M_for_Developers/wiki), protokół zarządzania standardowe urządzenie zarządzania w chmurze w części rozwiązania. Jednak urządzenia wysłać z obsługą telemetrycznego przy użyciu innych niż TCP/IP Protocol (protokół). Ponadto urządzenia warzywa dużej ilości danych i chcesz przekazać filtrowanych podzestaw telemetrycznego. Można utworzyć rozwiązanie, które zawiera bramy IoT do postępowania z dwoma strumieniami odrębnych danych. Bramy powinien:

-   Opis **telemetrycznego**, filtru, a następnie przekazanie go w chmurze przez bramę. Brama jest już router proste, po prostu przesyłania dalej danych między urządzeniem i chmury.

-   Po prostu wymiany **danych zarządzania urządzenia LWM2M** między urządzenia i chmury. Bramy nie jest konieczne opis danych do niej, a tylko musi upewnij się, że dane przekazywane i z powrotem między urządzenia i chmury.

Na poniższym rysunku przedstawiono w tym scenariuszu:

![][2]

## <a name="the-solution-azure-iot-device-management-and-the-iot-gateway-sdk"></a>Rozwiązanie: Zarządzanie urządzeniami Azure IoT i IoT SDK bramy 

Publicznej podglądu wersji [zarządzania urządzeniami Azure IoT] [ lnk-device-management] i ten scenariusz włączania wersji beta [SDK bramy IoT Azure] . Brama obsługuje każdego strumienia danych w następujący sposób:

-   **Telemetrycznego**: za pomocą SDK bramy IoT można tworzyć potok rozumie, filtry i wysyła telemetrycznego dane w chmurze. Zestaw SDK bramy IoT zawiera kod, który wykonuje części tego procesu w imieniu projektanta. Więcej informacji na temat architektury zestawu SDK można znaleźć w zestawie [SDK bramy IoT — wprowadzenie] [ lnk-gateway-get-started] samouczka.

-   **Zarządzanie urządzeniami**: Zarządzanie urządzeniami Azure zapewnia klientem LWM2M uruchamianej na urządzeniu, a także w interfejsie chmury wydawania poleceń zarządzania na urządzeniu.
    
    Nie wymagają wszelkie specjalne logikę bramy, ponieważ nie trzeba procesu wymienianych między urządzeniem i Twoim IoT centrum danych LWM2M. Możesz włączyć Udostępnianie połączenia internetowego, funkcja wielu nowoczesny systemach operacyjnych, na bramy w celu umożliwienia wymiany danych LWM2M. Ponieważ SDK bramy IoT obsługuje wiele systemów operacyjnych, możesz wybrać odpowiedni system operacyjny dla tego scenariusza. Poniżej zamieszczono instrukcje dotyczące włączania Udostępnianie połączenia internetowego dla [systemu Windows 10] i [Ubuntu], dwa wiele obsługiwane systemy operacyjne.

Na poniższej ilustracji przedstawiono architektury wysokiego poziomu umożliwia w tym scenariuszu przy użyciu [zarządzania urządzeniami Azure IoT] [ lnk-device-management] i [SDK bramy IoT Azure].

![][3]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać informacje dotyczące używania SDK bramy IoT, zobacz następujące samouczki:

- [Brama IoT SDK — wprowadzenie do korzystania z Linux][lnk-gateway-get-started]
- [Brama IoT SDK — wysyłanie wiadomości urządzenia w chmurze z urządzeniem symulowane przy użyciu Linux][lnk-gateway-simulated]

Aby jeszcze bardziej Poznawanie możliwości Centrum IoT, zobacz:

- [Przewodnik dewelopera][lnk-devguide]
- [Symulowanie urządzenia z IoT SDK bramy][lnk-gateway-simulated]

<!-- Images and links -->
[1]: media/iot-hub-gateway-device-management/overview.png
[2]: media/iot-hub-gateway-device-management/manage.png
[Azure bramy IoT SDK]: https://github.com/Azure/azure-iot-gateway-sdk/
[Windows 10]: http://windows.microsoft.com/en-us/windows/using-internet-connection-sharing#1TC=windows-7
[Ubuntu]: https://help.ubuntu.com/community/Internet/ConnectionSharing
[3]: media/iot-hub-gateway-device-management/manage_2.png
[lnk-gateway-get-started]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-gateway-simulated]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-devguide]: iot-hub-devguide.md