<properties
    pageTitle="Azure powiadomienie koncentratory Powiadom użytkowników dla systemu Android z wewnętrznej bazy danych programu .NET"
    description="Dowiedz się, jak wysyłać powiadomienia wypychane dla użytkowników w Azure. Przykłady kodu napisanego w Java dla systemu Android"
    documentationCenter="android"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-notify-users-for-android-with-net-backend"></a>Azure powiadomienie koncentratory Powiadom użytkowników dla systemu Android z wewnętrznej bazy danych programu .NET


[AZURE.INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

##<a name="overview"></a>Omówienie

Obsługa powiadomień wypychanych w Azure umożliwia dostęp do prostych w użyciu, multiplatform i infrastruktury wypychanych skalowanej, co znacznie upraszcza implementację powiadomień wypychanych dla zarówno dla klientów indywidualnych, jak i przedsiębiorstwa aplikacji dla urządzeń przenośnych platform. Ten samouczek pokazano, jak używać koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych dla konkretnej aplikacji użytkownika konkretnego urządzenia. ASP.NET WebAPI wewnętrznej bazy danych jest używany do uwierzytelniania klientów, a także do generowania powiadomień, jak pokazano w temacie wskazówki [Rejestrowanie z sieci wewnętrznej bazy danych aplikacji](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend). Ten samouczek tworzy koncentratora powiadomienie utworzonego w samouczku [Wprowadzenie powiadomienie koncentratory (system Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) .

> [AZURE.NOTE] Tego samouczka przyjęto założenie, że masz została utworzona i skonfigurowana do Centrum powiadomienie, zgodnie z opisem w [Wprowadzenie powiadomienie koncentratory (system Android)](notification-hubs-android-push-notification-google-gcm-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="create-the-android-project"></a>Tworzenie projektu systemu Android

Następnym krokiem jest utworzenie Android aplikacji.

1. Skorzystać z samouczka [Wprowadzenie powiadomienie koncentratory (system Android)](notification-hubs-android-push-notification-google-gcm-get-started.md) , tworzenie i konfigurowanie aplikacji otrzymywać powiadomienia wypychane od GCM.

2. Otwórz plik **res/layout/activity_main.xml** , Zamień z następujące definicje zawartości.

    Spowoduje to dodanie nowego formanty typu części EditText zalogować się jako użytkownik. Również pole zostanie dodane znacznika nazwy użytkownika, który będzie częścią powiadomień, które wysyłasz:

        <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
            android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

        <EditText
            android:id="@+id/usernameText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/usernameHint"
            android:layout_above="@+id/passwordText"
            android:layout_alignParentEnd="true" />
        <EditText
            android:id="@+id/passwordText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="@string/passwordHint"
            android:inputType="textPassword"
            android:layout_above="@+id/buttonLogin"
            android:layout_alignParentEnd="true" />
        <Button
            android:id="@+id/buttonLogin"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/loginButton"
            android:onClick="login"
            android:layout_above="@+id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:layout_marginBottom="24dp" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="WNS on"
            android:textOff="WNS off"
            android:id="@+id/toggleButtonWNS"
            android:layout_toLeftOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="GCM on"
            android:textOff="GCM off"
            android:id="@+id/toggleButtonGCM"
            android:checked="true"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true" />
        <ToggleButton
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textOn="APNS on"
            android:textOff="APNS off"
            android:id="@+id/toggleButtonAPNS"
            android:layout_toRightOf="@id/toggleButtonGCM"
            android:layout_centerVertical="true" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessageTag"
            android:layout_below="@id/toggleButtonGCM"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_tag_hint" />
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/editTextNotificationMessage"
            android:layout_below="@+id/editTextNotificationMessageTag"
            android:layout_centerHorizontal="true"
            android:hint="@string/notification_message_hint" />
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/send_button"
            android:id="@+id/sendbutton"
            android:onClick="sendNotificationButtonOnClick"
            android:layout_below="@+id/editTextNotificationMessage"
            android:layout_centerHorizontal="true" />
        </RelativeLayout>



3. Otwórz plik **res/values/strings.xml** i zamienić `send_button` definicji następujące wiersze, które zdefiniuj ciąg dla `send_button` i Dodaj ciągi dla pozostałych kontrolek:

        <string name="usernameHint">Username</string>
        <string name="passwordHint">Password</string>
        <string name="loginButton">1. Log in</string>
        <string name="send_button">2. Send Notification</string>
        <string name="notification_message_tag_hint">
            Recipient username tag
        </string>

    Układ graficzny main_activity.xml powinna teraz wyglądać następująco:

    ![][A1]

4. Tworzenie nowej klasy o nazwie **RegisterClient** w pakiecie z `MainActivity` zajęć. Poniższy kod za pomocą nowego pliku zajęć.

        import java.io.IOException;
        import java.io.UnsupportedEncodingException;
        import java.util.Set;

        import org.apache.http.HttpResponse;
        import org.apache.http.HttpStatus;
        import org.apache.http.client.ClientProtocolException;
        import org.apache.http.client.HttpClient;
        import org.apache.http.client.methods.HttpPost;
        import org.apache.http.client.methods.HttpPut;
        import org.apache.http.client.methods.HttpUriRequest;
        import org.apache.http.entity.StringEntity;
        import org.apache.http.impl.client.DefaultHttpClient;
        import org.apache.http.util.EntityUtils;
        import org.json.JSONArray;
        import org.json.JSONException;
        import org.json.JSONObject;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.util.Log;

        public class RegisterClient {
            private static final String PREFS_NAME = "ANHSettings";
            private static final String REGID_SETTING_NAME = "ANHRegistrationId";
            private String Backend_Endpoint;
            SharedPreferences settings;
            protected HttpClient httpClient;
            private String authorizationHeader;

            public RegisterClient(Context context, String backendEnpoint) {
                super();
                this.settings = context.getSharedPreferences(PREFS_NAME, 0);
                httpClient =  new DefaultHttpClient();
                Backend_Endpoint = backendEnpoint + "/api/register";
            }

            public String getAuthorizationHeader() {
                return authorizationHeader;
            }

            public void setAuthorizationHeader(String authorizationHeader) {
                this.authorizationHeader = authorizationHeader;
            }

            public void register(String handle, Set<String> tags) throws ClientProtocolException, IOException, JSONException {
                String registrationId = retrieveRegistrationIdOrRequestNewOne(handle);

                JSONObject deviceInfo = new JSONObject();
                deviceInfo.put("Platform", "gcm");
                deviceInfo.put("Handle", handle);
                deviceInfo.put("Tags", new JSONArray(tags));

                int statusCode = upsertRegistration(registrationId, deviceInfo);

                if (statusCode == HttpStatus.SC_OK) {
                    return;
                } else if (statusCode == HttpStatus.SC_GONE){
                    settings.edit().remove(REGID_SETTING_NAME).commit();
                    registrationId = retrieveRegistrationIdOrRequestNewOne(handle);
                    statusCode = upsertRegistration(registrationId, deviceInfo);
                    if (statusCode != HttpStatus.SC_OK) {
                        Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                        throw new RuntimeException("Error upserting registration");
                    }
                } else {
                    Log.e("RegisterClient", "Error upserting registration: " + statusCode);
                    throw new RuntimeException("Error upserting registration");
                }
            }

            private int upsertRegistration(String registrationId, JSONObject deviceInfo)
                    throws UnsupportedEncodingException, IOException,
                    ClientProtocolException {
                HttpPut request = new HttpPut(Backend_Endpoint+"/"+registrationId);
                request.setEntity(new StringEntity(deviceInfo.toString()));
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                request.addHeader("Content-Type", "application/json");
                HttpResponse response = httpClient.execute(request);
                int statusCode = response.getStatusLine().getStatusCode();
                return statusCode;
            }

            private String retrieveRegistrationIdOrRequestNewOne(String handle) throws ClientProtocolException, IOException {
                if (settings.contains(REGID_SETTING_NAME))
                    return settings.getString(REGID_SETTING_NAME, null);

                HttpUriRequest request = new HttpPost(Backend_Endpoint+"?handle="+handle);
                request.addHeader("Authorization", "Basic "+authorizationHeader);
                HttpResponse response = httpClient.execute(request);
                if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                    Log.e("RegisterClient", "Error creating registrationId: " + response.getStatusLine().getStatusCode());
                    throw new RuntimeException("Error creating Notification Hubs registrationId");
                }
                String registrationId = EntityUtils.toString(response.getEntity());
                registrationId = registrationId.substring(1, registrationId.length()-1);

                settings.edit().putString(REGID_SETTING_NAME, registrationId).commit();

                return registrationId;
            }
        }

    Wdrażane połączeń pozostałe wymagane do skontaktuj się z aplikacji wewnętrznej bazy danych, aby można było zarejestrować powiadomienia wypychane. Przechowuje również lokalnie *registrationIds* utworzone przez Centrum powiadomienie zgodnie z [Rejestrowanie z Twojej aplikacji wewnętrznej bazy danych](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend). Należy zauważyć, że używa tokenu autoryzacji przechowywane w magazynie lokalnym, po kliknięciu przycisku **Zaloguj** .

5. W swojej `MainActivity` klasy usunąć lub skomentować prywatne pole dla `NotificationHub`, i Dodaj pole `RegisterClient` zajęć i ciągu dla punktu końcowego usługi ASP.NET wewnętrznej bazy danych. Pamiętaj zamienić `<Enter Your Backend Endpoint>` z punkt końcowy rzeczywistych wewnętrznej bazy danych uzyskanego wcześniej. Na przykład `http://mybackend.azurewebsites.net`.


        //private NotificationHub hub;
        private RegisterClient registerClient;
        private static final String BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";


6. W swojej `MainActivity` klasy w `onCreate` metody, usunąć lub skomentować inicjowania `hub` pola i połączenie `registerWithNotificationHubs` metody. Następnie dodaj kod, aby zainicjować wystąpienie `RegisterClient` zajęć. Metoda powinien zawierać następujące wiersze:

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            MyHandler.mainActivity = this;
            NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);
            gcm = GoogleCloudMessaging.getInstance(this);

            //hub = new NotificationHub(HubName, HubListenConnectionString, this);
            //registerWithNotificationHubs();

            registerClient = new RegisterClient(this, BACKEND_ENDPOINT);

            setContentView(R.layout.activity_main);
        }

