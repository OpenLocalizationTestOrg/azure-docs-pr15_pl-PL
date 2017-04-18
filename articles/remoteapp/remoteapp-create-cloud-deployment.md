<properties 
    pageTitle="Jak utworzyć zbiór chmura Azure RemoteApp | Microsoft Azure" 
    description="Dowiedz się, jak utworzyć wdrożeniu RemoteApp Azure, która umożliwia zapisanie danych w chmurze Azure." 
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
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-cloud-collection-of-azure-remoteapp"></a>Jak utworzyć zbiór Azure RemoteApp w chmurze

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Istnieją dwa typy [Azure RemoteApp zbiorów](remoteapp-collections.md): 

- Chmury: znajduje się całkowicie Azure. Można zapisać wszystkie dane w chmurze (aby zbioru tylko do chmury) lub do połączenia kolekcji VNET i zapisywanie danych.   
- Hybrydowe: zawiera wirtualnej sieci dostępu lokalnego - wymaga użycia Azure AD oraz środowiska usługi Active Directory w lokalnej.

Ten samouczek przeprowadzi Cię przez proces tworzenia zbioru chmury. Istnieją cztery kroki: 

1.  Utwórz zbiór Azure RemoteApp.
2.  Opcjonalnie można skonfigurować synchronizacji katalogów. Jeśli korzystasz z platformy Azure AD + usługi Active Directory, należy zsynchronizować użytkowników, kontaktów i haseł z usługi Active Directory w lokalnej dzierżawy usługi Azure AD.
5.  Publikowanie aplikacji.
6.  Konfigurowanie dostępu użytkowników.


**Przed rozpoczęciem**

Należy wykonać poniższe czynności przed utworzeniem kolekcji:

