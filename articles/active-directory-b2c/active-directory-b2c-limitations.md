<properties
    pageTitle="Azure Active Directory B2C: Ograniczenia i ograniczeń | Microsoft Azure"
    description="Wykaz ograniczenia z Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure Active Directory B2C: Ograniczenia i ograniczeń

Istnieje kilka funkcji i funkcji B2C usługi Azure Active Directory (Azure AD), które nie są jeszcze obsługiwane. Wiele z tych znane problemy i ograniczenia zostaną uwzględnione wystawienia faktury, ale należy pamiętać z nich, jeśli tworzysz aplikacje dla klientów indywidualnych skierowaną przy użyciu Azure AD B2C.

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>Problemy podczas tworzenia Azure AD B2C dzierżaw

Jeśli wystąpią problemy podczas [tworzenia dzierżawę Azure AD B2C](active-directory-b2c-get-started.md), zobacz [Tworzenie dzierżawy usługi Azure AD lub dzierżawy Azure AD B2C problemów i rozwiązań](active-directory-b2c-support-create-directory.md) orientacji.

Należy zauważyć, że są znane problemy, usunięcie istniejącej dzierżawy B2C i utwórz je ponownie z tą samą nazwą domeny. Musisz utworzyć dzierżawy B2C z inną nazwę domeny.

## <a name="note-about-b2c-tenant-quotas"></a>Należy pamiętać o B2C dzierżawy przydziałów

Domyślnie to liczba użytkowników w dzierżawie B2C jest ograniczone do 50 000 użytkowników. Jeśli potrzebujesz podnieść przydziału dzierżawy usługi B2C, czy kontakt z pomocą techniczną.

## <a name="branding-issues-on-verification-email"></a>Znakowanie problemów na wiadomość e-mail

Domyślne wiadomość e-mail zawiera znakowania firmy Microsoft. Zostanie on usunięty w przyszłości. Teraz możesz usunąć go przy użyciu [funkcji znakowania firmy](../active-directory/active-directory-add-company-branding.md).

## <a name="restrictions-on-applications"></a>Ograniczenia dotyczące aplikacji

Następujące typy aplikacji nie są obecnie obsługiwane w Azure AD B2C. Aby uzyskać opis obsługiwanych typów aplikacji odwołują się do [Azure AD B2C: typów aplikacji](active-directory-b2c-apps.md).

### <a name="single-page-applications-javascript"></a>Aplikacje pojedynczej strony (JavaScript)

Wiele aplikacji nowoczesny masz pojedynczej strony aplikacji (SPA) zewnętrzną napisany przede wszystkim w JavaScript i często używa framework bezpieczne uwierzytelnianie HASŁA, takich jak AngularJS, Ember.js, Durandal itd. Ten przepływ nie jest jeszcze dostępne w Azure AD B2C.

### <a name="daemons--server-side-applications"></a>Demonów / po stronie serwera aplikacji

