<properties
    pageTitle="Tworzenie zbioru hybrydowego dla Azure RemoteApp | Microsoft Azure"
    description="Dowiedz się, jak utworzyć wdrożeniu RemoteApp, który łączy do sieci wewnętrznej."
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

# <a name="how-to-create-a-hybrid-collection-for-azure-remoteapp"></a>Tworzenie zbioru hybrydowego dla Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp jest są przerywane. W tym artykule [anons](https://go.microsoft.com/fwlink/?linkid=821148) , aby uzyskać szczegółowe informacje.

Istnieją dwa typy Azure RemoteApp zbiorów:

- Chmury: znajduje się całkowicie Azure. Można zapisać wszystkie dane w chmurze (aby zbioru tylko do chmury) lub do połączenia kolekcji VNET i zapisywanie danych.   
- Hybrydowe: zawiera wirtualnej sieci dostępu lokalnego - wymaga użycia Azure AD oraz środowiska usługi Active Directory w lokalnej.

Nie wiesz, który jest wymagany? Zapoznaj się z [Jakiego rodzaju zbioru potrzeba dla Azure RemoteApp](remoteapp-collections.md).

Ten samouczek przeprowadzi Cię przez proces tworzenia zbioru hybrydowych. Istnieje osiem czynności:

1.  Określ, jakie [obraz](remoteapp-imageoptions.md) do użycia dla zbioru. Można utworzyć niestandardowy obraz lub użyć jednego z obrazów Microsoft zawartych w subskrypcji.
2. Konfigurowanie wirtualnych sieci. Zapoznaj się z informacjami [VNET planowania](remoteapp-planvnet.md) i [zmiany rozmiaru](remoteapp-vnetsizing.md) .
2.  Tworzenie zbioru.
2.  Dołączanie do kolekcji do domeny lokalnej.
3.  Dodaj obraz szablonu do kolekcji.
4.  Konfigurowanie synchronizacji katalogów. Funkcja RemoteApp platformy Azure wymaga integracji z usługą Azure Active Directory, albo 1) konfigurowania Azure Active synchronizacji katalogów z opcją synchronizacji haseł lub (2) konfigurowania Azure Active synchronizacji katalogów bez opcji synchronizacji haseł, ale przy użyciu domeny, która ma być federacyjne usług AD FS. Zapoznaj się z [informacje o konfiguracji dla z RemoteApp usługi Active Directory](remoteapp-ad.md).
5.  Publikowanie aplikacji RemoteApp.
6.  Konfigurowanie dostępu użytkowników.

**Przed rozpoczęciem**

Należy wykonać poniższe czynności przed utworzeniem kolekcji:

