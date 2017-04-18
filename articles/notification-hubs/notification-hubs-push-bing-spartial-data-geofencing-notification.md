<properties
    pageTitle="Powiadomienia wypychane ogrodzone Geo koncentratory powiadomienie Azure i Bing przestrzenna danych | Microsoft Azure"
    description="W tym samouczku dowiesz się, jak to zrobić powiadomienia wypychane opartych na lokalizacji koncentratory powiadomienie Azure i Bing przestrzenna danych."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="powiadomienia push, powiadomienia wypychane"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Powiadomienia wypychane ogrodzone Geo koncentratory powiadomienie Azure i danych przestrzenna Bing
 
 > [AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).

W tym samouczku dowiesz się, jak to zrobić powiadomienia wypychane opartych na lokalizacji koncentratory powiadomienie Azure i danych przestrzenna Bing, osobie z aplikacji uniwersalny platformy systemu Windows.

##<a name="prerequisites"></a>Wymagania wstępne
Najpierw i wszystkim należy upewnić się, że wszystkie oprogramowania i usług wymagania wstępne:

* [Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) lub nowszym ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) wykonasz także). 
* Najnowsza wersja [Azure SDK](https://azure.microsoft.com/downloads/). 
* [Centrum deweloperów mapy Bing, konta](https://www.bingmapsportal.com/) (można utworzyć jedną bezpłatnie i kojarzenie go za pomocą konta Microsoft). 

##<a name="getting-started"></a>Wprowadzenie

Zacznijmy od utworzenia projektu. W programie Visual Studio rozpocząć nowy projekt typu **Pustej aplikacji (Windows uniwersalny)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Po zakończeniu tworzenia projektów powinien mieć wiązki dla samej aplikacji. Teraz skonfiguruj wszystko infrastruktury geo ogrodzenia. Firma Microsoft będą za pomocą usługi Bing tak, nie istnieje publicznej końcowy interfejsu API usługi REST, umożliwiająca abyśmy kwerendy ramki określonej lokalizacji:

    http://spatial.virtualearth.net/REST/v1/data/
    
Należy określić następujące parametry, aby działała:

* **Identyfikator źródła danych** , a **Nazwa źródła danych** — interfejsu API mapy Bing, źródła danych zawierają różne bucketed metadane, takie jak godzinach pracy i lokalizacji. Więcej informacji o tym miejscu. 
* **Nazwa obiektu** — obiekt, do którego chcesz użyć jako punkt odniesienia dla powiadomienia. 
* **Klucz interfejsu API mapy Bing** — jest klucz, który został pobrany wcześniej, podczas tworzenia konta Centrum deweloperów usługi Bing.
 
Załóżmy zrobić poszerzony w ustawieniach dla każdego z powyższych elementów.

##<a name="setting-up-the-data-source"></a>Aby skonfigurować źródła danych

Można to zrobić w Centrum deweloperów mapy Bing. Po prostu kliknij **źródeł danych** na górnym pasku nawigacyjnym i wybierz pozycję **Zarządzaj źródłami danych**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Jeśli nie masz doświadczenia w interfejsu API mapy Bing przed, prawdopodobnie nie można wszystkie źródła danych istnieją, więc można utworzyć nową, klikając pozycję przekazania danych do źródła danych. Upewnij się, że Wypełnij wszystkie wymagane pola:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Być może zastanawiasz się — co to jest plik danych i co powinien być przekazywany? Na potrzeby tego testu firma Microsoft korzysta tylko przykładowe oparte na potoku ramki obszaru waterfront San Francisco:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
Powyższe reprezentuje tego obiektu:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Po prostu skopiować i wkleić powyższego ciągu do nowego pliku i zapisać go jako **NotificationHubsGeofence.pipe**i przekazać go w Centrum deweloperów usługi Bing.

>[AZURE.NOTE]Może zostać wyświetlony monit określ nowy klucz dla **klucza głównego** , który różni się od **Klucza kwerendy**. Po prostu utworzyć nowy klucz za pomocą pulpitu nawigacyjnego i odświeżeniu strony przekazywania źródła danych.

Po przekazać plik danych, będzie konieczne upewnij się, że opublikować źródła danych. 

Przejdź do pozycji **Zarządzaj źródłami danych**, tak jak to zostało pracujemy nad, Znajdź źródło danych na liście i kliknij pozycję **Publikuj** w kolumnie **Akcje** . W nieco, powinny zostać wyświetlone źródło danych na karcie **Opublikowanych źródeł danych** :

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Kliknięcie przycisku **Edytuj**, będą mogli widzieć na pierwszy rzut jakich lokalizacjach możemy wprowadzić w niej:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

W tym momencie portalu nie zostać pokazany granic geofence, że utworzonych — wszystko, co jest potrzebna jest potwierdzenie, że określony znajduje się w pobliżu prawego.

Teraz masz wszystkie wymagania dla źródła danych. Aby uzyskać szczegółowe informacje na adresie URL żądania połączenia interfejsu API w Centrum deweloperów mapy Bing, kliknij pozycję **źródła danych** i wybierz **Źródło danych**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

**Adres URL kwerendy** jest jesteśmy po tutaj. To jest punkt końcowy, z którym firma Microsoft może wykonywać zapytania, aby sprawdzić, czy urządzenie jest obecnie w obrębie lokalizacji lub nie. Aby wykonać ten test, po prostu trzeba wykonać połączenia GET przed adres URL kwerendy z następującymi parametrami dołączana:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

W ten sposób wpisujesz elementu docelowego punktu uzyskujące z urządzenia i mapy Bing będzie automatycznie wykonywać obliczenia, aby sprawdzić, czy znajduje się w geofence. Po wykonaniu żądania za pośrednictwem przeglądarki (lub zwinięcie) zostanie wyświetlony standardowy odpowiedź JSON:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Odpowiedź się dzieje, gdy punkt faktycznie znajduje się w obrębie wyznaczonych. Jeśli nie jest dostępne, zostanie wyświetlony kolorem Opróżnij **wyniki** :

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##<a name="setting-up-the-uwp-application"></a>Konfigurowanie aplikacji UWP

Teraz gdy mamy już gotowy źródła danych, firma Microsoft może uruchomione na pasku aplikacji UWP, które było wcześniej załadować.

Najpierw i wszystkim możemy należy włączyć usługi lokalizacji dla naszych aplikacji. Aby to zrobić, kliknij dwukrotnie na `Package.appxmanifest` plików w **Eksploratorze rozwiązań**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Na karcie właściwości pakietu, który właśnie otwarto kliknij na **możliwości** i upewnij się, że wybierz **lokalizację**:

![](./media/notification-hubs-geofence/vs-package-location.png)

Podaną możliwości lokalizacji Utwórz nowy folder w rozwiązaniu o nazwie `Core`i dodać nowy plik w nim o nazwie `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

`LocationHelper` Na tym etapie samej klasy jest dość podstawowe — wszystko działa to zezwalają na uzyskanie lokalizacji użytkownika za pomocą interfejsu API systemu:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Więcej informacji o wprowadzenie lokalizacji użytkownika w aplikacjach UWP w oficjalnym [dokumencie w witrynie MSDN](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

Aby sprawdzić, czy rzeczywiście działa nabycia lokalizacji, otwórz kod części strony głównej (`MainPage.xaml.cs`). Tworzenie nowych zdarzeń dla `Loaded` zdarzenia w `MainPage` konstruktora:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Stosowania obsługi zdarzeń jest następująca:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Zwróć uwagę, możemy zadeklarowane obsługi jako asynchroniczne ponieważ `GetCurrentLocation` jest awaitable i w związku z tym wymaga do wykonania w kontekście asynchroniczne. Ponadto ponieważ w niektórych okolicznościach firma Microsoft może z lokalizacją null (przykład lokalizacji, które usługi są wyłączone lub aplikacja została odmowa uprawnienia do lokalizacji programu access), należy upewnij się, że jest prawidłowo obsługiwane ze znacznikiem wyboru zawiera wartości null.

Uruchom aplikację. Upewnij się, że zezwalasz na dostęp lokalizacji:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Raz uruchomiony aplikacji należy mogli widzieć współrzędnych w oknie **dane wyjściowe** :

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Teraz znasz lokalizacji nabycia działa — i zachęcamy usunąć obsługi zdarzeń test dla załadowane, ponieważ firma Microsoft nie będzie można go używać już.

Następnym krokiem jest przechwytywanie zmiany lokalizacji. Do tego Przyjrzyjmy powrót do `LocationHelper` klasy i dodawanie zdarzeń dla `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

W oknie **dane wyjściowe** wykonania przedstawia współrzędnych lokalizacji:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##<a name="setting-up-the-backend"></a>Aby skonfigurować wewnętrznej bazy danych

Pobierz [Próbki wewnętrznej bazy danych programu .NET z GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). Po zakończeniu pobierania pliku otworzyć `NotifyUsers` folder, a następnie — `NotifyUsers.sln` pliku.

Ustawianie `AppBackend` projektu jako **Projekt startowy** i uruchomić je.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Projekt jest już skonfigurowany do wysyłania powiadomień wypychanych do urządzenia, aby będą potrzebne do wykonywania operacji tylko dwa — zamieniać poprawnego połączenia ciągu dla Centrum powiadomień oraz dodawania identyfikatorów obramowanie, aby wysłać powiadomienie tylko wtedy, gdy użytkownik znajduje się w geofence.

Aby skonfigurować parametry połączenia w `Models` Otwórz folder `Notifications.cs`. `NotificationHubClient.CreateClientFromConnectionString` Funkcja powinien zawierać informacje o Twoim Centrum powiadomienie pobranego w [Azure Portal](https://portal.azure.com) (wygląd wewnątrz karta **Zasady dostępu** w **ustawieniach**). Zapisz plik zaktualizowaną konfigurację.

Teraz trzeba utworzyć model wynik interfejsu API mapy Bing. Aby to zrobić, najłatwiej jest, kliknij prawym przyciskiem myszy `Models` folderu, **Dodaj** > **zajęć**. Nadaj mu nazwę `GeofenceBoundary.cs`. Po zakończeniu skopiowania JSON odpowiedź interfejsu API wspomniano w pierwszej sekcji i użyj programu Visual Studio **Edytowanie** > **Wklej specjalnie** > **JSON Wklej jako klasy**. 

Dzięki temu firma Microsoft upewnij się, że obiekt będzie można rozszeregować dokładnie tak, jak planowano. Wynikowa zestawu klasy powinien być podobny do tego:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Następnie otwórz `Controllers`  >  `NotificationsController.cs`. Trzeba dostosować a połączenie wpis na koncie docelowej długości i szerokości geograficznych. W tym, po prostu dodać dwa ciągi w podpisie funkcji — `latitude` i `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Tworzenie nowej klasy w ramach projektu o nazwie `ApiHelper.cs` — użyjemy nawiązywania połączenia z witryny Bing — Aby sprawdzić punktu przecięcia obramowanie. Implementowanie `IsPointWithinBounds` funkcji, tak jak poniżej:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Upewnij się zastąpić punkt końcowy interfejsu API z adresem URL kwerendy, którą wcześniej pochodzi z Centrum deweloperów usługi Bing (taki sam dotyczy klucz interfejsu API). 

W przypadku wyniki kwerendy, które oznacza, że określony punkt wewnątrz granic geofence, więc zwróconych `true`. Jeśli istnieją żadne wyniki, Bing jest Przekaż nam że punktu jest spoza przedziału odnośnika zwróconych `false`.

W programie `NotificationsController.cs`, tworzenie wyboru bezpośrednio przed instrukcja switch:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

W ten sposób tylko są wysyłane powiadomienie, gdy punkt znajduje się wewnątrz granic.

##<a name="testing-push-notifications-in-the-uwp-app"></a>Testowanie powiadomień wypychanych w aplikacji UWP

Rozważmy ponownie, aby aplikacja UWP, możemy teraz powinno być możliwe do testowania powiadomienia. W ramach `LocationHelper` klasy, należy utworzyć nową funkcję — `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Zamiana `POST_URL` do lokalizacji, w aplikacji sieci web wdrożonym, utworzony w poprzedniej sekcji. Teraz możesz przeprowadzić lokalnie, ale podczas pracy nad wdrażania publicznej wersji, należy ją hostować z zewnętrznego dostawcy.

Przejdźmy teraz upewnij się, że możemy zarejestrować aplikacji UWP dla powiadomienia wypychane. W programie Visual Studio, kliknij pozycję nad **projektem** > **Przechowywanie** > **Kojarzenie aplikacji ze sklepu**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Po zalogowaniu się do Twojego konta dewelopera, upewnij się, wybierz pozycję istniejącej aplikacji lub Utwórz nowy i skojarzyć pakietu. 

Przejdź do Centrum deweloperów i Otwórz aplikację, która została właśnie utworzona. Kliknij pozycję **usługi** > **Powiadomienia Push** > **witryny usługi Live**.

![](./media/notification-hubs-geofence/ms-live-services.png)

W witrynie weź pod uwagę **Hasła aplikacji** i **SID pakiet**. Konieczne będzie zarówno w Portal Azure — Otwórz Centrum usługi powiadomień, kliknij pozycję **Ustawienia** > **Powiadamianie** > **Systemu Windows (WNS)** i wypełnij wymagane pola.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Wybierz polecenie **Zapisz**.

Kliknij prawym przyciskiem myszy **odwołania** w **Eksploratorze rozwiązań** i wybierz pozycję **Zarządzaj NuGet pakietów**. Będzie trzeba dodać odwołanie do **firmy Microsoft Azure usługi Bus zarządzane biblioteki** — można wyszukiwać `WindowsAzure.Messaging.Managed` i dodać je do projektu.

![](./media/notification-hubs-geofence/vs-nuget.png)

Podczas testowania, możemy utworzyć `MainPage_Loaded` ponownie program obsługi zdarzeń i Dodaj do niego poniższym przykładzie:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Powyższe rejestruje aplikację z poziomu Centrum powiadomienie. Jest gotowa do wysłania! 

W `LocationHelper`, wewnątrz `Geolocator_PositionChanged` obsługi, możesz dodać fragment kodu test, który wymusza umieści lokalizacji w geofence:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Ponieważ firma Microsoft nie jest przesyłany rzeczywistą współrzędnych (które obecnie nie mogą być w granicach) i używają test wstępnie zdefiniowanych wartości widzimy powiadomienie wyświetlane na aktualizację:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##<a name="whats-next"></a>Co to jest dalej?

Istnieje kilka czynności, które należy wykonać oprócz powyżej, aby upewnić się, że rozwiązaniem jest zawsze gotowe do produkcji.

Najpierw i wszystkim może być konieczne upewnij się, że geofences są dynamiczne. Aby móc przekazywać nowe ograniczenia w istniejącym źródle danych wymaga niektórych dodatkowej pracy z interfejsem API usługi Bing. Aby uzyskać więcej informacji na ten temat, zapoznaj się z [dokumentacją interfejsu API usługi Bing przestrzenna danych usług](https://msdn.microsoft.com/library/ff701734.aspx) .

Drugi jak pracujesz, aby upewnić się, czy dostawa została wykonana do innych uczestników w prawo, może być celem za pośrednictwem [znakowania](notification-hubs-tags-segment-push-message.md).

Rozwiązanie, jak pokazano powyżej opisuje scenariusz może być szeroką gamę platformy docelowe, aby firma Microsoft nie ograniczone geofencing na możliwości specyficzne dla systemu. Który wspomniano, Universal platformy Windows oferuje możliwości wykrywanie [geofences prawej części — gotowych do](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).

Aby uzyskać więcej informacji dotyczących możliwości koncentratory powiadomienie, zapoznaj się z naszą [dokumentację portalu](https://azure.microsoft.com/documentation/services/notification-hubs/).
