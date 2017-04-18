<properties
   pageTitle="Użytkownik zewnętrzny token formatu Azure Active Directory B2B współpracy Podgląd | Microsoft Azure"
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

# <a name="azure-ad-b2b-collaboration-preview-external-user-token-format"></a>Podgląd współpracy w usłudze Azure AD B2B: formatowanie token użytkowników zewnętrznych

Roszczeń dla standardowego Azure AD token opisane w artykule [obsługiwane Token i typy oświadczeń](active-directory-token-and-claims.md) na azure.microsoft.com.

Roszczeń, które różnią się w uwierzytelniony użytkownik zewnętrzny współpracy B2B są następujące:<br/>
- **OID:** identyfikator obiektu z dzierżawy zasobów<br/>
- **TID**: dzierżawa identyfikator z dzierżawy zasobów<br/>
- **Wystawcy**: jest to dzierżawy zasobów<br/>
- **Protokołu IDP**: jest to dzierżawy głównym użytkownika<br/>
- **AltSecId**: jest to identyfikator alternatywny zabezpieczeń, który jest nieprzezroczysta dla Ciebie<br/>

## <a name="related-articles"></a>Artykuły pokrewne
Przeglądanie naszych inne artykuły na Azure AD B2B współpracy:

- [Co to jest Azure AD B2B współpracy?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Jak to działa](active-directory-b2b-how-it-works.md)
- [Uzyskać szczegółowy opis](active-directory-b2b-detailed-walkthrough.md)
- [Odwołanie do formatu pliku CSV](active-directory-b2b-references-csv-file-format.md)
- [Zmiany atrybutów obiektu użytkowników zewnętrznych](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Ograniczenia w bieżącej wersji preview](active-directory-b2b-current-preview-limitations.md)
- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
