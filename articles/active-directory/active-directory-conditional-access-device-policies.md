<properties
    pageTitle="Zasady urządzenia warunkowego dostępu dla usług Office 365 | Microsoft Azure"
    description="Szczegóły dotyczące warunków jak opartych na urządzeniach kontrolowanie dostępu do usługi Office 365. Gdy pracownicy przetwarzający informacje (IWs) mają dostęp do usługi Office 365 usługi, takie jak programu Exchange i SharePoint Online w pracy lub szkole ze swoich osobistych urządzeń, ich administrator IT chce programu access jako administratorzy secure.IT umożliwia obsługę dostępu warunkowego urządzenia zasady bezpiecznego zasobów firmy znajduje się w tym samym czasie, co pozwala IWs zgodny z urządzenia, aby uzyskać dostęp do usług."
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
# <a name="conditional-access-device-policies-for-office-365-services"></a>Zasady urządzenia warunkowego dostępu dla usług Office 365

Ten termin "warunkowe dostępu" ma wiele warunków skojarzone z nim, takich jak użytkownik uwierzytelniony wieloskładnikowe, uwierzytelniony urządzenia, urządzenie zgodne z itp. W tym temacie przede wszystkim focusses opartych na urządzeniach warunki kontrolowanie dostępu do usługi Office 365. Gdy pracownicy przetwarzający informacje (IWs) mają dostęp do usługi Office 365 usługi, takie jak programu Exchange i SharePoint Online w pracy lub szkole ze swoich osobistych urządzeń, ich administrator chce dostęp do zabezpieczania zawartości. Administratorzy mogą obsługi administracyjnej zasady urządzenia warunkowego dostępu do zabezpieczania zasobów firmy znajduje się w tym samym czasie, co pozwala IWs na zgodny z urządzenia, aby uzyskać dostęp do usług. Zasady warunkowe dostępu do usługi Office 365 może być skonfigurowany z portalu dostępu warunkowego Intune firmy Microsoft.

Azure Active Directory wymusza zasady dostępu warunkowego z bezpiecznego dostępu do usług Office 365. Administrator dzierżawy można utworzyć zasadę dostępu warunkowego, która blokuje użytkownika na urządzeniu niezgodne z uzyskiwania dostępu do usługi Office 365. Użytkownik musi spełniać polityki firmy urządzenie przed dostęp może być przyznany z usługą. Ewentualnie administrator również utworzyć zasadę, która wymaga użytkownikom tylko zarejestrowanie swoich urządzeń, aby uzyskać dostęp do usługi Office 365. Zasady może być stosowane do wszystkich użytkowników w organizacji, lub ograniczona do kilku grup docelowych i rozszerzony w czasie do uwzględnienia grup dodatkowe docelowych.

Wymagania wstępne wymuszania zasad urządzenie jest użytkowników do rejestrowania swoich urządzeń z usługą Azure Active Directory Rejestracja urządzenia. Możesz wybrać opcję Włącz uwierzytelnianie wieloskładnikowe (MFA) za przeprowadzenie rejestracji urządzenia z usługą Azure Active Directory Rejestracja urządzenia. MFA jest zalecane dla usługi Azure Active Directory urządzenia rejestracji. Po włączeniu MFA użytkowników zarejestrowanie swoich urządzeń z usługą Azure Active Directory Rejestracja urządzenia będą musiały dla drugiego uwierzytelniania współczynnik.

##<a name="how-does-conditional-access-policy-work"></a>Jak działa zasady dostępu warunkowego?

