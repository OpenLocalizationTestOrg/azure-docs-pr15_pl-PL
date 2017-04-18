<properties
    pageTitle="Podłącz domeny urządzenia do Azure AD dla systemu Windows 10 napotkania | Microsoft Azure"
    description="W tym artykule wyjaśniono, jak Administratorzy mogą konfigurować zasad grupy, aby włączyć urządzenia, aby być domeny dołączony do sieci firmowej."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Podłącz urządzenia domeny do Azure AD dla środowiska systemu Windows 10

Dołączanie domeny jest organizacje tradycyjny sposób połączono urządzeń pracy od 15 lat i nie tylko. Jest włączony użytkownikom logowanie się do jego urządzeń przy użyciu swojej pracy systemu Windows Server usługi Active Directory (Active Directory) lub edukacyjne i dozwolone IT, aby w pełni zarządzać tych urządzeń. Organizacje zwykle oparte na obrazowania metodach urządzeń należy do użytkowników i ogólnie za pomocą Menedżera konfiguracji centrum System (SCCM) lub zasady grupy do zarządzania nimi.

Po podłączeniu urządzenia do usługi Azure Active Directory (Azure AD), dołączanie domeny w systemie Windows 10 zapewnia następujące korzyści:

- Logowania jednokrotnego (SSO) do zasobów Azure AD z dowolnego miejsca
- Dostęp do wersji enterprise ze Sklepu Windows przy użyciu konta służbowego (nie wymagane konto Microsoft)
- Zgodny z przedsiębiorstwa mobilnego ustawień użytkownika na urządzeniach przy użyciu konta służbowego (nie wymagane konto Microsoft)
- Silnego uwierzytelniania i wygodny logowania dla konta służbowego z Microsoft Passport i Witaj systemu Windows
- Możliwość Ogranicz dostęp tylko do urządzenia, które są zgodne z ustawienia zasad grupy organizacji urządzenia

## <a name="prerequisites"></a>Wymagania wstępne

Dołączanie domeny jest nadal przydatne. Jednak aby uzyskać Azure AD zalety logowania jednokrotnego, korzystania z roamingu ustawień z pracy lub szkole kont i dostęp do Sklepu Windows przy użyciu konta służbowego, konieczne będzie następujące czynności:

- Azure AD subskrypcji
- Narzędzie Azure AD Connect rozszerzenie katalogu lokalnego Azure AD
- Zasady, która została ustawiona nawiązania połączenia urządzeń domeny Azure AD
- Tworzenie systemu Windows 10 (kompilacja 10551 lub nowszej) dla urządzeń

Aby włączyć Microsoft Passport dla pracy i Witaj systemu Windows, należy także następujące czynności:

- Infrastruktury publicznych () wydaniem certyfikaty użytkownika.
- Menedżer konfiguracji centrum systemu wersji 1509 dla Technical Preview. Aby uzyskać więcej informacji zobacz [Microsoft System Centrum konfiguracji Menedżera Technical Preview](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) i [Blogu zespołu Menedżer konfiguracji systemu w Centrum](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx). Jest to wymagane do wdrożenia certyfikatów użytkowników oparte na programie Microsoft Passport.

Alternatywy dla wymagań wdrażania infrastruktury kluczy publicznych można wykonaj następujące czynności:

- Masz kilka kontrolerów domeny z systemem Windows Server 2016 usług domenowych Active Directory.

Aby włączyć dostęp warunkowego, można utworzyć ustawienia zasad grupy Zezwalaj na dostęp do domeny urządzeń z nie dodatkowych wdrożenia. Aby zarządzać kontrola dostępu oparta na zgodności urządzenia, będą potrzebne następujące elementy:

- Menedżer konfiguracji systemu Centrum wersji 1509 dla Technical Preview Passport scenariuszy

## <a name="deployment-instructions"></a>Instrukcje rozmieszczania



### <a name="step-1-deploy-azure-active-directory-connect"></a>Krok 1: Wdrażanie usługi Azure Active Directory nawiązywanie połączenia

