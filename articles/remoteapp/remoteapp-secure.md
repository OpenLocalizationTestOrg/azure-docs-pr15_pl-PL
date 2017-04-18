
<properties
    pageTitle="Zabezpieczanie aplikacji i zasobów w Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak zablokowanie aplikacji i zasobów w Azure RemoteApp"
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



# <a name="secure-apps-and-resources-in-azure-remoteapp"></a>Zabezpieczanie aplikacji i zasobów w Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Funkcja RemoteApp Azure umożliwia użytkownikom dostępu do centralnie zarządzane aplikacje systemu Windows, co pozwala kontrolować, co użytkownicy mogą, a nie.  Ta opcja jest przydatna, gdy użytkownik nawiązuje połączenie z niezarządzanej urządzenia (na przykład ich osobistych Macbook) i chcesz kontrolowanie dostępu użytkowników lub środowiska.

Na przykład jeśli korzystasz z usługi Active Directory do uwierzytelniania użytkowników i chcesz uniemożliwić użytkownikom kopiowania danych z aplikacji, możesz skonfigurować zasady grupy pulpitu zdalnego użytkownikom blok z kopiowanie danych.

Inny przykład to, czy chcesz zablokować dostęp do Internetu dla określonej aplikacji w kolekcji. Można utworzyć regułę zapory systemu Windows, która blokuje dostęp podczas tworzenia obrazu dla zbioru.

## <a name="implementation-options"></a>Opcje implementacji

  Poniżej przedstawiono opcje implementacji klucza, które mogą być stosowane pojedynczo lub w połączeniu stosownie do potrzeb:

1.  Jeśli zbiór RemoteApp jest domeną sprzężone można wymusić wszelkie [Zasady grupy](https://technet.microsoft.com/library/cc725828.aspx) (z wyjątkiem bezczynności i Odłącz przekroczenia limitu czasu zasady opisane [poniżej](../azure-subscription-service-limits.md)).
2.  Alternatywnie do zasad grupy (jeśli kolekcji nie jest domeną sprzężone lub jeśli nie masz prawo uprawnień w AD) można skonfigurować [Zasady lokalne](https://technet.microsoft.com/library/cc775702.aspx) do obrazu szablonu.  Należy zauważyć, że tej grupy zasady zasady lokalne atu, gdy wystąpi konflikt.
3.  Niektóre ustawienia systemu operacyjnego i aplikacji nie są można konfigurować za pomocą zasad, ale można za pomocą klucza rejestru przy użyciu [Narzędzia RegEdit](./remoteapp-hybridtrouble.md) podczas konfigurowania usługi obraz szablonu.
4.  [Zapora systemu Windows](http://windows.microsoft.com/en-US/windows-8/Windows-Firewall-from-start-to-finish) można użyć do kontrolowania dostępu do sieci do i z komputera, miejsce, w którym jest uruchomiona aplikacja. Po prostu upewnij się, że nie zablokować adresy URL i porty zdefiniowany w tym miejscu.
5.  Za pomocą [zasad ograniczeń oprogramowania](https://technet.microsoft.com/library/hh831440.aspx) do kontrolki, które aplikacje i pliki użytkowników może zostać uruchomiony. Na przykład pomysłowy użytkownicy mogą określenie sposobu uruchamiania aplikacji, że nie jest publikowana, ale są dostępne w obrazu, który został użyty do utworzenia kolekcji - zasad ograniczeń oprogramowania można zablokować to.

## <a name="detailed-information"></a>Szczegółowe informacje

- Poniższe zasady licencji są najbardziej użyteczne:
    - [Przekierowywanie urządzeń i zasobów](https://technet.microsoft.com/library/ee791794.aspx)
    - [Przekierowanie drukarki](https://technet.microsoft.com/library/ee791784.aspx)
    - [Profile](https://technet.microsoft.com/library/ee791865.aspx).
- Należy zauważyć, że konfigurowanie przekierowania przy użyciu programu RemoteApp PowerShell modułu (jako widoczna [poniżej](./remoteapp-redirection.md)) oparte na komputerze klienckim, aby wymusić zasady, więc jeśli głównym celem zabezpieczeń należy wymusić zasady za pomocą szablonu zasad lokalnych obraz lub za pomocą zasad grupy.
- [Windows Server 2012 R2 zasady](https://technet.microsoft.com/library/hh831791.aspx).
- [Zasady pakietu Office 2013](https://technet.microsoft.com/library/cc178969.aspx) (w tym [jak dostosowywanie paska narzędzi pakietu Office](https://technet.microsoft.com/library/cc179143.aspx)).
