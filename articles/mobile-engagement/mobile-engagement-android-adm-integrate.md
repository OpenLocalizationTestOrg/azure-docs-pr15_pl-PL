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


#<a name="how-to-integrate-adm-with-engagement"></a>Jak zintegrować ADM z zaangażowania

> [AZURE.IMPORTANT] Należy wykonać integracji procedurę opisaną w sekcji jak zintegrować zaangażowania Android dokumentu przed wykonaniem tego przewodnika.
>
> Ten dokument jest przydatna, tylko wtedy, gdy już zintegrowany moduł Reach i zamierzasz push Amazon urządzenia. Aby zintegrować kampanii Reach w aplikacji, należy przeczytać najpierw integracja zaangażowania adres kontaktowy w systemie Android.

##<a name="introduction"></a>Wprowadzenie

Integrowanie ADM umożliwia aplikacji mają zostać przeniesiony przy użyciu funkcji określania docelowych urządzeń z systemem Android Amazon.

Ładunki ADM przypisany do zestawu SDK zawsze zawierają `azme` klucza w obiekcie danych. A więc użycie ADM do innych celów w aplikacji, można filtrować według tego klawisza umieszcza.

> [AZURE.IMPORTANT] Tylko Amazon Kindle urządzeniach z systemem Android 4.0.3 lub powyżej są obsługiwane przez Amazon urządzenia wiadomości; jednak można zintegrować kod bezpiecznie na innych urządzeniach.

##<a name="sign-up-to-adm"></a>Zamów ADM

Jeśli tego nie zrobiono, należy włączyć ADM konta Amazon.

Procedury jest szczegółowe u: [<https://developer.amazon.com/sdk/adm/credentials.html>].

Po wykonaniu procedury, zostanie wyświetlony komunikat:

-   OAuth poświadczenia (identyfikator klienta i tajny klienta) zaangażowania można było push urządzenia.
-   Klucz interfejsu API, które muszą zostać włączone do aplikacji.

##<a name="sdk-integration"></a>Integracja SDK

### <a name="managing-device-registrations"></a>Zarządzanie rejestracji urządzenia

Każde urządzenie musi wysłać polecenie rejestracji się z serwerami ADM, w przeciwnym razie nie jest to możliwe.

Jeśli już używać [Biblioteka klienta ADM]i już [zintegrowane ADM] możesz bezpośrednio android-sdk-adm-receive.

Jeśli nie masz jeszcze zintegrowane ADM, zaangażowania występują prostszy sposób włączyć ją w aplikacji:

Edytowanie swojego `AndroidManifest.xml` pliku:

-   Dodawanie nazw Amazon plik powinna rozpoczynać tak jak poniżej:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   Wewnątrz `<application/>` znakowanie, Dodaj w tej sekcji:

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>

        <meta-data android:name="engagement:adm:register" android:value="true" />

-   Po dodaniu znaczników amazon, jeśli docelowy Tworzenie projektu jest poniżej Android 2.1 może być błąd kompilacji. Musisz użyć docelowej kompilacji **Android 2.1 +** (nie martw się, czy nadal masz `minSdkVersion` równa 4).
-   Integracja klucz interfejsu API ADM jako środka trwałego za następującą [tej procedury].

Następnie postępuj zgodnie z instrukcjami w następnej sekcji.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Komunikowanie się identyfikator rejestru z usługą Push zaangażowania i otrzymywać powiadomienia

Aby komunikować się identyfikator rejestracji urządzenia z usługą Push zaangażowania i jego powiadomień, Dodaj poniższe czynności, aby usługi `AndroidManifest.xml` plików wewnątrz `<application/>` znakowanie (nawet gdy używasz ADM bez zaangażowania):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Upewnij się, masz następujące uprawnienia w swojej `AndroidManifest.xml` (przed `</application>` znacznika).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##<a name="grant-engagement-oauth-credentials"></a>Udzielanie zaangażowania OAuth poświadczeń

Przesyłanie OAuth poświadczeń (identyfikator klienta i tajny klienta) w portalu zaangażowania.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[Biblioteka klienta ADM]:https://developer.amazon.com/sdk/adm/setup.html
[zintegrowane ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[Ta procedura]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
