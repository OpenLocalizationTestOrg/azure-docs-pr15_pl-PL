<properties
    pageTitle="Wysyłanie powiadomienia wypychane w systemie Android przy użyciu koncentratory powiadomienie Azure | Microsoft Azure"
    description="W tym samouczku dowiesz się, jak za pomocą Azure koncentratory powiadomień wypychanych powiadomień urządzeń z systemem Android."
    services="notification-hubs"
    documentationCenter="android"
    keywords="powiadomienia push, powiadomienia push, powiadomienia wypychane android"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/05/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Wysyłanie powiadomienia wypychane w systemie Android przy użyciu koncentratory powiadomienie Azure

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Omówienie

> [AZURE.IMPORTANT] W tym temacie przedstawiono powiadomienia wypychane z wiadomości chmury Google (GCM). Jeśli korzystasz z usługi Google i Firebase chmury wiadomości (FCM), zobacz [Wysyłanie powiadomień wypychanych i Android przy użyciu koncentratory powiadomienie Azure i FCM](notification-hubs-android-push-notification-google-fcm-get-started.md).

Ten samouczek pokazano, jak używać koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych dla systemu Android aplikacji.
Procedura tworzenia pustej aplikacji Android, który otrzymuje powiadomienia wypychane za pomocą usługi Google Cloud wiadomości (GCM).

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Ukończony kodu dla tego samouczka można pobrać z GitHub [tutaj](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStarted).


##<a name="prerequisites"></a>Wymagania wstępne

> [AZURE.IMPORTANT] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).

Oprócz aktywne konto Azure, wymienionych powyżej ten samouczek wymaga tylko najnowszą wersję pakietu [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797).

Ten samouczek jest wymagane w przypadku innych samouczki koncentratory powiadomienie dotyczące aplikacje.

##<a name="creating-a-project-that-supports-google-cloud-messaging"></a>Tworzenie projektu, który obsługuje Google Cloud wiadomości

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]


##<a name="configure-a-new-notification-hub"></a>Konfigurowanie nowego koncentratora powiadomień


