<properties
    pageTitle="Jak pracować z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile | Azure aplikacji usługi"
    description="Dowiedz się, jak pracować z serwera wewnętrznej bazy danych programu .NET SDK dla Mobile Azure aplikacji usługi."
    keywords="azure aplikacji usługi, aplikacji dla urządzeń przenośnych, usług mobilnych, wdrażaniem aplikacji wdrożenia azure aplikacji Skaluj skalowalna, w aplikacji usługi"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

W tym temacie pokazano, jak za pomocą serwera wewnętrznej bazy danych programu .NET SDK w kluczowe scenariusze Mobile Azure aplikacji usługi. SDK aplikacje Mobile Azure pomaga w pracy z klientów przenośnych z poziomu aplikacji ASP.NET.

>[AZURE.TIP] [.NET server SDK dla aplikacji Mobile Azure] [ 2] jest Otwórz źródło na GitHub. Repozytorium zawiera wszystkie kod źródłowy w tym zestawie testów całego serwera SDK jednostki i niektórych projektów próbki.

## <a name="reference-documentation"></a>Dokumentacji

Dokumentacji na serwerze SDK znajduje się tutaj: [Azure Mobile aplikacje .NET odwołanie][1].

## <a name="create-app"></a>Jak: tworzenie aplikacji Mobile .NET wewnętrznej bazie danych

Jeśli rozpoczynasz nowego projektu, możesz utworzyć aplikacji usługi aplikacji przy użyciu [Azure portal] lub Visual Studio. Można uruchomić aplikacji usługi aplikacji lokalnie lub opublikować projekt, aby usługi opartej na chmurze aplikacji usługi aplikacji dla urządzeń przenośnych.  

