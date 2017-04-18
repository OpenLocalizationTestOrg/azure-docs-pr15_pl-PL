<properties
   pageTitle="Zmiany atrybutów obiektu użytkownika zewnętrznego do podglądu współpracy Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B obsługuje relacji między firmy, włączając partnerów biznesowych selektywne dostępu do sieci firmowej aplikacji"
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
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Podgląd współpracy w usłudze Azure AD B2B: zmiany atrybutów obiektu użytkowników zewnętrznych

Każdy użytkownik w katalogu Azure AD jest reprezentowany przez użytkownika. Obiekt użytkownika w Azure AD podlega zmiany atrybutów w różnych etapów współpracy B2B Zaproś zrealizować przepływu. Obiekt użytkownika reprezentującą użytkownik partnera w katalogu ma atrybuty, które zmieniają się na realizowanie czas, po kliknięciu łącza w wiadomości e-mail Zaproś przez użytkownika partnera. W szczególności:

- Atrybuty **SignInName** i **AltSecId** są wypełnione
- Atrybut **DisplayName** zmienia się z głównej nazwy użytkownika (user_fabrikam.com#EXT#@contoso.com) nazwa logowania(user@fabrikam.com)

Śledzenie następujące atrybuty w Azure AD może ułatwić rozwiązywanie problemów z czy użytkownika partner ma zaginął ich B2B współpracy zaproszenie.

## <a name="related-articles"></a>Artykuły pokrewne
Przeglądanie naszych inne artykuły na Azure AD B2B współpracy:

- [Co to jest Azure AD B2B współpracy?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Jak to działa](active-directory-b2b-how-it-works.md)
- [Uzyskać szczegółowy opis](active-directory-b2b-detailed-walkthrough.md)
- [Odwołanie do formatu pliku CSV](active-directory-b2b-references-csv-file-format.md)
- [Format token użytkowników zewnętrznych](active-directory-b2b-references-external-user-token-format.md)
- [Ograniczenia w bieżącej wersji preview](active-directory-b2b-current-preview-limitations.md)
- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
