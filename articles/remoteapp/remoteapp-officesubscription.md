
<properties 
    pageTitle="Jak korzystać z subskrypcji usługi Office 365 z Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak za pomocą subskrypcji usługi Office 365 w Azure RemoteApp, aby udostępnić aplikacje pakietu Office."
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-your-office-365-subscription-with-azure-remoteapp"></a>Jak korzystać z subskrypcji usługi Office 365 z Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Czy wiesz, że możesz użyć istniejącej subskrypcji usługi Office 365 w Azure RemoteApp celu udostępnienia aplikacje pakietu Office z chmury? Poniżej informacji na temat usługi Office 365 + opcje Azure RemoteApp, tym łącza do artykułów dotyczących usługi Office 365, które pomagają w pełni wykorzystać subskrypcji.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Jak używać kont usługi Office 365 dla Azure RemoteApp?
Zapoznaj się z nowego artykułu Piotra dla wszystkich informacji: [jak służy funkcja RemoteApp Azure za pomocą kont użytkowników usługi Office 365](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-to-run-office-applications-in-azure-remoteapp"></a>Czy można używać do uruchamiania aplikacji pakietu Office w Azure RemoteApp subskrypcji usługi Office 365?

Tak! W rzeczywistości korzystając ze swojej subskrypcji usługi Office 365 jest jedynym sposobem Przesuń do aplikacji pakietu Office w taki sposób, aby Azure RemoteApp.

(Uwaga: Jeśli wdrożenia Azure RemoteApp został dostarczony przez partnera hostingu, mogą być w stanie dostarczyć Ci licencji pakietu Office, oparte na [Umowę licencyjną dostawcy usługi](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx))


Największą zaletą subskrypcji usługi Office 365 jest, możesz korzystać z tym samym licencji użytkownika na wielu różnych platformach i środowisk, łącznie z chmurą Azure umożliwia. Podczas korzystania z aplikacji pakietu Office w Azure RemoteApp nie musisz zakupić dodatkowe licencje lub skonfigurować istniejące licencji w żaden sposób specjalne. Potrzebny jest subskrypcji usługi Office 365, która zawiera [Usługi Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx).

Usługa Office 365 ProPlus umożliwia [aktywacji współużytkowanego komputera](https://technet.microsoft.com/library/Dn782860.aspx) — ta funkcja umożliwia tymczasowe aktywacji opartej na użytkownika dla pakietu Office w wirtualnej i środowiska chmury takich jak Azure RemoteApp (i usługami pulpitu zdalnego).

Które plany usługi Office 365 obejmują usługi Office 365 ProPlus? Zapoznaj się z tabeli [dostępność usługi w obrębie każdego planu](https://technet.microsoft.com/library/office-365-plan-options.aspx) . Należy zauważyć, że nie wszystkie plany obejmują usługi Office 365 ProPlus (na przykład plan usługi Office 365 Business). Jeśli plan nie jest, Rozważ uaktualnienie do planu, który ma (na przykład usługi Office 365 Education E3).

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>Dobrze, w jaki sposób są Mój Office 365 ProPlus licencji używanych z Azure RemoteApp?

Licencja każdego użytkownika dla usługi Office 365 ProPlus umożliwia jednego użytkownika aktywowania aplikacji pakietu Office na maksymalnie 5 komputerów i tabletów i telefonów. Każda aktywacja jest zarejestrowany użytkownika do momentu ich Dezaktywuj pakiet Office na urządzeniu. (Użytkownicy mogą zarządzać swoimi urządzeniami w [portalu usługi Office 365](https://portal.office365.com/)).

Z Azure RemoteApp pojedynczy użytkownik może Zaloguj się do kilku komputerach tego samego dnia bez wiedzy. Jest tak, ponieważ usługa automatycznie zarządza i skale zasobów w chmurze, gdy użytkownik widzi tylko te aplikacje i programy, którym udostępniono. W tym scenariuszu usługi Office 365 ProPlus oferuje trybie aktywacji współużytkowanego komputera — oznacza to, że tego użytkownika nie musisz wykonywać dowolne zarządzanie licencjami, aby uzyskać dostęp do tych zasobów, a poszczególne komputery nie są wliczane limit aktywacji 5 komputera.

Jak długo (Administrator) przypisaniu licencji usługi Office 365 ProPlus dla użytkowników, mogą również używać pakietu Office na swoich urządzeniach osobistych, a także do kolekcji Azure RemoteApp.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Które aplikacje pakietu Office można używać z usługi Office 365 i Azure RemoteApp?

Subskrypcja usługi Office 365 umożliwia aktywowanie i udostępnianie pakietu Office 2013 we wdrożeniach Azure RemoteApp. Pracujemy obecnie nie obsługują korzystanie z innych wersji pakietu Office z Azure RemoteApp. Ta opcja uwzględnia pakietu Office 2003, Office 2007, Office 2010 i Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Co z otwieraniem program Visio Pro lub programu Project Pro?

Office 365 ProPlus obrazu dostępnego w ramach subskrypcji RemoteApp obejmuje zarówno program Visio Pro i program Project Pro. Ale nie można aktywować tych programach za pomocą subskrypcji usługi Office 365 ProPlus — mają własne licencji. Można je uaktywnić w [portalu usługi Office 365](https://portal.office365.com/). 

Nie masz licencję tych programów, jeśli nie chcesz, aby z nich korzystać. Po prostu aktywowanie programów, których chcesz użyć — i pominąć pozostałe. Będą nadal wyświetlane na obrazie, ale nie można ich używać. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Jak rozpocząć pracę z usługą Office 365 i Azure RemoteApp?

Po zapoznaniu się szczegółowe informacje o licencji usługi Office 365, Przejdźmy ułatwiające rozpoczęcie korzystania z niego w Azure RemoteApp — jest bardzo proste:

Po utworzeniu kolekcji Azure RemoteApp za pomocą **Usługi Office 365 ProPlus (subskrypcji wymagane)** obrazu.

![Azure obraz RemoteApp z usługi Office 365 Pro Plus](./media/remoteapp-officesubscription/remoteapp-officeimage.png)


Ten obraz zawiera najnowszą wersję systemu Windows Server i usługi Office 365 ProPlus. Po skonfigurowaniu kolekcji (łącznie aplikacje publikowania) użytkowników po prostu zalogować się do Azure RemoteApp (przy użyciu swoich klientów RemoteApp) i podaj swoje poświadczenia usługi Office 365 dowolnej aplikacji pakietu Office. Licencje są automatycznie dostarczane bez dowolny zestaw w górę lub wymagane zarządzania.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Czy mogę utworzyć niestandardowy obraz z usługi Office 365 ProPlus?

Możesz utworzyć obraz niestandardowy kolekcji, która zawiera usługi Office 365 ProPlus. Ma opcji 2 - Użyj obrazu z galerii Azure, które udostępniamy lub można utworzyć obraz niestandardowy i instalowanie usługi Office 365 ProPlus.

### <a name="use-the-azure-gallery-image"></a>Używanie obrazu z galerii Azure

Najłatwiejszym sposobem wdrożenia usługi Office 365 ProPlus do kolekcji jest zawarte w subskrypcji usługi Azure RemoteApp, [warto zacząć od jednego zdjęcia z galerii Azure](remoteapp-image-on-azurevm.md) . Upewnij się, że możesz wybrać obraz **preinstalowane w systemie Windows Server hosta sesji pulpitu zdalnego z usługi Office 365 ProPlus** . Następnie należy zainstalować inne aplikacje, które mają na tym obrazie i masz kontynuować.

### <a name="use-a-custom-image"></a>Użyj obrazu niestandardowego

Zawsze można utworzyć niestandardowy obraz — można utworzyć [Maszyn wirtualnych Azure](remoteapp-image-on-azurevm.md) lub [Utwórz obraz lokalnie](remoteapp-create-custom-image.md) i przekaż go do Azure. W obu przypadkach upewnij się, że możesz zainstalować usługi Office 365 ProPlus przy użyciu myszy węzeł aktywacji współużytkowanego komputera. Użyj [Narzędzia wdrażania pakietu Office](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) i postępuj zgodnie z [instrukcjami](https://technet.microsoft.com/library/Dn782858.aspx) instalacji.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Wyłącz automatyczne aktualizacje dla usługi Office 365 ProPlus w obraz niestandardowy - ważne

Obraz niestandardowy jest używana przez Azure RemoteApp jako szablonu do dodawania dodatkowych zasobów jako żądanie ze swojego wzrostu użytkowników. Aby zapobiec opóźnienia i problemy z połączeniem, wyłącz automatyczne aktualizacje dla pakietu Office na ilustracji. W przeciwnym razie wszystkich zasobów utworzone za pomocą tego szablonu zostanie zaktualizowany automatycznie po uruchomieniu. Użyj zamiast tego procesu Azure RemoteApp standardowy aktualizacji obraz niestandardowy. W ten sposób aktualizowanie aplikacji pakietu Office na obraz szablonu, a następnie zezwolić na Azure RemoteApp przejmują pobrać aktualizacje użytkowników.

Aby wyłączyć automatyczne aktualizacje, Dodaj następujący plik konfiguracyjny narzędzia wdrażania pakietu Office:

        <Updates Enabled="FALSE" />

Aby teraz pliku konfiguracji powinien zawierać następujących wierszy:
    
        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>W jaki sposób aktualizacji obrazu przy użyciu usługi Office 365 ProPlus

Istnieje wiele powodów, aby zaktualizować obraz w kolekcji. Poniżej przedstawiono kilka:

- Uzyskiwanie najnowszych aktualizacji systemu Windows 
- Pobierz najnowsze aktualizacje aplikacji usługi Office 365 ProPlus
- Aktualizowanie aplikacji niestandardowej
- Zmienianie innych ustawień konfiguracji samego obrazu

Aby uzyskać procedurę zakończenia do końca aktualizowanie kolekcji, aby użyć obrazu, aktualizacji, przejdź [tutaj](remoteapp-update.md). Jednak aby uzyskać informacje dotyczące aktualizowania obrazu i usługi Office 365 ProPlus, zapoznaj się z następujących informacji.

Możesz mieć dwie opcje dotyczące aktualizowania obrazu — zamienianie obrazu zupełnie nową lub ręcznie zaktualizowania istniejącego obrazu.

### <a name="replace-your-image-with-the-latest-azure-gallery-image--add-customizations"></a>Zamienianie obrazu przy użyciu najnowszych Azure Galeria + Dodaj dostosowań
Po wybraniu tej opcji należy powiadomić firmę Microsoft przejmują aktualizacji dla systemu Windows Server i usługi Office 365 ProPlus. Zamiast aktualizowania istniejącego obrazu, utworzysz całkowicie nowy obraz oparte na najnowszych obrazu z galerii. Następnie powtórz kroki od dowolnego jak poprzednio Dostosuj obraz — zainstalować aplikacje niestandardowe modyfikowanie konfiguracji obrazu itp.

Zdjęcia z galerii są regularnie aktualizowane, więc można umieścić łatwe i wiedzy, że aplikacje systemu Windows Server i usługi Office 365 ProPlus są aktualne. Oczywiście rozwiązanie jest, że należy zastosować wprowadzone dostosowania za każdym razem, gdy zostanie wyświetlony nowy obraz. Można tworzyć skrypty, aby zautomatyzować ustawienie wprowadzonych zmian.

### <a name="manually-update-your-existing-image"></a>Ręczne aktualizowanie istniejącego obrazu

Po wybraniu tej opcji umożliwia standardowych narzędzi Windows stosowanie aktualizacji do obrazu. Dla usługi Office 365 ProPlus Użyj narzędzia wdrażania pakietu Office o pobranie i zainstalowanie najnowszych aktualizacji i wersje pakietu Office 365 ProPlus.

> [AZURE.IMPORTANT] Pamiętaj — wyłączyć usługi Office 365 ProPlus aktualizacje automatyczne.

Potrzebujesz więcej informacji na temat aktualizacji przy użyciu narzędzia wdrażania pakietu Office?

- [Wdrażanie Szybka instalacja dla produktów usługi Office 365 przy użyciu narzędzia wdrażania pakietu Office](https://technet.microsoft.com/library/JJ219423.aspx)
- [Wdrażanie i aktualizowanie usługi Office 365 ProPlus, przy użyciu narzędzia wdrażania pakietu Office](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (wideo)
- [Konfigurowanie ustawień aktualizacji dla usługi Office 365 ProPlus](https://technet.microsoft.com/library/dn761708.aspx)
