<properties
   pageTitle="Azure Active Directory B2B współpracy | Microsoft Azure"
   description="Azure Active Directory B2B współpracy umożliwia partnerów biznesowych uzyskać dostęp do aplikacji firmowych, z każdego z użytkowników reprezentowany przez pojedynczy Azure AD konta"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B współpracy

Azure współpracy B2B usługi Active Directory (Azure AD) pozwala na włączanie dostępu w aplikacji firmowej z zarządzaniem partnera tożsamości. Można tworzyć relacje między firmy przez zaproszenie i autoryzacji użytkowników z partnera firmy, aby uzyskać dostęp do zasobów. Złożoność jest ograniczona, ponieważ każdej firmy federates raz z usługą Azure Active Directory i każdego użytkownika jest reprezentowany przez jednego konta Azure AD. Jeśli partnerów biznesowych zarządzania kont w Azure AD, ponieważ odebraniu dostępu, gdy partnerów są zamykane z organizacji, a nie będzie mógł niezamierzonych dostępu za pośrednictwem członkostwa w katalogach wewnętrznych zwiększa się zabezpieczeń. Dla partnerów biznesowych, którzy nie mają już Azure AD współpracy B2B ma usprawniony aplecie aby zapewnić kont Azure AD dla partnerów biznesowych.

-   Partnerów biznesowych za pomocą własne poświadczenia logowania, które łączności z zarządzanie katalogiem partnerów zewnętrznych i z potrzeby usunięcie dostępu, gdy użytkownicy pozostawić organizacji partnera.

-   Określ zarządzanie dostępem do swoich aplikacji niezależnie od cyklu życia konta partnera biznesowego. Oznacza to, na przykład, aby odebrać dostęp bez konieczności poproś działu informatycznego partnera biznesowego nic robić.

## <a name="capabilities"></a>Funkcje

Współpraca za pomocą B2B upraszcza zarządzanie i poprawia zabezpieczeń partnera dostępu do zasobów firmy, w tym aplikacje władz akredytacji bezpieczeństwa, takich jak usługi Office 365, usługi Salesforce usługi Azure i każdej mobile w chmurze i lokalnej aplikacji. Partnerzy umożliwia współpracę B2B zarządzanie własnymi kontami i przedsiębiorstw można zastosować zasady zabezpieczeń do programu access partnera.

Azure Active B2B katalogu, które współpracy jest łatwe do konfigurowania uproszczone zapisów dla partnerów wszystkich rozmiarów, nawet jeśli nie ma własne usługi Azure Active Directory za pośrednictwem procesu weryfikacji wiadomości e-mail. Jest również łatwa w obsłudze nie zewnętrznych katalogach lub na konfiguracji federacyjnego partnera.

## <a name="b2b-collaboration-process"></a>Proces współpracy B2B

1. Azure AD B2B współpracy umożliwia firmie administratorowi zaprosić i Autoryzuj zbiór użytkowników zewnętrznych, przekazując plik wartości rozdzielanych przecinkami (CSV) nie więcej niż 2000 wierszy do portalu współpracy B2B.

  ![Okno dialogowe przekazywania pliku CSV](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. Portalu wysyła zaproszenia pocztą e-mail do tych użytkowników zewnętrznych.

3. Zaproszenie użytkownika będzie Zaloguj się do istniejącego konta pracy z programem Microsoft (zarządzania w Azure AD) albo Uzyskaj nowe konto pracy w Azure AD.

4. Po zalogowaniu się użytkownika będą przekierowywani do aplikacji, który został udostępniony z nimi.

Zaproszenia do adresów e-mail dla klientów indywidualnych (na przykład Gmail lub [*comcast.net*](http://comcast.net/)) nie są obecnie obsługiwane.

Aby uzyskać więcej informacji o tym, jak działa współpracy B2B Obejrzyj [ten klip wideo](http://aka.ms/aadshowb2b).

## <a name="next-steps"></a>Następne kroki
Przeglądaj nasze inne artykuły na współpracy Azure AD B2B.

- [Co to jest Azure AD B2B współpracy?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Jak to działa](active-directory-b2b-how-it-works.md)
- [Uzyskać szczegółowy opis](active-directory-b2b-detailed-walkthrough.md)
- [Odwołanie do formatu pliku CSV](active-directory-b2b-references-csv-file-format.md)
- [Format token użytkowników zewnętrznych](active-directory-b2b-references-external-user-token-format.md)
- [Zmiany atrybutów obiektu użytkowników zewnętrznych](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Ograniczenia w bieżącej wersji preview](active-directory-b2b-current-preview-limitations.md)
- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
