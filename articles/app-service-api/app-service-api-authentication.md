<properties
    pageTitle="Uwierzytelniania i autoryzacji dla aplikacji interfejsu API usługi aplikacji Azure | Microsoft Azure"
    description="Informacje na temat usługi uwierzytelniania i autoryzacji Azure aplikacji usługi znajdują się w przypadku aplikacji interfejsu API."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="rachelap"/>

# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>Uwierzytelnianie i autoryzacja aplikacji interfejsu API Azure aplikacji usługi

## <a name="overview"></a>Omówienie 

> [AZURE.NOTE] W tym temacie zostaną objęte migracją na skonsolidowaną [uwierzytelniania usługi aplikacji i autoryzacji](../app-service/app-service-authentication-overview.md) temacie opisano, jak w sieci Web, Mobile i interfejsu API aplikacji.

Azure usługa aplikacji zawiera wbudowane usługi uwierzytelniania i autoryzacji implementujących [OAuth 2.0](#oauth) i [Łączenie OpenID](#oauth). Ten artykuł zawiera opis usług i opcji, które są dostępne w przypadku aplikacji interfejsu API usługi aplikacji Azure.

Na poniższym diagramie przedstawiono niektóre główne cechy uwierzytelniania aplikacji usługi:

* Ją przetworzy wstępnie przychodzących żądań interfejsu API, co oznacza, że działa on z języka ani framework obsługiwana przez usługę aplikacji.
* Daje które kilka opcji uwierzytelniania ilości pracy, możesz wykonać w własnego kodu.
* Działa zarówno dla użytkowników końcowych i uwierzytelnianie za pomocą konta usługi. 
* Obsługuje pięć dostawcy tożsamości: usługi Azure Active Directory, Facebook, Google, Twitter i Account Microsoft.
* Działa tak samo interfejsu API aplikacje, aplikacje sieci Web i aplikacji Mobile.

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>Niezależne od języka

Przetwarzanie uwierzytelniania aplikacji usługi znajduje się przed żądania osiągnięcia aplikacji interfejsu API oznacza, że funkcje uwierzytelniania działać w przypadku aplikacji interfejsu API napisanym w języku ani framework.  Z interfejsu API mogą być oparte na ASP.NET, Java, Node.js lub dowolny framework obsługującego aplikacji usługi.

Przekazywane przez usługę aplikacji sieci web JSON token (JWT) w nagłówku autoryzacji żądania HTTP, a kodu napisanego w języku ani framework mogą pobrać potrzebnych informacji z tokenu. Ponadto aplikacji usługi umożliwia łatwiejsze dostęp do najczęściej używane roszczeń, ustawiając niektóre nagłówki specjalne, takie jak:

* X-MS--KAPITAŁU NAZWA KLIENTA
* X-MS-KLIENTA KAPITAŁU ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON
 
Interfejs API .NET umożliwia `Authorize` atrybutów, a dla szerokiego autoryzacji można łatwo pisać kodu oparte na oświadczeniach, ponieważ informacje o oświadczeniach jest wypełnione klas .NET.

## <a name="multiple-protection-options"></a>Wiele opcji ochrony

Aplikacji usługi zapobiec osiągnięciu aplikacji interfejsu API anonimowe żądania HTTP, może przenieść wszystkie żądania i sprawdź poprawność tokenów dla żądania, które je uwzględnić lub go może być ustawiana przez wszystkie żądania bez żadnych działań od nich:

1. Zezwalaj tylko uwierzytelnieni żądania do osiągnięcia aplikacji interfejsu API.

    Odebranie anonimowe żądanie z poziomu przeglądarki aplikacji usługi nastąpi przekierowanie do strony logowania dla dostawcy uwierzytelniania (Azure AD, Google, Twitter, itd.) wybranego przez użytkownika. 

    Po wybraniu tej opcji nie trzeba wcale pisania kodu uwierzytelniania w aplikacji, a kod autoryzacji jest uproszczone, ponieważ najważniejszych oświadczeniach znajdują się w nagłówkach HTTP.

2. Zezwalaj na wszystkie żądania do osiągnięcia aplikacji interfejsu API, ale sprawdzania poprawności żądań uwierzytelnionych i przekazać informacje uwierzytelniające w nagłówkach HTTP.

    Ta opcja umożliwia największą elastyczność obsługi zleceń anonimowe, ale trzeba napisać kod, jeśli chcesz uniemożliwić użytkownikom anonimowym korzystanie z interfejsu API. Ponieważ najpopularniejszych roszczeń są przekazywane w nagłówkach żądania HTTP, Kod autoryzacji jest stosunkowo proste.
    
3. Zezwalaj na wszystkie żądania nawiązywać połączenia z interfejsu API, nie zająć się informacje uwierzytelniające w żądania.

    Ta opcja pozostawia zadań uwierzytelniania i autoryzacji całkowicie do kodu aplikacji.

W [portalu Azure](https://portal.azure.com/), wybierz odpowiednią opcję na **uwierzytelniania i autoryzacji** karta.

![](./media/app-service-api-authentication/authblade.png)

Opcji 1 i 2 Włącz **Uwierzytelnianie usługi aplikacji**, a na liście rozwijanej **akcję do wykonania po żądanie nie jest uwierzytelniony** wybierz pozycję **Zaloguj się** lub **Zezwalaj na żądanie (żadnej akcji)**.  Jeśli zdecydujesz się **zalogować**, musisz wybierz dostawcę uwierzytelniania i konfigurowanie tego dostawcy.

![](./media/app-service-api-authentication/actiontotake.png)

Aby uzyskać szczegółowe informacje na temat sposobu konfigurowania uwierzytelniania Zobacz [jak skonfigurować aplikację usługi aplikacji do logowania usługi Azure Active Directory za pomocą](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Artykuł dotyczy aplikacji interfejsu API, a także aplikacji dla urządzeń przenośnych i łączy do innych artykułów dotyczących innych dostawców uwierzytelniania.
 
## <a id="internal"></a>Uwierzytelnianie za pomocą konta usługi

Uwierzytelnianie usługi aplikacji działa w przypadku wewnętrznych scenariusze takich jak dzwonisz z jednej aplikacji interfejsu API do innej aplikacji interfejsu API. W tym scenariuszu możesz uzyskać tokenu przy użyciu poświadczeń konta usługi zamiast poświadczenia użytkownika końcowego. Konto usługi jest nazywany *głównej usługi* Azure Active Directory, a uwierzytelnianie przy użyciu konta usługi jest nazywane także scenariusz do usługi. 

W przypadku scenariuszy do usługi ochrony nazywane aplikacji interfejsu API przy użyciu usługi Azure Active Directory i podaj tokenu autoryzacji głównej usługi AAD podczas połączenia z aplikacji interfejsu API. Możesz uzyskać tokenu, dostarczając klienta, identyfikator i tajny z poziomu aplikacji AAD klienta. Specjalne tylko do Azure kod nie jest wymagane, takie jak umożliwia spełnienie obsługi token Zumo usług Mobile. Przykład w tym scenariuszu korzystanie z aplikacji interfejsu API programu ASP.NET jest objęta samouczek [usługi kapitału uwierzytelniania dla aplikacji interfejsu API](app-service-api-dotnet-service-principal-auth.md).

Jeśli chcesz obsługiwać scenariusz do usługi bez przy użyciu funkcji uwierzytelniania aplikacji usługi służy uwierzytelniania podstawowego lub certyfikaty klienta. Aby uzyskać informacji na temat certyfikatów klienta w Azure zobacz [Jak do skonfigurowania TLS wzajemnego uwierzytelniania dla aplikacji sieci Web](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Uzyskać informacji na temat uwierzytelniania podstawowego w programie ASP.NET zobacz [Filtry uwierzytelniania w 2 interfejsu API sieci Web programu ASP.NET](http://www.asp.net/web-api/overview/security/authentication-filters).

Uwierzytelnianie konta usługi z aplikacji logiki aplikacji usługi aplikacji interfejsu API jest szczególny przypadek wyjaśniający na [Używanie swojego niestandardowego interfejsu API hostowana w aplikacji usługi z aplikacjami logiczny](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="mobile-client-authentication"></a>Uwierzytelnianie klienta przenośnego

Aby uzyskać informacje dotyczące obsługi uwierzytelniania z klientów przenośnych, zobacz [dokumentację dotyczącą uwierzytelniania dla aplikacji dla urządzeń przenośnych](../app-service-mobile/app-service-mobile-ios-get-started-users.md). Uwierzytelnianie aplikacji usługi działa tak samo dla aplikacji dla urządzeń przenośnych i aplikacje interfejsu API.
  
## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji o uwierzytelnianiu i autoryzacji w usłudze Azure aplikacji zobacz następujące zasoby:

* [Rozwijanym uwierzytelniania aplikacji usługi i autoryzacji](/blog/announcing-app-service-authentication-authorization/)
* [Jak skonfigurować aplikację aplikacji usługi, aby użyć logowania usługi Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md) (Zawiera łącza do innych dostawców uwierzytelniania w górnej części strony). 

Aby uzyskać więcej informacji na temat OAuth 2.0, łączenie OpenID i JSON tokeny sieci Web (JWT) zobacz następujące zasoby.

* [Wprowadzenie do programu OAuth 2.0] (http://shop.oreilly.com/product/0636920021810.do "Wprowadzenie do programu OAuth 2.0") 
* [Wprowadzenie do uwierzytelnianie OAuth2, OpenID połączyć i JSON Web tokeny (JWT) - PluralSight kursu](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [Tworzenie i zabezpieczanie RESTful interfejsu API dla wielu klientów w programie ASP.NET - PluralSight kursu](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

Aby uzyskać więcej informacji na temat usługi Azure Active Directory zobacz następujące zasoby.

* [Scenariusze Azure AD](http://aka.ms/aadscenarios)
* [Przewodnik Azure deweloperów AD](http://aka.ms/aaddev)
* [Azure AD próbki](http://aka.ms/aadsamples)

## <a name="next-steps"></a>Następne kroki

Ten artykuł zawiera omówienie uwierzytelniania i autoryzacji funkcje są dostępne dla aplikacji interfejsu API usługi aplikacji. Następnego samouczka z działaniem serii wprowadzenie przedstawiono sposoby zaimplementować [Uwierzytelnianie użytkownika w interfejsu API usługi aplikacji](app-service-api-dotnet-user-principal-auth.md).