[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. w karta **Ustawienia** wybierz **Usługi powiadomień** , a następnie **Google (GCM)**. Wprowadź klucz interfejsu API i kliknij przycisk **Zapisz**.

&emsp;&emsp;![Powiadomienie Azure koncentratory - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

Twoim Centrum powiadomienie jest skonfigurowany do pracy z GCM, i masz ciągów połączeń zarówno rejestrowania aplikacji do odbierania i wysyłania powiadomień wypychanych.

##<a id="connecting-app"></a>Łączenie aplikacji z Centrum powiadomień

###<a name="create-a-new-android-project"></a>Tworzenie nowego projektu systemu Android

1. W Android Studio rozpocząć nowy projekt Android Studio.

    ![Android Studio — nowego projektu][13]

2. Wybierz wygląd formularza **telefonów i tabletów** i **Minimalna SDK** do pomocy technicznej. Następnie kliknij przycisk **Dalej**.

    ![Android Studio — przepływ pracy Tworzenie projektu][14]

3. Wybierz **Pusty aktywności** głównym działania, kliknij przycisk **Dalej**, a następnie kliknij przycisk **Zakończ**.

###<a name="add-google-play-services-to-the-project"></a>Dodawanie usługi Google Play do projektu

[AZURE.INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

###<a name="adding-azure-notification-hubs-libraries"></a>Dodawanie bibliotek koncentratory powiadomienie Azure


1. W `Build.Gradle` plików **aplikacji**, Dodaj następujące wiersze w sekcji **zależności** .

        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'

2. Dodawanie repozytorium następujący po sekcji **zależności** .

        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>Aktualizowanie AndroidManifest.xml.


1. Do obsługi GCM, możemy wdrożenia usługi odbiornika identyfikator wystąpienia w naszym kod, który jest używany do [uzyskania tokeny rejestracji](https://developers.google.com/cloud-messaging/android/client#sample-register) za pomocą [Interfejsu API identyfikator wystąpienia firmy Google](https://developers.google.com/instance-id/). W tym samouczku możemy Nazwa klasy `MyInstanceIDService`. 
 
    Dodaj następujące definicji usługi do pliku AndroidManifest.xml wewnątrz `<application>` znacznik. Zamienianie `<your package>` symbol zastępczy nazwy pakietu rzeczywisty wyświetlany u góry `AndroidManifest.xml` pliku.

        <service android:name="<your package>.MyInstanceIDService" android:exported="false">
            <intent-filter>
                <action android:name="com.google.android.gms.iid.InstanceID"/>
            </intent-filter>
        </service>


2. Gdy otrzymane naszych tokenu rejestracji GCM z interfejsu API identyfikator wystąpienia użyjemy go zarejestrować za [pomocą Centrum powiadomienie Azure](notification-hubs-push-notification-registration-management.md). Będzie obsługujemy tej rejestracji przy użyciu tła `IntentService` o nazwie `RegistrationIntentService`. Będzie również odpowiedzialny za [odświeżanie naszych tokenu rejestracji GCM](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens)tej usługi.
 
    Dodaj następujące definicji usługi do pliku AndroidManifest.xml wewnątrz `<application>` znacznik. Zamienianie `<your package>` symbol zastępczy nazwy pakietu rzeczywisty wyświetlany u góry `AndroidManifest.xml` pliku. 

        <service
            android:name="<your package>.RegistrationIntentService"
            android:exported="false">
        </service>



3. Firma Microsoft określi także odbiorcy, aby otrzymywać powiadomienia. Dodaj następujące definicje odbiorcy do pliku AndroidManifest.xml wewnątrz `<application>` znacznik. Zamienianie `<your package>` symbol zastępczy nazwy pakietu rzeczywisty wyświetlany u góry `AndroidManifest.xml` pliku.

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>



4. Dodaj następujący niezbędne GCM dotyczące uprawnień poniżej `</application>` znacznik. Upewnij się zamienić `<your package>` z nazwą pakietu wyświetlany u góry `AndroidManifest.xml` pliku.

    Aby uzyskać więcej informacji o tych uprawnień zobacz [Konfigurowanie aplikacji dla klienta GCM dla systemu Android](https://developers.google.com/cloud-messaging/android/client#manifest).

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

        <permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
        <uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>


### <a name="adding-code"></a>Dodawanie kodu


1. W widoku projektu, rozwiń **aplikacji** > **src** > **głównym** > **java**. Kliknij prawym przyciskiem myszy folderu pakietu w obszarze **java**, kliknij pozycję **Nowy**, a następnie kliknij **Klasy języka Java**. Dodawanie nowej klasy o nazwie `NotificationSettings`. 

    ![Android Studio — nowej klasy języka Java][6]

    Upewnij się, że aktualizowania tych trzech symbole zastępcze w następujący kod dla `NotificationSettings` klasy:
    * **SenderId**: numer projektu uzyskanych wcześniej w [Google Cloud konsoli](http://cloud.google.com/console).
    * **HubListenConnectionString**: **DefaultListenAccessSignature** parametry połączenia z koncentratora. Karta **Ustawienia** z Twoim Centrum w [Azure Portal]klikając polecenie **Zasady dostępu** , możesz skopiować tego ciągu połączenia.
    * **HubName**: Użyj nazwy swojej Centrum powiadomienie, które pojawi się karta Centrum w [Azure Portal].

    `NotificationSettings`Kod:

        public class NotificationSettings {
            public static String SenderId = "<Your project number>";
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Your default listen connection string>";
        }

2. Za pomocą powyższych czynności, dodać innego nowej klasy o nazwie `MyInstanceIDService`. Są to nasz implementacji usługi odbiornika identyfikator wystąpienia.

    Kod tej klasy nawiąże połączenie naszych `IntentService` [GCM token odświeżanie](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) w tle.

        import android.content.Intent;
        import android.util.Log;
        import com.google.android.gms.iid.InstanceIDListenerService;
        
        
        public class MyInstanceIDService extends InstanceIDListenerService {
        
            private static final String TAG = "MyInstanceIDService";
        
            @Override
            public void onTokenRefresh() {
        
                Log.i(TAG, "Refreshing GCM Registration Token");
        
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


3. Dodawanie innego nowej klasy do projektu o nazwie `RegistrationIntentService`. Są to wykonania naszych `IntentService` którego będą obsługiwane [odświeżanie GCM token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) i [Rejestrowanie z poziomu Centrum powiadomienie](notification-hubs-push-notification-registration-management.md).

    Dla tej klasy, należy użyć następującego kodu.

        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;
        
        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.google.android.gms.iid.InstanceID;
        import com.microsoft.windowsazure.messaging.NotificationHub;
        
        public class RegistrationIntentService extends IntentService {
        
            private static final String TAG = "RegIntentService";
        
            private NotificationHub hub;
        
            public RegistrationIntentService() {
                super(TAG);
            }
        
            @Override
            protected void onHandleIntent(Intent intent) {      
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
        
                try {
                    InstanceID instanceID = InstanceID.getInstance(this);
                    String token = instanceID.getToken(NotificationSettings.SenderId,
                            GoogleCloudMessaging.INSTANCE_ID_SCOPE);        
                    Log.i(TAG, "Got GCM Registration Token: " + token);
        
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if ((regID=sharedPreferences.getString("registrationID", null)) == null) {      
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.i(TAG, "Attempting to register with NH using token : " + token);

                        regID = hub.register(token).getRegistrationId();

                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1", "tag2").getRegistrationId();

                        resultString = "Registered Successfully - RegId : " + regID;
                        Log.i(TAG, resultString);       
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                    } else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete token refresh", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
        
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }


        
4. W swojej `MainActivity` klasy, Dodaj następujący `import` instrukcji powyżej deklaracji klasy.

        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.google.android.gms.gcm.*;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

5. Dodaj następujące elementy prywatny u góry klasy. Używamy tych [sprawdzić dostępność usługi Google Play zgodnie z zaleceniami Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private GoogleCloudMessaging gcm;
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;

6. W swojej `MainActivity` klasy, Dodaj następujące metody dostępności usługi Google Play. 

        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }


7. W swojej `MainActivity` klasy, Dodaj następujący kod, który będzie sprawdzał usługi Google Play przed połączeniem usługi `IntentService` pobieranie rejestracji GCM token i zarejestrować w Twoim Centrum powiadomienie.

        public void registerWithNotificationHubs()
        {
            Log.i(TAG, " Registering with Notification Hubs");
    
            if (checkPlayServices()) {
                // Start IntentService to register this application with GCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }


8. W `OnCreate` metody `MainActivity` klasy, Dodaj następujący kod, aby rozpocząć proces rejestracji po utworzeniu aktywności.

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }


9. Dodaj następujące dodatkowe metody `MainActivity` Aby sprawdzić stan i raport stanu aplikacji w aplikacji.

        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
    
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
    
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
    
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
    
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }


10. `ToastNotify` Metody zastosowania *"Witaj świecie"* `TextView` formant, aby raportować stan i powiadomienia trwale w aplikacji. W układzie activity_main.xml Dodaj następujący identyfikator dla tego formantu.

        android:id="@+id/text_hello"

11. Następnie możemy dodać klasy Nasze odbiorcy zdefiniowanych w AndroidManifest.xml. Dodawanie innego nowej klasy do projektu o nazwie `MyHandler`.

12. Dodawanie poniższych instrukcji importu w górnej części `MyHandler.java`:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;

13. Dodaj następujący kod dla `MyHandler` zajęć, dzięki czemu rodzaj `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Zastępuje kod `OnReceive` metody, tak aby obsługi będzie raportować powiadomień, które są odbierane. Obsługę wysyła również powiadomień wypychanych do Menedżera powiadomień Android przy użyciu `sendNotification()` metody. `sendNotification()` Metody powinna zostać wykonana, gdy aplikacja ta nie działa i otrzyma powiadomienie.

        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
        
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
        
            private void sendNotification(String msg) {
        
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
        
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
        
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
        
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }


14. W Android Studio na pasku menu, kliknij przycisk **Konstruuj** > **Odbudowanie projektu** , aby upewnić się, że żadne błędy znajdują się w kodzie.

##<a name="sending-push-notifications"></a>Wysyłanie powiadomienia wypychane

Istnieje możliwość przetestowania otrzymywania powiadomień wypychanych w aplikacji przez wysłanie im przez [Azure Portal] - Odszukaj sekcję **Rozwiązywanie problemów** w karta Centrum, jak pokazano poniżej.

![Wyślij powiadomienie Azure koncentratory — Test](./media/notification-hubs-android-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Opcjonalnie) Wysyłanie powiadomienia wypychane bezpośrednio z aplikacji

Czy są zwykle Wyślij powiadomienia przy użyciu serwera wewnętrznej bazy danych. W niektórych przypadkach można mieć możliwość wysyłania powiadomień wypychanych bezpośrednio z aplikacji klienckiej. W tej sekcji wyjaśniono, jak wysyłać powiadomienia w kliencie za pomocą [Interfejsu API usługi REST Centrum powiadomienie Azure](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. W widoku projektu Android Studio rozwiń **aplikacji** > **src** > **głównym** > **rozdzielczość** > **układu**. Otwórz `activity_main.xml` układ plik i kliknij pozycję **tekst** kartę, aby zaktualizować zawartość tekstu pliku. Aktualizowanie za pomocą kodu poniżej, co spowoduje dodanie nowych `Button` i `EditText` służy do wysyłania powiadomień wypychanych do koncentratora powiadomienie. Dodaj ten kod u dołu, bezpośrednio przed `</RelativeLayout>`.

        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />

        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />

2. W widoku projektu Android Studio rozwiń **aplikacji** > **src** > **głównym** > **rozdzielczość** > **wartości**. Otwórz `strings.xml` plik i dodawanie wartości ciągu, które odwołują się nowy `Button` i `EditText` kontrolek. Dodaj te w dolnej części pliku, bezpośrednio przed `</resources>`.

        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>


3. W swojej `NotificationSetting.java` plików, Dodaj następujące ustawienia do `NotificationSettings` zajęć.

    Aktualizacja `HubFullAccess` przy użyciu parametrów połączenia **DefaultFullSharedAccessSignature** do koncentratora. Ten ciąg połączenia może zostać skopiowany z [Azure Portal] , klikając pozycję **Zasady dostępu** na karta **Ustawienia** z koncentratora powiadomienie.

        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccess Connection string>";

4. W swojej `MainActivity.java` plików, Dodaj następujący `import` instrukcji podanych `MainActivity` zajęć.

        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;

6. W swojej `MainActivity.java` pliku i dodaj następujące elementy u góry z `MainActivity` zajęć.  

        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;

6. Musisz utworzyć tokenu oprogramowania Access podpisu (SA) do uwierzytelniania żądania POST do wysyłania wiadomości do Twojej powiadomień Centrum. To jest analizowanie danych klucza z ciągu połączenia, a następnie tworząc token skojarzeń zabezpieczeń wymienione w odwołaniu interfejsu API usługi REST [Typowych pojęcia](http://msdn.microsoft.com/library/azure/dn495627.aspx) . Następujący kod jest przykładem implementacji.

    W `MainActivity.java`, dodawanie następującej metody `MainActivity` klasy przeanalizować ciągu połączenia.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
    
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }


7. W `MainActivity.java`, dodawanie następującej metody `MainActivity` klasy w celu utworzenia tokenu uwierzytelniania skojarzeń zabezpieczeń.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
    
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
    
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
    
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
    
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
    
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
    
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
    
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
    
            return token;
        }



8. W `MainActivity.java`, dodawanie następującej metody `MainActivity` klasy obsługiwać kliknij przycisk **Wyślij powiadomienie** i wysyłanie wiadomości z powiadomieniem o wypychanych do koncentratora za pomocą wbudowanych interfejsu API usługi REST.

        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
    
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
    
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
    
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
    
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
    
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
    
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //      "tag1 || tag2 || tag3");
    
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
    
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }

                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }



##<a name="testing-your-app"></a>Testowanie aplikacji

####<a name="push-notifications-in-the-emulator"></a>Powiadomienia push w emulatorze

Jeśli chcesz przetestować powiadomienia wypychane wewnątrz emulatora, upewnij się, że obraz emulatora obsługuje poziom interfejsu API usługi Google, który wybrano dla aplikacji. Jeśli obraz nie obsługuje macierzystych interfejsów API usługi Google, będzie na końcu **usługi\_nie\_dostępne** wyjątek.

Oprócz powyższych upewnij się, że dodano swoje konto Google do uruchomionego emulatora w obszarze **Ustawienia** > **konta**. W przeciwnym razie próby rejestrować GCM może spowodować **uwierzytelniania\_nie powiodło się** wyjątek.

####<a name="running-the-application"></a>Uruchamianie aplikacji

1. Uruchom aplikację i zwróć uwagę, że identyfikator rejestracji jest zgłoszone dla pomyślnego rejestracji.

    ![Testowanie w systemie Android - rejestracji kanału][18]

2. Wprowadź wiadomości z powiadomieniem były wysyłane do wszystkich urządzeń z systemem Android zarejestrowanych z poziomu Centrum.

    ![Testowanie w systemie Android — wysyłanie wiadomości][19]

3. Naciśnij klawisz **Wyślij powiadomienie**. Zostanie wyświetlona wszystkich urządzeń aplikację z `AlertDialog` wystąpienie z tą wiadomością powiadomień wypychanych. Nie masz aplikacji do uruchamiania, które zostały już zarejestrowane urządzenia powiadomienia wypychane, otrzymasz powiadomienie w Menedżerze powiadomień systemu Android. Te można wyświetlić, szybko przesuwając w dół w lewym górnym rogu.

    ![Testowanie w systemie Android — powiadomienia][21]

##<a name="next-steps"></a>Następne kroki

Następny krok zalecamy samouczka [Użyj koncentratory powiadomień wypychanych powiadomień dla użytkowników] . Jej procedurach pokazano, jak do wysyłania powiadomień z wewnętrznej bazy danych programu ASP.NET przy użyciu tagów do określonych użytkowników.

Jeśli chcesz segmentu użytkowników według grup odsetek, zapoznaj się z samouczka [Użyj koncentratory powiadomień do wysłania najnowszych wiadomości] .

Aby uzyskać więcej informacji o koncentratory powiadomień, zobacz nasze [Wskazówki koncentratory powiadomienie].

<!-- Images. -->
[6]: ./media/notification-hubs-android-get-started/notification-hub-android-new-class.png

[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-new-project.png
[14]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-choose-form-factor.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png

[18]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-registered.png
[19]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-set-message.png

[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-received-message.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png

[31]: ./media/notification-hubs-android-get-started/notification-hubs-android-studio-add-ui.png


<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Powiadomienie o koncentratory wskazówki]: http://msdn.microsoft.com/library/jj927170.aspx
[Powiadomienie o koncentratory za pomocą powiadomień wypychanych dla użytkowników]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com
