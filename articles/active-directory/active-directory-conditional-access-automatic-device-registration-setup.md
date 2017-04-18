<properties
    pageTitle="Konfigurowanie automatycznej rejestracji domeny urządzenia z systemem Windows z usługi Azure Active Directory | Microsoft Azure"
    description="Konfigurowanie urządzenia z systemem Windows z domeny zarejestrować automatycznie i w trybie cichym za pomocą usługi Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="markvi"/>



# <a name="set-up-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Konfigurowanie automatycznej rejestracji systemu Windows urządzeń domeny z usługi Azure Active Directory

Aby użyć [usługi Azure Active Directory oparte na urządzeniu dostępie warunkowym](active-directory-conditional-access.md), komputery domeny systemu Windows musi być zarejestrowany z usługą Azure Active Directory (Azure AD). W tym artykule przedstawiono co należy zrobić, aby skonfigurować rejestracji domeny urządzenia z systemem Windows z usługą Azure Active Directory w Twojej organizacji.

Używanie programu access warunkowe w Azure AD zapewnia następujące zalety:

-   Środowisko rozszerzonego logowania jednokrotnego (SSO) w aplikacjach Azure AD za pomocą konta służbowego
-   Przedsiębiorstwo mobilnego ustawień na urządzeniach
-   Dostęp do Sklepu Windows dla firm
-   Uwierzytelnianie silniejszego i wygodny Zaloguj się przy użyciu Witaj systemu Windows

> [AZURE.NOTE] Aktualizacja dla systemu Windows 10 listopada udostępnia niektóre środowiska użytkownika w Azure AD, ale aktualizacji dla systemu Windows 10 rocznicy obsługuje w pełni oparte na urządzeniu dostępie warunkowym. Aby uzyskać więcej informacji o dostępie warunkowym zobacz [usługi Azure Active Directory oparte na urządzeniu dostępie warunkowym](active-directory-conditional-access.md). Aby uzyskać więcej informacji na temat urządzeń systemu Windows 10 w miejscu pracy i jak użytkownik rejestruje urządzeniu z systemem Windows 10 z usługą Azure Active Directory, zobacz [systemu Windows 10 w przedsiębiorstwie: pracy za pomocą urządzeń](active-directory-azureadjoin-windows10-devices-overview.md).

Można zarejestrować niektóre starsze wersje systemu Windows, w tym następujących wersji:

-   System Windows 8.1
-   Windows 7

Jeśli korzystasz z komputera systemu Windows Server jako pulpit, można zarejestrować tych platform:

- System Windows Server 2016
- Windows Server 2012 R2
- System Windows Server 2012
- Windows Server 2008 R2


## <a name="prerequisites"></a>Wymagania wstępne

Główne wymagania automatycznej rejestracji urządzeń domeny przy użyciu Azure AD ma aktualną wersję programu Azure Active Directory Connect (Azure AD Connect).

W zależności od sposobu wdrożony Azure AD Connect i używane Instalacja ekspresowa lub niestandardowej lub uaktualnienie w miejscu następujące wymagania wstępne mogły zostać skonfigurowane automatycznie:

-   **Punkt połączenia usługi w lokalnej usługi Active Directory**. Dla odnajdowania Azure AD dzierżawy informacji przez komputery tego rejestru dla Azure AD.

-   **Active Directory Federation Services (AD FS) emisji Przekształć reguły**. Uwierzytelniania komputera rejestracji (dotyczy konfiguracje federacyjnych).

W przypadku niektórych urządzeń w Twojej organizacji nie urządzeń z systemem Windows 10 domeny, upewnij się, że możesz wykonać następujące zadania:

- Ustawianie zasad w Azure AD, tak aby użytkownicy będą mogli zarejestrować urządzeń
- Ustawianie zintegrowanego uwierzytelniania systemu Windows (Zintegrowane) jako prawidłowe sposobem uwierzytelnianie wieloskładnikowe w programie AD FS



## <a name="set-a-service-connection-point-for-discovery-of-the-azure-ad-tenant"></a>Ustaw punkt połączenia usługi dla odnajdowania dzierżawy Azure AD

Obiektu punktu połączenia usługi, musi istnieć w konfiguracji nazewnictwa podziału kontekstowe las domeny, w której komputery są połączone. Punkt połączenia usługi przechowuje informacji o dzierżawie Azure AD, której zarejestrować komputerów. W konfiguracji usługi Active Directory wiele las punktu połączenia usługi musi istnieć w lasach wszystkich komputerów domeny.

Ustaw punkt połączenia usługi w następujących lokalizacjach w usłudze Active Directory:

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context]

> [AZURE.NOTE] Las z usługi Active Directory domain nazwę *example.com*, konfiguracji kontekst nazw jest CN = Konfiguracja, kontrolera domeny = przykład kontrolera domeny = com.

