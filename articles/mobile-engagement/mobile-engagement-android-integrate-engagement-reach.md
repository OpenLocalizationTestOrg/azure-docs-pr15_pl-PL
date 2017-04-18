<properties
    pageTitle="Integracja Android SDK Azure zaangażowania urządzeń przenośnych"
    description="Najnowszych aktualizacji i procedury dla systemu Android SDK dla zaangażowania Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-reach-on-android"></a>Jak zintegrować Reach zaangażowania w systemie Android

> [AZURE.IMPORTANT] Należy wykonać integracji procedurę opisaną w sekcji jak zintegrować zaangażowania Android dokumentu przed wykonaniem tego przewodnika.

##<a name="standard-integration"></a>Integracja standardowy

SDK osiągnięcia wymaga **biblioteki pomocy technicznej Android (4)**.

Najszybszym sposobem dodania biblioteki do projektu w **Zaćmienie** jest `Right click on your project -> Android Tools -> Add Support Library...`.

Jeśli nie używasz Zaćmienie, można zapoznaj się z instrukcjami [poniżej].

Kopiowanie plików zasobów Reach z zestawu SDK w projekcie:

-   Kopiowanie plików z `res/layout` folder dostarczane z zestawu SDK do `res/layout` folderu aplikacji.
-   Kopiowanie plików z `res/drawable` folder dostarczane z zestawu SDK do `res/drawable` folderu aplikacji.

Edytowanie swojego `AndroidManifest.xml` pliku:

-   Dodaj następującą sekcję (między `<application>` i `</application>` znaczniki):

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
              <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
              </intent-filter>
            </receiver>

-   Potrzebujesz to uprawnienie do ponownego odtworzenia powiadomienia systemowe, które nie zostały kliknięte przy uruchomieniu (w przeciwnym razie będą przechowywane na dysku, ale nie będą już wyświetlane, które trzeba uwzględnić to).

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   Określ ikonę używaną w przypadku powiadomienia (zarówno w aplikacji i system z nich), kopiując i edytowania sekcji poniżej (między `<application>` i `</application>` znaczniki):

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [AZURE.IMPORTANT] Ta sekcja jest **obowiązkowe** , jeśli zamierzasz użyć powiadomienia systemowe, podczas tworzenia kampanii Reach. Android zapobiega wyświetlanych powiadomienia systemowe bez ikon. Dlatego pominięcie w tej sekcji użytkowników końcowych nie będzie odbieranie ich.