Narzędzie Azure AD Connect pozwoli obsługi administracyjnej komputerach lokalnego jako obiekty urządzeń w chmurze. Aby wdrożyć narzędzie Azure AD Connect, zapoznaj się z "Zainstaluj Azure AD Connect" w artykule [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md#install-azure-ad-connect).

 - Po wykonaniu [instalacji niestandardowej dla Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md) (nie Instalacja ekspresowa), należy wykonać procedury **Tworzenie punktu połączenia usługi w usłudze Active Directory w lokalnej**, w dalszej części tego kroku.
 - Jeśli masz federacyjnych konfiguracji z usługą Azure Active Directory przed zainstalowaniem narzędzie Azure AD Connect (na przykład jeśli wdrożono Active Directory Federation Services (AD FS) przed), następnie wykonaj procedury **Konfigurowanie usług AD FS roszczeń reguły** w dalszej części tego kroku.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Tworzenie punktu połączenia usługi w usłudze Active Directory w lokalnej

Przy użyciu punktu połączenia usługi domeny urządzeń odkrywanie Azure AD dzierżawy informacji w czasie automatycznej rejestracji z usługą Azure urządzenia rejestracji.

Na serwerze narzędzie Azure AD Connect uruchom następujące polecenia programu PowerShell:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Podczas uruchamiania polecenia cmdlet $aadAdminCred = Get-poświadczenie, użyj formatu *user@example.com* dla nazwy użytkownika poświadczeń wprowadzoną, gdy pojawi się okno Pobieranie poświadczeń.

Podczas uruchamiania polecenia cmdlet inicjowania ADSyncDomainJoinedComputerSync..., Zamień konta domeny, które jest używane jako konto łącznik usługi Active Directory [*Nazwa konta łącznik*].

#### <a name="configure-ad-fs-claim-rules"></a>Konfigurowanie reguł roszczeń AD FS
Konfigurowanie reguł roszczeń usług AD FS umożliwia chwilową rejestracji komputer z usługi rejestracji urządzenia Azure, umożliwiając komputerów do uwierzytelniania za pomocą protokołu Kerberos i NTLM za pomocą usług AD FS. Bez tego kroku komputerach rozpocznie się Azure AD w sposób opóźnione (naliczone Azure AD Connect synchronizacją razy).

>[AZURE.NOTE]
Jeśli nie masz AD FS Federacji lokalnego serwera, postępuj zgodnie z instrukcjami dostawcy do tworzenia reguł roszczeń.

Na serwerze usług AD FS (lub w sesji połączenia z serwerem usług AD FS) uruchom następujące polecenia programu PowerShell:

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Komputery systemu Windows 10 będzie uwierzytelniania za pomocą zintegrowane uwierzytelnianie systemu Windows do aktywnego końcowego zaufania WS obsługiwane w usługach AD FS. Upewnij się, że jest włączony tego punktu końcowego. Jeśli korzystasz z serwera Proxy uwierzytelniania sieci Web, również upewnij się, opublikowania tego punktu końcowego za pośrednictwem serwera proxy. Możesz to zrobić, zaznaczając pole wyboru adfs usług i zaufania-13-windowstransport. Należy wyświetlić, jako włączone w konsoli zarządzania usług AD FS w ramach **usługi** > **punktów końcowych**.


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Krok 2: Konfigurowanie urządzenia automatycznego rejestracji za pomocą zasad grupy w usłudze Active Directory

Zasady grupy w usłudze Active Directory umożliwia konfigurowanie urządzeń domeny systemu Windows 10 automatycznie zarejestrować z usługą Azure Active Directory.

> [AZURE.NOTE]
> Aby uzyskać najnowsze instrukcje dotyczące konfigurowania rejestracji urządzenia automatycznego Zobacz, [jak skonfigurować automatyczne rejestracji domeny systemu Windows sprzężone urządzeń z usługą Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> Ten szablon zasad grupy została zmieniona w systemie Windows 10. Jeśli korzystasz z narzędzia Zasady grupy na komputerze z systemem Windows 10, zasady pojawią się jako: <br>
> **Zarejestrować komputery domeny sprzężone jako urządzeń**<br>
> Zasady znajduje się w następującej lokalizacji:<br>
> ***Rejestracja składniki i urządzenia Szablony konfiguracji i zasady/administracyjne/Windows komputera***


## <a name="additional-information"></a>Dodatkowe informacje
* [Windows 10 w przedsiębiorstwie: sposoby za pomocą urządzenia do pracy](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie](active-directory-azureadjoin-user-upgrade.md)
* [Informacje na temat scenariusze użycia dla Azure AD dołączanie](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Podłącz urządzenia domeny do Azure AD dla środowiska systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurowanie Azure AD dołączanie](active-directory-azureadjoin-setup.md)
