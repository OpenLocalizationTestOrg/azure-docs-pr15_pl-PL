<properties
    pageTitle="Azure Active Directory hybrydowych tożsamości zagadnienia projektowe - określenie wymagań tożsamości | Microsoft Azure"
    description="Identyfikowanie potrzeb biznesowych firmy prowadzące zdefiniowanie wymagań dotyczących projektu tożsamości hybrydowych."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Określenie tożsamości wymagań dotyczących rozwiązania hybrydowego tożsamości
Pierwszym krokiem w projektowaniu rozwiązanie tożsamości hybrydowego jest ustalenie wymagań dotyczących organizacji business, która będzie używanie tego rozwiązania.  Tożsamość hybrydowego rozpoczyna się jako funkcją pomocniczą (obsługuje innych rozwiązań w chmurze, dostarczając uwierzytelniania) i przechodzi na dostarczanie i interesujące możliwości, jakie odblokować obciążenia nowych użytkowników.  Te obciążenia lub usługi, które chcesz przyjąć dla użytkowników będzie dyktowania wymagań dotyczących projektu tożsamości hybrydowych.  Tych usług i obciążenia muszą wykorzystać tożsamości hybrydowych zarówno w lokalnej i w chmurze.  

Chcesz zacząć tych najważniejszych aspektów jej działalności, aby dowiedzieć się, co to jest teraz wymagane i firmy plany w przyszłości. Jeśli nie masz widoczności długoterminową strategię hybrydowych tożsamości projektu, prawdopodobnie oznacza to, że rozwiązania nie będą skalowalna potrzeb firmy powiększanie i zmienić.   T on diagramu wymieniono przykład architektura tożsamości hybrydowych i obciążenia, które są są odblokowane dla użytkowników. To jest tylko przykładem wszystkie nowe możliwości, które mogą być odblokowywane a dostarczane z strategię tożsamości hybrydowych pełne. 
 
