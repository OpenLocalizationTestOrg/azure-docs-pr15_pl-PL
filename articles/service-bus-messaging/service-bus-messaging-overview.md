<properties
    pageTitle="Omówienie wiadomości usługi Bus | Microsoft Azure"
    description="Usługa Bus wiadomości: dostarczania elastyczne danych w chmurze"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="sethm"/>


# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Usługa Bus wiadomości: dostarczania elastyczne danych w chmurze

Microsoft Azure usługi Bus to usługa dostarczania wiarygodnych informacji. Celem tej usługi jest ułatwiają komunikacji. Chcąc wymiany informacji przez dwie lub więcej stron muszą mechanizm przekazywania informacji. Usługa Bus jest mechanizm komunikacji brokered lub innych firm. To jest podobna do usług pocztowych w świecie fizycznym. Usługi pocztowe ułatwiają bardzo wysyłanie różnych rodzajów litery i pakietów z różnymi gwarancji dostarczenia dowolnego miejsca na świecie.

Podobnie pocztę przedstawiania litery, usługa Bus jest przekazywanie informacji elastyczne od nadawcy i adresata. Usługi SMS zapewnia dostarczenie informacji, nawet jeśli obie strony nigdy nie są oba online w tym samym czasie, czy nie są dostępne w tym samym czasie dokładnie. W ten sposób wiadomości jest podobne do listu, gdy-brokered komunikacji jest podobne do umieszczania telefonicznego (lub używania rozmowy telefonicznej za - przed połączenia oczekiwania i identyfikator rozmówcy, które są znacznie więcej, takich jak brokered wiadomości).

Nadawcy wiadomości można również wymagają różnych cech dostarczenia tym transakcje, wykrywania duplikatów, wygasania opartych na czasie i tworzeniu partii. Deseni mają także analogie pocztowy: powtarzanie dostarczenia, wymagany podpis, Zmień adres lub odwołanie.

Usługa Bus obsługuje dwa różne desenie wiadomości: *przekazywania* i *brokered wiadomości*.

## <a name="service-bus-relay"></a>Usługa Bus przekazywania

Składnik [przekazywania](../service-bus-relay/service-bus-relay-overview.md) Bus usługi jest usługą scentralizowane (ale bardzo równoważenia obciążenia), która obsługuje wiele różnych protokołów i standardy usług sieci Web. Ta opcja uwzględnia SOAP WS-, *, a nawet pozostałych. [Usługi przekazywania](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) jest dostępnych wiele opcji łączności różnych przekazywania i może pomóc w Wynegocjuj bezpośrednie równorzędnych do połączenia, gdy jest możliwe. Usługa Bus jest zoptymalizowana pod kątem .NET deweloperzy, którzy używają Windows Communication Foundation (WCF), oba wydajność i użyteczności i zapewnia pełny dostęp do swojej usługi przekazywania za pośrednictwem interfejsów protokołu SOAP i Zatrzymaj. Dzięki temu SOAP lub pozostałych środowiska programowania do integracji z Bus usługi.

Usługi przekazywania obsługuje tradycyjnych jednokierunkowe wiadomości, żądanie/odpowiedź wiadomości i aby równorzędnych wiadomości. Obsługuje także rozkładu zdarzeń w zakresie Internet aby włączyć publikowanie Subskrybuj scenariusze i komunikacji socket dwukierunkowego w celu zwiększenia wydajności lepszą typu punkt-punkt. Odnośnie wzór wiadomości usługi lokalnej łączy się z usługą przekazywania przez port ruchu wychodzącego i tworzy socket dwukierunkowego komunikacji z adresu określonym rendez-vous. Klient następnie komunikować się z usługą lokalnego przez wysłanie wiadomości do usługi przekazującej kierowanie adres rendez-vous. Usługi przekazywania zostanie następnie "przekazywania" wiadomości w lokalnej usłudze za pośrednictwem socket dwukierunkowa już w miejscu. Klient nie jest konieczne bezpośredniego połączenia z usługą lokalnego ani nie jest wymagane miejsce, w którym znajduje się usługa oraz Usługa lokalnego nie musi dowolne porty przychodzące otwarty na zaporze.

Inicjuje połączenie między usługą lokalnego a usługą przekazywania za pomocą pakietu powiązań WCF "przekazywania". W tle powiązań przekazywania zamapuj transportu elementy zaprojektowane w celu tworzenia składników kanału WCF integrujących się z Bus usługa w chmurze.

Usługa Bus przekazywania oferuje wiele korzyści, ale wymaga użycia serwera i klienta, aby być w trybie online w tym samym czasie, aby można było wysyłać i odbierać wiadomości. Nie jest optymalne dla komunikacji styl HTTP, w których wnioski nie mogą być zwykle długotrwałe ani dla klientów, którzy połączyć tylko sporadycznie, takich jak przeglądarek, aplikacji dla urządzeń przenośnych i tak dalej. Brokered wiadomości obsługuje rozłączone komunikacji i ma swoje zalety; Klienci i serwery można połączyć, gdy trzeba i ich operacji w sposób asynchroniczny.

## <a name="brokered-messaging"></a>Brokered wiadomości

W odróżnieniu od programu przekazywania [brokered wiadomości](service-bus-queues-topics-subscriptions.md) mogą być traktować jako asynchroniczne lub "tymczasowe odłączona". Producenci (nadawców) i odbiorcy (odbiorców) nie muszą być w trybie online w tym samym czasie. Wiadomości infrastruktury niezawodne są przechowywane wiadomości w "brokera" (na przykład kolejki) przed dużo strona jest gotowa do ich odbierać. Dzięki temu części aplikacji rozproszonej rozłączenia, albo dobrowolnie; na przykład obsługi lub z powodu awarii składnika, bez wpływu na cały system. Ponadto aplikacja może być tylko do trybu online pewnych porach dnia, na przykład z systemu zarządzania zapasami tylko są wymagane do uruchomienia na koniec dnia roboczego.

Podstawowe składniki usługi Bus brokered infrastruktury wiadomości są kolejki, tematy i subskrypcji.  Podstawowa różnica polega na tym, że tematy pomocy technicznej możliwości publikowania subskrypcji, które mogą być używane dla zaawansowanych oparte na zawartości przesyłania i dostarczania logiczny, w tym wysyłanie do wielu adresatów. Składniki Włączanie nowych asynchroniczne scenariuszy wiadomości, takie jak czasowy rozdzielenie publikowania subskrypcji i równoważenia obciążenia. Aby uzyskać więcej informacji na temat tych obiektów wiadomości zobacz [kolejki Bus usługi, tematy i](service-bus-queues-topics-subscriptions.md).

Podobnie jak w przypadku infrastruktury przekazywania brokered funkcja wiadomości znajduje się dla programistów WCF i .NET Framework, a także za pośrednictwem pozostałych.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat usługi Bus wiadomości, zobacz następujące tematy.

- [Podstawy Bus usługi](service-bus-fundamentals-hybrid-solutions.md)
- [Obsługa kolejek Bus, tematy i subskrypcji](service-bus-queues-topics-subscriptions.md)
- [Jak używać usługi Bus kolejki](service-bus-dotnet-get-started-with-queues.md)
- [Jak używać usługi Bus tematy i subskrypcji](./service-bus-dotnet-how-to-use-topics-subscriptions.md)
 
