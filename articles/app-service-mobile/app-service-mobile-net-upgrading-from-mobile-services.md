<properties
    pageTitle="Uaktualnienie usług mobilnych usłudze Azure aplikacji"
    description="Dowiedz się, jak łatwo uaktualnić aplikacji usług Mobile aplikacji Mobile aplikacji usługi"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Uaktualnianie istniejących usługi mobilnej Azure .NET do aplikacji usługi

Aplikacja usługi Mobile jest nowy sposób tworzenia aplikacji dla urządzeń przenośnych za pomocą Microsoft Azure. Aby uzyskać więcej informacji, zobacz [Co to są aplikacje Mobile?].

W tym temacie opisano, jak przeprowadzić uaktualnienie istniejącą aplikację wewnętrznej bazy danych programu .NET z usługi Azure Mobile do nowej aplikacji Mobile usługi aplikacji. Podczas wykonywania uaktualniania istniejącej aplikacji usług Mobile będą działać.   Jeśli chcesz uaktualnić aplikację Node.js wewnętrznej bazy danych, zapoznaj się z [uaktualniania usługi Mobile Node.js](./app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Gdy przenośnych wewnętrznej bazy danych jest uaktualniany do Azure aplikacji usługi, ma dostęp do wszystkich funkcji aplikacji usługi i rozliczenia obsługuje według [ceny aplikacji usługi], nie usługi Mobile ceny.

##<a name="migrate-vs-upgrade"></a>Migrowanie rozbudowy

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] Zaleca się tym [Przeprowadzanie migracji](app-service-mobile-migrating-from-mobile-services.md) przed rozpoczęciem uaktualnienia. W ten sposób można umieścić obie wersje aplikacji na tym samym Plan aplikacji usługi i ponoszenia bez dodatkowych opłat.

###<a name="improvements-in-mobile-apps-net-server-sdk"></a>Ulepszenia w Mobile aplikacje .NET server SDK

Uaktualnianie do nowego [Zestawu SDK aplikacje Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) zapewnia następujące korzyści:

- Bardziej elastyczne o współzależnościach NuGet. Środowisko macierzyste informacje nie są już własnej wersji pakiety NuGet, aby móc używać alternatywne wersje zgodne. Jednak w przypadku nowych bugfixes krytyczne lub aktualizacje zabezpieczeń, które Mobile Server SDK lub zależności, możesz ręcznie zaktualizować tej usługi.

- Większa elastyczność podczas SDK urządzeń przenośnych. Wyraźnie można kontrolować, które funkcje i przekierowuje są skonfigurowane, takich jak uwierzytelnianie, tabeli interfejsy API i punkt końcowy rejestracji push. Aby uzyskać więcej informacji, zobacz [jak za pomocą serwera .NET SDK dla aplikacji Mobile Azure](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup).

- Obsługa innych typów projektów ASP.NET i przekierowuje. Można teraz udostępniać kontrolery MVC i interfejs API sieci Web, w tym samym projekcie jako projektu przenośnych wewnętrznej bazy danych.

- Obsługa nowe funkcje uwierzytelniania aplikacji usługi, które umożliwiają korzystanie z typowych konfiguracji uwierzytelniania w Twojej sieci web i aplikacji dla urządzeń przenośnych.

##<a name="overview"></a>Omówienie uaktualniania

W większości przypadków uaktualniania będzie prosty sposób przechodzenia do nowego serwera aplikacji Mobile SDK i ponowne publikowanie kodu do nowego wystąpienia aplikacji Mobile. Istnieją jednak kilka scenariuszy, wymagające dodatkowej konfiguracji, takie jak scenariusze zaawansowane uwierzytelnianie i pracy z zaplanowane zadania. Każdy z tych omówiono sekcji później.

>[AZURE.TIP] Zaleca się, aby przeczytać i zrozumieć pozostałej części tego tematu całkowicie przed rozpoczęciem uaktualnienia. Zanotuj ze wszystkich funkcji korzystasz z wymienionym poniżej.

Klient usług Mobile SDK są **niezgodny z nowym serwerem aplikacji Mobile SDK** . Aby zapewnić ciągłość usługi dla aplikacji, nie należy opublikować zmiany w witrynie obsługujący opublikowanych klientów. Zamiast tego należy utworzyć nowej aplikacji dla urządzeń przenośnych pełniącej funkcję duplikat. Ta aplikacja można umieścić na tego samego planu aplikacji usługi, aby uniknąć ponoszenia dodatkowych finansowa.

