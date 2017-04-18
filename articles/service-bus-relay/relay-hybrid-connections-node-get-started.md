<properties
    pageTitle="Wprowadzenie do przekazywania połączeń hybrydowych | Microsoft Azure"
    description="Jak pisać aplikacji konsoli węzła hybrydowego połączeń"
    services="service-bus"
    documentationCenter="node"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="node"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub"/>

# <a name="get-started-with-relay-hybrid-connections"></a>Wprowadzenie do przekazywania połączeń hybrydowego

[AZURE.INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Co to osiągnąć

Ponieważ połączenia hybrydowych wymaga zarówno klienta, jak i składnik serwera, zostanie utworzony dwóch aplikacji konsoli w tym samouczku. Wykonaj poniższe kroki:

1. Tworzenie nazw przekazywania, za pomocą portalu Azure.

2. Tworzenie połączenia hybrydowe, za pomocą portalu Azure.

3. Napisz serwer aplikacji konsoli do odbierania wiadomości.

4. Napisz klienta aplikacji konsoli do wysyłania wiadomości.

## <a name="prerequisites"></a>Wymagania wstępne

1. [Node.js](https://nodejs.org/en/) (w przykładzie użyto 7.0 węzeł).

2. Subskrypcję usługi Azure.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Tworzenie przestrzeń nazw za pomocą portalu Azure

Jeśli masz już utworzony przestrzeń nazw przekazywania, należy przejść do sekcji [Utwórz połączenie hybrydowych za pomocą portalu Azure](#2-create-a-hybrid-connection-using-the-azure-portal) .

[AZURE.INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Tworzenie połączenia funkcji hybrydowych za pomocą portalu Azure

Jeśli masz już połączenie hybrydowych utworzone, należy przejść do sekcji [Tworzenie aplikacji serwera](#3-create-a-server-application-listener) .

[AZURE.INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Tworzenie aplikacji serwera (odbiornik)

Aby odsłuchać i odbierać wiadomości z przekazywania, możemy zapisze aplikacji konsoli Node.js.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. Tworzenie aplikacji klienckiej (nadawcy)

Wysyłanie wiadomości do przekazywania, możemy zapisze aplikacji konsoli Node.js.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. uruchamianie aplikacji

1. Uruchom aplikację serwera.

2. Uruchom z aplikacją kliencką i wpisz tekst.

3. Upewnij się, że konsolę aplikacji serwera Wyświetla tekst, który został wprowadzony w aplikacji klienckiej.

![uruchamianie aplikacji](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Gratulacje, utworzył aplikację połączeń hybrydowych zakończenia do końca.

## <a name="next-steps"></a>Następny krok:

- [Relay — często zadawane pytania](relay-faq.md)
- [Tworzenie obszaru nazw](relay-create-namespace-portal.md)
- [Wprowadzenie do programu .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Wprowadzenie do węzła](relay-hybrid-connections-node-get-started.md)