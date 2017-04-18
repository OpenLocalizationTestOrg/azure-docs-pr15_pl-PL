<properties
    pageTitle="Konfigurowanie warunkowego dostępu do lokalnego przy użyciu Azure Active Directory Rejestracja urządzenia | Microsoft Azure"
    description="Przewodnik krok po kroku, aby umożliwić warunkowe dostęp do aplikacji lokalnego przy użyciu usługi federacyjne w usłudze Active Directory (AD FS) w systemie Windows Server 2012 R2."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>


# <a name="setting-up-on-premises-conditional-access-using-azure-active-directory-device-registration"></a>Aby skonfigurować warunkowego dostępu do lokalnego przy użyciu Azure Active Directory Rejestracja urządzenia

Osobiście własnością urządzeń użytkowników można oznaczyć znane dla Twojej organizacji przez wymaganie użytkownikom na dołączanie do w miejscu pracy urządzeń z usługą Azure Active Directory Rejestracja urządzenia. Poniżej znajduje się przewodnik krok po kroku, aby umożliwić warunkowe dostęp do aplikacji lokalnego przy użyciu usługi federacyjne w usłudze Active Directory (AD FS) w systemie Windows Server 2012 R2.

> [AZURE.NOTE]
> Licencji usługi Office 365 lub licencji Azure AD Premium jest wymagana podczas przy użyciu urządzeń w zasady dostępu warunkowego usługi Azure Active Directory Rejestracja urządzenia. Ta opcja uwzględnia zasady nałożonych przez Active Directory Federation Services (AD FS) do zasobów lokalnych.

