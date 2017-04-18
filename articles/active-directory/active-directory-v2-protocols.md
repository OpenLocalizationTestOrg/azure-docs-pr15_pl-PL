<properties
    pageTitle="Protokoły 2.0 Azure AD | Microsoft Azure"
    description="Przewodnik po protokołów obsługiwanych przez punkt końcowy 2.0 Azure AD."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20--openid-connect"></a>Łączenie protokoły - OAuth 2.0 i OpenID 2.0

Punkt końcowy 2.0 — można użyć Azure AD dla tożsamości jako z usługi z branży standardowe protokoły łączenie OpenID i OAuth 2.0.  Gdy usługa jest zgodny ze standardami, może być drobne różnice między dowolne dwa implementacji tych protokołów.  Tutaj informacje będą przydatne, jeśli wybierzesz opcję wpisz swój kod bezpośrednio wysyłając & Obsługa żądania HTTP lub użyj 3rd strona Otwieranie biblioteki źródeł, a nie przy użyciu jednej z naszych bibliotek Otwórz źródło.
<!-- TODO: Need link to libraries above -->

> [AZURE.NOTE]
    Nie wszystkie scenariusze usługi Azure Active Directory i funkcje są obsługiwane przez punkt końcowy 2.0.  Aby określić, należy użyć punktu końcowego 2.0, przeczytaj o [ograniczenia 2.0](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Podstawowe informacje
W niemal wszystkich przepływów OAuth i łączenie OpenID istnieją cztery strony biorących udział w programie exchange:

![Role OAuth 2.0](../media/active-directory-v2-flows/protocols_roles.png)

- **Autoryzacja serwera** jest punkt końcowy 2.0.  Jest odpowiedzialny za zapewnienie tożsamość użytkownika, udzielanie i odwoływanie dostępu do zasobów i wydawania tokeny.  Jest on również znany jako dostawcy tożsamości — obsługuje bezpieczne nic robić z informacje o użytkowniku, ich dostępu i relacji zaufania między stronami w przepływie.
- **Właściciel zasobu** jest zwykle użytkownika końcowego.  Jest stroną, należą dane, która ma uprawnienia umożliwiające stronom trzecim uzyskać dostęp do tych danych lub zasobu.
- **Klient OAuth** jest aplikacji, określone przez jego identyfikatora aplikacji.  Zwykle jest to strona, która użytkownika końcowego interakcji z, a żądania tokenów z serwera autoryzacji.  Klient musi mieć uprawnienie dostępu do zasobu przez właściciela zasobów.
- **Serwer zasobów** to miejsce, w którym znajduje się zasób lub danych.  Relacje zaufania na serwerze autoryzacji do bezpiecznego uwierzytelniania i autoryzacji klienta OAuth i używa access_tokens okaziciela, aby upewnić się, że można mieć udzielony dostęp do zasobu.


## <a name="app-registration"></a>Rejestracja aplikacji
Co aplikację, która używa punkt końcowy 2.0 — będzie konieczne było zarejestrować w [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) przed może współdziałać przy użyciu OAuth lub łączenie OpenID.  Proces rejestracji aplikacji będzie zbieranie i przypisać kilka wartości do aplikacji:

- **Identyfikator aplikacji** musi jednoznacznie identyfikować aplikacji
- **Przekierowywanie URI** lub **Identyfikator pakietu** , który może być używany w celu skierowania odpowiedzi z powrotem do aplikacji
- Kilka innych wartości specyficzne dla scenariusza.

Aby uzyskać więcej informacji, Dowiedz się, jak [zarejestrować aplikację](active-directory-v2-app-registration.md).

## <a name="endpoints"></a>Punkty końcowe
Po zarejestrowaniu, aplikacja komunikuje Azure AD przez wysłanie żądania do punktu końcowego 2.0:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Miejsce, w którym `{tenant}` można wykonać jedną z czterech różnych wartości:

| Wartość | Opis |
| ----------------------- | ------------------------------- |
| `common` | Umożliwia użytkownikom z osobistego konta Microsoft i konta służbowego/szkolnego z usługi Azure Active Directory, aby zalogować się do aplikacji. |
| `organizations` | Umożliwia dostęp tylko użytkownicy z konta służbowego/szkolnego z usługi Azure Active Directory, aby zalogować się do aplikacji. |
| `consumers` | Umożliwia dostęp tylko użytkownicy z osobistego konta Microsoft (MSA), aby zalogować się do aplikacji. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`lub`contoso.onmicrosoft.com` | Umożliwia dostęp tylko użytkownicy z konta służbowego/szkolnego z określonego dzierżawy usługi Azure Active Directory, aby zalogować się do aplikacji.  Można używać nazwy domeny przyjazną dzierżawy Azure AD lub identyfikator guid dzierżawy.  |

Aby uzyskać więcej informacji na temat interakcję te punkty końcowe wybierz typ określonej aplikacji poniżej.

## <a name="tokens"></a>Tokeny
2.0 stosowania OAuth 2.0 i łączenie OpenID wykorzystać rozległa tokeny okaziciela, łącznie z tokeny okaziciela przedstawione w postaci JWTs. Token okaziciela jest tokenu zabezpieczającego lightweight, którego udziela dostępu "okaziciela" do chronionego zasobu. W tym znaczeniu "okaziciela", to każda strona, która może prezentować token. Jeśli przyjęcie najpierw musi uwierzytelniania z usługą Azure Active Directory, aby otrzymywać token okaziciela, jeśli nie są podjąć kroki wymagane w celu bezpiecznego token na przekazywanie i miejsca do magazynowania, można przechwycone i używane przez niezamierzonych firmy. Gdy niektóre tokenów zabezpieczających ma wbudowane mechanizm uniemożliwia nieupoważnione korzystania z nich, tokeny okaziciela nie ma tego mechanizmu i musi transportu w bezpiecznego kanału, takie jak transportu warstwy zabezpieczeń (HTTPS). Jeśli token okaziciela są przesyłane w Wyczyść, mężczyzna w środkowym atakiem mogą być używane przez złośliwy przyjęcie uzyskać tokenu i używać go uzyskania nieautoryzowanego dostępu do chronionych zasobów. Te same zasady zabezpieczeń Zastosuj podczas przechowywania lub pamięci podręcznej tokenów okaziciela do późniejszego użycia. Zawsze upewnij się, że aplikacji przesyła i przechowuje tokeny okaziciela w bezpieczny sposób. Aby uzyskać więcej zagadnienia dotyczące zabezpieczeń na okaziciela tokenów zobacz [RFC 6750 sekcji 5](http://tools.ietf.org/html/rfc6750).

Szczegółowe różnego rodzaju tokeny używane w punkt końcowy 2.0 jest dostępna w [token 2.0 — odwołanie do punktu końcowego](active-directory-v2-tokens.md).

## <a name="protocols"></a>Protokoły

Jeśli możesz już przystąpić do Zobacz niektóre żądania przykład, Rozpoczynanie pracy z jedną z poniżej samouczki.  Każdy z nich odpowiada scenariusza określonego uwierzytelniania.  Jeśli potrzebujesz pomocy w wyborze, czyli prawo przepływu dla Ciebie, zapoznaj się z [Jakiego typu aplikacje, które można tworzyć przy użyciu 2.0](active-directory-v2-flows.md).

- [Tworzenie Mobile i natywnej aplikacji z OAuth 2.0](active-directory-v2-protocols-oauth-code.md)
- [Tworzenie sieci Web aplikacje o identyfikatorze Otwórz nawiązywanie połączenia](active-directory-v2-protocols-oidc.md)
- [Tworzenie aplikacji pojedynczej strony przepływu niejawne OAuth 2.0](active-directory-v2-protocols-implicit.md)
- [Tworzenie demonów lub serwera po stronie procesów przy użyciu poświadczeń klienta OAuth 2.0 przepływu](active-directory-v2-protocols-oauth-client-creds.md)
- Uzyskiwanie tokeny interfejs API sieci Web z OAuth 2.0 w imieniu z przepływem (już wkrótce)

<!-- - Get tokens using a username & password with the OAuth 2.0 Resource Owner Password Credentials Flow (coming soon) --> 
