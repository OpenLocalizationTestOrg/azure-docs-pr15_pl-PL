1. Otwórz plik w projekcie **aplikacji** `AndroidManifest.xml`. W kodzie w następnych dwóch krokach Zamień _`**my_app_package**`_ nazwy pakietu aplikacji dla projektu, który jest wartością `package` atrybut `manifest` znacznik.

2. Dodaj następujące nowe uprawnienia za istniejącą `uses-permission` elementu:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Dodaj następujący kod po `application` znaczników otwierających:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                        android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>


4. Otwórz plik *ToDoActivity.java*, a następnie dodaj następującą instrukcję importu:

        import com.microsoft.windowsazure.notifications.NotificationsManager;


5. Dodaj następującą zmienną prywatne klasy: Zamień _`<PROJECT_NUMBER>`_ z numerem projektu przypisane przez Google do aplikacji w powyższej procedurze:

        public static final String SENDER_ID = "<PROJECT_NUMBER>";

6. Zmienianie definicji *MobileServiceClient* z **prywatne** na **publiczne statyczne**, aby teraz wygląda następująco:

        public static MobileServiceClient mClient;

7. Następnie należy dodać nową klasę obsługi powiadomień. W oknie Project Explorer Otwórz **src** => **głównym** => węzły**java** i kliknij prawym przyciskiem myszy węzeł nazwa pakietu: kliknij pozycję **Nowy**, a następnie kliknij pozycję **Klasy języka Java**.

8. W polu Typ **nazwy** `MyHandler`, następnie kliknij **przycisk OK**.


    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)


9. W pliku MyHandler Zamień deklaracji klasy z

        public class MyHandler extends NotificationsHandler {


10. Dodawanie poniższych instrukcji importu dla `MyHandler` klasy:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;


11. Następnie dodać ten element do `MyHandler` klasy:

        public static final int NOTIFICATION_ID = 1;


12. W `MyHandler` klasy, Dodaj następujący kod, aby zastąpić metodę **onRegistered** , która rejestruje urządzenia z usługi mobilnej powiadomień Centrum.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
            super.onRegistered(context, gcmRegistrationId);

            new AsyncTask<Void, Void, Void>() {

                protected Void doInBackground(Void... params) {
                    try {
                        ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                        return null;
                    }
                    catch(Exception e) {
                        // handle error             
                    }
                    return null;            
                }
            }.execute();
        }


13. W `MyHandler` klasy, Dodaj następujący kod, aby zastąpić metodę **onReceive** , co powoduje, że po odebraniu wyświetlane powiadomienie.

        @Override
        public void onReceive(Context context, Bundle bundle) {
                String msg = bundle.getString("message");

                PendingIntent contentIntent = PendingIntent.getActivity(context,
                        0, // requestCode
                        new Intent(context, ToDoActivity.class),
                        0); // flags

                Notification notification = new NotificationCompat.Builder(context)
                        .setSmallIcon(R.drawable.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                        .setContentText(msg)
                        .setContentIntent(contentIntent)
                        .build();

                NotificationManager notificationManager = (NotificationManager)
                        context.getSystemService(Context.NOTIFICATION_SERVICE);
                notificationManager.notify(NOTIFICATION_ID, notification);
        }


14. W pliku TodoActivity.java aktualizowanie metody **onCreate** klasy *ToDoActivity* zarejestrować klasy obsługi powiadomień. Upewnij się dodać ten kod po *MobileServiceClient* zostanie uruchomiony.


        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Teraz zaktualizowaniu aplikacji do obsługi powiadomienia wypychane.