Jeśli dodajesz możliwości urządzeń przenośnych w istniejącym projekcie, zobacz sekcję [Pobieranie i zainicjować zestawu SDK](#install-sdk) .

### <a name="create-a-net-backend-using-the-azure-portal"></a>Tworzenie za pomocą portalu Azure wewnętrznej bazie danych programu .NET

Aby utworzyć aplikacji usługi przenośnych wewnętrznej bazy danych, albo wykonaj [Szybki Start samouczek] [ 3] lub wykonaj następujące czynności:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

Po powrocie do karta _wprowadzenie_ w obszarze **Utwórz tabelę interfejsu API**wybierz **C#** jako **języka wewnętrznej bazy danych**. Kliknij przycisk **Pobierz**, wyodrębnianie plików skompresowany projektu na komputer lokalny, a następnie otwórz rozwiązanie w programie Visual Studio.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Tworzenie wewnętrznej bazie danych programu .NET przy użyciu programu Visual Studio 2013 i Visual Studio 2015 r.

Instalowanie [SDK Azure dla środowiska .NET] [ 4] (wersja 2.9.0 lub nowsza) do tworzenia aplikacji Mobile Azure projektu w programie Visual Studio. Po zainstalowaniu zestawu SDK utworzyć aplikację programu ASP.NET, wykonując następujące czynności:

1. Otwórz okno dialogowe **Nowy projekt** (z *pliku* > **Nowy** > **projektu...**).
2. Rozwiń listę **Szablony** > **Visual C#**i wybierz pozycję **sieci Web**.
3. Wybierz **aplikację sieci Web programu ASP.NET**.
4. Wpisz nazwę projektu. Kliknij przycisk **OK**.
5. W obszarze _ASP.NET 4.5.2 szablonów_, wybierz **Aplikację Mobile Azure**. Zaznacz **hosta w chmurze** do tworzenia przenośnych wewnętrznej bazy danych w chmurze, do którego można opublikować tego projektu.
6. Kliknij **przycisk OK**.

## <a name="install-sdk"></a>Jak: pobieranie i zainicjować zestawu SDK

Zestaw SDK jest dostępna na [NuGet.org]. Ten pakiet zawiera podstawowe funkcje wymagane do rozpocząć korzystanie z zestawu SDK. Aby zainicjować zestawu SDK, należy wykonywać operacje na obiekcie **HttpConfiguration** .

### <a name="install-the-sdk"></a>Zainstaluj zestaw SDK

Aby zainstalować zestaw SDK, kliknij prawym przyciskiem myszy programu project server w programie Visual Studio, wybierz pozycję **Zarządzaj pakietów NuGet**, wyszukiwać pakiet [Microsoft.Azure.Mobile.Server] , a następnie kliknij przycisk **Zainstaluj**.

###<a name="server-project-setup"></a>Inicjowanie programu project server

Projekt serwera wewnętrznej bazy danych programu .NET zainicjowano podobne do innych projektów programu ASP.NET, dołączając klasę startową OWIN. Upewnij się, że masz odwołania do pakietu NuGet `Microsoft.Owin.Host.SystemWeb`. Aby dodać ten zajęć w programie Visual Studio, kliknij prawym przyciskiem myszy programu project server, a następnie wybierz pozycję **Dodaj** > 
**Nowy element**, a następnie **Web** > **Ogólne** > **klasy OWIN uruchamiania**.  Klasy jest generowany atrybutem następujące czynności:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

W `Configuration()` metody OWIN klasy uruchamiania, skonfigurować środowisko aplikacji Mobile Azure za pomocą obiektu **HttpConfiguration** .
Poniższy przykład inicjuje project server nie dodano funkcje:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Aby włączyć poszczególnych funkcji, musisz wywołać metody rozszerzenia obiektu **MobileAppConfiguration** przed nawiązywania połączeń z **ApplyTo**. Na przykład poniższy kod dodaje trasy domyślne do wszystkich kontrolerów interfejsu API, które mają atrybut `[MobileAppController]` podczas inicjowania:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

Szybki Start serwera z portalu Azure połączeń **UseDefaultConfiguration()**. Ten równoważne następujące ustawienia:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

Metody rozszerzenia używane są następujące:

* `AddMobileAppHomeController()`udostępnia domyślnej strony głównej aplikacji Mobile Azure.
* `MapApiControllers()`niestandardowe funkcje interfejsu API przewiduje kontrolery WebAPI udekorowanego z `[MobileAppController]` atrybut.
* `AddTables()`zawiera mapowanie `/tables` punktów końcowych do kontrolerów tabeli.
* `AddTablesWithEntityFramework()`jest dostępny krótkiej do mapowania `/tables` punkty końcowe przy użyciu struktury jednostki podstawie kontrolerów.
* `AddPushNotifications()`zapewnia prostą metodę rejestrowania urządzenia dla koncentratorów powiadomienie.
* `MapLegacyCrossDomainController()`zapewnia standardowych nagłówków CORS rozwoju lokalnego.

### <a name="sdk-extensions"></a>Rozszerzenia SDK

Następujące pakiety rozszerzenia oparte na NuGet zapewniają różnych urządzeń przenośnych funkcje, które mogą być używane przez aplikację. Możesz włączyć rozszerzenia podczas inicjowania przy użyciu obiektu **MobileAppConfiguration** .

- [Microsoft.Azure.Mobile.Server.Quickstart] obsługuje podstawowe ustawienia aplikacji Mobile. Dodany do konfiguracji, dzwoniąc metoda rozszerzenia **UseDefaultConfiguration** podczas inicjowania. To rozszerzenie zawiera po rozszerzenia: powiadomienia, uwierzytelnianie, obiekt, tabele, między domenami i pakietów Narzędzia główne. Ten pakiet jest używany przez Szybki Start aplikacje Mobile, które są dostępne w portalu Azure.

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) 
   wykonuje domyślny *jest tej aplikacji dla urządzeń przenośnych i rozpocząć pracę strony* dla na poziomie głównym witryny sieci web. Wywoływanie metody rozszerzenia   **AddMobileAppHomeController** , aby dodać do konfiguracji.

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) 
   zawiera klasy do pracy z danymi i zestawy up proces danych. Wywoływanie metody rozszerzenia **AddTables** , aby dodać do konfiguracji.

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) 
   umożliwia Framework jednostki dostęp do danych w bazie danych SQL. Wywoływanie metody rozszerzenia **AddTablesWithEntityFramework** , aby dodać do konfiguracji.

- [Microsoft.Azure.Mobile.Server.Authentication] umożliwia uwierzytelnianie i zestawy up pośredniczącym OWIN używany do sprawdzania poprawności tokenów. Dodawanie do konfiguracji, dzwoniąc **AddAppServiceAuthentication**  
   i **IAppBuilder**. Metody rozszerzenia **UseAppServiceAuthentication** .

