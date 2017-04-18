<properties 
    pageTitle="Azure RemoteApp — często zadawane pytania | Microsoft Azure" 
    description="Dowiedz się odpowiedzi na najczęściej często zadawane pytania dotyczące Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="azure-remoteapp-faq"></a>Funkcja RemoteApp Azure — często zadawane pytania

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Zadawane przez użytkowników na następujące pytania dotyczące Azure RemoteApp. Umożliwić innym użytkownikom? Odwiedź [Fora RemoteApp](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) i trafić co trzeba wiedzieć, lub rozwijane komentarza poniżej.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Nie możesz znaleźć, czego szukasz? Masz pytanie, na które nie możemy odpowiedzi?
Jeśli nie możesz znaleźć informacje potrzebne, lub możesz mieć dodatkowe pytanie, że firma Microsoft nie jest obejmujące poniżej, przejdź do [forum Azure RemoteApp](http://aka.ms/araforum) i Zadaj pytanie. Zawsze można dodać więcej odpowiedzi w tym miejscu.

## <a name="what-is-azure-remoteapp"></a>Co to jest funkcja RemoteApp platformy Azure? ##


- **Co to jest funkcja RemoteApp platformy Azure?** Funkcja RemoteApp jest usługą Azure ułatwia Ci zapewnianie bezpiecznego, dostęp zdalny do aplikacji z wielu urządzeń innego użytkownika. Dowiedz się więcej o [Azure RemoteApp](remoteapp-whatis.md).
- **Jakie są opcje wdrażania?** Istnieją dwa typy RemoteApp zbiorów: chmury i hybrydowego wdrożenia. Które z nich należy zależy od wielu czynników, takich jak czy potrzebujesz Dołączanie domeny. Porozmawiamy o wszystkich tych decyzji [tutaj](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Szybkie porady na temat korzystania z trybu RemoteApp Azure ##
- **Jak długo, dopóki nie został jest odłączony? Jak długo trwa mogę mieć bezczynne przed udostępnieniem mnie przy uruchomieniu?** 4 godziny. Jeśli użytkownika lub użytkowników jest bezczynny przez 4 godziny, można będzie można automatycznie wylogowany z Azure RemoteApp. Zapoznaj się z innymi domyślnych ustawień w [subskrypcji Azure i limity dotyczące usługi, przydziały oraz ograniczenia](../azure-subscription-service-limits.md).
- **Czy mogę wypróbować tę usługę bezpłatnie?** Wartość Tak. Brak dostępnej bezpłatną wersję próbną przez 30 dni. Po zakończeniu okresu próbnego, można przejść do płatnej konta (które są pomocne w produkcji) lub zaprzestać korzystania z usługi. Rozpocznij pobrać bezpłatną wersję próbną, przechodząc do [portal.azure.com](http://portal.azure.com) - Tworzenie nowego wystąpienia programu RemoteApp. Przy użyciu bezpłatnej wersji próbnej można tworzyć 2 wystąpienia RemoteApp użytkownikom 10 dla każdego wystąpienia. Pamiętaj, że korzystania z wersji próbnej istnieją tylko przez 30 dni.
## <a name="azure-remoteapp-subscription-details"></a>Szczegóły subskrypcji w usłudze Azure RemoteApp ##

- **Jakie są limity usługi?** Aby Dowiedz się więcej o ustawienia domyślne i ograniczenia usługi Azure funkcja RemoteApp w [subskrypcji Azure i limity dotyczące usługi, przydziały oraz ograniczenia](../azure-subscription-service-limits.md). Pozwól nam wiedzieć, jeśli masz więcej pytań.
- **Ilu użytkowników muszę mieć?** Istnieje co najmniej 20 użytkowników. Pozwól mi Powtórz, że jest to bardzo wyczyść — wartość minimalna to 20. Będą naliczane dla 20. 
- **Jaki jest koszt RemoteApp?** Zapoznaj się z [Azure RemoteApp ceny szczegóły ](https://azure.microsoft.com/pricing/details/remoteapp/).
- **Jeden typ zbioru kosztuje więcej niż innego?** Tak, można, w zależności od potrzeb zbioru. Kolekcja hybrydowych wymaga połączenia z Azure RemoteApp do sieci lokalnej. Jeśli korzystasz z istniejącą trasę VNET-Express, jest bez dodatkowych opłat. Jednak użycie nowego VNET Azure i bramy albo rozsyłania Express zostanie naliczona dla [bramy sieci VPN](https://azure.microsoft.com/pricing/details/vpn-gateway) lub [Rozsyłania Express](https://azure.microsoft.com/pricing/details/expressroute/). Ten koszt (dane szczegółowe w łącza) jest na bieżąco miesięczny funkcja RemoteApp platformy Azure kosztów.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Kolekcje — co jest obsługiwane, którego należy użyć, a inne osoby
- **Aplikacje niestandardowe z LOB (LOB) są obsługiwane?** Wartość Tak. Aby użyć niestandardowej aplikacji w Azure RemoteApp, utworzyć [szablon niestandardowy obraz](remoteapp-create-custom-image.md), a następnie przekazanie go do kolekcji RemoteApp.
- **Moje niestandardową aplikację LOB będą działać w Azure RemoteApp?** Najlepszym sposobem na rysunku, będącej się, aby przetestować. Zapoznaj się z [Centrum zgodności RD](http://www.rdcompatibility.com/compatibility/default.aspx).
- **Z jakiej metody wdrażania (chmurze lub hybrydowymi połączeniami) jest zalecane dla swojej organizacji?** Hybrydowe zbiorów podaj najbardziej kompletne środowisko, jeśli chcesz pełnej integracji z logowania jednokrotnego (SSO) i lokalnych bezpiecznego sieci łączności. Kolekcje chmury umożliwia adaptacyjne i łatwego wyodrębnienia wdrożenia przy użyciu wielu metod uwierzytelniania. Dowiedz się więcej na temat [opcji wdrażania](remoteapp-whatis.md).
- **Mamy SQL lub innej bazy danych obsługiwanych lokalnie lub na Azure. Jakiego typu wdrożenia należy użyć?** To zależy od gdzie jest baza danych SQL lub wewnętrznej bazy danych. Jeśli baza danych znajduje się w sieci prywatnej, użyj zbioru hybrydowych. Jeśli baza danych jest udostępniana w Internecie i umożliwia klienta połączenia się z nim połączyć, można korzystać z kolekcji chmury.
- **Co z otwieraniem mapowanie dysku, USB i portu szeregowego udostępnianie Schowka i przekierowanie drukarki?** Wszystkie te funkcje są obsługiwane w Azure RemoteApp. Przekierowywanie schowka drukarki i udostępniania jest domyślnie włączona. Dowiedz się więcej o przekierowywania [tutaj](remoteapp-redirection.md). 


## <a name="template-images"></a>Szablon obrazów
- **Czy można używać chmurki lub istniejącej maszyn wirtualnych jako szablon dla mojego zbioru RemoteApp?** Tak! Możesz utworzyć obraz według maszyn wirtualnych Azure, użyj jednej z obrazów dołączonych do subskrypcji lub utworzyć obraz niestandardowy. Zapoznaj się z [opcji obrazu RemoteApp](remoteapp-imageoptions.md).


## <a name="network-options"></a>Opcje sieci
- **W zbiorze hybrydowych wymaga VNET. Czy firma Microsoft korzysta z naszych istniejących VNET?** Możesz to zrobić w przypadku istniejących VNET VNET Azure. Zobacz "krok 1: Konfigurowanie wirtualnych sieci" [instrukcje zbioru hybrydowych](remoteapp-create-hybrid-deployment.md) Aby uzyskać więcej informacji.
- **Czy mogę korzystać z kolekcji chmury VNET?** Faktycznie możesz. Zapoznaj się z [Utwórz zbiór chmurze](remoteapp-create-cloud-deployment.md), szczególnie kroku 1, aby uzyskać więcej informacji.

## <a name="authentication-options"></a>Opcje uwierzytelniania



- **Jak uwierzytelniania? Które metody są obsługiwane?** W zbiorze chmury obsługuje konta serwera Microsoft i kont usługi Azure Active Directory, które są także kont usługi Office 365. Kolekcja hybrydowych obsługuje tylko konta usługi Azure Active Directory, które zostały zsynchronizowane (za pomocą narzędzie, takie jak [Azure Active Directory Sync](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) z wdrożenia usługi Active Directory systemu Windows Server; w szczególności albo zsynchronizowane z opcją synchronizacji haseł synchronizowane z skonfigurowane federacyjne Active Directory Federation Services (AD FS). Można również skonfigurować [Uwierzytelnianie wieloskładnikowe (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/).

>[AZURE.NOTE]Użytkownicy usługi Azure Active Directory muszą być z dzierżawy, który jest skojarzony z subskrypcją. (Można przeglądać i modyfikować swoją subskrypcję na karcie **Ustawienia** w portalu. Zobacz [Zmienianie dzierżawy usługi Azure Active Directory używane przez RemoteApp](remoteapp-changetenant.md) Aby uzyskać więcej informacji).

- **Dlaczego nie można przekazać moje dostępu do konta usługi Azure Active Directory?** Użytkownicy usługi Azure Active Directory muszą być z katalogu, który jest skojarzony z subskrypcją. Można wyświetlić lub zmodyfikować ten katalog na karcie Ustawienia w portalu. Aby uzyskać więcej informacji, zobacz [Zmienianie dzierżawy usługi Azure Active Directory używane przez RemoteApp](remoteapp-changetenant.md) .

## <a name="clients---what-device-can-i-use-to-access-azure-remoteapp"></a>Klienci - jakiego urządzenia można za pomocą można uzyskać dostęp do funkcji RemoteApp Azure?
Można znaleźć informacje warto klienta, w tym kroki wymagane do zainstalowania różnych klientów na [Uzyskiwanie dostępu do aplikacji w Azure RemoteApp](remoteapp-clients.md).

- **Urządzenia i systemy operacyjne są obsługiwane aplikacje klienckie?**
Pierwszy komputerów i tabletów: 
    - Windows 10 (wersja preview klienta)
    - Windows 8.1 i Windows 8
    - Windows 7 z dodatkiem Service Pack 1
    - Mac OS X
    - Windows RT
    - Tablety z systemem android
    - tablety Ipad i telefony:
    - Telefon iPhone
    - Telefon z systemem android
    - Windows Phone
 
    [Pobierz](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) teraz klienta RemoteApp.
- **Funkcja RemoteApp platformy Azure obsługuje cienkie klientów?** Tak, są obsługiwane następujące cienki klient osadzonego systemu Windows:
    - Osadzonymi standardowy 7
    - Osadzone systemu Windows 8 standardowe
    - Osadzone systemu Windows 8.1 branżowe Pro
    - Przedsiębiorstwo IoT systemu Windows 10

- **Która wersja systemu Windows Server jest obsługiwane dla zdalnego hosta sesji pulpitu (RDSH)?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Obsługa i opinii


- **Co to jest plan pomocy technicznej RemoteApp?** Pomoc dotycząca rozliczeń i subskrypcji zarządzania znajduje się bezpłatnie. Pomoc techniczna jest dostępna za pośrednictwem [usługi Azure planów](https://azure.microsoft.com/support/plans/). Możesz również wyświetlić społeczności bezpłatnej pomocy technicznej przez naszych [forum dyskusyjne Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp). 
- **Jak przesłać opinię?** Odwiedź [forum opinii](https://feedback.azure.com/forums/247748-azure-remoteapp/).
- **Osób, które można rozmawiać, aby dowiedzieć się więcej o Azure RemoteApp?** Oprócz nasze [forum dyskusyjne](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), czyli doskonałe miejsce do Publikuj pytania, można dołączyć tygodniowy [Ask seminarium w sieci Web ekspertów](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), gdzie porozmawiamy o wszystkich rozwiązania tego RemoteApp.
- **Co z otwieraniem dokumentacji RemoteApp?** Jest to okazuje się, że monit. Oprócz zawartości pomocy w szufladzie portalu pomocy (wystarczy kliknąć **?** na dowolnej stronie w portalu) następujące artykuły są dostępne do nauki wszystko o RemoteApp:
    - **Wprowadzenie:**
        - [Co to jest funkcja RemoteApp?](remoteapp-whatis.md)
        - [Co to jest funkcja RemoteApp obrazów szablonu?](remoteapp-images.md)
        - [Jak działa Licencjonowanie?](remoteapp-licensing.md)
        - [Jak funkcja RemoteApp i pakietu Office działają razem?](remoteapp-o365.md)
        - [Jak przekierowywania działa na RemoteApp](remoteapp-redirection.md)?
    - **Wdrażanie:**
        - [Tworzenie szablonu niestandardowego obrazu](remoteapp-create-custom-image.md)
        - [Tworzenie zbioru hybrydowego](remoteapp-create-hybrid-deployment.md)
        - [Tworzenie zbioru chmury](remoteapp-create-cloud-deployment.md)
        - [Konfigurowanie usługi Azure Active Directory dla RemoteApp](remoteapp-ad.md)
        - [Publikowanie aplikacji RemoteApp](remoteapp-publish.md)
    - **Zarządzanie:**
        - [Dodawanie użytkowników](remoteapp-user.md)
        - [Najważniejsze wskazówki dotyczące konfigurowania i używania RemoteApp](remoteapp-bestpractices.md)  

    Klipy wideo! Również mamy wiele klipów wideo dotyczących RemoteApp. Niektóre udostępniają wprowadzenie ([Wprowadzenie do Azure RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)), a inne osoby przeprowadził Cię przez proces rozmieszczania ([rozmieszczania chmury](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) i [wdrożenie hybrydowe](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Je wyewidencjonować!

 
### <a name="help-us-help-you"></a>Pomóż nam ułatwiają 
Czy wiesz, oprócz ocena niniejszego artykułu i wyrażania opinii w dół poniżej, umożliwia zmiany do tego samego artykułu? Czegoś brakuje? Problem? Pisanie jest coś, co jest po prostu mylące czy? Przewijanie w górę, a następnie kliknij przycisk **Edytuj na GitHub** , aby wprowadzić zmiany — te ułatwiających kontakt z nami do recenzji, a następnie po możemy zalogować się nad nimi, zostanie wyświetlony do zmiany i ulepszenia w tym artykule.