Niektóre składniki są częścią architektury tożsamości hybrydowego![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Określanie potrzeb biznesowych
Każdej firmy będzie mają różne wymagania, nawet jeśli te firmy są częścią tej samej branży rzeczywistej biznesowych, które mogą się różnić wymagań. Możesz nadal korzystać z najlepsze rozwiązania z branży, ale ostatecznie jest potrzeb biznesowych firmy prowadzące zdefiniowanie wymagań dotyczących projektu tożsamości hybrydowych. 

Upewnij się, że odpowiedzi na poniższe pytania, aby zidentyfikować potrzeb biznesowych:

- Firma chce wycięcie koszty operacyjne IT?
- Firmy chce bezpiecznego chmury aktywów (aplikacje władz akredytacji bezpieczeństwa, infrastruktura)?
- Firma chce modernizacji działu informatycznego?
  - Są użytkowników urządzeń przenośnych i wymagających IT, aby utworzyć wyjątki do Twojej strefy Zdemilitaryzowanej, aby umożliwić innego typu ruchu, aby uzyskać dostęp do różnych zasobów?
  - Czy firma ma starsze aplikacje, potrzebne do opublikowania do tych użytkowników nowoczesny, które nie są łatwe do edycji przykładu?
  - Czy firmy muszą wykonać te zadania i przesuń go w obszarze kontrolki w tym samym czasie?
- Firma chce zabezpieczanie tożsamość użytkownika i zmniejszenie ryzyka przez wprowadzenie nowych narzędzi, które używają specjalizacji firmy Microsoft Azure zabezpieczeń specjalizacji lokalne?
- Firma próbuje pozbyć "zewnętrzne" rachunków dreaded w środowisku lokalnym i przenieść je w chmurze, gdy nie są już zagrożenie nieaktywni w środowisku lokalnym?

## <a name="analyze-on-premises-identity-infrastructure"></a>Analizowanie infrastruktury tożsamości lokalnych
Teraz, gdy masz uzyskać ogólny obraz dotyczące wymagań biznesowych firmy, należy ocenić infrastruktury tożsamości lokalnych. Ocena ta jest ważna w przypadku Definiowanie wymagań technicznych, aby zintegrować rozwiązanie bieżącej tożsamości do systemu zarządzania tożsamości chmury. Upewnij się, że na następujące pytania:

- Jakie rozwiązanie uwierzytelniania i autoryzacji działa firma za pomocą lokalnego? 
- Firma ma obecnie jakichkolwiek usług synchronizacji lokalnej?
- Firma używa wszystkich dostawców tożsamości innych firm (protokołu IdP)?

Ponadto potrzebny się zapoznać z usługami w chmurze, które Twojej firmy może być. Ważne jest wykonywanie ocenę opis bieżącego Integracja z modelu władz akredytacji bezpieczeństwa, IaaS lub PaaS w środowisku. Upewnij się, że odpowiedzi na poniższe pytania podczas oceny:
- Czy firma ma integracji z dostawcę usług w chmurze?
- Jeśli tak, które usługi są używane?
- Integracja ta jest obecnie produkcji lub jest to wdrożenia pilotażowego?


>[AZURE.NOTE]
Jeśli nie ma dokładnego mapowania wszystkich aplikacji i usług w chmurze, służy narzędzie odnajdowania aplikacji chmury. To narzędzie może dostarczyć z działem INFORMATYCZNYM wgląd w Twojej organizacji wszystkich firm i aplikacje chmury dla klientów indywidualnych. Dzięki temu łatwiejsze niż kiedykolwiek do wykrywania cień IT w organizacji, w tym szczegółowych informacji na temat upodobania i użytkowników uzyskiwanie dostępu do aplikacje w chmurze. Aby uzyskać dostęp do tego narzędzia przejdź do [https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## <a name="evaluate-identity-integration-requirements"></a>Ocena wymagań integracji tożsamości
Następnie należy ocena wymagań integracji tożsamości. Ocena ważne jest, aby zdefiniować wymagania techniczne dotyczące jak użytkownicy będą służą do uwierzytelniania, wygląd obecności organizacji w chmurze, jak organizacji, aby umożliwić autoryzacji i obsługi użytkowników ma być. Upewnij się, że na następujące pytania:

- Twoja organizacja użyje Federacji, uwierzytelniania standardowego lub oba?
- Jest wymagane Federacja?  Ze względu na następujące czynności:
 - Opartego na protokole Kerberos logowania jednokrotnego
 - Firma ma aplikacji lokalnego (albo wbudowany przyjęcie wewnętrznych lub 3), które używa SAML lub podobne funkcje federacji.
 - MFA za pomocą kart inteligentnych. RSA SecurID itd.
 - Zasady dostępu klienta, usuwających poniższe pytania:
     1. Czy można zablokować wszystkie zewnętrzny dostęp do usługi Office 365 na podstawie adresu IP klienta?
     1. Czy można zablokować wszystkie dostępu zewnętrznego w usłudze Office 365, z wyjątkiem programu Exchange ActiveSync?
     1. Czy można zablokować wszystkie zewnętrzny dostęp do usługi Office 365, z wyjątkiem aplikacji przeglądarki (aplikacji OWA, usługi SPO)
     1. Dla członków grup AD wyznaczonych można zablokować wszystkie zewnętrzny dostęp do usługi Office 365
- Dotyczy inspekcji i zabezpieczeń
- Już istniejących inwestycji w federacyjnych uwierzytelniania
- Jaką nazwę organizacji będzie korzystać z naszych domeny w chmurze?
- Czy organizacja ma domenę niestandardową?
    1. Jest tej domeny publiczne i łatwe do sprawdzenia za pośrednictwem DNS?
    1. Jeśli nie jest dostępne, następnie masz publiczną, który może być używany do rejestrowania alternatywny UPN w AD?
- Identyfikatory użytkowników są spójne dla reprezentacja chmurze? 
- Czy aplikacje, które wymagają integracji z usługami w chmurze są dostępne dla organizacji?
- Organizacja ma wiele domen i będą one uwierzytelniania standardowy lub federacyjnych?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Szacowanie aplikacje, które działają w środowisku
Teraz, gdy masz uzyskać ogólny obraz dotyczące sieci lokalnej i w chmurze infrastruktury, należy ocenić aplikacje, które są uruchamiane w tych środowiskach. Ocena ważne jest, aby zdefiniować wymagania techniczne, aby zintegrować te aplikacje do systemu zarządzania tożsamości chmury. Upewnij się, że na następujące pytania:

- Miejsce, w którym będzie live naszych aplikacji
- Będą użytkownikom uzyskiwać dostęp do aplikacji lokalnej?  W chmurze? Lub oba?
- Czy istnieją plany, aby zrobić istniejących obciążenia aplikacji i przenieść je w chmurze?
- Czy istnieją plany tworzyć nowe aplikacje, które będą znajdować się obsługiwanych lokalnie lub w chmurze, który będzie używany w chmurze uwierzytelniania?

## <a name="evaluate-user-requirements"></a>Ocena wymagań użytkownika
Masz również ocena wymagań użytkownika. Ocena ważne jest, aby zdefiniować czynności, które będą potrzebne dla wprowadzającego i wspomaganie użytkowników, zgodnie z ich przejścia w chmurze. Upewnij się, że na następujące pytania:

- Będą użytkownikom uzyskiwać dostęp do aplikacji w środowisku lokalnym?
- Będą użytkownikom uzyskiwać dostęp do aplikacji w chmurze?
- Jak użytkownicy zwykle logujesz się do ich lokalnym środowiskiem?
- Jak będzie użytkownicy logować się w chmurze?

>[AZURE.NOTE]
Pamiętaj sporządzać notatki każdej odpowiedzi i opis uzasadnienie odpowiedź. [Określanie wymagania zdarzenia odpowiedzi](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) będą wyświetlane na przycisku Opcje dostępne i wad i zalet poszczególnych opcji.  Przez konieczności odpowiedzi na te pytania, która zostanie wybrana wybranej opcji najlepiej pasuje Twoja firma wymaga.

## <a name="next-steps"></a>Następne kroki
[Określenie wymagań dotyczących synchronizacji katalogów](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Zobacz też
[Omówienie zagadnienia projektu](active-directory-hybrid-identity-design-considerations-overview.md)
