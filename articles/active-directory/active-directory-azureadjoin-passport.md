<properties
    pageTitle="Uwierzytelnianie tożsamości bez hasła za pomocą Microsoft Passport | Microsoft Azure"
    description="Omówienie Microsoft Passport oraz dodatkowe informacje na temat wdrażania Microsoft Passport."
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

# <a name="authenticating-identities-without-passwords-through-microsoft-passport"></a>Uwierzytelnianie tożsamości bez hasła za pomocą Microsoft Passport

Bieżącej metody uwierzytelniania za pomocą hasła tylko nie są wystarczające, aby zapewnić bezpieczne użytkowników. Użytkownicy ponowne używanie i nie pamięta hasła. W hasłach jest breachable, phishable, podatne na pęknięcia i odgadnięcia. Są również wyświetlić trudne do zapamiętania i podatne na ataki, takie jak "[zaliczone skrótu](https://technet.microsoft.com/dn785092.aspx)".

## <a name="about-microsoft-passport"></a>Usługa Microsoft Passport — informacje
Microsoft Passport to klucza publicznego i prywatnego lub metody uwierzytelniania opartego na certyfikat dla organizacji i konsumentów wykracza poza hasła. Ten formularz uwierzytelniania zależy od poświadczeń parę kluczy, które można zastąpić haseł i na naruszenia, thefts i wyłudzaniu informacji.

 Microsoft Passport pozwala uwierzytelnienia do konta Microsoft, konto usługi Active Directory systemu Windows Server, konto Microsoft Azure Active Directory (Azure AD) lub z usługi firmy Microsoft, która obsługuje uwierzytelnianie szybkie tożsamości Online (FIDO). Po początkowej potrzeby weryfikacji dwuetapowej podczas rejestrowania Microsoft Passport Microsoft Passport jest skonfigurowana na urządzeniu użytkownika, a użytkownik ustawia gest, który może być Witaj systemu Windows lub numeru PIN. Użytkownik udostępnia gest w celu zweryfikowania jego tożsamości. System Windows używa następnie Microsoft Passport do uwierzytelnienia użytkownika i ułatwić dostęp do chronionych zasobów i usług.

Klucz prywatny jest udostępniana wyłącznie za pośrednictwem "gest użytkownika" przykład numeru PIN, biometria lub urządzenie zdalne, takich jak karty inteligentnej, używaną przez użytkownika do zalogowania się do urządzenia. Te informacje jest połączony z certyfikatu lub asymetryczne parę kluczy. Klucz prywatny jest sprzęt potwierdzone jeśli urządzenie ma układ moduł zaufanej platformy (TPM). Klucz prywatny nigdy nie pozostawia urządzenia.

Klucz publiczny jest zarejestrowany z usługi Azure Active Directory i systemu Windows Server Active Directory (dla lokalnego). Dostawcy tożsamości (IDPs) sprawdzania poprawności użytkownika przez mapowanie kluczem publicznym użytkownika na klucz prywatny, a następnie wprowadź informacje logowania za pośrednictwem jednego hasła czasu (OTP), PhoneFactor lub mechanizmu powiadomienie o różnych.

## <a name="why-enterprises-should-adopt-microsoft-passport"></a>Dlaczego przedsiębiorstw powinna przyjąć Microsoft Passport

Po włączeniu Microsoft Passport, przedsiębiorstw może zabezpieczyć ich zasobów nawet przez:

* Ustawianie Microsoft Passport za pomocą opcji preferowane sprzętu. Oznacza to, że będą generowane klucze na TPM 1.2 lub 2.0 TPM, gdy są dostępne. Gdy TPM nie jest dostępna, oprogramowanie wygeneruje klucz.

* Definiowanie złożoności i długości numeru PIN, i czy zastosowania powitania jest włączona w Twojej organizacji.

* Konfigurowanie Microsoft Passport do obsługi scenariuszy, jak karty inteligentnej przy użyciu na podstawie certyfikatu zaufania.

## <a name="how-microsoft-passport-works"></a>Jak działa Microsoft Passport
1. Klucze są generowane za pomocą sprzętu TPM lub oprogramowania. Wiele urządzeń mają wbudowane Układ TPM, który zabezpiecza sprzętu przez integrowanie klucze szyfrowania urządzenia. TPM 1.2 lub TPM 2.0 umożliwia generowanie kluczy lub certyfikatów, które są tworzone na podstawie wygenerowanych klawiszy.

2. Moduł TPM zaświadcza klucze związanych ze sprzętem.

3. Gest pojedynczy Odblokuj odblokowanie urządzenia. Ten gest umożliwia dostęp do wielu zasobów Jeśli urządzenie jest domeny lub Azure AD połączonych.

## <a name="how-the-microsoft-passport-lifecycle-works"></a>Jak działa cyklu życia Microsoft Passport

![Cykl życia Microsoft Passport](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Poprzedni diagram przedstawia pary kluczy publicznego i prywatnego i sprawdzanie poprawności przez dostawcę tożsamości. Każdy z tych kroków objaśniono tutaj:

1. Użytkownik jest jego tożsamości za pomocą wielu narzędzi sprawdzających metod wbudowanych (gestów, fizycznie kart inteligentnych, uwierzytelnianie wieloskładnikowe) i wysyła te informacje do tożsamości dostawcy (protokołu IDP) takich jak usługi Azure Active Directory lub lokalnej usługi Active Directory.

2. Urządzenie tworzy klucz, zaświadcza klucz, trwa publicznej części tego klucza, dołącza go w instrukcjach stacji, znaki w i przesyła go do protokołu IDP zarejestrować klucz.

4. Po zainicjowaniu protokołu IDP rejestruje publicznej części klucza, protokołu IDP wzywa Zaloguj się za pomocą prywatne część klucza na tym urządzeniu.

5. Następnie protokołu IDP sprawdza i problemy tokenu uwierzytelniania, który pozwala użytkownikowi i dostęp do urządzeń chronionych zasobów. IDPs można napisać aplikacje i platform lub tworzenie i używanie poświadczeń Microsoft Passport dla swoich użytkowników za pomocą obsługi przeglądarki (za pośrednictwem interfejsów API języka JavaScript/Webcrypto).

## <a name="the-deployment-requirements-for-microsoft-passport"></a>Wymagania dotyczące rozmieszczania dla Microsoft Passport
### <a name="at-the-enterprise-level"></a>Na poziomie organizacji

* Przedsiębiorstwa ma subskrypcję usługi Azure.

### <a name="at-the-user-level"></a>Na poziomie użytkownika

* Na komputerze użytkownika działa system Windows 10 Professional lub Enterprise.

Wdrożenie szczegółowe instrukcje zobacz [Włączanie usługi Microsoft Passport do pracy w organizacji](active-directory-azureadjoin-passport-deployment.md).


## <a name="additional-information"></a>Dodatkowe informacje

* [Windows 10 w przedsiębiorstwie: sposoby za pomocą urządzenia do pracy](active-directory-azureadjoin-windows10-devices-overview.md)
* [Rozszerzanie możliwości chmury urządzeń systemu Windows 10 za pośrednictwem Azure Active Directory dołączanie](active-directory-azureadjoin-user-upgrade.md)
* [Informacje na temat scenariusze użycia dla Azure AD dołączanie](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Podłącz urządzenia domeny do Azure AD dla środowiska systemu Windows 10](active-directory-azureadjoin-devices-group-policy.md)
* [Konfigurowanie Azure AD dołączanie](active-directory-azureadjoin-setup.md)