Możesz sprawdzić wartości obiektu i odnajdowanie dzierżawy Azure AD przy użyciu tego skryptu programu Windows PowerShell. (Zamiast kontekstu nazewnictwa Konfiguracja w przykładzie kontekstu nazewnictwa konfiguracji).

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

`$scp.Keywords` Dane wyjściowe wyświetla informacje o dzierżawie Azure AD:

    azureADName:microsoft.com

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Jeśli punkt połączenia usługi nie istnieje, utwórz ją, uruchamiając następujące skrypt programu PowerShell na serwerze narzędzie Azure AD Connect:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


> [AZURE.NOTE]Po uruchomieniu `$aadAdminCred = Get-Credential`, użyj formatu *user@example.com* dla nazwy użytkownika w oknie dialogowym **Pobieranie poświadczeń** .  
> Po uruchomieniu polecenia cmdlet ADSyncDomainJoinedComputerSync inicjowania Zamień konta domeny, które jest używane w konta łącznik usługi Active Directory [*Nazwa konta łącznik*].  
> Polecenie cmdlet używa moduł Active Directory programu PowerShell, który korzysta z usługi sieci Web w usłudze Active Directory w kontrolerze domeny. Usługi sieci Web w usłudze Active Directory jest obsługiwana w kontrolerach domen w systemie Windows Server 2008 R2 i nowszych wersjach. Kontrolerów domen w systemie Windows Server 2008 lub starszych wersjach Tworzenie punktu połączenia usługi przy użyciu interfejsu API System.DirectoryServices przy użyciu programu PowerShell, a następnie przypisz wartości słów kluczowych.

## <a name="create-ad-fs-rules-for-instant-device-registration-in-federated-organizations"></a>Tworzenie usług AD FS zasady rejestracji urządzenia błyskawiczne w organizacjach federacyjnych

W federacyjnych Azure AD konfiguracji urządzeń Polegaj usług AD FS (lub na serwerze federacyjnym lokalnego) do uwierzytelniania Azure AD. Następnie zarejestrować ich przed Azure usługa Active Directory urządzenia rejestracji (Usługa rejestracji Azure AD urządzenia).

> [AZURE.NOTE] W konfiguracji niefederacyjnej hasła użytkownika są synchronizowane z Azure AD i uwierzytelniania systemu Windows 10 i Windows Server 2016 komputerach domeny usługi rejestracji Azure AD urządzenia. Użytkownik uwierzytelnia się przy użyciu poświadczeń, który użytkownik zapisuje do ich lokalnych kont komputera i które jest przekazywany do Azure AD przy użyciu Azure AD Connect. 10 systemu Windows i komputerów systemu Windows Server 2016 w konfiguracji niefederacyjnej masz opcji, aby ustawić urzędu certyfikacji opartych na urządzeniach w Twojej organizacji. Aby uzyskać więcej informacji zobacz pakietów pobieranie Instalatora Windows sekcji dla komputerów - systemu Windows 10, w tym artykule.

W przypadku komputerów systemu Windows 10 i Windows Server 2016 narzędzie Azure AD Connect przypisuje obiektu urządzenia w Azure AD obiekt konta komputera lokalnego. Podczas uwierzytelniania dla usługi rejestracji Azure AD urządzenia ukończyć rejestracji i utworzyć obiektu urządzenia, musi istnieć oświadczeniach następujące czynności:

- http://schemas.microsoft.com/ws/2012/01/AccountType zawiera wartość DJ, która określa kapitału uwierzytelnienia jako komputer domeny.
- http://schemas.microsoft.com/Identity/Claims/onpremobjectguid zawiera wartość atrybutu **objectGUID** konta komputera lokalnego.
- http://schemas.microsoft.com/WS/2008/06/Identity/Claims/primarysid zawiera komputera zabezpieczeń podstawowy identyfikator, która odpowiada wartość atrybutu **objectSid** konta komputera lokalnego.
- http://schemas.microsoft.com/WS/2008/06/Identity/Claims/issuerid zawiera wartość, w którym Azure AD przy użyciu zaufania token wystawiony z usług AD FS lub z lokalnego zabezpieczeń tokenu usługi (STS). Jest to ważne w wiele las konfiguracji usługi Active Directory. W tej konfiguracji komputerów może być dołączone do las niż ten, który łączy się Azure AD przy usług AD FS lub STS lokalnego. Usług AD FS Użyj wartości z pola http://<*nazwy domeny*>/adfs/usługi/zaufania /, gdzie <*Nazwa domeny*> sprawdzanej nazwą domeny jest w Azure AD.