- [Microsoft.Azure.Mobile.Server.Notifications] umożliwia powiadomienia wypychane i definiuje punkt końcowy rejestracji push. Wywoływanie metody rozszerzenia **AddPushNotifications** , aby dodać do konfiguracji.

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) 
   tworzy kontrolerze służy dane z Twojej aplikacji Mobile starsze przeglądarki. Wywoływanie metody rozszerzenia   **MapLegacyCrossDomainController** , aby dodać do konfiguracji.

- [Microsoft.Azure.Mobile.Server.Login] zawiera metodę AppServiceLoginHandler.CreateToken(), która jest metodą statyczną używane podczas scenariusze niestandardowego uwierzytelniania.   

## <a name="publish-server-project"></a>Jak: publikowanie na serwerze project Server

W tej sekcji pokazano, jak opublikować projekt wewnętrznej bazy danych z programu Visual Studio .NET. Można także wdrożyć projektu wewnętrznej bazy danych przy użyciu cyfra lub innych metod omówione w [dokumentacji wdrażania usługi aplikacji Azure](../app-service-web/web-sites-deploy.md).

1. W programie Visual Studio Odbuduj projekt, aby przywrócić NuGet pakietów.

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projektu, kliknij pozycję **Publikuj**. Podczas pierwszego publikowania, należy zdefiniować Publikowanie profilu. Gdy masz już zdefiniowany profil, można go zaznaczyć i kliknij pozycję **Publikuj**.

2. Jeśli zostanie wyświetlony monit, aby wybrać obiekt docelowy publikowania, kliknij pozycję **Microsoft Azure aplikacji usługi** > **Dalej**, a następnie (w razie potrzeby) Zaloguj się przy użyciu poświadczeń Azure. 
   Visual Studio do pobrania i bezpieczne są przechowywane do ustawień bezpośrednio z Azure publikowania.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)

3. Wybierz pozycję **subskrypcji**, wybierz **Typ zasobu** z **widoku**, rozwiń **Aplikacji Mobile**i kliknij pozycję do wewnętrznej bazy danych aplikacji Mobile, a następnie kliknij **przycisk OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)

4. Weryfikowanie informacji o profilu publikowania i kliknij przycisk **Publikuj**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Do wewnętrznej bazy danych aplikacji Mobile został opublikowany pomyślnie, pojawi się Strona początkowa, wskazująca sukcesu.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

##<a name="define-table-controller"></a>Jak: Definiowanie kontrolerze tabeli

Definiowanie kontrolera tabeli, aby udostępnić tabeli SQL dla klientów przenośnych.  Konfigurowanie kontrolera tabeli wymaga wykonania trzech kroków:

1. Tworzenie klasy danych przeniesienie obiektu (DTO).
2. Konfigurowanie odwołania do tabeli w klasie Mobile DbContext.
3. Tworzenie kontrolera tabeli.

Skanowania danych przeniesienie obiektu (DTO) to zwykły C# obiekt dziedziczy `EntityData`.  Na przykład:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

