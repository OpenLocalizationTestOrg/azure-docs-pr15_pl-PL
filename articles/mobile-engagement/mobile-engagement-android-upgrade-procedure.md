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


#<a name="upgrade-procedures"></a>Procedury uaktualniania

Jeśli już masz zintegrowany ze starszej wersji programu naszych SDK do aplikacji, należy wziąć pod uwagę następujące punkty podczas uaktualniania zestawu SDK.

Być może trzeba wykonać kilka procedury nie odebrano kilka wersji pakietu. Na przykład możesz przeprowadzić migrację z 1.4.0 do 1.6.0, musisz najpierw należy wykonać procedurę "od 1.4.0 do 1.5.0" następnie procedury "od 1.5.0 do 1.6.0".

Niezależnie od wersji uaktualnienie, należy zastąpić `mobile-engagement-VERSION.jar` na nową.

##<a name="from-420-to-421"></a>Z 4.2.0 do 4.2.1

Ten krok faktycznie można przeprowadzić dowolna wersja zestawu SDK, jest bezpieczniejsze integracja Reach działania.

Teraz należy dodać `exported="false"` do wszystkich działań Reach.

Działania reach powinna wyglądać podobnie to Twojej `AndroidManifest.xml`:

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

##<a name="from-400-to-410"></a>Z 4.0.0 do 4.1.0

Zestaw SDK teraz uchwyt nowego uprawnień modelu z systemem Android M.

Jeśli używasz funkcji lokalizacji lub powiadomienia przeglądu przeczytaj [tę sekcję](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Oprócz nowy model uprawnień obsługujemy są teraz Konfigurowanie funkcji lokalizacji w czasie rzeczywistym.
Firma Microsoft są nadal zgodne z parametrami manifestu dla lokalizacji, ale jest teraz przestarzałe. Aby użyć konfiguracji środowisko uruchomieniowe, Usuń następujące sekcje z Twojej ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

i przeczytaj [tej procedury zaktualizowane](mobile-engagement-android-integrate-engagement.md#location-reporting) , aby zamiast tego użyj środowisko uruchomieniowe konfiguracji.

##<a name="from-300-to-400"></a>Z 3.0.0 do 4.0.0

### <a name="native-push"></a>Natywne push

Natywne wypychanych (GCM-ADM) teraz służy do w powiadomienia aplikacji, musisz skonfigurować poświadczenia natywnych wypychanych dla każdego typu wypychanych kampanii.

Jeśli jeszcze nie należy wykonaj [poniższą procedurę](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Integracja reach został zmodyfikowany w ``AndroidManifest.xml``.

Zastąp, to:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Według

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
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

Jest prawdopodobnie na ekranie ładowania teraz po kliknięciu ogłoszenie (z tekstu i zawartości sieci web) lub ankiety.
Należy dodać ten formularz tych kampanii Praca w 4.0.0:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Zasoby

Osadzanie nowy `res/layout/engagement_loading.xml` plików do projektu.

##<a name="from-240-to-300"></a>Z 2.4.0 do 3.0.0

Poniżej opisano, jak przeprowadzić migrację SDK integracji z usługi Capptain oferowanych przez skojarzenia zabezpieczeń Capptain do aplikacji korzystająca z zaangażowania Mobile Azure. Jeśli są migrowane z wcześniejszej wersji, zajrzyj do witryny sieci web Capptain, aby przeprowadzić migrację do 2.4.0, a następnie Zastosuj poniższą procedurę.

>[AZURE.IMPORTANT] Capptain i zaangażowania Mobile nie są takie same usługi i procedury przedstawionej poniżej tylko wyróżnia jak przeprowadzić migrację aplikacji klienta. Migrowanie SDK w aplikacji będą migrowane danych z serwerów Capptain się z serwerami zaangażowania Mobile.

### <a name="jar-file"></a>Plik SŁOIK

Zamienianie `capptain.jar` przez `mobile-engagement-VERSION.jar` w swojej `libs` folder.

### <a name="resource-files"></a>Pliki zasobów

Każdy plik zasobu, który możemy przewidzieć (poprzedzone `capptain_`) ma zostać zastąpiona nowych plików (prefiksem `engagement_`).

Po dostosowaniu te pliki, musisz ponownie zastosować dostosowań na nowe pliki, **również zmieniono identyfikatorów plików zasobów**.

### <a name="application-id"></a>Identyfikator aplikacji

Teraz zaangażowania umożliwia konfigurowanie identyfikatorów SDK, takie jak identyfikator aplikacji parametry połączenia.

Należy użyć `EngagementAgent.init` metody działalność uruchamiania w następujący sposób:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Parametry połączenia dla aplikacji jest wyświetlany na Azure Portal.

Usuń wszelkie połączenia do `CapptainAgent.configure` jako `EngagementAgent.init` powoduje zastąpienie tej metody.

`appId` Już nie można skonfigurować przy użyciu `AndroidManifest.xml`.

Usuń w tej sekcji z usługi `AndroidManifest.xml` Jeśli jest:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Interfejs API języka Java

Co połączenia dla każdej klasy języka Java naszych SDK musi ulec zmianie; na przykład `CapptainAgent.getInstance(this)` należy zmienić nazwę `EngagementAgent.getInstance(this)`, `extends CapptainActivity` należy zmienić nazwę `extends EngagementActivity` itp.

Jeśli zostały zintegrowany z plików preferencji agenta domyślne, domyślną nazwę pliku jest teraz `engagement.agent` i kluczem jest `engagement:agent`.

Podczas tworzenia ogłoszenia sieci web, spinacza Javascript jest teraz `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Licznych zmian stało, usługa nie jest już udostępniony i wielu odbiorców nie są już można eksportować.

Deklaracja usługi są teraz prostsze; Usuwanie filtru konwersji i wszystkie metadane w nim, a następnie dodaj `exportable=false`.

Ponadto wszystko jest zmieniana na używanie zaangażowania.

Teraz wygląda:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Aby włączyć dzienniki test, należy dane meta teraz został przeniesiony do niego znacznik aplikacji i została zmieniona:

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

Po prostu zmieniono wszystkie inne metadane, poniżej przedstawiono pełną listę (Zmień nazwę kursu tylko te, których używasz):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play i SmartAd śledzenia został usunięty z zestawu SDK, wystarczy usunąć ten bez zastępowania:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

Działania Reach są teraz deklarowany tak jak poniżej:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
Jeśli masz niestandardowe działania Reach, należy tylko zmienić konwersji akcji, które mają pasujące do jednego `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` lub `com.microsoft.azure.engagement.reach.intent.action.POLL`.

Zmieniono emisji odbiorców, a także teraz dodawać `exported=false`. Poniżej przedstawiono pełną listę odbiorców z nowych specyfikacji, (Zmień nazwę kursu tylko te, których używasz):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Śledzenie słuchawka zostały usunięte, więc musisz usunąć w tej sekcji:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Należy zauważyć, że deklaracji implementacji emisji odbiorcy **EngagementMessageReceiver** zmienił się w `AndroidManifest.xml`. Jest tak, ponieważ zostały usunięte z interfejsu API do wysyłania i odbierania wiadomości między urządzeniami i interfejsu API do wysyłania i usunąć dowolnego wiadomości XMPP z dowolnego jednostek XMPP. W związku z tym należy również usunąć następujące zwrotne z implementacji **EngagementMessageReceiver** :

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

oraz

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

następnie usuń wszystkie połączenia na **EngagementAgent** dla:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

oraz

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard

Proguard konfiguracji mogą mieć negatywny wpływ na przez rebranding, takich jak teraz szukasz reguł:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
