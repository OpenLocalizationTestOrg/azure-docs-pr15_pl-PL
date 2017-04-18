<properties
    pageTitle="Zarządzanie rejestracji"
    description="W tym temacie wyjaśniono, jak zarejestrować urządzenia za pomocą powiadomień koncentratory Aby otrzymywać powiadomienia wypychane."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="registration-management"></a>Zarządzanie rejestracji

##<a name="overview"></a>Omówienie

W tym temacie wyjaśniono, jak zarejestrować urządzenia za pomocą powiadomień koncentratory Aby otrzymywać powiadomienia wypychane. Temat zawiera opis rejestracji na wysokim poziomie, a następnie wprowadza dwa główne desenie za przeprowadzenie rejestracji urządzenia: rejestrowanie z urządzenia bezpośrednio do koncentratora powiadomienie i rejestrowanie za pośrednictwem aplikacji wewnętrznej bazy danych. 


##<a name="what-is-device-registration"></a>Co to jest rejestracja urządzenia

Rejestracja urządzenia z koncentratora powiadomienie jest realizowana za pomocą **rejestracji** lub **instalacji**.

#### <a name="registrations"></a>Rejestracji
Rejestracja przypisuje uchwyt platformy powiadomienie usługi (PNS) dla urządzenia znaczników i ewentualnie szablonu. Uchwyt obwodowym układzie Nerwowym może być ChannelURI, urządzenia tokenu lub identyfikator rejestracji GCM. Tagi są używane do kierowania powiadomienia do prawidłowego zestawu uchwyty urządzenia. Aby uzyskać więcej informacji zobacz [routingu i wyrażeń znacznik](notification-hubs-tags-segment-push-message.md). Szablony są używane do wykonania na rejestracji przekształcenia. Aby uzyskać więcej informacji zobacz [Szablony](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Instalacje
Instalacja jest rozszerzonych właściwości pokrewnych rejestracji zawierającego zbiór push. Jest to najnowsze i najlepsze podejście do rejestrowania urządzenia. Jednak jest nie obsługiwane przez po stronie klienta .NET SDK ([SDK Centrum powiadomień dla operacji wewnętrznej bazy danych](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) na jeszcze.  Oznacza to, jeśli rejestrujesz się z samego urządzenia klienta, należy za pomocą metody [Interfejsu API usługi REST koncentratory powiadomienie](https://msdn.microsoft.com/library/mt621153.aspx) do obsługi instalacji. Jeśli korzystasz z usługi sieci wewnętrznej bazy danych, należy za pomocą [Powiadomień Centrum SDK dla operacji wewnętrznej bazy danych](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Poniżej przedstawiono kluczowe korzyści przy użyciu instalacji:

* Tworzenie lub aktualizowanie instalacja jest w pełni idempotent. Aby można go ponowienia bez wszelkie problemy dotyczące rejestracji zduplikowane.
* Model instalacji ułatwia wykonaj poszczególnych umieszcza - kierowanie konkretnego urządzenia. Znacznik systemu **"$InstallationId: [installationId]"** zostaną automatycznie dodane z każdej rejestracji instalacji podstawie. Dlatego możesz zadzwonić do Wyślij do tego tagu do określonego urządzenia bez konieczności dodatkowy kod.
* Przy użyciu instalacji umożliwia też wykonać aktualizacje częściowe rejestracji. Aktualizacja częściowa instalacji jest wymagany metodą poprawki przy użyciu [poprawki JSON standardowe](https://tools.ietf.org/html/rfc6902). Ta opcja jest przydatna, gdy chcesz zaktualizować znaczniki w rejestracji. Nie musisz rozwijane w dół całej rejestracji i ponownie Wyślij ponownie wszystkie znaczniki poprzedniego.

Instalacja może zawierać następujące właściwości. Aby uzyskać pełną listę instalacji właściwości, zobacz [Utwórz lub Zastąp instalacji z interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/mt621153.aspx) lub [Właściwości instalacji](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) .

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }

 

Należy pamiętać, że rejestracji i instalacji domyślnie nie jest już wygaśnie.

Rejestracji i instalacji musi zawierać prawidłowy uchwyt obwodowym układzie Nerwowym dla każdego urządzenia kanału. Ponieważ uchwyty obwodowym układzie Nerwowym można uzyskać tylko w aplikacji dla klienta na tym urządzeniu, jednego wzorca jest zarejestrować bezpośrednio na tym urządzeniu, przy użyciu aplikacji klienta. Z drugiej strony zagadnienia dotyczące zabezpieczeń i reguły biznesowe dotyczące znaczników może potrwać do zarządzania Rejestracja urządzenia w wewnętrznej aplikacji. 

#### <a name="templates"></a>Szablony

Jeśli chcesz używać [szablonów](notification-hubs-templates-cross-platform-push-messages.md), instalacja urządzenia również naciśnij i przytrzymaj wszystkie szablony skojarzone z tym urządzeniem w formacie JSON formatowanie (zobacz przykładowy powyżej). Nazwy szablonów ułatwiają docelowej różnych szablonów dla tego samego urządzenia.

Zauważ, że każda nazwa szablonu mapy treści szablonu i opcjonalny zestaw znaczników. Ponadto każdej platformy może mieć właściwości dodatkowe szablonu. (Za pomocą WNS) ze Sklepu Windows i Windows Phone 8 (za pomocą MPNS) dodatkowy zestaw nagłówków może być częścią szablonu. W przypadku APN można ustawić właściwość wygaśnięcia stałą lub wyrażenie szablonu. Aby uzyskać pełną listę instalacji właściwości zobacz temat [Tworzenie lub Zastąp instalacji z pozostałych](https://msdn.microsoft.com/library/azure/mt621153.aspx) .

#### <a name="secondary-tiles-for-windows-store-apps"></a>Kafelki pomocnicza dla aplikacji ze Sklepu Windows

Dla aplikacje klienckie ze Sklepu Windows Wysyłanie powiadomienia do pomocniczej Kafelki jest taka sama, jak wysyłanie ich z główną. To jest obsługiwane w przypadku instalacji. Należy zauważyć, że pomocniczej Kafelki różnych ChannelUri, obsługujący przezroczysty zestawu SDK w aplikacji klienta.

Słownik SecondaryTiles korzysta z tym samym TileId, który jest używany do tworzenia obiektu SecondaryTiles w aplikacji ze Sklepu Windows.
Podobnie jak w przypadku podstawowego ChannelUri, ChannelUris pomocniczej kafelków można zmienić w dowolnym momencie. Aby zachować instalacji w Centrum powiadomienie o aktualizacji, urządzenie należy odświeżyć ich bieżącym ChannelUris pomocniczej kafelków.


##<a name="registration-management-from-the-device"></a>Zarządzanie rejestracji z urządzenia

Podczas zarządzania Rejestracja urządzenia w aplikacjach klienckich, wewnętrznej bazy danych odpowiada tylko do wysyłania powiadomień. Aplikacje klienta na bieżąco obwodowym układzie Nerwowym uchwyty i zarejestrować znaczniki. Poniżej przedstawiono tego wzorca.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Urządzenie najpierw pobiera uchwytu obwodowym układzie Nerwowym z obwodowym układzie Nerwowym, a następnie rejestruje bezpośrednio z poziomu Centrum powiadomienie. Po pomyślnym rejestracji wewnętrznej bazy danych aplikacji można wysłać powiadomienie kierowanie tej rejestracji. Aby uzyskać więcej informacji na temat wysyłania powiadomień zobacz [routingu i wyrażeń znacznik](notification-hubs-tags-segment-push-message.md).
Należy zauważyć, że w tym przypadku będzie używany tylko nasłuchują praw dostępu do koncentratorów powiadomienie z urządzenia. Aby uzyskać więcej informacji zobacz [Zabezpieczenia](notification-hubs-push-notification-security.md).

Rejestrowanie z urządzenia jest najprostszym metody, ale ma kilka wad.
Pierwszy wadą jest to, że aplikacji klienckiej tylko można zaktualizować jego znaczniki, gdy aplikacja jest aktywna. Na przykład jeśli użytkownik ma dwa urządzenia zarejestrować znaczniki związane z zespołów sport, po pierwszym urządzeniu rejestruje dodatkowe znacznik (na przykład Seahawks), drugie urządzenie nie otrzymasz powiadomienia o Seahawks do momentu aplikację na drugim urządzeniu jest wykonywane po raz drugi. Ogólnie gdy znaczniki dotyczy wielu urządzeń, zarządzanie znaczniki z wewnętrznej bazy danych jest pożądane opcji.
Druga zwrot Zarządzanie rejestracji z aplikacji klienckiej jest, ponieważ może być zaatakowana przez hakerów aplikacje, zabezpieczenie przeprowadzenie rejestracji konkretne znaczniki wymaga dodatkowych care zgodnie z opisem w sekcji "zabezpieczenia na poziomie znacznika."



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Przykładowy kod zarejestrować za pomocą koncentratora powiadomienie z urządzenia w instalacji 

W tym czasie to jest obsługiwane tylko za pomocą [Interfejsu API usługi REST koncentratory powiadomienie](https://msdn.microsoft.com/library/mt621153.aspx).

Umożliwia także metodzie poprawki przy użyciu [poprawki JSON standardowe](https://tools.ietf.org/html/rfc6902) o aktualizację instalacji.

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }

   

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Przykładowy kod zarejestrować za pomocą koncentratora powiadomienie z urządzenia za pomocą rejestracji


Te metody Tworzenie lub aktualizowanie rejestracji urządzenia, na którym są nazywane. Oznacza to, że aby zaktualizować uchwytu lub znaczniki, należy zastąpić całą rejestracji. Należy pamiętać, że rejestracji przejściowych, dlatego należy zawsze masz zaufanego magazynu przy użyciu tagów bieżącego, których potrzebuje konkretnego urządzenia.


    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }
     
    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Zarządzanie rejestracji z wewnętrznej bazy danych

Zarządzanie rejestracji z wewnętrznej bazy danych wymaga, pisanie kodu dodatkowe. Aplikację z urządzenia należy podać zaktualizowane obwodowym układzie Nerwowym obsługiwać do wewnętrznej bazy danych każdym uruchomieniu aplikacji (wraz z znaczniki i szablony) i wewnętrznej bazy danych należy zaktualizować ten uchwyt koncentratora powiadomienie. Poniżej przedstawiono tego projektu.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Możliwość modyfikowania tagów do rejestracji, nawet jeśli odpowiedniej aplikacji na urządzeniu jest nieaktywne i uwierzytelniania aplikacji klienckiej przed dodaniem znacznika do jego rejestracji między innymi następujące korzyści zarządzania rejestracji od wewnętrznej bazy danych.


#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Przykładowy kod zarejestrować za pomocą koncentratora powiadomienie z wewnętrznej bazy danych za pomocą instalacji

Urządzenia klienta nadal otrzymuje jego uchwyt obwodowym układzie Nerwowym i właściwości odpowiednią instalację jako przed i połączeń niestandardowego interfejsu API do wewnętrznej bazy danych, które można wykonać rejestracji Autoryzuj znaczniki itp. Wewnętrznej bazy danych można wykorzystać [SDK Centrum powiadomień dla operacji wewnętrznej bazy danych](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Umożliwia także metodzie poprawki przy użyciu [poprawki JSON standardowe](https://tools.ietf.org/html/rfc6902) o aktualizację instalacji.
 

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Przykładowy kod zarejestrować za pomocą koncentratora powiadomienie z urządzenia za pomocą identyfikatora rejestracji

Podstawowe operacje CRUDS rejestracji można wykonywać z Twojej aplikacji wewnętrznej bazy danych. Na przykład:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");
            
    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


Wewnętrznej bazy danych musi obsługiwać współbieżności między aktualizacjami rejestracji. Usługa Bus oferuje sterowania optymistyczny współbieżności zarządzania rejestracji. Na poziomie HTTP to jest zaimplementowana przy użyciu ETag na operacji zarządzania rejestracji. Ta funkcja służy przezroczysty SDKs firmy Microsoft, które zostać zgłoszony wyjątek odrzucony aktualizację współbieżności powodów. Wewnętrznej bazy danych aplikacji jest odpowiedzialny za obsługę tych wyjątków i ponawianie próby aktualizacji w razie potrzeby.