Aplikacji, które zawierają długotrwałych procesów lub bez obecności użytkownika, które działają muszą również sposobem uzyskania dostępu do zabezpieczonych zasoby, takie jak interfejsy API sieci Web. Te aplikacje można uwierzytelnić i uzyskać tokenów przy użyciu tożsamości aplikacji (zamiast tożsamości delegowanej dla klientów indywidualnych) w [OAuth 2.0 klienta poświadczenia przepływ](active-directory-b2c-reference-protocols.md#oauth2-client-credentials-grant-flow). Ten przepływ nie jest jeszcze dostępne w Azure AD B2C, dzięki czemu na obecnym aplikacji można uzyskać tokenów tylko wtedy, gdy wystąpił interaktywnych logowania przepływu.

### <a name="standalone-web-apis"></a>Interfejsy API sieci Web autonomicznego

W AD B2C Azure użytkownik ma możliwość [tworzenia interfejs API sieci Web, która jest zabezpieczone przy użyciu tokenów OAuth 2.0](active-directory-b2c-apps.md#web-apis). Jednak tego interfejsu API sieci Web tylko będzie mógł otrzymywać tokeny od klienta, który udostępnia samej identyfikator aplikacji. Tworzenie interfejsu API sieci Web, który jest dostępny z kilku różnych klientów nie jest obsługiwane.

### <a name="web-api-chains-on-behalf-of"></a>Interfejs API łańcuchów w sieci Web (w imieniu – z)

Wiele architektury obejmuje interfejs API sieci Web, który należy nawiązać połączenie z inną podrzędne interfejs API sieci Web, oba zabezpieczone Azure AD B2C. W tym scenariuszu jest wspólny dla macierzystych klientów, którzy mają interfejs API sieci Web wewnętrznej, która z kolei wywołuje usługi online firmy Microsoft, takie jak interfejs API Azure AD wykresu.

W tym scenariuszu łańcuchowej interfejs API sieci Web są obsługiwane za pomocą udzielanie OAuth 2.0 Jwt okaziciela poświadczeń, znanej także jako przepływu w imieniu-z. Jednak przepływu w imieniu-z nie jest obecnie zaimplementowana w Azure AD B2C.

## <a name="restriction-on-libraries-and-sdks"></a>Ograniczenia w bibliotekach i SDK

Zestaw Microsoft obsługiwane bibliotek, które działają Azure AD B2C jest bardzo ograniczona w tej chwili. Mamy pomocy technicznej dla aplikacji sieci web .NET podstawie i usług, a także NodeJS sieci web aplikacji i usług.  Ponadto mamy Podgląd Biblioteka klienta .NET znana pod nazwą MSAL, który może być używany z Azure AD B2C w systemie Windows i innych aplikacji .NET.

Pracujemy obecnie ma biblioteki obsługi innych języków lub platform, łącznie z systemem iOS i Android.  Jeśli chcesz utworzyć na platformie innej niż wymienione powyżej, zaleca się przy użyciu zestawu SDK Otwórz źródło, odwołując się do naszych [OAuth 2.0 i odwołanie do OpenID połączenia protokołu](active-directory-b2c-reference-protocols.md) stosownie do potrzeb.  Azure AD B2C wykonuje OAuth i połączyć OpenID, dzięki czemu można użyć biblioteki ogólnego OAuth lub łączenie OpenID integracji.

Nasze iOS i Android szybki start samouczków za pomocą bibliotek Otwórz źródło, które przetestowano pod kątem zgodności z Azure AD B2C.  Wszystkie materiały szkoleniowe szybki start są dostępne w naszym sekcji [wprowadzenie](active-directory-b2c-overview.md#getting-started) .

## <a name="restriction-on-protocols"></a>Ograniczenia protokołów

Azure AD B2C obsługuje łączenie OpenID i OAuth 2.0. Jednak zastosowano nie wszystkie funkcje i możliwości każdego protokołu. Aby lepiej zrozumieć zakres funkcji obsługiwanych protokołu w Azure AD B2C, zapoznaj się z naszych [OpenID połączyć i odwołanie do protokołu OAuth 2.0](active-directory-b2c-reference-protocols.md). Obsługa protokołu SAML i WS Fed nie jest dostępna.

## <a name="restriction-on-tokens"></a>Ograniczenie tokenów

Wiele tokenów wydawany przez Azure AD B2C są wykonywane jako tokeny Web JSON lub JWTs. Jednak nie wszystkie informacje zawarte w JWTs (nazywanego "oświadczeniach") jest bardzo, jak powinny być lub Brak. Przykładem mogą być oświadczeniach "preferred_username" i "sub".  Jako wartości, formatowanie lub znaczenia oświadczeniach zmian w czasie tokeny zasady istniejące pozostaną niezmienione — polega na ich wartości w aplikacjach produkcji.  Zmieniających się wartości, możemy pozwala możliwość skonfigurowania tych zmian dla każdej z zasad.  Aby lepiej zrozumieć tokeny obecnie wysyłane przez usługę Azure AD B2C, zapoznaj się z naszą [odwołania do tokenu](active-directory-b2c-reference-tokens.md).

## <a name="restriction-on-nested-groups"></a>Ograniczenia dotyczące zagnieżdżonych grup

Członkostwa w grupach zagnieżdżonych nie są obsługiwane w Azure AD B2C dzierżaw. Firma Microsoft nie zamierzasz dodać tę funkcję.

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>Ograniczenie funkcja różnicy kwerendy na interfejsu API Azure AD wykresu

[Funkcja różnicy kwerendy na interfejsu API Azure AD wykresu](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query) nie jest obsługiwana w Azure AD B2C dzierżaw. To jest w naszym długoterminowy plan.

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>Problemy z zarządzaniem użytkownikami w portalu klasyczny Azure

Funkcje B2C są dostępne w portalu Azure. Jednak portalu klasyczny Azure umożliwia dostęp do innych funkcji dzierżawy, takich jak zarządzanie użytkownikami. Obecnie istnieje kilka znanych problemów z zarządzaniem użytkownikami (karta **użytkowników** ) w portalu klasyczny Azure:

- Dla lokalnego konta użytkownika (to znaczy, użytkownik, logujący przy użyciu adresu e-mail i hasła, lub nazwy użytkownika i hasła) pole **Nazwa użytkownika** nie odpowiada logowania identyfikator (adres e-mail lub nazwę użytkownika), który został użyty podczas tworzenia konta. Jest to spowodowane pole wyświetlane w portalu klasyczny Azure jest faktycznie użytkownika kapitału nazwę (UPN), który nie jest używany w scenariuszach B2C. Aby wyświetlić identyfikator logowania lokalnego konta, Znajdź obiekt użytkownika w [Eksploratorze wykresu](https://graphexplorer.cloudapp.net/). Można znaleźć ten sam problem z tym użytkownikiem społecznościowych konta (to znaczy indywidualnych, logujący z serwisem Facebook, Google + itp.), ale w takim przypadku istnieje nie identyfikator logowania, aby skontaktować się z.

    ![Konta lokalne — głównej nazwy użytkownika](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)

- Dla użytkownika lokalnego konta konieczne będzie nie można edytować dowolne pola i zapisać zmiany na karcie **profil** .

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>Problemy z resetowania w portalu klasyczny Azure hasła inicjowanych przez administratora

Po zresetowaniu hasła dla lokalnego konsumenta związane z klientami w portalu klasyczny Azure (polecenie **Resetuj hasło** na karcie **Użytkownicy** ) tę consumer nie będzie można zmienić hasło na stronie następnego logowania, jeśli jest używany podczas logowania w górę lub zaloguj się zasady i zablokowane aplikacji. Obejść ten problem za pomocą [interfejsu API Azure AD wykresu](active-directory-b2c-devquickstarts-graph-dotnet.md) Resetowanie hasła dla klientów indywidualnych (bez wygasania haseł) korzystanie z logowania zasad zamiast znakiem lub zaloguj się zasad.

## <a name="issues-with-creating-a-custom-attribute"></a>Problemy z tworzeniem atrybutu niestandardowego

[Atrybut niestandardowy dodane na Azure portal](active-directory-b2c-reference-custom-attr.md) nie od razu utworzono w dzierżawie B2C. Musisz utworzyć w dzierżawie B2C i stanie się dostępna za pośrednictwem interfejsu API wykresu za pomocą atrybut niestandardowy w co najmniej jednym z zasad dla niego.

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>Problemy z weryfikacją domeny w portalu klasyczny Azure

Obecnie nie może zweryfikować domenę pomyślnie w [portal Azure klasyczny](https://manage.windowsazure.com/).

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>Problemy z logowaniem z zasad MFA przeglądarki Safari

Żądania błędów logowania zasady (z włączonym MFA) okresowo na przeglądarki Safari z błędami HTTP 400 (niewłaściwe żądanie). Jest to ukończenia limity rozmiarów plików cookie niski i Safari. Istnieją dwa obejścia tego problemu:

- Za pomocą "zapisów lub logowania zasady" zamiast "zasad w logowania".
- Zmniejsz liczbę **oświadczeń aplikacji** żądanego w zasad.
