 <properties
   pageTitle="Odwołanie Azure AD Token | Microsoft Azure"
   description="Przewodnik opis i oceny roszczeń w tokeny SAML 2.0 i JSON tokeny sieci Web (JWT), wystawiony przez Azure Active Directory (AAD)"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Odwołania do Azure AD tokenu

Azure Active Directory (Azure AD) emituje kilka typów tokenów zabezpieczających przetwarzania każdy przepływ uwierzytelniania. W tym dokumencie opisano format, właściwości zabezpieczeń i zawartości każdego typu token.

## <a name="types-of-tokens"></a>Typy tokenów

Azure AD obsługuje [Protokół OAuth 2.0](active-directory-protocols-oauth-code.md), korzystające z access_tokens i refresh_tokens.  Obsługuje również uwierzytelnianie i logowania za pośrednictwem [Połączenia OpenID](active-directory-protocols-openid-connect-code.md), które wprowadza trzeci typ tokenu, id_token.  Każdy z tych tokenów jest reprezentowane jako "token okaziciela".

Token okaziciela jest tokenu zabezpieczającego lightweight, którego udziela dostępu "okaziciela" do chronionego zasobu. W tym znaczeniu "okaziciela", to każda strona, która może prezentować tokenu. Uwierzytelnianie za pomocą Azure AD jest wymagane, aby otrzymać token okaziciela, ale można podjąć kroki w celu bezpiecznego tokenu, aby uniemożliwić przechwycenie niezamierzonych strony. Ponieważ tokeny okaziciela nie ma wbudowane mechanizm uniemożliwić nieupoważnione korzystanie z nich, musi być transportu w bezpiecznego kanału, takie jak transportu warstwy zabezpieczeń (HTTPS). Jeśli token okaziciela są przesyłane w Wyczyść, mężczyzna w środkowym atakiem można uzyskać tokenu i uzyskania nieautoryzowanego dostępu do chronionych zasobów. Te same zasady zabezpieczeń Zastosuj podczas przechowywania lub pamięci podręcznej tokenów okaziciela do późniejszego użycia. Zawsze upewnij się, że aplikacji przesyła i przechowuje tokeny okaziciela w bezpieczny sposób. Aby uzyskać więcej zagadnienia dotyczące zabezpieczeń na okaziciela tokenów zobacz [RFC 6750 sekcji 5](http://tools.ietf.org/html/rfc6750).

Wiele tokenów wydawany przez Azure AD są wykonywane jako tokeny Web JSON lub JWTs.  JWT jest sposób niewielkie, bezpiecznych adres URL przekazywania informacji między dwiema stronami.  Informacje zawarte w JWTs są nazywane "roszczenia" i potwierdzenia informacji o okaziciela i temat tokenu.  Roszczeń w JWTs są obiektami JSON zakodowany i seryjny dla transmisji.  Ponieważ JWTs wydany przez Azure AD są zalogowani, ale nie szyfrowane, można łatwo sprawdzić zawartość JWT na potrzeby debugowania.  Istnieje kilka narzędzi zrobić, takich jak [jwt.calebb.net](http://jwt.calebb.net). Aby uzyskać więcej informacji o JWTs może dotyczyć [Specyfikacja JWT](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens są formie tokenu zabezpieczającego logowania, odbierająca w aplikacji do uwierzytelniania przy użyciu funkcji [OpenID połączenie](active-directory-protocols-openid-connect-code.md).  Są przedstawiane jako [JWTs](#types-of-tokens)i zawierają roszczeń, które służą do logowania się użytkownika do aplikacji.  Roszczeń służy w id_token jako odpowiednio do potrzeb — często są używane do wyświetlania informacji o koncie lub podejmowania decyzji kontroli dostępu w aplikacji.

Id_tokens są zalogowani, ale nie są szyfrowane w tej chwili.  Aplikacji otrzymuje id_token, musi, [Sprawdź poprawność podpisu](#validating-tokens) potwierdza tokenu autentyczności i sprawdź poprawność kilka roszczeń w tokenu, aby udowodnić ważności.  Roszczeń sprawdzone przez aplikację są zależne od wymagania scenariusz, ale istnieje kilka [typowych roszczeń reguły sprawdzania poprawności](#validating-tokens) aplikacji należy wykonać w każdej sytuacji.

Zobacz następną sekcję, aby uzyskać informacje na oświadczeniach id_tokens, a także id_token próbki.  Zauważ, że roszczeń w id_tokens nie są zwracane w określonej kolejności.  Ponadto nowych wniosków mogą zostać wprowadzone do id_tokens w dowolnym momencie w czasie — aplikacji nie powinna zostać podzielona na wprowadzoną nowych wniosków.  Poniższa lista zawiera oświadczeń, które aplikacji niezawodne może zinterpretować podczas pisania tego dokumentu.  W razie potrzeby nawet więcej szczegółów można znaleźć w [specyfikacji OpenID połączenia](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Przykładowe id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] Ćwiczenia spróbuj sprawdzanie roszczeń w id_token próbki wklejając go w [calebb.net](http://jwt.calebb.net).

#### <a name="claims-in-idtokens"></a>Roszczeń w id_tokens

| Roszczeń JWT | Nazwa | Opis |
|-----------|------|-------------|
| `appid`| Identyfikator aplikacji | Służy do identyfikowania aplikacji, która przy użyciu tokenu uzyskiwać dostęp do zasobu. Aplikacja może działać jako samego lub w imieniu użytkownika. Identyfikator aplikacji zwykle reprezentuje obiekt aplikacji, ale także może oznaczać obiektem głównej usługi w Azure AD. <br><br> **Przykład JWT wartość**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| Grupy odbiorców | Adresat tokenu. Aplikacja odbierająca token musi Sprawdź, czy wartość odbiorców jest poprawny i Odrzuć wszystkie tokeny przeznaczone dla różnych odbiorców. <br><br> **Przykład SAML wartość**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Przykład JWT wartość**: <br> `"aud":"https://contoso.com"` |
| `appidacr`| Aplikacja uwierzytelniania kontekstu zajęć | Wskazuje, jak uwierzytelnienia klienta. W przypadku publicznych klienta wartość wynosi 0. Jeśli zostaną użyte identyfikator klienta i hasła klienta, wartość wynosi 1. <br><br> **Przykład JWT wartość**: <br> `"appidacr": "0"`|
| `acr`| Uwierzytelnianie kontekstu zajęć | Wskazuje, jak uwierzytelnienia tematu, a nie klienta w roszczeń klasy kontekstu uwierzytelniania aplikacji. Wartość "0" wskazuje, że uwierzytelniania użytkowników końcowych nie spełnia wymagań ISO/IEC 29115. <br><br> **Przykład JWT wartość**: <br> `"acr": "0"`|
| | Błyskawiczne uwierzytelniania | Rekordy, Data i godzina wystąpienia uwierzytelniania. <br><br> **Przykład SAML wartość**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| Metody uwierzytelniania | Określa, jak uwierzytelnienia temat tokenu. <br><br> **Przykład SAML wartość**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Przykład JWT wartość**:`“amr”: ["pwd"]` |
| `given_name`| Imię | Udostępnia pierwszego lub "" Nazwa użytkownika, określonych na obiekcie użytkownika Azure AD. <br><br> **Przykład SAML wartość**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Przykład JWT wartość**: <br> `"given_name": "Frank"` |
| `groups`| Grupy | Udostępnia identyfikatory obiektów, reprezentujących członkostwa w grupach podmiotu. Wartości te są unikatowe (patrz identyfikator obiektu) i może być bezpiecznie stosowany do zarządzania programu access, takie jak wymuszanie zezwolenia na dostęp do zasobu. Grupy zawarte w roszczeń grupy są skonfigurowane na poziomie aplikacji, za pośrednictwem właściwości "groupMembershipClaims" manifest aplikacji. Wartość null powoduje wyłączenie we wszystkich grupach, wartość "SecurityGroup" będzie zawierać tylko członkostwa grupy zabezpieczeń usługi Active Directory i wartość "Wszystkie" będzie zawierać grup zabezpieczeń i listy dystrybucyjne usługi Office 365. <br><br> **Przykład SAML wartość**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Przykład JWT wartość**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | Dostawcy tożsamości | Rekordów dostawcy tożsamości, który uwierzytelniony temat tokenu. Ta wartość jest taka sama, jak wartość wystawcy, chyba że konto użytkownika jest w innej dzierżawie niż wystawcy. <br><br> **Przykład SAML wartość**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Przykład JWT wartość**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | Przechowuje czas, w którym token został wystawiony. Często służy do pomiaru token aktualność. <br><br> **Przykład SAML wartość**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Przykład JWT wartość**: <br> `"iat": 1390234181` |
| `iss` | Wystawcy | Służy do identyfikowania usługę tokenu zabezpieczającego (STS), który tworzy i zwraca token. Tokeny zwraca Azure AD wystawcy jest sts.windows.net. Identyfikator GUID wartości roszczeń wystawcy jest identyfikator dzierżawy katalogu Azure AD. Identyfikator dzierżawy jest niezmienny i niezawodne identyfikatorem katalogu. <br><br> **Przykład SAML wartość**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Przykład JWT wartość**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | Nazwisko | Zgodnie z definicją w obiekcie użytkownika Azure AD zawiera ostatnią nazwę, nazwisko lub nazwisko użytkownika. <br><br> **Przykład SAML wartość**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Przykład JWT wartość**: <br> `"family_name": "Miller"` |
| `unique_name`| Nazwa | Udostępnia ludzi czytelne wartość identyfikująca temat tokenu. Ta wartość nie jest gwarantowana być unikatowa w obrębie dzierżawy i ma być używana tylko do wyświetlania. <br><br> **Przykład SAML wartość**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Przykład JWT wartość**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | Identyfikator obiektu | Zawiera unikatowy identyfikator obiektu w Azure AD. Ta wartość jest niezmienny i nie można przydzielić ponownie, ani ponownie. Identyfikator obiektu służy do identyfikowania obiektu w kwerendach do Azure AD. <br><br> **Przykład SAML wartość**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Przykład JWT wartość**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | Role | Reprezentuje wszystkich ról aplikacji, których temat zostało udzielone bezpośrednio i pośrednio za pośrednictwem członkostwa w grupach i może służyć do wymuszenia kontrola dostępu oparta na rolach. Role aplikacji są definiowane na podstawie aplikacji za pomocą `appRoles` właściwość manifest aplikacji. `value` Właściwość dla poszczególnych ról aplikacji jest wartość, która jest wyświetlana w roszczeń role. <br><br> **Przykład SAML wartość**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Przykład JWT wartość**: <br> `“roles”: ["Admin", … ]` |
| `scp` | Zakres | Wskazuje personifikacji uprawnienia do aplikacji klienckiej. Domyślne uprawnienia `user_impersonation`. Właściciel zabezpieczonego zasobu można rejestrować dodatkowe wartości Azure AD. <br><br> **Przykład JWT wartość**: <br> `"scp": "user_impersonation"`|
| `sub` |Temat| Służy do identyfikowania kapitału, o którym tokenu potwierdza informacje, takie jak użytkownika aplikacji. Ta wartość jest niezmienny i nie można przydzielić ponownie lub ponownie wykorzystywać, dlatego może służyć do sprawdzania autoryzacji bezpieczne. Ponieważ temat zawsze występuje tokeny problemów Azure AD, zaleca się użycie tej wartości w układzie autoryzacji uniwersalny. <br> `SubjectConfirmation`nie jest roszczenia. Tym artykule opisano sposób temat tokenu zostanie zweryfikowana. `Bearer`Wskazuje, że posiadanych tokenu potwierdzone jest temat. <br><br> **Przykład SAML wartość**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Przykład JWT wartość**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | Identyfikator dzierżawy | Identyfikator niezmienne, jednorazowego, który identyfikuje której wystawiony token dzierżawy katalogu. Za pomocą tej wartości można uzyskać dostęp do aplikacji dzierżawy wielu zasobów katalog dzierżawy. Za pomocą tej wartości można na przykład identyfikowanie dzierżawy w trakcie rozmowy API wykresu. <br><br> **Przykład SAML wartość**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Przykład JWT wartość**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|Okres ważności tokenu | Określa przedział czasu, w którym tokenu jest prawidłowy. Usługa, która sprawdza tokenu należy sprawdzić, czy bieżącą datę w okres ważności tokenu, jeszcze czy odrzucić tokenu. Usługa mogące pozwolić na maksymalnie pięć minut poza zakresem okres ważności tokenu pod uwagę różnice w czasie zegar ("pochylanie godziny") między Azure AD i usługi. <br><br> **Przykład SAML wartość**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Przykład JWT wartość**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| Głównej nazwy użytkownika | Jest przechowywana nazwa użytkownika główna użytkownika.<br><br> **Przykład JWT wartość**: <br> `"upn": frankm@contoso.com`|
| `ver`| Wersja | Przechowuje numer wersji tokenu. <br><br> **Przykład JWT wartość**: <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>Tokeny dostępu

Tokeny dostępu nadają się tylko przez program Microsoft Services w tym momencie.  Twoje aplikacje należy nie trzeba wykonywać sprawdzanie poprawności ani inspekcji tokeny dostępu dla dowolnego z obecnie obsługiwane scenariuszy.  Tokeny dostępu można traktować jako całkowicie nieprzezroczysty — są one tylko ciągi, które aplikacji można przekazać do firmy Microsoft w żądania HTTP.

Gdy użytkownik zażąda token dostępu, Azure AD zwraca również niektóre metadane dotyczącego token dostępu do Twojej aplikacji przez.  Te informacje obejmują czas wygaśnięcia token dostępu i zakresów, dla których jest prawidłowy.  Dzięki aplikacji do wykonywania inteligentnego buforowanie tokeny dostępu bez konieczności przeanalizować otwórz sam token dostępu.

## <a name="refresh-tokens"></a>Odświeżanie tokenów

Odświeżanie tokeny są tokenów zabezpieczających, które aplikacji można używać w celu pobrania nowe tokeny dostępu w przepływie OAuth 2.0.  Umożliwia dodanie aplikacji do osiągnięcia długoterminowe dostęp do zasobów w imieniu użytkownika bez konieczności interakcji przez użytkownika.

Odświeżanie tokeny są wielu zasobów, co oznacza, ta osoba może być odebranych żądania tokenu dla jednego zasobu, ale zaginął tokeny dostępu do zupełnie innego zasobu. Aby określić wielu zasobów, ustaw `resource` parametr w wezwaniu do wybranych zasobów.

Odświeżanie tokeny są całkowicie nieprzezroczysty do aplikacji. Są one długotrwałe, ale aplikacji nie powinny być zapisywane można oczekiwać, że tokenu odświeżanie trwa dowolnego okresu.  W dowolnym momencie w czasie dla różnych powodów, dla których może być unieważnione tokeny odświeżania.  Jedynym sposobem na dodanie aplikacji do ustalić, czy token odświeżania jest prawidłowy jest próbuje wykorzystać dokonując żądania tokenu Azure AD token punktu końcowego.

Gdy zrealizować tokenu odświeżania dla nowych token dostępu, otrzymasz nowy token odświeżania w token odpowiedzi.  Należy zapisać token nowo wydane odświeżania zamieniania, którego użyto w wezwaniu.  Gwarantuje to usługi tokeny odświeżania i ważność tak długo.

## <a name="validating-tokens"></a>Sprawdzanie poprawności tokenów

W tym momencie tylko token sprawdzania poprawności, należy potrzebne do wykonania dla aplikacji klienckich jest sprawdzanie poprawności id_tokens.  Aby sprawdzić poprawność id_token, aplikacji należy sprawdzić poprawność zarówno id_token podpisu, jak i roszczeń w id_token.

Firma Microsoft udostępnia biblioteki i przykłady kodu, których pokazująca, jak łatwo obsługiwać token sprawdzanie poprawności, aby zrozumieć proces źródłowych.  Istnieją również kilka innych firm, Otwórz źródło bibliotek dostępne dla sprawdzania poprawności JWT, co najmniej jedną opcję prawie wszystkie platformy i języka. Aby uzyskać więcej informacji o bibliotekach uwierzytelniania Azure AD i przykłady kodu zobacz [Azure AD uwierzytelniania bibliotek](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>Sprawdzanie poprawności podpisu

JWT zawiera trzy segmenty, które są oddzielone `.` znaków.  Pierwszego odcinka nosi nazwę **nagłówka**, drugi w **treści**i innych jako **podpisu**.  Części podpis może służyć do sprawdzania poprawności autentyczności id_token, tak aby może być zaufany przez aplikację.

Id_Tokens są zalogowani przy użyciu branżowe standardowy asymetrycznym algorytmy, takich jak RSA 256. Nagłówek id_token zawiera informacje dotyczące klucza i szyfrowania metodę podpisywania tokenu:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

`alg` Oświadczeń wskazuje algorytm, który został użyty do podpisania token, podczas `x5t` oświadczeń wskazuje określonego klucz publiczny, który został użyty do podpisania token.

Na dowolnym etapie w czasie Azure AD zarejestrować id_token przy użyciu jednego z określonego zestawu par kluczy publicznych i prywatnych. Azure AD obraca możliwe zestaw kluczy w regularnych odstępach czasu, aby aplikacji powinna zostać zapisana automatycznie obsługiwać tych najważniejszych zmian.  Rozsądne częstotliwość ma sprawdzać dostępność aktualizacji do kluczy publicznych używane przez Azure AD jest co 24 godziny.

Mogą uzyskiwać podpisywania niezbędne do sprawdzania poprawności podpisu przy użyciu metadanych OpenID Łączenie dokumentu znajdującego się w danych klucza:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] Wypróbuj ten adres URL w przeglądarce!

Ten dokument metadanych jest obiekt JSON zawierający kilka fragmenty przydatne informacje, takie jak lokalizacji różnych punktach wymagane dla uwierzytelniania OpenID połączenia.  

Zawiera również `jwks_uri`, co daje lokalizację zestawu kluczy publicznych służący do logowania tokeny.  Dokument JSON znajdujący się w `jwks_uri` zawiera wszystkie informacje o kluczu publicznym używany w danym momencie określonego w czasie.  Korzystając z aplikacji `kid` rościć sobie w nagłówku JWT, aby wybrać, które klucz publiczny w tym dokumencie został użyty do podpisywania tokenu określonego.  Następnie można wykonywać przy użyciu poprawne kluczem publicznym i wskazanej algorytm sprawdzania poprawności podpisu.

Sprawdzaniu poprawności podpisu wykracza poza zakres tego dokumentu — są dostępne dla pomaga zrobić w razie potrzeby liczbę bibliotek Otwórz źródło.

#### <a name="validating-the-claims"></a>Sprawdzanie poprawności roszczeń

Kiedy aplikacji odbiera id_token na logowania użytkownika, jego również wykonać kilka testy z roszczeń w id_token.  Te obejmują, ale nie są ograniczone do:

  - Roszczeń dotyczących **odbiorców** — Aby sprawdzić, czy id_token mają mieć do aplikacji.
  - **Nie wcześniej niż** i **Czas wygaśnięcia** roszczeń — Aby sprawdzić, czy nie wygasł id_token.
  - Roszczeń dotyczących **wystawcy** — Aby sprawdzić, czy token rzeczywiście został wystawiony do aplikacji przez Azure AD.
  - **Identyfikator jednorazowy** - zmniejszeniu atakiem token powtarzania.
  - i inne...

Aby uzyskać pełną listę sprawdzania roszczeń, które należy wykonać aplikacji odwołują się do [OpenID łączenie Specyfikacja](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Szczegółowe informacje o wartości oczekiwane dla tych roszczeń są uwzględniane w poprzedniej sekcji [id_token sekcji](#id-tokens) .

## <a name="sample-tokens"></a>Tokeny próbki

W tej sekcji Wyświetla przykłady tokeny SAML i JWT, które zwraca Azure AD. Te przykłady pozwalają na wyświetlanie roszczeń w kontekście.
SAML Token

To jest przykład typowego tokenu SAML.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT Token - personifikacji użytkownika

To jest przykład typowego tokenu web JSON (JWT) używane w przepływie udzielanie kodu autoryzacji.
Oprócz roszczeń token zawiera numer wersji w **wersja** i **appidacr**, odwołania klasy kontekście uwierzytelniania, co wskazuje, jak uwierzytelnienia klienta. W przypadku publicznych klienta wartość wynosi 0. Jeśli użyto identyfikator klienta lub tajny klienta, wartość wynosi 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Zawartość pokrewna
- Zobacz Azure AD wykresu [operacji polityki](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) i [obiektu zasad](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity), aby dowiedzieć się więcej na temat zarządzania zasadami okres ważności tokenu przy użyciu interfejsu API Azure AD wykresu.
- Aby uzyskać więcej informacji i próbki Zarządzanie zasadami przy użyciu poleceń cmdlet programu PowerShell, łącznie z próbkami zobacz [token istnienia można konfigurować w Azure AD](active-directory-configurable-token-lifetimes.md). 
