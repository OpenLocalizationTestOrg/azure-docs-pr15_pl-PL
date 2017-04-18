<properties
    pageTitle="Wprowadzenie do koncentratorów powiadomienie Azure za pomocą Baidu | Microsoft Azure"
    description="W tym samouczku dowiesz się, jak za pomocą Azure koncentratory powiadomień wypychanych powiadomień urządzeń z systemem Android przy użyciu Baidu."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="08/19/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-using-baidu"></a>Wprowadzenie do koncentratorów powiadomień przy użyciu Baidu

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Omówienie

Baidu chmury push to usługa w chmurze języka chińskiego, który służy do wysyłania powiadomień wypychanych na urządzenia przenośne. Ta usługa jest szczególnie przydatna w Chinach, gdzie przedstawiania powiadomień wypychanych w systemie Android jest złożone ze względu na obecność przechowuje różnych aplikacji i usług wypychanych, oprócz dostępności urządzeń z systemem Android, które nie są zazwyczaj połączone GCM (Google Cloud wiadomości).

##<a name="prerequisites"></a>Wymagania wstępne

Ten samouczek ma następujące wymagania:

+ Android SDK (przyjęto założenie, że użyje Zaćmienie), który można pobrać z <a href="http://go.microsoft.com/fwlink/?LinkId=389797">witryny Android</a>
+ [Usług mobilnych SDK systemu Android]
+ [Baidu wypychanych SDK systemu Android]

>[AZURE.NOTE] Aby użyć tego samouczka, musi mieć konto Azure aktywne. Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##<a name="create-a-baidu-account"></a>Tworzenie konta Baidu

Aby użyć Baidu, musisz mieć konto Baidu. Jeśli masz już jeden, logowanie się do [portalu Baidu] i przejdź do następnego kroku. W przeciwnym razie zobacz instrukcje na temat tworzenia konta Baidu.  

1. Przejdź do [portalu Baidu] i kliknij łącze**登录**(**Logowanie**). Kliknij przycisk**立即注册**, aby rozpocząć proces rejestracji konta.

    ![][1]

2. Wprowadź wymagane informacje — telefonu i adres, hasło i weryfikacji kod pocztą e-mail — i kliknij pozycję **Zapisywanie**.

    ![][2]

3. Wiadomości e-mail będą wysyłane na adres e-mail wprowadzono z łączem do aktywowanie konta Baidu.

    ![][3]

4. Zaloguj się do konta e-mail, Otwórz program mail aktywacji Baidu i kliknij łącze aktywacji, aby aktywować konto Baidu.

    ![][4]

Po utworzeniu konta Baidu aktywowany, zaloguj się do [portalu Baidu].

##<a name="register-as-a-baidu-developer"></a>Zarejestruj się jako deweloper Baidu

1. Gdy użytkownik zalogował się do [portalu Baidu], kliknij**更多 >>** (**więcej**).

    ![][5]

2. Przewiń w dół w sekcji**站长与开发者服务 (webmastera i usług dla deweloperów)** , a następnie kliknij przycisk**百度开放云平台**(**Baidu Otwórz platformy chmury**).

    ![][6]

3. Na następnej stronie kliknij**开发者服务**(**Developer Services**) w prawym górnym rogu.

    ![][7]

4. Na następnej stronie kliknij pozycję**注册开发者**(**Registered deweloperów**) z menu w prawym górnym rogu.

    ![][8]

5. Wprowadź swoją nazwę, opis i numer telefonu komórkowego do odbierania wiadomości SMS weryfikacji, a następnie kliknij**送验证码**(**Wyślij kod weryfikacyjny**). Należy zauważyć, że dla międzynarodowe numery telefonów, możesz kod kraju, należy ująć w nawiasy. Na przykład dla numeru Stanów Zjednoczonych będzie **(1) 1234567890**.

    ![][9]

6. Następnie powinny otrzymać wiadomość tekstowa numer weryfikacji, jak pokazano w poniższym przykładzie:

    ![][10]

