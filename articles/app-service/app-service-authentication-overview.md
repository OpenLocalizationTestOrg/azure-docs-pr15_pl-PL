<properties
    pageTitle="Uwierzytelniania i autoryzacji w usłudze Azure aplikacji | Microsoft Azure"
    description="Omówienie uwierzytelniania i koncepcyjny odwołanie / autoryzacji funkcji Azure aplikacji usługi"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-app-service"></a>Uwierzytelniania i autoryzacji w usłudze Azure aplikacji

## <a name="what-is-app-service-authentication--authorization"></a>Co to jest aplikacja usługi uwierzytelniania i autoryzacji?

Aplikacja usługi uwierzytelniania / autoryzacji to funkcja, która umożliwia aplikacji do zalogowania się użytkowników, dzięki czemu nie trzeba zmienić kod do wewnętrznej bazy danych aplikacji. Umożliwia łatwy sposób ochrony aplikacji i pracę z danymi na użytkownika.

Usługa aplikacji używa tożsamości federacyjnych, w którym są przechowywane konta dostawcy tożsamości innych firm, a uwierzytelnia użytkowników. Aplikacja zależy od dostawcy tożsamości informacji tak, aby aplikacji nie ma do przechowywania informacji o tej samej. Usługa aplikacji obsługuje pięć dostawcy tożsamości okno: usługi Azure Active Directory, Facebook, Google, Account Microsoft i Twitter. Aplikacji można użyć dowolnego numeru z tych dostawców tożsamości, aby zapewnić użytkownikom z opcjami dla jak zalogować się. Aby rozwinąć wbudowanych funkcji, można zintegrować innego dostawcy tożsamości lub [rozwiązanie tożsamości niestandardowej][custom-auth].

Jeśli chcesz rozpocząć pracę od razu, zobacz jeden z następujące samouczki:

