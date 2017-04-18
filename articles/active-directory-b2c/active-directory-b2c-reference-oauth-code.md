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

# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: Przepływ kod autoryzacji OAuth 2.0

Udzielanie kod autoryzacji OAuth 2.0 można używać w aplikacji, które są zainstalowane na urządzeniu, uzyskanie dostępu do chronionych zasoby, takie jak interfejsy API sieci web. Przy użyciu protokołu B2C usługi Azure Active Directory (Azure AD) OAuth 2.0, możesz dodać tożsamości zapisów i logowania oraz inne zadania związane z zarządzaniem do swoich urządzeń przenośnych i stacjonarnych aplikacji. Ten przewodnik jest niezależny od języka. Ją opisano wysyłanie i odbieranie wiadomości HTTP bez przy użyciu jednej z naszych bibliotek Otwórz źródło.

<!-- TODO: Need link to libraries -->

Przepływ kod autoryzacji OAuth 2.0 zostało opisane w [sekcji 4.1 specyfikacji OAuth 2.0](http://tools.ietf.org/html/rfc6749). Można użyć go do wykonywania uwierzytelniania i autoryzacji w większości typów aplikacji, takich jak [aplikacje sieci web](active-directory-b2c-apps.md#web-apps) i [oryginalnie zainstalowane aplikacje](active-directory-b2c-apps.md#mobile-and-native-apps). Umożliwia aplikacji bezpieczne uzyskanie **access_tokens**, dzięki czemu można uzyskać dostęp do zasobów zabezpieczonych za pośrednictwem [serwera autoryzacji](active-directory-b2c-reference-protocols.md#the-basics).

Ten przewodnik poświęcona określonego podtyp OAuth 2.0 autoryzacji kod przepływu —**klientów publicznych**. Klient publicznej jest dowolną aplikację klienta, którego nie można powierzyć bezpieczne zachować integralność tajne hasło. Ta opcja uwzględnia aplikacji dla urządzeń przenośnych, aplikacje komputerowe i dużym stopniu dowolnej aplikacji, która działa na urządzeniu i musi uzyskać access_tokens. Jeśli chcesz dodać Zarządzanie tożsamościami do aplikacji sieci web przy użyciu Azure AD B2C, należy używać [Łączenie OpenID](active-directory-b2c-reference-oidc.md) zamiast OAuth 2.0.

Azure AD B2C rozszerza standardowe, które OAuth 2.0 przepływał do więcej niż prosty uwierzytelniania i autoryzacji. Podaj [**parametr zasad**](active-directory-b2c-reference-policies.md), który umożliwia dodawanie użytkowników do aplikacji, takich jak za pomocą OAuth 2.0 zapisów, logowanie i zarządzanie profilami. W tym miejscu pokażemy jak za pomocą OAuth 2.0 i zasady wdrożenia każdego z tych doświadczeń w natywnej aplikacji. Poniżej opisano również jak uzyskać dostęp do interfejsów API sieci web access_tokens.

Żądania HTTP przykładzie poniżej będą używane nasze przykładowe B2C katalogu, **fabrikamb2c.onmicrosoft.com**, a także naszych przykładowej aplikacji i zasady. Jesteś bezpłatne wypróbować żądania się siebie za pomocą tych wartości lub zamień je na własny.
Dowiedz się, jak [uzyskać katalogu własne B2C, aplikacji i zasady](#use-your-own-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. wyświetlony kod autoryzacji
Przepływ kod autoryzacji zaczyna się od klienta kierujące użytkownikowi `/authorize` punktu końcowego. To interakcyjne część przepływ, miejsce, w którym użytkownik faktycznie wykonywanie akcji. W tym żądaniu klienta wskazuje uprawnień, które trzeba ją uzyskać od użytkownika w `scope` parametru oraz zasady do wykonania w `p` parametru. Trzy przykłady znajdują się poniżej (z podziały wierszy dla czytelności), każdy za pomocą różnych zasad.

#### <a name="use-a-sign-in-policy"></a>Za pomocą zasad logowania

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Za pomocą zasad zapisów

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Za pomocą zasad Edytuj profil

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parametr | Wymagane? | Opis |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Wymagane | Identyfikator aplikacji [Azure portal](https://portal.azure.com) przydzielonych do aplikacji. |
| response_type | Wymagane | Typ odpowiedzi, który może zawierać `code` przepływ kodu autoryzacji. |
| redirect_uri | Wymagane | Redirect_uri aplikacji, której uwierzytelniania odpowiedzi można wysyłane i otrzymywane przez aplikację. Go muszą dokładnie odpowiadać jeden z redirect_uris, które zarejestrowane w portalu, z wyjątkiem, że musi być zakodowany w adresie URL. |
| zakres | Wymagane | Rozdzielany spacjami zakresów. Wartość jednego zakresu wskazuje Azure AD obu uprawnień, które są żądane. Jak zakres wskazuje, że aplikacji wymaga **token dostępu** używany przed własnych usług lub interfejsu API sieci web przy użyciu identyfikator klienta, reprezentowany przez ten sam identyfikator klienta.  `offline_access` Zakres wskazuje, że aplikacji muszą **refresh_token** długotrwałe dostępu do zasobów.  Można również użyć `openid` zakres żądania **id_token** od Azure AD B2C. |
| response_mode | Zalecane | Metodę, która powinna być używane do wysyłania wyniku authorization_code z powrotem do aplikacji. Może być "query", "form_post" lub "fragment".
| Województwo | Zalecane | Wartość zawarte w żądaniu, która będzie również zwracana w odpowiedzi token. Może być ciągiem dowolną zawartość, która ma być. Losowo wygenerowanym wartości unikatowe jest zazwyczaj używany służące do zapobiegania powstawaniu atakami fałszowaniu żądanie między witrynami. Stan służy także do kodowania informacji o stanie użytkownika w aplikacji, przed wystąpieniem żądanie uwierzytelnienia, takich jak strona, których znajdowały się na lub zasady wykonywane. |
| p | Wymagane | Zasady, które zostaną wykonane. Jest to nazwa zasady, który jest tworzony w katalogu B2C. Zasady wartości z pola Nazwa powinna rozpoczynać "b2c\_1\_". Dowiedz się więcej o zasadach w [ramach zasad Extensible](active-directory-b2c-reference-policies.md). |
| wiersz | Opcjonalne | Typ interakcji użytkownika, który jest wymagane. Tylko wartości w tej chwili jest "identyfikator logowania", który wymusza na użytkowniku wprowadź swoje poświadczenia na żądanie. Rejestracji jednokrotnej nie zostaną wprowadzone. |

Na tym etapie użytkownik zostanie poproszony do wykonania zasad przepływu pracy. Może to obejmować użytkownik podczas wprowadzania swojej nazwy użytkownika i hasła, logowanie się przy użyciu tożsamości społecznościowe, tworzący konto w usłudze katalogu lub dowolną inną liczbę kroków w zależności od sposobu zdefiniowania zasady.

Po zakończeniu zasady użytkownika Azure AD zwróci odpowiedź dotycząca aplikacji na wskazanej `redirect_uri`, przy użyciu metody określonej w `response_mode` parametru. Odpowiedź będzie dokładnie taki sam dla każdej z powyższych przypadkach, niezależnie od zasad, która została wykonana.

Pomyślną odpowiedź, która korzysta z `response_mode=query` wygląda jak:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Kod | Authorization_code, tej aplikacji. Aplikacja umożliwia kodu autoryzacji żądania access_token dla zasobu docelowego. Authorization_codes są bardzo krótko. Zazwyczaj wygasną po około 10 minut. |
| Województwo | Zobacz pełny opis w powyższej tabeli. Jeśli parametr Stan znajduje się w wezwaniu na tej samej wartości powinien być wyświetlany w odpowiedzi. Aplikacja należy sprawdzić, czy wartości stanu żądania i odpowiedzi są identyczne. |

Odpowiedzi na błędy mogą być również wysyłane do `redirect_uri` tak, aby aplikacja może podejmie wobec nich odpowiednio:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kod błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Wiadomość konkretnego problemu może pomóc w Deweloper do identyfikowania przyczyny błędu uwierzytelniania. |
| Województwo | Zobacz pełny opis w pierwszej tabeli w tej sekcji. Jeśli parametr Stan znajduje się w wezwaniu na tej samej wartości powinien być wyświetlany w odpowiedzi. Aplikacja należy sprawdzić, czy wartości stanu żądania i odpowiedzi są identyczne. |


## <a name="2-get-a-token"></a>2. uzyskać tokenu
Teraz, gdy zostało nabyte authorization_code, można zrealizować `code` token żądanego zasobu, wysyłając `POST` żądanie `/token` punktu końcowego. W Azure AD B2C tylko zasób, którego możesz zażądać token jest wewnętrznej bazy danych w aplikacji sieci web interfejsu API. Konwencja, która jest używana do żądania token do siebie jest użycie identyfikator klienta Twojej aplikacji jako zakres:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parametr | Wymagane? | Opis |
| ----------------------- | ------------------------------- | --------------------- |
| p | Wymagane | Zasady, który został użyty do uzyskania kodu autoryzacji. Inne zasady nie można użyć tego żądania. Zwróć uwagę, Dodaj ten parametr do *ciągu kwerendy*, nie w treści WPIS. |
| client_id | Wymagane | Identyfikator aplikacji [Azure portal](https://portal.azure.com) przydzielone do aplikacji. |
| grant_type | Wymagane | Typ Udziel, która musi być `authorization_code` dla przepływu kodu autoryzacji. |
| zakres | Reccommended | Rozdzielany spacjami zakresów. Wartość jednego zakresu wskazuje Azure AD obu uprawnień, które są żądane. Jak zakres wskazuje, że aplikacji wymaga **token dostępu** używany przed własnych usług lub interfejsu API sieci web przy użyciu identyfikator klienta, reprezentowany przez ten sam identyfikator klienta.  `offline_access` Zakres wskazuje, że aplikacji muszą **refresh_token** długotrwałe dostępu do zasobów.  Można również użyć `openid` zakres żądania **id_token** od Azure AD B2C. |
| Kod | Wymagane | Authorization_code, które zostało nabyte w pierwszym nogi przepływu. |
| redirect_uri | Wymagane | Redirect_uri aplikacji, w którym odebrano authorization_code. |

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
| access_token | Podpisany token JSON Token sieci Web (JWT) zażądano. |
| zakres | Zakresów token jest prawidłowy, które mogą być używane w pamięci podręcznej tokenów do późniejszego użycia. |
| expires_in | Okres, który tokenu jest prawidłowy (w sekundach). |
| refresh_token | Refresh_token OAuth 2.0. Aplikacja umożliwia uzyskiwanie dodatkowych tokeny po wygaśnięciu tokenu bieżącego token ten. Refresh_tokens są tam długo i pozwalają zachować dostęp do zasobów przez dłuższy czas. Aby uzyskać więcej szczegółów zapoznaj się z [odwołania do tokenu B2C](active-directory-b2c-reference-tokens.md). |

Błąd odpowiedzi będzie wyglądać:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kod błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Wiadomość konkretnego problemu może pomóc w Deweloper do identyfikowania przyczyny błędu uwierzytelniania. |

## <a name="3-use-the-token"></a>3. Użyj tokenu
Teraz, gdy został pomyślnie pozyskano `access_token`, możesz użyć tokenu w żądania do sieci wewnętrznej bazy danych sieci web interfejsy API przez dołączanie go w `Authorization` nagłówka:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. odświeżanie tokenu
Tokeny dostępu i identyfikator są krótko. Należy je odświeżyć po ich wygaśnięciu, aby kontynuować, można uzyskać dostęp do zasobów. Możesz to zrobić przez przesłanie innego `POST` żądanie `/token` punktu końcowego. Tym razem zapewniają `refresh_token` zamiast `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametr | Wymagane? | Opis |
| ----------------------- | ------------------------------- | -------- |
| p | Wymagane | Zasady, które został użyty w celu uzyskania oryginalnej refresh_token. Inne zasady nie można użyć tego żądania. Zwróć uwagę, Dodaj ten parametr do *ciągu kwerendy*, nie w treści WPIS. |
| client_id | Zalecane | Identyfikator aplikacji [Azure portal](https://portal.azure.com) przydzielonych do aplikacji. |
| grant_type | Wymagane | Typ Udziel, która musi być `refresh_token` dla tego nogi przepływ kodu autoryzacji. |
| zakres | Zalecane | Rozdzielany spacjami zakresów. Wartość jednego zakresu wskazuje Azure AD obu uprawnień, które są żądane. Jak zakres wskazuje, że aplikacji wymaga **token dostępu** używany przed własnych usług lub interfejsu API sieci web przy użyciu identyfikator klienta, reprezentowany przez ten sam identyfikator klienta.  `offline_access` Zakres wskazuje, że aplikacji muszą **refresh_token** długotrwałe dostępu do zasobów.  Można również użyć `openid` zakres żądania **id_token** od Azure AD B2C. |
| redirect_uri | Opcjonalne | Redirect_uri aplikacji, w którym odebrano authorization_code. |
| refresh_token | Wymagane | Oryginalny refresh_token, które zostało nabyte w drugim nogi przepływu. |

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
| access_token | Podpisany token JSON Token sieci Web (JWT) zażądano. |
| zakres | Zakresów token jest prawidłowy, które mogą być używane w pamięci podręcznej tokenów do późniejszego użycia. |
| expires_in | Okres, który token jest prawidłowy (w sekundach). |
| refresh_token | Refresh_token OAuth 2.0. Aplikacja umożliwia uzyskiwanie dodatkowych tokeny po wygaśnięciu tokenu bieżącego token ten. Refresh_tokens są tam długo i pozwalają zachować dostęp do zasobów przez dłuższy czas. Aby uzyskać więcej szczegółów zapoznaj się z [odwołania do tokenu B2C](active-directory-b2c-reference-tokens.md). |

Błąd odpowiedzi będzie wyglądać:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametr | Opis |
| ----------------------- | ------------------------------- |
| Błąd | Ciąg kod błędu, który może służyć do klasyfikowania typów błędów występujących i może służyć do reagować na błędy. |
| error_description | Komunikat o błędzie określonych, które mogą być pomocne Deweloper Określ przyczynę błędu uwierzytelniania. |

## <a name="use-your-own-b2c-directory"></a>Za pomocą katalogu B2C

Użytkownik chce wypróbować te żądania dla siebie, należy najpierw wykonać następujące trzy kroki, a następnie zastąpić przykładowe wartości nad własnym:

- [Tworzenie katalogu B2C](active-directory-b2c-get-started.md) i użyj nazwy katalogu w żądania.
- [Tworzenie aplikacji](active-directory-b2c-app-registration.md) , aby uzyskać identyfikator aplikacji i redirect_uri. Należy uwzględnić z **klientami** w aplikacji.
- [Tworzenie zasad](active-directory-b2c-reference-policies.md) uzyskanie nazwy zasad.
