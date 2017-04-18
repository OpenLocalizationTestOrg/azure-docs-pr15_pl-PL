<properties
    pageTitle="Ograniczenia punktu końcowego 2.0 i ograniczenia | Microsoft Azure"
    description="Lista ograniczeń i ograniczeń z punktem końcowym 2.0 Azure AD."
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

# <a name="should-i-use-the-v20-endpoint"></a>Czy należy używać punkt końcowy 2.0?

Podczas tworzenia aplikacji, które zostały zintegrowane z usługi Azure Active Directory, musisz zdecydować, czy protokoły punktu końcowego i uwierzytelniania 2.0 — zależnie od potrzeb.  Oryginalny końcowy Azure AD jest nadal w pełni obsługiwane oraz w niektórych względem więcej sformatowany funkcji niż wersja 2.0.  Jednak 2.0 punktu końcowego [wprowadzono znaczące korzyści](active-directory-v2-compare.md) dla deweloperów, którzy mogą nakłonić użytkownika, aby użyć nowy model programowania.

W tym momencie naszych rekomendacji przy użyciu punkt końcowy 2.0 jest następująca:

- Do obsługi osobistych kont Microsoft w aplikacji, możesz użyć punkt końcowy 2.0.  Ale upewnij się, że rozumiesz ograniczenia wymienione w tym artykule, zwłaszcza odnoszących się do pracy i edukacyjne.
- Jeśli aplikacja wymaga tylko obsługi pracy i szkolnych kont, należy użyć [oryginalnego punkty końcowe Azure AD](active-directory-developers-guide.md).

Czasem punkt końcowy 2.0 wzrośnie do wyeliminować ograniczenia wymienione w tym miejscu, tak, aby tylko należy używać punktu końcowego 2.0.  W międzyczasie w tym artykule ma na celu pomóc określić, czy to punkt końcowy 2.0.  Będziemy aktualizować ten artykuł w czasie, aby odzwierciedlały bieżący stan punkt końcowy 2.0, więc zaglądaj powrót do ponowne rozpatrzenie wymagań z możliwościami 2.0.

Jeśli masz istniejącej aplikacji z usługą Active Directory Azure, która nie korzysta z punkt końcowy 2.0, wystarczy zacząć od podstaw.  W przyszłości firma Microsoft będzie dostarczać sposób, aby włączyć istniejących aplikacji Azure AD do użytku z punktem końcowym 2.0.

## <a name="restrictions-on-apps"></a>Ograniczenia dotyczące aplikacji
Następujące aplikacje nie są obecnie obsługiwane przez punkt końcowy 2.0.  Aby uzyskać opis obsługiwanych typów aplikacji odwołują się do [tego artykułu](active-directory-v2-flows.md).

