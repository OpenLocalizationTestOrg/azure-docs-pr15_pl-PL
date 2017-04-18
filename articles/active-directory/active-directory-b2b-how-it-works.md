<properties
   pageTitle="Podgląd współpracy w usłudze Azure AD B2B: jak działa | Microsoft Azure"
   description="W tym artykule opisano, jak współpracy Azure Active Directory B2B obsługuje relacji między firmy, włączając partnerów biznesowych selektywne dostępu do sieci firmowej aplikacji"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Podgląd współpracy w usłudze Azure AD B2B: jak działa
Azure AD B2B współpraca jest oparty na zaproszenie i zrealizować modelu. Podane adresy e-mail strony, którą chcesz pracować, wraz z aplikacji mają być. Azure AD wysyła je zaproszenie e-mail z łączem. Użytkownika partner następujący link i zostanie wyświetlony monit o Zaloguj się przy użyciu ich Azure AD konto lub Utwórz dla nowego Azure AD konta.

1. Administrator stanowi zaproszenie partnerów, przekazując [pliku CSV strukturalnych](active-directory-b2b-references-csv-file-format.md) za pomocą portalu Azure.
2. Wysyła portalu Zaproś wiadomości e-mail do tych użytkowników partnera.
3. Użytkownicy partnera kliknij łącze w wiadomości e-mail i jest wyświetlany monit o zalogowanie się przy użyciu poświadczeń pracy (jeśli są one już w Azure AD) lub do utworzenia konta jako użytkownik współpracy Azure AD B2B.
4. Partner użytkownicy są przekierowywani do aplikacji, którą zostali zaproszeni, gdzie obecnie mają dostęp.

## <a name="directory-operations"></a>Operacje katalogu
Istnieje partnerów w usługi Azure AD jako użytkowników zewnętrznych. Oznacza to, administratora można obsługi administracyjnej licencji, przypisz członkostwo w grupach i dodatkowo udzielić dostępu do aplikacji firmowych przez Azure portal lub przy użyciu programu PowerShell Azure tak samo jak dla użytkowników w firmie.

Podczas płatnej Azure AD subskrypcji (Basic lub premia) nie jest konieczne użycie Azure AD B2B, dzierżaw, którzy mają płatnej subskrypcji Azure AD (Basic lub premia) uzyskać następujące dodatkowe korzyści:

 - Administratorzy mogą przypisywać grupy aplikacje, dostarczając upraszcza zarządzanie dostępem użytkowników zaproszonych.
 - Administrator dzierżawy związane ze znakowaniem, jest używana do znakowanie wiadomości e-mail zaproszenie i obsługę wykupu, dostarczając więcej informacji o zaproszonych użytkowników partnera.

## <a name="related-articles"></a>Artykuły pokrewne
 Przejrzyj naszych innych artykułów na współpracy Azure AD B2B

 - [Co to jest Azure AD B2B współpracy?](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [Uzyskać szczegółowy opis](active-directory-b2b-detailed-walkthrough.md)
 - [Odwołanie do formatu pliku CSV](active-directory-b2b-references-csv-file-format.md)
 - [Format token użytkowników zewnętrznych](active-directory-b2b-references-external-user-token-format.md)
 - [Zmiany atrybutów obiektu użytkowników zewnętrznych](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [Ograniczenia w bieżącej wersji preview](active-directory-b2b-current-preview-limitations.md)
 - [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
