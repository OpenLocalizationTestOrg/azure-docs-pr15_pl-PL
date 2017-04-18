<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Typy aplikacji można tworzyć w B2C Azure Active Directory."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Aplikacje tego typu

Azure B2C usługi Active Directory (Azure AD) obsługuje uwierzytelnianie dla różnych architektury aplikacji nowoczesny. Każdy z nich są oparte na standardowych protokołów branżowe [OAuth 2.0](active-directory-b2c-reference-protocols.md) lub [Łączenie OpenID](active-directory-b2c-reference-protocols.md). W tym dokumencie opisano krótko typy aplikacji, które można tworzyć, niezależnie od języka lub platformy wolisz. Pomaga także opis wysokiego poziomu scenariusze przed możesz [rozpocząć tworzenie aplikacji](active-directory-b2c-overview.md#getting-started).

## <a name="the-basics"></a>Podstawowe informacje
Każdej aplikacji, która używa Azure AD B2C muszą być zarejestrowane w [katalogu B2C](active-directory-b2c-get-started.md) przez [Azure Portal](https://portal.azure.com/). Proces rejestracji aplikacji zbiera i przypisuje kilka wartości do aplikacji:

- **Identyfikator aplikacji** musi jednoznacznie identyfikować aplikacji.
- **Przekierowywanie URI** służącej do kierowania odpowiedzi z powrotem do aplikacji.
- Inne wartości specyficzne dla scenariusza. Aby uzyskać więcej informacji, Dowiedz się, jak [zarejestrować aplikację](active-directory-b2c-app-registration.md).

Po zarejestrowaniu aplikacji się komunikuje Azure AD przez wysłanie żądania do punktu końcowego 2.0 Azure AD:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Każdego żądania, które są wysyłane do Azure AD B2C Określa **zasady**. Zasady określa zachowanie Azure AD. Za pomocą tych punktów końcowych do utworzenia zestawu stopniu dostosować środowiska użytkownika. Wspólne zasady obejmują zapisów, logowania i Edytuj profil zasady. Jeśli nie znasz zasady, należy przeczytać o Azure AD B2C [Struktura zasad dotyczących extensible](active-directory-b2c-reference-policies.md) przed kontynuowaniem.

Interakcja każdej aplikacji z końcowym 2.0 obejmuje tego samego typu wysokiego poziomu:

1. Aplikacja kieruje użytkownika do punktu końcowego 2.0 do wykonywania [zasad](active-directory-b2c-reference-policies.md).
2. Użytkownik wykonuje zasad zgodnie z definicją zasad.
4. Aplikacja otrzymuje niektórych rodzajów tokenu zabezpieczającego od punktu końcowego 2.0.
5. Aplikacja używa tokenu zabezpieczającego dostępu do chronionych informacji lub chronionego zasobu.
6. Serwer zasobów sprawdza tokenu zabezpieczającego, aby sprawdzić, czy można udzielić dostępu.
7. Aplikacja okresowo odświeża tokenu zabezpieczającego.

<!-- TODO: Need a page for libraries to link to -->
Poniższe kroki mogą różnić się nieco zależy od typu aplikacji, którą tworzysz. Otwórz źródło bibliotek można adres szczegóły.

## <a name="web-apps"></a>Aplikacje sieci Web
W przypadku aplikacji sieci web (w tym .NET, PHP Java, dopiskiem, Python i Node.js), które znajdują się na serwerze i dostępne za pośrednictwem przeglądarki sieci Azure AD B2C obsługuje [Łączenie OpenID](active-directory-b2c-reference-protocols.md) dla wszystkich środowiska użytkownika. Ta opcja uwzględnia logowania, zapisywania i zarządzanie profilami. Azure AD B2C stosowania łączenie OpenID aplikacji sieci web inicjuje tych wrażenia wysyłając żądania uwierzytelniania do Azure AD. Wynik żądania jest `id_token`. Ten tokenu zabezpieczającego reprezentuje tożsamość użytkownika. Zawiera także informacje o użytkowniku w formie roszczeń:

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

Dowiedz się więcej o typach tokenów i roszczeń dostępne w [Odwołanie B2C token](active-directory-b2c-reference-tokens.md)aplikacji.

W aplikacji sieci web każdego wykonanie [zasad](active-directory-b2c-reference-policies.md) trwa tak wysokiego poziomu:

![Obraz torów aplikacji sieci Web](./media/active-directory-b2c-apps/webapp.png)

Sprawdzanie poprawności `id_token` przy użyciu podpisywania klucz publiczny jest odebrana Azure AD wystarcza do zweryfikowania tożsamości użytkownika. Spowoduje to także ustawienie cookie sesji, który może być używany do identyfikowania użytkownika na następnej stronie żądania.

Aby sprawdzić, w tym scenariuszu w działaniu, spróbuj wykonać jedną z sieci web app logowania przykłady w naszym [wprowadzenie sekcji](active-directory-b2c-overview.md#getting-started).

Oprócz ułatwienia prosty logowania, aplikacji serwera sieci web może być konieczne również uzyskać dostęp do usługi sieci web wewnętrznej. W tym przypadku aplikacji sieci web można wykonywać nieco inną [OpenID połączyć przepływu](active-directory-b2c-reference-oidc.md) i uzyskiwanie tokeny przy użyciu kodów autoryzacji i odświeżanie tokeny. W tym scenariuszu są przedstawione w poniższej [sekcji interfejsy API sieci Web](#web-apis).

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Interfejsy API sieci Web
Azure AD B2C umożliwia zabezpieczanie usług sieci web, takich jak RESTful Twojej aplikacji interfejsu API sieci web. Interfejsy API sieci Web umożliwia OAuth 2.0 bezpiecznego swoje dane przez uwierzytelnianie tokeny przychodzące żądania HTTP. Wywołujący interfejsu API sieci web dołącza tokenu w nagłówku autoryzacji żądania HTTP:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Interfejsu API sieci web można używać tokenu konieczność zweryfikowania tożsamości rozmówcy interfejsu API i aby wyodrębnić informacje o osobie dzwoniącej z oświadczeń, które są kodowane tokenu. Dowiedz się więcej o typach tokenów i roszczeń dostępne w [Odwołanie token Azure AD B2C](active-directory-b2c-reference-tokens.md)aplikacji.

> [AZURE.NOTE]
    Azure AD B2C obsługuje obecnie web interfejsów API, które są dostępne dla znanych klientów. Na przykład ukończono aplikacji mogą zawierać aplikacji w systemie iOS, Android aplikacji i wewnętrznej interfejsu API sieci web. Ta architektura jest w pełni obsługiwany. Zezwalanie partnera klienta, takich jak innej aplikacji systemu iOS uzyskać dostęp do tej samej sieci, które interfejsu API nie jest obecnie obsługiwane. Wszystkie składniki wykonania aplikacji muszą mieć identyfikator jednej aplikacji

Interfejsu API sieci web, można otrzymywać tokeny wiele typów klientów, w tym aplikacji sieci web, pulpitu i aplikacji dla urządzeń przenośnych aplikacji pojedynczej strony, demonów po stronie serwera i innych interfejsów API sieci web. Oto przykładowy przepływ wykonania dla aplikacji sieci web, który wymaga interfejsu API sieci web:

![Obraz torów interfejsu API sieci Web aplikacji sieci Web](./media/active-directory-b2c-apps/webapi.png)

Aby dowiedzieć się więcej na temat kody autoryzacji, tokeny odświeżania i procedura uzyskiwania tokenów, przeczytaj temat [OAuth 2.0 Protocol (protokół)](active-directory-b2c-reference-oauth-code.md).

Aby dowiedzieć się, jak zabezpieczyć interfejsu API sieci web przy użyciu Azure AD B2C, zapoznaj się z sieci web samouczki interfejsu API w naszym [wprowadzenie sekcji](active-directory-b2c-overview.md#getting-started).

## <a name="mobile-and-native-apps"></a>Telefon komórkowy i natywnej aplikacji
Aplikacje, które są zainstalowane na urządzeniach, takich jak aplikacje dla urządzeń przenośnych i stacjonarnych, często potrzebują dostępu do usług wewnętrznej lub sieci web interfejsy API w imieniu użytkowników. Możesz dodać środowiska zarządzania tożsamości niestandardowych do natywnej usługami i aplikacjami bezpieczne połączenie wewnętrznej przy użyciu Azure AD B2C i [Przepływ kod autoryzacji OAuth 2.0](active-directory-b2c-reference-oauth-code.md).  

W tym przepływie aplikacja wykonuje [zasad](active-directory-b2c-reference-policies.md) i odbiera `authorization_code` z Azure AD po jego zakończeniu zasady. `authorization_code` Reprezentuje uprawnienia aplikacji do połączeń usług wewnętrznej w imieniu użytkownika, który jest obecnie zalogowany. Następnie wymieniający aplikacji `authorization_code` w tle dla `id_token` i `refresh_token`.  Korzystając z aplikacji `id_token` do uwierzytelniania wewnętrznej interfejsu API sieci web w żądania HTTP. Można także użyć `refresh_token` uzyskanie nowego `id_token` wygaśnięcia starsze jeden.

> [AZURE.NOTE]
    Azure AD B2C obecnie obsługuje tylko tokeny, które pozwalają uzyskać dostęp do usługi sieci web wewnętrznej o aplikacji. Na przykład ukończono aplikacji mogą zawierać aplikacji w systemie iOS, Android aplikacji i wewnętrznej interfejsu API sieci web. Ta architektura jest w pełni obsługiwany. Zezwalanie aplikacji iOS na dostęp do sieci web partnera interfejsu API przy użyciu tokeny dostępu OAuth 2.0 nie jest obecnie obsługiwane. Wszystkie składniki wykonania aplikacji muszą mieć identyfikator jednej aplikacji

![Obraz torów natywnej aplikacji](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Ograniczenia w bieżącym
Azure AD B2C obecnie nie obsługuje następujących typów aplikacji, ale znajdują się na roadmapy. Ograniczenia dotyczące Azure AD B2C i dodatkowe ograniczenia, które zostały opisane w [ograniczenia](active-directory-b2c-limitations.md).

### <a name="single-page-apps-javascript"></a>Aplikacje pojedynczej strony (JavaScript)
Wiele aplikacji nowoczesny mają fronton aplikacji jednostronicowy przede wszystkim napisana JavaScript. Ramy, takich jak AngularJS, Ember.js lub Durandal są często używane. Ogólnie dostępne usługi Azure AD obsługuje te aplikacje przy użyciu przepływu niejawne OAuth 2.0. Jednak tego przepływu nie jest jeszcze dostępne w Azure AD B2C.

### <a name="daemonsserver-side-apps"></a>Aplikacje demonów i po stronie serwera
Aplikacje zawierające długotrwałych procesów lub bez obecności użytkownika, które działają muszą również sposobem uzyskania dostępu do zabezpieczonych zasobów, takich jak interfejsy API sieci web. Te aplikacje można uwierzytelnić i uzyskać tokenów przy użyciu tożsamości aplikacji (zamiast delegowanej tożsamość użytkownika) i przy użyciu klienta OAuth 2.0 poświadczenia przepływ.

Ten przepływ nie jest obecnie obsługiwane przez Azure AD B2C. Te aplikacje można uzyskać tokenów tylko wtedy, gdy wystąpił przepływu użytkownika interakcyjnego.

### <a name="web-api-chains-on-behalf-of-flow"></a>Interfejsu API sieci Web jest dołączony (w imieniu z przepływ)
Wiele architektury zawierać interfejsu API, który należy połączeń innej podrzędne interfejsu API sieci web, gdzie są oba zabezpieczone Azure AD B2C w sieci web. W tym scenariuszu jest wspólny natywnych klientów, którzy mają wewnętrzną interfejs API sieci Web. Następnie wymaga usługi online firmy Microsoft, takie jak interfejs API Azure AD wykresu.

W tym scenariuszu łańcuchowej interfejsu API sieci web są obsługiwane za pomocą udzielanie poświadczeń okaziciela OAuth 2.0 JWT, nazywane także w imieniu z przepływ.  Jednak przepływu w imieniu z nie jest obecnie zaimplementowana w Azure AD B2C.