DTO jest używana do definiowania tabeli w bazie danych SQL.  Aby utworzyć wpis bazy danych, Dodaj `DbSet<>` właściwości DbContext używasz.  W domyślnym szablonie projektu dla aplikacji Mobile Azure, nosi nazwę DbContext `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Jeśli masz SDK Azure zainstalowany, teraz można utworzyć kontrolera szablonu tabeli w następujący sposób:

1. Kliknij prawym przyciskiem myszy folder kontrolerów i wybierz polecenie **Dodaj** > **... kontrolerze**.
2. Wybierz opcję **Azure Mobile aplikacje tabeli kontrolerze** , a następnie kliknij przycisk **Dodaj**.
3. W oknie dialogowym **Dodawanie kontrolera** :
    * Na liście rozwijanej **Klasa modelu** zaznacz do nowej DTO.
    * Na liście rozwijanej **DbContext** wybierz klasę DbContext usługi Mobile.
    * Nazwa kontrolera jest tworzony.
4. Kliknij przycisk **Dodaj**.

Project server Szybki Start zawiera przykład prostej **TodoItemController**.

### <a name="how-to-adjust-the-table-paging-size"></a>Jak: dopasowywanie rozmiaru tabeli nawigacji między stronami

Domyślnie aplikacje Mobile Azure zwraca 50 rekordów na żądanie.  Stronicowanie gwarantuje, że klient nie blokuje ich wątku interfejsu użytkownika ani na serwerze przez dłuższy czas zapewnić poprawne działanie. Aby zmienić rozmiar tabeli stronicowanie, Zwiększ po stronie serwera "dozwolony rozmiar kwerendy" i rozmiar strony po stronie klienta po stronie serwera "dozwolony rozmiar kwerendy" zostanie dopasowana przy użyciu `EnableQuery` atrybut:

    [EnableQuery(PageSize = 500)]

Upewnij się, wartość PageSize jest taki sam lub większy niż rozmiar żądany przez klienta.  Zapoznaj się z określonego klienta dokumentacji instrukcje, aby uzyskać szczegółowe informacje na temat zmieniania rozmiaru strony klienta.

## <a name="how-to-define-a-custom-api-controller"></a>Jak: Definiowanie niestandardowego kontrolera interfejsu API

Niestandardowy kontroler interfejsu API zawiera najbardziej podstawowe funkcje do sieci wewnętrznej bazy danych aplikacji Mobile, umożliwiając punktu końcowego. Można zarejestrować Kontroler interfejsu API specyficzne dla mobile przy użyciu atrybutu [MobileAppController]. `MobileAppController` Atrybut rejestruje trasę, konfiguruje serializatora JSON aplikacje Mobile i włącza [Sprawdzanie wersji klienta](app-service-mobile-client-and-server-versioning.md).

1. W programie Visual Studio, kliknij prawym przyciskiem myszy folder kontrolery, a następnie kliknij przycisk **Dodaj** > **kontrolerze**, wybierz pozycję **Kontroler 2 interfejsu API sieci Web&mdash;pustego** i kliknij przycisk **Dodaj**.

2. Wprowadź **nazwę kontrolera**, takich jak `CustomController`i kliknij przycisk **Dodaj**.

3. Nowy plik klasy kontrolerze, Dodaj następujący za pomocą instrukcji:

        using Microsoft.Azure.Mobile.Server.Config;

4. Zastosowanie atrybutu **[MobileAppController]** do definicji klasy Kontroler interfejsu API, tak jak w poniższym przykładzie:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }

4. W pliku App_Start/Startup.MobileApp.cs Dodaj wywołanie metody rozszerzenia **MapApiControllers** , tak jak w poniższym przykładzie:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Można również użyć `UseDefaultConfiguration()` rozszerzenia zamiast metody `MapApiControllers()`. Nadal jest możliwy dowolnego kontrolera, który nie zawiera **MobileAppControllerAttribute** zastosowane przez klientów, ale go może nie być poprawnie używane przez klientów korzystających z dowolnego klienta aplikacji Mobile SDK.

## <a name="how-to-work-with-authentication"></a>Jak: Praca z uwierzytelniania

Azure aplikacji Mobile używa aplikacji usług uwierzytelniania i autoryzacji do zabezpieczenia usługi przenośnych wewnętrznej bazy danych.  W tej sekcji pokazano, jak wykonywać następujące zadania związane z uwierzytelniania w projekcie serwera wewnętrznej bazy danych programu .NET:

+ [Jak: Dodawanie uwierzytelniania do programu project server](#add-auth)
+ [Jak: Użyj niestandardowych uwierzytelniania dla aplikacji](#custom-auth)
+ [Jak: Pobierz uwierzytelnieniu informacje o użytkowniku](#user-info)
+ [Jak: Ogranicz dostęp do danych dla autoryzowanych użytkowników](#authorize)

### <a name="add-auth"></a>Jak: Dodawanie uwierzytelniania do programu project server

Uwierzytelnianie można dodać do programu project server, wydłużając obiekt **MobileAppConfiguration** i konfigurując pośredniczącym OWIN. Po zainstalowaniu pakietu [Microsoft.Azure.Mobile.Server.Quickstart] i połączeń metoda rozszerzenia **UseDefaultConfiguration** , można przejść do kroku 3.

1. W programie Visual Studio należy zainstalować pakiet [Microsoft.Azure.Mobile.Server.Authentication] .

2. W pliku projektu Startup.cs Dodaj poniższy wiersz kodu na początku metody **konfiguracji** :

        app.UseAppServiceAuthentication(config);

    Ten składnik pośredniczącym OWIN sprawdza tokeny wydawany przez skojarzona brama aplikacji usługi.

3. Dodawanie `[Authorize]` atrybutów kontroler lub metodę, która wymaga uwierzytelniania. 

Aby uzyskać informacje o uwierzytelnienia klientom do wewnętrznej bazy danych aplikacji Mobile, zobacz [Dodaj uwierzytelniania aplikacji](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Jak: Użyj niestandardowych uwierzytelniania dla aplikacji

Jeśli nie chcesz używać jednego z dostawców usług aplikacji uwierzytelniania i autoryzacji, można zaimplementować systemu logowania. Zainstaluj pakiet [Microsoft.Azure.Mobile.Server.Login] pomagające Generowanie token uwierzytelniania.  Podaj własny kod sprawdzania poprawności poświadczeń użytkownika. Na przykład może sprawdzić przed solone i mieszanych hasła w bazie danych. W poniższym przykładzie `isValidAssertion()` metody (zdefiniowany w innym miejscu) jest odpowiedzialny za kontrole.

Niestandardowe uwierzytelnianie są wyświetlane przez tworzenie ApiController i Uwidacznianie `register` i `login` akcje. Klient powinien używać niestandardowe elementy interfejsu użytkownika na potrzeby zbierania informacji przez użytkownika.  Informacje jest następnie przesyłany do interfejsu API standardowy połączenia HTTP POST. Gdy serwer sprawdza potwierdzenia, tokenu jest wydawany przy użyciu `AppServiceLoginHandler.CreateToken()` metody.  Użyj **nie powinien** ApiController `[MobileAppController]` atrybut. 

Przykład `login` akcji:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

W powyższym przykładzie LoginResult i LoginResultUser są obiekty można Uwidacznianie wymagane właściwości. Klient oczekuje odpowiedzi logowania mają się pojawić jako obiekty JSON formularza:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

`AppServiceLoginHandler.CreateToken()` Metoda zawiera _grupy odbiorców_ , a parametrem _wystawcy_ . Oba parametry są ustawione na adres URL katalogu aplikacji przy użyciu schematu HTTPS. Podobnie należy skonfigurować _secretKey_ się, że wartość aplikacji podpisania klucza. Nie rozpowszechnienie klucz podpisywania w kliencie, jak może być użyty do trudniej zdobyć klawiszy i personifikacji użytkowników. Można uzyskać klucz podpisywania podczas obsługiwany w aplikacji usługi odwołując się _witryny sieci Web\_AUTH\_PODPISYWANIA\_klucza_ zmiennej środowiska. W razie potrzeby w lokalnym kontekście debugowania, postępuj zgodnie z instrukcjami [lokalne debugowania za pomocą uwierzytelniania](#local-debug) sekcji, aby pobrać klucza i zapisać go jako ustawienia aplikacji.

Wystawiony token może także zawierać inne roszczenia i datę wygaśnięcia.  Minimalny wystawiony token musi zawierać roszczeń tematu (**sub**).

Można obsługiwać klientów standardowych `loginAsync()` metody przeciążenie trasę uwierzytelniania.  Jeśli klient `client.loginAsync('custom');` do logowania, należy do rozsyłania `/.auth/login/custom`.  Można ustawić trasę przy użyciu kontrolera niestandardowego uwierzytelniania `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