- [Utwórz konto](https://azure.microsoft.com/services/remoteapp/) w usłudze Azure RemoteApp.
- Tworzenie konta użytkownika w usłudze Active Directory ma zostać użyte jako konto usługi Azure RemoteApp. Ograniczyć uprawnienia dla tego konta, tak aby tylko można dołączyć komputerów do domeny.
- Zbieranie informacji o sieci lokalnej: adres IP, informacje i szczegóły urządzenia VPN.
- Zainstaluj moduł [Azure programu PowerShell](../powershell-install-configure.md) .
- Gromadzenie informacji o użytkownikach, których chcesz udzielić dostępu do. Konieczne będzie głównej nazwy użytkownika usługi Azure Active Directory (na przykład name@contoso.com) dla każdego użytkownika. Upewnij się, że UPN zgodne między Azure AD i usługi Active Directory.
- Wybierz obraz szablonu. Obraz szablonu Azure RemoteApp zawiera te aplikacje i programy, które chcesz opublikować dla użytkowników. Aby uzyskać więcej informacji, zobacz [Opcje obrazu Azure RemoteApp](remoteapp-imageoptions.md) .
- Chcesz korzystać z usługi Office 365 ProPlus obrazu? Zapoznaj się z informacje [poniżej](remoteapp-officesubscription.md).
- [Konfigurowanie usługi Active Directory dla RemoteApp](remoteapp-ad.md).



## <a name="step-1-set-up-your-virtual-network"></a>Krok 1: Konfigurowanie wirtualnych sieci
Można wdrażać zbioru hybrydowe, który używa istniejącego Azure wirtualnej sieci lub można utworzyć nowy wirtualnej sieci. Wirtualna sieć umożliwia danych programu access użytkownicy w Twojej sieci lokalnej przy użyciu programów RemoteApp zdalnego zasobów. Przy użyciu Azure wirtualną sieć zapewnia dostęp do sieci bezpośredni zbioru innych usług Azure i maszyn wirtualnych wdrożony wirtualnej sieci.

Upewnij się, że przed utworzeniem usługi VNET, zapoznaj się z informacjami [VNET planowania](remoteapp-planvnet.md) i [rozmiar VNET](remoteapp-vnetsizing.md) .

### <a name="create-an-azure-vnet-and-join-it-to-your-active-directory-deployment"></a>Tworzenie VNET Azure i dołączyć go do wdrożenia usługi Active Directory

Zacznij od tworzenie [wirtualnych sieci](../virtual-network/virtual-networks-create-vnet-arm-pportal.md). Można to zrobić na karcie **sieć** w portalu Azure. Potrzebujesz nawiązania połączenia wirtualnej sieci wdrożenia usługi Active Directory, która jest synchronizowana do Twojej dzierżawy usługi Azure Active Directory.

Aby uzyskać więcej informacji, zobacz [Tworzenie wirtualnych sieci przy użyciu Azure portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="make-sure-your-virtual-network-is-ready-for-azure-remoteapp"></a>Upewnij się, że wirtualnej sieci jest teraz gotowy do Azure RemoteApp
Przed utworzeniem kolekcji Przejdźmy upewnij się, czy nowy wirtualnej sieci jest gotowa. Można to sprawdzić, wykonując następujące czynności:

1. Tworzenie Azure maszyn wirtualnych wnętrza podsieci sieci wirtualnej właśnie utworzonego dla RemoteApp.
2. Nawiązywanie połączenia maszyn wirtualnych za pomocą pulpitu zdalnego. (Kliknij przycisk **Połącz**).
3. Dołączanie go do samej wdrożenia usługi Active Directory, który ma być używany dla programów RemoteApp.

Który pracował. Wirtualna sieć i podsieci są gotowe do Azure RemoteApp!

Można znaleźć więcej informacji na temat tworzenia Azure maszyn wirtualnych oraz łączenie ich z pulpitu zdalnego [tutaj](https://msdn.microsoft.com/library/azure/jj156003.aspx).

## <a name="step-2-create-an-azure-remoteapp-collection"></a>Krok 2: Tworzenie zbioru Azure RemoteApp ##



1. W [portalu Azure](http://manage.windowsazure.com)przejdź do strony Azure RemoteApp.
2. Kliknij pozycję **Nowy > Utwórz z VNET**.
3. Wprowadź nazwę dla zbioru.
4. Wybierz plan, że chcesz użyć — standardowy lub podstawowe.
5. Z listy rozwijanej lista, a następnie danej podsieci, wybierz pozycję usługi VNET.
6. Wybierz dołączyć go do domeny.
5. Kliknij pozycję **zbiór tworzenie RemoteApp**.

Po utworzeniu kolekcji Azure RemoteApp kliknij dwukrotnie nazwę kolekcji. Który powoduje wyświetlenie strony **Szybki Start** — jest to miejsce, w którym skonfigurowaniu kolekcji.

Czy coś pójść źle? Zapoznaj się z [informacje dotyczące rozwiązywania problemów zbioru hybrydowych](remoteapp-hybridtrouble.md).

## <a name="step-3-link-your-collection-to-the-local-domain"></a>Krok 3: Łączenie kolekcji do domeny lokalnej ##


1. Na stronie **Szybkie uruchamianie** kliknij pozycję **Dołącz do domeny lokalnej**.
2. Dodaj konto usługi Azure RemoteApp do domeny lokalnej usługi Active Directory. Konieczne będzie nazwy domeny, jednostka organizacyjna, nazwa użytkownika konta usługi i hasło.

    Jest to informacji zebranych, jeśli zostały wykonane kroki [Konfigurowania usługi Active Directory dla Azure RemoteApp](remoteapp-ad.md).


## <a name="step-4-link-to-an-azure-remoteapp-image"></a>Krok 4: Łącze do obrazu Azure RemoteApp ##

Obraz szablonu Azure RemoteApp zawiera programy, które chcesz udostępnić użytkownikom. Możesz utworzyć nowy [obraz szablonu](remoteapp-imageoptions.md) lub utworzyć łącze do istniejącego obrazu (jeden już zaimportowane lub przekazanie go do Azure RemoteApp). Można również łączyć do jednego z Azure RemoteApp [obrazów szablonów](remoteapp-images.md) zawierają usługi Office 365 lub programów pakietu Office 2013 (dla wersji próbnej).

Jeśli przesyłasz nowy obraz, musisz wprowadź nazwę i wybierz lokalizację dla obrazu. Na następnej stronie kreatora będzie dostępny zestaw poleceń cmdlet środowiska PowerShell — kopiowanie i uruchom następujące polecenia cmdlet z podwyższonym poziomem uprawnień wiersza programu Windows PowerShell, aby przekazać określonego obrazu.

Jeśli łączysz się z istniejącym obrazem szablonu, po prostu Określ nazwę obrazu, lokalizację i skojarzone Azure subskrypcji.



## <a name="step-5-configure-active-directory-directory-synchronization"></a>Krok 5: Konfigurowanie synchronizacji katalogów usługi Active Directory ##

Funkcja RemoteApp platformy Azure wymaga integracji z usługą Azure Active Directory, albo 1) konfigurowania Azure Active synchronizacji katalogów z opcją synchronizacji haseł lub (2) konfigurowania Azure Active synchronizacji katalogów bez opcji synchronizacji haseł, ale przy użyciu domeny, która ma być federacyjne usług AD FS.

Zapoznaj się z [AD Connect](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) — w tym artykule opisano konfigurowanie integracji katalogów w krokach 4.

W planowaniu informacje i uzyskać szczegółowe instrukcje, zobacz [Plan synchronizowania katalogu](http://msdn.microsoft.com//library/azure/hh967642.aspx) .

## <a name="step-6-publish-apps"></a>Krok 6: Publikowanie aplikacji ##

Aplikacja Azure RemoteApp jest aplikacji lub programu, w którym można udostępnić użytkownikom. Znajduje się w obraz szablonu, które zostały przekazane do kolekcji. Gdy użytkownik uzyskuje dostęp do aplikacji, wydaje się, aby uruchomić w środowisku lokalnym, ale naprawdę działa w Azure.

Zanim użytkownicy mogą uzyskać dostęp do aplikacji, musisz opublikować je — dzięki temu użytkownikom dostęp do aplikacji w klienta pulpitu zdalnego.

Możesz opublikować wiele aplikacji do kolekcji. Po wyświetleniu strony publikowania kliknij przycisk **Publikuj** , aby dodać aplikację. Albo można publikować w menu **Start** obraz szablonu lub określając ścieżkę na obraz szablonu dla aplikacji. Jeśli chcesz dodać do menu **Start** , wybierz program, aby dodać. Jeśli wybierzesz wprowadź ścieżkę do aplikacji, podaj nazwę dla tej aplikacji i ścieżkę do miejsca na obraz szablonu, w którym jest zainstalowany.

## <a name="step-7-configure-user-access"></a>Krok 7: Konfigurowanie dostępu użytkowników ##

Teraz, gdy użytkownik utworzył kolekcji, musisz dodać użytkowników, których chcesz korzystać z zasobów zdalnych. Użytkownicy podane dostęp do muszą istnieć w dzierżawie usługi Active Directory skojarzone z subskrypcją, możesz służy do tworzenia tego Azure RemoteApp zbioru.

1.  Na stronie Szybkie uruchamianie kliknij pozycję **Konfigurowanie dostępu użytkowników**.
2.  Wprowadź konta służbowego (z usługi Active Directory) lub konta Microsoft, którego chcesz udzielić dostępu.

    **Uwagi:**

    Upewnij się, że używasz “user@domain.com” format.

    Jeśli korzystasz z usługi Office 365 ProPlus w kolekcji, należy użyć tożsamości usługi Active Directory dla użytkowników. Pomaga to sprawdzić poprawność licencjonowania.


3.  Po zatwierdzeniu użytkowników kliknij przycisk **Zapisz**.


## <a name="next-steps"></a>Następne kroki ##
To wszystko — pomyślnie utworzone i wdrożone kolekcji hybrydowych Azure RemoteApp. Następnym krokiem jest mają użytkownikom pobieranie i instalowanie klienta pulpitu zdalnego. Adres URL można znaleźć klienta na stronie Azure RemoteApp Szybki Start. Następnie dziennik użytkowników do klienta i uzyskać dostęp do aplikacji, które zostały opublikowane.



### <a name="help-us-help-you"></a>Pomóż nam ułatwiają
Czy wiesz, oprócz ocena niniejszego artykułu i wyrażania opinii w dół poniżej, umożliwia zmiany do tego samego artykułu? Czegoś brakuje? Problem? Pisanie jest coś, co jest po prostu mylące czy? Przewijanie w górę, a następnie kliknij przycisk **Edytuj na GitHub** , aby wprowadzić zmiany — te ułatwiających kontakt z nami do recenzji, a następnie po możemy zalogować się nad nimi, zostanie wyświetlony do zmiany i ulepszenia w tym artykule.
