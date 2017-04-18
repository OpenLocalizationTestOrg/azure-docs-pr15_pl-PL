<properties
    pageTitle="Narzędzie Azure AD Connect: Instalacja niestandardowa | Microsoft Azure"
    description="Ten szczegółowo opisano opcje instalacji niestandardowej Azure AD Connect. Należy zainstalować usługi Active Directory za pośrednictwem Azure AD Connect te instrukcje."
    services="active-directory"
    keywords="Co to jest Azure AD Connect, zainstaluj usługi Active Directory, składniki wymagane w przypadku Azure AD"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="custom-installation-of-azure-ad-connect"></a>Instalacja niestandardowa: narzędzie Azure AD Connect
Azure AD Connect **niestandardowych ustawień** jest używana, gdy chcesz dodatkowe opcje instalacji. Jeśli masz wiele lasów lub jeśli chcesz skonfigurować opcjonalne funkcje nie są objęte Instalacja ekspresowa są one używane. Jest on używany we wszystkich przypadkach, gdzie opcja [**Instalacja ekspresowa**](active-directory-aadconnect-get-started-express.md) nie spełnia swojego wdrożenia lub topologii.

Przed rozpoczęciem instalacji narzędzie Azure AD Connect, upewnij się, aby [pobrać narzędzie Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) i wykonano kroki warunki wstępne [Narzędzie Azure AD Connect: sprzęt i wymagania wstępne dotyczące](../active-directory-aadconnect-prerequisites.md). Upewnij się, że trzeba konta dostępne, zgodnie z opisem w [Narzędzie Azure AD Connect kontach i uprawnieniach](active-directory-aadconnect-accounts-permissions.md)również.

