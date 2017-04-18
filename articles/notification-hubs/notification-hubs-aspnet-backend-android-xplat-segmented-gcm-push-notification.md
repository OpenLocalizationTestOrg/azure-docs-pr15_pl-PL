<properties
    pageTitle="Przerywanie wiadomości samouczka — koncentratory powiadomień systemu Android"
    description="Dowiedz się, jak używać koncentratory powiadomienie Bus usługi Azure do wysyłania powiadomień wiadomości podziału na urządzeniach z systemem Android."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>


# <a name="use-notification-hubs-to-send-breaking-news"></a>Wysyłanie najnowszych wiadomości za pomocą koncentratorów powiadomień

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

##<a name="overview"></a>Omówienie

W tym temacie przedstawiono sposoby emisja podziału powiadomień wiadomości aplikację Android przy użyciu koncentratory powiadomienie Azure. Po zakończeniu będzie mógł rejestrować zakłócenia kategorie wiadomości, który Cię interesuje się i odbierać tylko powiadomień wypychanych dla tych kategorii. W tym scenariuszu jest wzorzec wspólne dla wielu aplikacji miejsce, w którym powiadomienia były wysyłane do grup użytkowników, które zadeklarowały wcześniej zainteresowanie ich przykład czytnika danych RSS, aplikacje dla wentylatory muzyki itp.

Scenariusze emisji są włączone, dołączając jeden lub więcej _znaczników_ , podczas tworzenia rejestracji w Centrum powiadomienie. Powiadomienia o wysłaniu do znacznika wszystkich urządzeń, które zarejestrowane znacznika otrzyma powiadomienie. Ponieważ znaczniki są ciągami, po prostu, nie muszą być przygotowana z wyprzedzeniem. Aby uzyskać więcej informacji na temat znaczników odwołują się do [routingu koncentratory powiadomienie i wyrażeń znacznik](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Wymagania wstępne

W tym temacie tworzy w aplikacji utworzonej w [wprowadzenie koncentratory powiadomienie][get-started]. Przed rozpoczęciem tego samouczka, musisz zostały już wykonane [wprowadzenie koncentratory powiadomienie][get-started].

##<a name="add-category-selection-to-the-app"></a>Dodaj wybór kategorii do aplikacji

Pierwszym krokiem jest dodanie elementów interfejsu użytkownika do istniejącego aktywności głównym umożliwiające użytkownikowi wybór kategorii, aby zarejestrować. Kategorie wybrane przez użytkownika są przechowywane na tym urządzeniu. Po uruchomieniu aplikacji Rejestracja urządzenia zostanie utworzona w Twoim Centrum powiadomienie z wybranych kategorii jako znaczniki.

1. Otwórz plik res/layout/activity_main.xml i zastąpić zawartości z następujących czynności:

        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:tools="http://schemas.android.com/tools"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/activity_vertical_margin"
            android:paddingLeft="@dimen/activity_horizontal_margin"
            android:paddingRight="@dimen/activity_horizontal_margin"
            android:paddingTop="@dimen/activity_vertical_margin"
            tools:context="com.example.breakingnews.MainActivity"
            android:orientation="vertical">

                <CheckBox
                    android:id="@+id/worldBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_world" />
                <CheckBox
                    android:id="@+id/politicsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_politics" />
                <CheckBox
                    android:id="@+id/businessBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_business" />
                <CheckBox
                    android:id="@+id/technologyBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_technology" />
                <CheckBox
                    android:id="@+id/scienceBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_science" />
                <CheckBox
                    android:id="@+id/sportsBox"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="@string/label_sports" />
                <Button
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:onClick="subscribe"
                    android:text="@string/button_subscribe" />
        </LinearLayout>

2. Otwórz plik res/values/strings.xml i dodaj następujące wiersze:

        <string name="button_subscribe">Subscribe</string>
        <string name="label_world">World</string>
        <string name="label_politics">Politics</string>
        <string name="label_business">Business</string>
        <string name="label_technology">Technology</string>
        <string name="label_science">Science</string>
        <string name="label_sports">Sports</string>

    Układ graficzny main_activity.xml powinna teraz wyglądać następująco:

    ![][A1]

3. Teraz można utworzyć klasę **powiadomienia** w pakiecie swojej klasy **MainActivity** .

        import java.util.HashSet;
        import java.util.Set;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.os.AsyncTask;
        import android.util.Log;
        import android.widget.Toast;

        import com.google.android.gms.gcm.GoogleCloudMessaging;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class Notifications {
            private static final String PREFS_NAME = "BreakingNewsCategories";
            private GoogleCloudMessaging gcm;
            private NotificationHub hub;
            private Context context;
            private String senderId;

            public Notifications(Context context, String senderId, String hubName, 
                                    String listenConnectionString) {
                this.context = context;
                this.senderId = senderId;
        
                gcm = GoogleCloudMessaging.getInstance(context);
                hub = new NotificationHub(hubName, listenConnectionString, context);
            }

            public void storeCategoriesAndSubscribe(Set<String> categories)
            {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                settings.edit().putStringSet("categories", categories).commit();
                subscribeToCategories(categories);
            }

            public Set<String> retrieveCategories() {
                SharedPreferences settings = context.getSharedPreferences(PREFS_NAME, 0);
                return settings.getStringSet("categories", new HashSet<String>());
            }

            public void subscribeToCategories(final Set<String> categories) {
                new AsyncTask<Object, Object, Object>() {
                    @Override
                    protected Object doInBackground(Object... params) {
                        try {
                            String regid = gcm.register(senderId);
        
                            String templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";
        
                            hub.registerTemplate(regid,"simpleGCMTemplate", templateBodyGCM, 
                                categories.toArray(new String[categories.size()]));
                        } catch (Exception e) {
                            Log.e("MainActivity", "Failed to register - " + e.getMessage());
                            return e;
                        }
                        return null;
                    }
        
                    protected void onPostExecute(Object result) {
                        String message = "Subscribed for categories: "
                                + categories.toString();
                        Toast.makeText(context, message,
                                Toast.LENGTH_LONG).show();
                    }
                }.execute(null, null, null);
            }

        }

    Ta klasa używa lokalnego magazynu do przechowywania kategorie to urządzenie ma odbierać wiadomości. Zawiera również metody rejestrowania dla tych kategorii.


4. W swojej klasy **MainActivity** usuwanie prywatnych pól **NotificationHub** i **GoogleCloudMessaging**i dodać pole **powiadomień**:

        // private GoogleCloudMessaging gcm;
        // private NotificationHub hub;
        private Notifications notifications;

5. Następnie w polu Metoda **onCreate** usunąć inicjowanie pola **Centrum** i metody **registerWithNotificationHubs** . Następnie dodaj następujące wiersze, które zainicjować wystąpienie klasy **powiadomienia** . 


        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            MyHandler.mainActivity = this;
    
            NotificationsManager.handleNotifications(this, SENDER_ID,
                    MyHandler.class);
    
            notifications = new Notifications(this, SENDER_ID, HubName, HubListenConnectionString);
    
            notifications.subscribeToCategories(notifications.retrieveCategories());
        }

    `HubName`i `HubListenConnectionString` powinny zostać ustalone już `<hub name>` i `<connection string with listen access>` symboli zastępczych z nazwą Centrum powiadomienie i parametry połączenia dla *DefaultListenSharedAccessSignature* uzyskany wcześniej.

    > [AZURE.NOTE] Ponieważ poświadczeń, które są rozdzielane z aplikacji klienckiej nie są zwykle bezpieczne, należy tylko rozpowszechnić klucz dostępu słuchać aplikację klienta programu. Odsłuchać umożliwia dostępu, których nie można zmodyfikować aplikacji zarejestrować powiadomienia, ale istniejące rejestracji i nie mogą być wysyłane powiadomienia. Klucz pełny dostęp jest używany w usłudze bezpiecznego wewnętrznej bazy danych do wysyłania powiadomień i zmieniania istniejących rejestracji.


