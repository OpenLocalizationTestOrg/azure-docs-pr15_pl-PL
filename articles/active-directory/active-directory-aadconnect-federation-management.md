<properties
    pageTitle="Active Directory Federacja usług zarządzania i dostosowania z Azure AD Connect | Microsoft Azure"
    description="AD FS zarządzanie przy użyciu Azure AD Connect i dostosowywaniu AD FS logowania zgodnie z oczekiwaniami użytkowników Azure AD Connect i programu PowerShell."
    keywords="Usług AD FS, usług ADFS, usług AD FS zarządzania AAD połączyć, Połącz, logowania, usług AD FS dostosowywanie, napraw zaufania usługi Office 365, rosyjska, uzależnioną"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Zarządzania w usłudze Active Directory Federation Services i dostosowywanie z Azure AD Connect

Ten artykuł szczegóły zadania związane z Active Directory Federation Services (AD FS), które mogą być wykonywane przy użyciu programu Microsoft Azure Active Directory łączenie oraz innych typowych zadań usług AD FS, które mogą być wymagane na pełną konfigurację farmy usług AD FS.

| Temat | Co obejmuje |
|:------|:-------------|
|**AD FS zarządzania**|
|[Naprawianie zaufanie](#repairthetrust)| Naprawianie relacji zaufania federacji z usługi Office 365 |
|[Dodaj serwer usług AD FS](#addadfsserver) | Rozwijanie farmy usług AD FS przy dodatkowy serwer usług AD FS|
|[Dodawanie serwera proxy aplikacji sieci web usług AD FS](#addwapserver) | Rozwijanie farmy usług AD FS przy dodatkowy serwer WAP|
|[Dodawanie domeny federacyjnych](#addfeddomain)| Dodawanie domeny federacyjnych|
| **AD FS dostosowywania**|
|[Dodawanie logo firmy niestandardowej lub ilustracji](#customlogo)| Dostosowywanie usług AD FS strona logowania z logo firmy i ilustracji |
|[Dodawanie opisu logowania](#addsignindescription) | Dodawanie opisu strona logowania |
|[Modyfikowanie reguł roszczeń AD FS](#modclaims) | Modyfikowanie oświadczeń usług AD FS dla różnych scenariuszy Federacji |

## <a name="ad-fs-management"></a>AD FS zarządzania

Narzędzie Azure AD Connect zapewnia różne zadania związane z usług AD FS, które mogą być wykonywane przy użyciu Kreatora Azure AD Connect z minimalnymi cichym. Po zakończeniu instalacji narzędzie Azure AD Connect za pomocą kreatora można uruchomić kreatora ponownie, aby wykonać dodatkowe zadania.

### Naprawianie Ufaj<a name=repairthetrust></a>

Narzędzie Azure AD Connect można sprawdzić bieżąca kondycja usług AD FS i usługi Azure Active Directory zaufania i wykonać odpowiednie kroki w celu naprawienia zaufanie. Wykonaj poniższe czynności, aby naprawić usługi Azure AD i zaufania usług AD FS.

1. Wybierz z listy zadań dodatkowe **AAD napraw i którym ufasz ADFS** .
![Naprawianie AAD i ADFS zaufania](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. Na stronie **Nawiązywanie połączenia z Azure AD** udostępnia poświadczenia administratora globalnego dla Azure AD i kliknij przycisk **Dalej**.
![Nawiązywanie połączenia z Azure AD](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. Na stronie **poświadczenia dostępu zdalnego** wprowadź poświadczenia administratora domeny.
![Poświadczenia dostępu zdalnego](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    Po kliknięciu przycisku **Dalej**Azure AD Connect sprawdzanie kondycji certyfikatu i Pokaż wszystkie problemy.

    ![Stan certyfikatów](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    Na stronie **gotowy do skonfigurowania** zostanie wyświetlona na liście Akcje wykonywane naprawić zaufania.

    ![Gotowy do skonfigurowania](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. Kliknij przycisk **Zainstaluj** naprawić zaufania.

>[AZURE.NOTE] Azure AD Connect mogą tylko naprawa lub ustawy o certyfikatów, które są podpisem. Narzędzie Azure AD Connect nie może naprawić certyfikatów innych firm.

### Dodaj serwer usług AD FS<a name=addadfsserver></a>

> [AZURE.NOTE] Narzędzie Azure AD Connect wymaga plik certyfikatu PFX, aby dodać serwer usług AD FS. Dlatego tylko wtedy, gdy skonfigurowano farmy usług AD FS za pomocą narzędzie Azure AD Connect można wykonać tej operacji.

1. Wybierz pozycję **Deploy dodatkowy serwer federacyjny** i kliknij przycisk **Dalej**.
![Serwer federacyjny dodatkowe](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. Na stronie **Nawiązywanie połączenia z Azure AD** wprowadzenie poświadczeń administratora globalnego usługi Azure AD, a następnie kliknij przycisk **Dalej**.
![Nawiązywanie połączenia z Azure AD](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. Podaj poświadczenia administratora domeny.
![Poświadczenia administratora domeny](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Narzędzie Azure AD Connect zostanie wyświetlone pytanie o podanie hasła pliku PFX podana podczas konfigurowania nowej farmy usług AD FS z Azure AD Connect. Kliknij przycisk **Wprowadź hasło** o podanie hasła dla pliku PFX.
![Hasło certyfikatu](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![Określ certyfikat SSL](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. Na stronie **AD FS serwery** wprowadź nazwę serwera lub adresu IP w celu dodania do farmy usług AD FS.
![Serwery AD FS](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. Kliknij przycisk **Dalej** , a następnie przejdź do ostatniej stronie **Configure** . Po zakończeniu Azure AD Connect dodanie serwerów do farmy usług AD FS użytkownik będzie miał opcję, aby sprawdzić łączność.
![Gotowy do skonfigurowania](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![Zakończenie instalacji](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### Dodawanie serwera proxy aplikacji sieci web usług AD FS<a name=addwapserver></a>

> [AZURE.NOTE] Narzędzie Azure AD Connect wymaga plik certyfikatu PFX, aby dodać serwera proxy aplikacji sieci web. W związku z tym można wykonać tę operację tylko wtedy, gdy skonfigurowano farmy usług AD FS za pomocą narzędzie Azure AD Connect.

1. **Wdrażanie aplikacji serwera Proxy sieci Web** wybierz z listy dostępnych zadań.
![Wdrażanie serwera proxy aplikacji sieci web](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. Poświadczenia administratora globalnego Azure.
![Nawiązywanie połączenia z Azure AD](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. Na stronie **certyfikat SSL Określ** podania hasła pliku PFX podana podczas konfigurowania farmy usług AD FS za pomocą narzędzie Azure AD Connect.
![Hasło certyfikatu](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![Określ certyfikat SSL](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. Dodaj serwer do dodania jako serwer proxy aplikacji sieci web. Ponieważ serwer proxy aplikacji sieci web mogą nie można dołączyć do domeny, kreator poprosi poświadczeń administracyjnych na serwerze dodana.
![Poświadczenia administracyjne serwera](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. Na stronie **poświadczeń zaufania serwera Proxy** podać poświadczenia administracyjne do konfigurowania zaufania serwera proxy i uzyskać dostęp do serwera podstawowego w farmie usług AD FS.
![Poświadczenia konta zaufania serwera proxy](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. Na stronie **gotowy do skonfigurowania** Kreator wyświetli listę akcji, które zostaną wykonane.
![Gotowy do skonfigurowania](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. Kliknij przycisk **Zainstaluj** , aby zakończyć konfigurację. Po zakończeniu konfiguracji Kreator udostępnia opcję, aby sprawdzić łączność z serwerami. Kliknij przycisk **Weryfikuj** , aby sprawdzić połączenie.
![Zakończenie instalacji](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### Dodawanie domeny federacyjnych<a name=addfeddomain></a>

Jest proste dodać domenę, aby być Federacji Azure AD przy użyciu Azure AD Connect. Narzędzie Azure AD Connect powoduje dodanie domeny do Federacji i modyfikowanie reguł oświadczeń do wystawcy odzwierciedla poprawnie, gdy masz wiele domen federacji z usługą Azure Active Directory.

1. Aby dodać domenę federacyjnych, zaznacz zadanie **dodać dodatkowe domeny Azure AD**.
![Dodatkowe domeny Azure AD](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. Na następnej stronie kreatora należy podać poświadczenia administratora globalnego dla Azure AD.
![Nawiązywanie połączenia z Azure AD](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. Na stronie **poświadczenia dostępu zdalnego** poświadczenia domeny administratora.
![Poświadczenia dostępu zdalnego](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. Na następnej stronie kreatora zawiera listę domen Azure AD przy użyciu których można utworzyć Federację katalogu lokalnego. Wybierz domenę z listy.
![Azure AD domeny](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    Po wybraniu domeny Kreatora umożliwi odpowiednie informacje dotyczące dalsze akcje, które będą miały kreatora i wpływu konfiguracji. W niektórych przypadkach jeśli zaznacz domenę, która nie została jeszcze zweryfikowana w Azure AD kreator umożliwi informacje ułatwiające zweryfikuj domenę. Aby uzyskać więcej informacji, zobacz temat [Dodawanie niestandardowej nazwy domeny do usługi Azure Active Directory](active-directory-add-domain.md) .

5. Kliknij przycisk **Dalej**i **rozpocząć konfigurowanie** stronie zostanie wyświetlona na liście Akcje, które wykonują Azure AD Connect. Kliknij przycisk **Zainstaluj** , aby zakończyć konfigurację.
![Gotowy do skonfigurowania](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>AD FS dostosowywania

W poniższych sekcjach przedstawiono szczegółowe informacje o niektórych typowych zadań, które należy wykonać podczas dostosowywania strony logowania usług AD FS.

### Dodawanie logo firmy niestandardowej lub ilustracji<a name=customlogo></a>

Aby zmienić logo firmy, która jest wyświetlana na stronie **logowania** , użyj następującego polecenia cmdlet programu Windows PowerShell i składni.

> [AZURE.NOTE] Zalecane wymiary logo są 260 x 35 @ 96 dpi o rozmiarze pliku nie przekracza 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] Parametr *Nazwa_obiektu_docelowego* jest wymagany. Motyw domyślny zwolnienia z usług AD FS nosi nazwę domyślną.


### Dodawanie opisu logowania<a name=addsignindescription></a>

Aby dodać opis strona logowania do **strony logowania**, użyj następującego polecenia cmdlet programu Windows PowerShell i składni.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### Modyfikowanie reguł roszczeń AD FS<a name=modclaims></a>

Usług AD FS obsługuje język sformatowanego roszczeń, który umożliwia tworzenie reguły niestandardowej zastrzeżenie. Aby uzyskać więcej informacji zobacz [Roli języka reguły zastrzeżenie](https://technet.microsoft.com/library/dd807118.aspx).

W poniższych sekcjach opisano, jak pisać niestandardowych reguł w niektórych scenariuszach dotyczących dotyczące Azure AD i Federacja usług AD FS.

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Identyfikator niezmienne warunkowe na wartość atrybutu obecności

Narzędzie Azure AD Connect umożliwia określenie atrybut może być używany jako kotwica źródła, gdy obiekty są synchronizowane z Azure AD. Wartość atrybutu niestandardowe nie jest puste, warto problemu niezmienne roszczeń identyfikator. Na przykład możesz jako atrybut kotwicy źródła wybierz **ms-ds-consistencyguid** i chcesz problemu **ImmutableID** jako **ms-ds-consistencyguid** w przypadku, gdy atrybut ma wartość przed nim. W przypadku żadnej wartości dla atrybutu problemu **objectGuid** jako niezmienne identyfikatora.  Można utworzyć zbioru reguł niestandardowych roszczeń, zgodnie z opisem w poniższej sekcji.

**Reguły 1: Atrybuty kwerendy**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

W tej reguły wyszukując wartości **ms-ds-consistencyguid** i **objectGuid** dla użytkownika z usługi Active Directory. Zmień nazwę Sklepu nazwa odpowiedniego sklepu w wdrożenia usług AD FS. Również zmienić typ roszczeń na oświadczeniach pisane z wielkiej litery typ usługi federacyjne określonych **objectGuid** i **ms-ds-consistencyguid**.

Ponadto przy użyciu **Dodawanie** i **problem**, nie dodawaj wychodzących problem dla obiektu i można użyć wartości jako wartości pośrednie. Będą wydawać roszczeń w regule później, po ustawieniu wartość, która ma zostać użyte jako niezmienne identyfikatora.

**Reguła 2: Sprawdź, czy ms-ds-consistencyguid istnieje dla użytkownika**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Jest to reguła definiująca tymczasowe flagę o nazwie **idflag** jest ustawiona na **useguid** , jeśli istnieje nie **ms-ds-concistencyguid** wypełnione dla użytkownika. Logiki to jest fakt, że usług AD FS nie zezwalają na oświadczeniach puste. Dlatego po dodaniu roszczeń http://contoso.com/ws/2016/02/identity/claims/objectguid i http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid w zasadzie 1 możesz zakończenia w górę z **msdsconsistencyguid** rościć sobie tylko jeśli wartość jest pusta dla użytkownika. Jeśli nie została wpisana, usług AD FS widzi ma wartość pustą i umieści ją natychmiast. Wszystkie obiekty będzie miała **objectGuid**, dlatego Opinia ta będzie zawsze dostępne po wykonaniu 1 reguły.

**Reguła 3: Problemu ms-ds-consistencyguid jako identyfikator niezmienne ma**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

To jest niejawne wyboru **istnieją** . Jeśli istnieje wartość roszczenia, następnie problemu który jako niezmienne identyfikatora. W poprzednim przykładzie użyto roszczeń **nameidentifier** . Należy zmieniać typu roszczeń odpowiednie dla niezmienne identyfikator w środowisku.

**Zasada 4: Problemu objectGuid jako niezmienne ID, jeśli brakuje ms-ds-consistencyGuid**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

W tej reguły są po prostu sprawdzanie tymczasowe flagi **idflag**. Możesz zdecydować, czy problem roszczeń, w zależności od ich wartości.

> [AZURE.NOTE] Ważne jest kolejność tych reguł.

#### <a name="sso-with-a-subdomain-upn"></a>Usługa rejestracji Jednokrotnej z poddomeny głównej nazwy użytkownika

Możesz dodać więcej niż jedną domenę Federacja jest za pomocą narzędzie Azure AD Connect, zgodnie z opisem w [dodać nową domenę federacyjnych](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain). Należy zmodyfikować oświadczeń głównej nazwy użytkownika, aby identyfikator wystawcy odpowiada domeny głównej, a nie poddomeny, ponieważ domeny głównej federacyjnych obejmuje także podrzędne.

Dla reguły roszczeń identyfikator wystawcy domyślnie jako:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Roszczeń identyfikator wystawcy domyślne](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

Reguły domyślnej po prostu przejście sufiksu głównej nazwy użytkownika i używany w oświadczeń identyfikator wystawcy. Na przykład Jan jest użytkownikiem w sub.contoso.com i contoso.com federacji z usługą Azure Active Directory. Wprowadza Jan john@sub.contoso.com jako nazwę użytkownika podczas logowania się w usłudze Azure AD i identyfikator wystawcy domyślne rościć sobie reguły w programie AD FS obsługuje ją w następujący sposób.

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Rościć sobie wartość:** http://sub.contoso.com/adfs/services/trust/

Aby zachować tylko domeny głównej wartość roszczeń wystawcy, zmień reguła roszczeń zgodne z następującymi.

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat [opcji logowania użytkownika](active-directory-aadconnect-user-signin.md).
