<properties
    pageTitle="Omówienie przekaźnika Bus usługi | Microsoft Azure"
    description="Omówienie przekazywania Bus usługi."
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
    ms.date="09/01/2016"
    ms.author="sethm"/>


# <a name="overview-of-service-bus-relay"></a>Omówienie przekazywania Bus usługi

Główna część Bus usługi to usługa scentralizowane (ale bardzo równoważenia obciążenia) *przekazywania* , która umożliwia tworzenie hybrydowych aplikacje, które działają zarówno Azure centrum danych, jak i własne lokalnego środowiska przedsiębiorstwa.  Przekazywania usługi Bus obsługuje wiele różne protokoły transportu i standardy usług sieci web. Ta opcja uwzględnia SOAP WS-, *, a nawet pozostałych. Usługi przekazywania ułatwia aplikacji hybrydowe, umożliwiając bezpiecznego udostępniania usługi Windows Communication Foundation (WCF), które znajdują się w sieci firmowej przedsiębiorstwa w chmurze publicznej, bez konieczności otwierania połączenia przez zaporę sieciową, lub wymagają inwazyjnych zmian w firmowej infrastrukturze sieciowej. 

![Pojęcia przekazywania](./media/service-bus-relay-overview/sb-relay-01.png)

Usługi przekazywania obsługuje tradycyjnych jednokierunkowe wiadomości, żądanie/odpowiedź wiadomości i aby równorzędnych wiadomości. Obsługuje ona również rozkładu zdarzeń w zakresie internet umożliwiające scenariusze publikowania subskrypcji i komunikacji socket dwukierunkowego w celu zwiększenia wydajności lepszą typu punkt-punkt. 

Odnośnie wzór wiadomości usługi lokalnej łączy się z usługą przekazywania przez port ruchu wychodzącego i tworzy socket dwukierunkowego komunikacji z adresu określonym rendez-vous. Klient następnie komunikować się z usługą lokalnego przez wysłanie wiadomości do usługi przekazującej kierowanie adres rendez-vous. Usługi przekazywania zostanie następnie "przekazywania" wiadomości w lokalnej usłudze za pośrednictwem socket dwukierunkowego już w miejscu. Klient nie potrzebujesz bezpośredniego połączenia z usługą lokalnego, nie jest wymagane wiedzieć, gdzie znajduje się usługa, a usługa lokalnego nie musi dowolne porty przychodzące otwarty na zaporze.

Inicjuje połączenie między usługą lokalnego a usługą przekazywania za pomocą pakietu powiązań WCF "przekazywania". W tle powiązań przekazywania mapować nowych elementów wiązania transportu przeznaczonych do tworzenia składników kanału WCF integrujących się z Bus usługa w chmurze. 

## <a name="next-steps"></a>Następne kroki

Szczegółowe informacje na temat przekazywania Bus usługi zobacz następujące tematy.

- [Omówienie architektury Bus usługi Azure](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Jak używać usługi przekazującej Bus usługi](service-bus-dotnet-how-to-use-relay.md)

 