Następnie, dostępne będą dwie wersje aplikacji: jedną, która pozostaje taki sam i służy opublikowanych w galerii znaków symboli i inne, co można uaktualnić i docelowy nowa wersja klienta aplikacji. Można przenieść i przetestować kod w swojej tempie, ale należy się upewnić, że wszelkie poprawki wprowadzone zostać zastosowane do obu. Gdy uznasz, że żądaną liczbę klienta, które aplikacje w warunkach naturalnych zostały zaktualizowane do najnowszej wersji, możesz usunąć oryginalny aplikacji migracją Jeśli jest to wymagane.

Pełna konspektu w procesie uaktualniania jest następująca:

1. Tworzenie nowej aplikacji Mobile
2. Aktualizowanie projektu, aby używać nowego SDK serwera
3. Zwolnij nową wersję aplikacji klienta
4. (Opcjonalnie) Usuń oryginalny wystąpienia migracją

##<a name="mobile-app-version"></a>Tworzenie drugiego wystąpienia aplikacji
Uaktualnianie pierwszym krokiem jest utworzenie zasobu aplikacji Mobile, w której będzie przechowywana nowa wersja aplikacji. Już migracji istniejących usługi mobilnej należy utworzyć tej wersji na tego samego planu hostingu. Otwórz [Azure portal] i przejdź do aplikacji migracją. Zanotuj planu aplikacji usługi pracuje.

Następnie należy utworzyć drugiego wystąpienia aplikacji, wykonując [instrukcje dotyczące tworzenia wewnętrznej bazy danych .NET](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app). Po wyświetleniu monitu o wybierz Plan usług aplikacji lub "plan hostingu" należy wybrać odpowiedni plan migracją aplikacji.

Prawdopodobnie można korzystać z tej samej bazy danych i powiadomień Centrum tak samo jak w usługach Mobile. Możesz skopiować te wartości, otwierając [Azure portal] i przechodzenie do oryginalnej aplikacji, a następnie kliknij pozycję **Ustawienia** > **Ustawienia aplikacji**. W obszarze **Parametry połączenia**, skopiuj `MS_NotificationHubConnectionString` i `MS_TableConnectionString`. Przejdź do nowej witryny uaktualnienia i wklej je do zastępowania wszelkie istniejące wartości. Powtórz ten proces dla innych ustawień aplikacji potrzeb aplikacji. Jeśli nie korzystasz z migracją usługi, parametry połączenia i ustawienia aplikacji można znaleźć na karcie **Konfigurowanie** sekcji usług Mobile [portal Azure klasyczny].

Utwórz kopię projektu programu ASP.NET aplikacji i opublikować go do nowej witryny. Za pomocą kopię aplikacji klienckiej zaktualizowany przy użyciu nowego adresu URL, sprawdź poprawność wszystko działa zgodnie z oczekiwaniami.

## <a name="updating-the-server-project"></a>Aktualizowanie programu project server

Aplikacje Mobile udostępnia nowe [Mobile aplikacji Server SDK] , która zapewnia dostęp do większości funkcji Środowisko uruchomieniowe usługi Mobile. Najpierw należy usunąć wszystkie odwołania do pakietów usług Mobile. W Menedżerze pakietów NuGet Wyszukaj `WindowsAzure.MobileServices.Backend`. Większość aplikacji zostanie wyświetlona kilku pakietach, łącznie z `WindowsAzure.MobileServices.Backend.Tables` i `WindowsAzure.MobileServices.Backend.Entity`. W takim przypadku początkowych pakietu najniższe w drzewie zależności takich jak `Entity`i usuń go. Gdy zostanie wyświetlony monit, nie usuwaj wszystkie pakiety zależne. Powtórz tę procedurę, aż usuniesz `WindowsAzure.MobileServices.Backend` się.

W tym momencie należy projekt, który nie jest już odwołuje się SDK usług Mobile.

Następnie należy dodać odwołania SDK aplikacji Mobile. Dla tego uaktualnienia większość deweloperów będzie pobrać i zainstalować `Microsoft.Azure.Mobile.Server.Quickstart` pakiet, jak to będzie uwzględniał całego zestawu wymagane.

Będzie jest kilka błędów kompilatora wynikających z różnic między SDK, ale te są łatwe do adresu i znajdują się w dalszej części tej sekcji.

### <a name="base-configuration"></a>Podstawowa konfiguracja

Następnie w WebApiConfig.cs, możesz zastąpić:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

