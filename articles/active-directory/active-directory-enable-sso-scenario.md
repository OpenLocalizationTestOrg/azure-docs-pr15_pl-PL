<properties
    pageTitle="Zarządzanie aplikacjami z usługą Azure Active Directory | Microsoft Azure"
    description="Ten artykuł korzyści wynikających z integracji usługi Azure Active Directory z lokalnego, chmura i aplikacjami władz akredytacji bezpieczeństwa."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/10/2016"
      ms.author="markvi"/>

# <a name="managing-applications-with-azure-active-directory"></a>Zarządzanie aplikacjami z usługą Azure Active Directory

Poza faktyczne lub zawartości firmy występują dwa podstawowe wymagania dla wszystkich aplikacji:

1. Aby zwiększyć wydajność, należy ułatwia odnajdowanie i uzyskać dostęp do aplikacji

2. Aby włączyć zabezpieczeń i zarządzania, organizacji wymaga kontroli i nadzoru na kto może i faktycznie uzyskuje dostęp do każdej aplikacji

Na świecie aplikacje w chmurze najlepiej można to osiągnąć przy użyciu tożsamości do kontrolki "*KTO może wykonywać co*".

W przypadku komputerów terminologia:

- *Kto* jest nazywany *tożsamości* - zarządzania użytkownikami i grupami

- *Jakie* nosi nazwę *Zarządzanie dostępem* — Zarządzanie dostępu do chronionych zasobów

Oba składniki ze sobą są nazywane *tożsamości i zarządzania programu Access (IAM)*, która jest zdefiniowana przez grupę [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) jako "*dyscypliny zabezpieczeń, umożliwiający prawo osób uzyskać dostęp do odpowiednich zasobów po prawej stronie razy powodów prawo*".

Dobrze, więc na czym polega problem? Jeśli IAM nie jest *obsługiwana* w jednym miejscu za pomocą zintegrowanego rozwiązania:

- Tożsamość Administratorzy mają do pojedynczo tworzenie i aktualizowanie kont użytkowników we wszystkich aplikacjach oddzielnie, zbędne i czasochłonne aktywności.

- Użytkownicy musieli zapamiętaniu wiele poświadczeń w celu uzyskania dostępu do aplikacji, które są potrzebne do pracy z. Dzięki temu użytkownicy zwykle Zanotuj haseł lub za pomocą innych rozwiązań zarządzania hasła, które wprowadza ryzyko związane z zabezpieczeniami innych danych.

- Nadmiarowe, czasochłonne działania zmniejszyć liczbę użytkowników i administratorów pracują nad działalności, które zwiększają mierzenie Twojej firmy.

Tak co zapobiega zazwyczaj organizacje z przyjęcia zintegrowane rozwiązania IAM?

- Rozwiązania najbardziej techniczne są oparte na platformach oprogramowania, które muszą być wdrożony i dostosować przy każdej organizacji na potrzeby własnych aplikacji.

- Często przyjmuje się w poziomie wyższym niż działem INFORMATYCZNYM można zintegrować z istniejących rozwiązań IAM aplikacje w chmurze.

- Zabezpieczenia i monitorowanie narzędzia wymagają dodatkowe dostosowanie i integracja uzyskanie pełna scenariusze E2E.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory zintegrowany z aplikacjami

Pełna tożsamości firmy Microsoft jako rozwiązania usługi (IDaaS) jest Azure Active Directory który:

- Umożliwia IAM jako usługi w chmurze 

- Zapewnia dostęp centralne zarządzanie, logowania jednokrotnego na (SSO) i raportowania 