>[AZURE.TIP] Za pomocą `loginAsync()` podejście gwarantuje, że token uwierzytelniania dołączony do każdej kolejnej połączenia z usługą.

###<a name="user-info"></a>Jak: Pobierz uwierzytelniony informacje o użytkowniku

Po uwierzytelnieniu użytkownika przez usługę aplikacji, masz dostęp do ID przypisanego użytkownika i inne informacje w kodzie wewnętrznej bazy danych .NET. Informacje o użytkowniku może być używany do podejmowania decyzji dotyczących autoryzacji w wewnętrznej bazy danych. Poniższy kod uzyskuje identyfikator użytkownika skojarzonego z żądaniem:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

Identyfikatory zabezpieczeń pochodzi od Identyfikatora użytkownika specyficzne dla dostawcy i jest statyczny dla danego użytkownika i dostawca logowania.  Identyfikatory zabezpieczeń ma wartość null tokeny nieprawidłowego uwierzytelniania.

Usługi aplikacji pozwala także na żądanie określonego roszczeń od dostawcy logowania. Każdy dostawcy tożsamości mogą dostarczyć więcej informacji za pośrednictwem dostawcy tożsamości SDK.  Na przykład służy interfejsu API wykresu Facebook dla znajomych informacji.  Możesz określić oświadczeń, które są wymagane w karta dostawcy w portalu Azure. Niektóre oświadczeniach wymagać dodatkowej konfiguracji z dostawcą tożsamości.