##### <a name="standalone-web-apis"></a>Interfejsy API sieci Web autonomicznego
Z punktem końcowym 2.0 — masz możliwość [tworzenia interfejs API sieci Web, która jest chronione za pomocą OAuth 2.0](active-directory-v2-flows.md#web-apis).  Jednak tego interfejsu API sieci Web tylko będzie mógł otrzymywać tokenów z aplikacji, która współużytkuje ten sam identyfikator aplikacji.  Tworzenie interfejsu API, który jest dostępny na kliencie z różnych identyfikator aplikacji sieci web nie jest obsługiwane.  Ten klient nie będzie można zażądać albo uzyskać uprawnienia do interfejsu API usługi sieci web.

Aby wyświetlić sposobu tworzenia interfejs API sieci Web, która akceptuje tokeny na kliencie z tej samej aplikacji, zobacz próbki 2.0 punktu końcowego interfejs API sieci Web [Rozpoczynania](active-directory-appmodel-v2-overview.md#getting-started)pracy.

##### <a name="web-api-on-behalf-of-flow"></a>Przepływ w imieniu z interfejsu API sieci Web
Wiele architektury obejmuje interfejs API sieci Web, który należy nawiązać połączenie z inną podrzędne interfejs API sieci Web, oba zabezpieczone punkt końcowy 2.0.  W tym scenariuszu jest wspólny natywne klientów, którzy mają interfejs API sieci Web wewnętrznej bazie danych, która z kolei wywołuje usługi online firmy Microsoft lub innego niestandardowych interfejsu API sieci web obsługującego Azure AD.

W tym scenariuszu są obsługiwane za pomocą udzielanie OAuth 2.0 Jwt okaziciela poświadczeń, nazywanego przepływ On-Behalf-Of.  Jednak przepływu w imieniu-z nie jest obecnie obsługiwane dla punktu końcowego 2.0.  Aby zobaczyć, jak działa ten przepływ w ogólnodostępne Azure AD usługi, zapoznaj się z [próbki w imieniu-z kodu na GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).

## <a name="restrictions-on-app-registrations"></a>Ograniczenia dotyczące rejestracji aplikacji
W tym momencie wszystkie aplikacje, które chcesz zintegrować produkt z punktem końcowym 2.0 — należy utworzyć nową rejestrację aplikacji u [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Wszelkie istniejące Azure AD Account Microsoft aplikacje nie będą zgodne z punktem końcowym 2.0 ani będzie aplikacje zarejestrowane w dowolnym portalu oprócz portalu rejestracji aplikacji.  Planujemy umożliwia włączenie aplikacji do użytku jako aplikacji 2.0. W tej chwili jednak istnieje nie ścieżkę migracji dla aplikacji do punktu końcowego 2.0.

Podobnie aplikacje zarejestrowane w portalu rejestracji aplikacji przed oryginalny końcowy uwierzytelniania Azure AD nie będzie działać.  Jednak umożliwia aplikacji utworzonych w portalu rejestracji aplikacji pomyślnie Integracja z końcowym uwierzytelniania konta Microsoft, `https://login.live.com`.

Ponadto rejestracji aplikacji utworzonych w [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) ma następujące ostrzeżenia:

- Właściwość **strony głównej** , nazywane także **logowania jednokrotnego adres URL** nie jest obsługiwana.  Bez strony głównej te aplikacje nie będą wyświetlane w panelu Moje aplikacje pakietu Office.
- Tylko dwa hasła aplikacji są dozwolone na identyfikator aplikacji w tej chwili.
- Rejestracja aplikacji tylko można wyświetlać i zarządzane przez konto jednego Deweloper.  Nie można udostępniać między wielu deweloperów.
- Istnieje kilka ograniczeń dotyczących formatu redirect_uri dozwolone.  Zobacz następną sekcję, aby uzyskać więcej informacji.

## <a name="restrictions-on-redirect-uris"></a>Ograniczenia dotyczące przekierowania URI
Aplikacje, które są zarejestrowane w portalu rejestracji aplikacji są obecnie ograniczone do ograniczonego zestawu wartości redirect_uri.  Redirect_uri dla aplikacji sieci web i usług muszą zaczynać się schemat `https`, a wszystkie wartości redirect_uri muszą mieć jednej domeny DNS.  Na przykład nie jest możliwe do rejestrowania aplikacji sieci web, która ma redirect_uris:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Systemu rejestracji porównano całą nazwę DNS istniejącej redirect_uri z nazwą DNS redirect_uri, którą chcesz dodać. Wniosek o dodanie nazwy DNS nie powiedzie się, jeśli jest spełniony dowolny z następujących warunków:  

- Całą nazwę DNS nowego redirect_uri jest niezgodna z nazwą DNS istniejących redirect_uri
- Jeśli całą nazwę DNS nowego redirect_uri nie jest poddomeną istniejących redirect_uri

Jeśli na przykład aplikacja ma obecnie redirect_uri:

`https://login.contoso.com`

Następnie jest można dodać:

`https://login.contoso.com/new`

które dokładnie pasuje do nazwy DNS lub:

`https://new.login.contoso.com`

który jest poddomeną DNS login.contoso.com.  Jeśli chcesz mieć aplikacji, który zawiera east.contoso.com logowania i logowania west.contoso.com jako redirect_uris, możesz dodać następujące redirect_uris w kolejności:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Tych dwóch można dodać, ponieważ są one poddomen pierwszego redirect_uri, contoso.com. To ograniczenie zostanie usunięty w nowej wersji.

Aby dowiedzieć się, jak zarejestrować aplikację w portalu rejestracji aplikacji, skorzystaj z [tego artykułu](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services--apis"></a>Ograniczenia dotyczące usług i interfejsów API
Punkt końcowy 2.0 — obecnie obsługuje logowanie dla dowolnej aplikacji zarejestrowane w nowej aplikacji rejestracji portalu, pod warunkiem, że należącą do listy przepływów [obsługiwanego uwierzytelniania](active-directory-v2-flows.md).  Jednak te aplikacje tylko będą mogli uzyskać tokeny dostępu OAuth 2.0 dla bardzo ograniczonego zestawu zasobów.  Punkt końcowy 2.0 będzie tylko problemu access_tokens dla:

- Aplikacja, żądany tokenu.  Aplikację mogą uzyskiwać access_token dla siebie, jeśli aplikację logiczne składa się z kilku różnych elementów lub warstwy.  Aby sprawdzić, w tym scenariuszu w działaniu, zapoznaj się z naszą samouczki [Wprowadzenie](active-directory-appmodel-v2-overview.md#getting-started) .
- Poczta programu Outlook, kalendarz i kontakty pozostałych interfejsów API, które znajdują się w https://outlook.office.com.  Aby dowiedzieć się, jak napisać aplikację, która uzyskuje dostęp do tych interfejsów API, skorzystaj z tych samouczkach [Wprowadzenie do aplikacji pakietu Office](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) .
- Interfejsy API programu Microsoft Graph.  Aby uzyskać informacje o programu Microsoft Graph i wszystkie dane, które są dostępne, odwiedź stronę [https://graph.microsoft.io](https://graph.microsoft.io).

Żadnych innych usług są obsługiwane w tej chwili.  Więcej usług Microsoft Online zostaną dodane w przyszłości, a także obsługę własnych niestandardowych gotowy API sieci Web i usług.

## <a name="restrictions-on-libraries--sdks"></a>Ograniczenia dotyczące bibliotek i SDK
Biblioteka obsługę punkt końcowy 2.0 jest dość ograniczona w tej chwili.  Jeśli chcesz używać punktu końcowego 2.0 w aplikacji produkcji, dostępne są następujące opcje:

- Jeśli tworzysz aplikację sieci web, możesz bezpiecznie korzystać naszych ogólnodostępne pośredniczącym po stronie serwera do wykonywania logowania i token sprawdzania poprawności.  Obejmują pośredniczącym OWIN Otwórz identyfikator połączenia dla naszych dodatku NodeJS Passport i ASP.NET.  Przykłady kodu przy użyciu naszych pośredniczącym są dostępne w naszym także sekcji [Wprowadzenie](active-directory-appmodel-v2-overview.md#getting-started) .
- Dla innych platform i aplikacji macierzystych i urządzeń przenośnych można także zintegrować z punktem końcowym 2.0 przez bezpośrednio wysyłania i odbierania wiadomości protokołu w kodzie aplikacji.  2.0 — łączenie OpenID i OAuth protokoły [zostały jawnie opisano](active-directory-v2-protocols.md) ułatwiające wykonywanie takiej integracji.
- Na koniec Otwórz źródło Otwórz połączyć identyfikator i OAuth biblioteki umożliwia Integracja z punktem końcowym 2.0.  Protokół 2.0 — powinny być zgodne z wielu bibliotekami protokół Otwórz źródło bez głównych zmian.  Dostępność tych bibliotek zmienia się na językowi i platformie, a witryny sieci Web [Otwórz połączyć identyfikator](http://openid.net/connect/) i [OAuth 2.0](http://oauth.net/2/) prowadzi listę popularnych implementacji. [Azure Active Directory (AD) 2.0 i uwierzytelniania biblioteki](active-directory-v2-libraries.md) podano szczegółowe informacje, a na liście Otwórz źródło bibliotek klienta i próbki, które zostały przetestowane z punktem końcowym 2.0.

Masz również opublikowano początkowej Podgląd [Biblioteki uwierzytelniania firmy Microsoft (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) dla środowiska .NET tylko.  Jesteś Zapraszamy wypróbować tę bibliotekę w aplikacjach .NET klienta i serwera, ale jako biblioteki podglądu, których nie można dołączać przez GA jakości pomocy technicznej.

## <a name="restrictions-on-protocols"></a>Ograniczenia dotyczące protokołów
Punkt końcowy 2.0 obsługuje tylko otwarte Połącz identyfikator & OAuth 2.0.  Jednak nie wszystkie funkcje i możliwości każdego protokołu zostały włączone do punktu końcowego 2.0 — w tym:

- Łączenie OpenID `end_session_endpoint`, dzięki czemu aplikację zakończyć sesję użytkownika z punktem końcowym 2.0.
- id_tokens wydany przez punkt końcowy 2.0 zawierać tylko parowania identyfikator użytkownika.  Oznacza to, że dwóch różnych aplikacji otrzyma różnych identyfikatorach dla tego użytkownika.  Należy zauważyć, że przez badanie programu Microsoft Graph `/me` punktu końcowego, będzie można uzyskać correlatable identyfikator użytkownika, który może być używany w aplikacjach.
- nie zawierają id_tokens wydany przez punkt końcowy 2.0 — `email` rościć sobie dla użytkownika w tej chwili, nawet jeśli uzyskiwanie uprawnień użytkownika do wyświetlania poczty e-mail.
- Informacje o użytkowniku łączenie OpenID punkt końcowy. Informacje o użytkowniku punktu końcowego nie jest zaimplementowana w tej chwili na punkt końcowy 2.0.  Jednak wszystkich danych profilu użytkownika, potencjalnie otrzyma w tym punkcie końcowym jest udostępniany przez firmę Microsoft Graph `/me` punktu końcowego.
- Rola i oświadczeń grupy.  W tej chwili punkt końcowy 2.0 nie obsługuje wydające oświadczeniach grupie lub roli w id_tokens.

Aby lepiej zrozumieć zakres protokół funkcje obsługiwane w punkt końcowy 2.0, zapoznaj się z naszym [Połącz OpenID & odwołanie do OAuth 2.0 protokołu](active-directory-v2-protocols.md).

## <a name="restrictions-for-work--school-accounts"></a>Ograniczenia w przypadku kont służbowych i szkolnych
Dostępnych jest kilka funkcji określonym użytkownikom organizacji i firm firmy Microsoft, które nie są jeszcze obsługiwane przez punkt końcowy 2.0.

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>Oparte na urządzeniu dostępie warunkowym, aplikacje natywne i urządzeń przenośnych i programu Microsoft Graph
Punkt końcowy 2.0 obsługuje jeszcze urządzenia uwierzytelniania dla urządzeń przenośnych i natywnych aplikacjach, takich jak natywnej aplikacji uruchomionych dla systemu iOS lub Android.  Może to zablokować natywnej aplikacji dzwoniąc Microsoft Graph dla niektórych organizacjach.  Uwierzytelnianie urządzenie jest wymagane, gdy administrator ustawia zasadę dostępu warunkowego opartych na urządzeniach w aplikacji.  Punkt końcowy 2.0 najprawdopodobniej scenariusz dostępu warunkowego opartych na urządzeniach w przypadku administrator ustawienie zasad do zasobu w programie Microsoft Graph, na przykład interfejsu API programu Outlook.  Jeśli administrator ustawia tych zasad i natywnej aplikacji żądania tokenu do programu Microsoft Graph, żądanie ostatecznie będzie działać, ponieważ uwierzytelnianie urządzenia nie jest jeszcze obsługiwane.  Jednak aplikacje sieci Web, żądające tokeny do programu Microsoft Graph są obsługiwane, gdy skonfigurowano zasad opartych na urządzeniach.  W sieci web app scenariusz urządzenia uwierzytelniania za pośrednictwem przeglądarki sieci web użytkownika.

Projektant, prawdopodobnie nie kontrolę nad przebiegiem przez zasady są ustawione na zasoby programu Microsoft Graph lub nawet pamiętać podczas tak się dzieje.  Jeśli tworzysz aplikację służbowych i szkolnych użytkowników, należy użyć [oryginalnego końcowy Azure AD](active-directory-developers-guide.md) aż punkt końcowy 2.0 obsługuje uwierzytelnianie urządzenia.  Aby uzyskać więcej informacji o opartych na urządzeniu dostępie warunkowym zapoznaj się z [tego artykułu](active-directory-conditional-access.md#device-based-conditional-access).

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>Zintegrowane uwierzytelnianie systemu Windows dla federacyjnych dzierżaw
Jeśli użyto ADAL (i w związku z tym oryginalny końcowy Azure AD) w aplikacji systemu Windows, możliwe, że wybrano zaletą obsługującej udzielanie potwierdzenia SAML.  Ta udzielanie użytkownikom federacyjnych Azure AD dzierżaw w trybie cichym uwierzytelnianie z ich wystąpieniem usługi Active Directory w wersji lokalnej bez konieczności wprowadzania poświadczeń.  Udzielanie potwierdzenia SAML nie jest obecnie obsługiwane na punkt końcowy 2.0.