z

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] Jeśli chcesz dowiedzieć się więcej na temat nowego serwera .NET SDK i Dodaj lub usuń funkcje z Twojej aplikacji, zobacz temat [jak za pomocą serwera .NET SDK] .

Jeśli jest aplikacji za pomocą funkcji uwierzytelniania, należy również zarejestrować pośredniczącym OWIN. W tym przypadku należy przenieść powyżej kodu konfiguracji do nowej klasy OWIN uruchamiania.

1. Dodawanie pakietu NuGet `Microsoft.Owin.Host.SystemWeb` Jeśli nie jest już dostępny w projekcie.
2. W programie Visual Studio, kliknij prawym przyciskiem myszy nad projektem, a następnie wybierz pozycję **Dodaj** -> **Nowy element**. Wybierz pozycję **Web** -> **Ogólne** -> **klasy OWIN uruchamiania**.
3. Przesuń powyżej kod MobileAppConfiguration z `WebApiConfig.Register()` do `Configuration()` metody nowej klasy uruchamiania.

Upewnij się, `Configuration()` metody kończy się na:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

Istnieją dodatkowe zmiany dotyczące uwierzytelniania opisane w poniższej sekcji pełny uwierzytelniania.

### <a name="working-with-data"></a>Praca z danymi

W usługach Mobile Nazwa aplikacji dla urządzeń przenośnych są obsługiwane jako domyślną nazwę schematu w ustawieniach Framework jednostki.

Aby upewnić się, że ten sam schemat odwołują się zgodnie z wcześniej, za pomocą następujących stanowi schematu DbContext aplikacji:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Upewnij się, że masz MS_MobileServiceName Ustawianie po wykonaniu powyższych. Możesz także podać inną nazwę schematu aplikacji dostosowała to wcześniej.

### <a name="system-properties"></a>Właściwości systemu

#### <a name="naming"></a>Nadawanie nazw

W polu Serwer usługi Azure Mobile SDK właściwości systemu zawsze zawierają podwójnego podkreślenia (`__`) prefiksu dla właściwości:

- __createdAt
- __updatedAt
- __deleted
- __version

Klient usługi Mobile SDK ma specjalną logikę w celu analizowania właściwości systemu w tym formacie.

W aplikacji Mobile Azure właściwości systemu nie są już masz specjalny format i mają następujące nazwy:

- createdAt
- updatedAt
- usunięte
- Wersja

Klient aplikacji Mobile SDK używane nowe nazwy właściwości systemu, więc żadne zmiany nie są wymagane do kodu klienta. Jednak jeśli są bezpośrednio nawiązywanie połączeń pozostałych w usłudze następnie należy zmienić zapytań odpowiednio.

#### <a name="local-store"></a>Magazyn lokalny

Zmiany nazw właściwości systemu oznacza, że dla usług Mobile lokalnej bazy danych synchronizacja w trybie offline nie jest zgodny z aplikacji Mobile. Jeśli to możliwe należy unikać uaktualniania klienta, które zostały wysłane aplikacje z usług Mobile aplikacji Mobile poczekaj, aż po oczekujące zmiany na serwerze. Następnie uaktualniona aplikacja należy używać nową nazwę pliku bazy danych.

Jeśli aplikacja klienta zostanie uaktualniony z usług Mobile do aplikacji Mobile podczas istnieją oczekujące zmiany w trybie offline w kolejce operacji, systemowej bazy danych należy zaktualizować do nowych nazw kolumn. W systemie iOS można to osiągnąć zmienianie nazw kolumn za pomocą lightweight migracji. W systemie Android i zarządzanego klienta programu .NET należy zapisać niestandardowy kod SQL, aby zmienić nazwy kolumn w tabelach obiektu danych.

W systemie iOS należy zmienić schemat podstawowych danych dla jednostek danych zgodnie z następujących czynności. Należy zauważyć, że właściwości `createdAt`, `updatedAt` i `version` już `ms_` prefiks:

| Atrybut |  Typ   | Uwaga                                                 |
|---------- |  ------ | -----------------------------------------------------|
| Identyfikator        | Ciąg oznaczona jako wymagana  | klucz podstawowy w magazynie zdalnym         |
| createdAt | Data    | (opcjonalnie) mapy właściwość systemu createdAt         |
| updatedAt | Data    | (opcjonalnie) mapy właściwość systemu updatedAt         |
| Wersja   | Ciąg  | (opcjonalnie) używane do wykrywania konfliktów, mapy do wersji |