- [Dodawanie uwierzytelniania do aplikacji w systemie iOS] [ iOS] (lub [Android], [systemu Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms]lub [Cordova])
- [Uwierzytelnianie użytkownika w przypadku aplikacji interfejsu API usługi aplikacji Azure][apia-user]
- [Wprowadzenie do usługi aplikacji Azure — część 2][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>Jak działa uwierzytelnianie w aplikacji usługi

W celu uwierzytelnienia przy użyciu jednego z dostawców tożsamości, najpierw należy skonfigurować dostawcy tożsamości wiedzieć o aplikacji. Dostawcy tożsamości następnie zawiera identyfikatory i hasła, które zapewniają do aplikacji usługi. Wykonuje relacji zaufania, tak aby aplikacji usługi można sprawdzić poprawność potwierdzeń użytkownika, takie jak tokenów uwierzytelniania od dostawcy tożsamości.

Aby zalogować się użytkownika przy użyciu jednego z tych dostawców, użytkownik musi nastąpi przekierowanie do punktu końcowego, który podpisuje użytkowników dla tego dostawcy. Jeśli klientów z przeglądarki sieci web, możesz mieć aplikacji usługi automatycznego kierowania wszystkich użytkowników nieuwierzytelnionych do punktu końcowego, który podpisuje użytkowników. W przeciwnym razie należy kierować klientów `{your App Service base URL}/.auth/login/<provider>`, gdzie `<provider>` jest jednym z następujących wartości: aad, facebook, google, firma microsoft lub twitter. Scenariusze Mobile i interfejsu API opisano w sekcjach w dalszej części tego artykułu.

Użytkownicy, którzy interakcję aplikację za pomocą przeglądarki sieci web ma cookie, ustawianie, tak aby może pozostać uwierzytelniony zgodnie z ich przeglądanie aplikacji. Dla innych typów klienta, na przykład numer telefonu komórkowego, JSON token sieci web (JWT), które powinny być prezentowane w `X-ZUMO-AUTH` nagłówek, zostanie wydana do klienta. Klient aplikacji Mobile SDK będzie obsługiwany to za Ciebie. Alternatywnie tokenu tożsamości usługi Azure Active Directory lub token dostępu mogą być bezpośrednio objęte `Authorization` nagłówek jako [token okaziciela](https://tools.ietf.org/html/rfc6750).

Aplikacji usługi będzie sprawdzać poprawność plików cookie ani token aplikacji problemy do uwierzytelniania użytkowników. Aby ograniczyć, kto może uzyskiwać dostęp do aplikacji, zobacz sekcję [autoryzacji](#authorization) w dalszej części tego artykułu.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Uwierzytelnianie urządzeń przenośnych z dostawcą SDK

Gdy wszystko jest skonfigurowany do wewnętrznej bazy danych, można modyfikować klientów przenośnych Zaloguj się przy użyciu aplikacji usługi. Istnieją dwie metody poniżej:

- Za pomocą SDK publikującej dostawcę podanej tożsamości do ustalania tożsamości i uzyskać dostęp do aplikacji usługi.
- Użyć jednego wiersza kodu, tak aby klient aplikacji Mobile SDK mogli logować się użytkownicy.

>[AZURE.TIP] Większość aplikacji należy używać dostawcy SDK, aby uzyskać bardziej spójny wygląd, gdy użytkownicy Zaloguj, aby użyć funkcji odświeżania i zapoznać inne korzyści, które określa dostawcę.

Gdy używasz dostawcy SDK użytkownicy mogli logować się do środowiska, który jest bardziej mocno zintegrowany z systemem operacyjnym uruchomionym aplikacji. Zapewnia to również token dostawcy i niektóre informacje o użytkowniku na klienta, który ułatwia używanie wykresu interfejsy API do dostosowywania środowiska użytkownika. Od czasu do czasu na blogi i fora zostanie wyświetlona to określane jako "przepływu klienta" lub "przepływ kierowany klienta", ponieważ znaki kodu na komputerze klienckim w użytkowników i kod klienta ma dostęp do tokenu dostawcy.

Po uzyskaniu tokenu dostawcy musi być przesyłane aplikacji usługi sprawdzania poprawności. Po tokenu przez usługę aplikacji, aplikacji usługi tworzy nowy token aplikacji usługi, który został zwrócony do klienta. Klient aplikacji Mobile SDK ma Pomocnik sposoby zarządzania wymiany i automatycznie dołączyć tokenu na wszystkie żądania do wewnętrznej bazy danych aplikacji. Deweloperów również zachować odwołanie do token dostawcy ich wyboru.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Uwierzytelnianie urządzeń przenośnych bez dostawcy SDK

Jeśli nie chcesz skonfigurować dostawcę SDK, można zezwolić na używanie funkcji aplikacji Mobile Azure aplikacji usługi się zalogować za Ciebie. Klient aplikacji Mobile SDK będzie Otwórz widok sieci web dostawcy wybrane i zaloguj się użytkownika. Od czasu do czasu na blogi i fora będą widoczne to nazywane "serwer przepływu" lub "przepływ kierowany serwera" ponieważ serwer zarządza procesu znaki w użytkowników, a klient SDK nigdy nie odbiera token dostawcy.

Kod, aby uruchomić ten przepływ znajduje się w samouczku uwierzytelniania dla każdej platformy. Na końcu przepływu klient SDK ma tokenu usługi aplikacji, a token są automatycznie dołączane do wszystkie żądania do wewnętrznej bazy danych aplikacji.

### <a name="service-to-service-authentication"></a>Aby usługi uwierzytelniania

Mimo że można udostępnić użytkownikom dostęp do aplikacji, możesz również ufać innej aplikacji, aby nawiązać połączenie samodzielnego interfejsu API. Na przykład może mieć jedną aplikację sieci web połączeń interfejs API w innej aplikacji sieci web. W tym scenariuszu umożliwia poświadczenia konta usługi zamiast poświadczeń użytkownika uzyskać token. Konto usługi jest nazywany *głównej usługi* Azure Active Directory terminologii i uwierzytelniania przy użyciu tego konta jest nazywany scenariusz do usługi.

>[AZURE.IMPORTANT] Ponieważ uruchamianie aplikacji dla urządzeń przenośnych na urządzeniach klienckich, aplikacji dla urządzeń przenośnych wykonaj _nie_ liczba, jako zaufane aplikacje, a nie należy używać usługi przepływu kapitału. Zamiast tego należy korzystać przepływ użytkownika, który został wcześniej szczegółowe.

W przypadku scenariuszy do usługi aplikacji usługi można chronić aplikacji przy użyciu usługi Azure Active Directory. Tylko aplikacji wywołującej należy podać tokenu kapitału autoryzacji usługi Azure Active Directory, uzyskany przez podanie klienta, identyfikator i klienta tajne z usługi Azure Active Directory. Przykład w tym scenariuszu korzystającego z interfejsu API programu ASP.NET aplikacje tłumaczy samouczka [usługi kapitału uwierzytelniania dla aplikacji interfejsu API][apia-service].

Jeśli chcesz używać uwierzytelniania usługi aplikacji do obsługi scenariusz do usługi, można użyć uwierzytelniania podstawowego lub certyfikaty klienta. Aby uzyskać informacji na temat certyfikatów klienta w Azure zobacz [Jak do skonfigurowania TLS wzajemnego uwierzytelniania dla aplikacji sieci Web](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Uzyskać informacji na temat uwierzytelniania podstawowego w programie ASP.NET zobacz [Filtry uwierzytelniania w 2 interfejsu API sieci Web programu ASP.NET](http://www.asp.net/web-api/overview/security/authentication-filters).

Konto usługi uwierzytelniania z aplikacji logiki aplikacji usługi aplikacji interfejsu API jest szczególnym wypadku szczegółowo w [Korzystanie z niestandardowej interfejsu API hostowana w aplikacji usługi z aplikacjami logiki](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="authorization"></a>Jak działa autoryzacji w aplikacji usługi

Masz pełną kontrolę nad żądania, które mogą uzyskiwać dostęp do aplikacji. Aplikacja usługi uwierzytelniania / autoryzacji można skonfigurować dowolną z następujących problemów:

- Zezwalaj tylko uwierzytelnieni żądania do osiągnięcia aplikacji.

    Jeśli przeglądarka odbierze anonimowe żądanie, aplikacji usługi przekieruje do strony dla dostawcy tożsamości, wybranego przez użytkownika, tak aby użytkownicy mogli logować się. Jeśli wezwanie pochodzi z urządzenia przenośnego, odpowiedź HTTP _401 Unauthorized_ jest zwracane odpowiedzi.

    Po wybraniu tej opcji nie musisz pisać kodu uwierzytelniania w ogóle w aplikacji. W razie potrzeby autoryzacji precyzyjniej zarządzać, informacje o użytkowniku, jest dostępna w kodzie.

- Zezwalaj na wszystkie żądania do osiągnięcia aplikacji, ale sprawdzania poprawności żądań uwierzytelnionych i przekazać informacje uwierzytelniające w nagłówkach HTTP.

    Ta opcja różni się decyzji dotyczących autoryzacji w kodzie aplikacji. Większa elastyczność w obsłudze anonimowe żądania, ale trzeba napisać kod.

- Zezwalaj na wszystkie żądania do osiągnięcia aplikacji, a nie zająć się informacje uwierzytelniające w żądania.

    W tym przypadku uwierzytelnianie / autoryzacji funkcja jest wyłączona. Zadania uwierzytelniania i autoryzacji są całkowicie do kodu aplikacji.

Poprzedniego zachowania są kontrolowane przez opcję **Akcja wykonywana, gdy żądanie nie jest uwierzytelniony** w portalu Azure. Jeśli *Nazwa dostawcy *Zaloguj się przy użyciu ** **, wszystkie żądania trzeba można uwierzytelnić.** Zezwalaj na żądanie (żadnej akcji) ** różni się decyzja autoryzacji w kodzie, ale nadal zawiera informacje uwierzytelniające. Jeśli chcesz kod obsługiwać wszystkie elementy, możesz wyłączyć uwierzytelnianie i funkcji autoryzacji.

## <a name="working-with-user-identities-in-your-application"></a>Praca z tożsamości użytkowników w aplikacji

Aplikacja usługi przekazuje niektóre informacje o użytkowniku w aplikacji przy użyciu specjalnych nagłówków. Żądania zewnętrznych zabronią te nagłówki i tylko jeśli obecne ustalonym przez uwierzytelniania usługi aplikacji i autoryzacji. Kilka nagłówków przykład obejmują:

* X-MS--KAPITAŁU NAZWA KLIENTA
* X-MS-KLIENTA KAPITAŁU ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Kodu napisanego w języku ani framework przejść niezbędne informacje z tych nagłówków. W przypadku aplikacji ASP.NET 4.6 **ClaimsPrincipal** jest automatycznie ustawiany z odpowiednimi wartościami.

Aplikacja można również uzyskać dodatkowe szczegóły za pośrednictwem HTTP GET na `/.auth/me` punkt końcowy aplikacji. Prawidłowe tokenu, który jest dołączony do żądania zwróci ładunku JSON z szczegółowe informacje o dostawcy, który jest używany, źródłowych token dostawcy i innych informacji użytkownika. Serwer aplikacji Mobile SDK zapewnia sposobów Pomocnik pracować z tymi danymi. Aby uzyskać więcej informacji zobacz [jak za pomocą SDK Node.js aplikacje Mobile Azure](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)i [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumentacja i dodatkowe zasoby

### <a name="identity-providers"></a>Dostawcy tożsamości
Następujące samouczki pokazująca, jak skonfigurować usługę aplikacji do używania dostawców uwierzytelniania inny:

- [Jak skonfigurować aplikację umożliwia logowania usługi Azure Active Directory][AAD]
- [Jak skonfigurować aplikacji umożliwia logowania w serwisie Facebook][Facebook]
- [Jak skonfigurować aplikację umożliwia logowania usługi Google][Google]
- [Jak skonfigurować aplikację do logowania się do programu Microsoft Account za pomocą][MSA]
- [Jak skonfigurować aplikację umożliwia logowania Twitter][Twitter]

Jeśli chcesz użyć system tożsamości niż opisane tutaj umożliwia także [Podgląd pomocy technicznej niestandardowego uwierzytelniania na serwerze Mobile aplikacje .NET SDK][custom-auth], które mogą zostać użyte w aplikacjach sieci web, aplikacje dla urządzeń przenośnych lub aplikacji interfejsu API.

### <a name="web-applications"></a>Aplikacje sieci Web
Następujące samouczki pokazano, jak dodać uwierzytelniania do aplikacji sieci web:

- [Wprowadzenie do usługi aplikacji Azure — część 2][web-getstarted]

### <a name="mobile-applications"></a>Aplikacji dla urządzeń przenośnych
Następujące samouczki pokazująca, jak dodać uwierzytelniania dla klientów przenośnych za pomocą przepływu kierowany serwera:

- [Dodawanie uwierzytelniania do aplikacji w systemie iOS][iOS]
- [Dodawanie uwierzytelniania aplikacji systemu Android] [Android]
- [Dodawanie uwierzytelniania aplikacji systemu Windows] [Systemu Windows]
- [Dodaj uwierzytelniania aplikacji Xamarin.iOS] [Xamarin.iOS]
- [Dodaj uwierzytelniania aplikacji Xamarin.Android] [Xamarin.Android]
- [Dodaj uwierzytelniania aplikacji Xamarin.Forms] [Xamarin.Forms]
- [Dodawanie uwierzytelniania aplikacji Cordova] [Cordova]

Jeśli chcesz używać przepływu przekierowanie klienta dla usługi Azure Active Directory, skorzystaj z następujących zasobów:

- [Używanie Active Directory Authentication Library dla systemu iOS][ADAL-iOS]
- [Używanie biblioteki uwierzytelniania usługi Active Directory dla systemu Android][ADAL-Android]
- [Korzystanie z biblioteki uwierzytelniania usługi Active Directory dla systemu Windows i Xamarin][ADAL-dotnet]

Jeśli chcesz używać przepływu przekierowywany klienta serwisu Facebook, skorzystaj z następujących zasobów:

- [Użycie zestawu SDK Facebook dla systemu iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Jeśli chcesz używać przepływu przekierowywany klienta dla Twitter, skorzystaj z następujących zasobów:

- [Za pomocą serwisu Twitter tkaninie dla systemu iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Jeśli chcesz używać przepływu przekierowywany klienta do usługi Google, skorzystaj z następujących zasobów:

- [Za pomocą usługi Google logowania SDK dla systemu iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>Interfejs API aplikacji
Następujące samouczki pokazująca, jak chronić swoje aplikacje interfejsu API:

- [Uwierzytelnianie użytkownika w przypadku aplikacji interfejsu API usługi aplikacji Azure][apia-user]
- [Usługa kapitału uwierzytelniania dla aplikacji interfejsu API usługi aplikacji Azure][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Systemu Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
