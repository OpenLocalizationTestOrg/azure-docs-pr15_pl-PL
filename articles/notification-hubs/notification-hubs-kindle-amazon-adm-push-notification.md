<properties
    pageTitle="Wprowadzenie Azure powiadomienie koncentratorów dla aplikacji Kindle | Microsoft Azure"
    description="W tym samouczku dowiesz się, jak za pomocą koncentratorów powiadomienie Azure wysłać powiadomienia wypychane do aplikacji Kindle."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Rozpoczynanie pracy z koncentratorów powiadomienia w przypadku aplikacji Kindle

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Omówienie

Ten samouczek pokazano, jak używać koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych aplikacji Kindle.
Procedura tworzenia pustej aplikacji Kindle, który otrzymuje powiadomienia wypychane przy użyciu wiadomości urządzenia Amazon (ADM).

##<a name="prerequisites"></a>Wymagania wstępne

Ten samouczek ma następujące wymagania:

+ Pobieranie zestawu SDK systemu Android (przyjęto założenie, że użyje Zaćmienie) z <a href="http://go.microsoft.com/fwlink/?LinkId=389797">witryny Android</a>.
+ Postępuj zgodnie z instrukcjami w <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Ustawienie w górę Twoje środowisko projektowania</a> Konfigurowanie środowiska programowania dla Kindle.

##<a name="add-a-new-app-to-the-developer-portal"></a>Dodawanie nowej aplikacji do portalu — Deweloper

1. Najpierw należy utworzyć aplikację w [Amazon dzięki portalowi deweloperów].

    ![][0]

2. Kopiowanie **klawisz aplikacji**.

    ![][1]

3. W portalu kliknij nazwę aplikacji, a następnie kliknij kartę **Urządzenia wiadomości** .

    ![][2]

4. Kliknij przycisk **Utwórz nowy profil zabezpieczeń**, a następnie utwórz nowy profil zabezpieczeń (na przykład **profil zabezpieczeń TestAdm**). Następnie kliknij przycisk **Zapisz**.

    ![][3]

5. Kliknij pozycję **Profile zabezpieczeń** , aby wyświetlić profil zabezpieczeń, która została właśnie utworzona. Skopiuj wartości **Identyfikator klienta** i **Hasło klienta** do późniejszego użycia.

    ![][4]

## <a name="create-an-api-key"></a>Tworzenie klucz interfejsu API

1. Otwórz wiersz polecenia z uprawnieniami administratora.
2. Przejdź do folderu SDK systemu Android.
3. Wpisz następujące polecenie:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  W przypadku **elementu keystore** hasło wpisz **android**.

5.  Kopiowanie **MD5** odcisku palca.
6.  W portalu Deweloper, na karcie **wiadomości** kliknij pozycję **Android/Kindle** i wprowadź nazwę pakietu aplikacji (na przykład **com.sample.notificationhubtest**) i wartości **MD5** , a następnie kliknij **Wygenerować klucz interfejsu API**.

## <a name="add-credentials-to-the-hub"></a>Dodawanie poświadczeń do koncentratora

W portalu Dodaj klucz tajny klienta i identyfikator klienta na karcie **Konfigurowanie** Twoim Centrum powiadomienie.

## <a name="set-up-your-application"></a>Konfigurowanie aplikacji

> [AZURE.NOTE] Gdy tworzysz aplikację, wykonaj co najmniej 17 poziom interfejsu API.

Dodawanie bibliotek ADM do projektu Zaćmienie:

1. Aby uzyskać biblioteki ADM, [pobrać zestaw SDK]. Wyodrębnianie pliku zip SDK.
2. W Zaćmienie kliknij prawym przyciskiem myszy projektu, a następnie kliknij polecenie **Właściwości**. Wybierz **Ścieżkę tworzenie Java** po lewej stronie, a następnie wybierz kartę **bibliotek **u góry. Kliknij pozycję **Dodaj Jar zewnętrznych**i wybierz plik `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` z katalogu, w którym zostały wyodrębnione Amazon SDK.
3. Pobierz NotificationHubs SDK systemu Android (łącze).
4. Rozpakuj plik pakietu, a następnie przeciągnij plik `notification-hubs-sdk.jar` do `libs` folderu Zaćmienie.

Edytowanie manifeście aplikacji do obsługi ADM:

1. Dodawanie nazw Amazon w elemencie głównym manifestu:


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Dodaj uprawnienia jako pierwszy element w obszarze manifestu elementu. **[Nazwa pakietu i]** należy zastąpić pakietu, który został użyty do utworzenia aplikacji.

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Wstaw następujący element jako pierwszy element podrzędny elementu aplikacji. Pamiętaj, aby zastąpić nazwę usługi obsługi wiadomości ADM, które są tworzone w następnej sekcji (w tym pakietu) **[TWOJA nazwa usługi]** i zamienić **[TWOJA nazwa pakietu]** nazwa pakietu, z którym utworzono aplikacji.

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Tworzenie programu obsługi wiadomości ADM

1. Tworzenie nowej klasy, która dziedziczy `com.amazon.device.messaging.ADMMessageHandlerBase` i nadaj mu nazwę `MyADMMessageHandler`, jak pokazano na poniższym rysunku:

    ![][6]

2. Dodaj następujący `import` instrukcje:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Dodaj następujący kod w klasie utworzone przez Ciebie. Pamiętaj zastąpić ciąg połączenia i nazwę Centrum (słuchać):

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. Dodaj następujący kod do `OnMessage()` metody:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. Dodaj następujący kod do `OnRegistered` metody:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  Dodaj następujący kod do `OnUnregistered` metody:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. W `MainActivity` metody, należy dodać następującą instrukcję importu:

        import com.amazon.device.messaging.ADM;

8. Dodaj następujący kod na końcu `OnCreate` metody:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-to-your-app"></a>Dodawanie klucza interfejsu API do aplikacji

1. W Zaćmienie Utwórz nowy plik o nazwie **api_key.txt** w katalogu składniki majątku projektu.
2. Otwórz plik, a następnie skopiuj klucz interfejsu API, który wygenerował w portalu Amazon Deweloper.

## <a name="run-the-app"></a>Uruchamianie aplikacji

1. Rozpocznij emulatorze.
2. W emulatorze szybko przesuń od góry i kliknij pozycję **Ustawienia**, a następnie kliknij pozycję **Moje konto** i zarejestrować przy użyciu prawidłowego konta Amazon.
3. W Zaćmienie Uruchom aplikację.

> [AZURE.NOTE] Jeśli wystąpi problem, należy sprawdzić czas emulatora (lub urządzenia). Wartość musi odpowiadać. Aby zmienić czas emulatora Kindle, można uruchomić następujące polecenie z katalogu narzędzia platformy SDK systemu Android:

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Wysyłanie wiadomości

Aby wysłać wiadomość przy użyciu .NET:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Dzięki portalowi deweloperów Amazon]: https://developer.amazon.com/home.html
[Pobieranie zestawu SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
