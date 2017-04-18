<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Sposoby tworzenia aplikacji bezpośrednio za pomocą protokołów obsługiwanych przez Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C: Protokoły

Azure Active Directory (Azure AD) B2C udostępnia tożsamości jako usługi dla aplikacji dzięki obsłudze dwóch standardowych protokołów branżowe: łączenie OpenID i OAuth 2.0. Usługa jest zgodny ze standardami, ale wszelkie dwóch implementacji tych protokołów mogą mieć drobne różnice.  Informacje zawarte w tym przewodniku spowoduje przydatny wpisz swój kod bezpośrednio wysyłając i obsługi zleceń HTTP, a nie za pomocą biblioteki Otwórz źródło. Zalecamy, przeczytaj tę stronę, zanim zagłębić się do szczegółów każdego określonego protokołu. Jednak jeśli już znasz Azure AD B2C, możesz przejść bezpośrednio do [przewodniki Protocol (protokół)](#protocols).

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>Podstawowe informacje
Każdej aplikacji, która używa Azure AD B2C musi być zarejestrowana w katalogu B2C w [Azure portal](https://portal.azure.com). Proces rejestracji aplikacji zbiera i przypisuje kilka wartości do aplikacji:

- **Identyfikator aplikacji** musi jednoznacznie identyfikować aplikacji.
- **Przekierowywanie URI** lub **identyfikator pakietu** , który może być używany w celu skierowania odpowiedzi z powrotem do aplikacji.
- Kilka innych wartości specyficzne dla scenariusza. Aby uzyskać więcej informacji Dowiedz się, [jak zarejestrować aplikację](active-directory-b2c-app-registration.md).

Po zarejestrowaniu aplikacji się komunikuje Azure AD przez wysłanie żądania do punktu końcowego 2.0:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Prawie wszystkie przepływów OAuth i łączenie OpenID cztery strony uczestniczą w programie exchange:

![Role OAuth 2.0](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- **Autoryzacja serwera** jest punkt końcowy 2.0 Azure AD. Bezpieczne obsługi wszelkich sytuacji związanych z programu access i informacje o użytkowniku. Obsługuje ona również relacje zaufania między stronami w formie przepływu. Jest odpowiedzialny za zweryfikować tożsamość użytkownika, udzielanie i odwoływanie dostępu do zasobów i wydawania tokeny. Jest on również znany jako dostawcy tożsamości.
- **Właściciel zasobu** jest zwykle użytkownika końcowego. Przyjęcie, które należą dane, a moc umożliwia stronom trzecim uzyskać dostęp do tych danych lub zasobu.
- **Klient OAuth** jest aplikacji. W przypadku określenia przez jego identyfikator aplikacji. Zazwyczaj jest strona, która interakcję użytkowników końcowych. Żąda także tokenów z serwera autoryzacji. Właściciel zasobu musi udzielić uprawnień klienta dostępu do zasobu.
- **Serwer zasobów** to miejsce, w którym znajduje się zasób lub danych. Zaufaną serwer autoryzacji bezpiecznego uwierzytelniania i autoryzacji klienta OAuth. Używa też okaziciela tokeny dostępu, aby upewnić się, że można mieć udzielony dostęp do zasobu.

## <a name="policies"></a>Zasady
Mieć prawdopodobnie zasady Azure AD B2C są najbardziej istotne funkcje usługi. Azure AD B2C rozszerza standardowych protokołów OAuth 2.0 i łączenie OpenID przez wprowadzenie do zasad. Zezwalanie na te Azure AD B2C do wykonywania bardziej niż prosty uwierzytelniania i autoryzacji. Zasady opisano w pełni dla klientów indywidualnych tożsamości środowiska, łącznie z zapisów, logowania i edytowanie profilu. Zasady można zdefiniować w administracyjnej interfejsu użytkownika. Można je wykonać przy użyciu parametru zapytania specjalne w żądania HTTP uwierzytelniania. Zasady są standardowe funkcje OAuth 2.0 i połączyć OpenID, dlatego należy podjąć czasu na ich opis. Aby uzyskać więcej informacji zobacz [Przewodnik po Azure AD B2C zasad](active-directory-b2c-reference-policies.md).

## <a name="tokens"></a>Tokeny
Azure AD B2C stosowania OAuth 2.0 i łączenie OpenID wykorzystuje rozległa tokenów okaziciela, łącznie z tokenów okaziciela, które są przedstawiane jako JSON web tokeny (JWTs). Token okaziciela jest tokenu zabezpieczającego lightweight, którego udziela dostępu "okaziciela" do chronionego zasobu. Okaziciela jest każda strona, która może prezentować tokenu. Azure AD najpierw uwierzytelnienia przyjęcie może odbierać token okaziciela. Jednak jeśli kroków, które nie są wykonywane zagwarantowanie token na przekazywanie i przestrzeni dyskowej, może być przechwycone i używane przez niezamierzonych firmy.

Niektóre tokenów zabezpieczających mieć mechanizm wbudowany, który zapobiega nieupoważnione za ich pomocą, ale tokeny okaziciela nie mają tego mechanizmu. Muszą być przewożone w bezpiecznego kanału, takie jak transportu warstwy zabezpieczeń (HTTPS). Jeśli token okaziciela są przesyłane poza bezpiecznego kanału, złośliwy strony umożliwia ataki w pośrednik uzyskiwanie tokenu i używać go do uzyskania nieautoryzowanego dostępu do chronionych zasobów. Te same zasady zabezpieczeń dotyczą sytuacji okaziciela tokeny są przechowywane lub do późniejszego użycia. Zawsze upewnij się, że aplikacji przesyła i przechowuje tokeny okaziciela w bezpieczny sposób.

Aby uzyskać zagadnienia dotyczące zabezpieczeń token okaziciela dodatkowe zobacz [RFC 6750 sekcji 5](http://tools.ietf.org/html/rfc6750).

Dodatkowe informacje na temat różnych typów używanych w Azure AD B2C tokeny są dostępne w [odwołaniu token Azure AD](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protokoły

Gdy wszystko będzie już gotowe do przejrzenia niektóre żądania przykład, można uruchomić z jednym z następujących samouczki. Każdy z nich odpowiada scenariusza określonego uwierzytelniania. Jeśli potrzebujesz pomocy podczas ustalania, które przepływ jest dla Ciebie, zapoznaj się z [Typy aplikacji można tworzyć przy użyciu Azure AD B2C](active-directory-b2c-apps.md).

- [Tworzenie urządzeń przenośnych i natywnej aplikacji przy użyciu OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
- [Tworzenie aplikacji sieci web przy użyciu połączenia OpenID](active-directory-b2c-reference-oidc.md)
