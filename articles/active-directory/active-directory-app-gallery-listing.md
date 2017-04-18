<properties
   pageTitle="Wyświetlanie listy aplikacji w galerii aplikacji usługi Azure Active Directory"
   description="Jak wyświetlić listę aplikacji, która obsługuje logowania jednokrotnego w galerii usługi Azure Active Directory | Microsoft Azure"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>


# <a name="listing-your-application-in-the-azure-active-directory-application-gallery"></a>Wyświetlanie listy aplikacji w galerii aplikacji usługi Azure Active Directory

Aby wyświetlić listę aplikacji, która obsługuje rejestracji jednokrotnej z usługą Azure Active Directory w [galerii Azure AD](https://azure.microsoft.com/marketplace/active-directory/all/), aplikacja najpierw musi zaimplementować jedną z następujących trybów integracji:

* **Łączenie OpenID** - bezpośredni Integracja z Azure AD przy użyciu funkcji OpenID połączenie dla uwierzytelniania i interfejsu API zgody Azure AD konfiguracji. Jeśli aplikacji nie obsługuje SAML tylko uruchamiasz integracji, to jest tryb zalecane.

* **SAML** — aplikacja już ma możliwość Konfigurowanie dostawców tożsamości innych firm za pomocą protokołu SAML.

Lista wymagania dotyczące poszczególnych trybach są poniżej.

##<a name="openid-connect-integration"></a>Integracja połączenia OpenID

Aby zintegrować aplikacji z usługą Azure Active Directory, postępując zgodnie z [instrukcjami Deweloper](active-directory-authentication-scenarios.md). Następnie wykonaj poniższe pytania i wysłać do waadpartners@microsoft.com.

* Poświadczenia dla dzierżawy test lub konta z aplikacją, który może być używany przez zespół Azure AD, aby przetestować integracja.  

* Zawiera instrukcje dotyczące jak zalogować się i nawiązać wystąpienie Azure AD aplikację za pomocą [Azure AD zgodę framework](active-directory-integrating-applications.md#overview-of-the-consent-framework)zespołu Azure AD. 

* Podaj wszelkie dodatkowe informacje wymagane dla zespołu Azure AD przetestować rejestracji jednokrotnej z aplikacją. 

* Wprowadź poniższe informacje:

> Nazwa firmy:
> 
> Witryna sieci Web firmy:
> 
> Nazwa aplikacji:
> 
> Opis aplikacji (limit 256 znaków):
> 
> Aplikacji witryny sieci Web (informacyjna):
> 
> Aplikacja techniczne pomocy technicznej witryny sieci Web lub informacje kontaktowe:
> 
> Identyfikator klienta aplikacji, jak pokazano w sekcji szczegółów aplikacji u https://manage.windowsazure.com:
> 
> Gdzie klienci do logowania się w celu uzyskania adresu URL zapisywania się do aplikacji i notebooka zakupu aplikacji:
> 
> Wybierz maksymalnie trzy kategorie dla aplikacji można wyświetlić w obszarze (patrz dostępne kategorie Azure Active Directory Marketplace):
> 
> Dołączanie małą ikonę aplikacji (plik PNG, 45px przez 45px, jednolity kolor tła):
> 
> Dołączanie duże ikony aplikacji (plik PNG, 215px przez 215px, jednolity kolor tła):
> 
> Dołącz Logo aplikacji (plik PNG, 150px przez 122px, przezroczystego tła):

##<a name="saml-integration"></a>Integracja SAML

Bezpośrednio z dzierżawcą Azure AD przy użyciu [poniższe instrukcje dotyczące dodawania aplikacji niestandardowej](active-directory-saas-custom-apps.md)można zintegrować dowolnej aplikacji, która obsługuje SAML 2.0. Po przetestowaniu się, że usługi integracji aplikacji działa z usługą Azure Active Directory, Wyślij poniższe informacje, aby <waadpartners@microsoft.com>.

* Poświadczenia dla dzierżawy test lub konta z aplikacją, który może być używany przez zespół Azure AD, aby przetestować integracja.  

* Podaj wartości (usługa dla klientów indywidualnych potwierdzenia) SAML logowania jednokrotnego adresu URL, wystawcy adresu URL (identyfikator jednostki) i adres URL odpowiedzi dla aplikacji, jak opisano [w tym miejscu](active-directory-saas-custom-apps.md). Jeśli te wartości zazwyczaj podane jako część pliku metadanych SAML, następnie wyślij który także.

* Podaj krótki opis sposobu konfigurowania Azure AD jako dostawcy tożsamości w aplikacji przy użyciu SAML 2.0. Jeśli aplikacja obsługuje konfigurowanie Azure AD jako dostawcy tożsamości za pośrednictwem administracyjne portalu, następnie sprawdź, czy poświadczenia podanego powyżej obejmują możliwość go skonfigurować.

* Wprowadź poniższe informacje:

> Nazwa firmy:
> 
> Witryna sieci Web firmy:
> 
> Nazwa aplikacji:
> 
> Opis aplikacji (limit 256 znaków):
> 
> Aplikacji witryny sieci Web (informacyjna):
> 
> Aplikacja techniczne pomocy technicznej witryny sieci Web lub informacje kontaktowe:
> 
> Gdzie klienci do logowania się w celu uzyskania adresu URL zapisywania się do aplikacji i notebooka zakupu aplikacji:
> 
> Wybierz maksymalnie trzy kategorie dla aplikacji można wyświetlić w obszarze (patrz dostępne kategorie [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/))):
> 
> Dołączanie małą ikonę aplikacji (plik PNG, 45px przez 45px, jednolity kolor tła):
> 
> Dołączanie duże ikony aplikacji (plik PNG, 215px przez 215px, jednolity kolor tła):
> 
> Dołącz Logo aplikacji (plik PNG, 150px przez 122px, przezroczystego tła):
