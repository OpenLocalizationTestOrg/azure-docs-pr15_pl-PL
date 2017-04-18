
<properties
    pageTitle="Informacje techniczne usługi Azure Active Directory dostępu warunkowego | Microsoft Azure"
    description="Za pomocą pola Kontrola dostępu warunkowego usługi Azure Active Directory sprawdza określone warunki wybierz podczas uwierzytelniania użytkownika i przed zezwoleniem na dostęp do aplikacji. Po tych warunki są spełnione, użytkownik uwierzytelniony i dostęp do aplikacji."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-technical-reference"></a>Informacje techniczne usługi Azure Active Directory warunkowego dostępu

## <a name="services-enabled-with-conditional-access"></a>Włączone z dostępem warunkowym usługi
Reguły dostępu warunkowego są obsługiwane przez różne typy aplikacji Azure AD. Ta lista zawiera:

- Aplikacji federacyjnych z galerii aplikacji Azure AD
- Hasło aplikacji rejestracji Jednokrotnej z galerii aplikacji Azure AD
- Zarejestrowanych aplikacji z serwera Proxy aplikacji Azure
- Rozwinięte linii biznesowych i aplikacji wielu dzierżawy zarejestrowane z usługą Azure Active Directory
- Program Visual Studio w trybie Online
- Aplikacja Remote Azure
-   Dynamics CRM
- Microsoft Office 365 usługi Yammer
- Microsoft Office 365 Exchange Online
- Microsoft Office 365 SharePoint Online (w tym OneDrive dla firm)


## <a name="enable-access-rules"></a>Włączanie reguły dostępu

Każdą regułę można włączać lub wyłączać dla na podstawach aplikacji. Gdy reguły są **włączone** będzie być włączone i wymuszone użytkownikom uzyskiwanie dostępu do aplikacji. Są one **wyłączone** nie będą używane i nie będzie miało wpływ wygody rejestrowania użytkowników.

## <a name="applying-rules-to-specific-users"></a>Stosowanie reguł do określonych użytkowników
Reguły można stosować do określonych zestawów według grupy zabezpieczeń, ustawiając **Zastosuj do**użytkowników. **Zastosuj do** można ustawić do **Wszystkich użytkowników** lub **grup**. Po ustawieniu **Wszystkim** użytkownikom zasad zostaną zastosowane do dowolnego użytkownika z dostępem do aplikacji. Opcja **grup** umożliwia zabezpieczeń i grupy dystrybucyjne, należy wybrać reguły będą wymuszane tylko dla tych grup.

Wdrażając reguły są często najpierw stosowanie go ograniczony zestaw użytkowników, które są członkami grupy piloting. Po zakończeniu można stosować reguły do **Wszystkich użytkowników**. Spowoduje to regułę, aby wymuszane dla wszystkich użytkowników w organizacji.

Wybierz pozycję grupy również mogą być wyłączone z zasad za pomocą opcji **z wyjątkiem** . Wszystkich członków grupy zostanie wyłączone, nawet jeśli znajdują się w grupie uwzględniane.

## <a name="at-work-networks"></a>"W miejscu pracy" sieci


Reguły dostępu warunkowego, które "w miejscu pracy" sieci zależne od zaufanych zakresy adresów IP, które zostały skonfigurowane w Azure AD lub użyj roszczenia "wewnątrz corpnet" z usług AD FS. Te reguły obejmują:

- Wymaganie uwierzytelnianie wieloskładnikowe, gdy nie w pracy
- Zablokuj dostęp, gdy nie w pracy

Opcje podając sieci "w miejscu pracy"

1. Konfigurowanie zaufanych zakresy adresów IP w [uwierzytelnianie wieloskładnikowe strony konfiguracji](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Warunkowe zasady dostępu przy użyciu skonfigurowanych zakresów na każdym żądanie uwierzytelnienia i token wydania oceny reguł. 
2. Skonfiguruj używanie wewnątrz rościć sobie corpnet tę opcję, można używać z katalogów federacyjnych, za pomocą usług AD FS. [Dowiedzieć się więcej o wewnątrz roszczeń coronet](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).
3. Konfigurowanie publicznej zakresy adresów IP. Na karcie Konfigurowanie katalogu, możesz ustawić publicznych adresów IP. Warunkowe Access użyje tych "w miejscu pracy" adresy IP, dzięki temu dodatkowe zakresy, aby skonfigurować, powyżej 50 limit adres IP jest wymuszane przez strony ustawień MFA.



## <a name="rules-based-on-application-sensitivity"></a>Reguły oparte na charakter aplikacji

Reguły są skonfigurowane dla każdego aplikacji zezwalania usług wysokiej wartości zapewniane bez wpływania dostępu do innych usług. Na karcie **Konfigurowanie** aplikacji można skonfigurować reguły dostępu warunkowego. 

Reguły dostępne:

- **Wymaga uwierzytelniania wieloskładnikowego**
 - Wszyscy użytkownicy, którzy tej zasady są stosowane do będzie wymagane do uwierzytelniania za pośrednictwem uwierzytelnianie wieloskładnikowe co najmniej raz.
 
- **Wymaganie uwierzytelnianie wieloskładnikowe, gdy nie w pracy**
 - Jeśli ta zasada jest stosowana, wszyscy użytkownicy będzie wymagane zlecić uwierzytelnianie wieloskładnikowe co najmniej raz, jeśli dostępu do usługi z lokalizacji zdalnej bez pracy. Jeśli zostały przeniesione z pracy do lokalizacji zdalnej muszą przeprowadzić uwierzytelnianie wieloskładnikowe podczas uzyskiwania dostępu do usługi.
 
- **Zablokuj dostęp, gdy nie w pracy** 
 - Gdy użytkownicy przenoszą z pracy do zdalnie, ich zostaną zablokowane, jeśli zasady "Zablokować dostęp, gdy nie w pracy" są stosowane do nich.  Ich ponownie mogą uzyskiwać dostęp podczas w miejscu pracy.


## <a name="related-topics"></a>Tematy pokrewne

- [Zabezpieczanie dostępu do usługi Office 365 i inne aplikacje połączony z usługi Azure Active Directory](active-directory-conditional-access.md)
- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
