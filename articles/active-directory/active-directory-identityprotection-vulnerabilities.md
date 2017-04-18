<properties
    pageTitle="Luk wykryte przez Azure Active Directory ochronę tożsamości | Microsoft Azure"
    description="Omówienie luk wykryte przez Azure Active Directory ochronę tożsamości."
    services="active-directory"
    keywords="ochrona tożsamości usługi Azure active directory chmury aplikacji odnajdowanie, zarządzanie aplikacji, zabezpieczeń, czynnik ryzyka, poziom ryzyka, luka w zabezpieczeniach, zasady zabezpieczeń"
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
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Luk wykryte przez Azure Active Directory ochronę tożsamości 

Luk są słabych w środowisku, w którym można można wykorzystać. Zalecamy dotyczy tych luk, aby poprawić postawie zabezpieczeń Twojej organizacji i przez nieupoważnione ich wykorzystanie osoby.
<br><br>
![usterek](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")
<br>

W poniższych sekcjach przedstawiono omówienie luk zgłaszane przez ochronę tożsamości.

## <a name="multi-factor-authentication-registration-not-configured"></a>Uwierzytelnianie wieloskładnikowe rejestracji nieskonfigurowane 

Luka ta ułatwia sterowanie wdrażaniem uwierzytelnianie wieloskładnikowe Azure w Twojej organizacji. 

Uwierzytelnianie wieloskładnikowe Azure zawiera drugą warstwy zabezpieczeń uwierzytelniania użytkowników. Jego pomaga zabezpieczyć dostęp do danych i aplikacji podczas spotkania żądanie użytkownika prosty proces logowania. Zapewnia silne uwierzytelnianie za pomocą zakresu opcje weryfikacji łatwe — rozmowy telefonicznej, SMS lub aplikacji dla urządzeń przenośnych powiadomienia lub weryfikacji kod i 3rd firmy PRZYSIĘGĄ tokeny.

Zalecane jest wymagane uwierzytelnianie wieloskładnikowe Azure dla dodatki logowania użytkownika. Uwierzytelnianie wieloskładnikowe odtwarzany kluczową rolę w zasadach ryzyka warunkowego dostępu dostępne za pośrednictwem ochronę tożsamości.

Aby uzyskać więcej informacji, zobacz [Co to jest Azure uwierzytelnianie wieloskładnikowe?](../multi-factor-authentication/multi-factor-authentication.md)


## <a name="unmanaged-cloud-apps"></a>Aplikacje niezarządzanej chmury

Luka ta pomaga zidentyfikować chmury niezarządzanej aplikacji w Twojej organizacji.
 
W nowoczesnym przedsiębiorstw działów informatycznych są często nie wszystkie aplikacje korzystające z użytkowników w organizacji w pracy w chmurze. Jest proste sprawdzić, dlaczego Administratorzy woli wątpliwości nieautoryzowanego dostępu do danych firmowych, wycieku danych i inne ryzyko związane z zabezpieczeniami. 

Zaleca się, że Twoja organizacja wdrażanie chmury odnajdowania aplikacji do znalezienia aplikacje w chmurze niezarządzanej i zarządzanie te aplikacje przy użyciu usługi Azure Active Directory.

Aby uzyskać więcej informacji zobacz [Znajdowanie aplikacje w chmurze niezarządzanej z chmury odnajdowania aplikacji](active-directory-cloudappdiscovery-whatis.md).



##<a name="security-alerts-from-privileged-identity-management"></a>Alerty zabezpieczeń w kierownictwa uprzywilejowanych tożsamości

Luka ta ułatwia odnajdowanie i rozwiązywanie alertów dotyczących uprzywilejowanych tożsamości w Twojej organizacji.  

Aby umożliwić użytkownikom do wykonywania operacji uprzywilejowanych, organizacje muszą udostępnić użytkownikom tymczasowe lub stałe uprzywilejowanych w Azure AD zasobów Azure lub usługi Office 365 lub inne aplikacje władz akredytacji bezpieczeństwa. Każdy z tych użytkowników uprzywilejowanych zwiększa powierzchni atakiem Twojej organizacji. Luka ta umożliwia określenie użytkowników z dostępem uprzywilejowanych niepotrzebne i wykonać odpowiednie kroki w celu zmniejszenia lub wyeliminowania ryzyka, które stanowią one. 

Zaleca się Azure AD uprzywilejowanych Zarządzanie tożsamościami w organizacji jest używany do zarządzania sterowanie i monitorowanie uprzywilejowane tożsamości i ich dostęp do zasobów w Azure AD, a także inne usługi online firmy Microsoft, takich jak usługi Office 365 lub Intune firmy Microsoft.

Aby uzyskać więcej informacji zobacz [Zarządzanie tożsamościami Azure AD uprzywilejowanych](active-directory-privileged-identity-management-configure.md). 



## <a name="see-also"></a>Zobacz też

 - [Ochrona tożsamości Azure Active Directory](active-directory-identityprotection.md)
