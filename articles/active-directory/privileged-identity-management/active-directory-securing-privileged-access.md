<properties
    pageTitle="Zabezpieczanie dostępu uprzywilejowanych w Azure AD | Microsoft Azure"
    description="Temat objaśniający metod do zabezpieczania dostępu uprzywilejowanych Azure, usługi Azure Active Directory i usług Microsoft Online Services."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="mwahl"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="kgremban"/>


# <a name="securing-privileged-access-in-azure-ad"></a>Zabezpieczanie dostępu uprzywilejowanych w Azure AD

Bezpieczeństwo uprzywilejowanych dostępu jest pierwszym krokiem krytyczna chronić wyposażenia firmy w nowoczesnym organizacji. Uprzywilejowanych kont to administrowanie i zarządzanie nimi systemów informatycznych. Ataki Cyber kierować tych kont, aby uzyskać dostęp do danych i systemów w organizacji. Aby zabezpieczyć uprzywilejowanych programu access, należy wyodrębnianie kont i systemów przed ryzykiem na złośliwy użytkownik.

Więcej użytkowników rozpoczęła się na uprzywilejowanych dostęp do usług w chmurze. Dotyczy to także globalnej administratorów usługi Office 365, Administratorzy Azure subskrypcji i użytkowników, którzy mają dostęp administracyjny maszyny wirtualne lub w aplikacjach władz akredytacji bezpieczeństwa.

Firma Microsoft zaleca, które obserwujesz ten przewodnik [Zabezpieczanie uprzywilejowanych](https://technet.microsoft.com/library/mt631194.aspx)dostępu.

Dla klientów korzystających z usługi Azure Active Directory zarządzać dostępem Azure, Office 365, lub innych usług firmy Microsoft i aplikacje czy konta użytkowników są zarządzane i uwierzytelnianie w usłudze Active Directory lub w usłudze Active Directory platformy Azure stosuje się te zasady. W poniższych sekcjach przedstawiono więcej informacji na temat funkcji Azure AD do obsługi bezpieczeństwo uprzywilejowanych dostępu.

## <a name="multi-factor-authentication"></a>Uwierzytelnianie wieloskładnikowe

Aby zwiększyć bezpieczeństwo uwierzytelniania administratora, czy wymagają uwierzytelnianie wieloskładnikowe (MFA) przed przyznaniem uprawnień. MFA jest metodę weryfikacji kim jesteś, która wymaga korzystania z więcej niż tylko nazwę użytkownika i hasło. Zawiera drugą warstwę zabezpieczeń dodatki logowania użytkownika i transakcji.

Azure uwierzytelnianie wieloskładnikowe ułatwia ochronę informacji dostęp do danych i aplikacji podczas spotkania żądanie użytkownika prosty proces logowania. Zapewnia silne uwierzytelnianie za pomocą zakresu opcje łatwe weryfikacji, łącznie z połączeń telefonicznych, wiadomości SMS, powiadomienia aplikacji dla urządzeń przenośnych lub kod weryfikacyjny i tokeny PRZYSIĘGĄ strona 3.

Jak działa uwierzytelnianie wieloskładnikowe Azure uzyskać w poniższym klipie wideo.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]

Aby uzyskać więcej informacji zobacz [MFA dla Office 365 i MFA dla Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Granica czasu uprawnień

Niektóre organizacje może się okazać, że zbyt wielu użytkowników w wysoce uprzywilejowanych ról. Użytkownik może zostały dodane do roli dla poszczególnych aktywności, takie jak do utworzenia konta usługi, ale nie często później użyć tych uprawnień.

Aby obniżyć poziom czasu Uwidacznianie uprawnień i zwiększyć jego widoczności do użytku, ograniczyć użytkownikom tylko określone na ich uprawnień tylko na czas (JIT), gdy są potrzebne do wykonania zadania. Dla usługi Azure Active Directory i usług Microsoft Online Services można użyć [Azure AD uprzywilejowanych tożsamości Zarządzanie](http://aka.ms/AzurePIM).


![Pulpit nawigacyjny PIM][2]


## <a name="attack-detection"></a>Wykrywanie atakiem

[Azure Active Directory ochronę tożsamości](../active-directory-identityprotection.md) Wyświetla skonsolidowanej na zdarzenia ryzyka i słabych wpływających na tożsamości Twojej organizacji. W zależności od zdarzenia ryzyka, ochronę tożsamości oblicza poziom ryzyka użytkownika dla każdego użytkownika, co umożliwia konfigurowanie zasad ryzyka automatycznie ochrony tożsamości organizacji. Te zasady oraz inne funkcje kontroli dostępu warunkowego dostarczony przez usługi Azure Active Directory i EMS, można automatycznie zablokować użytkownikowi lub oferują sugestie, zawierających resetowania hasła i uwierzytelnianie wieloskładnikowe wymuszania.

![Ochrona tożsamości Azure AD][3]

## <a name="conditional-access"></a>Dostępu warunkowego

Za pomocą pola Kontrola dostępu warunkowego usługi Azure Active Directory sprawdza określone warunki, wybrane podczas uwierzytelniania użytkownika, przed zezwoleniem na dostęp do aplikacji. Po tych warunki są spełnione, użytkownik uwierzytelniony i dostęp do aplikacji.


![Ustawianie reguł dostępu warunkowego z MFA][4]


## <a name="related-articles"></a>Artykuły pokrewne

- Włącz [uwierzytelnianie wieloskładnikowe Azure](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
- Włącz [Zarządzanie tożsamościami uprawnieniach Azure AD](../active-directory-privileged-identity-management-configure.md)
- Włącz [ochronę tożsamości Azure AD](../active-directory-identityprotection.md)
- Włączanie [dostępu warunkowego kontrolek](../active-directory-conditional-access.md)


Uzyskać więcej informacji dotyczących tworzenia planu pełnego bezpieczeństwa zobacz sekcję "obowiązki klienta i mapa drogowa" dokumentu [Program Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) . Aby uzyskać więcej informacji na atrakcyjnych usług firmy Microsoft, aby pomóc w dowolnej z następujących tematów skontaktuj się z przedstawicielem firmy Microsoft lub odwiedź naszą [Cybersecurity rozwiązań strony](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