7. W swojej `MainActivity` klasy, usunąć lub skomentować całej `registerWithNotificationHubs` metody. Go nie będą używane w tym samouczku.

8. Dodaj następujący `import` instrukcji do pliku **MainActivity.java** .

        import android.widget.Button;
        import java.io.UnsupportedEncodingException;
        import android.content.Context;
        import java.util.HashSet;
        import android.widget.Toast;
        import org.apache.http.client.ClientProtocolException;
        import java.io.IOException;
        import org.apache.http.HttpStatus;


9. Następnie dodaj następujące metody obsługi przycisk **Zaloguj się** kliknij zdarzenie i wysyłania powiadomień wypychanych.

        @Override
        protected void onStart() {
            super.onStart();
            Button sendPush = (Button) findViewById(R.id.sendbutton);
            sendPush.setEnabled(false);
        }

        public void login(View view) throws UnsupportedEncodingException {
            this.registerClient.setAuthorizationHeader(getAuthorizationHeader());

            final Context context = this;
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {
                        String regid = gcm.register(SENDER_ID);
                        registerClient.register(regid, new HashSet<String>());
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to register", e.getMessage());
                        return e;
                    }
                    return null;
                }

                protected void onPostExecute(Object result) {
                    Button sendPush = (Button) findViewById(R.id.sendbutton);
                    sendPush.setEnabled(true);
                    Toast.makeText(context, "Logged in and registered.",
                            Toast.LENGTH_LONG).show();
                }
            }.execute(null, null, null);
        }

        private String getAuthorizationHeader() throws UnsupportedEncodingException {
            EditText username = (EditText) findViewById(R.id.usernameText);
            EditText password = (EditText) findViewById(R.id.passwordText);
            String basicAuthHeader = username.getText().toString()+":"+password.getText().toString();
            basicAuthHeader = Base64.encodeToString(basicAuthHeader.getBytes("UTF-8"), Base64.NO_WRAP);
            return basicAuthHeader;
        }

        /**
         * This method calls the ASP.NET WebAPI backend to send the notification message
         * to the platform notification service based on the pns parameter.
         *
         * @param pns     The platform notification service to send the notification message to. Must
         *                be one of the following ("wns", "gcm", "apns").
         * @param userTag The tag for the user who will receive the notification message. This string
         *                must not contain spaces or special characters.
         * @param message The notification message string. This string must include the double quotes
         *                to be used as JSON content.
         */
        public void sendPush(final String pns, final String userTag, final String message)
                throws ClientProtocolException, IOException {
            new AsyncTask<Object, Object, Object>() {
                @Override
                protected Object doInBackground(Object... params) {
                    try {

                        String uri = BACKEND_ENDPOINT + "/api/notifications";
                        uri += "?pns=" + pns;
                        uri += "&to_tag=" + userTag;

                        HttpPost request = new HttpPost(uri);
                        request.addHeader("Authorization", "Basic "+ getAuthorizationHeader());
                        request.setEntity(new StringEntity(message));
                        request.addHeader("Content-Type", "application/json");

                        HttpResponse response = new DefaultHttpClient().execute(request);

                        if (response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                            DialogNotify("MainActivity - Error sending " + pns + " notification",
                                response.getStatusLine().toString());
                            throw new RuntimeException("Error sending notification");
                        }
                    } catch (Exception e) {
                        DialogNotify("MainActivity - Failed to send " + pns + " notification ", e.getMessage());
                        return e;
                    }

                    return null;
                }
            }.execute(null, null, null);
        }


    `login` Obsługi dla przycisku **Zaloguj** generuje uwierzytelnianie podstawowe token za pomocą na wprowadzania nazwy użytkownika i hasła (należy zauważyć, że jest to dowolny token używa schemat uwierzytelniania), a następnie używa `RegisterClient` połączenie wewnętrznej bazy danych rejestracji.

    `sendPush` Metoda wywołuje wewnętrznej bazy danych wyzwalać bezpiecznego powiadomienie użytkownikowi według tagu użytkownika. Usługa powiadomień platformy `sendPush` elementów docelowych zależy od `pns` przekazany ciąg.

