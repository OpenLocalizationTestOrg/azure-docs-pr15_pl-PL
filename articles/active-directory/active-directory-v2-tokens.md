<properties
    pageTitle="Azure AD 2.0 odwołania do tokenu | Microsoft Azure"
    description="Typy tokenów i roszczeń dostarczanych przez punkt końcowy 2.0"
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

# <a name="v20-token-reference"></a>odwołania do tokenu 2.0

Punkt końcowy 2.0 emituje kilka typów tokenów zabezpieczających przetwarzania każdy [Przepływ uwierzytelniania](active-directory-v2-flows.md). W tym dokumencie opisano format, właściwości zabezpieczeń i zawartości każdego typu token.

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

## <a name="types-of-tokens"></a>Typy tokenów

Punkt końcowy 2.0 obsługuje [Protokół OAuth 2.0](active-directory-v2-protocols.md), korzystające z access_tokens i refresh_tokens.  Obsługuje również uwierzytelnianie i logowania za pośrednictwem [Połączenia OpenID](active-directory-v2-protocols.md#openid-connect-sign-in-flow), które wprowadza trzeci typ tokenu, id_token.  Każdy z tych tokenów jest reprezentowane jako "token okaziciela".

Token okaziciela jest tokenu zabezpieczającego lightweight, którego udziela dostępu "okaziciela" do chronionego zasobu. W tym znaczeniu "okaziciela", to każda strona, która może prezentować token. Jeśli przyjęcie najpierw musi uwierzytelniania z usługą Azure Active Directory, aby otrzymywać token okaziciela, jeśli nie są podjąć kroki wymagane w celu bezpiecznego token na przekazywanie i miejsca do magazynowania, można przechwycone i używane przez niezamierzonych firmy. Gdy niektóre tokenów zabezpieczających ma wbudowane mechanizm uniemożliwia nieupoważnione korzystania z nich, tokeny okaziciela nie ma tego mechanizmu i musi transportu w bezpiecznego kanału, takie jak transportu warstwy zabezpieczeń (HTTPS). Jeśli token okaziciela są przesyłane w Wyczyść, mężczyzna w środkowym atakiem mogą być używane przez złośliwy przyjęcie uzyskać tokenu i używać go uzyskania nieautoryzowanego dostępu do chronionych zasobów. Te same zasady zabezpieczeń Zastosuj podczas przechowywania lub pamięci podręcznej tokenów okaziciela do późniejszego użycia. Zawsze upewnij się, że aplikacji przesyła i przechowuje tokeny okaziciela w bezpieczny sposób. Aby uzyskać więcej zagadnienia dotyczące zabezpieczeń na okaziciela tokenów zobacz [RFC 6750 sekcji 5](http://tools.ietf.org/html/rfc6750).

Wiele tokenów wydawany przez punkt końcowy 2.0 — są wykonywane jako tokeny Web Json lub JWTs.  JWT jest sposób niewielkie, bezpiecznych adres URL przekazywania informacji między dwiema stronami.  Informacje zawarte w JWTs są nazywane "roszczenia" i potwierdzenia informacji o okaziciela i temat tokenu.  Roszczeń w JWTs są obiektami JSON zakodowany i seryjny dla transmisji.  Ponieważ JWTs wydany przez punkt końcowy 2.0 — są zalogowani, ale nie szyfrowane, można łatwo sprawdzić zawartość JWT na potrzeby debugowania. Aby uzyskać więcej informacji o JWTs może dotyczyć [Specyfikacja JWT](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens są formie tokenu zabezpieczającego logowania, odbierająca w aplikacji do uwierzytelniania przy użyciu funkcji [OpenID połączenie](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  Są przedstawiane jako [JWTs](#types-of-tokens)i zawierają roszczeń, które służą do logowania się użytkownika do aplikacji.  Roszczeń służy w id_token jako odpowiednio do potrzeb — często są używane do wyświetlania informacji o koncie lub podejmowania decyzji kontroli dostępu w aplikacji.  Punkt końcowy 2.0 problemów tylko jeden typ id_token, który ma jednolity zestaw oświadczeń niezależnie od typu użytkownika, który podpisał w.  To znaczy, że format i zawartość id_tokens będą takie same dla obu osobistych użytkowników Account Microsoft i konta służbowego.

Id_tokens są zalogowani, ale nie są szyfrowane w tej chwili.  Aplikacji otrzymuje id_token, musi, [Sprawdź poprawność podpisu](#validating-tokens) potwierdza tokenu autentyczności i sprawdź poprawność kilka roszczeń w tokenu, aby udowodnić ważności.  Roszczeń sprawdzone przez aplikację są zależne od wymagania scenariusz, ale istnieje kilka [typowych poprawności roszczeń](#validating-tokens) aplikacji należy wykonać w każdej sytuacji.

Szczegółowe informacje na oświadczeniach w id_tokens są przedstawione poniżej, a także id_token próbki.  Zauważ, że roszczeń w id_tokens nie są zwracane w określonej kolejności.  Ponadto nowych wniosków mogą zostać wprowadzone do id_tokens w dowolnym momencie w czasie — aplikacji nie powinna zostać podzielona na wprowadzoną nowych wniosków.  Poniższa lista zawiera oświadczeń, które aplikacji niezawodne może zinterpretować podczas pisania tego dokumentu.  W razie potrzeby jeszcze więcej szczegółów można znaleźć w [specyfikacji OpenID połączenia](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Przykładowe id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [AZURE.TIP] Ćwiczenia spróbuj sprawdzanie roszczeń w id_token próbki wklejając go w [calebb.net](https://calebb.net).

#### <a name="claims-in-idtokens"></a>Roszczeń w id_tokens
| Nazwa | Roszczeń | Przykładowa wartość | Opis |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Grupy odbiorców | `aud` | `6731de76-14a6-49ae-97bc-6eba6914391e` | Służy do identyfikowania adresata tokenu.  W id_tokens grupy odbiorców jest identyfikator aplikacji usługi aplikacji, przypisane do aplikacji w portalu rejestracji aplikacji.  Aplikacji należy sprawdzić poprawność tej wartości i odrzucanie tokenu, jeśli nie odpowiada. |
| Wystawcy | `iss` | `https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` | Służy do identyfikowania usługę tokenu zabezpieczającego (STS), który tworzy i zwraca token, a także dzierżawy Azure AD, w którym został uwierzytelniony użytkownik.  Aplikacji, należy sprawdzić, czy roszczeń wystawcy, aby upewnić się, że tokenu pochodzi od punktu końcowego 2.0.  Należy również używać guid części roszczenia Aby ograniczyć zbiór dzierżaw, które można zalogować się do aplikacji.  Identyfikator guid, który wskazuje użytkownik jest użytkownikiem dla klientów indywidualnych firmy Microsoft jest konto `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Wydane w | `iat` | `1452285331` | Czas, w którym token został wystawiony, przedstawione w czasie epoch. |
| Czas wygaśnięcia | `exp` | `1452289231` | Czas, w którym token staje się nieprawidłowe, przedstawione w czasie epoch.  Aby sprawdzić ważność okres ważności tokenu aplikacji należy użyć to zastrzeżenie.  |
| Nie wcześniej niż | `nbf` | `1452285331` |  Czas co token zaczyna obowiązywać, przedstawiane w czasie epoch. Zazwyczaj jest taki sam, jak podczas publikowania.  Aplikacji należy używać to zastrzeżenie do sprawdzania poprawności okres ważności tokenu.  |
| Wersja | `ver` | `2.0` | Wersja id_token, zdefiniowane przez Azure AD.  W przypadku punkt końcowy 2.0 będzie wartość `2.0`. |
| Identyfikator dzierżawy | `tid` | `b9419818-09af-49c2-b0c3-653adc1f376e` | Identyfikator guid reprezentujące Azure AD dzierżawy, czyli użytkownika z.  W przypadku kont służbowych i szkolnych identyfikator guid będzie identyfikator niezmienne dzierżawy organizacji, do której należy użytkownik.  W przypadku konta osobiste będzie wartość `9188040d-6c67-4c5b-b112-36a304b66dad`.  `profile` Zakres jest wymagana, aby otrzymać to zastrzeżenie. |
| Kod skrótu | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Skrót kod znajduje się w id_tokens tylko wtedy, gdy id_token jest wydawany obok kodu autoryzacji OAuth 2.0.  Może służyć do sprawdzania poprawności autentyczności kodu autoryzacji.  Zobacz [Specyfikacja łączenie OpenID](http://openid.net/specs/openid-connect-core-1_0.html) uzyskać więcej szczegółowych informacji na wykonywanie tego sprawdzania poprawności. |
| Skrót Token dostępu | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Skrót token dostępu znajduje się w id_tokens tylko wtedy, gdy id_token jest wydawany razem z pakietem token dostępu OAuth 2.0.  Może służyć do sprawdzania poprawności autentyczności token dostępu.  Zobacz [Specyfikacja łączenie OpenID](http://openid.net/specs/openid-connect-core-1_0.html) uzyskać więcej szczegółowych informacji na wykonywanie tego sprawdzania poprawności. |
| Identyfikator jednorazowy | `nonce` | `12345` | Identyfikator jednorazowy jest strategii łagodzenia atakami token powtarzania.  Aplikacji można określić identyfikatora jednorazowego w wezwaniu autoryzacji za pomocą `nonce` kwerenda parametryczna.  Wartości podane w wezwaniu będą wysyłane w id_token `nonce` roszczeń za w niezmienionej postaci.  Dzięki aplikacji do weryfikacji wartości względem wartość, która go określonej na żądanie, której przypisuje id_token danej sesji tej aplikacji.  Aplikacji należy wykonać ten sprawdzania poprawności podczas sprawdzania poprawności id_token. |
| Nazwa | `name` | `Babe Ruth` | Roszczeń nazwa zawiera ludzi czytelne wartość identyfikująca temat tokenu. Ta wartość nie jest gwarantowana być unikatowe, jest tych i ma być używana tylko do wyświetlania.  `profile` Zakres jest wymagane, aby otrzymać to zastrzeżenie. |
| Adres e-mail | `email` | `thegreatbambino@nyy.onmicrosoft.com` | Podstawowy adres e-mail skojarzony z kontem użytkownika, jeśli istnieje.  Wartość jest tych i może się zmienić w czasie dla danego użytkownika.  `email` Zakres jest wymagane, aby otrzymać to zastrzeżenie. |
| Preferowany nazwy użytkownika | `preferred_username` | `thegreatbambino@nyy.onmicrosoft.com` | Podstawowy nazwa użytkownika, która jest używany do przedstawiania użytkownika końcowego 2.0.  Być może adres e-mail, numer telefonu lub ogólnego nazwy użytkownika bez określonym formacie.  Wartość jest tych i może się zmienić w czasie dla danego użytkownika.  `profile` Zakres jest wymagane, aby otrzymać to zastrzeżenie. |
| Temat | `sub` | `MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | Główna, o którym tokenu potwierdza informacje, takie jak użytkownika aplikacji. Ta wartość jest niezmienny i nie można przydzielić ponownie lub ponownie wykorzystywać, dlatego może służyć do sprawdzania autoryzacji bezpiecznie, takich jak podczas token jest używany do uzyskiwania dostępu do zasobu. Ponieważ temat zawsze występuje tokeny problemów Azure AD, zaleca się użycie tej wartości w układzie autoryzacji uniwersalny. |
| Identyfikator obiektu | `oid` | `a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Obiekt identyfikatora konto służbowe w module Azure AD.  To zastrzeżenie nie będą wydawane dla osobistego konta Microsoft.  `profile` Zakres jest wymagane, aby otrzymać to zastrzeżenie. |


## <a name="access-tokens"></a>Tokeny dostępu

Tokeny dostępu wydawany przez punkt końcowy 2.0 nadają się tylko przez program Microsoft Services w tym momencie.  Twoje aplikacje należy nie trzeba wykonywać sprawdzanie poprawności ani inspekcji tokeny dostępu dla dowolnego z obecnie obsługiwane scenariuszy.  Tokeny dostępu można traktować jako całkowicie nieprzezroczysty — są one tylko ciągi, które aplikacji można przekazać do firmy Microsoft w żądania HTTP.

Punkt końcowy 2.0 wprowadzi w najbliższej przyszłości możliwość aplikacji do odbierania tokeny dostępu z innymi klientami.  W tym czasie te informacje zostaną zaktualizowane informacje, które aplikacji musi wykonać sprawdzanie poprawności token dostępu i inne podobne zadania.

Gdy użytkownik zażąda token dostępu od punktu końcowego 2.0, punkt końcowy 2.0 zwraca również niektóre metadane dotyczącego token dostępu do Twojej aplikacji przez.  Te informacje obejmują czas wygaśnięcia token dostępu i zakresów, dla których jest prawidłowy.  Dzięki aplikacji do wykonywania inteligentnego buforowanie tokeny dostępu bez konieczności przeanalizować otwórz sam token dostępu.

## <a name="refresh-tokens"></a>Odświeżanie tokenów

Odświeżanie tokeny są tokenów zabezpieczających, które aplikacji można używać w celu pobrania nowych dostępu tokeny w przepływie OAuth 2.0.  Umożliwia dodanie aplikacji do osiągnięcia długoterminowe dostęp do zasobów w imieniu użytkownika bez konieczności interakcji przez użytkownika.

Odświeżanie tokeny są wielu zasobów.  To znaczy że token odświeżania odebranych żądania tokenu dla jednego zasobu można wykorzystać dla tokeny dostępu do zupełnie innego zasobu.

Aby otrzymywać odświeżania w token odpowiedzi, aplikacji musisz poprosić i mieć przyznany `offline_acesss` zakres.   Aby dowiedzieć się więcej o `offline_access` zakres, zobacz [następujący artykuł zgody i zakresów](active-directory-v2-scopes.md).

Odświeżanie tokeny są i będzie zawsze, całkowicie nieprzezroczysty do aplikacji.  Ich są wydawane przez punkt końcowy 2.0 Azure AD i można tylko inspekcji & interpretowane przez punkt końcowy 2.0.  Są one długotrwałe, ale aplikacji nie powinny być zapisywane można oczekiwać, że tokenu odświeżanie trwa dowolnego okresu.  W dowolnym momencie w czasie dla różnych powodów, dla których może być unieważnione tokeny odświeżania.  Jedynym sposobem na dodanie aplikacji do ustalić, czy token odświeżania jest prawidłowy jest próbuje wykorzystać dokonując żądania tokenu do punktu końcowego 2.0.

Gdy zrealizować tokenu odświeżania dla nowych token dostępu (i jeśli udzieliła aplikacji `offline_access` zakres), w token odpowiedzi zostanie wyświetlony nowy token odświeżania.  Należy zapisać token nowo wydane odświeżania zamieniania, którego użyto w wezwaniu.  Gwarantuje to usługi tokeny odświeżania i ważność tak długo.

## <a name="validating-tokens"></a>Sprawdzanie poprawności tokenów

W tym momencie tylko token sprawdzania poprawności, należy potrzebnych do wykonania dla aplikacji jest sprawdzanie poprawności id_tokens.  Aby sprawdzić poprawność id_token, aplikacji należy sprawdzić poprawność zarówno id_token podpisu, jak i roszczeń w id_token.

<!-- TODO: Link -->
Firma Microsoft udostępnia biblioteki i przykłady kodu, pokazujące, jak łatwo obsługiwać token sprawdzanie poprawności — poniżej informacji po prostu dostępne dla osób, które chcesz zrozumieć proces źródłowych.  Również są dostępne sprawdzania poprawności JWT kilka bibliotek Otwórz źródło strona 3 — istnieje co najmniej jedną opcję dla prawie wszystkie platformy i tam język.

#### <a name="validating-the-signature"></a>Sprawdzanie poprawności podpisu
JWT zawiera trzy segmenty, które są oddzielone `.` znaków.  Pierwszego odcinka nosi nazwę **nagłówka**, drugi w **treści**i innych jako **podpisu**.  Części podpis może służyć do sprawdzania poprawności autentyczności id_token, tak aby może być zaufany przez aplikację.

Id_Tokens są zalogowani przy użyciu branżowe standardowy asymetrycznym algorytmy, takich jak RSA 256. Nagłówek id_token zawiera informacje dotyczące klucza i szyfrowania metodę podpisywania tokenu:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

`alg` Oświadczeń wskazuje algorytm, który został użyty do podpisania token, podczas `kid` oświadczeń wskazuje określonego klucz publiczny, który został użyty do podpisania token.

Na dowolnym etapie w czasie punkt końcowy 2.0 zarejestrować id_token przy użyciu jednego z określonego zestawu par kluczy publicznych i prywatnych.  Punkt końcowy 2.0 obraca możliwe zestaw kluczy w regularnych odstępach czasu, aby aplikacji powinna zostać zapisana automatycznie obsługiwać jakiekolwiek zmiany klucza.  Rozsądne częstotliwość ma sprawdzać dostępność aktualizacji do kluczy publicznych używane przez punkt końcowy 2.0 jest około 24 godzin.

Mogą uzyskiwać podpisywania niezbędne do sprawdzania poprawności podpisu przy użyciu metadanych OpenID Łączenie dokumentu znajdującego się w danych klucza:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [AZURE.TIP] Wypróbuj ten adres URL w przeglądarce!

Ten dokument metadanych jest obiekt JSON zawierający klasyfikować przydatnych informacji, takich jak lokalizacja różnych wymagane dla uwierzytelniania OpenID łączenie punktów końcowych.  

Zawiera również `jwks_uri`, co daje lokalizację zestawu kluczy publicznych służący do logowania tokeny.  Dokument JSON znajdujący się w `jwks_uri` zawiera wszystkie informacje o kluczu publicznym używany w danym momencie określonego w czasie.  Korzystając z aplikacji `kid` rościć sobie w nagłówku JWT, aby wybrać, które kluczem publicznym w tym dokumencie został użyty do podpisywania tokenu określonego.  Następnie można wykonywać przy użyciu poprawne kluczem publicznym i wskazanej algorytm sprawdzania poprawności podpisu.

Sprawdzaniu poprawności podpisu wykracza poza zakres tego dokumentu — są dostępne dla pomaga zrobić w razie potrzeby liczbę bibliotek Otwórz źródło.

#### <a name="validating-the-claims"></a>Sprawdzanie poprawności roszczeń
Kiedy aplikacji odbiera id_token na logowania użytkownika, jego również wykonać kilka testy z roszczeń w id_token.  Te obejmują, ale nie są ograniczone do:

- Roszczeń dotyczących **odbiorców** — Aby sprawdzić, czy id_token mają mieć do aplikacji.
- **Nie wcześniej niż** i **Czas wygaśnięcia** roszczeń — Aby sprawdzić, czy nie wygasł id_token.
- Roszczeń dotyczących **wystawcy** — Aby sprawdzić, czy token faktycznie został wystawiony do aplikacji przez punkt końcowy 2.0.
- **Identyfikator jednorazowy** - jako łagodzenia atakiem token powtarzania.
- i inne...

Pełną listę sprawdzania roszczeń, które należy wykonać aplikacji można znaleźć w [specyfikacji OpenID połączenia](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Szczegółowe informacje o wartości oczekiwane dla tych oświadczeń znajdują się powyżej w [sekcji id_token](#id_tokens).


## <a name="token-lifetimes"></a>Istnienia tokenu

Następujące istnienia token zostaną udostępnione wyłącznie dla zrozumienie, jak mogą pomóc w zakresie opracowywania i debugowanie aplikacji.  Twoje aplikacje nie powinny być zapisywane się spodziewać dowolnego z tych istnienia pozostać stały — można i zmieni się na dowolnym etapie w czasie.

| Tokenu | Czas użytkowania | Opis |
| ----------------------- | ------------------------------- | ------------ |
| Id_Tokens (konta służbowego) | 1 godzina | Id_Tokens obowiązują zwykle przez godzinę.  Aplikacji sieci web, można używać tego samego istnienia utrzymania własnej sesji z danym użytkownikiem (zalecane) lub wybierz istnienia całkowicie innej sesji.  Jeśli aplikacji musi uzyskać nowy id_token, po prostu musi nawiązać nowe logowania żądanie do 2.0 Autoryzuj punktu końcowego.  Jeśli użytkownik ma prawidłową przeglądarką z punktem końcowym 2.0, nie mogą one wymagane ponownie wprowadzać poświadczeń. |
| Id_Tokens (konta osobiste) | 24 godziny | Id_Tokens dla konta osobiste są zwykle prawidłowe 24 godzin.  Aplikacji sieci web, można używać tego samego istnienia utrzymania własnej sesji z danym użytkownikiem (zalecane) lub wybierz istnienia całkowicie innej sesji.  Jeśli aplikacji musi uzyskać nowy id_token, po prostu musi nowego logowania żądania umożliwiającego 2.0 Autoryzuj punktu końcowego.  Jeśli użytkownik ma prawidłową przeglądarką z punktem końcowym 2.0, nie mogą one wymagane ponownie wprowadzać poświadczeń. |
| Tokeny dostępu (konta służbowego) | 1 godzina | Oznaczony token odpowiedzi jako część tokenu metadanych. |
| Tokeny dostępu (konta osobiste) | 1 godzina | Oznaczony token odpowiedzi jako część tokenu metadanych.  Access_tokens wydany imieniu osobistych kont może być skonfigurowany dla innego długotrwałe, ale godzinę zwykle jest to wielkość liter. |
| Odświeżanie tokeny (konto służbowe) | Maksymalnie 14 dni | Token pojedynczy odświeżania jest prawidłowy dla maksymalnie 14 dni.  Jednak token odświeżania może stać się nieprawidłowe w dowolnym momencie dla różnych sytuacjach, w celu kontynuowania aplikacji do wypróbowania i używania tokenu odświeżania do momentu jej kończy się niepowodzeniem, lub aplikacji zamieni go nowy token odświeżania.  Token odświeżania również staną się nieprawidłowe, ile zostało 90 dni, ponieważ użytkownik wpisze swoje poświadczenia. |
| Odświeżanie tokeny (konta osobiste) | Do 1 roku | Token pojedynczy odświeżania jest prawidłowy dla maksymalnie 1 rok.  Jednak token odświeżania mogą być nieprawidłowe w dowolnym momencie dla dowolnej liczby powody, w celu kontynuowania aplikacji do wypróbowania i używania tokenu odświeżania, dopóki go nie powiedzie się. |
| Kody autoryzacji (konta służbowego) | 10 minut | Kody autoryzacji są celowo krótkotrwałe i powinny być natychmiast zaginął access_tokens i refresh_tokens, po ich otrzymaniu. |
| Kody autoryzacji (konta osobiste) | 5 minut | Kody autoryzacji są celowo krótkotrwałe i powinny być natychmiast zaginął access_tokens i refresh_tokens, po ich otrzymaniu.  Kody autoryzacji wydane w imieniu konta osobiste są również jednokrotnego użycia. |