Jeśli niestandardowych ustawień nie odpowiada topologii, na przykład uaktualniania DirSync, zobacz [dotyczące dokumentacji](#related-documentation) dla innych sytuacjach.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Niestandardowe ustawienia instalacji narzędzie Azure AD Connect

### <a name="express-settings"></a>Ustawienia ekspresowe
Na tej stronie kliknij przycisk **Dostosuj** , aby rozpocząć instalację ustawień niestandardowych.

### <a name="install-required-components"></a>Zainstaluj wymagane składniki
Po zainstalowaniu usługi synchronizacji, możesz pozostawić w sekcji opcjonalnym zaznaczona pozycja i narzędzie Azure AD Connect konfiguruje wszystko automatycznie. Konfiguruje wystąpienie programu SQL Server 2012 Express LocalDB, tworzenie odpowiednich grup i przypisać uprawnienia. Jeśli chcesz zmienić ustawienia domyślne, możesz opcji opcjonalnym, które są dostępne za pomocą poniższej tabeli.

![Wymagane składniki](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

Opcjonalna konfiguracja  | Opis
------------- | -------------
Używanie istniejącego programu SQL Server | Pozwala określić nazwę serwera SQL oraz nazwę wystąpienia. Wybierz tę opcję, jeśli masz już z serwerem bazy danych, którego chcesz użyć. Wprowadź nazwę wystąpienia następuje przecinek i numer portu w polu **Nazwa wystąpienia** , jeśli program SQL Server nie jest włączone przeglądanie.
Użyj istniejącego konta usługi | Domyślnie narzędzie Azure AD Connect tworzy konto usługi lokalnej do za pomocą usług synchronizacji. Hasło jest automatycznie wygenerowane i nieznanych osobie instalowania Azure AD Connect. Jeśli za pomocą zdalnego programu SQL server lub Użyj serwera proxy, który wymaga uwierzytelniania, potrzebna jest usługa kont w domenie i prawidłowego hasła. W tych przypadkach wprowadź nazwę konta usługi, aby użyć. Upewnij się, że użytkownik uruchamiający instalacji jest Skojarzenie w języku SQL, więc można utworzyć identyfikator logowania do konta usługi. Zobacz [Narzędzie Azure AD Connect kontach i uprawnieniach](active-directory-aadconnect-accounts-permissions.md#custom-settings-installation)
Określ grupy niestandardowej synchronizacji | Domyślnie narzędzie Azure AD Connect tworzy cztery grupy lokalne na serwerze są zainstalowane usługi synchronizacji. Są następujące grupy: Administratorzy, Operatorzy, Przeglądaj grupy i grupa resetowanie haseł. Możesz określić własne grupy tutaj. Grupy musi być lokalna na serwerze i nie może znajdować się w tej domenie.

### <a name="user-sign-in"></a>Logowania użytkownika
Po zainstalowaniu wymaganych składników, zostanie wyświetlony monit o Wybieranie użytkowników pojedynczego logowania jednokrotnego metody. Poniższa tabela zawiera krótki opis dostępnych opcji. Aby uzyskać pełny opis metody logowania zobacz [logowania użytkownika](../active-directory-aadconnect-user-signin.md).

![Logowania użytkownika](./media/active-directory-aadconnect-get-started-custom/usersignin.png)

Opcja rejestracji jednokrotnej | Opis
------------- | -------------
Synchronizacji haseł | Użytkownicy będą mogli logować się do programu Microsoft cloud services, takiego jak usługi Office 365, przy użyciu tego samego hasła, używanych w sieci lokalnej. Hasła użytkowników są synchronizowane z Azure AD jako skrótu hasła i uwierzytelnianie odbywa się w chmurze. Aby uzyskać więcej informacji, zobacz [synchronizacji haseł](../active-directory-aadconnectsync-implement-password-synchronization.md) .
Federacji z usług AD FS | Użytkownicy będą mogli logować się do programu Microsoft cloud services, takiego jak usługi Office 365, przy użyciu tego samego hasła, używanych w sieci lokalnej.  Użytkownicy są przekierowywani do ich lokalnych usług AD FS wystąpienia się zalogować i uwierzytelnianie odbywa się w lokalnej.
Nieskonfigurowane | Ani funkcja jest zainstalowana i skonfigurowana. Wybierz tę opcję, jeśli masz już 3 serwer federacyjny innych firm lub innego istniejącego rozwiązania w miejscu.

### <a name="connect-to-azure-ad"></a>Nawiązywanie połączenia z Azure AD
Na stronie Połącz Azure AD ekranu wprowadź nazwę i hasło konta administratora globalnego. W przypadku wybrania **federacji z usług AD FS** na poprzedniej stronie nie Zaloguj się przy użyciu konta z domeny, które chcesz włączyć do Federacji. Zaleca się korzystanie z konta w domenie **onmicrosoft.com** domyślnej, która jest zawarty w katalogu Azure AD.

To konto jest używane jedynie w celu utworzenia konta usługi w Azure AD i nie jest używana po zakończeniu działania kreatora.  
![Logowania użytkownika](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Jeśli Twoje konto administratora globalnego ma MFA włączone, należy wprowadź hasło ponownie w menu podręczne logowania i wypełnij największym wyzwaniem MFA. Największym wyzwaniem może być, dostarczając kod weryfikacyjny lub rozmowy telefonicznej.  
![Logowania użytkownika MFA](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

Konta administratora globalnego mogą mieć także [Uprzywilejowanych Zarządzanie tożsamościami](../active-directory-privileged-identity-management-getting-started.md) włączone.

Jeśli komunikat o błędzie i występują problemy z łącznością, zobacz sekcję [Rozwiązywanie problemów z łącznością](../active-directory-aadconnect-troubleshoot-connectivity.md).

## <a name="pages-under-the-section-sync"></a>Stron w sekcji synchronizacji

### <a name="connect-your-directories"></a>Nawiązywanie połączenia z katalogów
Aby połączyć się z usługi domeny usługi Active Directory, narzędzie Azure AD Connect wymaga poświadczeń konta z odpowiednimi uprawnieniami. Możesz wprowadzić domeny w formacie NetBios lub nazwy FQDN, oznacza to, że FABRIKAM\syncuser lub fabrikam.com\syncuser. To konto może być konto zwykłego użytkownika, ponieważ musi tylko domyślne uprawnienia do odczytu. Jednak w zależności od aktualnego scenariusza, może być konieczne więcej uprawnień. Aby uzyskać więcej informacji zobacz [Azure AD łączenie kontach i uprawnieniach](../active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account)

![Łączenie katalogu](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### <a name="azure-ad-sign-in-configuration"></a>Azure AD logowania konfiguracji
Ta strona umożliwia przeglądanie domeny głównej nazwy użytkownika w lokalnym zweryfikowane usług AD DS, które w Azure AD. Ta strona umożliwia również skonfigurować atrybut służących do userPrincipalName.

![Niezweryfikowany domeny](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Przejrzyj każda domena oznaczone **Nie zostały dodane** i **Nie został zweryfikowany**. Upewnij się, że w Azure AD zweryfikowane tych domen, którego używasz. Po zweryfikowaniu domeny usługi, kliknij symbol odświeżania. Aby uzyskać więcej informacji zobacz [dodać i zweryfikować domenę](../active-directory-add-domain.md)

**UserPrincipalName** - atrybut userPrincipalName jest użytkownicy atrybut używać podczas logowania się w do Azure AD i usługi Office 365. Domeny używane, nazywane także sufiksu głównej nazwy użytkownika —, należy zweryfikować w Azure AD przed użytkowników są synchronizowane. Firma Microsoft zaleca, aby zachować userPrincipalName atrybut domyślne. Jeśli ten atrybut jest bez obsługi routingu i nie może zostać zweryfikowane, będzie można wybrać inny atrybut. Na przykład można wybrać wiadomości e-mail jako atrybut, przytrzymując identyfikator logowania. Przy użyciu atrybutu innego niż userPrincipalName nosi nazwę **Alternatywny identyfikator**. Wartość atrybutu alternatywny identyfikator muszą być zgodne ze standardem RFC822. Alternatywny identyfikator można używać z synchronizacji haseł i federacji.

>[AZURE.WARNING]
Za pomocą alternatywny identyfikator nie jest zgodny z wszystkich obciążeń pracą usługi Office 365. Aby uzyskać więcej informacji odwołują się do [Konfigurowania alternatywny identyfikator logowania](https://technet.microsoft.com/library/dn659436.aspx).

### <a name="domain-and-ou-filtering"></a>Domeny i OU filtrowania
Domyślnie wszystkie domeny i organizacyjnych są synchronizowane. W przypadku niektórych domen lub organizacyjnych, czy chcesz zsynchronizować Azure AD, możesz usunąć zaznaczenie tych domen i jednostek organizacyjnych.  
![Filtrowanie DomainOU](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png) ta strona w kreatorze jest konfiguracji domeny filtrowania. Aby uzyskać więcej informacji zobacz [opartych na domenie filtrowanie](../active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering).

Istnieje także możliwość, że niektóre domeny są niedostępne z powodu ograniczeń zapory. Następujące domeny są niezaznaczone domyślnie i że masz ostrzeżenie.  
![Domeny jest niedostępny](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Jeśli to ostrzeżenie jest wyświetlane, upewnij się, że te domeny są w rzeczywistości jest nieosiągalny i oczekuje ostrzeżenie.

### <a name="uniquely-identifying-your-users"></a>Jednoznacznie identyfikujące użytkowników
Pasujące przez funkcję lasami pozwala na zdefiniowanie, jak użytkowników z usługi lasy usług AD DS są przedstawione w Azure AD. Użytkownik może albo być reprezentowany tylko raz w lasach wszystkich lub mieć Kombinacja konta włączono i wyłączone. Użytkownik może być również reprezentowany jako kontakt w niektórych lasach.

![Unikatowe](./media/active-directory-aadconnect-get-started-custom/unique.png)

Ustawienie | Opis
------------- | -------------
[Użytkownicy tylko są reprezentowane raz w lasach wszystkich](../active-directory-aadconnect-topologies.md#multiple-forests-separate-topologies) | Wszyscy użytkownicy są tworzone jako pojedyncze obiekty w Azure AD. Obiekty nie są połączone w metaverse.
[Atrybut poczty](../active-directory-aadconnect-topologies.md#multiple-forests-full-mesh-with-optional-galsync) | Ta opcja łączy użytkowników i kontaktów, jeśli ten atrybut poczty ma tę samą wartość w różnych lasach. Użyj tej opcji, gdy kontakty zostały utworzone za pomocą GALSync.
[ObjectSID i msExchangeMasterAccountSID-msRTCSIP-OriginatorSid](../active-directory-aadconnect-topologies.md#multiple-forests-account-resource-forest) | Ta opcja łączy użytkownik włączone w las konta z tym użytkownikiem wyłączone w las zasobów. W programie Exchange taka konfiguracja nosi nazwę skrzynki pocztowej połączonego. Również można tę opcję, jeśli używasz tylko programu Lync i programu Exchange nie znajduje się w las zasobów.
atrybuty sAMAccountName i MailNickName | Ta opcja łączy atrybutów miejsce, w którym oczekuje się, że identyfikator logowania użytkownika znajduje się.
Określony atrybut | Ta opcja umożliwia wybierz własną atrybut. **Ograniczenie:** Upewnij się wybrać atrybut, który już znajdują się w metaverse. Jeśli wybierzesz atrybutu niestandardowego (nie w metaverse), nie można ukończyć kreatora.

**Źródło kotwicy** - sourceAnchor atrybut jest atrybut, który jest niezmienny czasie trwania obiekt użytkownika. Jest kluczem podstawowym łączenia użytkownika lokalnego o w Azure AD. Ponieważ nie można zmienić atrybutu, należy zaplanować atrybutu warto używać. Aby pole nadawało jest objectGUID. Ten atrybut nie ulega zmianie, chyba że konto użytkownika jest przenoszony między lasami i domenami. W środowisku wielu las miejsce, w którym przenosić konta między lasami innego należy użyć atrybutu, takich jak atrybut o pole Identyfikator pracownika. Unikaj atrybuty, które chcesz zmienić po posiadającego osoby lub przydziałów. Nie można używać atrybutów @-sign, , wiadomości e-mail i userPrincipalName nie mogą być używane. Atrybut również jest uwzględniana wielkość liter tak po przeniesieniu obiektu między lasami, upewnij się, że zachowanie górnym i małe litery. Binarny atrybuty są kodowane base64, ale inne typy atrybut pozostają w stanie kodowanej. Scenariusze Federacji i niektóre interfejsy Azure AD dany atrybut jest nazywany immutableID. Więcej informacji na temat kotwicy źródła można znaleźć w [pojęć dotyczących projektowania](../active-directory-aadconnect-design-concepts.md#sourceAnchor).

### <a name="sync-filtering-based-on-groups"></a>Filtrowanie synchronizacji na podstawie grup
Filtrowanie funkcję grup umożliwia synchronizowanie mały podzbiór obiektów dla wdrożenia pilotażowego. Aby użyć tej funkcji, należy utworzyć grupę, w tym celu w usłudze Active Directory w lokalnej. Następnie dodaj użytkowników i grup, które mają być synchronizowane do Azure AD jako członków bezpośredniego. Możesz później dodawać i usuwać użytkowników do tej grupy, aby zachować na liście obiektów, które powinny znajdować się w Azure AD. Wszystkie obiekty, które chcesz zsynchronizować musi być bezpośredni członkiem grupy. Użytkownicy, grupy, kontakty i komputerach i urządzeniach wszystkie muszą być członkami bezpośredniego. Nie można rozpoznać zagnieżdżonych członkostwo w grupach. Po dodaniu grupy po dodaniu członka, tylko grupy, a nie jej elementów.

![Filtrowanie synchronizacji](./media/active-directory-aadconnect-get-started-custom/filter2.png)

>[AZURE.WARNING]
Ta funkcja jest przeznaczony tylko do obsługi wdrożenia pilotażowego. Nie należy używać go we wdrożeniu full-blown produkcji.

We wdrożeniu full-blown produkcji ma być trudne do obsługi jednej grupy ze wszystkimi obiektami do synchronizacji. Zamiast tego warto z nich korzystać z metod w [konfiguracji filtrowania](../active-directory-aadconnectsync-configure-filtering.md).

### <a name="optional-features"></a>Opcjonalne funkcje
Ten ekran umożliwia wybierz opcjonalne funkcje dla swojego określonych scenariuszach.

![Opcjonalne funkcje](./media/active-directory-aadconnect-get-started-custom/optional.png)

>[AZURE.WARNING]
Jeśli masz aktualnie DirSync lub aktywne Azure AD Sync, nie zostanie aktywowany funkcji zapisu w narzędzie Azure AD Connect.

Opcjonalne funkcje | Opis
------------------- | -------------
Wdrożenie hybrydowe programu Exchange | Funkcja wdrożenie hybrydowe programu Exchange umożliwia współistnienie skrzynki pocztowe programu Exchange zarówno w lokalnej i w usłudze Office 365. Narzędzie Azure AD Connect synchronizuje określonego zestawu [atrybutów](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) Azure AD do katalogu lokalnego.
Azure AD aplikacji i atrybut filtrowania | Po włączeniu Azure AD aplikacji i filtrowania atrybut, można dostosować do zestaw atrybutów zsynchronizowane. Ta opcja dodaje dwie strony konfiguracji więcej do kreatora. Aby uzyskać więcej informacji zobacz [Azure AD aplikacji i atrybut filtrowania](#azure-ad-app-and-attribute-filtering).
Synchronizacja haseł | Jeśli wybrano federacyjne rozwiązanie logowania, można włączyć tę opcję. Następnie można synchronizacji haseł jako opcja kopii zapasowej. Aby uzyskać dodatkowe informacje zobacz [synchronizacji haseł](../active-directory-aadconnectsync-implement-password-synchronization.md).
Hasło wystornowaniem | Po włączeniu zapisu hasła, zmiany hasła, które pochodzą z Azure AD jest zapisywane w katalogu lokalnego. Aby uzyskać więcej informacji zobacz [Wprowadzenie do zarządzania hasłami](../active-directory-passwords-getting-started.md).
Grupa wystornowaniem | Jeśli funkcja **Grupami usługi Office 365** , może mieć następujące grupy przedstawione w usłudze Active Directory w lokalnej. Ta opcja jest dostępna tylko jeśli masz Exchange w usłudze Active Directory w lokalnej. Aby uzyskać więcej informacji zobacz [grupy wystornowania](../active-directory-aadconnect-feature-preview.md#group-writeback).
Urządzenie wystornowaniem | Umożliwia zapisu urządzenia obiektów w Azure AD do usługi Active Directory w lokalnej w scenariuszach dostępu warunkowego. Aby uzyskać więcej informacji zobacz [Włączanie urządzenia zapisu w narzędzie Azure AD Connect](../active-directory-aadconnect-feature-device-writeback.md).
Synchronizacja atrybut rozszerzenie katalogów | Po włączeniu synchronizacji atrybut rozszerzenia katalogów, atrybutów określonych są synchronizowane z Azure AD. Aby uzyskać więcej informacji zobacz [rozszerzenia katalogu](../active-directory-aadconnectsync-feature-directory-extensions.md).

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure AD aplikacji i atrybut filtrowania
Jeśli chcesz ograniczyć atrybuty być synchronizowane z Azure AD, uruchomić, wybierając usługach, których używasz. Po wprowadzeniu zmian w konfiguracji na tej stronie nowej usługi musi być zaznaczone jawnie uruchamiając Kreatora instalacji.

![Opcjonalne funkcje aplikacji](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

W zależności od usług wybrany w poprzednim kroku, ta strona zawiera wszystkie atrybuty, które są synchronizowane. Ta lista jest kombinacją wszystkie typy obiektów, które są synchronizowane. W przypadku niektórych określonych atrybutów, które trzeba nie są synchronizowane, możesz usunąć zaznaczenie tych atrybutów.

![Opcjonalne funkcje atrybutów](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

>[AZURE.WARNING]
Usuwanie atrybutów może mieć wpływ na funkcje. Najważniejsze wskazówki dotyczące i zalecenia zobacz [atrybuty synchronizowane](../active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).

### <a name="directory-extension-attribute-sync"></a>Synchronizacja atrybut rozszerzenie katalogów
W przypadku rozszerzenia schematu w Azure AD z atrybutami niestandardowymi dodane przez organizacji lub inne atrybuty w usłudze Active Directory. Aby użyć tej funkcji, wybierz pozycję **Synchronizuj atrybut rozszerzenie katalogu** na stronie **Funkcje opcjonalne** . Możesz zaznaczyć więcej atrybutów zsynchronizować na tej stronie.

![Rozszerzenia katalogu](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Aby uzyskać więcej informacji zobacz [rozszerzenia katalogu](../active-directory-aadconnectsync-feature-directory-extensions.md).

## <a name="configuring-federation-with-ad-fs"></a>Konfigurowanie Federacji za pomocą usług AD FS
Konfigurowanie usług AD FS za pomocą narzędzie Azure AD Connect jest proste — wystarczy tylko kilka kliknięć. Poniższy jest wymagane przed konfiguracji.

- Serwer Windows Server 2012 R2 dla serwera federacyjnego z zarządzania zdalnego włączone
- Serwer Windows Server 2012 R2 serwera Proxy aplikacji sieci Web przy użyciu funkcji zdalnego zarządzania włączone
- Certyfikat SSL dla nazwy usługi federacyjne zamierzasz użyć (na przykład sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>Wymagania wstępne AD FS konfiguracji
Aby skonfigurować farmie usług AD FS przy użyciu Azure AD Connect, upewnij się, że program WinRM jest włączona na serwerach zdalnych. Ponadto przejść przez wymaganie porty wymienionych w [tabeli 3 — narzędzie Azure AD Connect i federacyjnych serwerów-WAP](../active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-federation-serverswap).

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Tworzenie nowej farmy serwerów usług AD FS lub użyj istniejącej farmy usług AD FS
Można użyć istniejącej farmy usług AD FS lub można utworzyć nowej farmy serwerów usług AD FS. Jeśli chcesz utworzyć nową, należy podać certyfikatu SSL. Jeśli certyfikat SSL jest chroniony hasłem, zostanie wyświetlony monit o podanie hasła.

![AD FS farmy](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Jeśli zdecydujesz się na użycie istniejącej farmy usług AD FS, są pobierane bezpośrednio do konfigurowania relacji zaufania między usług AD FS i Azure AD ekranu.

### <a name="specify-the-ad-fs-servers"></a>Określ serwer usług AD FS
Wprowadź serwerów, które chcesz zainstalować usług AD FS na. Możesz dodać jeden lub więcej serwerów oparte na wydajność Planowanie wymagań. Dołączanie do wszystkich serwerów usługi Active Directory przed wykonaniem tej konfiguracji. Firma Microsoft zaleca instalowanie jeden serwer usług AD FS wdrożeniach badania i pilotażowego. Następnie dodaj i wdrażanie większej liczby serwerów do własnych potrzeb skalowania, uruchamiając narzędzie Azure AD Connect ponownie po początkowej konfiguracji.

>[AZURE.NOTE]
Upewnij się, że wszystkie serwery są połączone z domeną AD przed wykonaniem tej konfiguracji.

![Serwery AD FS](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>Określanie serwerów Proxy aplikacji sieci Web
Wprowadź serwerów, które ma być serwerów proxy aplikacji sieci Web. Serwer proxy aplikacji sieci web jest wdrożona w Twojej strefy Zdemilitaryzowanej (widocznych dla ekstranetu) i obsługuje żądania uwierzytelniania z ekstranetu. Możesz dodać jeden lub więcej serwerów oparte na wydajność Planowanie wymagań. Firma Microsoft zaleca instalowanie pojedynczego serwera proxy aplikacji sieci Web w przypadku wdrożeń badania i pilotażowego. Następnie dodaj i wdrażanie większej liczby serwerów do własnych potrzeb skalowania, uruchamiając narzędzie Azure AD Connect ponownie po początkowej konfiguracji. Zaleca się o równoważnej liczby serwerów proxy spełnienia wymagań uwierzytelniania z intranetu.

>[AZURE.NOTE]
<li> Jeśli konto, którego używasz nie jest administratorem lokalnym na serwerach usług AD FS, następnie zostanie wyświetlony monit o poświadczenia administratora.</li>
<li> Upewnij się, że jest łączność protokołu HTTP/HTTPS na serwerze narzędzie Azure AD Connect i serwer Proxy aplikacji sieci Web przed uruchomieniem tego kroku.</li>
<li> Upewnij się, że jest protokołu HTTP/HTTPS łączność między serwerze aplikacji sieci Web i serwer usług AD FS, aby umożliwić żądania uwierzytelniania przepływał przez.</li>

![W przeglądarce](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Zostanie wyświetlony monit o podanie poświadczeń, tak aby serwerze aplikacji sieci web można ustanowić bezpiecznego połączenia z serwerem usług AD FS. Te poświadczenia muszą być administratorem lokalnym na serwerze usług AD FS.

![Serwer proxy](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>Określanie konta usługi dla usług AD FS
Usługa usług AD FS wymaga konta usługi domeny do uwierzytelniania użytkowników i informacje o użytkowniku wyszukiwania w usłudze Active Directory. Obsługuje dwa typy kont usługi:

- **Konto usługi zarządzania grupą** - wprowadzone w usługach domenowych Active Directory w systemie Windows Server 2012. Ten typ konta zapewnia usługi, takie jak AD FS, jednego konta bez konieczności regularne aktualizowanie hasła do konta. Użyj tej opcji, jeśli masz już systemu Windows Server 2012 kontrolery w domenie, które należy serwery usług AD FS.
- **Konto użytkownika domeny** — konta tego typu wymaga o podanie hasła i regularnie aktualizować hasło, gdy hasło zmienione lub wygaśnie. Użyj tej opcji tylko wtedy, gdy nie masz systemu Windows Server 2012 kontrolery domeny, którą należy serwery usług AD FS.

Jeśli wybrano konto usługi zarządzanych grupy i ta funkcja nigdy nie został użyty w usłudze Active Directory, zostanie wyświetlony monit o poświadczenia administratora przedsiębiorstwa. Te poświadczenia są używane do inicjowania magazynie kluczy i włączyć funkcję w usłudze Active Directory.

![Konto usługi usług AD FS](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Wybierz domenę Azure AD, który chcesz utworzyć Federację
Ta konfiguracja jest używany do konfiguracji federacyjnego powiązanie AD FS i Azure AD. Konfiguruje usług AD FS do tokenów zabezpieczających problem do Azure AD, a konfiguruje Azure AD za zaufany tokenów z tego konkretnego wystąpienia usług AD FS. Ta strona umożliwia tylko skonfigurować pojedynczą domenę w początkowej instalacji. Więcej domen można skonfigurować później, uruchamiając narzędzie Azure AD Connect.

![Azure AD domeny](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>Zweryfikuj domenę Azure AD zaznaczone do Federacji
Po wybraniu domeny, która ma być Federacji Azure AD Connect zawiera informacje potrzebne do zweryfikowania domeny niezweryfikowany. Zobacz [Dodaj i sprawdź, czy domeny](../active-directory-add-domain.md) jak skorzystać z tych informacji.

![Azure AD domeny](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

>[AZURE.NOTE]
AD Connect próbuje zweryfikuj domenę na etapie Konfiguruj. Kontynuowanie konfigurowania bez dodawania rekordy DNS wymagane Kreator nie jest możliwe zakończyć konfigurację.

## <a name="configure-and-verify-pages"></a>Konfigurowanie i sprawdź, czy stron
Konfiguracja odbywa się na tej stronie.

>[AZURE.NOTE]
Przed kontynuowaniem instalacji i skonfigurowano Federacji, upewnij się, skonfigurowano [Rozpoznawanie nazw dla serwerów federacyjnych](../active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers).

![Gotowy do skonfigurowania](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Tryb tymczasowego
Użytkownik może skonfigurować nowego serwera synchronizacji równolegle z tymczasowej tryb. Jest obsługiwana tylko jeden serwer synchronizacji eksportowania do jednego katalogu w chmurze. Ale jeśli chcesz przenieść z innego serwera, na przykład jeden DirSync uruchomiony, można włączyć narzędzie Azure AD Connect w trybie tymczasowej. Po włączeniu aparat synchronizacji importowanie i synchronizowanie danych w zwykły sposób, ale go nie eksportuje cokolwiek Azure AD lub AD. Hasło funkcje zapisu synchronizacji i hasło są wyłączone podczas pracy w trybie przemieszczenia.

![Tryb tymczasowego](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

W trybie tymczasowy jest możliwe wprowadź wymagane zmiany do aparatu synchronizacji i przeglądanie, co ma być eksportowane. Podczas konfiguracji odpowiada Twoim potrzebom, ponownie uruchom Kreatora instalacji i wyłączanie trybu tymczasowy. Dane są teraz eksportowane do Azure AD z tego serwera. Upewnij się wyłączyć innego serwera w tym samym czasie, dlatego tylko jeden serwer aktywnie jest eksportowany.

Aby uzyskać więcej informacji zobacz [Organizowanie tryb](../active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Sprawdź konfigurację Federacji
Narzędzie Azure AD Connect sprawdza ustawień DNS, po kliknięciu przycisku Weryfikuj.

![Kończenie](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Sprawdź](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Ponadto wykonaj następujące czynności weryfikacji:

- Sprawdź, czy możesz się zalogować przy użyciu przeglądarki na komputerze sprzężonych domeny w intranecie: nawiązywanie połączenia z https://myapps.microsoft.com i sprawdź, czy logowanie przy użyciu konta zalogowany. Wbudowane konto administratora usług AD DS nie jest synchronizowany i nie można używać w celu weryfikacji.
- Sprawdź, czy możesz się zalogować za pomocą urządzenia z ekstranetu. Na głównym komputera lub urządzenia przenośnego nawiązywanie połączenia z https://myapps.microsoft.com i podać poświadczenia.
- Sprawdź poprawność logowania klienta wzbogaconego. Nawiązywanie połączenia z https://testconnectivity.microsoft.com, wybierz kartę **usługi Office 365** i wybierz pozycję **Office 365 pojedynczego logowania jednokrotnego badania**.

## <a name="next-steps"></a>Następne kroki
Po zakończeniu instalacji, wyloguj się, a następnie ponownie zaloguj się do systemu Windows przed użyciem Menedżera usługi synchronizacji lub Edytor reguł synchronizacji.

Teraz, gdy masz Azure AD Connect możesz [sprawdzić poprawność instalacji i przypisywanie licencji](../active-directory-aadconnect-whats-next.md).

Dowiedz się więcej o tych funkcjach, które zostały włączone z instalacją: [Azure AD łączenie zdrowia](../active-directory-aadconnect-health-sync.md)i [Zapobiegaj przypadkowym usunięciom](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) .

Dowiedz się więcej o tych typowych tematów: [Harmonogram i jak wyzwalanie synchronizacji](../active-directory-aadconnectsync-feature-scheduler.md).

Dowiedz się więcej o [integrowaniu lokalnego tożsamości z usługą Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Dokumentacja pokrewne

Temat |  
--------- | ---------
Omówienie Azure AD Connect | [Integrowanie tożsamości lokalnych z usługi Azure Active Directory](../active-directory-aadconnect.md)
Instalowanie przy użyciu ustawień Express | [Instalacja ekspresowa Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Uaktualnienie DirSync | [Uaktualnienie narzędzie do synchronizacji usługi Azure AD (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Konta używane do instalacji | [Więcej informacji na temat Azure AD Connect kontach i uprawnieniach](active-directory-aadconnect-accounts-permissions.md)