Gdy żądania dostępu użytkowników do usługi Office 365 z platformy obsługiwanego urządzenia, usługi Azure Active Directory uwierzytelnia użytkownika i urządzenie, z której użytkownik uruchamia wniosku; i dotacje dostęp do usługi tylko wtedy, gdy użytkownik odpowiada zasady ustawione przez usługę. Użytkowników, których nie ma urządzenia zarejestrowana otrzymuje naprawcze instrukcje dotyczące rejestrowania i była zgodna dostępu do usług firmy usługi Office 365. Użytkowników iOS i na urządzeniach z systemem Android będą musieli o zarejestrowanie swoich urządzeń za pomocą aplikacji portalu firmy. Gdy użytkownik rejestruje swojego urządzenia, urządzenie jest zarejestrowana z usługą Azure Active Directory i zarejestrować w celu zarządzania urządzeniami i zgodność. Klienci, należy użyć usługi Azure Active Directory urządzenia rejestracji w połączeniu z Microsoft Intune umożliwiający zarządzanie urządzeniami przenośnymi dla usługi Office 365. Rejestracja urządzenia jest wstępnie wymagane dla użytkowników usługi Office 365 dostęp do usług, gdy są wymuszane zasady urządzenia.

Gdy użytkownik rejestruje pomyślnie swojego urządzenia, urządzenie staje się zaufany. Usługa Azure Active Directory-jednokrotnej dostępu do firmy aplikacji i wymusza zasady dostępu warunkowego do udzielania dostępu do godziny usługa nie tylko pierwszy użytkownik zażąda dostępu, ale zawsze użytkownik zażąda odnowić programu access. Użytkownik będzie mieć dostępu do usług podczas poświadczeń logowania zostaną zmienione, urządzenie jest tracone kradzież lub zasady nie są spełnione w momencie żądania odnowienia.

## <a name="deployment-considerations"></a>Zagadnienia dotyczące rozmieszczania:
Należy użyć usługi Azure Active Directory Rejestracja urządzenia dla Rejestracja urządzenia.

Gdy użytkownicy uwierzytelniania w środowisku lokalnym, Active Directory Federation Services (AD FS) (1.0 i powyżej) jest wymagana. Uwierzytelnianie wieloskładnikowe, aby dołączyć do pracy zakończy się niepowodzeniem, gdy dostawcy tożsamości nie jest w stanie uwierzytelnianie wieloskładnikowe. Na przykład usług AD FS 2.0 nie jest wieloskładnikowe obsługuje uwierzytelniania. Administrator dzierżawy musi zapewnić, że w lokalnym usług AD FS jest MFA stanie i prawidłowej metody MFA jest włączona, przed włączeniem MFA usługi Azure Active Directory Rejestracja urządzenia. Na przykład usług AD FS w systemie Windows Server 2012 R2 ma możliwości MFA. Przed włączeniem MFA usługi Azure Active Directory Rejestracja urządzenia, musisz również włączyć metody uwierzytelniania prawidłowe dodatkowe (MFA) na serwerze usług AD FS. Aby uzyskać więcej informacji o obsługiwanych metodach MFA usług AD FS zobacz Konfigurowanie dodatkowe metody uwierzytelniania dla usług AD FS.

## <a name="frequently-asked-questions-faq"></a>Często zadawane pytania

P: co to jest domyślną zasadę wykluczeń dla platform nieobsługiwane urządzenie?

O: w danej chwili zasady dostępu warunkowego selektywne są wymuszane dla użytkowników iOS i na urządzeniach z systemem Android. Aplikacje na innych platformach urządzenia są domyślnie nie mają wpływu zasady dostępu warunkowego dla systemów iOS i urządzeń z systemem Android. Administrator dzierżawy jednak wybrać zastępują zasady globalny nie zezwalaj użytkownikom na platformach nieobsługiwane.
Ta funkcja jest włączona plan, aby rozszerzyć zasady warunkowe dostępu użytkownikom na innych platformach, łącznie z systemem Windows.

P: po zostaną zasady warunkowego dostępu do usług Office 365 rozszerzone do aplikacji przeglądarce (na przykład aplikacji OWA, oparte na przeglądarce programu SharePoint).

O: w danej chwili warunkowego dostępu do usług usługi Office 365 jest ograniczone do zaawansowanych aplikacji na urządzeniu. Ta funkcja jest włączona plan, aby rozszerzyć zasady warunkowego dostępu użytkownikom uzyskiwanie dostępu do usług przeglądarki.
