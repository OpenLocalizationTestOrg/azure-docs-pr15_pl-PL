<properties
    pageTitle="Integracja Android SDK Azure zaangażowania urządzeń przenośnych"
    description="Najnowszych aktualizacji i procedury dla systemu Android SDK dla zaangażowania Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-gcm-with-mobile-engagement"></a>Jak zintegrować GCM przy użyciu urządzeń przenośnych zaangażowania

> [AZURE.IMPORTANT] Należy wykonać integracji procedurę opisaną w sekcji jak zintegrować zaangażowania Android dokumentu przed wykonaniem tego przewodnika.
>
> Ten dokument jest przydatna, tylko wtedy, gdy już zintegrowany moduł Reach i zamierzasz push urządzeń Google Play. Aby zintegrować kampanii Reach w aplikacji, należy przeczytać najpierw integracja zaangażowania adres kontaktowy w systemie Android.

##<a name="introduction"></a>Wprowadzenie

Integrowanie GCM umożliwia aplikacji mają zostać przeniesiony.

Ładunki GCM przypisany do zestawu SDK zawsze zawierają `azme` klucza w obiekcie danych. A więc użycie GCM do innych celów w aplikacji, można filtrować według tego klawisza umieszcza.

> [AZURE.IMPORTANT] Tylko urządzenia z systemem Android 2.2 lub powyżej, o Google Play zainstalowane i konieczności Google połączenie tła włączona może być przy użyciu GCM; jednak można zintegrować kod bezpiecznie na urządzeniach nieobsługiwane (tylko używa opcji).

##<a name="create-a-google-cloud-messaging-project-with-api-key"></a>Tworzenie projektu Google Cloud wiadomości z klucz interfejsu API

[AZURE.INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

##<a name="sdk-integration"></a>Integracja SDK

### <a name="managing-device-registrations"></a>Zarządzanie rejestracji urządzenia

Każde urządzenie musi wysłać polecenie rejestracji się z serwerami Google, w przeciwnym razie nie jest to możliwe.

Urządzenia mogą również unregister subskrypcję powiadomień GCM (urządzenie jest automatycznie zarejestrowany w przypadku odinstalowania aplikacji).

Jeśli nie korzystasz z [Usługi Google odtwarzanie SDK] lub nie już wysyłania przeznaczenie rejestracji samodzielnie, można tworzyć zaangażowania rejestruje urządzenie automatycznie dla Ciebie.

Aby włączyć tę opcję, Dodaj poniższe czynności, aby usługi `AndroidManifest.xml` plików wewnątrz `<application/>` znacznika:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Komunikowanie się identyfikator rejestru z usługą Push zaangażowania i otrzymywać powiadomienia

Aby komunikować się identyfikator rejestracji urządzenia z usługą Push zaangażowania i jego powiadomień, Dodaj poniższe czynności, aby usługi `AndroidManifest.xml` plików wewnątrz `<application/>` znakowanie (nawet gdy zarządzać rejestracji urządzenia, samodzielnie):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Upewnij się, masz następujące uprawnienia w swojej `AndroidManifest.xml` (po `</application>` znacznika).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##<a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Udzielanie dostępu zaangażowania urządzeń przenośnych do klucz interfejsu API GCM

Postępuj zgodnie z [tego przewodnika](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) do udostępnienia zaangażowania Mobile klucz interfejsu API GCM.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
