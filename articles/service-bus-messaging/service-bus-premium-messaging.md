<properties
    pageTitle="Usługa Bus Premium i wiadomości standardowe ceny omówienie poziomów | Microsoft Azure"
    description="Usługa Bus Premium i standardowy wiadomości"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2016"
    ms.author="darosa;sethm"/>

# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Usługa Bus Premium i standardowy poziomów wiadomości 

Bus usługi wiadomości, które zawierają wiadomości obiektów, takich jak kolejki i tematów, enterprise łączy wiadomości możliwości sformatowany znaczeń właściwych w chmurze skali publikowania — subskrypcji. Bus usługi wiadomości jest używany jako podstawy komunikacji wiele rozwiązań zaawansowanych chmury.

Warstwa *Premium* wiadomości usługi Bus adresy typowych zleceń klientów wokół skali, wydajności i dostępności dla kluczowych aplikacji. Zestawy funkcji są niemal identyczne, te dwie warstwy Bus usługi wiadomości mają służyć przypadków użycia różnych.

Pewne różnice wysokiego poziomu zostaną wyróżnione w poniższej tabeli.

| Premium                               | Standardowe                       |
|---------------------------------------|--------------------------------|
| Wysokiej wydajności                       | Zmienna przepustowość            |
| Przewidywalne wydajności               | Opóźnienie zmiennych               |
| Przewidywalne ceny                   | Płatność przy zakupie zmiennych ceny |
| Możliwość skalowania obciążenie pracą w górę lub w dół | N/D!                            |
| Rozmiar wiadomości > 256KB                  | Rozmiar wiadomości jest 256KB          |

**Usługa Bus Premium wiadomości** jest izolacji zasobu w warstwie Procesora i pamięci, dzięki czemu obciążenie pracą każdego klienta jest uruchamiany oddzielnie. Ten kontener zasobu jest określana mianem *wiadomości jednostek*. Przestrzeń nazw poszczególnych premium przydzielonego co najmniej jedną jednostkę wiadomości. Możesz kupić 1, 2 lub 4 jednostek wiadomości dla każdego obszaru nazw usługi Bus Premium. Pojedynczy obciążenie pracą lub podmiotów może obejmować wiele jednostek wiadomości i liczba jednostek wiadomości można zmienić w będzie, mimo że rozliczeń znajduje się w 24-godzinnego lub codziennie stawka opłaty. Wynik to przewidywalne i powtarzalnych wydajności dotyczących rozwiązania oparte na Bus usługi.

Nie tylko jest to wydajności, bardziej przewidywalne i dostępne, ale jest również szybciej. Usługa Premium Bus wiadomości opiera się na aparat magazynu wprowadzone w [Koncentratory zdarzenia Azure](https://azure.microsoft.com/services/event-hubs/). Dzięki wiadomościom Premium, wydajność szczytowa jest szybciej niż z poziomu standardowy.

## <a name="premium-messaging-technical-differences"></a>Różnice techniczne wiadomości Premium

Poniżej przedstawiono kilka różnice między Premium i standardowy poziomów wiadomości.

### <a name="partitioned-queues-and-topics"></a>Podzielony na partycje kolejki i tematy

Podzielony na partycje kolejki i tematy są obsługiwane w Premium wiadomości, ale nie działają tak samo jak warstwy standardowe oraz podstawowe Bus usługi wiadomości. Nie jest już zawiera konkurencji zasobów związanych z udostępnionej platformy oraz Premium wiadomości nie korzysta z SQL przechowywania danych. W wyniku podziału nie jest konieczne. Ponadto liczba partition została zmieniona od 16 partycje w standardowej wiadomości do 2 partycje w Premium. Masz dwie partycje zapewnia dostępność i jest liczbą bardziej odpowiednie dla środowiska środowisko uruchomieniowe Premium. Aby uzyskać więcej informacji o partycjonowaniu zobacz [kolejek Partitioned i tematów](service-bus-partitioning.md).

### <a name="express-entities"></a>Express jednostek

Ponieważ wiadomości Premium działa w środowisku całkowicie odizolowane środowisko uruchomieniowe, express podmioty nie są obsługiwane w Premium nazw. Aby uzyskać więcej informacji na temat funkcji express Zobacz właściwość [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) .

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat usługi Bus wiadomości, zobacz następujące tematy.

- [Wprowadzenie do Premium Bus usługi Azure wiadomości (wpis w blogu)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Wprowadzenie do Premium Bus usługi Azure wiadomości (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Omówienie wiadomości Bus usługi](service-bus-messaging-overview.md)
- [Jak używać usługi Bus kolejki](service-bus-dotnet-get-started-with-queues.md)
