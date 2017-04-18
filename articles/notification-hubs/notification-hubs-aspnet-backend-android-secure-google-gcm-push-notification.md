<properties
    pageTitle="Wysyłanie powiadomienia wypychane zabezpieczeń z koncentratorów Azure powiadomień"
    description="Dowiedz się, jak wysyłać powiadomienia wypychane bezpiecznego do Android aplikacji z platformy Azure. Przykłady kodu napisanego w Java i C#."
    documentationCenter="android"
    keywords="powiadomienia push, powiadomienia push push wiadomości, powiadomień wypychanych android"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

#<a name="sending-secure-push-notifications-with-azure-notification-hubs"></a>Wysyłanie powiadomienia wypychane zabezpieczeń z koncentratorów Azure powiadomień

> [AZURE.SELECTOR]
- [Uniwersalny systemu Windows](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)

##<a name="overview"></a>Omówienie

> [AZURE.IMPORTANT] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Obsługa powiadomień wypychanych w Microsoft Azure umożliwia dostęp do wypychanych prostych w użyciu, wiele platform, skalowanej infrastruktury wiadomości, które znacznie upraszcza implementację powiadomień wypychanych dla zarówno dla klientów indywidualnych, jak i przedsiębiorstwa aplikacji dla urządzeń przenośnych platform.

Ze względu na prawne lub ograniczeń dotyczących zabezpieczeń, czasami aplikacji może być obejmować coś powiadomienie, które nie będą przesyłane przez infrastruktury powiadomień wypychanych standardowy. Ten samouczek opisano, jak można uzyskać, używając takich samych warunkach, wysyłając poufnych informacji za pomocą bezpieczne, uwierzytelnionego połączenia między urządzeniem Android klienta i wewnętrznej bazy danych aplikacji.

Na wysokim poziomie przepływ jest w następujący sposób:

1. Aplikacja wewnętrznej:
    - Sklepy bezpiecznego ładunek wewnętrznej bazy danych.
    - Wysyła identyfikator to powiadomienie na urządzeniu Android (nie bezpiecznego informacje są wysyłane).
2. Aplikacja na urządzeniu, gdy odbiera powiadomienie:
    - Urządzenie z systemem Android kontaktów wewnętrznej żąda bezpiecznego ładunku.
    - Aplikacja ładunku jako powiadomienie na tym urządzeniu mogą być wyświetlane.

Należy pamiętać, że w poprzednim przepływu (oraz w tym samouczku) przyjęto założenie, że urządzenie przechowuje token uwierzytelniania w magazynu lokalnego, po zalogowaniu się użytkownika. Gwarantuje całkowicie płynną obsługę, zgodnie z urządzenia można pobrać ładunku bezpieczne powiadomień przy użyciu tego tokenu. Jeśli aplikacji nie są zapisywane tokenów uwierzytelniania na urządzeniu lub tych tokenów można wygasła, aplikację urządzenia po otrzymaniu powiadomień wypychanych powinien być wyświetlany ogólnego powiadomienie o monitowania użytkownika Uruchom aplikację. Aplikacja następnie uwierzytelnia użytkownika i przedstawia ładunek powiadomienie.

Ten samouczek pokazano, jak do wysyłania powiadomień wypychanych bezpiecznego. Tworzy go na samouczek [Powiadom użytkowników](notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md) , należy wykonać czynności opisane w tym samouczku najpierw Jeśli jeszcze tego nie zrobiono.