10. W swojej `MainActivity` klasy, aktualizowanie `sendNotificationButtonOnClick` metody nawiązać połączenie `sendPush` w następujący sposób wybrana metoda z danym użytkownikiem usługi powiadomień platformy.

        /**
         * Send Notification button click handler. This method sends the push notification
         * message to each platform selected.
         *
         * @param v The view
         */
        public void sendNotificationButtonOnClick(View v)
                throws ClientProtocolException, IOException {

            String nhMessageTag = ((EditText) findViewById(R.id.editTextNotificationMessageTag))
                    .getText().toString();
            String nhMessage = ((EditText) findViewById(R.id.editTextNotificationMessage))
                    .getText().toString();

            // JSON String
            nhMessage = "\"" + nhMessage + "\"";

            if (((ToggleButton)findViewById(R.id.toggleButtonWNS)).isChecked())
            {
                sendPush("wns", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonGCM)).isChecked())
            {
                sendPush("gcm", nhMessageTag, nhMessage);
            }
            if (((ToggleButton)findViewById(R.id.toggleButtonAPNS)).isChecked())
            {
                sendPush("apns", nhMessageTag, nhMessage);
            }
        }



## <a name="run-the-application"></a>Uruchamianie aplikacji


1. Uruchom aplikację na urządzeniu lub emulatora przy użyciu Android Studio.

2. W aplikacji, Android wprowadź nazwę użytkownika i hasło. Ich muszą być tej samej wartości ciągu i nie może zawierać spacji ani znaków specjalnych.

3. W aplikacji, Android kliknij przycisk **Zaloguj**. Czekaj na komunikat wyskakującego **zarejestrowane i zarejestrowane**. Spowoduje to włączenie przycisku **Wyślij powiadomienie** .

    ![][A2]

4. Kliknij przycisk przełączania przyciski umożliwiające wszystkich platform, w której znajduje się uruchomieniu aplikacji i zarejestrowana użytkownika.
5. Wprowadź nazwę użytkownika, który otrzyma powiadomienie. Użytkownik musi być zarejestrowany powiadomień urządzenia.

6. Wpisz wiadomość dla użytkownik ma odbierać jako wiadomości z powiadomieniem push.
7. Kliknij przycisk **Wyślij powiadomienie**.  Każde urządzenie, zawierającą rejestracji znacznikiem pasujące nazwy użytkownika otrzyma powiadomienie o push.


[A1]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users.png
[A2]: ./media/notification-hubs-aspnet-backend-android-notify-users/android-notify-users-enter-password.png