Aby uzyskać więcej informacji na scenariuszach warunkowego dostępu dla lokalnego zobacz [Dołączanie do pracy z dowolnego urządzenia w celu logowania jednokrotnego i bezproblemowa drugiego czynniki uwierzytelniania w aplikacji firmy](https://technet.microsoft.com/library/dn280945.aspx).

Te funkcje są dostępne dla klientów, którzy zakupili licencji usługi Azure Active Directory Premium.

<a name="supported-devices"></a>Obsługiwane urządzenia
-------------------------------------------------------------------------
* Windows 7 domeny sprzężone urządzenia.
* System Windows 8.1 osobiste i domeny sprzężone urządzenia.
* iOS 6 lub nowszy dla przeglądarki Safari
* Android 4.0 lub nowszym, Samsung GS3 lub powyżej telefony, Samsung Uwaga2 lub powyżej tabletów.


<a name="scenario-prerequisites"></a>Wymagania wstępne dotyczące scenariuszy
------------------------------------------------------------------------
* Subskrypcja usługi Office 365 lub Premium Azure Active Directory
* Dzierżawę usługi Azure Active Directory
* Usługi Active Directory systemu Windows Server (Windows Server 2008 lub nowszego)
* Zaktualizowanego schematu w systemie Windows Server 2012 R2
* Licencja usługi Azure Active Directory Premium
* Windows Server 2012 R2 Federation Services, skonfigurowany do logowania jednokrotnego do Azure AD
* Windows Server 2012 R2 sieci Web aplikacji serwera Proxy Microsoft Azure Active Directory nawiązywanie połączenia (Azure AD Connect). [Pobierz Azure AD Connect tutaj](http://www.microsoft.com/en-us/download/details.aspx?id=47594).
* Zweryfikowaną domeną.

<a name="known-issues-in-this-release"></a>Znane problemy w tej wersji
-------------------------------------------------------------------------------
* Zasady dostępu warunkowego urządzenie podstawie wymagają obiektu urządzenia zapisu z powrotem do usługi Active Directory z usługi Azure Active Directory. Może potrwać do 3 godziny dla obiektów urządzenia za napisane z powrotem do usługi Active Directory
* urządzeń z systemem iOS 7 zawsze wyświetli monit o wybranie certyfikatu podczas uwierzytelnianie klienta na podstawie certyfikatu.
* Niektóre wersje iOS8, zanim iOS 8.3 nie działają.

## <a name="scenario-assumptions"></a>Założenia scenariusz
W tym scenariuszu założono, że masz środowiska hybrydowego składający się z dzierżawą usługi Azure AD i usługi Active Directory w lokalnej. Te dzierżaw powinny być połączone za pomocą narzędzie Azure AD Connect oraz zweryfikowaną domeną i usług AD FS dla rejestracji Jednokrotnej. Poniższa lista kontrolna pomoże Ci skonfigurować środowisko do etapu opisany powyżej.

<a name="checklist-prerequisites-for-conditional-access-scenario"></a>Lista kontrolna: Wymagania wstępne dotyczące scenariusz dostępu warunkowego
--------------------------------------------------------------
Łączenie dzierżawy usługi Azure AD z usługą Active Directory w lokalnej.

## <a name="configure-azure-active-directory-device-registration-service"></a>Konfigurowanie usługi rejestracji urządzenia Azure Active Directory
Skorzystaj z tego przewodnika wdrażać i konfigurowanie usługi rejestracji urządzenia Active Directory platformy Azure dla Twojej organizacji.

Ten przewodnik założono, że skonfigurowano usługi Active Directory systemu Windows Server i zasubskrybowano usługę Microsoft Azure Active Directory. Zobacz wymagania wstępne dotyczące powyżej.

Aby wdrożyć usługi rejestracji urządzenia Active Directory platformy Azure dzierżawy usługi Azure Active Directory, wykonywanie zadań z poniższej listy kontrolnej w kolejności. Po łącza umożliwia przejście do tematami, wróć do tej listy kontrolnej po przejrzeniu temat koncepcyjny tak, aby można przystąpić do pozostałych zadań z tej listy kontrolnej. Niektóre zadania będą obejmowały krokiem sprawdzania poprawności scenariusza, który pomoże Ci upewnij się, że krok zakończyło się pomyślnie.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Część 1: Rejestracja urządzenia Azure Active Directory Włącz

Postępuj zgodnie z listy kontrolnej poniżej, aby włączyć i skonfigurować Azure Active Directory urządzenia usługi rejestracji.

| Zadanie                                                                                                                                                                                                                                                                                                                                                                                             | Odwołania                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Włącz Rejestracja urządzenia w dzierżawie usługi Azure Active Directory, aby umożliwić urządzenia, aby dołączyć do obszaru roboczego. Domyślnie uwierzytelnianie wieloskładnikowe nie jest włączone dla usługi. Jednak uwierzytelnianie wieloskładnikowe zaleca się podczas rejestrowania urządzenia. Przed włączeniem uwierzytelnianie wieloskładnikowe w ADRS, upewnij się, że usług AD FS jest skonfigurowany do dostawcy uwierzytelnianie wieloskładnikowe. | [Włączanie Rejestracja urządzenia Azure Active Directory](active-directory-conditional-access-device-registration-overview.md)               |
| Urządzenia wykryje Azure Active urządzenia rejestracji usługi katalogowej, szukając znanego rekordy DNS. Firma DNS należy skonfigurować tak, aby urządzenia mogą one znaleźć Azure Active urządzenia rejestracji usługi katalogowej.                                                                                                                                                   | [Konfigurowanie odnajdowania Azure Active Directory Rejestracja urządzenia.](active-directory-conditional-access-device-registration-overview.md) |

##<a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Część 2: Wdrażanie i skonfigurowania systemu Windows Server 2012 R2 Active Directory Federation Services i relacji federacji z usługą Azure Active Directory

| Zadanie                                                                                                                                                                                                                                                                                                                                                                                             | Odwołania                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Wdrażanie usług domenowych Active Directory domeny w usłudze Windows Server 2012 R2 rozszerzenia schematu. Nie musisz uaktualnić żadnej kontrolerów domeny do systemu Windows Server 2012 R2. Uaktualnienie schematu jest wymagane tylko. | [Uaktualnianie schematu usług domeny usługi Active Directory](#upgrade-your-active-directory-domain-services-schema)               |
| Urządzenia wykryje Azure Active urządzenia rejestracji usługi katalogowej, szukając znanego rekordy DNS. Firma DNS należy skonfigurować tak, aby urządzenia mogą one znaleźć Azure Active urządzenia rejestracji usługi katalogowej.                                                                                                                                                   | [Przygotowywanie urządzenia pomocy technicznej usługi Active Directory](#prepare-your-active-directory-to-support-devices) |


##<a name="part-3-enable-device-writeback-in-azure-ad"></a>Część 3: Włącz urządzenie zapisu w Azure AD

| Zadanie                                                                                                                                                                                                                                                                                                                                                                                             | Odwołania                                                       |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| Wykonywanie część 2 włączanie urządzenia zapisu w narzędzie Azure AD Connect. Po zakończeniu zwracana to tego przewodnika. | [Włączanie zapisu urządzenia w Azure AD Connect](#upgrade-your-active-directory-domain-services-schema)               |


##<a name="optional-part-4-enable-multi-factor-authentication"></a>[Opcjonalne] Część 4: Włącz uwierzytelnianie wieloskładnikowe

Zdecydowanie zaleca się, aby skonfigurować jedną z kilku opcji uwierzytelniania wieloskładnikowego. Jeśli chcesz wymagać MFA, zobacz [Wybierz rozwiązanie wieloskładnikowe zabezpieczeń dla Ciebie](../multi-factor-authentication/multi-factor-authentication-get-started.md). Zawiera opis każdego z rozwiązań i łącza ułatwiające konfigurowanie rozwiązania wybranych przez użytkownika.

## <a name="part-5-verification"></a>Część 5: Weryfikacja

Wdrażanie zostało zakończone. Można teraz wypróbować kilka scenariuszy. Skorzystaj z łączy poniżej, aby poeksperymentować z usługą i zapoznać się z funkcjami


| Zadanie                                                                                                                                                                                                                         | Odwołania                                                                       |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Dołączanie do niektórych urządzeń z Twojej pracy przy użyciu Azure Active Directory Rejestracja urządzenia. Możesz dołączyć iOS, systemu Windows i urządzeń z systemem Android                                                                                         | [Dołączanie do urządzenia z Twojej pracy przy użyciu Azure Active Directory Rejestracja urządzenia](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Możesz wyświetlać i włączanie i wyłączanie zarejestrowanych urządzeń za pomocą portalu administratora. W tym zadaniu będzie wyświetlanie niektórych urządzeń zarejestrowanych za pomocą portalu administratora.                                                        | [Przegląd rejestracji urządzenia Azure Active Directory](active-directory-conditional-access-device-registration-overview.md)                             |
| Upewnij się, że obiekty urządzeń zostaną zapisane z usługi Azure Active Directory do systemu Windows Server Active Directory.                                                                                                                  | [Sprawdź zarejestrowanych urządzeń są zapisywane z powrotem do usługi Active Directory](#verify-registered-devices-are-written-back-to-active-directory)                  |
| Teraz, gdy użytkownicy będą mogli zarejestrować swoich urządzeń, możesz utworzyć aplikację zasad dostępu w programie AD FS Zezwalaj tylko zarejestrowanych urządzenia. W tym zadaniu, które będą tworzone reguła aplikacji programu access i niestandardowy dostępu odmowa wiadomości | [Tworzenie aplikacji programu access zasad i niestandardowe komunikat o odmowie dostępu](#create-an-application-access-policy-and-custom-access-denied-message)            |



## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Integracja z usługą Active Directory w lokalnej usługi Azure Active Directory
Dzięki temu można zintegrować z lokalnej usługi active directory, przy użyciu Azure AD Connect dzierżawcy usługi Azure AD. Mimo że czynności są dostępne w portalu klasyczny Azure, Zanotuj wszelkie specjalne instrukcje wymienionych w tej sekcji.

1.  Zaloguj się do portalu klasyczny Azure przy użyciu konta administratora globalnego w Azure AD.
2.  W okienku po lewej stronie wybierz pozycję **Usługi Active Directory**.
3.  Na karcie **katalog** zaznacz katalogu.
4.  Wybierz kartę **Integracja katalogów** .
5.  W sekcji **Wdrażanie i zarządzanie nią** wykonaj kroki od 1 do 3, aby Integracja usługi Azure Active Directory z katalogu lokalnego.
  1.    Dodawanie domeny.
  2.    Zainstaluj i uruchom narzędzie Azure AD Connect: Instalowanie Azure AD Connect przy użyciu poniższych instrukcji, [Instalacja niestandardowa: narzędzie Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).
  3. Sprawdź i zarządzanie synchronizacji katalogów. Instrukcje rejestracji jednokrotnej są dostępne w ramach tego kroku.

  > [AZURE.NOTE]
  > Konfigurowanie federacji z usług AD FS opisane w dokumencie połączone powyżej. Nie musisz skonfigurować dowolne z funkcji podglądu.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Uaktualnianie schematu usług domenowych Active Directory

> [AZURE.NOTE]
> Uaktualnianie schematu usługi Active Directory nie można cofnąć. Zaleca się najpierw wykonanie to w środowisku testowym.

1. Zaloguj się do kontrolera domeny przy użyciu konta z uprawnieniami zarówno administrator przedsiębiorstwa, jak i administratorów schematu.
2. Kopiowanie katalogu **\support\adprep [multimediów]** i podkatalogów do jednego z kontrolerów domeny usługi Active Directory.
3. Gdzie [multimediów] to ścieżka do nośnika instalacyjnego systemu Windows Server 2012 R2.
4. W wierszu polecenia przejdź do katalogu adprep i wykonywanie: **adprep.exe/forestprep**. Postępuj zgodnie z instrukcjami na ekranie, aby ukończyć uaktualnienie schematu.

## <a name="prepare-your-active-directory-to-support-devices"></a>Przygotowywanie usługi Active Directory w celu obsługi urządzeń

>[AZURE.NOTE] Jest to operacja jednorazowa, który należy uruchomić, aby przygotować las usługi Active Directory do obsługi urządzeń. Użytkownik musi być zalogowany z uprawnieniami administratora przedsiębiorstwa i las usługi Active Directory musi mieć schematu systemu Windows Server 2012 R2, aby wykonać tę procedurę.


##<a name="prepare-your-active-directory-forest-to-support-devices"></a>Przygotować las usługi Active Directory do obsługi urządzeń

> [AZURE.NOTE]
>Jest to operacja jednorazowa, który należy uruchomić, aby przygotować las usługi Active Directory do obsługi urządzeń. Użytkownik musi być zalogowany z uprawnieniami administratora przedsiębiorstwa i las usługi Active Directory musi mieć schematu systemu Windows Server 2012 R2, aby wykonać tę procedurę.

### <a name="prepare-your-active-directory-forest"></a>Przygotować las usługi Active Directory

1.  Na serwerze Federacji, Otwórz okno poleceń programu Windows PowerShell i wpisz: ADDeviceRegistration inicjowania
2.  Po wyświetleniu monitu dla **ServiceAccountName**, wprowadź nazwę konta usługi wybranego jako konta usługi dla usług AD FS. Jeśli jest to konto gMSA, wprowadź poświadczenia konta w formacie **domain\accountname$** . Konta domeny użyj formatu **domain\accountname**.



### <a name="enable-device-authentication-in-ad-fs"></a>Włącz uwierzytelnianie urządzenia w programie AD FS

1. Na serwerze Federacji Otwórz konsolę zarządzania usług AD FS i przejdź do **usług AD FS** > **Zasady uwierzytelniania**.
2. Kliknij przycisk **Edytuj globalny uwierzytelniania podstawowego** w okienku **akcji** .
3. Sprawdzanie, **Włącz uwierzytelnianie urządzenia** , a następnie wybierz**przycisk OK**.
4. Domyślnie usług AD FS będzie okresowo Usuń nieużywane urządzenia z usługi Active Directory. Po użyciu Azure Active Directory Rejestracja urządzenia, tak aby urządzenia można zarządzać w Azure, musisz wyłączyć to zadanie.


### <a name="disable-unused-device-cleanup"></a>Wyłączanie Oczyszczanie nieużywanego urządzenia
1. Na serwerze Federacji, Otwórz okno poleceń programu Windows PowerShell i wpisz: Set-AdfsDeviceRegistration - MaximumInactiveDays 0

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Przygotowywanie do zapisu urządzenia Azure AD Connect

1.  Wypełnij część 1: Przygotowywanie Azure AD połączenia.


## <a name="join-devices-to-your-workplace-using-azure-active-directory-device-registration"></a>Dołączanie do urządzenia z Twojej pracy przy użyciu Azure Active Directory Rejestracja urządzenia

### <a name="join-an-ios-device-using-azure-active-directory-device-registration"></a>Dołączanie przy użyciu Azure Active Directory urządzenia rejestracji urządzenia z systemem iOS

Azure Rejestracja urządzenia Active Directory używa procesu rejestracji Over-the-Air profilu dla urządzeń z systemem iOS. Ten proces zaczyna się od nawiązywanie połączenia za pomocą przeglądarki Safari adres URL rejestrowania profilu użytkownika. Format adresu URL jest w następujący sposób:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Miejsce, w którym `yourdomainname` jest nazwą domeny, który został skonfigurowany z usługą Azure Active Directory. Na przykład jeśli nazwą domeny jest contoso.com, adres URL jest następujący:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Istnieje wiele różnych sposobów komunikowania się ten adres URL do użytkowników. Jednym ze sposobów zalecane jest, aby opublikować ten adres URL w programie access niestandardową aplikację odmowa wiadomości w programie AD FS. Ten omówiono w sekcji informacje o nadchodzących: [Tworzenie zasad aplikacji programu access i niestandardowe komunikat o odmowie dostępu](#create-an-application-access-policy-and-custom-access-denied-message).

###<a name="join-a-windows-81-device-using-azure-active-directory-device-registration"></a>Dołączanie przy użyciu Azure Active Directory urządzenia rejestracji urządzenia Windows 8.1

1. Na urządzeniu z systemem Windows 8.1, przejdź do **Obszaru Ustawienia komputera** > **sieci** > **miejscu pracy**.
2. Wprowadź nazwę użytkownika w formacie głównej nazwy użytkownika. Na przykład dan@contoso.com..
3. Wybierz pozycję **Dołącz**.
4. Po wyświetleniu monitu zaloguj się przy użyciu poświadczeń. Urządzenie jest teraz połączone.

###<a name="join-a-windows-7-device-using-azure-active-directory-device-registration"></a>Dołączanie do urządzenia systemu Windows 7 przy użyciu Azure Active Directory Rejestracja urządzenia
Aby zarejestrować domeny systemu Windows 7 sprzężone urządzeń, należy zainstalować pakiet oprogramowania rejestracji urządzenia. Pakiet oprogramowania nosi nazwę obszaru roboczego dołączanie dla systemu Windows 7 i jest dostępna do pobrania w [witrynie sieci Web Microsoft Connect](https://connect.microsoft.com/site1164). Instrukcje dotyczące używania pakietu są dostępne w [Rejestracja urządzenia automatyczne konfigurowanie dla systemu Windows 7 domeny sprzężone urządzeń](active-directory-conditional-access-automatic-device-registration-windows7.md).

### <a name="join-an-android-device-using-azure-active-directory-device-registration"></a>Dołączanie do urządzenia z systemem Android przy użyciu Azure Active Directory Rejestracja urządzenia

[Uwierzytelnienia Azure tematu Android](active-directory-conditional-access-azure-authenticator-app.md) zawiera instrukcje dotyczące instalowania aplikacji Azure uwierzytelnienia na urządzeniu z systemem Android i dodawanie konta służbowego. Gdy konta służbowego jest tworzony pomyślnie na urządzeniu z systemem Android, urządzenie jest dołączony do organizacji w miejscu pracy.

## <a name="verify-registered-devices-are-written-back-to-active-directory"></a>Sprawdź zarejestrowanych urządzenia są zapisywane z powrotem do usługi Active Directory
Można przeglądać i sprawdź, czy obiekty urządzenia zapisano ponownie do usługi Active Directory przy użyciu LDP.exe lub Edycja ADSI. Obie funkcje są dostępne narzędzia Active Directory administratora.

Domyślnie obiekty urządzenia, które są zapisywane wstecz z usługi Azure Active Directory zostaną umieszczone w tej samej domenie co farmie usług AD FS.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Tworzenie aplikacji programu access zasad i niestandardowe komunikat o odmowie dostępu
Scenariusz: tworzenie aplikacji Relying strona zaufania w programie AD FS i skonfigurować reguły autoryzacji wydania umożliwiający tylko zarejestrowanych urządzenia. Teraz tylko urządzenia, które są zarejestrowane mogą uzyskiwać dostęp do aplikacji. Aby ułatwić użytkownikom dostęp do aplikacji, można skonfigurować niestandardowe wiadomości, która zawiera instrukcje na temat dołączyć do swoich urządzeń dostępu. Użytkownicy mają teraz swobodne zarejestrować urządzeń tej osoby, aby można było uzyskać dostęp do aplikacji.

Poniższe kroki pokazują jak wdrażać w tym scenariuszu.

>[AZURE.NOTE]
W tej sekcji przyjęto założenie, że została już skonfigurowana Polegaj firmy zaufanie dla aplikacji usług AD FS.

1. Otwieranie narzędzia AD FS MMC i przejdź do usług AD FS > Relacje zaufania > Strona Polegaj zaufanie.
2. Znajdź aplikację, dla której ta nowa reguła dostępu zostaną zastosowane. Kliknij prawym przyciskiem myszy aplikację i wybierz pozycję Edytuj reguły zastrzeżenie...
3. Wybierz kartę **Wydawania autoryzacji** , a następnie wybierz pozycję **Dodaj regułę**
4. **Reguły zastrzeżenie** szablon listy rozwijanej zaznacz **o zezwolenie lub odmowa użytkowników w oparciu o przychodzących zastrzeżenie**. Wybierz przycisk **Dalej**.
5. W polu Nazwa reguły roszczeń: typ pola: **zezwolenia na dostęp z zarejestrowanych urządzeń**
6. Z poczty przychodzącej rościć sobie typ: liście rozwijanej, zaznacz **Użytkownik jest zarejestrowany**.
7. W przychodzące przejmowania wartość: wpisz: **PRAWDA**
8. Wybierz przycisk radiowy **zezwolenia na dostęp do użytkowników z tym roszczeń przychodzących** .
9. Wybierz przycisk **Zakończ** , a następnie wybierz pozycję **Zastosuj**.
10. Usuwanie wszystkich reguł, które są bardziej ograniczeń niż regułę, która została właśnie utworzona. Na przykład usuń reguły domyślnej **Umożliwić dostęp do wszystkich użytkowników** .

Aplikacja jest skonfigurowany do Zezwalaj na dostęp tylko wtedy, gdy użytkownik pochodzi z urządzenia, które zarejestrowane i dołączony do obszaru roboczego. Zasady dostępu bardziej zaawansowane, można znaleźć [Zarządzania ryzykiem z wieloskładnikowe kontrola dostępu](https://technet.microsoft.com/library/dn280949.aspx).

Następnie skonfiguruj niestandardowy komunikat o błędzie dla aplikacji. Komunikat o błędzie będzie informować użytkowników, że musisz dołączania urządzenia do miejsca pracy przed uzyskaniem dostępu do aplikacji. Możesz utworzyć niestandardową aplikację dostępu wiadomości przy użyciu niestandardowych HTML i programu Windows PowerShell.

Na serwerze Federacji Otwórz okno poleceń programu Windows PowerShell i wpisz następujące polecenie. Zastąp części polecenie elementy, które są specyficzne dla używanego systemu:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Aby korzystać z tej aplikacji, musisz zarejestrować urządzenie.

**Jeśli korzystasz z urządzenia z systemem iOS, wybierz to łącze, aby dołączyć do urządzenia**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Dołączanie to urządzenie iOS do miejsca pracy.


**Jeśli korzystasz z urządzenia Windows 8.1**, można dołączać do urządzenia, przechodząc do **Obszaru Ustawienia komputera ** >  **sieci** > **miejscu pracy**.


Gdzie "**Polegaj Nazwa zaufania jednostki**" jest nazwą obiektu Polegaj zaufania firmy aplikacji w programie AD FS.
Gdzie **twoja_domena.com** oznacza nazwę domeny, który został skonfigurowany z usługą Azure Active Directory. Na przykład contoso.com.
Pamiętaj usunąć wszystkie podziały wierszy (jeśli istnieją) z zawartości html, który należy przekazać do polecenia cmdlet **Set-AdfsRelyingPartyWebContent** .


Teraz gdy użytkownicy uzyskują dostęp do aplikacji z urządzenia, który nie jest zarejestrowany w usłudze Azure Active Directory urządzenia rejestracji, otrzymają stronę, która będzie podobny do zrzut poniżej ekranu.

![Screeshot komunikat o błędzie, gdy użytkownicy nie zarejestrowany urządzenia z usługą Azure Active Directory](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)

##<a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