6. Następnie dodaj następujące operacje importowania i `subscribe` metody obsługi przycisk Subskrybuj kliknij zdarzenie:
        
        import android.widget.CheckBox;
        import java.util.HashSet;
        import java.util.Set;

        public void subscribe(View sender) {
            final Set<String> categories = new HashSet<String>();

            CheckBox world = (CheckBox) findViewById(R.id.worldBox);
            if (world.isChecked())
                categories.add("world");
            CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
            if (politics.isChecked())
                categories.add("politics");
            CheckBox business = (CheckBox) findViewById(R.id.businessBox);
            if (business.isChecked())
                categories.add("business");
            CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
            if (technology.isChecked())
                categories.add("technology");
            CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
            if (science.isChecked())
                categories.add("science");
            CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
            if (sports.isChecked())
                categories.add("sports");

            notifications.storeCategoriesAndSubscribe(categories);
        }

    Ta metoda tworzy listę kategorii i używa klasy **powiadomienia** do przechowywania listy w magazynie lokalnym i zarejestrować odpowiednich znaczników w Twoim Centrum powiadomienie. Po zmianie kategorie rejestracji zostanie ponownie utworzony przy użyciu nowych kategorii.

Aplikacja jest teraz można przechowywać zestawu kategorii w magazynu lokalnego na tym urządzeniu i zarejestrować w Centrum powiadomienie o zmianie użytkownika wybór kategorii.