-   Jeśli tworzysz kampanii z powiadomienia systemu przy użyciu przeglądu, musisz dodać następujące uprawnienia (po `</application>` znacznika) Jeśli brakuje:

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Android M i jeśli aplikacja jest przeznaczony dla poziomu Android interfejsu API 23 lub nowszego ``WRITE_EXTERNAL_STORAGE`` uprawnień wymaga zatwierdzenia przez użytkownika. Przeczytaj [tę sekcję](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

-   Powiadomienia systemu można również określić w kampanii Reach Jeśli urządzenie należy dzwonienie i/lub wibracje. Ta opcja działała, należy upewnij się, zadeklarowanych następujących uprawnień (po `</application>` znacznika):

            <uses-permission android:name="android.permission.VIBRATE" />

    Bez tego uprawnienia Android zapobiega powiadomienia systemowe jest wyświetlany, jeśli zostanie zaznaczone pole wyboru pierścienia lub opcji vibrate w Menedżerze osiągnięcia kampanii.

-   Jeśli możesz utworzyć aplikację za pomocą **ProGuard** i masz błędów związanych z biblioteki Android pomocy technicznej lub słoik zaangażowania, Dodaj następujące wiersze do swojego `proguard.cfg` pliku:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

## <a name="native-push"></a>Natywne Push

Teraz, gdy skonfigurowano Reach moduł, musisz skonfigurować natywnych naciśnij, aby mieć możliwość odbierania kampanii na urządzeniu.

W systemie Android obsługujemy dwie usługi:

  - Urządzenia Google Play: używanie [Usługi Google Cloud wiadomości] , wykonując przewodnik [jak zintegrować GCM zaangażowania prowadnicy](mobile-engagement-android-gcm-integrate.md) .
  - Urządzenia Amazon: używanie [Amazon urządzenia wiadomości] , wykonując przewodnik [jak zintegrować ADM zaangażowania prowadnicy](mobile-engagement-android-adm-integrate.md) .

Jeśli chcesz przeanalizować urządzeń zarówno Amazon i Google Play, najbliżej elementy wewnątrz 1 AndroidManifest.xml/APK rozwoju. Jednak podczas przesyłania do Amazon, mogą oni odrzucić aplikacji jeśli znajdą GCM kodu.

W tym przypadku należy korzystać z wielu APKs.

**Aplikacja jest teraz gotowy do odbierania i wyświetlenie kampanii reach!**

##<a name="how-to-handle-data-push"></a>Jak obsługiwać wypychanych danych

### <a name="integration"></a>Integracja z programem

Jeśli chcesz aplikację, aby mieć możliwość odbierania Reach umieszcza danych, należy utworzyć klasy `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` i odwołanie w `AndroidManifest.xml` pliku (między `<application>` i/lub `</application>` znaczniki):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Następnie można zastąpić `onDataPushStringReceived` i `onDataPushBase64Received` zwrotne. Oto przykład:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Kategoria

Parametr kategorii jest opcjonalny, podczas tworzenia kampanii Push danych i umożliwia umieszcza umożliwia filtrowanie danych. To jest przydatne, jeśli masz kilka odbiorców emisji obsługi różnych rodzajów umieszcza danych lub jeśli chcesz przekazać różne typy z `Base64` danych i chcesz identyfikowanie ich typ przed ich analizą.

### <a name="callbacks-return-parameter"></a>Parametr zwrotu zwrotne

Oto kilka wskazówek prawidłową obsługę parametr zwrotny `onDataPushStringReceived` i `onDataPushBase64Received`:

-   Słuchawka emisji powinna zwrócić `null` w zwrotnym, jeśli nie znasz sposobu obsługi wypychanych danych. Za pomocą kategorii należy określić, czy usługi emisji odbiorca powinien obsługiwać wypychanych danych, czy nie.
-   Jedną z emisji odbiorca powinna zwrócić `true` w zwrotnym, jeśli przyjmuje wypychanych danych.
-   Jedną z emisji odbiorca powinna zwrócić `false` w zwrotnym, jeśli rozpoznaje wypychanych danych, ale usuwa go bez względu na powód. Jeśli na przykład zwraca `false` po otrzymaniu danych są nieprawidłowe.
-   Jeśli emisja jedną zwraca odbiorcy `true` podczas innego zwraca jedną `false` dla samej zamówienie danych, zachowanie nie zdefiniowano, nigdy nie powinna to zrobić.

Zwracany typ jest używany tylko w przypadku statystyki Reach:

-   `Replied`jest zwiększany, jeśli niektórych z nich emisji albo `true` lub `false`.
-   `Actioned`jest zwiększany tylko wtedy, gdy jeden z emisji odbiorców zwrócił `true`.

##<a name="how-to-customize-campaigns"></a>Jak dostosować kampanii

Aby dostosować kampanii, można modyfikować układy określone w SDK osiągnięcia.

Należy zachować wszystkie identyfikatory używane w układach i zachować typy widoków, które za pomocą identyfikatora, szczególnie w przypadku widoków tekstu i obrazów. Niektóre widoki są używane tylko do ukrywanie i pokazywanie obszarów, więc ich typ może być zmieniony. Jeśli chcesz zmienić typ widoku w układach dostarczonych Sprawdź kod źródłowy.

### <a name="notifications"></a>Powiadomienia

Istnieją dwa typy powiadomień: system i w aplikacji powiadomień, które korzystanie z plików inny układ.

#### <a name="system-notifications"></a>Powiadomienia systemowe

Okno dialogowe Dostosowywanie powiadomień systemu muszą korzystać z **kategorii**. Można przejść bezpośrednio do [kategorii](#categories).

#### <a name="in-app-notifications"></a>Powiadomienia w aplikacji

Powiadomienie w aplikacji jest domyślnie widok, w którym jest dynamicznie dodawane do bieżącego interfejsu użytkownika działanie dzięki metodę Android `addContentView()`. Jest to nakładki powiadomienie. Powiadomienie o nakładki są nadaje się do szybkiego integracji, ponieważ nie wymagają modyfikowania dowolny układ w aplikacji.

Aby zmodyfikować wygląd nakładki usługi powiadomień, wystarczy zmodyfikować plik `engagement_notification_area.xml` do własnych potrzeb.

> [AZURE.NOTE] Plik `engagement_notification_overlay.xml` jest, w którym jest używany do tworzenia nakładki powiadomień, w tym plik `engagement_notification_area.xml`. Możesz również dostosować go do własnych potrzeb (tak jak w przypadku położenia w obszarze powiadomień nakładki).

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Dołączanie układu powiadomienie jako część układu aktywności

Nakładki doskonale nadają się do szybkiego integracji, ale można niewłaściwym lub mieć wpływ strony w szczególnych przypadkach. System nakładki można dostosować na poziomie aktywności, co ułatwia zapobieganie stronie efektów specjalnych działań.

Można zdecydować uwzględnić naszych układu powiadomienie w układzie istniejących wykonywanie instrukcji Android **zawiera** . Oto przykład zmienione `ListActivity` układ zawierający tylko `ListView`.

**Przed połączeniem zaangażowania:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Po połączeniu zaangażowania:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

W tym przykładzie dodana kontenera nadrzędnego ponieważ oryginalny układ używany widoku listy jako element najwyższego poziomu. Również dodany `android:layout_weight="1"` aby można było dodać widok poniżej widoku listy skonfigurowano `android:layout_height="fill_parent"`.

SDK osiągnięcia zaangażowania automatycznie wykrywa, że układ powiadomień znajduje się w tej czynności i nie zostaną dodane nakładki dla tego działania.

> [AZURE.TIP] Jeśli używasz ListActivity w aplikacji, widoczne nakładki Reach uniemożliwi reakcji do już kliknięte elementów w widoku listy. Jest to znany problem. Aby obejść ten problem Sugerujemy umożliwia osadzanie układu powiadomień na własne aktywności układ lista podobnie jak w poprzednim przykładzie.

##### <a name="disabling-application-notification-per-activity"></a>Wyłączanie powiadomień aplikacji dla działania

Jeśli nie chcesz nakładki mają zostać dodane do swojej aktywności, i nie zawiera układ powiadomienie własnego układu, możesz wyłączyć nakładki dla tego działania w `AndroidManifest.xml` , dodając `meta-data` sekcji, takich jak w poniższym przykładzie:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Kategorie

Po zmodyfikowaniu przedstawionych układów modyfikowanie wyglądu wszystkich powiadomień o wiadomościach. Kategorie umożliwiają definiowanie różnych wygląda docelowej (prawdopodobnie zachowania) powiadomienia. Po utworzeniu kampanii Reach można określić kategorii. Pamiętać, że kategorie umożliwiają również dostosować ogłoszenia i ankiety, opisanej w dalszej części tego dokumentu.

Aby zarejestrować obsługi kategorii powiadomienia, musisz dodać połączenia podczas inicjowania aplikacji.

> [AZURE.IMPORTANT] Przeczytaj ostrzeżenie dotyczące atrybutu android: proces \<zaangażowania procesu, android sdk w-\> w sposób zintegrować zaangażowania temat Android przed kontynuowaniem.

W poniższym przykładzie założono potwierdzone poprzednim ostrzeżeniem i używanie klasy `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

`MyNotifier` Obiekt jest stosowania obsługi kategorii powiadomienie. Jest implementacja z `EngagementNotifier` interfejsu lub klasy podrzędnej realizacji domyślne: `EngagementDefaultNotifier`.

Zwróć uwagę, że tym samym powiadamiający może obsługiwać kilka kategorii, można zarejestrować je tak jak poniżej:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Aby zamienić Domyślna implementacja kategorii, można zarejestrować implementacji, podobnie jak w poniższym przykładzie:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

Kategoria bieżąca używane w uchwyt przekazywana jako parametr w większości metod można zastąpić w `EngagementDefaultNotifier`.

Przekazywana jako `String` parametru lub pośrednio `EngagementReachContent` obiekt, który ma `getCategory()` metody.

Możesz zmienić większość procesu tworzenia powiadomienie, zmiana definicji metod na `EngagementDefaultNotifier`, aby uzyskać bardziej zaawansowane dostosowanie zachęcamy do zapoznaj się z dokumentacją techniczną i kod źródłowy.

##### <a name="in-app-notifications"></a>Powiadomienia w aplikacji

Jeśli chcesz tylko za pomocą układów alternatywne dla określonej kategorii, można zaimplementować tak jak w poniższym przykładzie:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Przykład `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Jak widać, identyfikator widoku nakładki jest inny niż standardowy. Należy pamiętać, że każdy układ używać Unikatowy identyfikator do nakładki.

**Przykład `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Jak widać, identyfikator widoku obszar powiadomień jest innej niż standardowy. Należy pamiętać, że każdy układ używa Unikatowy identyfikator obszary powiadomień.

Ten prosty przykład kategorii sprawia, że powiadomienia aplikacji (lub w aplikacji) wyświetlany w górnej części ekranu. Nie został zmieniony używane w obszarze powiadomień się identyfikatory standardowy.

Jeśli chcesz zmienić to ustawienie, należy zdefiniować ponownie `EngagementDefaultNotifier.prepareInAppArea` metody. Zalecamy Szukaj w dokumentacji technicznej i kod źródłowy `EngagementNotifier` i `EngagementDefaultNotifier` Jeśli chcesz to poziom dostosowania zaawansowanego.

##### <a name="system-notifications"></a>Powiadomienia systemowe

Wydłużając `EngagementDefaultNotifier`, można zastąpić `onNotificationPrepared` zmienić powiadomienie, które został przygotowany przez wykonanie domyślnej.

Na przykład:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

W tym przykładzie sprawia, że powiadomienie systemu dla zawartości są wyświetlane jako zdarzenie trwających, kiedy "stałe" kategoria jest używana.

Jeśli chcesz utworzyć `Notification` obiekt od podstaw, można przywrócić `false` metody i połączenia `notify` sobie na `NotificationManager`. W tym przypadku ważne jest, aby zachować `contentIntent`, `deleteIntent` i identyfikator powiadomienie, używany przez `EngagementReachReceiver`.

Oto przykład poprawnego takich realizacji:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Powiadomienie o tylko ogłoszenia

Zarządzanie kliknij pozycję tylko zawiadomienie o można dostosować przez zastąpienie powiadomienia o `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` do modyfikowania przygotowanej `Intent`. Ta metoda pozwala na łatwe dostosowywanie flagi.

Na przykład aby dodać `SINGLE_TOP` flagi:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

W przypadku starszych użytkowników zaangażowania należy pamiętać, że powiadomienia systemowe bez akcji adresu URL teraz uruchamia aplikację Jeśli został on w tle, więc ta metoda może być wywołany z ogłoszenie bez adresu URL akcji. Które należy uwzględnić, podczas dostosowywania zamiarem.

Można także zaimplementować `EngagementNotifier.executeNotifAnnouncementAction` od podstaw.

##### <a name="notification-life-cycle"></a>Powiadomienie o cyklu

Podczas korzystania z domyślnej kategorii, niektóre metody cyklu życia są nazywane na `EngagementReachInteractiveContent` obiektu do raportu statystyk i zaktualizować stan kampanii:

-   Gdy powiadomienie jest wyświetlane w aplikacji lub umieszczenie na pasku stanu `displayNotification` metoda jest wywoływana (który raportów statystyki) przez `EngagementReachAgent` Jeśli `handleNotification` zwraca `true`.
-   Jeśli powiadomienie jest odrzucone, `exitNotification` metody zgłoszone statystyczny i następnej kampanii teraz mogą być przetwarzane.
-   Po kliknięciu powiadomienie `actionNotification` jest wywoływana statystyczny jest zgłaszany i uruchomieniu przeznaczenie skojarzone.

Jeśli instalacja programu `EngagementNotifier` pomija domyślne zachowanie masz nawiązywanie połączenia z tych metod cyklu przez siebie. W poniższych przykładach przedstawiono w niektórych przypadkach, gdy pominięte domyślne zachowanie:

-   Nie można rozszerzyć `EngagementDefaultNotifier`, np. wdrożonej obsługi kategorii od podstaw.
-   Powiadomienia system overrode `onNotificationPrepared` i zmodyfikowano `contentIntent` lub `deleteIntent` w `Notification` obiektu.
-   Powiadomienia w aplikacji app overrode `prepareInAppArea`, pamiętaj zamapować co najmniej `actionNotification` do jednego z U.I kontrolki.

> [AZURE.NOTE] Jeśli `handleNotification` Wystąpił wyjątek, zawartość zostanie usunięty i `dropContent` nosi nazwę. Jest to zgłoszone w statystyce i teraz można przetwarzać następnej kampanii.

### <a name="announcements-and-polls"></a>Ogłoszenia i ankiety

#### <a name="layouts"></a>Układy

Można modyfikować `engagement_text_announcement.xml`, `engagement_web_announcement.xml` i `engagement_poll.xml` pliki, aby dostosować tekst ogłoszenia, anonsy sieci web i ankiety.

Te pliki udostępnianie dwa układy wspólne dla obszaru tytułu i obszar przycisku. Układ tytuł jest `engagement_content_title.xml` i używa gromadzący pliku drawable tła. Układ przycisków akcji i Zakończ jest `engagement_button_bar.xml` i używa gromadzący pliku drawable tła.

W ankiety, układ pytanie i wybory ich są dynamicznie zawyżone przy użyciu kilka razy `engagement_question.xml` plik układu pytania oraz `engagement_choice.xml` pliku dla opcji.

#### <a name="categories"></a>Kategorie

##### <a name="alternate-layouts"></a>Alternatywne układy

Takie jak powiadomienia Kategoria kampanii może służyć do istnieją alternatywne układy ogłoszenia i ankiety.

Na przykład, aby utworzyć kategorię anons tekstu, można rozszerzyć `EngagementTextAnnouncementActivity` i odwoływać `AndroidManifest.xml` pliku:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Należy zauważyć, że w kategorii w celu filtrowania używany do zabezpieczania różnica z działaniem zawiadomienie o domyślnej.

SDK osiągnięcia korzysta z określonym przeznaczeniem systemu rozwiązywać prawo działania dla określonej kategorii, a wypada Wstecz w domyślnej kategorii Jeśli rozdzielczość nie powiodła się.

Następnie masz do wprowadzenia w życie `MyCustomTextAnnouncementActivity`, jeśli chcesz zmienić układ (, ale zachować samej identyfikatory widok), należy w celu zdefiniowania klasy podobnie jak w poniższym przykładzie:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Aby zamienić kategorii domyślny tekst ogłoszenia, po prostu Zamień `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` przez implementacji.

W podobny sposób można dostosować anonsy sieci Web i ankiety.

Dla ogłoszeń sieci web można rozszerzyć `EngagementWebAnnouncementActivity` i zadeklarować aktywności w `AndroidManifest.xml` , takich jak w poniższym przykładzie:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

Ankiety można rozszerzyć `EngagementPollActivity` i zadeklarować usługi w `AndroidManifest.xml` , takich jak w poniższym przykładzie:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Implementacja od podstaw

Można zaimplementować kategorie działalności zawiadomienie o (i ankiety) bez rozszerzania jeden z `Engagement*Activity` klasy dostarczony przez SDK osiągnięcia. Jest to przydatne, na przykład jeśli chcesz zdefiniować układ, która nie korzysta z tym samym widoków jako układy standardowe.

Przykład dostosowywania powiadomień zaawansowane, zaleca się przeglądanie kodu źródłowego standardowej implementacji.

Należy pamiętać o kilku kwestiach: Reach uruchomi aktywności z zamiarem określonych (odpowiadającą do konwersji filtru) oraz dodatkowy parametr, który jest identyfikator zawartości.

Aby pobrać obiekt zawartości, które zawierają pola, które zostało określone podczas tworzenia kampanii w witrynie sieci web możesz to zrobić:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Statystyki, zgłoś zawartość jest wyświetlana w `onResume` zdarzeń:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Następnie, nie zapomnij połączeń albo `actionContent(this)` lub `exitContent(this)` na obiekt zawartości przed wprowadzeniem działanie w tle.

Jeśli nie zostanie wywołana albo `actionContent` lub `exitContent`, nie zostaną wysłane statystyki (to znaczy nie analizy kampanii) i co ważne, następny kampanii nie zostaną powiadomieni, ponownym uruchomieniu procesu aplikacji.

Orientacja lub innych zmian w konfiguracji można wprowadzić kod kłopotliwe określić, czy to działanie przechodzi w tle lub nie, standardowej implementacji służy do sprawdzenia, zawartość jest zgłoszone jako zakończone, jeśli użytkownik opuści działania (albo, naciskając klawisz `HOME` lub `BACK`), ale nie zmiana orientacji.

Oto interesująca wykonania:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Jak widać, jeśli `actionContent(this)` , a następnie zakończyła działanie, `exitContent(this)` może być bezpiecznie wywołana bez wpływu.

[w tym miejscu]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Usługa Google Cloud wiadomości]:http://developer.android.com/guide/google/gcm/index.html
[Amazon urządzenia wiadomości]:https://developer.amazon.com/sdk/adm.html
