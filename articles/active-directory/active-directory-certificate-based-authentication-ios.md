<properties 
    pageTitle="Wprowadzenie do uwierzytelniania certyfikatu opartego na iOS | Microsoft Azure" 
    description="Dowiedz się, jak skonfigurować uwierzytelnianie certyfikatu opartego w rozwiązań z urządzeniami z systemem iOS" 
    services="active-directory" 
    authors="MarkusVi"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="10/21/2016" 
    ms.author="markvi" />



# <a name="get-started-with-certificate-based-authentication-on-ios---public-preview"></a>Wprowadzenie do uwierzytelniania certyfikatu opartego na iOS — Public Preview

> [AZURE.SELECTOR]
- [iOS](active-directory-certificate-based-authentication-ios.md)
- [Android](active-directory-certificate-based-authentication-android.md)


W tym temacie przedstawiono sposób konfigurowania i korzystania certyfikat oparty uwierzytelniania (CBA) na urządzeniu z systemem iOS dla użytkowników z dzierżawami w planach usługi Office 365 Enterprise, firm i instytucji edukacyjnych. 

CBA umożliwia uwierzytelnione przez usługi Azure Active Directory z certyfikatem klienta na urządzeniu z systemem Android lub iOS podczas nawiązywania połączenia z kontem usługi Exchange online do: 

- Przenośnych aplikacji pakietu Office, takich jak program Microsoft Outlook i Microsoft Word   
- Klienci programu Exchange ActiveSync (EAS) 

Konfigurowanie ta funkcja eliminuje potrzebę Wprowadź kombinację nazwy użytkownika i hasła w niektórych poczty oraz w aplikacjach Microsoft Office na urządzeniu przenośnym. 
 

## <a name="supported-scenarios-and-requirements"></a>Scenariusze obsługiwane i wymagania  



### <a name="general-requirements"></a>Ogólne wymagania 


W przypadku wszystkich scenariuszy w tym temacie wymagane są następujące zadania:  

- Dostęp do certyfikatu authority(s) do wydawania certyfikatów klienta.  

