<properties
   pageTitle="Bieżący ograniczenia podglądu do współpracy Azure Active Directory B2B | Microsoft Azure"
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
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Podgląd współpracy w usłudze Azure AD B2B: bieżące Podgląd ograniczenia

- Nie jest obsługiwana w użytkowników zewnętrznych uwierzytelnianie wieloskładnikowe (MFA). Na przykład jeśli Contoso ma MFA, ale nie organizacji partnera, następnie organizacji partnera użytkownikom nie można udzielić MFA do współpracy B2B.
- Zaproszenia są dostępne tylko za pośrednictwem CSV; poszczególne zaproszenia i interfejsu API programu access nie są obsługiwane.
- Tylko administratorzy globalnego Azure AD przekazać pliki CSV.
- Zaproszenia do adresów e-mail dla klientów indywidualnych (na przykład hotmail.com, Gmail.com lub comcast.net) nie są obecnie obsługiwane.
- Użytkownik zewnętrzny dostęp do lokalnego aplikacji nie jest sprawdzany.
- Użytkownicy zewnętrzni są nie automatycznie czyszczone po rzeczywisty użytkownik zostanie usunięty z ich katalogu.
- Zaproszenia do listy dystrybucyjne nie są obsługiwane.
- Maksymalnie 2000 rekordów można przekazać za pośrednictwem CSV.

## <a name="related-articles"></a>Artykuły pokrewne
Przeglądanie naszych inne artykuły na Azure AD B2B współpracy:

- [Co to jest Azure AD B2B współpracy?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Jak to działa](active-directory-b2b-how-it-works.md)
- [Uzyskać szczegółowy opis](active-directory-b2b-detailed-walkthrough.md)
- [Odwołanie do formatu pliku CSV](active-directory-b2b-references-csv-file-format.md)
- [Format token użytkowników zewnętrznych](active-directory-b2b-references-external-user-token-format.md)
- [Zmiany atrybutów obiektu użytkowników zewnętrznych](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