Aby ręcznie utworzyć tych reguł w usług AD FS skorzystaj z tego skryptu programu PowerShell w sesji, który jest połączony z serwerem. Zamień pierwszy wiersz z nazwą domeny organizacji sprawdzanej w Azure AD.

> [AZURE.NOTE] Jeśli nie korzystasz z usług AD FS dla lokalnego serwera Federacji wykonaj instrukcje z dostawcą, aby utworzyć reguły, które problemu tych roszczeń.

    $validatedDomain = "example.com"      # Replace example.com with your organization's validated domain name in Azure AD

    $rule1 = '@RuleName = "Issue object GUID"

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain-joined computers"

      c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

> [AZURE.NOTE] Należy użyć tylko trzy pierwsze reguły do środowiska lokalnego pojedynczy las. Jeśli komputery znajdują się w różnych las niż synchronizacji z usługą Azure Active Directory lub jeśli używasz nazw alternatywnych do nich w konfiguracji synchronizacji, można także dodać pozostałych reguł.

    $rule3 = '@RuleName = "Pass through primary SID"

      c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(claim = c2);'

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account"

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"])

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");'

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID"

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));'

    $rule6 = '@RuleName = "Update issuer for DJ computer auth"

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");'

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

> [AZURE.NOTE] Uwierzytelniania systemu Windows 10 i Windows Server 2016 komputerach domeny przy użyciu Zintegrowane do aktywnego punktu końcowego WS zaufania, który znajduje się w usługach AD FS. Upewnij się, że punkt końcowy jest aktywny. Jeśli korzystasz z serwera Proxy aplikacji sieci Web, upewnij się, opublikowania tego punktu końcowego za pośrednictwem serwera proxy. Punkt końcowy jest adfs usług i zaufania-13-windowstransport. Aby sprawdzić, czy jest aktywny w konsoli zarządzania usług AD FS przejdź do **usługi** > **punktów końcowych**. Jeśli nie korzystasz z usług AD FS dla lokalnego serwera Federacji postępuj zgodnie instrukcjami z dostawcą, aby upewnić się, że odpowiednie punkt końcowy jest aktywny.



## <a name="set-up-ad-fs-for-authentication-of-device-registration"></a>Konfigurowanie usług AD FS uwierzytelniania rejestracji urządzenia

Upewnij się, że Zintegrowane jest ustawiony na prawidłową sposobem uwierzytelnianie wieloskładnikowe rejestracji urządzenia w programie AD FS. W tym celu należy następująco skonfigurować emisji Przekształć regułę, która przechodzi metodę uwierzytelniania.

1. W konsoli zarządzania usług AD FS przejdź do **usług AD FS** > **Relacji zaufania** > **Polegaj strona zaufanie**.

2. Kliknij prawym przyciskiem myszy obiekt zaufania strona pośrednicząca platformy tożsamości pakietu Microsoft Office 365, a następnie wybierz pozycję **Edytuj reguły oświadczeń**.

3.  Na karcie **Przekształcanie wydawania** wybierz pozycję **Dodaj regułę**.

4.  Na liście szablon **reguły zastrzeżenie** zaznacz pole wyboru **Wyślij oświadczeń za pomocą reguły niestandardowe**.

5.  Wybierz przycisk **Dalej**.

6.  W polu **Nazwa reguły zastrzeżenie** wprowadzanie **Reguły zastrzeżenie metody uwierzytelniania**.