- Zarządzanie dostępem zintegrowane obsługuje [tysiące aplikacji](https://azure.microsoft.com/marketplace/active-directory/) w galerii aplikacji, takich jak usługi Salesforce, usługi Google Apps, pole i Concur. 


Z usługi Azure Active Directory wszystkie aplikacje publikowanie dla partnerów i klientów (firmy lub dla klientów indywidualnych) ma taką samą tożsamość i uzyskać dostęp do funkcji zarządzania.<br> Dzięki temu można znacznie zmniejszyć koszty operacyjne.

Co zrobić, jeśli jest potrzebna do wykonania aplikację, która nie jest jeszcze umieszczona w galerii aplikacji? Gdy jest to nieco więcej czasu niż Konfigurowanie rejestracji Jednokrotnej dla aplikacji z galerii aplikacji, Azure AD umożliwia za pomocą kreatora, który pomoże Ci z konfiguracją.

Wartość Azure AD wykracza poza "tak" aplikacje w chmurze. Można go również używać z aplikacji lokalnej, dostarczając bezpieczny dostęp zdalny. Bezpiecznego dostępu zdalnego, można wyeliminować potrzebę sieci VPN lub innych tradycyjnych dostępu zdalnego zarządzania implementacji.

Udostępniając logowaniu jednokrotnym (SSO) i zarządzanie centralnej dostępu dla wszystkich aplikacji, Azure AD udostępnia rozwiązanie problemów dotyczących zabezpieczeń i wydajności główne dane.

- Użytkownicy mają dostęp do wielu aplikacji z jednym logowaniu nadanie dłużej dochód generowania lub firmowy działań operacyjnych gotowe.

- Tożsamość Administratorzy mogą zarządzać dostęp do aplikacji w jednym miejscu.

Korzyścią dla użytkownika i firmy jest oczywiste. Przyjrzyjmy się przyjrzeć się bliżej zalet administrator tożsamości i organizacji.

## <a name="integrated-application-benefits"></a>Zalety zintegrowanej aplikacji

Proces logowania jednokrotnego ma dwie czynności:

- Uwierzytelnianie, proces sprawdzania poprawności tożsamość użytkownika.

- Autoryzacja, decyzja Włącz lub blokowanie dostępu do zasobu z zasad dostępu.

Podczas używania Azure AD do zarządzania aplikacjami i Włączanie rejestracji Jednokrotnej:

- Uwierzytelnianie jest wykonywane na lokalnym (na przykład AD) lub Azure AD konta użytkownika.

- Autoryzacja wykonuje na Azure AD przydziałów i ochrona zasady zapewnienia spójnych użytkownika końcowego i pozwala na dodawanie przydziału, lokalizacji i warunków MFA w dowolnej aplikacji, bez względu na jej możliwości.

Jego pamiętać, że sposób zezwolenia jest wprowadzany w aplikacji docelowej zależnie od integracji aplikacji z usługą Azure Active Directory.

- **Aplikacje wstępnie scałkowanej w przedziale przez usługodawcę** Podobnie jak usługi Office 365 i Azure są aplikacji bezpośrednio na Azure AD i oparte na nim ich kompleksowe możliwości zarządzania tożsamości i access. Dostęp do nich jest włączona za pośrednictwem informacji katalogowych i token wydania.

- **Aplikacje wstępnie scałkowanej w przedziale przez firmę Microsoft i niestandardowe aplikacje** Są to aplikacje w chmurze niezależnych, które zależą od katalog aplikacji wewnętrznych i może działać niezależnie od Azure AD. Dostęp do nich jest włączona wysyłając poświadczenie określonej aplikacji zamapowane na konto aplikacji. W zależności od możliwości aplikacji poświadczeń może być token Federacji lub nazwę użytkownika i hasło konta, który został wcześniej zainicjowany w aplikacji.

- **Aplikacje lokalnego** Aplikacje są publikowane w usłudze Azure AD serwera proxy aplikacji, przede wszystkim umożliwiające dostęp do aplikacji lokalnej. Te aplikacje są oparte na środkowej w katalogu lokalna, takich jak usługi Active Directory systemu Windows Server. Dostęp do nich jest włączona przez powodujące serwera proxy do dostarczania zawartości aplikacji do użytkownika końcowego, przy jednoczesnym zachowaniu wymaganie logowania jednokrotnego lokalnego.

Na przykład użytkownik dołączy organizacji, musisz utworzyć konto użytkownika w Azure AD dla operacji logowania głównych. Jeśli ten użytkownik wymaga dostępu do zarządzanej aplikacji, takich jak usługi Salesforce, również musisz utworzyć konto dla tego użytkownika do usługi Salesforce i połączyć go z konto Azure nawiązać logowania jednokrotnego pracy. Gdy użytkownik opuści organizacji, zaleca się usunąć konto Azure AD i wszystkie konta odpowiednika w IAM są przechowywane w aplikacji, których użytkownik ma dostęp do.

## <a name="access-detection"></a>Wykrywanie programu Access

W nowoczesnym przedsiębiorstwach działów informatycznych często nie są znane wszystkie aplikacje, które są używane w chmurze. W połączeniu z chmury aplikacji odnajdowanie Azure AD oferuje rozwiązanie wykrywanie te aplikacje.

## <a name="account-management"></a>Zarządzanie kontami

Zazwyczaj zarządzania kontami w różnych aplikacji jest wykonywane przez proces ręczny IT lub obsługuje osobistych w organizacji. Azure AD pełni automatycznego zarządzania kontami wszystkich aplikacji usług dla dostawcy zintegrowane i te aplikacje wstępnie zintegrowane przez firmę Microsoft pomocniczych użytkownika automatycznego inicjowania obsługi administracyjnej lub SAML JIT.

## <a name="automated-user-provisioning"></a>Przypisywanie użytkowników automatycznego

Niektóre aplikacje udzielanie interfejsów automatyzacji tworzenia i usuwania lub dezaktywacji konta. Jeśli dostawca oferuje taki interfejs, jest ona osobie Azure AD. Ogranicza koszty operacyjne, ponieważ zadania administracyjne zostanie przeprowadzona automatycznie i zwiększa bezpieczeństwo środowiska, ponieważ zmniejsza ryzyko nieautoryzowanego dostępu.

## <a name="access-management"></a>Zarządzanie dostępem

Za pomocą Azure AD można zarządzać dostęp do aplikacji przy użyciu poszczególnych lub reguły zmiennych przydziałów. Można również delegować uzyskać dostęp do zarządzania odpowiednim osobom w organizacji najlepsze nadzorem zapewnienie i zmniejszenia obciążenia działu pomocy technicznej.

## <a name="on-premises-applications"></a>Aplikacje lokalne

Gotowy w aplikacji serwera proxy umożliwia publikowanie aplikacji lokalnych użytkowników uzyskując oba spójne dostęp doświadczenia w chmurze nowoczesny aplikację i korzyści wynikających z możliwości Azure AD monitorowanie, raportowanie i zabezpieczeń.

## <a name="reporting-and-monitoring"></a>Raportowanie i monitorowania

Azure AD oferuje wstępnie zintegrowane raportowania i monitorowania możliwości, które umożliwiają wiesz, kto ma dostęp do aplikacji i są faktyczną one używane.

## <a name="related-capabilities"></a>Funkcje pokrewne

Aplikacje z zasady dostępu szczegółowego i wstępnie zintegrowane MFA można zabezpieczyć z usługą Azure Active Directory. Aby uzyskać więcej informacji na temat Azure MFA zobacz [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Wprowadzenie

Aby rozpocząć pracę, integracji aplikacji z usługą Azure Active Directory, zapoznaj się [Przewodnik Wprowadzenie do integracji usługi Azure Active Directory z aplikacjami wprowadzenie](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Zobacz też

[Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