- [Utwórz konto](https://azure.microsoft.com/services/remoteapp/) w usłudze Azure RemoteApp. 
- Gromadzenie informacji o użytkownikach, których chcesz udzielić dostępu do. Może to być informacje o koncie Microsoft i informacje o koncie pracy usługi Active Directory dla użytkowników.
- W poniższej procedurze założono, że jesteś, albo będzie używany jeden z szablonów obrazów w ramach subskrypcji lub czy masz już przekazane obraz szablonu, który ma być używany. Jeśli musisz przekazać obraz innego szablonu, możesz to zrobić na stronie obrazów szablonów. Po prostu kliknij przycisk **Przekaż obraz szablonu** i postępuj zgodnie z instrukcjami w kreatorze. 
- Chcesz korzystać z usługi Office 365 ProPlus obrazu? Zapoznaj się z informacje [poniżej](remoteapp-officesubscription.md).
- Chcesz podać niestandardowe aplikacje lub LOB programy? Tworzenie nowego [obrazu](remoteapp-imageoptions.md) i używać go w zbiorze chmury.
- Ustalanie, czy jest potrzebny nawiązywania połączenia z VNET. Jeśli chcesz nawiązać połączenie VNET, upewnij się, spełnia [wskazówki dotyczące zmiany rozmiaru](remoteapp-vnetsizing.md) i że [można nawiązać RemoteApp](remoteapp-vnet.md). Zapoznaj się z [artykuł planowaniu VNET ](remoteapp-planvnet.md), aby uzyskać więcej informacji.
- Jeśli używasz VNET zdecyduj, czy chcesz dołączyć go do domeny lokalnej usługi Active Directory.

## <a name="step-1-create-a-cloud-collection---with-or-without-a-vnet"></a>Krok 1: Tworzenie zbioru chmury — z lub bez VNET##


Wykonaj następujące czynności, aby **utworzyć zbiór tylko do chmury**:

1. W portalu zarządzania przejdź do strony RemoteApp.
2. Kliknij pozycję **Nowy > Szybkie tworzenie**.
3. Wprowadź nazwę kolekcji, a następnie zaznacz obszar.
4. Wybierz plan, że chcesz użyć — standardowy lub podstawowe.
5. Wybierz szablon ma być używany dla tej kolekcji. 

    **Porada:** Subskrypcji pakietu RemoteApp pochodzi z [obrazów szablonów](remoteapp-images.md) zawierającym usługi Office 365 lub pakietu Office 2013 (dla wersji próbnej) programów, niektóre opublikowane (na przykład program Word) i innych gotowa do opublikowania. Można również utworzyć nowy [obraz](remoteapp-imageoptions.md) i używać go w zbiorze chmury.


1. Kliknij pozycję **zbiór tworzenie RemoteApp**.
    
    **Ważne:** Może potrwać do 30 minut do zapewniania obsługi kolekcji.

Po utworzeniu kolekcji Azure RemoteApp kliknij dwukrotnie nazwę kolekcji. Który powoduje wyświetlenie strony **Szybki Start** — jest to miejsce, w którym Dokończ konfigurowanie kolekcji.

Aby utworzyć **chmury + zbioru VNET**, wykonaj następujące czynności:

1. W portalu zarządzania przejdź do strony Azure RemoteApp.
2. Kliknij przycisk **Nowy** > **Tworzenie z VNET**.
3. Wprowadź nazwę dla zbioru.
4. Wybierz plan, że chcesz użyć — standardowy lub podstawowe.
5. Wybierz pozycję VNET już utworzone. Nie wiesz, jak to zrobić? Na obecnym wykonać czynności, w tym temacie [hybrydowych](remoteapp-create-hybrid-deployment.md) .
6. Zdecyduj, czy chcesz dołączyć do kolekcji z domeną. Jeśli tak, konieczne będzie integracja Azure AD przy użyciu AD Connect a środowiskiem usługi Active Directory. Który omówiono poniżej w **kroku 2**.
6. Kliknij pozycję **zbiór tworzenie RemoteApp**.


## <a name="step-2-configure-active-directory-directory-synchronization-optional"></a>Krok 2: Konfigurowanie synchronizacji katalogów usługi Active Directory (opcjonalnie) ##

Jeśli chcesz używać usługi Active Directory, funkcja RemoteApp platformy Azure wymaga synchronizacji katalogu między usługi Azure Active Directory i usługi Active Directory w lokalnej, aby zsynchronizować użytkowników, kontaktów i haseł do Twojej dzierżawy usługi Azure Active Directory. Zobacz [Konfigurowanie usługi Active Directory dla Azure RemoteApp](remoteapp-ad.md) w planowaniu informacji. Możesz również przejść bezpośrednio do [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) informacji.

## <a name="step-3-publish-apps"></a>Krok 3: Publikowanie aplikacji ##

Aplikacja Azure RemoteApp jest aplikacji lub programu, w którym można udostępnić użytkownikom. Znajduje się w obraz szablonu, które zostały przekazane do kolekcji. Gdy użytkownik uzyskuje dostęp do aplikacji, pojawia się aplikacja do uruchamiania w środowisku lokalnym, ale naprawdę działa w maszyny wirtualnej Azure. 

Zanim użytkownicy mogą uzyskać dostęp do aplikacji, musisz opublikować je — publikowanie aplikacji umożliwia użytkownikom dostęp do aplikacji w klienta pulpitu zdalnego.
 
Możesz opublikować wiele aplikacji do kolekcji Azure RemoteApp. Po wyświetleniu strony publikowania kliknij przycisk **Publikuj** , aby dodać program. Albo można publikować w menu **Start** obraz szablonu lub określając ścieżkę na obraz szablonu dla aplikacji. Jeśli chcesz dodać do menu **Start** , wybierz aplikację, aby opublikować. Jeśli wybierzesz wprowadź ścieżkę do aplikacji, podaj nazwę dla tej aplikacji i ścieżkę do miejsca na obraz szablonu, w którym jest zainstalowany.

## <a name="step-4-configure-user-access"></a>Krok 4: Konfigurowanie dostępu użytkowników ##

Teraz, gdy użytkownik utworzył kolekcji, musisz dodać użytkowników, których chcesz korzystać z zasobów zdalnych. Jeśli korzystasz z usługi Active Directory, użytkownicy podane dostęp do muszą istnieć w dzierżawie usługi Active Directory skojarzone z subskrypcją użyto do utworzenia tej kolekcji.

1.  Na stronie Szybkie uruchamianie kliknij pozycję **Konfigurowanie dostępu użytkowników**. 
2.  Wprowadź konta służbowego (z usługi Active Directory) lub konta Microsoft, którego chcesz udzielić dostępu.

    **Uwagi:** 

    Upewnij się, że używasz “user@domain.com” format.

    Jeśli korzystasz z usługi Office 365 ProPlus w kolekcji, należy użyć tożsamości usługi Active Directory dla użytkowników. Pomaga to sprawdzić poprawność licencjonowania. 

3.  Po zatwierdzeniu użytkowników kliknij przycisk **Zapisz**.


## <a name="next-steps"></a>Następne kroki ##

To wszystko — pomyślnie utworzone i wdrożone kolekcji chmury Azure RemoteApp. Następnym krokiem jest mają użytkownikom pobieranie i instalowanie klienta pulpitu zdalnego. Adres URL można znaleźć klienta na stronie Azure RemoteApp Szybki Start. Następnie dziennik użytkowników do klienta i uzyskać dostęp do aplikacji, które zostały opublikowane.

### <a name="help-us-help-you"></a>Pomóż nam ułatwiają 
Czy wiesz, oprócz ocena niniejszego artykułu i wyrażania opinii w dół poniżej, umożliwia zmiany do tego samego artykułu? Coś Brak? Problem? Pisanie jest coś, co jest po prostu mylące czy? Przewijanie w górę, a następnie kliknij przycisk **Edytuj na GitHub** , aby wprowadzić zmiany — te ułatwiających kontakt z nami do recenzji, a następnie po możemy zalogować się nad nimi, zostanie wyświetlony do zmiany i ulepszenia w tym artykule.