7.  W oknie dialogowym **reguły roszczeń** wprowadź następujące polecenie:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
    => issue(claim = c);`.

8. Na serwerze Federacji wprowadź tego polecenia programu PowerShell:

    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

<*RPObjectName*> uzależnionej nazwa obiektu strony obiektu zaufania strona pośrednicząca Azure AD. Ten obiekt jest zwykle nazwę *Platformy tożsamości pakietu Microsoft Office 365*.



## <a name="deployment-and-rollout"></a>Wdrażanie i rozmieszczenia

Komputery domeny spełnia wymagania wstępne, są gotowe do rejestrowania z usługą Azure Active Directory.

Windows Update rocznicy 10 i Windows Server 2016 komputerach domeny automatycznie rejestrować Azure AD przy następnym uruchomieniu urządzenia lub gdy użytkownik rejestruje do systemu Windows. Nowe komputery, które są połączone z domeną rejestrować Azure AD przy uruchomieniu urządzenia po operacji dołączania domeny.

> [AZURE.NOTE] Komputery domeny systemu Windows 10 automatycznie rejestrować Azure AD tylko wtedy, gdy ustawiono rozmieszczenia obiektu zasad grupy.

Za pomocą obiektu zasad grupy do sterowania wprowadzaniem automatycznej rejestracji systemu Windows 10 i Windows Server 2016 komputerów domeny. Do wdrożenia automatyczną rejestrację komputerów domeny — systemu Windows 10, należy wdrożyć pakiet Instalatora Windows na komputerach, które można wybrać.

> [AZURE.NOTE] Zasady grupy dla kontrolki rozmieszczenia uaktywnia również rejestracji komputerów domeny Windows 8.1. Zasady służy do rejestrowania komputerów domeny Windows 8.1. Lub, jeśli masz kilka wersji systemu Windows, łącznie z wersjami systemu Windows 7 lub Windows Server, przy użyciu pakietu Instalatora Windows można zarejestrować — systemu Windows 10 i komputerach z systemem Windows Server 2016.

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>Tworzenie obiektu zasad grupy, aby kontrolować wprowadzaniem automatycznej rejestracji

Aby kontrolować wdrożenia automatycznej rejestracji komputerów domeny z usługą Azure Active Directory, należy wdrożyć zasad grupy **zarejestrować domeny komputery jako urządzeń** na komputerach, które chcesz zarejestrować. Na przykład należy wdrożyć zasady jednostkami organizacyjnymi lub grupy zabezpieczeń.

Aby ustawić zasady, wykonaj następujące czynności:

1. Otwórz Menedżera serwera, a następnie przejdź do pozycji **Narzędzia** > **Zarządzania zasadami grupy**.

2. Przejdź do węzła domeny, który odpowiada domenę, której chcesz uaktywnić automatyczne rejestracji systemu Windows 10 lub Windows Server 2016 komputerów.

3. Kliknij prawym przyciskiem myszy **Obiekty zasad grupy**, a następnie wybierz pozycję **Nowy**.

4. Wprowadź nazwę obiektu zasad grupy. Na przykład *Automatycznej rejestracji do Azure AD*. Wybierz **przycisk OK**.

4. Kliknij prawym przyciskiem myszy nowy obiekt zasad grupy, a następnie kliknij przycisk **Edytuj**.

5. Przejdź do pozycji **Konfiguracja komputera** > **zasady** > **Szablony administracyjne** > **składniki Windows** > **Rejestracja urządzenia**. Kliknij prawym przyciskiem myszy **domeny rejestru sprzężone komputery jako urządzenia**, a następnie wybierz polecenie **Edytuj**.

    > [AZURE.NOTE] Ten szablon zasad grupy została zmieniona z wcześniejszych wersji konsoli zarządzania zasadami grupy. Jeśli używasz wcześniejszej wersji konsoli, przejdź do **Konfiguracji komputera** > **zasady** > **Szablony administracyjne** > **Składniki Windows** > **Sprzężenia pracy** > **automatycznie pracy komputerów klienckich sprzężenia**.

6. Wybierz opcję **włączone**, a następnie wybierz pozycję **Zastosuj**.

7. Wybierz **przycisk OK**.

8. Łączenie obiektu zasad grupy z wybraną lokalizację. Na przykład można je połączyć określonej jednostce organizacyjnej. Możesz również można połączyć je określonej grupy zabezpieczeń programu komputerach automatyczne rejestrowanie z usługą Azure Active Directory. Aby ustawić tych zasad dla wszystkich domeny systemu Windows 10 i Windows Server 2016 komputerach w organizacji, połącz obiekt zasad grupy z domeną.

### <a name="download-windows-installer-packages-for-non-windows-10-computers"></a>Pobieranie pakietów Instalatora Windows na komputerach innych niż systemu Windows 10  

Aby zarejestrować domeny komputerach z systemem Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 lub Windows Server 2008 R2, można pobrać i zainstalować plików tych Instalatora Windows (msi):

- [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
- [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

Wdrażanie pakietu przy użyciu systemu dystrybucji oprogramowania takich jak Menedżera konfiguracji centrum systemu. Pakiet obsługuje opcje instalacji standardowej odbiorcze z parametrem *ciche* . Menedżer konfiguracji programu System Center 2016 oferuje dodatkowe korzyści z wcześniejszych wersji, takich jak możliwość śledzenia złożonym rejestracji. Aby uzyskać więcej informacji zobacz [System Centrum 2016](https://www.microsoft.com/en-us/cloud-platform/system-center).

Instalator tworzy zadanie w systemie działa w kontekście użytkownika. Zadanie jest wyzwalane, gdy użytkownik znaki w do systemu Windows. Zadanie cichym rejestruje urządzenie Azure AD przy użyciu poświadczeń użytkownika po uwierzytelnieniu za pośrednictwem Zintegrowane. Aby wyświetlić zaplanowane zadanie, należy przejść do **witryny Microsoft** > **Dołączanie do obszaru roboczego**, a następnie przejdź do biblioteki harmonogram zadań.



## <a name="next-steps"></a>Następne kroki

- [Azure Active Directory warunkowego dostępu](active-directory-conditional-access.md)