#### <a name="querying-system-properties"></a>Kwerenda właściwości systemu

W usługach Mobile Azure, właściwości systemu nie są wysyłane domyślnie, ale tylko wtedy, gdy są wymagane, za pomocą ciągu kwerendy `__systemProperties`. Natomiast w systemie aplikacji Mobile Azure właściwości są **zawsze zaznaczona** , ponieważ są one częścią model obiektowy SDK serwera.

Ta zmiana głównie wpływa na niestandardowej implementacji menedżerów domeny, takiej jak rozszerzenia `MappedEntityDomainManager`. W usługach Mobile, jeśli klient żąda nigdy nie wszystkie właściwości systemu jest możliwe za pomocą `MappedEntityDomainManager` które rzeczywiście Mapuj wszystkie właściwości. Jednak w aplikacji Mobile Azure, tych niezamapowane właściwości powoduje błąd w kwerendach GET.

Najprostszym sposobem, aby rozwiązać ten problem, jest zmodyfikowanie programu DTOs tak, aby dziedziczą z `ITableData` zamiast `EntityData`. Następnie dodaj `[NotMapped]` atrybutu pola, które należy pominąć.

Na przykład poniższy definiuje `TodoItem` bez właściwości systemu:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Uwaga: Jeśli zostanie wyświetlony błędy `NotMapped`, Dodaj odwołanie do zestawu `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS

Usługi Mobile uwzględnione niektórych obsługę CORS zawijania rozwiązanie ASP.NET CORS. Aby nadać projektanta więcej możliwości sterowania, więc można bezpośrednio wykorzystać [CORS ASP.NET obsługują](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)została usunięta tej warstwy zawijanie.

Główne obszary zainteresowania, korzystając z CORS to `eTag` i `Location` nagłówków należy zezwolić w celu klienta SDK działał poprawnie.

### <a name="push-notifications"></a>Powiadomienia wypychane
Zamówienie główny element, który może się okazać, Brak z zestawu SDK serwera jest klasą PushRegistrationHandler. Rejestracji są nieco inaczej obsługiwane w aplikacji Mobile, a tagless rejestracji są domyślnie włączone. Zarządzanie znaczniki mogą osiągnąć przy użyciu niestandardowych interfejsów API. Zobacz instrukcje dotyczące [rejestrowania znaczniki](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) , aby uzyskać więcej informacji.

### <a name="scheduled-jobs"></a>Zaplanowane zadania
Zaplanowane zadania nie są wbudowane w aplikacji Mobile, więc istniejących zadań, które masz w swojej wewnętrznej bazy danych programu .NET będzie trzeba pojedynczo uaktualnione. Jedną z opcji jest utworzenie zaplanowane [Zadanie sieci Web] w witrynie kod aplikacji Mobile. Można również skonfigurować kontroler, na którym jest przechowywany kod zadania i skonfigurować [Harmonogram Azure] trafienie tego punktu końcowego zgodnie z harmonogramem oczekiwanych.

### <a name="miscellaneous-changes"></a>Różne zmiany
Wszystkie ApiControllers, które będą używane przez klienta przenośnego teraz musi być `[MobileAppController]` atrybut. Nie jest to już domyślnie dostępne tak że inne ApiControllers Przejdź nie mają wpływu formatters urządzeń przenośnych.

`ApiServices` Obiekt jest już część zestawu SDK. Aby uzyskać dostęp do ustawień aplikacji Mobile, można wykonać następujące czynności:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Podobnie rejestrowanie jest teraz osiągnąć, za pomocą standardowej pisania śledzenia ASP.NET:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

##<a name="authentication"></a>Zagadnienia dotyczące uwierzytelniania

Składniki uwierzytelniania usług Mobile teraz zostały przeniesione do funkcji aplikacji usług uwierzytelniania i autoryzacji. Aby Dowiedz się więcej o włączenie tej witryny, czytając temat [Dodaj uwierzytelniania usługi aplikacji dla urządzeń przenośnych](app-service-mobile-ios-get-started-users.md) .

Dla niektórych dostawców, na przykład AAD, Facebook i Google można je wykorzystać istniejące rejestracji z aplikacji do kopiowania. Należy po prostu przejdź do portalu dostawcy tożsamości i dodać nowy adres URL przekierowanie do rejestracji. Następnie skonfiguruj aplikację usługi uwierzytelniania i autoryzacji z identyfikator klienta i hasło.

### <a name="controller-action-authorization"></a>Kontroler akcji autoryzacji
Wszystkie wystąpienia `[AuthorizeLevel(AuthorizationLevel.User)]` teraz można zmienić atrybutu przy użyciu standardowych środowiska ASP.NET `[Authorize]` atrybut. Ponadto kontrolery są teraz anonimowy domyślnie, tak jak w innych aplikacjach ASP.NET.
Jeśli zostały przy użyciu jednej z innych opcji AuthorizeLevel, takich jak administrator lub aplikacji, zwróć uwagę na to, że są one usunięto. Zamiast tego można skonfigurować AuthorizationFilters dla wspólne hasła lub skonfigurować kapitału usługi AAD umożliwiające bezpieczne połączenia do usługi.

### <a name="getting-additional-user-information"></a>Uzyskiwanie informacji na temat dodatkowych użytkowników

Można uzyskać dodatkowe informacje dotyczące użytkownika, łącznie z tokeny dostępu za pośrednictwem `GetAppServiceIdentityAsync()` metody:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Ponadto jeśli aplikacja ma zależności od identyfikatory, takie jak przechowywanie ich w bazie danych, należy Zauważ, że nazwy użytkowników między usługami Mobile i Mobile usługi aplikacji są różne. Nadal można uzyskać identyfikator użytkownika usługi Mobile, ale. Wszystkie podklasy ProviderCredentials ma właściwości identyfikator użytkownika. Aby kontynuować z przykładu przed:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Jeśli aplikacji zastosować zależności nazwy użytkownika, należy wykorzystać tę samą rejestrację z dostawcą tożsamości, jeśli to możliwe. Identyfikatory użytkowników zwykle są ograniczone do rejestracji aplikacji, która została użyta, więc wprowadzenie nowej rejestracji można utworzyć problemów z dopasowanie użytkowników do swoich danych.

### <a name="custom-authentication"></a>Niestandardowe uwierzytelnianie

Jeśli aplikacji korzysta rozwiązanie niestandardowego uwierzytelniania, należy upewnij się, że uaktualnionej witryny ma dostęp do systemu. Postępuj zgodnie z instrukcjami nowego niestandardowego uwierzytelniania w [Omówienie SDK serwera .NET] Aby zintegrować rozwiązanie. Pamiętaj, że składniki niestandardowego uwierzytelniania są nadal w podglądzie.

##<a name="updating-clients"></a>Aktualizowania klientów
Po umieszczeniu operacyjne wewnętrznej bazy danych aplikacji Mobile, można pracować nad nową wersję aplikacji klienta, który używa go. Aplikacje Mobile zawiera również nowa wersja klienta SDK i podobne do uaktualnienia serwera powyżej, konieczne będzie usunięcie wszystkich odwołań do SDK usług Mobile przed zainstalowaniem wersji aplikacji Mobile.

Jedną z główne zmiany między wersjami jest to, że konstruktory już nie wymagają klawisz aplikacji. Możesz teraz po prostu przekazać adres URL aplikacji Mobile. Na przykład w klientach .NET `MobileServiceClient` Konstruktor jest teraz:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Można przeczytać o instalowaniu nowego SDK i używaniu nową strukturę za pośrednictwem z poniższych łączy:

- [iOS wersji 3.0.0 lub nowszym](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows-Xamarin) wersji 2.0.0 lub nowszym](app-service-mobile-dotnet-how-to-use-client-library.md)

Jeśli aplikacja jest stosowanie powiadomienia wypychane, zanotuj instrukcje określonej rejestracji dla każdej z tych platform, jak nie są dostępne niektóre zmiany także.

Jeśli masz nowa wersja klienta gotowa, przetestuj przed projektu uaktualnionego serwera. Po sprawdzania, czy działa, można zwolnić nowej wersji aplikacji do klientów. Po pewnym czasie po klientów mieć możliwość odbierania tych aktualizacji, można usunąć wersji usługi Mobile aplikacji. W tym momencie uaktualnieniu do aplikacji Mobile usługi dla aplikacji przy użyciu najnowszych serwera aplikacji Mobile SDK.

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Portal Azure klasyczny]: https://manage.windowsazure.com/
[Co to są aplikacje Mobile?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Zestaw SDK serwera aplikacji]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Harmonogram Azure]: /en-us/documentation/services/scheduler/
[Zadania sieci Web]: ../app-service-web/websites-webjobs-resources.md
[Jak używać .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Cennik aplikacji usługi]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Omówienie SDK serwera .NET]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
