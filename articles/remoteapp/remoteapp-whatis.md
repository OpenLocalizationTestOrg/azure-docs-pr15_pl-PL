<properties 
    pageTitle="Co to jest funkcja RemoteApp platformy Azure? | Microsoft Azure" 
    description="Dowiedz się, jak udostępnianie aplikacji i zasobów do dowolnego urządzenia przy użyciu funkcji RemoteApp Azure." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="what-is-azure-remoteapp"></a>Co to jest funkcja RemoteApp platformy Azure?

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Funkcja RemoteApp Azure łączy funkcje programu Microsoft RemoteApp lokalnych kopii przez usługami pulpitu zdalnego Azure. Azure RemoteApp ułatwia Ci zapewnianie bezpiecznego, dostęp zdalny do aplikacji z wielu urządzeń innego użytkownika. Azure RemoteApp zasadniczo obsługuje nietrwałe sesje terminalowymi w chmurze i przejść do ich używać i udostępnianie ich innym użytkownikom.

Z Azure RemoteApp możesz udostępnić innym użytkownikom na niemal dowolnym urządzeniu aplikacji i zasobów. Firma Microsoft udostępniać swoje aplikacje w chmurze, co oznacza, że możemy zajmować sprzętu i skalowanie do spełnienia wymagań użytkownika. Wszystko, co trzeba zrobić to Przekaż aplikacje, które chcesz udostępnić, a następnie Pobierz użytkownikom korzystanie z tych aplikacjach. [Zachować własne urządzenia dostępu użytkownikom](remoteapp-clients.md), podczas zarządzania wszystko za pośrednictwem portalu Azure. Nawet masz opcji przy użyciu swoich poświadczeń firmowej, umożliwiając zabezpieczenia aplikacji i danych.