- Certyfikaty authority(s) musi być skonfigurowany w usłudze Azure Active Directory. Znajdować można uzyskać szczegółowe instrukcje na temat zakończyć konfigurację w sekcji [Wprowadzenie](#getting-started) .  

- Główny urząd certyfikacji i żadnych urzędów certyfikacji pośrednie musi być skonfigurowany w usłudze Azure Active Directory.  

- Każdy urząd certyfikacji musi być listy odwołań certyfikatów (CRL), który można odwoływać się przez Internet przeciwległych adresu URL.  

- Certyfikat klienta musi wydany uwierzytelniania klienta.  


- Dotyczy tylko klientów programu Exchange ActiveSync certyfikat klienta musi mieć adres routingu poczty e-mail użytkownika w usłudze Exchange online w głównej nazwy lub wartości z pola Nazwa RFC822 pola alternatywna nazwa podmiotu. Azure Active Directory mapy wartość RFC822 atrybut adres serwera Proxy w katalogu.  



### <a name="office-mobile-applications-support"></a>Obsługa aplikacji dla urządzeń przenośnych pakietu Office 

| Aplikacje                      | Pomoc techniczna      |
| ---                       | ---          |
| W programie Word / w programie Excel i PowerPoint | ![Sprawdzanie][1]  |
| Program OneNote                   | ![Sprawdzanie][1]  |
| Usługi OneDrive                  | ![Sprawdzanie][1]  |
| Program Outlook                   | ![Sprawdzanie][1]  |
| Usługi Yammer                    | ![Sprawdzanie][1]  |
| Program Skype dla firm        | Wkrótce  |


### <a name="requirements"></a>Wymagania dotyczące  

Wymaga wersji urządzenia z systemem operacyjnym iOS 9 i powyżej 

Serwer federacyjny musi być skonfigurowany.  

Azure uwierzytelnienia jest wymagany dla aplikacji pakietu Office w systemie iOS.  

Dla usługi Azure Active Directory do odwołania certyfikatu klienta ADFS token musi mieć następujące oświadczeń:  

  - `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
(Liczba kolejna certyfikat klienta) 

  - `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
(Ciąg wystawcy certyfikatu klienta) 

Azure Active Directory dodaje tych roszczeń do tokenu odświeżania, jeśli są one dostępne w ADFS token (lub innych token SAML). Gdy token odświeżania musi być sprawdzana poprawność, te informacje jest używana do sprawdzania odwołania. 

Zgodnie z zaleceniami dotyczącymi należy zaktualizować stron błędów ADFS z następujących czynności:

- Wymagania dotyczące instalowania uwierzytelnienia Azure w systemie iOS

- Instrukcje dotyczące uzyskiwania certyfikatu użytkownika. 

Aby uzyskać więcej informacji zobacz [Dostosowywanie AD FS logowaniem stron](https://technet.microsoft.com/library/dn280950.aspx).  



### <a name="exchange-activesync-clients-support"></a>Obsługa klientów programu Exchange ActiveSync 


W systemie iOS 9 lub nowsza klienta poczty systemu iOS natywnych jest obsługiwana. Dla wszystkich innych aplikacji w programie Exchange ActiveSync Aby określić, jeśli ta funkcja jest obsługiwana, skontaktuj się z deweloperem.  



## <a name="getting-started"></a>Wprowadzenie 


Aby rozpocząć pracę, musisz skonfigurować urzędów certyfikacji w usłudze Azure Active Directory. Dla każdego urzędu certyfikacji należy przekazać następujące czynności: 

- Części publicznej certyfikatu, w obszarze format *cer* 

- Internet przeciwległych adresy URL, w którym znajdują się odwołania list certyfikatów (CRL)
 

Poniżej przedstawiono schemat urząd certyfikacji: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 


Aby przekazać informacje, można użyć modułu Azure AD przy użyciu programu Windows PowerShell.  
Poniżej przedstawiono przykłady Dodawanie, usuwanie lub modyfikowanie urząd certyfikacji. 



### <a name="configuring-your-azure-ad-tenant-for-certificate-based-authentication"></a>Konfigurowanie dzierżawy usługi Azure AD dla certyfikatu uwierzytelniania opartego na 

1. Uruchom program Windows PowerShell z uprawnieniami administratora. 

2. Zainstaluj moduł usługi Azure AD. Należy zainstalować wersję [1.1.143.0](http://www.powershellgallery.com/packages/AzureADPreview/1.1.143.0) lub nowszym.  

        Install-Module -Name AzureADPreview –RequiredVersion 1.1.143.0 

3. Połącz się z dzierżawcą docelowej: 

        Connect-AzureAD 

### <a name="adding-a-new-certificate-authority"></a>Dodawanie nowego urząd certyfikacji

1. Ustaw różne właściwości urząd certyfikacji i dodać go do usługi Azure Active Directory: 

        $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
        $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
        $new_ca.AuthorityType=0 
        $new_ca.TrustedCertificate=$cert 
        New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 

5. Uzyskiwanie urzędów certyfikacji: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="retrieving-the-list-certificate-authorities"></a>Pobieranie listy urzędów certyfikacji

Pobieranie urzędach certyfikacji przechowywanych w usłudze Active Directory platformy Azure dzierżawcy usługi: 

        Get-AzureADTrustedCertificateAuthority 


### <a name="removing-a-certificate-authority"></a>Usuwanie urząd certyfikacji

1.  Pobieranie urzędów certyfikacji: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Usuwanie certyfikatu dla urzędu certyfikacji: 

        Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiying-a-certificate-authority"></a>Modfiying urząd certyfikacji 

1.  Pobieranie urzędów certyfikacji: 

        $c=Get-AzureADTrustedCertificateAuthority 


2. Modyfikowanie właściwości na urząd certyfikacji: 

        $c[0].AuthorityType=1 

3. Ustaw **urząd certyfikacji**: 

        Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 




## <a name="testing-office-mobile-applications"></a>Testowanie aplikacji pakietu Office dla urządzeń przenośnych  

Aby przetestować certyfikatu uwierzytelniania w aplikacji pakietu Office mobile: 

1.  Na urządzeniu z systemem test należy zainstalować aplikacji pakietu Office mobile (np. OneDrive) ze sklepu App Store.

2.  Upewnij się, że na urządzeniu test zainicjowano certyfikat użytkownika. 

3.  Uruchom aplikację. 

4.  Wprowadź nazwę użytkownika, a następnie wybierz certyfikat użytkownika, którego chcesz użyć. 

Użytkownik powinien być pomyślnie zarejestrowany. 





## <a name="testing-exchange-activesync-client-applications"></a>Testowanie aplikacji klienta programu Exchange ActiveSync

Aby uzyskać dostęp do programu Exchange ActiveSync za pomocą certyfikatu opartego uwierzytelniania, profilu programu EAS zawierający certyfikat klienta musi być dostępne dla aplikacji. Profil EAS musi zawierać następujące informacje:

- Certyfikat użytkownika może być używany do uwierzytelniania 

- Punkt końcowy EAS musi znajdować się outlook.office365.com (Ta funkcja jest obecnie obsługiwane tylko w środowisku wielu dzierżawy online Exchange)

Profil programu EAS można skonfigurowane i umieszczana w urządzenia za pomocą wykorzystania MDM, na przykład Intune lub ręcznie umieszczając certyfikat w profilu EAS na urządzeniu.  

### <a name="testing-eas-client-applications-on-ios"></a>Testowanie aplikacji klienckich EAS w systemie iOS 

Aby przetestować certyfikatu uwierzytelniania dla aplikacji macierzystych poczty systemu iOS 9 lub nad: 

1.  Konfigurowanie profilu programu EAS, która spełnia wymagania powyżej. 

2.  Instalowanie profilu na urządzeniu iOS (albo za pomocą MDM, takich jak usługi lub aplikacji konfiguratora Apple)

3.  Gdy profil prawidłowo zostanie zainstalowana, otwórz natywnej aplikacji poczty i sprawdź, synchronizuje poczty



## <a name="revocation"></a>Odwołania

Aby odebrać certyfikat klienta, usługi Azure Active Directory pobiera listy odwołań certyfikatów (CRL) z przekazane jako część informacji urząd certyfikacji adresy URL i umieszcza go w pamięci podręcznej. Publikowanie ostatniego sygnatury (**Data wykorzystania** właściwość) listy odwołań certyfikatów jest używany do zapewnienia listy odwołań certyfikatów jest nadal ważna. Odwołuje się okresowo listy odwołań certyfikatów, aby odebrać dostęp do certyfikatów, które są częścią listy.

Jeśli odwołania więcej błyskawiczne jest wymagane (na przykład, jeśli użytkownik traci urządzenia), można unieważniane token autoryzacji użytkownika. Aby powodować token autoryzacji, ustaw pole **StsRefreshTokenValidFrom** dla tego określonego użytkownika, przy użyciu programu Windows PowerShell. Musisz zaktualizować pole **StsRefreshTokenValidFrom** dla każdego użytkownika, które chcesz odwołać dostęp.
 
Aby upewnić się, że odwołania będzie nadal występował, należy ustawić **Datą** listy odwołań certyfikatów na datę późniejszą od wartości określonych przez **StsRefreshTokenValidFrom** i upewnij się, certyfikat w danym znajduje się w listy odwołań certyfikatów.
 
Proces aktualizacji i unieważnieniu token autoryzacji przez ustawienie pola **StsRefreshTokenValidFrom** w konspekcie, następujące czynności. 

1. Nawiązywanie połączenia przy użyciu poświadczeń administratora usługi MSOL: 

        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

1.  Pobranie bieżącej wartości StsRefreshTokensValidFrom dla użytkownika: 

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 


1.  Skonfiguruj nową wartość StsRefreshTokensValidFrom dla użytkownika równa bieżącą sygnaturę czasową: 

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")


Datę, dla której ustawiono musi być datą w przyszłości. Jeśli daty nie znajduje się w przyszłości, nie jest ustawiona właściwość **StsRefreshTokensValidFrom** . W przypadku datę w przyszłości, **StsRefreshTokensValidFrom** zostaje ustawiona na bieżącą godzinę (nie datę wskazywana przez polecenie Set-MsolUser). 



<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png