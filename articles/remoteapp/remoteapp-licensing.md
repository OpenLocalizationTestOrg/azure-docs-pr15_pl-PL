<properties
    pageTitle="Licencjonowanie Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak działa Licencjonowanie w Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-does-licensing-work-in-azure-remoteapp"></a>Jak licencjonowanie działa w Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Aby skonfigurowaną usługi Azure RemoteApp z szablonów utworzonych przez i chcesz opublikować aplikacje do użytkowników. Ale nadal jest jedno ostatniej ustalanie: licencjonowania. Tak jak działa licencjonowania dla RemoteApp i aplikacje, które udostępniasz przy użyciu funkcji RemoteApp?

Funkcja RemoteApp nie wymaga ani Windows licencji CAL pulpitu zdalnego. Twoja subskrypcja trwa istotnych samej strony RemoteApp. (Zapoznaj się z szczegóły [ceny planów](https://azure.microsoft.com/pricing/details/remoteapp)).

Jeśli używasz jeden z obrazów, które znajduje się w ramach subskrypcji, możesz udostępnić dowolnej z zainstalowanym obrazu bez określania jej oddzielnej licencji dla aplikacji. Na przykład jeśli używasz systemu Windows Server 2012 R2 obraz szablonu do utworzenia kolekcji, możesz udostępnić ochrona punktu końcowego Centrum systemu użytkowników. Jedynym wyjątkiem od tej reguły są usługi Office 365 ProPlus, której wymaga oddzielnych subskrypcji, i Office 2013, którego nie można udostępniać w zbiorze produkcji.

Jeśli chcesz użyć obrazu szablonu usługi Office 365, który zawiera RemoteApp, musisz mieć *istniejący* plan usługi Office 365 ProPlus. Dotyczy to także dowolnej aplikacji usługi Office 365, którą możesz opublikować za pomocą szablonu niestandardowego. Musisz aktywować aplikacji przy użyciu własnej subskrypcji. Dotyczy to zarówno wersji próbnej i płatnej subskrypcji. Jeśli chcesz używać obraz szablonu usługi Office 365 w wersji próbnej, *i nie masz jeszcze subskrypcję*, przejdź do strony usługi Office 365, aby [utworzyć konto](https://go.microsoft.com/fwlink/p/?LinkID=403802) subskrypcji wersji próbnej. Zobacz [jak funkcja RemoteApp i pakietu Office działają razem?](remoteapp-o365.md) uzyskać więcej informacji.

Jeśli podczas okresu próbnego nie chcesz uzyskać subskrypcji wersji próbnej usługi Office 365, użyj pakietu Office 2013 Professional Plus obraz szablonu, który zawiera RemoteApp. Ten obraz szablonu może być używany tylko do 30 dni i nie można przekonwertować na zbiór płatnej.

W przypadku innych aplikacji musisz upewnij się, że masz licencję na udostępnianie aplikacji.

Który ma sens, prawy? Możesz opublikować dowolną aplikację, którą masz prawnie uprawnienia do udostępniania. I musisz upewnij się, że naprawdę są mogą udostępniać swoje programy.

Uwaga w chmurze kolekcji można CAL lub licencja zbiorcza Umowa. *Można* użyć umowę licencyjną głośność do aktywowania aplikacji w zbiorze hybrydowych (poza biurem). Wystarczy zainstalować je na obrazie szablonu z pliku z licencją zbiorczą. Wykonaj informacje z dostawcą aplikacji w celu zainstalowania licencji w środowisku pulpitu zdalnego.