Czytaj dalej, aby uzyskać więcej informacji na temat Azure RemoteApp lub, jeśli firma Microsoft ma już przekonane, możesz [go teraz wypróbować](https://azure.microsoft.com/services/remoteapp/).

Masz pytania dotyczące Azure RemoteApp? Zapoznaj się z naszą [— często zadawane pytania](remoteapp-faq.md).

Azure RemoteApp jest częścią [Microsoft Virtual Desktop Infrastructure](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Nowy!** Chcesz dowiedzieć się więcej na temat Azure RemoteApp? Czy chcesz sprawdzić poprawność Azure RemoteApp w skali? Dołączanie do naszych co tydzień [Zapytaj ekspertów seminarium w sieci Web](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure zbiory RemoteApp
Istnieją dwa typy [Azure RemoteApp zbiorów](remoteapp-collections.md):


- W **chmurze zbioru** jest obsługiwany w i są przechowywane dane dotyczące programów w chmurze. Użytkownicy mogą korzystać z aplikacji, logując się przy użyciu ich konta Microsoft lub poświadczenia firmowe synchronizowanych lub federacji z usługą Azure Active Directory.

    Wybierz zbiór chmury Jeśli aplikację, którą chcesz udostępnić nie wymaga połączenia z dowolnym zasobem sieci prywatnej firmy (na przykład za pomocą urządzenia sieci VPN). Jeśli aplikacja używa zasobów w Internecie, OneDrive lub Azure, zbioru chmury będzie działać dla Ciebie. Jest również najszybszą tworzenie.

- **Zbioru hybrydowego** jest obsługiwany w i przechowywanie danych w chmurze Azure, ale również umożliwia użytkownikom dostęp do danych i zasobów przechowywanych w sieci lokalnej. Użytkownicy mogą korzystać z aplikacji, logując się przy użyciu poświadczeń firmową synchronizowanych lub federacji z usługą Azure Active Directory.

    Wybierz zbiór hybrydowe, jeśli jest wymagane połączenie z zasobów w firmowej sieci prywatnej. Na przykład jeśli aplikacja wymaga dostępu do jednego z następujących czynności:

    - Serwery plików znajdujących się w sieci intranet
    - Quicken
    - Bazy danych za zaporą

    Jest to zazwyczaj bardziej przydatne dla dużej firmy z dużą liczbą zasobów na ich sieci prywatnych, których nie można przenieść w chmurze.

Różne kolekcje mają różne opcje sieci, w tym celu sprawdź, [jakie zbioru](remoteapp-collections.md) najlepiej pasuje do Ciebie. 


### <a name="updating-your-collection"></a>Aktualizowanie kolekcji
Jedną z najważniejszych różnic między zbiory hybrydowych i chmura jest obsługi aktualizacji oprogramowania. Z kolekcji chmury korzystającego z preinstalowanych obrazu usługi Office 365 ProPlus lub Office 2013 nie masz martwić się o wszystkich aktualizacji. Usługa zachowuje się i na bieżąco, zarówno w aplikacji, jak i w systemie operacyjnym z rozwiniętymi informacjami aktualizacji.

Dla zbiorów hybrydowe, a także zbiory chmury, korzystające z obrazem szablonu niestandardowego są odpowiedzialnych za obraz i aplikacje. W przypadku obrazów domeny możesz sterować aktualizacje za pomocą narzędzi, takich jak Windows Update, zasady grupy lub System Center.

Po zaktualizowaniu obrazu niestandardowego szablonu możesz przekazać nowy obraz do chmury Azure, a następnie zaktualizuj zbioru używać nowego obrazu. (Możesz to zrobić na stronie Azure RemoteApp **Szybki Start** lub pulpitu nawigacyjnego).

Aby uzyskać więcej informacji, zobacz [Aktualizowanie kolekcji](remoteapp-update.md) .

## <a name="supported-remoteapp-clients"></a>Obsługiwani klienci RemoteApp
Azure RemoteApp jest obsługiwana w aplikacji klienckich RemoteApp dla systemu Windows i Windows RT, a także aplikacji Microsoft pulpitu zdalnego dla komputerów Mac, iOS i Android. Użytkowników można użyć te aplikacje na jej telefon komórkowy lub obliczyć urządzenia, aby uzyskać dostęp do nowych programów Azure RemoteApp.

Aby uzyskać więcej informacji o klientach, zobacz [Uzyskiwanie dostępu do aplikacji w Azure RemoteApp](remoteapp-clients.md) .

## <a name="next-steps"></a>Następne kroki
Przejdź! Przetestuj! Rozpoczęcie pracy z Azure RemoteApp pomóc poniższe artykuły:

- [Jakiego rodzaju zbioru należy Azure RemoteApp?](remoteapp-collections.md)
- [Tworzenie obrazu Azure RemoteApp](remoteapp-imageoptions.md)
- [Jak utworzyć zbiór Azure RemoteApp w chmurze](remoteapp-create-cloud-deployment.md)
- [Jak utworzyć zbiór hybrydowych Azure RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Jak licencjonowanie działa w Azure RemoteApp?](remoteapp-licensing.md)
- [Najważniejsze wskazówki dotyczące korzystania z funkcji RemoteApp Azure](remoteapp-bestpractices.md)
- [Funkcja RemoteApp Azure — często zadawane pytania](remoteapp-faq.md)
 

### <a name="help-us-help-you"></a>Pomóż nam ułatwiają 
Czy wiesz, oprócz ocena niniejszego artykułu i wyrażania opinii w dół poniżej, umożliwia zmiany do tego samego artykułu? Coś Brak? Problem? Pisanie jest coś, co jest po prostu mylące czy? Przewijanie w górę, a następnie kliknij przycisk **Edytuj na GitHub** lub **Edytuj** , aby wprowadzić zmiany — te ułatwiających kontakt z nami do recenzji, a następnie po możemy zalogować się nad nimi, zostanie wyświetlony do zmiany i ulepszenia w tym artykule.