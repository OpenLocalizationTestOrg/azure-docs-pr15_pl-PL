<properties 
    pageTitle="Architektura Bus usługi | Microsoft Azure"
    description="W tym artykule opisano wiadomości i przekazywania architektury Bus usługi Azure przetwarzania."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm" />

# <a name="service-bus-architecture"></a>Architektura Bus usługi

Ten artykuł zawiera opis wiadomości i przekazywania architektury Bus usługi Azure przetwarzania.

## <a name="service-bus-scale-units"></a>Usługa Bus skali

Usługa Bus jest zorganizowane według *skali*. Jednostka skali jest jednostką wdrożenia i zawiera wszystkie składniki wymagane uruchamiania usługi. Każdego regionu wdraża jedną lub więcej jednostek skali Bus usługi.

Przestrzeń nazw Bus usługi są mapowane na jednostkę skali. Jednostki skali obsługuje wszystkie rodzaje jednostek Bus usługi: przekaźniki i brokered podmioty wiadomości (kolejek, tematy, subskrypcje). Jednostka skali Bus usługi składa się z następujących składników:

- **Zestaw węzłów bramy.** Węzły bramy uwierzytelniania przychodzących żądań i przekazywania żądań obsługi. Każdy węzeł bramy ma publiczny adres IP.

- **Zestaw wiadomości węzły broker.** Wiadomości węzły brokera przetwarzania żądań dotyczących SMS podmiotów.

- **Magazyn jednej bramy.** W sklepie bramy przechowuje dane dla każdej jednostki, która jest zdefiniowana w tym jednostki skali. W sklepie bramy jest zaimplementowana na bieżąco bazy danych platformy SQL Azure.

- **Wiele magazynów wiadomości.** Wiadomości sklepy przechowywania wiadomości kolejki, tematy i subskrypcje, zdefiniowane w tym jednostki skali. Zawiera również wszystkie dane subskrypcji. Jeśli nie włączono [podzielone na partycje wiadomości podmioty](service-bus-partitioning.md) kolejki lub temat są mapowane do jednego magazynu wiadomości. Subskrypcje są przechowywane w tym samym magazynie wiadomości jako temat nadrzędnej. Z wyjątkiem usługi Bus [Wiadomości Premium](service-bus-premium-messaging.md)sklepy wiadomości są wykonywane na bieżąco baz danych platformy SQL Azure.

## <a name="containers"></a>Kontenery

Każdy obiekt wiadomości zostanie przypisany określonego kontenera. Kontener jest logiczna określająca używa dokładnie jedną sklepu wiadomości wszystkich odpowiednich danych dla tego kontenera. Każdy kontener jest przypisana do wiadomości węzeł broker. Zazwyczaj są kontenerów więcej niż wiadomości węzły broker. Dlatego każdej wiadomości węzeł brokera ładuje wielu kontenerów. Rozkład kontenerów do węzła brokera wiadomości są zorganizowane, tak, aby wszystkie wiadomości węzły brokera jednakowo są ładowane. Jeśli Załaduj deseniu zmiany (na przykład jeden z pobiera kontenerów bardzo zajęty) lub w przypadku wiadomości węzeł brokera staje się tymczasowo niedostępny, kontenerów są rozdzielane węzłach brokera wiadomości.

## <a name="processing-of-incoming-messaging-requests"></a>Przetwarzanie przychodzących żądań wiadomości

Gdy klient wysyła żądanie usługi Bus, równoważenia obciążenia Azure kieruje go do dowolnego węzłów bramy. Węzeł bramy zezwala na żądanie. Jeżeli żądanie dotyczy podmiot wiadomości (kolejki, temat, subskrypcji), węzeł bramy wyszukuje jednostki w magazynie bramy i określa, w którym magazynu wiadomości znajduje się jednostka. Następnie wyszukuje który wiadomości węzeł brokera aktualnie obsługiwanych ten kontener i przesyła żądanie do tego węzła brokera wiadomości. Wiadomości węzeł brokera przetwarza żądanie i aktualizacje stanu jednostki w magazynie kontener. Węzeł brokera wiadomości przesyła odpowiedź do węzła bramy, otrzyma odpowiednie odpowiedzi z powrotem do klienta, który wydane oryginalne wezwanie.

![Przetwarzanie przychodzących żądań wiadomości](./media/service-bus-architecture/IC690644.png)

## <a name="processing-of-incoming-relay-requests"></a>Przetwarzanie przychodzących żądań przekazywania

Gdy klient wysyła żądanie usługi Bus, równoważenia obciążenia Azure kieruje go do dowolnego węzłów bramy. Żądanie jest żądanie słuchanie, węzeł bramy tworzy nowe przekazywania. Jeśli żądanie jest żądanie połączenia do określonych przekazywania, węzeł bramy przesyła żądanie połączenia do węzła bramy, który jest właścicielem przekazywania. Węzeł bramy, który jest właścicielem przekaz wysyła żądanie rendez-vous klientowi słuchanie pytaniem odbiornika, aby utworzyć kanał tymczasowe do węzła bramy, które otrzymano żądanie połączenia.

Po nawiązaniu połączenia przekazywania klienci wymieniać wiadomości za pomocą węzeł bramy, która służy do rendez-vous.

![Przetwarzanie przychodzących żądań przekazywania](./media/service-bus-architecture/IC690645.png)

## <a name="next-steps"></a>Następne kroki

Teraz, gdy przeczytane omówienie architektury Bus usługi, aby rozpocząć pracę, skorzystaj z poniższych łączy:

- [Omówienie wiadomości Bus usługi](service-bus-messaging-overview.md)
- [Podstawy Bus usługi](service-bus-fundamentals-hybrid-solutions.md)
- [Korzystanie z usługi Bus kolejek kolejce rozwiązanie wiadomości](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)
