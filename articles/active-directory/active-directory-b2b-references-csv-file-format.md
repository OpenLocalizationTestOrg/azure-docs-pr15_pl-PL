<properties
   pageTitle="Format pliku CSV do podglądu współpracy Azure Active Directory B2B | Microsoft Azure"
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

# <a name="azure-ad-b2b-collaboration-preview-csv-file-format"></a>Podgląd współpracy w usłudze Azure AD B2B: format pliku CSV

Wersja zapoznawcza współpracy Azure AD B2B wymaga pliku CSV, określając informacje o użytkowniku partnera do przekazania za pośrednictwem portalu Azure AD. Plik CSV powinien zawierać wymagane poniżej etykiety i pola opcjonalne stosownie do potrzeb. Modyfikowanie przykładowy plik CSV (patrz poniżej) bez zmieniania pisownię etykiety w pierwszym wierszu.

>[AZURE.NOTE] Pierwszy wiersz etykiet (na przykład wiadomości E-mail, DisplayName i tak dalej) jest niezbędne do pliku CSV można przeanalizować pomyślnie. Pisownię musi odpowiadać pola określone poniżej. Poniższe etykiety identyfikacji zawartości pod nimi. Dla pola opcjonalne, które nie są potrzebne można usunąć ich etykiety z pliku CSV. Cała kolumna może być puste.

## <a name="required-fields-br"></a>Wymagane pola: <br/>
**Poczty e-mail:** Adres e-mail użytkownika zaproszenie. <br/>
**DisplayName:** Nazwa wyświetlana użytkownika zaproszonych (zazwyczaj imię i nazwisko).<br/>


## <a name="optional-fields-br"></a>Pola opcjonalne: <br/>

**InvitationText:** Dostosowywanie tekstu wiadomości e-mail zaproszenie, po związane ze znakowaniem, aplikacji i przed łącze wykupu.<br/>
**InvitedToApplications:** AppIDs do aplikacji firmowej można przypisać użytkowników. AppIDs są uwzględnianej podczas pobierania wyników w programie PowerShell, dzwoniąc`Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`<br/>
**InvitedToGroups:** Identyfikatory obiektów dla grup, aby dodać użytkownika do. Identyfikatory obiektów są uwzględnianej podczas pobierania wyników w programie PowerShell, dzwoniąc`Get-MsolGroup | fl DisplayName, ObjectId`<br/>
**InviteRedirectURL:** Adres URL umożliwiający bezpośredni zaproszenie użytkownika po zaakceptowaniu zaproszenia. Należy to adres URL konkretnej firmy (na przykład [*contoso.my.salesforce.com*](http://contoso.my.salesforce.com/)). Jeśli to opcjonalne pole nie zostanie określony, zaproszenie użytkownika jest przekierowanie do Panel dostępu aplikacji, której można przejść do wybranej aplikacji firmowej. Adres URL panelu aplikacji Access ma postać `https://account.activedirectory.windowsazure.com/applications/default.aspx?tenantId=<TenantID>`.<br/>
**CcEmailAddress**: adres, aby skopiować wysłane zaproszenie E-mail. Użycie pola CcEmailAddress to zaproszenie nie można używać dla zweryfikowanych e-mail użytkownika lub tworzenie dzierżawy.<br/>
**Język:** Język zaproszenie e-mail oraz wykupu możliwości "en" (w języku angielskim) jako domyślny, gdy nieokreślone. Inne 10 obsługiwanych języków, kodów:<br/>
1. de: niemiecki<br/>
2. ES: hiszpańskim<br/>
3. FR: francuski<br/>
4. jego: włoski<br/>
5. ja: japoński<br/>
6. Ko: koreański<br/>
7. pt-BR: portugalski (Brazylia)<br/>
8. RU: rosyjski<br/>
9. zh HANS: chiński uproszczony<br/>
10. zh HANT: chiński (tradycyjny)<br/>

## <a name="sample-csv-file"></a>Przykładowy plik CSV
Poniżej przedstawiono przykładowy plik CSV można modyfikować.

>[AZURE.NOTE] Skopiuj i Wklej to do Notatnika i zapisz go z rozszerzeniem "CSV". Następnie edytować to w programie Excel. Jego strukturę do tabeli z etykietami w pierwszym wierszu.

> Dodaj więcej pól opcjonalne w programie Excel, podając etykiety i wypełniać kolumny znajdujące się pod nim.

```
Email,DisplayName,InvitationText,InviteRedirectUrl,InvitedToApplications,InvitedToGroups,CcEmailAddress,Language
wharp@contoso.com,Walter Harp,Hi Walter! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
jsmith@contoso.com,Jeff Smith,Hi Jeff! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en
bsmith@contoso.com,Ben Smith,Hi Ben! I hope you’ve been doing well.,,cd3ed3de-93ee-400b-8b19-b61ef44a0f29,,you@yourcompany.com,en

```

## <a name="related-articles"></a>Artykuły pokrewne
Przejrzyj naszych innych artykułów na współpracy Azure AD B2B

- [Co to jest Azure AD B2B współpracy?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Jak to działa](active-directory-b2b-how-it-works.md)
- [Uzyskać szczegółowy opis](active-directory-b2b-detailed-walkthrough.md)
- [Format token użytkowników zewnętrznych](active-directory-b2b-references-external-user-token-format.md)
- [Zmiany atrybutów obiektu użytkowników zewnętrznych](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Ograniczenia w bieżącej wersji preview](active-directory-b2b-current-preview-limitations.md)
- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