##<a name="register-for-notifications"></a>Rejestruj powiadomienia

Te kroki rejestrować Centrum powiadomień podczas uruchamiania przy użyciu kategorii, przechowywane w magazynie lokalnym.

> [AZURE.NOTE] Ponieważ atrybutów registrationId, przypisane przez wiadomości chmury Google (GCM) można zmienić w dowolnym momencie, należy zarejestrować powiadomienia często uniknąć błędów powiadomienie. W tym przykładzie rejestruje powiadomienie o każdym uruchomieniu aplikacji. W przypadku aplikacji uruchamianych często więcej niż jeden raz dziennie, można prawdopodobnie pominąć rejestracji w celu zachowania odpowiedniej przepustowości sieci, jeśli od poprzedniej rejestracji upłynął mniej niż jeden dzień.


1. Na koniec metoda **onCreate** klasy **MainActivity** , Dodaj następujący kod:

        notifications.subscribeToCategories(notifications.retrieveCategories());

    To służy do sprawdzenia, że zawsze podczas uruchamiania aplikacji pobiera kategorie z magazynu lokalnego i żądania registeration dla tych kategorii. 

2. Następnie zaktualizuj `onStart()` metody `MainActivity` klasy w następujący sposób:

    @Overridechroniony void onStart() {super.onStart();      isVisible = PRAWDA;

        Set<String> categories = notifications.retrieveCategories();

        CheckBox world = (CheckBox) findViewById(R.id.worldBox);
        world.setChecked(categories.contains("world"));
        CheckBox politics = (CheckBox) findViewById(R.id.politicsBox);
        politics.setChecked(categories.contains("politics"));
        CheckBox business = (CheckBox) findViewById(R.id.businessBox);
        business.setChecked(categories.contains("business"));
        CheckBox technology = (CheckBox) findViewById(R.id.technologyBox);
        technology.setChecked(categories.contains("technology"));
        CheckBox science = (CheckBox) findViewById(R.id.scienceBox);
        science.setChecked(categories.contains("science"));
        CheckBox sports = (CheckBox) findViewById(R.id.sportsBox);
        sports.setChecked(categories.contains("sports"));
    }

    To uaktualnienie głównej działalności na podstawie stanu kategorii zapisane wcześniej.

Aplikacja zostało zakończone i mogą zawierać zbiór kategorie w magazynie lokalnym urządzenia używane do rejestrowania z poziomu Centrum powiadomienie o zmianie użytkownika wybór kategorii. Następnie firma Microsoft określi wewnętrznej bazy danych, które mogą wysyłać powiadomienia kategorii do tej aplikacji.

##<a name="sending-tagged-notifications"></a>Wysyłanie powiadomienia oznakowane

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Uruchom aplikację i generowanie powiadomień

1. W Android Studio tworzenie aplikacji, a następnie uruchom go na urządzeniu lub emulatora.

    Notatki, że aplikacja interfejsu użytkownika zawiera zestaw przełącza umożliwiającego wybierz kategorie, aby subskrybować.

2. Włączanie jeden lub więcej przełącza kategorii, a następnie kliknij przycisk **Subskrybuj**.

    Aplikacja konwertuje wybranych kategorii znaczników i zażąda nowej rejestracji urządzenia dla wybranych znaczników z poziomu Centrum powiadomienie. Kategorie zarejestrowanych są zwracane i wyświetlane w wyskakującego powiadomienia.

4. Wyślij powiadomienie o nowej, uruchamiając aplikację konsoli .NET.  Alternatywnie możesz wysłać powiadomienia szablonów oznaczonych przy użyciu karty debugowania Twoim Centrum powiadomienie w [Klasycznym Portal Azure].

    Powiadomienia dla wybranych kategorii są wyświetlane jako wyskakującego powiadomienia.

##<a name="next-steps"></a>Następne kroki

W tym samouczku opisano sposoby emisja najnowszych wiadomości według kategorii. Należy rozważyć, wykonując jedną z następujące samouczki wyróżniające innych zaawansowanych scenariuszy koncentratory powiadomień:

+ [Emisja zlokalizowanym najnowszych wiadomości za pomocą koncentratorów powiadomień]

    Dowiedz się, jak rozwiń aplikację wiadomości podziału, aby włączyć wysyłanie powiadomienia o zlokalizowanym.





<!-- Images. -->
[A1]: ./media/notification-hubs-aspnet-backend-android-breaking-news/android-breaking-news1.PNG

<!-- URLs.-->
[get-started]: notification-hubs-android-push-notification-google-gcm-get-started.md
[Emisja zlokalizowanym najnowszych wiadomości za pomocą koncentratorów powiadomień]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Portal Azure klasyczny]: https://manage.windowsazure.com
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