7. Wprowadź numer weryfikacji w wiadomości w**验证码**(**kod potwierdzający**).

8. Następnie dokończ rejestracji Deweloper, akceptując Umowy Baidu i klikając pozycję**提交**(**Prześlij**). Zostanie wyświetlona strona następujące od pomyślnego zakończenia rejestracji:

    ![][11]

##<a name="create-a-baidu-cloud-push-project"></a>Tworzenie projektu Baidu chmury push

Po utworzeniu projektu Baidu chmury push otrzymasz Identyfikatora aplikacji klucz interfejsu API i klucz tajny.

1. Gdy użytkownik zalogował się do [portalu Baidu], kliknij**更多 >>** (**więcej**).

    ![][5]

2. Przewiń w dół w sekcji**站长与开发者服务**(**webmastera i usług dla deweloperów**), a następnie kliknij przycisk**百度开放云平台**(**Baidu Otwórz platformy chmury**).

    ![][6]

3. Na następnej stronie kliknij**开发者服务**(**Developer Services**) w prawym górnym rogu.

    ![][7]

4. Na następnej stronie kliknij**云推送**(**Chmury Push**) z sekcji**云服务**(**Usług w chmurze**).

    ![][12]

5. Po wejściu zarejestrowanych Deweloper pojawi się**管理控制台**(**Konsola zarządzania**) w górnym menu. Kliknij pozycję**开发者服务管理**(**deweloperów usługi zarządzania**).

    ![][13]

6. Na następnej stronie kliknij**创建工程**(**Utwórz projekt**).

    ![][14]

7. Wprowadź nazwę aplikacji, a następnie kliknij przycisk**创建**(**Utwórz**).

    ![][15]

8. Po utworzenia Baidu chmury push projektu zostanie wyświetlona strona z **Identyfikator aplikacji**, **Klucz interfejsu API**i **Klucz tajny**. Zanotuj klucz interfejsu API i klucz tajny, której użyjemy później.

    ![][16]

9. Konfigurowanie projektu do powiadomienia wypychane, klikając pozycję**云推送**(**Chmury Push**) w lewym okienku.

    ![][31]

10. Na następnej stronie kliknij przycisk**推送设置**(**Push ustawienia**).

    ![][32]  

11. Na stronie Konfiguracja Dodaj nazwę pakietu, że można będzie się przy użyciu w projekcie Android w polu**应用包名**(**pakiet aplikacji**), a następnie kliknij**保存设置**(**Zapisz**).  

    ![][33]

Zostanie wyświetlona**保存成功!** (**zostały pomyślnie zapisane!**) wiadomości.

##<a name="configure-your-notification-hub"></a>Konfigurowanie usługi Centrum powiadomień

1. Zaloguj się do [Portalu klasyczny Azure], a następnie kliknij **+ Nowe** u dołu ekranu.

2. Kliknij pozycję **Usługi aplikacji**, kliknij pozycję **Bus usługi**, kliknij pozycję **Centrum powiadomień**, a następnie kliknij **Szybkie tworzenie**.

3. Podaj nazwę **Centrum powiadomień**, wybierz pozycję **Region** i **Namespace** , w której zostanie utworzony tego Centrum powiadomień, a następnie kliknij pozycję **Utwórz nowe Centrum powiadomienie**.  

    ![][17]

4. Kliknij obszar nazw, w którym został utworzony Twoim Centrum powiadomień, a następnie kliknij **Koncentratory powiadomienia** u góry.

    ![][18]

5. Wybierz pozycję Centrum powiadomienie, utworzone przez Ciebie, a następnie kliknij **Konfiguruj** w górnym menu.

    ![][19]

6. Przewiń w dół do sekcji **baidu ustawienia powiadomień** , a następnie wprowadź klucz interfejsu API i tajny klucz pochodzi z konsoli Baidu wcześniej Baidu chmury push projektu. Kliknij przycisk **Zapisz**.

    ![][20]

7. Kliknij kartę **pulpit nawigacyjny** u góry koncentratora powiadomień, a następnie kliknij **Widok parametry połączenia**.

    ![][21]