Poniższy kod wywołuje metodę rozszerzenia **GetAppServiceIdentityAsync** uzyskać poświadczenia logowania, zawierające token dostępu niezbędnych do uzyskania żądania API serwisu Facebook wykresu:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Dodawanie przy użyciu poufności informacji dotyczące `System.Security.Principal` o podanie metoda rozszerzenia **GetAppServiceIdentityAsync** .

### <a name="authorize"></a>Jak: Ogranicz dostęp do danych dla autoryzowanych użytkowników

W poprzedniej sekcji możemy pokazano, jak pobrać identyfikator użytkownika uwierzytelnionego użytkownika. Można ograniczyć dostęp do danych i inne zasoby na podstawie tej wartości. Na przykład dodanie kolumny Identyfikator użytkownika do tabel i filtrowanie wyników kwerendy przy użyciu Identyfikatora użytkownika jest prosty sposób, aby ograniczyć zwracane dane tylko dla autoryzowanych użytkowników. Poniższy kod zwraca wierszy danych tylko wtedy, gdy identyfikator zabezpieczeń pasuje do wartości w kolumnie Nazwa użytkownika w tabeli TodoItem:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

`Query()` Zwraca metody `IQueryable` którą może używać LINQ obsługę filtrowania.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Jak: Dodawanie wypychanych powiadomień programu project server

Dodawanie powiadomienia wypychane do programu project server rozszerzanie obiektu **MobileAppConfiguration** i tworząc klienta koncentratory powiadomienie.

1. W programie Visual Studio, kliknij prawym przyciskiem myszy programu project server i kliknij pozycję **Zarządzaj pakiety NuGet**, wyszukaj `Microsoft.Azure.Mobile.Server.Notifications`, następnie kliknij przycisk **Zainstaluj**. 

2. Powtórz ten krok, aby zainstalować `Microsoft.Azure.NotificationHubs` pakietu, obejmującego Biblioteka klienta koncentratory powiadomienie.

3. W App_Start/Startup.MobileApp.cs i dodać wywołanie metody rozszerzenia **AddPushNotifications()** podczas inicjowania:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

4. Dodaj następujący kod, który tworzy klienta koncentratory powiadomień:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Za pomocą klienta koncentratory powiadomień można teraz wysyłania powiadomień wypychanych zarejestrowanych urządzeń. Aby uzyskać więcej informacji zobacz [Dodawanie powiadomienia wypychane do aplikacji](app-service-mobile-ios-get-started-push.md). Aby dowiedzieć się więcej na temat koncentratory powiadomienie, zobacz [Omówienie koncentratory powiadomienie](../notification-hubs/notification-hubs-push-notification-overview.md).

##<a name="tags"></a>Jak: Włączanie przeznaczona wypychanych przy użyciu tagów

Powiadomienie o koncentratory umożliwia wysyłanie wybranych powiadomienia do określonych rejestracji za pomocą znaczników. Kilka znaczników są tworzone automatycznie:

* Identyfikator instalacji służy do identyfikowania konkretnego urządzenia.
* Identyfikator użytkownika według uwierzytelniony identyfikator zabezpieczeń służy do identyfikowania określonego użytkownika.

Identyfikator instalacji jest możliwy z właściwość **installationId** **MobileServiceClient**.  W poniższym przykładzie pokazano, jak za pomocą Identyfikatora instalacji Dodawanie znacznika do określonych instalacji w koncentratory powiadomień:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Wszystkie znaczniki dostarczoną przez klienta podczas rejestrowania powiadomień wypychanych są ignorowane przez wewnętrznej bazy danych, tworząc instalacji. Aby włączyć klienta dodać znaczniki do instalacji, możesz utworzyć niestandardowe interfejs API, który dodaje znaczniki za pomocą powyższej deseniu. 

Wyświetlić [znaczniki powiadomień wypychanych dodane klienta] [ 5] w próbce złożonym Szybki Start Mobile usługi aplikacji, na przykład.

##<a name="push-user"></a>Jak: wysyłania powiadomień wypychanych dla uwierzytelnionych użytkowników

