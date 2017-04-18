<properties 
    pageTitle="Sposób debugowania opartego na protokole SAML rejestracji jednokrotnej dla aplikacji w usłudze Active Directory platformy Azure | Microsoft Azure" 
    description="Dowiedz się, jak debugowanie opartego na protokole SAML rejestracji jednokrotnej dla aplikacji w usłudze Active Directory platformy Azure " 
    services="active-directory" 
    authors="asmalser-msft"  
    documentationCenter="na" manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="02/09/2016" 
    ms.author="asmalser" />

#<a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Sposób debugowania opartego na protokole SAML rejestracji jednokrotnej dla aplikacji w usłudze Active Directory platformy Azure

Podczas debugowania integracji opartego na protokole SAML aplikacji, często jest przydatne za pomocą narzędzi, takich jak [Fiddler](http://www.telerik.com/fiddler) Zobacz żądanie SAML, odpowiedź SAML i rzeczywistej token SAML wystawiony do aplikacji. Sprawdzając SAML token daje pewność, że wszystkie wymagane atrybuty, nazwa użytkownika w temacie SAML i wystawcy URI pochodzą za pośrednictwem zgodnie z oczekiwaniami.

![][1]

Odpowiedź Azure AD, która zawiera SAML token jest zazwyczaj występujący po HTTP 302 przekierować z https://login.windows.net i są wysyłane do skonfigurowanych **Odpowiedź adres URL** aplikacji. 
 
Wybranie tego wiersza, a następnie wybierając można wyświetlać SAML token **inspektorów > formularzy sieci Web** kartę w prawym panelu. Stamtąd kliknij prawym przyciskiem myszy wartość **SAMLResponse** , a następnie zaznacz pole wyboru **Wyślij do TextWizard**. Menu **Przekształcanie** dekodowanie tokenu i wyświetlić jego zawartość, a następnie wybierz **Z Base64** .
 
**Uwaga**: Aby wyświetlić zawartość tego żądania HTTP, Fiddler może zostać wyświetlony komunikat można skonfigurować odszyfrowywanie ruchu HTTPS, które należy wykonać.

## <a name="related-articles"></a>Artykuły pokrewne

- [Artykuł indeks do zarządzania aplikacjami w usłudze Azure Active Directory](active-directory-apps-index.md)
- [Konfigurowanie rejestracji jednokrotnej dla aplikacji, które nie znajdują się w galerii aplikacji usługi Azure Active Directory](active-directory-saas-custom-apps.md)
- [Jak dostosować oświadczeniach wydawane w Token SAML dla wstępnie zintegrowane aplikacje](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ./media/active-directory-saml-debugging/fiddler.png
