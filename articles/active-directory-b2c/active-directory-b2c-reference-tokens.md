<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Typy tokenów wydawane w B2C Azure Active Directory."
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
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>


# <a name="azure-ad-b2c-token-reference"></a>Azure AD B2C: Token odwołania

Azure Active Directory (Azure AD) B2C emituje kilka typów tokenów zabezpieczających podczas przetwarzania każdy [Przepływ uwierzytelniania](active-directory-b2c-apps.md). W tym dokumencie opisano format, właściwości zabezpieczeń i zawartości każdego typu token.

## <a name="types-of-tokens"></a>Typy tokenów

Azure AD B2C obsługuje [Protokół OAuth 2.0](active-directory-b2c-reference-protocols.md), korzystające z zarówno tokeny dostępu i tokeny odświeżania. Obsługuje również uwierzytelnianie i logowania za pośrednictwem [Połączenia OpenID](active-directory-b2c-reference-protocols.md), które wprowadza trzeci typ tokenu: token Identyfikatora. Każdy z tych tokenów jest reprezentowane jako token okaziciela.

Token okaziciela jest tokenu zabezpieczającego lightweight, którego udziela dostępu "okaziciela" do chronionego zasobu. Okaziciela jest każda strona, która może prezentować tokenu. Azure AD najpierw uwierzytelnienia przyjęcie może odbierać token okaziciela. Jednak jeśli kroków, które nie są wykonywane zapewnienie token na przekazywanie i przestrzeni dyskowej, może być przechwycone i używane przez niezamierzonych firmy. Niektóre tokenów zabezpieczających ma wbudowane mechanizm uniemożliwia nieupoważnione za ich pomocą, ale tokeny okaziciela nie tego mechanizmu. Muszą być przewożone w bezpiecznego kanału, takie jak transportu warstwy zabezpieczeń (HTTPS).

Jeśli token okaziciela są przesyłane poza bezpiecznego kanału, złośliwy strony umożliwia ataki w pośrednik uzyskiwanie tokenu i używać go do uzyskania nieautoryzowanego dostępu do chronionych zasobów. Te same zasady zabezpieczeń dotyczą sytuacji okaziciela tokeny są przechowywane lub do późniejszego użycia. Zawsze upewnij się, że aplikacji przesyła i przechowuje tokeny okaziciela w bezpieczny sposób.