Gdy żaden uwierzytelniony użytkownik rejestruje powiadomienia wypychane, znacznika identyfikator użytkownika zostaną automatycznie dodane do rejestracji. Korzystając z tego tagu, możesz wysłać powiadomienia wypychane do wszystkich urządzeń zarejestrowany przez tę osobę. Poniższy kod pobiera identyfikator zabezpieczeń użytkownika zgłaszającego żądanie i wyśle odbiorcy powiadomienie wypychanych szablon rejestracji urządzenia dla tej osoby:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Podczas rejestrowania dla powiadomień wypychanych w kliencie uwierzytelnionego, upewnij się, że przed podjęciem rejestracji zakończeniem uwierzytelniania. Aby uzyskać więcej informacji, zobacz [Push użytkownikom] [ 6] w próbce Mobile usługi aplikacji złożonym Szybki Start dla .NET wewnętrznej bazy danych.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Jak: debugowanie i rozwiązywanie problemów z .NET Server SDK

Azure aplikacji usługi zawiera kilka debugowania i techniki dla aplikacji ASP.NET rozwiązywania problemów:

- [Monitorowanie Azure usługi aplikacji](../app-service-web/web-sites-monitor.md)
- [Włącz rejestrowanie diagnostyczne w usłudze Azure aplikacji](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Rozwiązywanie problemów z usługą Azure aplikacji w programie Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Rejestrowanie

Dzienniki diagnostyczne usługi aplikacji można napisać, za pomocą standardowej pisania śledzenia programu ASP.NET. Aby można było tworzyć dzienniki, należy włączyć diagnostyki w swojej aplikacji Mobile wewnętrznej bazy danych.

Aby włączyć narzędzia diagnostyczne i zapisać dzienniki:

1. Postępuj zgodnie z instrukcjami [włączania diagnostyki](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).

2. Dodaj następujący za pomocą instrukcji w pliku kodu:

        using System.Web.Http.Tracing;

3. Utwórz zapis śledzenia, aby zapisać z wewnętrznej bazy danych programu .NET dzienniki diagnostyczne w następujący sposób:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");

4. Ponowne publikowanie programu project server i uzyskać dostęp do wewnętrznej bazy danych aplikacji Mobile wykonać ścieżkę kodu z rejestrowanie.

5. Pobieranie i ocenianie dzienniki, zgodnie z opisem w [jak: Pobierz dzienniki](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Lokalne debugowania za pomocą uwierzytelniania

Można uruchamiać aplikacji lokalnie, aby sprawdzić zmiany przed opublikowaniem ich w chmurze. Dla większości aplikacji Mobile Azure wewnętrznych bazach danych naciśnij klawisz *F5* znajduje się w programie Visual Studio. Istnieją pewne dodatkowe kwestie podczas korzystania z uwierzytelniania.

Musi być oparte na chmurze aplikacji dla urządzeń przenośnych z aplikacji usługi uwierzytelniania i autoryzacji skonfigurowane, a klient może zawierać punkt końcowy chmury określony jako host login alternatywny. Zapoznaj się z dokumentacją dla swojej platformy klienta dla określonych czynności wymagane.

Zapewnić [Microsoft.Azure.Mobile.Server.Authentication] zainstalowane usługi przenośnych wewnętrznej bazy danych. Następnie w klasie uruchamiania OWIN aplikacji, Dodaj następujące polecenie, po `MobileAppConfiguration` jest stosowany do swojego `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

W poprzednim przykładzie należy skonfigurować ustawienia aplikacji _authAudience_ i _authIssuer_ w pliku Web.config każdemu adres URL katalogu aplikacji przy użyciu schematu HTTPS. Podobnie należy skonfigurować _authSigningKey_ się, że wartość aplikacji podpisania klucza. Aby uzyskać klucz podpisywania:

1. Przejdź do aplikacji w [Azure portal] 
2. Kliknij przycisk **Narzędzia** **Kudu** **Przejdź**.
3. W witrynie usługi zarządzania Kudu kliknij **środowiska**.
4. Znajdowanie wartości _witryny sieci Web\_AUTH\_PODPISYWANIA\_klucza_. 

Użyj klawisza podpisywania w parametrze _authSigningKey_ w pliku config miejscowego.  Z urządzeń przenośnych wewnętrznej bazy danych jest teraz wyposażony do sprawdzania poprawności tokenów, gdy działa lokalnie, które klient uzyskuje tokenu od punktu końcowego opartej na chmurze.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure portal]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