8. Zanotuj **DefaultListenSharedAccessSignature** i **DefaultFullSharedAccessSignature** w oknie **informacje o połączeniu programu Access** .

    ![][22]

##<a name="connect-your-app-to-the-notification-hub"></a>Łączenie aplikacji z Centrum powiadomień

1. W ADT Zaćmienie utworzyć nowy projekt Android (**plik** > **Nowy** > **Android projekt aplikacji**).

    ![][23]

2. Wprowadź **Nazwę aplikacji** , a następnie upewnij się, że wersji **Zestawu SDK minimalne wymagane** jest ustawiona na **interfejsu API 16: Android 4.1**.

    ![][24]

3. Kliknij przycisk **Dalej** , aż po kreatora do pojawi się okno **Tworzenie aktywności** . Upewnij się, że **Pusty aktywności** jest zaznaczone, a na koniec wybierz pozycję **Zakończ** , aby utworzyć nową aplikację Android.

    ![][25]

4. Upewnij się, czy **Docelowej Tworzenie projektu** jest ustawiona prawidłowo.

    ![][26]

5. Pobierz plik powiadomienie — koncentratory 0.4.jar na karcie **pliki** [Powiadomienie — koncentratory — Android — SDK na Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4). Dodawanie pliku do folderu **bibliotekami** Zaćmienie projektu, a następnie Odśwież folder *bibliotekami* .

6. Pobierz i rozpakuj plik [Baidu Push SDK systemu Android], otwórz folder **bibliotekami** , a następnie skopiuj plik słoik **pushservice x.y.z** i **armeabi** & **mips** foldery w folderze **bibliotekami** Android aplikacji.

7. Otwórz plik **AndroidManifest.xml** Android projektu i Dodaj uprawnienia, które są wymagane przez Baidu SDK.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. Dodaj właściwość **android: Nazwa** do danego elementu **aplikacji** w **AndroidManifest.xml**, zamieniając *yourprojectname* (np. **com.example.BaiduTest**). Upewnij się, że ta nazwa projektu zgodne z skonfigurowanym w konsoli Baidu.

        <application android:name="yourprojectname.DemoApplication"

