<properties 
    pageTitle="Jak utworzyć aplikację sieci Web przy użyciu Redis pamięć podręczną | Microsoft Azure" 
    description="Dowiedz się, jak utworzyć aplikację sieci Web przy użyciu Redis pamięci podręcznej" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/11/2016" 
    ms.author="sdanie"/>

# <a name="how-to-create-a-web-app-with-redis-cache"></a>Jak utworzyć aplikację sieci Web przy użyciu Redis pamięci podręcznej

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [PROGRAMU ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Ten samouczek pokazano, jak tworzyć i wdrożyć aplikację sieci web programu ASP.NET w aplikacji sieci web w usłudze Azure aplikacji przy użyciu programu Visual Studio 2015 r. Przykładowa aplikacja wyświetla listę statystyk zespołu z bazy danych i przedstawiono różne sposoby Azure Redis w pamięci podręcznej umożliwia przechowywanie i pobieranie danych z pamięci podręcznej. Po zakończeniu samouczka masz żywa aplikacji sieci web odczytuje i zapisuje z bazą danych, zoptymalizowane z pamięci podręcznej Azure Redis i obsługiwany w Azure.

Opisano następujące zagadnienia:

-   Jak utworzyć aplikację sieci web programu ASP.NET MVC 5 w programie Visual Studio.
-   Jak uzyskać dostęp do danych z bazy danych przy użyciu struktury obiektu.
-   Jak zwiększyć przepustowość i zmniejsz obciążenie bazy danych, przechowywanie i pobieranie danych przy użyciu Azure Redis w pamięci podręcznej.
-   Jak używać Redis sortowane zestawu do pobierania górny 5 członkom zespołu.
-   Jak zainicjować Azure zasobów dla aplikacji przy użyciu szablonu Menedżera zasobów.
-   Jak opublikować aplikację Azure za pomocą programu Visual Studio.

## <a name="prerequisites"></a>Wymagania wstępne

Do zakończenia tego samouczka, musisz mieć następujące wymagania wstępne.

-   [Konto Azure](#azure-account)
-   [Visual Studio 2015 z Azure SDK dla środowiska .NET](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Konto Azure

Potrzebne jest konto Azure do zakończenia tego samouczka. Można:

* [Otwórz konto Azure bezpłatnie](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Uzyskaj środków używane wypróbować płatnych usług Azure. Nawet w przypadku, gdy środki są używane w górę, możesz zachować konta i za pomocą bezpłatnej usługi Azure i funkcje.
* [Aktywowanie Visual Studio subskrybentów korzyści](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Twoja subskrypcja MSDN umożliwia środków co miesiąc, używanego do płatnej usługi Azure.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>Visual Studio 2015 z Azure SDK dla środowiska .NET

Samouczek jest napisana dla programu Visual Studio 2015 z [SDK Azure dla środowiska .NET](../dotnet-sdk.md) 2.8.2 lub nowszej. [Pobierz najnowszą SDK Azure Visual Studio 2015 r. w tym miejscu](http://go.microsoft.com/fwlink/?linkid=518003). Jeśli jeszcze nie masz programu Visual Studio zostanie automatycznie zainstalowana z zestawu SDK.

Jeśli masz Visual Studio 2013, możesz [pobrać najnowszą SDK Azure Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Niektóre ekrany może wyglądać inaczej niż ilustracje przedstawiona w tym samouczku.

>[AZURE.NOTE] W zależności od tego, ile zależności SDK jest już na komputerze instalowania zestawu SDK może zająć dużo czasu, na kilka minut pół godziny lub więcej.

## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

1. Otwórz program Visual Studio i kliknij **plik**, **Nowy** **Projekt**.

2. Rozwiń węzeł **Visual C#** na liście **Szablony** , wybierz pozycję **chmurze**, a następnie kliknij **Aplikacji sieci Web programu ASP.NET**. Upewnij się, że jest zaznaczona opcja **.NET Framework 4.5.2** .  Wpisz **ContosoTeamStats** w polu tekstowym **Nazwa** i kliknij **przycisk OK**.
 
    ![Tworzenie projektu][cache-create-project]

3. Wybierz pozycję **MVC** jako typu projektu. Wyczyść pole wyboru **hosta w chmurze** . [Zapewnij Azure zasoby](#provision-the-azure-resources) i [Publikowanie aplikacji Azure](#publish-the-application-to-azure) kolejne kroki po otwarciu w samouczku. Na przykład inicjowania obsługi administracyjnej aplikacji usługi aplikacji sieci web z programu Visual Studio, pozostawiając **hosta w chmurze** zaznaczone pole wyboru zobacz [Wprowadzenie do aplikacji sieci Web w usłudze aplikacji Azure za pomocą programu ASP.NET i Visual Studio](../app-service-web/web-sites-dotnet-get-started.md).

    ![Wybierz szablon projektu][cache-select-template]

4. Kliknij przycisk **OK** , aby utworzyć projekt.

## <a name="create-the-aspnet-mvc-application"></a>Tworzenie aplikacji programu ASP.NET MVC

W tej części samouczka utworzysz podstawowe aplikacji, który odczytuje i wyświetla statystyki zespołu z bazy danych.

-   [Dodawanie modelu](#add-the-model)
-   [Dodawanie administratora](#add-the-controller)
-   [Konfigurowanie widoków](#configure-the-views)

### <a name="add-the-model"></a>Dodawanie modelu

1. Kliknij prawym przyciskiem myszy **modeli** w **Eksploratorze rozwiązań**, a następnie wybierz przycisk **Dodaj**, **zajęć**. 

    ![Dodawanie modelu][cache-model-add-class]

2. Wprowadź `Team` dla klasy Określ nazwę i kliknij przycisk **Dodaj**.

    ![Dodawanie klasy modelu][cache-model-add-class-dialog]

3. Zamienianie `using` instrukcje w górnej części `Team.cs` plik z następujących czynności za pomocą instrukcji.


        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


4. Zamienianie definicji `Team` klasy następujący fragment kodu, zawierający zaktualizowanych `Team` klasy definicja, a także kilka innych Framework jednostki klasy pomocy. Aby uzyskać więcej informacji o kodzie pierwszy podejście do Framework jednostki, który jest używany w tym samouczku wyświetlony [kod najpierw do nowej bazy danych](https://msdn.microsoft.com/data/jj193542).


        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }
        
            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();
        
                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }
        
        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }
        
            public DbSet<Team> Teams { get; set; }
        }
        
        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };
        
                Team.PlayGames(teams);
        
                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }
        
        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


2. W **Eksploratorze rozwiązań**kliknij dwukrotnie **web.config** , aby go otworzyć.

    ![Web.config][cache-web-config]

3.  Dodaj następujący ciąg połączenia w celu `connectionStrings` sekcji. Nazwy parametrów połączenia musi odpowiadać nazwie klasy kontekstu struktury obiektu bazy danych, która jest `TeamContext`.

        <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />


    Po dodaniu, `connectionStrings` sekcji powinien wyglądać w poniższym przykładzie.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"  providerName="System.Data.SqlClient" />
        </connectionStrings>

### <a name="add-the-controller"></a>Dodawanie administratora

1. Naciśnij klawisz **F6** , aby utworzyć projekt. 
2. W **Eksploratorze rozwiązań**kliknij prawym przyciskiem myszy folder **kontrolery** , a następnie wybierz przycisk **Dodaj**, **Kontroler**.

    ![Dodawanie kontrolera][cache-add-controller]

3. Wybierz **Kontroler 5 MVC z widoków, za pomocą struktury obiektu**, a następnie kliknij przycisk **Dodaj**. Jeśli zostanie wyświetlony komunikat o błędzie po kliknięciu przycisku **Dodaj**, upewnij się, najpierw wbudowany projektu.

    ![Dodawanie kontrolera zajęć][cache-add-controller-class]

5. Wybierz **zespołu (ContosoTeamStats.Models)** z listy rozwijanej **Klasa modelu** . Z listy rozwijanej **klasy kontekstu danych** , wybierz pozycję **TeamContext (ContosoTeamStats.Models)** . Typ `TeamsController` w polu tekstowym Nazwa **kontrolera** (Jeśli nie jest automatycznie wypełnione). Kliknij przycisk **Dodaj** , aby utworzyć klasy kontroler i Dodaj domyślne widoki.

    ![Konfigurowanie kontrolera][cache-configure-controller]

4. W **Eksploratorze rozwiązań**rozwiń **Global.asax** i kliknij dwukrotnie **Global.asax.cs** , aby go otworzyć.

    ![Global.asax.CS][cache-global-asax]

5. Dodaj następujące dwa przy użyciu instrukcji u góry pliku w obszarze inne przy użyciu instrukcji.


        using System.Data.Entity;
        using ContosoTeamStats.Models;


6. Dodaj następujący wiersz kodu na końcu `Application_Start` metody.


        Database.SetInitializer<TeamContext>(new TeamInitializer());


7. W **Eksploratorze rozwiązań**, rozwiń `App_Start` i kliknij dwukrotnie `RouteConfig.cs`.

    ![RouteConfig.cs][cache-RouteConfig-cs]

8. Zamienianie `controller = "Home"` poniższy kod w `RegisterRoutes` metody z `controller = "Teams"` jak pokazano w poniższym przykładzie.


        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### <a name="configure-the-views"></a>Konfigurowanie widoków

1. W **Eksploratorze rozwiązań**rozwiń folder **widoków** , a następnie folder **udostępniony** , a następnie kliknij dwukrotnie **_Layout.cshtml**. 

    ![_Layout.cshtml][cache-layout-cshtml]

2. Zmienianie zawartości `title` element i Zamień `My ASP.NET Application` z `Contoso Team Stats` jak pokazano w poniższym przykładzie.


        <title>@ViewBag.Title - Contoso Team Stats</title>


3. W `body` sekcji, najpierw zaktualizować `Html.ActionLink` instrukcji i zamienianie `Application name` z `Contoso Team Stats` i zamienić `Home` z `Teams`.
    -   Przed:`@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
    -   Po:`@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`

    ![Zmiany kodu][cache-layout-cshtml-code]

4. Naciśnij klawisze **Ctrl + F5** , aby utworzyć i uruchomić aplikację. Ta wersja aplikacji odczytuje wyniki bezpośrednio z bazy danych. Uwaga **Utwórz nowy**, **Edytowanie**, **Szczegóły**i **Usuń** akcje, które zostały automatycznie dodane do aplikacji przez scaffold **MVC kontroler 5 z widoków, za pomocą struktury obiektu** . W następnej sekcji samouczka zostanie dodana Redis pamięci podręcznej, aby zoptymalizować dostęp do danych i podaj dodatkowe funkcje aplikacji.

![Aplikacja Starter][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>Konfigurowanie aplikacji umożliwia Redis pamięci podręcznej

W tej części samouczka będzie skonfigurować przykładowej aplikacji do przechowywania i pobierania statystyki zespołu Contoso z pamięci podręcznej Azure Redis wystąpienia przy użyciu klienta pamięci podręcznej [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .

-   [Konfigurowanie aplikacji umożliwia StackExchange.Redis](#configure-the-application-to-use-stackexchangeredis)
-   [Aktualizowanie klasy TeamsController, aby zwracało wyniki z pamięci podręcznej lub bazy danych](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
-   [Tworzenie, edytowanie i usuwanie metody pracy z pamięci podręcznej aktualizacji](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
-   [Aktualizowanie indeksu członkom zespołu widok do pracy z pamięci podręcznej](#update-the-teams-index-view-to-work-with-the-cache)


### <a name="configure-the-application-to-use-stackexchangeredis"></a>Konfigurowanie aplikacji umożliwia StackExchange.Redis

1. Aby skonfigurować aplikację klienta w programie Visual Studio przy użyciu pakietu StackExchange.Redis NuGet, kliknij prawym przyciskiem myszy projektu w **Eksploratorze rozwiązań** i wybierz pozycję **Zarządzaj NuGet pakietów**. 

    ![Zarządzanie pakietów NuGet][redis-cache-manage-nuget-menu]

2. Wpisz **StackExchange.Redis** w polu tekstowym Wyszukaj, wybierz odpowiednią wersję z listy wyników, a następnie kliknij przycisk **Zainstaluj**.

    ![Pakiet StackExchange.Redis NuGet][redis-cache-stack-exchange-nuget]

    Pakiet NuGet do pobrania i dodaje odwołania wymagane zestawu do aplikacji klienta uzyskać dostęp do pamięci podręcznej Azure Redis z klientem StackExchange.Redis pamięci podręcznej. Jeśli wolisz przy użyciu wersji silnych o nazwie **StackExchange.Redis** biblioteki klienta, wybierz pozycję **StackExchange.Redis.StrongName**; w przeciwnym razie wybierz **StackExchange.Redis**.

3. W **Eksploratorze rozwiązań**rozwiń folder **kontrolery** , a następnie kliknij dwukrotnie **TeamsController.cs** , aby go otworzyć.

    ![Kontroler członkom zespołu][cache-teamscontroller]

4. Dodaj następujące dwa przy użyciu instrukcji w celu **TeamsController.cs**.

        using System.Configuration;
        using StackExchange.Redis;

5. Dodaj następujące właściwości dwóch `TeamsController` zajęć.

        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
  
1. Tworzenie pliku na komputerze o nazwie `WebAppPlusCacheAppSecrets.config` i umieścić ją w lokalizacji, która nie będzie sprawdzane przy użyciu kodu źródłowego aplikacji próbki, należy zdecydować to sprawdzić w innym miejscu. W tym przykładzie `AppSettingsSecrets.config` plik znajduje się w `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.

    Edytowanie `WebAppPlusCacheAppSecrets.config` plików i dodaj następujące zagadnienia. Po uruchomieniu aplikacji lokalnie te informacje służy do łączenia się z wystąpieniem Azure Redis w pamięci podręcznej. W dalszej części samouczka możesz dodawać wystąpienie Azure Redis w pamięci podręcznej i aktualizowanie pamięci podręcznej nazwę i hasło. Jeśli nie zamierzasz lokalnie uruchomić aplikację przykładowe można pominąć tworzenia tego pliku i kolejne kroki, które odwołują się do pliku, ponieważ po wdrożeniu Azure program pobiera informacje o połączeniu pamięci podręcznej z ustawieniem aplikacji dla aplikacji sieci Web, a nie z tego pliku. Ponieważ `WebAppPlusCacheAppSecrets.config` nie jest używany do Azure z aplikacją, nie będzie potrzebny chyba że ma być lokalnie uruchomić aplikację.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


2. W **Eksploratorze rozwiązań**kliknij dwukrotnie **web.config** , aby go otworzyć.

    ![Web.config][cache-web-config]

3. Dodaj następujący `file` atrybutów do `appSettings` elementu. Jeśli użyto inną nazwę lub lokalizację, należy zastąpić te wartości dla nich w przykładzie.
    -   Przed:`<appSettings>`
    -   Po:` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`

    Środowisko uruchomieniowe ASP.NET łączy zawartość zewnętrznego pliku z adiustacji w `<appSettings>` elementu. Środowisko uruchomieniowe ignoruje atrybut pliku, jeśli nie można znaleźć określonego pliku. Swojego hasła (parametry połączenia z pamięci podręcznej) nie są uwzględniane kodu źródłowego aplikacji. Po wdrożeniu aplikacji sieci web Azure, `WebAppPlusCacheAppSecrests.config` plik nie będzie używany (który jest, co ma). Istnieje kilka sposobów, aby określić te hasła w Azure, a w tym samouczku są konfigurowane automatycznie dla Ciebie podczas [Zapewnij Azure zasoby](#provision-the-azure-resources) w kolejnych instrukcji krok. Aby uzyskać więcej informacji na temat pracy z hasła Azure zobacz [Najważniejsze wskazówki dotyczące rozmieszczania haseł i inne dane poufne ASP.NET i Azure aplikacji usługi](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).


### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>Aktualizowanie klasy TeamsController, aby zwracało wyniki z pamięci podręcznej lub bazy danych

W tym przykładzie statystyki zespołu mogą być pobierane z bazy danych lub z pamięci podręcznej. Statystyki zespołu są przechowywane w pamięci podręcznej jako seryjnych `List<Team>`, a także jako posortowanego zestawu przy użyciu typów danych Redis. Podczas pobierania elementów z zestawu posortowane, można pobrać niektóre, wszystkie lub kwerendy dla określonych elementów. W tym przykładzie zostanie kwerendę posortowanego zestawu górny sklasyfikowane jako liczby wins 5 członkom zespołu.

>[AZURE.NOTE] Nie jest wymagane do przechowywania statystyki zespołu w różnych formatach w pamięci podręcznej usługi Azure Redis w pamięci podręcznej. Ten samouczek użyto wielu formatów, aby przedstawiają niektóre sposoby i różne typy danych używanego do danych z pamięci podręcznej.



1. Dodaj następujący przy użyciu instrukcji w celu `TeamsController.cs` pliku na początku z innym przy użyciu instrukcji.

        using System.Diagnostics;
        using Newtonsoft.Json;

2. Zamień bieżące `public ActionResult Index()` metody wykonania następujących czynności.


        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;
        
            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;
        
                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;
        
                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }
        
            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();
        
            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;
        
                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;
        
                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;
        
                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }
        
            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();
        
            return View(teams);
        }


3. Dodaj następujące trzy metody `TeamsController` klasy `playGames`, `clearCache`, i `rebuildDB` typów akcji z instrukcji switch dodane w poprzednim wstawkę kodu.

    `PlayGames` Metody aktualizuje statystyki zespołu symulowanie świąteczne gier, zapisuje wyniki do bazy danych i usuwa teraz nieaktualnych danych z pamięci podręcznej.


        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;
    
            Team.PlayGames(teams);
    
            db.SaveChanges();
    
            // Clear any cached results
            ClearCachedTeams();
        }


    `RebuildDB` Metody reinitializes bazy danych za pomocą domyślnego zestawu członkom zespołu, generuje statystyki dla nich i usuwa teraz nieaktualnych danych z pamięci podręcznej.
    
        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


    `ClearCachedTeams` Metoda usuwa żadnej statystyki pamięci podręcznej zespołu z pamięci podręcznej.

    
        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


4. Dodaj następujące cztery metody `TeamsController` klasy różne sposoby pobierania statystyki zespołu z pamięci podręcznej i w bazie danych. Każdej z tych metod zwraca `List<Team>` wyświetlone w widoku.

    `GetFromDB` Metody odczytuje statystyki zespołu z bazy danych.

        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
    
            return results.ToList<Team>();
        }


    `GetFromList` Metody odczytuje statystyki zespołu z pamięci podręcznej jako seryjnych `List<Team>`. W przypadku nie trafienia pamięci podręcznej, statystyki zespołu są odczytać bazy danych, a następnie przechowywanych w pamięci podręcznej do użycia w przyszłości. W tym przykładzie użyto programu szeregowania JSON.NET do szeregować obiekty .NET do i z pamięci podręcznej. Aby uzyskać więcej informacji zobacz [jak pracować z .NET obiektów w Azure Redis pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    `GetFromSortedSet` Metody odczytuje statystyki zespołu z pamięci podręcznej zestawu posortowany. W przypadku nie trafienia pamięci podręcznej, statystyki zespołu są odczytać bazy danych i przechowywane w pamięci podręcznej jako zestaw posortowany.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";
    
                // Read from DB
                teams = GetFromDB();
    
                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    `GetFromSortedSetTop5` Metody odczytuje zespoły 5 pierwszych z zestawu posortowany pamięci podręcznej. Uruchamiania, zaznaczając pole wyboru pamięci podręcznej obecność `teamsSortedSet` klucza. Jeśli ten klucz nie występuje, `GetFromSortedSet` metoda jest wywoływana do odczytu statystyki zespołu i przechowywać je w pamięci podręcznej. Następny zestaw posortowany pamięci podręcznej jest badany pod kątem górny 5 członkom zespołu, które są zwracane.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>Tworzenie, edytowanie i usuwanie metody pracy z pamięci podręcznej aktualizacji

Kod rusztowania, który został utworzony w ramach tego przykładu zawiera metod do dodawania, edytowania i usuwania członkom zespołu. W dowolnym momencie zespołu jest dodane, edytowane lub usuwane, dane w pamięci podręcznej staje się przestarzały. W tej sekcji będą wprowadzane zmiany tych trzech metod, aby wyczyścić pamięci podręcznej członkom zespołu, dzięki czemu pamięci podręcznej nie jest zsynchronizowany z bazą danych.

1. Przejdź do `Create(Team team)` metoda `TeamsController` zajęć. Dodawanie połączenia do `ClearCachedTeams` metody, jak pokazano w poniższym przykładzie.


        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
    
            return View(team);
        }


2. Przejdź do `Edit(Team team)` metoda `TeamsController` zajęć. Dodawanie połączenia do `ClearCachedTeams` metody, jak pokazano w poniższym przykładzie.


        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


3. Przejdź do `DeleteConfirmed(int id)` metoda `TeamsController` zajęć. Dodawanie połączenia do `ClearCachedTeams` metody, jak pokazano w poniższym przykładzie.


        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>Aktualizowanie indeksu członkom zespołu widok do pracy z pamięci podręcznej

1. W **Eksploratorze rozwiązań**rozwiń folder **widoków** , następnie folder **członkom zespołu** , a następnie kliknij dwukrotnie **Index.cshtml**.

    ![Index.cshtml][cache-views-teams-index-cshtml]

2. W górnej części pliku Znajdź następujący element akapitu.

    ![Tabela akcji][cache-teams-index-table]

    To łącze, aby utworzyć nowego zespołu. Zamień elementu akapitu z poniższej tabeli. W tej tabeli znajdują się łącza akcji do tworzenia nowego zespołu, odtwarzanie nowego święta gier, wyczyszczenie pamięci podręcznej, pobieranie zespołów z pamięci podręcznej w wielu formatów, pobieranie zespołów z bazy danych i odbudowywanie bazy danych za pomocą świeży przykładowych danych.


        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


3. Przewiń do końca pliku **Index.cshtml** i Dodaj następujący `tr` element, że jest ostatniego wiersza w tabeli ostatniej w pliku.

        <tr><td colspan="5">@ViewBag.Msg</td></tr>

    Ten wiersz zawiera wartość `ViewBag.Msg` zawiera raport o stanie o bieżącej operacji, które ma wartość po kliknięciu łącza akcji w poprzednim kroku.   

    ![Komunikat o stanie][cache-status-message]

4. Naciśnij klawisz **F6** , aby utworzyć projekt.

## <a name="provision-the-azure-resources"></a>Zapewnij zasoby Azure

Do obsługi aplikacji Azure, należy najpierw umożliwić usług Azure, wymagane przez aplikację. Przykładowa aplikacja w tym samouczku korzysta z następujących usług Azure.

-   Azure Redis pamięci podręcznej
-   Usługa aplikacji Web App
-   Baza danych SQL

Aby wdrożyć usługi te grupy zasobów nowym lub istniejącym wybranych przez użytkownika, kliknij przycisk następujące **Rozmieszczanie Azure** .

[! [Wdroż Azure] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Kliknięcie tego przycisku **Rozmieszczanie Azure** korzysta z szablonu [Szybki Start Azure](https://github.com/Azure/azure-quickstart-templates) [Tworzenie aplikacji sieci Web oraz Redis pamięci podręcznej oraz bazy danych SQL](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) do obsługi administracyjnej tych usług i ustawianie parametrów połączenia z bazą danych SQL i ustawienie parametrów połączenia Azure Redis w pamięci podręcznej aplikacji.

>[AZURE.NOTE] Jeśli nie masz konta usługi Azure, możesz [utworzyć bezpłatne konto Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) na kilka minut.

Kliknięcie przycisku **Rozmieszczanie Azure** przejście do portalu Azure i inicjuje proces tworzenia zasobów opisanych w szablonie.

![Wdrażanie Azure][cache-deploy-to-azure-step-1]

1. Na karta **wdrożenia niestandardowe** wybierz Azure subskrypcję, aby użyć i wybierz istniejącej grupy zasobów lub Utwórz nowy i określić lokalizację grupy zasobów.
2. Na karta **Parametry** Określ nazwę konta administratora (**ADMINISTRATORLOGIN** - nie używaj **Administrator**), hasło logowania administratora (**ADMINISTRATORLOGINPASSWORD**), a nazwa bazy danych (**DATABASENAME**). Inne parametry są skonfigurowane do bezpłatnej usługi aplikacji hostingu planu i dolnym opcji koszt dla bazy danych SQL i Azure Redis pamięci podręcznej, które nie pochodzą z poziomu bezpłatne.
3. Zmienianie innych ustawień, w razie potrzeby lub Zachowaj wartości domyślne, a następnie kliknij **przycisk OK**.


![Wdrażanie Azure][cache-deploy-to-azure-step-2]

1. Kliknij pozycję **Recenzja warunki prawne**.
2. Zapoznaj się z warunkami na karta **zakupu** i kliknij przycisk **Kup**.
3. Aby rozpocząć, inicjowania obsługi administracyjnej zasobów, kliknij przycisk **Utwórz** na karta **wdrożenia niestandardowe** .

Aby wyświetlić postęp wdrożenia, kliknij ikonę powiadomień, a następnie kliknij przycisk **Wdrażanie uruchomione**.

![Wdrażanie rozpoczęte][cache-deployment-started]

Umożliwia wyświetlanie stanu wdrożenia na karta **Microsoft.Template** .

![Wdrażanie Azure][cache-deploy-to-azure-step-3]

Po zakończeniu inicjowania obsługi administracyjnej aplikacji Azure z programu Visual Studio można opublikować.

>[AZURE.NOTE] Na karta **Microsoft.Template** są wyświetlane wszystkie błędy, które mogą wystąpić podczas inicjowania obsługi administracyjnej. Typowe błędy są zbyt wiele serwerów SQL lub zbyt wiele bezpłatnej aplikacji usługi hostingu planów na subskrypcję. Naprawianie błędów i ponownie uruchomić proces klikając **ponownie wdróż** karta **Microsoft.Template** lub przycisku **Rozmieszczanie Azure** w tym samouczku.

## <a name="publish-the-application-to-azure"></a>Publikowanie aplikacji Azure

W tym kroku samouczka możesz opublikować aplikację Azure i uruchom go w chmurze.

1. Kliknij prawym przyciskiem myszy **ContosoTeamStats** projektu w programie Visual Studio, a następnie wybierz pozycję **Publikuj**.

    ![Publikowanie][cache-publish-app]

2. Kliknij pozycję **Microsoft Azure aplikacji usługi**.

    ![Publikowanie][cache-publish-to-app-service]

3. Wybierz subskrypcję, używany przy tworzeniu Azure zasobów, rozwiń grupy zasobów zawierające zasobów, wybierz odpowiednią aplikację sieci Web i kliknij **przycisk OK**. Użycie przycisku **Rozmieszczanie Azure** nazwy aplikacji sieci Web zostanie uruchomiony z **witryny sieci Web** , a po nim kilka dodatkowych znaków.

    ![Wybierz aplikację sieci Web][cache-select-web-app]

4. Kliknij pozycję **Sprawdź poprawność połączenia** , aby zweryfikować ustawienia, a następnie kliknij pozycję **Publikuj**.

    ![Publikowanie][cache-publish]

    Po chwili zostanie ukończona procesu publikowania i zostanie uruchomiona przeglądarka prowadzeniem przykładowej aplikacji. Jeśli zostanie wyświetlony komunikat o błędzie DNS podczas sprawdzania poprawności lub publikowania i obsługi administracyjnej proces Azure zasobów dla aplikacji niedawno została ukończona, poczekaj chwilę i spróbuj ponownie.

    ![Dodane pamięci podręcznej][cache-added-to-application]

W poniższej tabeli opisano każdego łącza akcji z przykładowej aplikacji.

| Akcja                  | Opis                                                                                                                                                      |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Utwórz nowe              | Tworzenie nowego zespołu.                                                                                                                                               |
| Odtwarzanie świąteczne             | Odtwarzanie świąteczne gier, zaktualizować statystykę zespołu i wyczyść dowolne nieaktualne zespołu dane w pamięci podręcznej.                                                                          |
| Wyczyść pamięć podręczną             | Wyczyść statystykę zespołu z pamięci podręcznej.                                                                                                                             |
| Lista z pamięci podręcznej         | Pobieranie statystykę zespołu z pamięci podręcznej. W przypadku nie trafienia pamięci podręcznej, załaduj statystykę z bazy danych i zapisywanie w pamięci podręcznej na przyszłość.                                        |
| Ustawianie posortowany z pamięci podręcznej   | Pobierz statystykę zespołu z pamięci podręcznej, przy użyciu zestawu posortowany. W przypadku nie trafienia pamięci podręcznej, załaduj statystykę z bazy danych i zapisywanie w pamięci podręcznej, przy użyciu zestawu posortowany.  |
| 5 pierwszych członkom zespołu z pamięci podręcznej  | Pobierz górny zespoły 5 z pamięci podręcznej, przy użyciu zestawu posortowany. W przypadku nie trafienia pamięci podręcznej, załaduj statystykę z bazy danych i zapisywanie w pamięci podręcznej, przy użyciu zestawu posortowany. |
| Ładowanie z bazy danych            | Pobieranie statystykę zespołu z bazy danych.                                                                                                                       |
| Odbudowywanie bazy danych              | Odbudowywanie bazy danych i załadować ponownie z przykładowymi danymi zespołu.                                                                                                        |
| Edytowanie / szczegóły / Usuń | Edytowanie zespołu, wyświetlanie szczegółów dla zespołu i usuwanie zespołu.                                                                                                             |


Kliknij akcje i eksperymentować pobieranie danych z różnych źródeł. Nie różnice w czas potrzebny na wykonanie różne sposoby pobierania danych z bazy danych i pamięci podręcznej.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>Usuwanie zasobów, po zakończeniu pracy z aplikacją

Po zakończeniu przykładowej aplikacji samouczka, możesz usunąć Azure zasoby używane w celu zaoszczędzenia zasoby kosztowe i. Jeśli przycisk **Deploy Azure** w sekcji [Zapewnij Azure zasoby](#provision-the-azure-resources) i wszystkie zasoby są zawarte w tej samej grupy zasobów, możesz usunąć je razem w jednej operacji przez usunięcie grupy zasobów.

1. Zaloguj się do [portalu Azure](https://portal.azure.com) i kliknij pozycję **grupy zasobów**.
2. W polu tekstowym **Filtrowanie elementów...** , wpisz nazwę grupy zasobów.
3. Kliknij przycisk **...** po prawej stronie grupy zasobów.
4. Kliknij przycisk **Usuń**.

    ![Usuwanie][cache-delete-resource-group]

5. Wpisz nazwę grupy zasobów i kliknij przycisk **Usuń**.

    ![Potwierdzanie usunięcia][cache-delete-confirm]

Po chwili grupy zasobów i wszystkie jego zawartych zasoby są usuwane.

>[AZURE.IMPORTANT] Należy zauważyć, że oraz jest usunięcie grupy zasobów i trwałe usunięcie grupy zasobów i wszystkie zasoby w nim. Upewnij się, że nie przypadkowo usuniesz grupa zasobów problem lub zasobów. Jeśli utworzono zasobów do obsługi w tym przykładzie wewnątrz istniejącej grupy zasobów, możesz usunąć każdemu zasobowi pojedynczo z ich odpowiedniej karty.

## <a name="run-the-sample-application-on-your-local-machine"></a>Uruchamianie aplikacji przykładowej na komputerze lokalnym

Aby uruchomić aplikację lokalnie na komputerze, należy wystąpienie Azure Redis w pamięci podręcznej, w której mają być buforowane dane. 

-   Jeśli aplikacja Azure zostały opublikowane zgodnie z opisem w poprzedniej sekcji, możesz użyć wystąpienia Azure Redis w pamięci podręcznej, który został zainicjowany podczas tego etapu.
-   Jeśli masz inne istniejące wystąpienie Azure Redis w pamięci podręcznej, możesz go używać, aby uruchomić ten przykład lokalnie.
-   Jeśli chcesz utworzyć wystąpienie Azure Redis w pamięci podręcznej, możesz wykonaj kroki w sekcji [Tworzenie pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Po zaznaczeniu lub utworzone pamięci podręcznej, aby użyć przejdź do pamięci podręcznej w portalu Azure i pobrać [Nazwa hosta](cache-configure.md#properties) i [klawisze dostępu](cache-configure.md#access-keys) dla pamięci podręcznej. Aby uzyskać instrukcje zobacz [Konfigurowanie Redis ustawienia pamięci podręcznej](cache-configure.md#configure-redis-cache-settings).

1. Otwórz `WebAppPlusCacheAppSecrets.config` plik utworzony w kroku [Konfiguruj aplikację do korzystania z pamięci podręcznej Redis](#configure-the-application-to-use-redis-cache) tego samouczka przy użyciu dowolnego edytora.

2. Edytowanie `value` atrybutów i zamienić `MyCache.redis.cache.windows.net` [Nazwa hosta](cache-configure.md#properties) pamięć podręczną i określ albo [klucz podstawowy i pomocniczy](cache-configure.md#access-keys) pamięć podręczną jako hasło.


        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


3. Naciśnij klawisze **Ctrl + F5** , aby uruchomić aplikację.

>[AZURE.NOTE] Należy zauważyć, że ponieważ aplikacji, łącznie z bazą danych jest uruchomiony lokalnie i przechowywane w pamięci podręcznej Redis w Azure, pamięci podręcznej może pojawić się w obszarze przeprowadzanie bazy danych. Aby uzyskać optymalną wydajność w tym samym miejscu należy aplikacji klienckiej i wystąpienie Azure Redis w pamięci podręcznej. 

## <a name="next-steps"></a>Następne kroki

-   Dowiedz się więcej o [Wprowadzenie do programu ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) w witrynie sieci Web programu [ASP.NET](http://asp.net/) .
-   Aby uzyskać więcej przykładów tworzenie aplikacji sieci Web programu ASP.NET w aplikacji usługi, zobacz [Tworzenie i wdrażanie aplikacji sieci web programu ASP.NET w usłudze Azure aplikacji](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) z Połącz [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [Pokaz](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
    -   Aby uzyskać więcej Przewodniki Szybki Start z pokaz HealthClinic.biz zobacz [Azure Deweloper narzędzia Przewodniki Szybki Start](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
-   Dowiedz się, dlaczego [kodu najpierw do nowej bazy danych](https://msdn.microsoft.com/data/jj193542) sposobem Framework jednostki, który jest używany w tym samouczku.
-   Dowiedz się więcej na temat [aplikacji web apps w usłudze Azure aplikacji](../app-service-web/app-service-web-overview.md).
-   Dowiedz się, jak [monitor](cache-how-to-monitor.md) pamięć podręczną w portalu Azure.

-   Zapoznaj się z funkcjami premium Azure Redis w pamięci podręcznej
    -   [Jak skonfigurować pozostawanie na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-persistence.md)
    -   [Jak skonfigurować klaster na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-clustering.md)
    -   [Jak skonfigurować wirtualną sieć obsługę bufor Redis Azure Premium](cache-how-to-premium-vnet.md)
    -   Zobacz [Azure Redis pamięci podręcznej często zadawane pytania](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) , aby uzyskać więcej informacji o rozmiarze, przepustowość i przepustowości z pamięci podręcznej premium.



<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