Aby uzyskać informacje dodatkowe zabezpieczenia na okaziciela tokenów zobacz [RFC 6750 sekcji 5](http://tools.ietf.org/html/rfc6750).

Wiele tokeny Azure AD B2C problemy są wykonywane jako JSON web tokeny (JWTs). JWT jest sposób niewielkie, bezpiecznych adres URL przekazywania informacji między dwiema stronami. JWTs zawierają informacje znana pod nazwą roszczeń. Są to potwierdzeń informacji na temat okaziciela i temat token. Roszczeń w JWTs są JSON obiektów, które są kodowane i seryjnych transmisji. Ponieważ JWTs wydany przez Azure AD B2C utworzył, ale nie szyfrowane, można łatwo sprawdzić zawartość JWT debugowania go. Dostępnych jest kilka narzędzi który zrobić, łącznie z [calebb.net](http://calebb.net). Aby uzyskać więcej informacji na temat JWTs dotyczą [Specyfikacje JWT](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Tokeny identyfikator

Token identyfikator jest formą tokenu zabezpieczającego, pobierającego aplikacji z Azure AD B2C `authorize` i `token` punktów końcowych. Identyfikator tokeny są przedstawiane jako [JWTs](#types-of-tokens)i zawierają roszczeń, które służą do identyfikowania użytkowników w aplikacji. Gdy identyfikator tokeny są uzyskane z `authorize` punktu końcowego, są często używane do zalogowania się użytkownikom aplikacji sieci web. Gdy identyfikator tokeny są uzyskane z `token` punktu końcowego, mogą być wysyłane w żądania HTTP podczas komunikacji między dwoma części tej samej aplikacji lub usługi. Roszczeń służy identyfikator tokenu odpowiednio do potrzeb. Często są one używane, aby wyświetlić informacje o koncie lub aby sterować dostępem w aplikacji.  

Identyfikator tokeny są podpisane, ale nie są szyfrowane w tej chwili. Usługi aplikacji lub interfejsu API otrzymuje token Identyfikatora, musi, [Sprawdź poprawność podpisu](#token-validation) , aby udowodnić, że token jest autentyczny. Z aplikacji lub interfejsu API również sprawdzić poprawność kilka oświadczeniach tokenu, aby udowodnić, że jest prawidłowy. W zależności od potrzeb scenariusz oświadczeniach sprawdzone przez aplikację może być różna, ale aplikacji należy wykonać kilka [typowych poprawności roszczeń](#token-validation) w każdej sytuacji.

#### <a name="sample-id-token"></a>Przykładowy identyfikator tokenu
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Tokeny dostępu

Token dostępu jest również formularzem tokenu zabezpieczającego, pobierającego aplikacji z Azure AD B2C `authorize` i `token` punktów końcowych. Tokeny dostępu są również przedstawiane jako [JWTs](#types-of-tokens)i zawierają roszczeń, które służą do identyfikowania użytkowników w swojej usług sieci web i interfejsów API.

Korzystasz z tokeny dostępu, ale nie są szyfrowane w tej chwili —, jak i bardzo podobne do tokeny identyfikator.  Tokeny dostępu należy umożliwia dostęp do usług sieci web i interfejsów API i zidentyfikować i uwierzytelnienia użytkownika w tych usług.  Jednak nie zapewniają wszelkie potwierdzenia zgody na tych usług.  To znaczy, że `scp` roszczenia tokeny dostępu ograniczyć lub nie w przeciwnym razie reprezentują uprawnienia dostępu, których przypisano temat tokenu.

Gdy z interfejsu API odbierze token dostępu, musi [sprawdzić poprawność podpisu](#token-validation) , aby udowodnić, że token jest autentyczny. Z interfejsu API należy zweryfikować kilka oświadczeniach tokenu, aby udowodnić, że jest prawidłowy. W zależności od potrzeb scenariusz oświadczeniach sprawdzone przez aplikację może być różna, ale aplikacji należy wykonać kilka [typowych poprawności roszczeń](#token-validation) w każdej sytuacji.

### <a name="claims-in-id--access-tokens"></a>Roszczeń w nazwę i tokeny dostępu

Gdy używasz Azure AD B2C masz precyzyjne sterowanie zawartość z tokenów. Można skonfigurować [zasady](active-directory-b2c-reference-policies.md) wysyłanie niektórych zestawów danych użytkownika roszczeń wymagane dla jego operacji aplikacji. Roszczenia te mogą zawierać standardowe właściwości, takie jak użytkownika `displayName` i `emailAddress`. Można także dołączyć [atrybuty niestandardowe użytkownika](active-directory-b2c-reference-custom-attr.md) podany w katalogu B2C. Każdy identyfikator i access token otrzymany będzie zawierać określonego zestawu roszczeń związanych z zabezpieczeniami. Aplikacje mogą być bezpiecznego uwierzytelniania użytkowników i żądań tych roszczeń.

Zauważ, że roszczeń w tokeny identyfikator nie są zwracane w określonej kolejności. Ponadto nowych wniosków mogą zostać wprowadzone w tokeny identyfikator, w dowolnym momencie. Nie należy przerwać aplikacji, wprowadzoną nowych wniosków. Poniżej przedstawiono roszczeń, które powinny być istnieje w nazwę i access tokeny wydawany przez Azure AD B2C. Wszelkie dodatkowe roszczenia zależą od zasad. Ćwiczenia spróbuj sprawdzanie roszczeń w token Identyfikatora przykładowe wklejając go w [calebb.net](http://calebb.net). Dodatkowe informacje można znaleźć w [specyfikacji OpenID połączenia](http://openid.net/specs/openid-connect-core-1_0.html).

| Nazwa | Roszczeń | Przykładowa wartość | Opis |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Grupy odbiorców | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Roszczeń odbiorców służy do identyfikowania adresata token. Dla Azure AD B2C grupy odbiorców jest identyfikator aplikacji usługi aplikacji, przypisane do aplikacji w portalu rejestracji aplikacji. Aplikacji należy sprawdzić poprawność tej wartości i odrzucanie tokenu, jeśli nie odpowiada. |
| Wystawcy | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | To zastrzeżenie identyfikuje usługę tokenu zabezpieczającego (STS), który tworzy i zwraca token. Identyfikuje katalogu Azure AD, w którym został uwierzytelniony użytkownik. Aplikacji, należy sprawdzić, czy oświadczeń wystawcy, aby upewnić się, że token pochodzi od punktu końcowego 2.0. |
| Wydane w | `iat` | `1438535543` | To zastrzeżenie jest czas co token został wystawiony, przedstawione w czasie epoch. |
| Czas wygaśnięcia | `exp` | `1438539443` | Czas wygaśnięcia, który roszczeń jest czasu, w którym tokenu staje się nieprawidłowe, przedstawione w czasie epoch. Aby sprawdzić ważność okres ważności tokenu aplikacji należy użyć to zastrzeżenie.  |
| Nie wcześniej niż | `nbf` | `1438535543` | To zastrzeżenie jest czas co token staje się prawidłowy, reprezentowanego w czasie epoch. Zazwyczaj jest taki sam, jak razem, gdy token został wystawiony. Aby sprawdzić ważność okres ważności tokenu aplikacji należy użyć to zastrzeżenie.  |
| Wersja | `ver` | `1.0` | To jest wersja token Identyfikatora, zdefiniowane przez Azure AD. |
| Kod skrótu | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Skrót kod znajduje się w token identyfikator tylko wtedy, gdy token jest wydawany razem z kodem autoryzacji OAuth 2.0. Kod skrótu może służyć do sprawdzania poprawności autentyczności kodu autoryzacji. Zobacz [Specyfikacja łączenie OpenID](http://openid.net/specs/openid-connect-core-1_0.html) uzyskać więcej szczegółowych informacji na temat wykonywania tego sprawdzania poprawności. |
| Skrót token dostępu | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Skrót token dostępu znajduje się w token identyfikator tylko wtedy, gdy token jest wydawany razem z token dostępu OAuth 2.0. Skrót token dostępu może służyć do sprawdzania poprawności autentyczność token dostępu. Zobacz [Specyfikacja łączenie OpenID](http://openid.net/specs/openid-connect-core-1_0.html) uzyskać więcej szczegółowych informacji na temat wykonywania tego sprawdzania poprawności. |
| Identyfikator jednorazowy | `nonce` | `12345` | Identyfikator jednorazowy jest strategię umożliwia ograniczanie atakami token powtarzania. Aplikacji można określić identyfikatora jednorazowego w wezwaniu autoryzacji za pomocą `nonce` kwerenda parametryczna. Wartości podane w wezwaniu będą emitować za w niezmienionej postaci, w `nonce` rościć sobie tylko identyfikator tokenu. Dzięki aplikacji do weryfikacji wartości względem wartość, która go określonej na żądanie, które kojarzy sesji aplikacji z danym token Identyfikatora. Aplikacji należy wykonać ten sprawdzania poprawności podczas procesu token sprawdzania poprawności Identyfikatora. |
| Temat | `sub` | `Not supported currently. Use oid claim.` | Jest to podmiot, o którym tokenu potwierdza informacje, takie jak użytkownika aplikacji. Ta wartość jest niezmienny i nie można przydzielić ponownie, ani ponownie. Może służyć do sprawdzania autoryzacji bezpiecznie, takich jak gdy tokenu jest używany do uzyskiwania dostępu do zasobu. Jednak roszczeń temat nie jest jeszcze zaimplementowana w Azure AD B2C. Należy skonfigurować zasad do uwzględnienia identyfikator obiektu `oid` przejmowanie i jej wartość umożliwia określenie użytkowników, a nie za pomocą temat wniosek o zezwolenie. |
| Uwierzytelnianie kontekstu zajęć | `acr` | `b2c_1_sign_in` | Jest to nazwa zasady, które został użyty w celu uzyskania token Identyfikatora.  |
| Uwierzytelniania | `auth_time` | `1438535543` | To zastrzeżenie jest czas co ostatniej poświadczeń wprowadzone użytkownika, przedstawione w czasie epoch. |


### <a name="refresh-tokens"></a>Odświeżanie tokenów

Odświeżanie tokeny są tokenów zabezpieczających, które aplikacji umożliwia uzyskiwanie nowych tokeny identyfikator i uzyskać dostęp do tokenów w strumieniu OAuth 2.0. Zapewniają aplikacji długoterminowe dostępu do zasobów w imieniu użytkowników bez konieczności interakcji z tych użytkowników.

Do odbierania odświeżania token w token odpowiedzi aplikacji musisz poprosić `offline_acesss` zakres. Aby dowiedzieć się więcej o `offline_access` zakres, poszukaj [Azure AD B2C protokół odwołania](active-directory-b2c-reference-protocols.md).

Odświeżanie tokeny są i będzie zawsze, całkowicie nieprzezroczysty do aplikacji. Są wydawane przez Azure AD i mogą być sprawdzane pod i interpretowane tylko przez Azure AD. Są one długotrwałe, ale aplikacji nie powinny być zapisywane przy założeniu, że tokenu odświeżanie trwa w określonym okresie. W dowolnym momencie dla różnych powodów, dla których może być unieważnione tokeny odświeżania. Jedynym sposobem na dodanie aplikacji do ustalić, czy token odświeżania jest prawidłowy jest próbuje wykorzystać wprowadzając żądania tokenu w Azure AD.

Gdy zrealizować token odświeżania, aby uzyskać nowy token (i jeśli udzielono aplikacji `offline_access` zakres), w token odpowiedzi zostanie wyświetlony nowy token odświeżania. Należy zapisać token nowo wydane odświeżania. Go należy zastąpić tokenu odświeżania uprzednio użytym w wezwaniu. Pomaga to zagwarantować usługi tokeny odświeżania i ważność tak długo.

## <a name="token-validation"></a>Sprawdzanie poprawności tokenu

Aby sprawdzić poprawność token, aplikacji należy sprawdzić podpis i oświadczeń token.

Wiele bibliotek Otwórz źródło są dostępne sprawdzania poprawności JWTs, w zależności od preferowany język. Zaleca się, że możesz wyświetlić tych opcji zamiast zaimplementować logiki sprawdzania poprawności. Informacje zawarte w tym przewodniku pomoże Ci dowiedzieć się, jak prawidłowo za pomocą tych bibliotek.

### <a name="validate-the-signature"></a>Sprawdź poprawność podpisu
JWT zawiera trzy segmenty, oddzielając je `.` znaków. Pierwszego odcinka jest **Nagłówek**, drugim jest **treści**, a trzecia jest **Podpis**. Części podpis może służyć do sprawdzania poprawności autentyczności tokenu, tak aby może być zaufany przez aplikację.

Azure AD B2C tokeny są zalogowani przy użyciu standardowych asymetrycznym algorytmy, takich jak RSA 256. Nagłówek tokenu zawiera informacje dotyczące metody klucza i szyfrowania używany do podpisywania tokenu:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

`alg` Oświadczeń wskazuje algorytm, który został użyty do podpisania token. `kid` Roszczeń wskazuje określonego klucz publiczny, którego użyto do podpisywania tokenu.

W dowolnym momencie Azure AD zarejestrować tokenu przy użyciu jednego z określonego zestawu par kluczy publicznych i prywatnych. Azure AD obraca możliwe zestaw kluczy okresowo, więc aplikacji powinna zostać zapisana automatycznie obsługiwać tych najważniejszych zmian. Rozsądne częstotliwość ma sprawdzać dostępność aktualizacji do kluczy publicznych używane przez Azure AD jest co 24 godziny.

Azure AD B2C ma punktu końcowego łączenie OpenID metadanych. Dzięki temu na uzyskiwanie zdalnego dostępu do informacji na temat Azure AD B2C w czasie wykonywania aplikacji. Te informacje zawiera punkty końcowe, zawartość tokenów i token podpisywania. Katalogu B2C zawiera dokument metadanych JSON dla każdej zasady. Na przykład dokument metadanych dla `b2c_1_sign_in` zasad w `fabrikamb2c.onmicrosoft.com` znajduje się w:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`jest to katalog B2C używane do uwierzytelnienia użytkownika, a `b2c_1_sign_in` zasad umożliwia uzyskiwanie tokenu. Aby określić zasady, które został użyty do podpisania tokenu (i miejsce docelowe do pobierania metadanych), masz do wyboru dwie opcje. Najpierw nazwę zasady znajduje się w `acr` przejmowania tokenu. Roszczeń związanych z treści JWT można analizować, base-64 dekodowanie treści i deserializacji ciągu JSON, którego wynikiem. `acr` Roszczeń będzie nazwę zasady, użytego do wykonania tokenu.  Jest kodowanie zasad wartości z innych opcji `state` parametru w przypadku problemów żądanie, a następnie dekodowanie go, aby określić, które zasady został użyty. Każda z tych metod jest prawidłowy.

Dokument metadanych jest obiektem JSON, który zawiera kilka przydatne rodzajów informacji. Należą do lokalizacji wymagane do uwierzytelniania OpenID łączenie punktów końcowych. Zawierają również `jwks_uri`, dające lokalizację zestawu kluczy publicznych są używane do podpisywania tokenów. Poniżej podano lokalizacji, że warto pobierać lokalizacji dynamiczne przy użyciu dokument metadanych i analizowania się `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

Znajduje się pod tym adresem URL dokumentu JSON zawiera wszystkie informacje o kluczu publicznym używany w danym momencie. Korzystając z aplikacji `kid` przejmowania w nagłówku JWT, aby zaznaczyć klucz publiczny w dokumencie JSON, który jest używany do podpisywania tokenu określonego. Następnie można wykonywać sprawdzanie poprawności podpisu przy użyciu poprawne kluczem publicznym i algorytmu wskazanej.

Opis sposobu sprawdzana poprawność podpisu wykracza poza zakres tego dokumentu. Wiele bibliotek Otwórz źródło są dostępne dla ułatwiają z tym, jeśli jest potrzebna.

### <a name="validate-the-claims"></a>Sprawdź poprawność roszczeń
Kiedy usługi aplikacji lub interfejsu API odbiera token identyfikator, go również wykonać kilka testy z roszczeń tokenu identyfikator. Te obejmują, ale nie są ograniczone do:

- Roszczeń **odbiorców** : sprawdza, czy token Identyfikatora mają mieć do aplikacji.
- Roszczeń **nie wcześniej niż** i **czas wygaśnięcia** : Sprawdź te nie wygasł token Identyfikatora.
- Roszczeń **wystawcy** : sprawdza, czy token został wystawiony aplikacji, przez Azure AD.
- **Identyfikator jednorazowy**: jest to strategii łagodzenia atakiem token powtarzania.

Pełną listę reguły sprawdzania poprawności, które należy wykonać aplikacji można znaleźć w [specyfikacji OpenID połączenia](https://openid.net). Szczegółowe informacje o wartości oczekiwane dla tych roszczeń są uwzględniane w poprzedniej [sekcji token](#types-of-tokens).  

## <a name="token-lifetimes"></a>Token istnienia

Następujące istnienia token służą do dalszych swoją wiedzę. Prowadnice ułatwiają po projektowanie i sprawdzanie aplikacji. Należy zauważyć, że nie można zapisać swoje aplikacje mogą się spodziewać dowolnego z tych istnienia pozostać stały. Mogą i ulegnie zmianie.  Więcej informacji o dostosowanie token istnienia w Azure AD B2C [tutaj](active-directory-b2c-token-session-sso.md).

| Token | Czas użytkowania | Opis |
| ----------------------- | ------------------------------- | ------------ |
| Tokeny identyfikator | Jedna godzina | Identyfikator tokeny są zwykle prawidłowe przez godzinę. Aplikacji sieci web umożliwia tym istnienia Obsługa własnej sesji użytkownikom (zalecane). Możesz również wybrać istnienia innej sesji. Jeśli aplikacji musi uzyskać nowy identyfikator token, po prostu musi zostać Azure AD nowe żądanie logowania. Jeśli użytkownik ma prawidłową przeglądarką z usługą Azure Active Directory, użytkownik nie będzie musiał ponownie wprowadzać poświadczeń. |
| Odświeżanie tokenów | Maksymalnie 14 dni | Token pojedynczy odświeżania jest prawidłowy dla maksymalnie 14 dni. Jednak token odświeżania może stać się nieprawidłowe w dowolnym momencie dla dowolnej liczby powody, dla których. Aplikacji powinna nadal próbie użycia token odświeżania do momentu żądanie kończy się niepowodzeniem, lub aplikacji zamienia token odświeżania na nową.  Token odświeżania również może stać się nieprawidłowe po 90 dni od ostatnio wprowadzone poświadczenia użytkownika. |
| Kody autoryzacji | Pięć minut | Kody autoryzacji są celowo krótkotrwałe. Ich należy można zrealizować natychmiast tokeny dostępu, tokeny identyfikator lub Odśwież tokeny po ich otrzymaniu. |
