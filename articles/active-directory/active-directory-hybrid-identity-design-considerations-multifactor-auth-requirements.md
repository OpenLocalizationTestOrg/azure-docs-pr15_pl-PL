<properties
    pageTitle="Określanie Azure Active Directory hybrydowych tożsamości zagadnienia projektowe — wymagania dotyczące uwierzytelniania wieloskładnikowego"
    description="Za pomocą pola Kontrola dostępu warunkowego usługi Azure Active Directory sprawdza określone warunki wybierz podczas uwierzytelniania użytkownika i przed zezwoleniem na dostęp do aplikacji. Po tych warunki są spełnione, użytkownik uwierzytelniony i dostęp do aplikacji."
    documentationCenter=""
    services="active-directory"
    authors="femila"
    manager="billmath"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Określenie uwierzytelnianie wieloskładnikowe wymagań dotyczących rozwiązania hybrydowego tożsamości

W tym świata mobilności użytkownikom uzyskiwanie dostępu do danych i aplikacje w chmurze i z dowolnego urządzenia zabezpieczenie tych informacji stało najważniejsze.  Codziennie jest nowy nagłówek o naruszenia zabezpieczeń.  Mimo że nie jest gwarantowana przed takie naruszenia, uwierzytelnianie wieloskładnikowe zapewnia dodatkową warstwę zabezpieczeń, aby zapobiec te naruszenia.
Zacznij od obliczonego wymagania organizacji uwierzytelniania wieloskładnikowego. Oznacza to co jest organizacji próby zabezpieczania.  Ocena ważne jest, aby zdefiniować wymagania techniczne dotyczące konfigurowania i pozwala użytkownikom organizacje uwierzytelniania wieloskładnikowego.

>[AZURE.NOTE]
Jeśli nie znasz MFA i działanie, zalecane jest przeczytanie artykuł [Co to jest Azure uwierzytelnianie wieloskładnikowe?](../multi-factor-authentication/multi-factor-authentication.md) przed kontynuuj czytanie tej sekcji.

Upewnij się, że odpowiedzi na poniższe czynności:

- Firma próbuje bezpiecznego aplikacji Microsoft? 
- Jak są publikowane te aplikacje?
- Sposób firmy zapewnia dostęp zdalny umożliwia pracownikom dostęp do lokalnego aplikacje?

Jeśli tak, jaki typ dostępu zdalnego? Ponadto potrzebny ma być obliczona miejsce, w którym będą umieszczane użytkowników, którzy korzystają z nich. Ocena jest kolejnym kroku ważne do określenia strategii uwierzytelnianie wieloskładnikowe pisane z wielkiej litery. Upewnij się, że na następujące pytania:

- Miejsce, w którym użytkownicy będą znajdować się?
- Mogą one znajdować się w dowolnym?
- Czy firma chcesz ustanowić ograniczenia zgodnie z lokalizacji użytkownika?

Po opis tych wymagań, należy również ocenić wymagania użytkownika uwierzytelniania wieloskładnikowego. Ocena ta jest ważne, ponieważ określi wymagań dotyczących wdrażania uwierzytelnianie wieloskładnikowe. Upewnij się, że na następujące pytania:

- Zna użytkowników uwierzytelnianie wieloskładnikowe?
- Niektóre zastosowania będzie wymagane podanie dodatkowych uwierzytelniania?  
 - Jeśli tak, zawsze, gdy pochodzącego z sieci zewnętrznych lub uzyskiwania dostępu do określonych aplikacji lub w innych warunkach?
- Użytkownicy wymaga szkolenie dotyczące instalacji i zaimplementować uwierzytelnianie wieloskładnikowe?
- Co to są kluczowe scenariusze firmy chce włączyć uwierzytelnianie wieloskładnikowe dla swoich użytkowników?

Po odebraniu odpowiedzi na powyższe pytania, można zrozumieć, jeśli istnieje już wdrożone uwierzytelnianie wieloskładnikowe lokalnego. Ocena ważne jest, aby zdefiniować wymagania techniczne dotyczące konfigurowania i pozwala użytkownikom organizacje uwierzytelniania wieloskładnikowego. Upewnij się, że na następujące pytania:

- Czy firma należy chronić uprzywilejowanych kont z MFA?
- Czy firma należy włączyć MFA dla niektórych aplikacji dla zachowania zgodności?
- Czy firma należy włączyć MFA dla wszystkich użytkowników uprawniony do korzystania z tych aplikacji lub tylko administratorzy?
- Potrzebujesz masz MFA zawsze włączone lub tylko wtedy, gdy użytkownicy są rejestrowane poza siecią firmową?


## <a name="next-steps"></a>Następne kroki
[Definiowanie strategii wdrażania tożsamości hybrydowego](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)


## <a name="see-also"></a>Zobacz też
[Omówienie zagadnienia projektu](active-directory-hybrid-identity-design-considerations-overview.md)
