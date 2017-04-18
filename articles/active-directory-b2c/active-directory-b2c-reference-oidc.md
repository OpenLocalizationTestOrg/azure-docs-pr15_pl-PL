<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Tworzenie aplikacji sieci web przy użyciu usługi Azure Active Directory stosowania protokołu uwierzytelniania OpenID połączenia."
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

# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Web logowania przy użyciu połączenia OpenID

Łączenie OpenID jest protokół uwierzytelniania wbudowane na wierzchu OAuth 2.0, który może być używany do bezpiecznego logowania użytkowników w aplikacji sieci web.  Za pomocą połączenia OpenID wykonania B2C usługi Azure Active Directory (Azure AD), czy zewnętrzny zapisów, logowania, a inne Zarządzanie tożsamościami występują w aplikacji sieci web do Azure AD. Ten przewodnik procedurach pokazano, jak to zrobić w sposób niezależny od języka. Będzie ją opisano sposób na wysyłanie i odbieranie wiadomości HTTP bez przy użyciu jednej z naszych bibliotek Otwórz źródło.

[Łączenie OpenID](http://openid.net/specs/openid-connect-core-1_0.html) rozszerza protokół *autoryzacji* OAuth 2.0 do użytku protokół *uwierzytelniania* . Umożliwia wykonywanie rejestracji jednokrotnej przy użyciu OAuth. Podaj pojęcia `id_token`, która jest tokenu zabezpieczającego, który pozwala klientów zweryfikować tożsamość użytkownika i uzyskać podstawowe informacje o profilu użytkownika.

Ponieważ rozszerza OAuth 2.0, umożliwia aplikacji bezpieczne uzyskanie **access_tokens**. Możesz użyć access_tokens uzyskiwać dostęp do zasobów zabezpieczonych za pośrednictwem [serwera autoryzacji](active-directory-b2c-reference-protocols.md#the-basics). Zalecamy OpenID połączenia, jeśli tworzysz aplikację sieci web znajdującej się na serwerze i dostępne za pośrednictwem przeglądarki sieci. Jeśli chcesz dodać Zarządzanie tożsamościami aplikacji mobilnych lub pulpitu przy użyciu Azure AD B2C, należy używać [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) zamiast OpenID połączenia.

Azure AD B2C rozszerza standardowy protokół łączenie OpenID do więcej niż prosty uwierzytelniania i autoryzacji. Podaj [**parametr zasad**](active-directory-b2c-reference-policies.md), który umożliwia łączenie OpenID takich jak dodawanie użytkowników do aplikacji — zapisów, logowanie i zarządzanie profilami. W tym miejscu pokażemy jak za pomocą zasadach i łączenie OpenID wdrożenia każdego z tych funkcji w aplikacji sieci web. Firma Microsoft będzie również pokazano, jak uzyskać dostęp do interfejsów API sieci web access_tokens.

Żądania HTTP przykładzie poniżej użyje naszego katalogu B2C próbki, **fabrikamb2c.onmicrosoft.com**, a także nasze przykładowe aplikacji **https://aadb2cplayground.azurewebsites.net** i zasady. Jesteś bezpłatne wypróbować żądania siebie za pomocą tych wartości lub zamień je na własny.
Dowiedz się, jak [uzyskać własnych B2C dzierżawy, aplikacji i zasady](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Wysyłanie żądania uwierzytelniania
Gdy aplikacji sieci web wymaga do uwierzytelnienia użytkownika, a następnie wykonaj zasady, można kierować użytkownikowi `/authorize` punktu końcowego. To interakcyjne części przepływu, gdzie użytkownik faktycznie zajmie akcji, w zależności od zasad.

W tym żądaniu klienta wskazuje uprawnień, które trzeba ją uzyskać od użytkownika w `scope` parametru oraz zasady do wykonania w `p` parametru. Trzy przykłady znajdują się poniżej (z podziały wierszy dla czytelności), każdy za pomocą różnych zasad. Działanie uzyskać działanie każdego żądania, spróbuj wklejanie żądania w przeglądarce i uruchomić je.

#### <a name="use-a-sign-in-policy"></a>Za pomocą zasad logowania

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Za pomocą zasad zapisów

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Za pomocą zasad Edytuj profil

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parametr | Wymagane? | Opis |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Wymagane | Identyfikator aplikacji [Azure portal](https://portal.azure.com/) przydzielone do aplikacji. |
| response_type | Wymagane | Typ odpowiedzi, który może zawierać `id_token` dla OpenID połączenia. Jeśli aplikacji sieci web muszą również tokeny do nawiązywania połączeń z interfejsu API sieci web, możesz użyć `code+id_token`, jak firma Microsoft wykonane w tym miejscu. |
| redirect_uri | Zalecane | Redirect_uri aplikacji, której uwierzytelniania odpowiedzi można wysyłane i otrzymywane przez aplikację. Go muszą dokładnie odpowiadać jeden z redirect_uris, które zarejestrowane w portalu, z wyjątkiem, że musi być zakodowany w adresie URL. |
| zakres | Wymagane | Rozdzielany spacjami zakresów. Wartość jednego zakresu wskazuje Azure AD obu uprawnień, które są żądane. `openid` Wskazuje zakres uprawnień Logowanie użytkownika i pobieranie danych dotyczących użytkownika w formularzu **id_tokens** (więcej zostanie dodane później na to w dalszej części tego artykułu). `offline_access` Zakres jest opcjonalne dla aplikacji sieci web. Oznacza to, że aplikacji muszą **refresh_token** długotrwałe dostępu do zasobów. |
| response_mode | Zalecane | Metodę, która powinna być używane do wysyłania wyniku authorization_code z powrotem do aplikacji. Może być "query", "form_post" lub "fragment".  "form_post" zaleca się najlepiej. |
| Województwo | Zalecane | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token. Może być ciągiem dowolną zawartość, która ma być. Losowo wygenerowanym wartości unikatowe jest zazwyczaj używany służące do zapobiegania powstawaniu atakami fałszowaniu żądanie między witrynami. Stan służy także do kodowania informacji o stanie użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strony, w których znajdowały się na. |
| Identyfikator jednorazowy | Wymagane | Wartość zawarte w żądaniu (generowane za pomocą aplikacji), które zostaną uwzględnione w wyniku id_token jako roszczenia. Aplikacja może zweryfikować tę wartość na zmniejszeniu atakami token powtarzania. Wartość jest zazwyczaj te, unikatowy ciąg, który może służyć do identyfikowania początku żądania. |
| p | Wymagane | Zasady, które zostaną wykonane. Jest to nazwa zasady, która jest tworzona w dzierżawie B2C. Zasady wartości z pola Nazwa powinna rozpoczynać "b2c\_1\_". Dowiedz się więcej o zasadach w [ramach zasad Extensible](active-directory-b2c-reference-policies.md). |
| wiersz | Opcjonalne | Typ interakcji użytkownika, który jest wymagane. Tylko wartości w tej chwili jest "identyfikator logowania", który wymusza na użytkowniku wprowadź swoje poświadczenia na żądanie. Rejestracji jednokrotnej nie zostaną wprowadzone. |

Na tym etapie użytkownik zostanie poproszony do wykonania zasad przepływu pracy. Może to obejmować użytkownik podczas wprowadzania swojej nazwy użytkownika i hasła, logowanie się przy użyciu tożsamości społecznościowe, tworzący konto w usłudze katalogu lub dowolną inną liczbę kroków w zależności od sposobu zdefiniowania zasady.

Po zakończeniu zasady użytkownika Azure AD zwróci odpowiedź dotycząca aplikacji na wskazanej `redirect_uri`, przy użyciu metody określonej w `response_mode` parametru. Odpowiedź będzie dokładnie taki sam dla każdej z powyższych przypadkach, niezależnie od zasad, która została wykonana.

Pomyślną odpowiedź przy użyciu `response_mode=fragment` powinien wyglądać tak:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| id_token | Id_token, tej aplikacji. Za pomocą id_token zweryfikować tożsamość użytkownika i Rozpocznij sesję z danym użytkownikiem. Więcej informacji na temat id_tokens i ich zawartość są uwzględniane w [Azure AD B2C odwołania do tokenu](active-directory-b2c-reference-tokens.md). |
| Kod | Authorization_code, wymagającej aplikacji, jeśli został użyty `response_type=code+id_token`. Aplikacja umożliwia kodu autoryzacji żądania access_token dla zasobu docelowego. Authorization_codes są bardzo krótko. Zazwyczaj wygasną po około 10 minut. |
| Województwo | Jeśli parametr Stan znajduje się w wezwaniu na tej samej wartości powinien być wyświetlany w odpowiedzi. Aplikacja należy sprawdzić, czy wartości stanu na żądanie i odpowiedź są identyczne. |

Odpowiedzi na błędy mogą być również wysyłane do `redirect_uri` tak, aby aplikacja może podejmie wobec nich odpowiednio:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kodu błędu może służyć do klasyfikowania typów błędów występujących, która umożliwia reagowanie na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania. |
| Województwo | Zobacz pełny opis w powyższej tabeli. Jeśli parametr Stan znajduje się w wezwaniu na tej samej wartości powinien być wyświetlany w odpowiedzi. Aplikacja należy sprawdzić, czy wartości stanu na żądanie i odpowiedź są identyczne. |


## <a name="validate-the-idtoken"></a>Sprawdź poprawność id_token
Po prostu otrzymywania id_token mało do uwierzytelnienia użytkownika — musisz sprawdzić poprawność podpisu id_token i sprawdzić oświadczeniach tokenu zgodnie wymagania aplikacji sieci. Azure AD B2C używa [Tokenów Web JSON (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) i szyfrowania kluczem publicznym do podpisania tokeny i sprawdź, czy są prawidłowe.

Istnieje wiele bibliotek Otwórz źródło, które są dostępne dla sprawdzania poprawności JWTs, w zależności od preferencji język. Zaleca się uzyskiwanie informacji o tych opcji, a nie implementacji logiki sprawdzania poprawności. Tutaj informacje będą przydatne objaśnienie się, jak prawidłowo za pomocą tych bibliotek.

Azure AD B2C ma łączenie OpenID metadanych punktu końcowego, który umożliwia uzyskiwanie zdalnego dostępu do informacji na temat Azure AD B2C w czasie wykonywania aplikacji. Te informacje zawiera punkty końcowe, zawartość tokenów i podpisywania tokenu. Istnieje dokument JSON metadanych dla każdej zasady w dzierżawie B2C. Na przykład dokument metadanych dla `b2c_1_sign_in` zasad w `fabrikamb2c.onmicrosoft.com` znajduje się w:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Jedną z właściwości tego dokumentu konfiguracji jest `jwks_uri`, których wartość dla tych samych zasad będzie:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

W celu określenia zasady, które został użyty w logowaniu id_token (i gdzie można pobierać metadane z), masz do wyboru dwie opcje. Najpierw nazwę zasady znajduje się w `acr` rościć sobie w id_token. Aby uzyskać informacje na temat analizy roszczeń związanych z id_token, zobacz [odwołania do tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md). Jest kodowanie zasad wartości z innych opcji `state` parametru w przypadku problemów żądanie, a następnie dekodowanie go, aby określić, które zasady został użyty. Każda z tych metod jest idealnie prawidłowy.

Po zostało nabyte dokument metadanych od punktu końcowego łączenie OpenID metadanych, można użyć kluczy publicznych RSA 256 (które znajdują się w tym punkcie końcowym) do sprawdzania poprawności podpisu id_token. Może być wiele kluczy wymienione w tym punkcie końcowym na dowolnym etapie w czasie, każda oznaczona `kid`. Nagłówek id_token również zawiera `kid` rościć sobie, która wskazuje, które klucze został użyty do podpisania id_token. Zobacz [odwołania do tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md) uzyskać więcej informacji, w tym sekcjach dotyczących [sprawdzania poprawności tokenów](active-directory-b2c-reference-tokens.md#validating-tokens) i [Ważne informacje dotyczące logowania przerzucania klucza](active-directory-b2c-reference-tokens.md#validating-tokens).
<!--TODO: Improve the information on this-->

Po zostały poprawności Podpis id_token, istnieje kilka oświadczeń, które należy sprawdzić, na przykład:

- Należy sprawdzić, czy `nonce` rościć sobie przed atakami token powtarzania. Jej wartość powinna być określony w żądaniu logowania.
- Należy sprawdzić, czy `aud` rościć sobie upewnić się, że id_token został wystawiony dla aplikacji. Jej wartość powinna być identyfikator aplikacji aplikacji.
- Należy sprawdzić, czy `iat` i `exp` roszczeń upewnić się, że nie wygasł id_token.

Istnieje kilka więcej sprawdzanie poprawności, które należy wykonać, opisane szczegółowo w [Specyfikacji podstawową połączyć OpenID](http://openid.net/specs/openid-connect-core-1_0.html).  Warto też sprawdzić poprawność oświadczeń dodatkowe, w zależności od aktualnego scenariusza. Niektóre typowe reguły sprawdzania poprawności obejmują:

- Zapewnienie, że użytkownika/organizacji po zarejestrowaniu aplikacji.
- Zapewnienie, że użytkownik ma odpowiednie autoryzacja i uprawnienia.
- Zapewnienie, że niektóre siły uwierzytelniania wystąpił, takich jak uwierzytelnianie wieloskładnikowe Azure.

Aby uzyskać więcej informacji na oświadczeniach w id_token zobacz [odwołania do tokenu Azure AD B2C](active-directory-b2c-reference-tokens.md).

Po całkowicie poprawności id_token można rozpocząć sesję z użytkownikiem i użyć roszczeń w id_token, aby uzyskać informacje o użytkowniku w aplikacji. Te informacje może służyć do wyświetlania, rekordów, autoryzacji i tak dalej.

## <a name="get-a-token"></a>Uzyskiwanie tokenu
W przypadku wszystkie te potrzeb aplikacji sieci web, należy wykonać zasady, możesz pominąć następnych kilka sekcji. Tych sekcji dotyczą tylko aplikacje, które muszą zostać uwierzytelnione połączenia do interfejsu API sieci web i są chronione przez Azure AD B2C w sieci web.

Można zrealizować authorization_code, który został zakupiony (za pomocą `response_type=code+id_token`) token żądanego zasobu, wysyłając `POST` żądanie `/token` punktu końcowego. Obecnie tylko zasób, którego możesz zażądać tokenu do jest wewnętrznej bazy danych w aplikacji sieci web interfejsu API. Konwencja umożliwiający zażądanie token do siebie jest użycie identyfikator klienta Twojej aplikacji jako zakres:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parametr | Wymagane? | Opis |
| ----------------------- | ------------------------------- | --------------------- |
| p | Wymagane | Zasady, który został użyty do uzyskania kodu autoryzacji. Inne zasady nie można użyć tego żądania. Zwróć uwagę, Dodaj ten parametr do **ciągu kwerendy**, nie w treści WPIS. |
| client_id | Wymagane | Identyfikator aplikacji [Azure portal](https://portal.azure.com/) przydzielone do aplikacji. |
| grant_type | Wymagane | Typ Udziel, która musi być `authorization_code` dla przepływu kodu autoryzacji. |
| zakres | Zalecane | Rozdzielany spacjami zakresów. Wartość jednego zakresu wskazuje Azure AD obu uprawnień, które są żądane. `openid` Uprawnienia, aby zalogować się użytkownika i pobieranie danych dotyczących użytkownika w formularzu **id_tokens**wskazuje zakres. Może służyć do uzyskać tokenów do wewnętrznej bazy danych w aplikacji sieci web interfejsu API, który jest reprezentowany przez tej samej aplikacji, co klient. `offline_access` Zakres wskazuje, że aplikacji muszą **refresh_token** długotrwałe dostępu do zasobów. |
| Kod | Wymagane | Authorization_code, które zostało nabyte w pierwszym nogi przepływu. |
| redirect_uri | Wymagane | Redirect_uri aplikacji, w którym odebrano authorization_code. |
| client_secret | Wymagane | Tajny aplikacji, który wygenerował w [Azure portal](https://portal.azure.com/). To hasło aplikacji jest struktura ważne zabezpieczeń. Należy zapisać go bezpiecznie na serwerze. Należy można też wykonać obrotu tego hasła klienta w regularnych odstępach czasu. |

Pomyślną odpowiedź token będzie wyglądać:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametr | Opis |
| ----------------------- | ------------------------------- |
| not_before | Czas, jaką tokenu jest traktowany jako ważne, w momencie epoch. |
| token_type | Wartość typ tokenu. Tylko typ, która obsługuje Azure AD jest okaziciela. |
| access_token | Podpisany token JWT zażądano. |
| zakres | Zakresów token jest prawidłowy, które mogą być używane w pamięci podręcznej tokenów do późniejszego użycia. |
| expires_in | Okres, który access_token jest prawidłowy (w sekundach). |
| refresh_token | Refresh_token OAuth 2.0. Aplikacja umożliwia uzyskiwanie dodatkowych tokeny po wygaśnięciu tokenu bieżącego token ten.  Refresh_tokens są tam długo i pozwalają zachować dostęp do zasobów przez dłuższy czas. Aby uzyskać więcej szczegółów zapoznaj się z [odwołania do tokenu B2C](active-directory-b2c-reference-tokens.md). Uwaga musi użycie zakresu `offline_access` autoryzacji i token żądania, aby otrzymać refresh_token. |

Błąd odpowiedzi będzie wyglądać:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kodu błędu może służyć do klasyfikowania typów błędów występujących, która umożliwia reagowanie na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania. |

## <a name="use-the-token"></a>Korzystanie z tokenu
Teraz, gdy został pomyślnie pozyskano `access_token`, możesz użyć tokenu w żądania do sieci wewnętrznej bazy danych sieci web interfejsy API przez dołączanie go w `Authorization` nagłówka:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Odświeżanie tokenu
Id_tokens są krótko. Należy je odświeżyć po ich wygaśnięciu, aby kontynuować, można uzyskać dostęp do zasobów. Możesz to zrobić przesyłając innego `POST` żądanie `/token` punktu końcowego. Tym razem zapewniają `refresh_token` zamiast `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parametr | Wymagane | Opis |
| ----------------------- | ------------------------------- | -------- |
| p | Wymagane | Zasady, które został użyty w celu uzyskania oryginalnej refresh_token. Inne zasady nie można użyć tego żądania. Zwróć uwagę, Dodaj ten parametr do **ciągu kwerendy**, nie w treści WPIS. |
| client_id | Wymagane | Identyfikator aplikacji [Azure portal](https://portal.azure.com/) przydzielone do aplikacji. |
| grant_type | Wymagane | Typ Udziel, która musi być `refresh_token` dla tego nogi przepływ kodu autoryzacji. |
| zakres | Zalecane | Rozdzielany spacjami zakresów. Wartość jednego zakresu wskazuje Azure AD obu uprawnień, które są żądane. `openid` Uprawnienia, aby zalogować się użytkownika i pobieranie danych dotyczących użytkownika w formularzu **id_tokens**wskazuje zakres. Może służyć do uzyskać tokenów do wewnętrznej bazy danych w aplikacji sieci web interfejsu API, który jest reprezentowany przez tej samej aplikacji, co klient. `offline_access` Zakres wskazuje, że aplikacji muszą **refresh_token** długotrwałe dostępu do zasobów. |
| redirect_uri | Zalecane | Redirect_uri aplikacji, w którym odebrano authorization_code. |
| refresh_token | Wymagane | Oryginalny refresh_token, które zostało nabyte w drugim nogi przepływu. Uwaga musi użycie zakresu `offline_access` autoryzacji i token żądania, aby otrzymać refresh_token. |
| client_secret | Wymagane | Tajny aplikacji, który wygenerował w [Azure portal](https://portal.azure.com/). To hasło aplikacji jest struktura ważne zabezpieczeń. Należy zapisać go bezpiecznie na serwerze. Należy można też wykonać obrotu tego hasła klienta w regularnych odstępach czasu. |

Pomyślną odpowiedź token będzie wyglądać:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametr | Opis |
| ----------------------- | ------------------------------- |
| not_before | Czas, jaką tokenu jest traktowany jako ważne, w momencie epoch. |
| token_type | Wartość typ tokenu. Tylko typ, która obsługuje Azure AD jest okaziciela. |
| access_token | Podpisany token JWT zażądano. |
| zakres | Zakresów token jest prawidłowy, które mogą być używane w pamięci podręcznej tokenów do późniejszego użycia. |
| expires_in | Okres, który access_token jest prawidłowy (w sekundach). |
| refresh_token | Refresh_token OAuth 2.0. Aplikacja umożliwia uzyskiwanie dodatkowych tokeny po wygaśnięciu tokenu bieżącego token ten.  Refresh_tokens są tam długo i pozwalają zachować dostęp do zasobów przez dłuższy czas. Aby uzyskać więcej szczegółów zapoznaj się z [odwołania do tokenu B2C](active-directory-b2c-reference-tokens.md). |

Błąd odpowiedzi będzie wyglądać:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kodu błędu może służyć do klasyfikowania typów błędów występujących, która umożliwia reagowanie na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania. |


## <a name="send-a-sign-out-request"></a>Wysyłanie zlecenia wyrejestrowywania

Gdy chcesz podpisać użytkownika się z aplikacji, nie jest wystarczająco, aby wyczyścić pliki cookie lub w inny sposób end Twojej aplikacji sesji z danym użytkownikiem. Użytkownik musi również przekierować do Azure AD się wylogować. Jeśli nie możesz to zrobić, użytkownik może być możliwe ponowne uwierzytelnianie do aplikacji bez wprowadzania ponownie swoje poświadczenia. Jest tak, ponieważ będą one miały prawidłowy pojedynczej logowania jednokrotnego sesji z usługą Azure Active Directory.

Po prostu można przekierowywać użytkownikowi `end_session_endpoint` wymieniony w tym samym łączenie OpenID metadanych dokumencie opisano w sekcji wcześniejszej "Sprawdź poprawność id_token":

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametr | Wymagane? | Opis |
| ----------------------- | ------------------------------- | ------------ |
| p | Wymagane | Zasady, które mają być używane do zalogowania użytkownika wylogowywanie się z aplikacji. |
| post_logout_redirect_uri | Zalecane | Adres URL, który użytkownik powinien nastąpi przekierowanie do po pomyślnym wyrejestrowywania. Jeśli nie jest używany, zostanie wyświetlony komunikat ogólny przez Azure AD B2C.  |

> [AZURE.NOTE]
    Podczas kierujące użytkownikowi `end_session_endpoint` wyczyści część użytkownika logowania jednokrotnego Członkowskich z Azure AD B2C, nie będzie logowania użytkownika z sesji dostawcy (protokołu IDP) społecznościowych tożsamość użytkownika. Jeśli użytkownik wybierze samego protokołu IDP podczas kolejnych logowania, ta osoba będzie się następuje ponowne uwierzytelnianie, bez wprowadzania poświadczeń. Jeśli użytkownik chce, aby wylogować się z aplikacji B2C, nie niekoniecznie oznacza to, że całkowicie wylogować się z kontem w serwisie Facebook. Jednak w przypadku kont lokalnych sesji użytkownika zostanie zakończona poprawnie.

## <a name="use-your-own-b2c-tenant"></a>Używanie własnej dzierżawie B2C

Użytkownik chce wypróbować te żądania dla siebie, należy najpierw wykonać następujące trzy kroki, a następnie zastąpić przykładowe wartości nad własnym:

- [Tworzenie dzierżawy B2C](active-directory-b2c-get-started.md)i użyj nazwy swojej dzierżawy w sekcji żądania.
- [Tworzenie aplikacji](active-directory-b2c-app-registration.md) , aby uzyskać identyfikator aplikacji i redirect_uri. Należy uwzględnić **sieci web app-web interfejsu api** w aplikacji i opcjonalnie utworzyć **hasło aplikacji**.
- [Tworzenie zasad](active-directory-b2c-reference-policies.md) uzyskanie nazwy zasad.