9. Dodaj następujące konfiguracji w elemencie aplikacji po **. MainActivity** elementu aktywności, zamieniając *yourprojectname* (np. **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Dodawanie nowej klasy o nazwie **ConfigurationSettings.java** do projektu.

    ![][28]

    ![][29]

10. Dodaj następujący kod do niego:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    Ustaw wartość **API_KEY** z pobrane z Baidu chmury projektu wcześniej **NotificationHubName** z nazwą Centrum powiadomienie z portalu klasyczny Azure i **NotificationHubConnectionString** z DefaultListenSharedAccessSignature z portalu klasyczny Azure.

11. Dodaj nowe klasy o nazwie **DemoApplication.java**, a następnie dodaj następujący kod do niego:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Dodawanie innego nowe klasy o nazwie **MyPushMessageReceiver.java**, a następnie dodaj poniższy kod do niego. Jest to klasa obsługujący powiadomień wypychanych, które są odbierane z serwera wypychanych Baidu.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. Otwórz **MainActivity.java**, a następnie dodaj je do metody **onCreate** :

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. Otwórz poniższych instrukcji importu u góry:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##<a name="send-notifications-to-your-app"></a>Wysyłanie powiadomienia do aplikacji


Można szybko sprawdzić otrzymywania powiadomień w aplikacji, wysyłając powiadomienia w [Azure Portal](https://portal.azure.com/) za pomocą przycisku **Testowanie wysyłanie** koncentratora powiadomienie, jak pokazano poniżej ekranu.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Powiadomienia wypychane zwykle są wysyłane w wewnętrznej usług, takich jak usługi Mobile lub ASP.NET przy użyciu zgodne biblioteki. Za pomocą interfejsu API usługi REST bezpośrednio do wysyłania powiadomień, jeśli biblioteki nie jest dostępna dla sieci wewnętrznej.

W tym samouczku pracujemy Zachowaj prostotę i tylko wykazać testowanie aplikacji klienckich przez wysłanie powiadomienia przy użyciu zestawu SDK .NET dla koncentratorów powiadomienie w aplikacji konsoli zamiast usługi wewnętrznej bazy danych. Następny krok do wysyłania powiadomień z wewnętrznej bazy danych programu ASP.NET zalecamy samouczka [Użyj koncentratory powiadomień wypychanych powiadomień dla użytkowników](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) . Jednak poniższych metod może być używany do wysyłania powiadomień:

* **Interfejsu REST**: powiadomienie o stanie obsługiwać na dowolnej platformie wewnętrznej bazy danych przy użyciu [interfejsu REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Microsoft Azure powiadomienie koncentratory .NET SDK**: W Nuget Menedżera pakietów programu Visual Studio, uruchom [Pakiet instalacyjny Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [sposobu używania powiadomienie koncentratory z Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

* **Aplikacje Mobile**: na przykład sposobu wysyłania powiadomień z wewnętrznej bazy danych aplikacji Mobile Azure aplikacji usługi, który jest zintegrowany z koncentratorów powiadomienie zobacz [Dodaj powiadomień wypychanych Twojej aplikacji dla urządzeń przenośnych](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java i PHP**: na przykład sposobu wysyłania powiadomień przy użyciu interfejsy API pozostałych zobacz "Dotyczące używania powiadomienie koncentratory z języka Java-PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

##<a name="optional-send-notifications-from-a-net-console-app"></a>(Opcjonalnie) Wysyłanie powiadomienia za pomocą aplikacji konsoli .NET.

W tej sekcji pokazano wysyłania powiadomień przy użyciu aplikacji konsoli .NET.

1. Tworzenie nowej aplikacji konsoli Visual C#:

    ![][30]

2. W oknie Menedżer pakietów konsoli ustaw **domyślne projektu** z nowym projektem aplikacji konsoli, a następnie w oknie konsoli wykonaj następujące polecenie:

        Install-Package Microsoft.Azure.NotificationHubs

    Spowoduje to dodanie odwołanie do SDK koncentratory powiadomienie Azure za pomocą <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakietu NuGet koncentratory Microsoft.Azure.Notification</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Otwórz plik **Plik Program.cs** i Dodaj następujący za pomocą instrukcji:

        using Microsoft.Azure.NotificationHubs;

4. W swojej `Program` klasy, Dodaj następujące metody i Zamień wartości, które masz *DefaultFullSharedAccessSignatureSASConnectionString* i *NotificationHubName* .

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Dodaj następujące wiersze w metodę **główne** :

         SendNotificationAsync();
         Console.ReadLine();

##<a name="test-your-app"></a>Testowanie aplikacji

Aby przetestować tę aplikację z rzeczywisty telefonu, po prostu Podłącz telefon do komputera za pomocą kabla USB. Służy do ładowania aplikacji na dołączonym telefonu.

Aby przetestować tę aplikację emulatorze na górnym pasku Zaćmienie kliknij polecenie **Uruchom**, a następnie wybierz aplikację. Zostanie uruchomiony emulator i ładuje i uruchamia aplikację.

Aplikacja pobiera "Nazwa użytkownika" i "channelId" z Usługa powiadomień Baidu Push i rejestruje Centrum powiadomienie.

Aby wysłać powiadomienie test służy kartę debugowania Portal klasyczny Azure. Jeśli utworzono aplikacji konsoli programu .NET dla programu Visual Studio, wystarczy nacisnąć klawisz F5 w programie Visual Studio, aby uruchomić aplikację. Aplikacja będzie wysyłać powiadomienie, które będą wyświetlane w obszarze powiadomień górny urządzenia lub emulatora.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Usług mobilnych SDK systemu Android]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu wypychanych SDK systemu Android]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Portal Azure klasyczny]: https://manage.windowsazure.com/
[Baidu portal]: http://www.baidu.com/