> [AZURE.NOTE] Tego samouczka przyjęto założenie, że masz została utworzona i skonfigurowana do Centrum powiadomienie, zgodnie z opisem w [Wprowadzenie powiadomienie koncentratory (system Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-android-project"></a>Modyfikowanie Android projektu

Teraz, gdy zmodyfikowany Twojej aplikacji wewnętrznej wysłać tylko *identyfikator* powiadomienia wypychane, musisz zmienić Android aplikacji do obsługi powiadomienie i oddzwonić do wewnętrznej do pobierania bezpiecznej wiadomości mają być wyświetlane.
Aby osiągnąć ten cel, należy upewnić się, że Android aplikacji wie, jak należy uwierzytelniony do wewnętrznej po odebraniu powiadomienia wypychane.

Firma Microsoft będzie teraz zmodyfikować przepływu *logowania* w celu zapisania wartość nagłówka uwierzytelniania w preferencjach udostępnionych aplikacji. Mechanizmy analogiczny można przechowywać dowolne tokenu uwierzytelniania (np. OAuth tokeny), który aplikacji należy użyć bez konieczności poświadczeń użytkownika.

1. W projekcie aplikacji systemu Android Dodaj następujące stałych w górnej części klasy **MainActivity** :

        public static final String NOTIFY_USERS_PROPERTIES = "NotifyUsersProperties";
        public static final String AUTHORIZATION_HEADER_PROPERTY = "AuthorizationHeader";

2. Nadal w klasie **MainActivity** aktualizowanie `getAuthorizationHeader()` metody zawiera następujący kod:

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);

            SharedPreferences sp = getSharedPreferences(NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            sp.edit().putString(AUTHORIZATION_HEADER_PROPERTY, basicAuthHeader).commit();

            return basicAuthHeader;
        }

3. Dodaj następujący `import` instrukcji u góry pliku **MainActivity** :

        import android.content.SharedPreferences;

Teraz zostanie zmieniona odwołującą nosi nazwę po odebraniu powiadomienia.

4. W **MyHandler** klasy Zmień `OnReceive()` sposobem zawierają:

        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String secureMessageId = bundle.getString("secureId");
            retrieveNotification(secureMessageId);
        }

5. Następnie dodaj `retrieveNotification()` metody, zamieniając symbol zastępczy `{back-end endpoint}` z punktem końcowym wewnętrznej uzyskane podczas wdrażania usługi wewnętrznej:

        private void retrieveNotification(final String secureMessageId) {
            SharedPreferences sp = ctx.getSharedPreferences(MainActivity.NOTIFY_USERS_PROPERTIES, Context.MODE_PRIVATE);
            final String authorizationHeader = sp.getString(MainActivity.AUTHORIZATION_HEADER_PROPERTY, null);

            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        HttpUriRequest request = new HttpGet("{back-end endpoint}/api/notifications/"+secureMessageId);
                        request.addHeader("Authorization", "Basic "+authorizationHeader);
                        HttpResponse response = new DefaultHttpClient().execute(request);
                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            Log.e("MainActivity", "Error retrieving secure notification" + response.getStatusLine().getStatusCode());
                            throw new RuntimeException("Error retrieving secure notification");
                        }
                        String secureNotificationJSON = EntityUtils.toString(response.getEntity());
                        JSONObject secureNotification = new JSONObject(secureNotificationJSON);
                        sendNotification(secureNotification.getString("Payload"));
                    } catch (Exception e) {
                        Log.e("MainActivity", "Failed to retrieve secure notification - " + e.getMessage());
                        return e;
                    }
                    return null;
                }
            }.execute(null, null, null);
        }


Ta metoda wymaga usługi aplikacji wewnętrznej do pobierania zawartości powiadomień przy użyciu poświadczeń przechowywanych w preferencjach udostępnionych i wyświetla go jako normalny powiadomienie. Powiadomienie wygląda użytkownikowi aplikacji dokładnie tak samo jak inne powiadomienia push.

Należy zauważyć, że zaleca się obsługiwanie przypadkach brak właściwości nagłówka uwierzytelniania lub odrzucenia przy wewnętrznej. Określone obsługi tych przypadkach zależy od głównie środowiska użytkownika docelowego. Jedną z opcji są wyświetlane powiadomienie z ogólnego monit dla użytkownika do uwierzytelnienia do pobierania rzeczywisty powiadomienie.

## <a name="run-the-application"></a>Uruchamianie aplikacji

Aby uruchomić aplikację, wykonaj następujące czynności:

1. Upewnij się, że **AppBackend** zostanie wdrożony w Azure. Jeśli używasz programu Visual Studio, uruchom aplikację interfejs API sieci Web **AppBackend** . Zostanie wyświetlona strona sieci web programu ASP.NET.

2. W Zaćmienie Uruchom aplikację na urządzeniu Android fizycznie lub emulatorze.

3. W aplikacji Android interfejsu użytkownika wprowadź nazwę użytkownika i hasło. Może to być dowolny ciąg, ale muszą być identyczne.

4. W aplikacji Android interfejsu użytkownika kliknij przycisk **Zaloguj**. Następnie kliknij przycisk **Wyślij push**.
