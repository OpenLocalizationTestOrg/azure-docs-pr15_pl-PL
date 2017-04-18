<properties
    pageTitle="Co to jest Azure przekazywania? | Microsoft Azure"
    description="Omówienie przekazywania Azure"
    services="service-bus"
    documentationCenter=".net"
    authors="banisadr"
    manager="timlt"
    editor="" />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="babanisa" />

# <a name="what-is-azure-relay"></a>Co to jest Azure przekazywania?

Azure usługi przekazującej ułatwia aplikacji hybrydowe, umożliwiając bezpieczne udostępnianie usług, które znajdują się w sieci firmowej przedsiębiorstwa w chmurze publicznej, bez konieczności otwierania połączenia przez zaporę sieciową, lub mogą być wymagane inwazyjnych zmian w firmowej infrastrukturze sieciowej. Funkcja przekazywania obsługuje wiele różne protokoły transportu i standardy usług sieci web.

Usługi przekazywania obsługuje tradycyjnych jednokierunkowe żądanie/odpowiedź i ruchu w sesjach równorzędnych. Obsługuje ona również rozkładu zdarzeń w zakresie internet umożliwiające scenariusze publikowania subskrypcji i komunikacji socket dwukierunkowego w celu zwiększenia wydajności lepszą typu punkt-punkt. 

W strukturze przesyłanie zawartości odnośnie danych usługi lokalnej łączy się z usługą przekazywania przez port ruchu wychodzącego i tworzy socket dwukierunkowego komunikacji z adresu określonym rendez-vous. Klienta następnie komunikować się z usługą lokalnego, wysyłając ruch do usługi przekazującej kierowanie adres rendez-vous. Usługi przekazywania zostanie następnie "przekazywania" dane do usługi lokalnego za pośrednictwem socket dwukierunkowego zarezerwowane dla każdego klienta. Klient nie potrzebujesz bezpośredniego połączenia z usługą lokalnego, nie jest wymagane wiedzieć, gdzie znajduje się usługa, a usługa lokalnego nie musi dowolne porty przychodzące otwarty na zaporze.

Elementy kluczowe funkcje udostępniane przez przekazywania są dwukierunkowe, niebuforowanego komunikacji przez granice sieci z ograniczania TCP przypominających odnajdowania punktu końcowego, stan, a czy zabezpieczeń punktu końcowego. Funkcjami i przekazywania różnią się od technologii integracji poziom sieci, takich jak sieci VPN w że go może być ograniczone do punkt końcowy jednej aplikacji na jednym komputerze, natomiast technologii sieci VPN jest o wiele bardziej inwazyjnych, opiera się na zmiany środowiska sieciowego.

Azure przekazywania występują dwie funkcje:

1. [Połączenia hybrydowym](#hybrid-connections) — używa Otwórz standardowy Sockets Web umożliwienie realizacji scenariuszy dla wielu platform

2. [Przekaźniki WCF](#wcf-relays) - używa Windows Communication Foundation umożliwiające zdalne wywołania procedury

Połączenia hybrydowych i przekaźniki WCF włączyć bezpieczne połączenie assests, która istnieje w sieci firmowej przedsiębiorstwa. Wykorzystywanie jedna nad drugą zależy od określonych potrzeb poniżej:

|                                    | Przekazywania WCF | Hybrydowe połączenia |
| ---------------------------------- |:---------:|:------------------:|
| **WCF**                            |     x     |                    |
| **Podstawowe .NET**                      |           |         x          |
| **.NET framework**                 |     x     |         x          |
| **JavaScript/NodeJS**              |           |         x          |
| **Java***                          |           |         x          |
| **Zgodne ze standardami protokołu Open**  |           |         x          |
| **Wiele modeli Programing RPC** |           |         x          |
*<sub>Według dostępności ogólne</sub>

## <a name="hybrid-connections"></a>Hybrydowe połączenia

Azure przekazywania połączeń hybrydowego jest bezpieczne, Otwórz protokół rozwoju istniejących funkcji przekazywania, które mogą być wykonywane na wszystkich platformach i w dowolnym języku, który ma podstawowych funkcji WebSocket, zawierający jawnie interfejsu API WebSocket wspólne przeglądarki sieci web. Połączenia hybrydowego jest oparty na HTTP i WebSockets.

## <a name="wcf-relays"></a>Przekaźniki WCF

Przekazywania WCF działa dla pełnego .NET Framework (NETFX) i WCF. Inicjuje połączenie między usługą lokalnego a usługą przekazywania za pomocą pakietu powiązań WCF "przekazywania". W tle powiązań przekazywania mapować nowych elementów wiązania transportu przeznaczonych do tworzenia składników kanału WCF integrujących się z Bus usługa w chmurze.

## <a name="service-history"></a>Historia serwisu

Połączenia hybrydowych transplants pierwsze, jednakowo o nazwie funkcja "BizTalk usług", który został utworzony na przekazywania WCF Bus usługi Azure. Nowa funkcja połączeń hybrydowych uzupełnia istniejące przekazywania WCF i te funkcje usługi dwóch wystąpią przez siebie w usłudze przekazywania w najbliższej przyszłości; udostępnianie typowych bramy, ale mogą w inny sposób różnych implementacji.

Przekazywania WCF jest starsze przekazywania, oferowanie, że wielu klientów może być już używane w programie modeli programing WCF.

## <a name="next-steps"></a>Następny krok:

- [Relay — często zadawane pytania](relay-faq.md)
- [Tworzenie obszaru nazw](relay-create-namespace-portal.md)
- [Wprowadzenie do programu .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Wprowadzenie do węzła](relay-hybrid-connections-node-get-started.md)