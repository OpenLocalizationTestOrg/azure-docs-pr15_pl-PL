<properties
    pageTitle="Typy punkt końcowy 2.0 | Microsoft Azure"
    description="Typy aplikacji i scenariusze obsługiwane przez punkt końcowy 2.0 Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="types-of-apps-for-the-v20-endpoint"></a>Typy aplikacji dla punktu końcowego 2.0
Punkt końcowy 2.0 obsługuje uwierzytelnianie dla różnych architektury aplikacji nowoczesny, które są oparte na standardowych protokołów branżowe [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) i/lub [Łączenie OpenID](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  Ten dokument także pokrótce opisano typy aplikacje, które można tworzyć, niezależnie od języka lub platformy wolisz.  Ułatwi to opis wysokiego poziomu scenariusze przed możesz [od razu do kodu przejść](active-directory-appmodel-v2-overview.md#getting-started).

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Podstawowe informacje
Co aplikację, która używa punkt końcowy 2.0 muszą być zarejestrowane na [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Proces rejestracji aplikacji będzie zbieranie i przypisać kilka wartości do aplikacji:

- **Identyfikator aplikacji** musi jednoznacznie identyfikować aplikacji
- **Przekierowywanie identyfikatora URI** , który może być używany w celu skierowania odpowiedzi z powrotem do aplikacji
- Kilka innych wartości specyficzne dla scenariusza.  Aby uzyskać więcej szczegółów, Dowiedz się, jak [zarejestrować aplikację](active-directory-v2-app-registration.md).

Po zarejestrowaniu, aplikacja komunikuje się z Azure AD przez wysłanie żądania do punktu końcowego usługi Azure Active Directory 2.0.  Firma Microsoft udostępnia RAM Otwórz źródło i biblioteki, zajmowała szczegóły te żądania lub można zaimplementować logiczny uwierzytelniania samodzielnie przy rzemieślniczy żądania te punkty końcowe:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Aplikacje sieci Web
Aplikacjach sieci web (.NET PHP, Java, dopiskiem, Python, węzeł, itp), które są dostępne za pośrednictwem przeglądarki sieci mogą wykonywać użytkownika logowania przy użyciu funkcji [OpenID połączenie](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  W OpenID łączenie aplikacji sieci web otrzymuje `id_token`, tokenu zabezpieczającego, która sprawdza tożsamość użytkownika, a także informacje o użytkowniku w formie roszczeń:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Aby Dowiedz się więcej o wszystkich typów tokenów i roszczeń dostępne w [odwołania do tokenu 2.0](active-directory-v2-tokens.md)aplikacji.

W sieci web apps server przepływ uwierzytelniania logowania trwa tak wysokiego poziomu:

![Obraz torów aplikacji sieci Web](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Sprawdzanie poprawności id_token przy użyciu publicznego klucza podpisywania otrzymane od punktu końcowego 2.0 jest wystarczające dla zapewnienia tożsamość użytkownika i ustawić cookie sesji, który może być używany do identyfikowania użytkownika na następnej stronie żądania.

Aby sprawdzić, w tym scenariuszu w działaniu, wypróbuj jedną z sieci web app logowania przykłady w naszym sekcji [Wprowadzenie](active-directory-appmodel-v2-overview.md#getting-started) .

Oprócz prostych logowania aplikacji serwera sieci web może być konieczne również uzyskać dostęp do niektórych inne usługi sieci web, takich jak interfejsu API usługi REST.  W tym przypadku aplikacji serwera sieci web można prowadzić Scalonej łączenie OpenID i OAuth 2.0 przepływu, przy użyciu [kodu autoryzacji 2.0 OAuth przepływu](active-directory-v2-protocols.md#oauth2-authorization-code-flow). W tym scenariuszu omówiono poniżej naszych [Wprowadzenie do aplikacji sieci Web WebAPI tematu](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Interfejsy API sieci Web
Punkt końcowy 2.0 — umożliwia bezpiecznego usług sieci web oraz, takie jak interfejs API usługi aplikacji RESTful sieci Web.  Zamiast sesji i id_tokens pliki cookie interfejsy API sieci Web umożliwia access_tokens OAuth 2.0 Zabezpieczanie danych i uwierzytelniania przychodzących żądań.  Wywołujący interfejs API sieci Web dołącza access_token w nagłówku autoryzacji żądania HTTP:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Interfejs API sieci Web można użyć do zweryfikowania tożsamości rozmówcy interfejsu API i wyodrębnić informacje o osobie dzwoniącej z oświadczeń, które są kodowane w access_token access_token.  Aby Dowiedz się więcej o wszystkie typy tokenów i roszczeń, które są dostępne w [odwołania do tokenu 2.0](active-directory-v2-tokens.md)aplikacji.

Interfejs API sieci Web można udostępnić użytkownikom możliwości w i rezygnacji niektórych funkcji lub dane umożliwiając uprawnienia, znanej także jako [zakresy](active-directory-v2-scopes.md).  Wywołujący aplikacji umożliwia uzyskanie uprawnień do zakresu użytkownik musi wyraża zgodę na zakres w strumieniu.  Punkt końcowy 2.0 będzie zajmować pytania użytkownika o zgodę i rejestrowania tych uprawnień w wszystkie access_tokens, otrzymywanych interfejs API sieci Web.  Po wyświetleniu wszystkich interfejs API sieci Web należy pamiętać o jest sprawdzanie poprawności access_tokens, otrzymywanych w każdej konwersacji i metodę sprawdzania autoryzacji pisane z wielkiej litery.

Interfejs API sieci Web, można otrzymywać access_tokens ze wszystkich typów aplikacji, w tym aplikacji serwera sieci web, pulpitu i aplikacji dla urządzeń przenośnych aplikacji pojedynczej strony, demonów po stronie serwera i inne interfejsy API sieci Web.  Wysokiego poziomu przepływ uwierzytelniania interfejsu api sieci web jest w następujący sposób:

![Obraz torów interfejsu API sieci Web](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Aby dowiedzieć się więcej na temat authorization_codes, refresh_tokens i uzyskać szczegółowe instrukcje pobierania access_tokens, przeczytaj temat [OAuth 2.0 Protocol (protokół)](active-directory-v2-protocols-oauth-code.md).

Aby dowiedzieć się, jak bezpiecznego sieci web interfejsu api z access_tokens uwierzytelnianie OAuth2, zapoznaj się z przykłady kodu interfejsu api sieci web w naszym [sekcji wprowadzenie](active-directory-appmodel-v2-overview.md#getting-started).


## <a name="mobile-and-native-apps"></a>Telefon komórkowy i natywnej aplikacji
Aplikacje, które są zainstalowane na urządzeniu, takim jak aplikacje dla urządzeń przenośnych i stacjonarnych, często potrzebują dostępu do usług wewnętrznej bazy danych lub interfejsów API sieci Web, które przechowywanie danych i wykonywać różne funkcje w imieniu użytkownika.  Te aplikacje można dodać logowania i autoryzacji do usług wewnętrznej bazy danych przy użyciu [kodu autoryzacji 2.0 OAuth przepływu](active-directory-v2-protocols-oauth-code.md).  

W tym przepływie aplikacji odbiera authorization_code od punktu końcowego 2.0 na użytkownika logowania, co stanowi uprawnienia aplikacji do połączeń usług wewnętrznej bazy danych w imieniu użytkownika obecnie zalogowany.  Następnie można się wymieniać authoriztion_code w tle access_token OAuth 2.0 i refresh_token aplikacji.  Aplikacja służy access_token do uwierzytelniania do interfejsów API usługi sieci Web w żądania HTTP i umożliwia uzyskiwanie nowych access_tokens po wygaśnięciu tych starszych refresh_token.

![Obraz torów natywnej aplikacji](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Aplikacje pojedynczej strony (javascript)
Wiele aplikacji nowoczesny mają pojedynczej strony aplikacji (SPA) frontonu napisanym przede wszystkim w javascript i często korzystasz ram takich jak AngularJS, Ember.js, Durandal itd.  Punkt końcowy 2.0 Azure AD obsługuje te aplikacje za pomocą [Niejawnego przepływ OAuth 2.0](active-directory-v2-protocols-implicit.md).

W tym przepływie aplikacji otrzymuje tokenów z 2.0 Autoryzuj punktu końcowego bezpośrednio, bez wykonywania dowolnej wymiany do serwera wewnętrznej bazy danych.  Dzięki temu wszystkie logiczny uwierzytelniania i obsługi sesji podjęcie umieścić wyłącznie w kliencie javascript bez wykonywania dodatkowych Strona przekierowania.

![Obraz torów niejawne przepływ](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Aby sprawdzić, w tym scenariuszu w działaniu, wypróbuj jedną z przykłady kodu aplikacji pojedynczej strony w naszym sekcji [Wprowadzenie](active-directory-appmodel-v2-overview.md#getting-started) .

### <a name="daemonsserver-side-apps"></a>Aplikacje po stronie serwera demonów
Aplikacje zawierających długotrwałych procesów lub bez obecności użytkownika, które działają muszą również sposobem uzyskania dostępu do zabezpieczonych zasoby, takie jak interfejsy API sieci Web.  Te aplikacje można uwierzytelniania i pobieranie elementów przy użyciu tożsamości aplikacji (zamiast delegowanej tożsamość użytkownika) przy użyciu klienta OAuth 2.0 poświadczenia przepływ.

W tym przepływie aplikacji uzyskuje tokeny przez interakcja bezpośrednio z `/token` punkt końcowy:

![Demon aplikacji torów obrazu](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Aby utworzyć aplikację demona, zobacz documeenation poświadczeń klienta w naszym sekcji [Wprowadzenie](active-directory-appmodel-v2-overview.md#getting-started) lub odwołują się do [tej aplikacji przykładowych .NET](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

## <a name="current-limitations"></a>Ograniczenia w bieżącym
Poniższe typy aplikacji nie są obecnie obsługiwane przez punkt końcowy 2.0, ale znajdują się na mapy drogowej.  Dodatkowe ograniczenia i ograniczeń dla punktu końcowego 2.0 opisane w [artykule ograniczenia 2.0](active-directory-v2-limitations.md).

### <a name="chained-web-apis-on-behalf-of"></a>Web łańcuchowej interfejsy API (w imieniu z)
Wiele architektury obejmuje interfejs API sieci Web, który należy nawiązać połączenie z inną podrzędne interfejs API sieci Web, oba zabezpieczone punkt końcowy 2.0.  W tym scenariuszu jest wspólny dla macierzystych klientów, którzy mają interfejs API sieci Web wewnętrznej bazie danych, która z kolei wywołuje usługi Microsoft Online, takiej jak usługi Office 365 lub interfejsu API wykresu.

W tym scenariuszu łańcuchowej interfejs API sieci Web są obsługiwane za pomocą udzielanie OAuth 2.0 Jwt okaziciela poświadczeń, nazywanego [Przepływ On-Behalf-Of](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow).  Jednak przepływu w imieniu-z nie jest obecnie zaimplementowana w punkt końcowy 2.0.  Aby zobaczyć, jak działa ten przepływ w ogólnodostępne Azure AD usługi, zapoznaj się z [próbki w imieniu-z kodu na GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).
