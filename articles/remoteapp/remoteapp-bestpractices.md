<properties
    pageTitle="Najważniejsze wskazówki Azure RemoteApp | Microsoft Azure"
    description="Najważniejsze wskazówki dotyczące konfigurowania i używania Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Najważniejsze wskazówki dotyczące konfigurowania i używania Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Poniższe informacje mogą pomóc możesz skonfigurować i efektywnie używać Azure RemoteApp.

## <a name="connectivity"></a>Łączność


- Zawsze używaj najnowszej wersji klienta. Przy użyciu starszych klientów może powodować problemy z łącznością i innych ograniczone środowiska. Włączanie aktualizacji automatycznych aplikacji dla swojego urządzenia zapewni zawsze zainstalowano najnowszą wersję.
- Zawsze używaj najbardziej stabilny i niezawodnego połączenia z Internetem dostępne dla Ciebie.  
- Użyj obsługiwane tylko połączenia serwera proxy dla łączności optymalnej wydajności.  Serwer proxy SOCKS nie jest obsługiwane.

## <a name="applications"></a>Aplikacje


- Zapisz i zamknij aplikacje RemoteApp po wykonaniu tych czynności w aplikacji. Nie zamykając aplikacji może spowodować utratę danych.
- Sprawdź poprawność niestandardowe aplikacje przed użyciem ich w Azure RemoteApp. Ta opcja uwzględnia zapewnienie pracy na platformie wiele sesji i nie używają niepotrzebne zasobów, takich jak pamięci i Procesora, który może być starve innego użytkownika do tego samego zbioru. Aby uzyskać informacje Pobierz i zapoznaj się z [Aplikacji zgodności najważniejsze wskazówki dotyczące usług pulpitu zdalnego](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Konfiguracja i zarządzanie


- Aktualizowanie obrazów szablonu, instalowanie aktualizacji oprogramowania i innych krytyczne poprawki, stosownie do potrzeb. Dzięki temu, że jako Azure RemoteApp automatyczne skal spotkania możliwości poprawkami każdego wystąpienia.  
- Upewnij się, że wdrożenia usługi Active Directory Federation Services (AD FS) jest bezpieczne i niezawodne. W przeciwnym razie uwierzytelnienia klienta może się nie powieść, blokując użytkownikom dostęp do funkcji RemoteApp Azure.
- Konfigurowanie szablonu obrazów z zainstalowane aplikacje, ról lub funkcji tak, aby były one stateless. Ta osoba nie będą miały na wszystkie wystąpienia maszyn wirtualnych w usłudze RemoteApp są w stanie trwałych.
    - Przechowywać wszystkie dane użytkowników w profilów użytkowników lub w innych lokalizacjach miejsca do magazynowania zewnętrznych do usługi, takie jak lokalnego pliku akcji lub OneDrive.
    - Przechowywania danych udostępniony w lokalizacji przechowywania zewnętrznych do usługi, takie jak lokalnego pliku akcji lub OneDrive.
    - Skonfiguruj ustawienia systemowe w obraz szablonu, a nie na poszczególnych maszyn wirtualnych w usłudze.
    - Wyłącz automatyczne aktualizacje oprogramowania dla aplikacji opublikowanych — zamiast tego ręcznie zastosować do obrazu szablonu i je przetestować, przed wdrożeniem w szablonie.
