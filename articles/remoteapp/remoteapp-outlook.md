<properties
    pageTitle="Używanie programu Outlook w Azure RemoteApp | Microsoft Azure" 
    description="Dowiedz się, jak skonfigurować program Outlook działa w Azure RemoteApp | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Za pomocą programu Microsoft Outlook w Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Funkcja RemoteApp Azure obsługuje usługi Office 365 programu Microsoft Outlook. Przeczytaj więcej o sposobie [działania pakietu Office w Azure RemoteApp](remoteapp-officesubscription.md). Istnieje kilka zalecanych ustawień dla programu Outlook, gdy używana w Azure RemoteApp.

## <a name="cached-mode"></a>Tryb buforowanej
Tryb buforowanej jest zalecana konfiguracja, gdy w Azure RemoteApp za pomocą programu Outlook. Po skonfigurowaniu konta programu Outlook 2013 do trybu buforowanej wymiany za pomocą programu Outlook 2013 działa z lokalną kopię skrzynki pocztowej programu Microsoft Exchange użytkownika, który jest przechowywany w pliku danych trybu offline (OST plik) na komputerze użytkownika, razem z trybu Offline książki adresowej (OAB). Tryb buforowanej skrzynki pocztowej, a OAB są okresowo aktualizowane w usłudze Office 365. Dowiedz się więcej o [różnicach między tryb buforowanej i w trybie online](https://technet.microsoft.com/library/jj683103.aspx).

Użytkownik może zaznaczyć **Trybu buforowanej wymiany** lub w **Trybie Online** podczas konfigurowania konta lub zmieniając ustawienia kont. Można także wdrożyć jednego lub drugiego za pomocą narzędzia dostosowywania pakietu Office (OCT) lub zasad grupy.  

Przeczytaj [instrukcje krok po kroku dotyczące włączania tryb buforowanej](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Wyszukiwanie
W Azure RemoteApp za pomocą wyszukiwania w programie Outlook występują ograniczenia. Azure RemoteApp używa puli maszyny wirtualne, aby zezwalały na sesji użytkowników. Indeksowanie wyszukiwania zależy od identyfikator komputera, która jest różna dla różnych maszyny wirtualne. Istnieje możliwość, że zawsze podczas logowania użytkownika do Azure RemoteApp, są one kierowane do nowych maszyn wirtualnych. Oznacza to, jeśli firma Microsoft włączyć wyszukiwanie lokalne, indeksatora będzie działać w każdej zmianie identyfikator komputera (Jeśli użytkownik jest na różnych maszyn wirtualnych). W zależności od rozmiaru. Plik OST indeksatora może zająć dużo czasu do ukończenia i korzystanie z zasobów potrzebnych do innych aplikacji. Wyszukiwanie tylko jest powolne, ale może dać wyniki. Za pomocą profilu konta w trybie Online czy obejścia tego problemu, ale ogólną wydajność może odczuwać z powodu braku lokalnej pamięci podręcznej (patrz łącze powyżej, aby uzyskać więcej informacji o różnicach między tryb buforowanej i w trybie online). Niestety nie można wyłączyć indeksowane lokalnego wyszukiwania i wyszukiwanie w trybie online nie można włączyć domyślnie w programie Outlook 2013.
