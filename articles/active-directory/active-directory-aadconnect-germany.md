<properties
    pageTitle="Azure AD Connect w Niemcy chmury firmy Microsoft"
    description="Narzędzie Azure AD Connect będzie integracja katalogów z lokalnego z usługi Azure Active Directory. Pozwala na dostarcza tożsamości wspólnych dla aplikacji usługi Office 365, Azure i władz akredytacji bezpieczeństwa zintegrowany z usługą Azure Active Directory."
    keywords="wprowadzenie w celu narzędzie Azure AD Connect, przegląd Azure AD Connect, co to jest Azure AD Connect, zainstaluj usługi active directory, Niemcy, czarny las"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/08/2016"
    ms.author="billmath"/>

#<a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect w Niemcy chmury firmy Microsoft — publicznej Preview

## <a name="introduction"></a>Wprowadzenie
Narzędzie Azure AD Connect zapewnia synchronizację między lokalnej usługi Active Directory a usługą Azure Active Directory.
Obecnie wiele scenariuszy w [Niemcy chmury firmy Microsoft](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) musi odbywać się przez operatora. Podczas korzystania z programu Microsoft Cloud Niemcy, należy pamiętać o następujących czynności:


- Następujące adresy URL muszą być otwarte na serwerze proxy dla synchronizacji pomyślnie:
    - *. microsoftonline.de
    - *. windows.net
    - + Listy odwołań certyfikatów

- Po zalogowaniu się do katalogu Azure AD należy użyć konta w tej domenie onmicrosoft.de.
- Nie są dostępne następujące funkcje:
    - Azure AD Connect kondycji
    - Aktualizacje automatyczne
    - Hasło wystornowaniem

## <a name="download"></a>Plik do pobrania
Narzędzie Azure AD Connect można pobrać z karta Azure AD Connect w portalu.  Skorzystaj z poniższych instrukcji, aby zlokalizować karta Azure AD Connect.

### <a name="the-azure-ad-connect-blade"></a>Azure AD Connect karta

Gdy użytkownik został wylogowany z portalem Azure, wykonaj następujące czynności:

1. Przejdź do przeglądania
2.  Wybierz pozycję Azure Active Directory
3.  Następnie wybierz narzędzie Azure AD Connect

Powinny zostać wyświetlone następujące czynności:

![Azure AD Connect karta](media\active-directory-aadconnect-germany\germany1.png)

 
W poniższej tabeli opisano funkcje pokazano karta.


Tytuł|Opis|
----- | ----- |
STAN SYNCHRONIZACJI|Załóżmy wiadomo, czy synchronizacja jest włączone lub wyłączone.|
OSTATNIA SYNCHRONIZACJA|Ostatniego pomyślnego synchronizacja.|
DOMENACH FEDERACYJNYCH|Pokazuje liczbę obecnie skonfigurowanych w domenach federacyjnych.|


## <a name="installation"></a>Instalacji
Aby zainstalować narzędzie Azure AD Connect, możesz użyć dokumentacji [tutaj](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Zaawansowane funkcje i dodatkowych informacji
Aby uzyskać dodatkowe informacje i wskazówki dotyczące ustawień niestandardowych i zaawansowanych konfiguracji rozpoczynać [Integrowanie tożsamości lokalnych z usługą Azure Active Directory](active-directory-aadconnect.md).  Ta strona zawiera informacje i łącza do dodatkowych wskazówek.
