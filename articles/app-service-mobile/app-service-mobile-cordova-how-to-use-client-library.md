<properties
    pageTitle="Jak używać Apache Cordova dodatek do Azure aplikacji dla urządzeń przenośnych"
    description="Jak używać Apache Cordova dodatek do Azure aplikacji dla urządzeń przenośnych"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-apache-cordova-client-library-for-azure-mobile-apps"></a>Jak używać biblioteki klienta Cordova Apache dla Azure aplikacji dla urządzeń przenośnych

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ten przewodnik przedstawiono wykonywanie typowych scenariuszy przy użyciu najnowszych [Apache Cordova wtyczkę w przypadku aplikacji Mobile Azure]. Jeśli jesteś nowym użytkownikiem aplikacji Mobile Azure, pierwszego wykonania [Azure Mobile aplikacje Szybki Start] tworzenie wewnętrznej bazie danych, utworzyć tabelę, a pobieranie gotowych projektu Apache Cordova. W tym przewodniku możemy skupić się na tę wtyczkę Cordova Apache klienta.

## <a name="supported-platforms"></a>Platformy obsługiwane

Zestaw SDK obsługuje v6.0.0 Apache Cordova i później iOS, Android i Windows urządzenia.  Obsługa platformy jest następująca:

* Interfejs API systemu android (KitKat za pośrednictwem nugacie) 19-24
* iOS w wersji 8.0 lub nowszej.
* Windows Phone 8.0
* Windows Phone 8.1
* Platformy Windows uniwersalny

##<a name="Setup"></a>Instalacja i wymagania wstępne

Ten przewodnik przyjęto założenie, że utworzono wewnętrznej bazie danych z tabeli. Ten przewodnik przyjęto założenie, że tabela zawiera ten sam schemat jako tabele w tych samouczkach. Ten przewodnik również założono, że zostało dodane dodatek Cordova Apache w kodzie.  Jeśli użytkownik nie zostało zrobione, możesz dodać wtyczkę Apache Cordova do projektu w wierszu polecenia:

```
cordova plugin add cordova-plugin-ms-azure-mobile-apps
```

Aby uzyskać więcej informacji na temat tworzenia [pierwszej aplikacji Apache Cordova]zobacz dokumentację.

[AZURE.INCLUDE [app-service-mobile-html-js-library.md](../../includes/app-service-mobile-html-js-library.md)]


##<a name="auth"></a>Jak: uwierzytelniania użytkowników

Azure aplikacji usługi obsługuje uwierzytelnianie i autoryzowanie użytkowników aplikacji przy użyciu różnych dostawcy tożsamości zewnętrzne: Facebook, Google, Account Microsoft i Twitter. Można ustawić uprawnienia w tabelach w celu ograniczenia dostępu dla określonych operacje, aby tylko uwierzytelnieni użytkownicy. Za pomocą tożsamości uwierzytelnionych użytkowników do wykonania reguł autoryzacji w skrypty serwera. Aby uzyskać więcej informacji zobacz samouczek [Wprowadzenie do uwierzytelniania] .

Gdy używasz uwierzytelniania aplikacji Apache Cordova, następujące dodatki Cordova muszą być dostępne:

* [cordova dodatek urządzeń]
* [cordova — dodatek inappbrowser]

Obsługiwane są dwie przebiegu uwierzytelniania: serwer i przepływ klienta.  Przepływ server zapewnia najłatwiejszym obsługę uwierzytelniania, zależy od interfejs uwierzytelniania sieci web dostawcy. Przepływ klienta umożliwia lepsza integracja możliwości specyficzne dla urządzenia z takich jak-jednokrotnej jako zależy od SDK specyficzne dla urządzenia specyficzne dla dostawcy.

[AZURE.INCLUDE [app-service-mobile-html-js-auth-library.md](../../includes/app-service-mobile-html-js-auth-library.md)]

###<a name="configure-external-redirect-urls"></a>Jak: Konfigurowanie urządzeń przenośnych usługi aplikacji dla zewnętrznego przekierowywania adresów URL.

Kilka typów aplikacji Apache Cordova za pomocą funkcji sprzężenia zwrotnego obsługiwać przepływy OAuth interfejsu użytkownika.  Interfejs użytkownika OAuth przepływów w hosta lokalnego spowodować problemy, ponieważ usługa uwierzytelniania tylko wie, jak korzystać z usługą domyślnie.  Przykłady problematyczny przepływów OAuth interfejsu użytkownika:

- Emulator wpływu.
- Załaduj ponownie z jonowa na żywo.
- Uruchomienie lokalnie przenośnych wewnętrznej bazy danych
- W innej aplikacji usługi Azure niż jeden uwierzytelniania przekazywanie uruchomiona przenośnych wewnętrznej bazy danych.

Wykonaj te instrukcje, aby dodać ustawienia lokalne konfiguracji:

1. Zaloguj się do [portalu Azure]
2. Zaznacz **wszystkie zasoby** lub **Usług aplikacji** , a następnie kliknij nazwę aplikacji Mobile.
3. Kliknij menu **Narzędzia**
4. W OBSERVE menu kliknij polecenie **Eksploratora zasobów** , a następnie kliknij przycisk **Przejdź**.  Zostanie otwarte nowe okno lub tab.
5. Rozwijanie **konfiguracji**, węzły **authsettings** witryny w obszarze nawigacji po lewej.
6. Kliknij przycisk **Edytuj**
7. Poszukaj elementu "allowedExternalRedirectUrls".  Może zostać ustawiony na wartość null lub tablicę wartości.  Zmień wartość na następującą wartość:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Zamień adresy URL adresy URL usługi.  Jako przykład można wymienić "http://localhost:3000" (usługi Node.js próbki) lub "http://localhost:4400" (w przypadku usługi wpływu).  Jednak te adresy URL są przykłady - danej sytuacji, w tym dla usług wymienionych w przykładach mogą się różnić.
8. Kliknij przycisk **Odczytu/zapisu** w prawym górnym rogu ekranu.
9. Kliknij zielony przycisk **Umieść** .

Ustawienia są zapisywane w tym momencie.  Nie zamykaj okna przeglądarki, aż do zakończenia ustawienia zapisywania.
Również dodać te adresy URL sprzężenia zwrotnego w ustawieniach CORS usługi aplikacji:

1. Zaloguj się do [portalu Azure]
2. Zaznacz **wszystkie zasoby** lub **Usług aplikacji** , a następnie kliknij nazwę aplikacji Mobile.
3. Karta Ustawienia jest otwierany automatycznie.  W przeciwnym razie kliknij pozycję **Wszystkie ustawienia**.
4. Kliknij pozycję **CORS** w menu interfejsu API.
5. Wpisz adres URL, który chcesz dodać w polu dostępne i naciśnij klawisz Enter.
6. Wprowadź dodatkowe adresy URL, stosownie do potrzeb.
7. Kliknij przycisk **Zapisz** , aby zapisać ustawienia.

Trwa około 10 do 15 sekund nowe ustawienia zostały wprowadzone.

##<a name="register-for-push"></a>Jak: rejestr powiadomienia wypychane

Zainstaluj [phonegap — dodatek wypychanych] obsługę powiadomienia wypychane.  Ten dodatek można łatwo dodawać przy użyciu `cordova plugin add` polecenia w wierszu polecenia lub za pośrednictwem Instalatora dodatek cyfra w programie Visual Studio.  Poniższy kod w aplikacji Apache Cordova rejestruje urządzenia w celu powiadomienia wypychane:

```
var pushOptions = {
    android: {
        senderId: '<from-gcm-console>'
    },
    ios: {
        alert: true,
        badge: true,
        sound: true
    },
    windows: {
    }
};
pushHandler = PushNotification.init(pushOptions);

pushHandler.on('registration', function (data) {
    registrationId = data.registrationId;
    // For cross-platform, you can use the device plugin to determine the device
    // Best is to use device.platform
    var name = 'gcm'; // For android - default
    if (device.platform.toLowerCase() === 'ios')
        name = 'apns';
    if (device.platform.toLowerCase().substring(0, 3) === 'win')
        name = 'wns';
    client.push.register(name, registrationId);
});

pushHandler.on('notification', function (data) {
    // data is an object and is whatever is sent by the PNS - check the format
    // for your particular PNS
});

pushHandler.on('error', function (error) {
    // Handle errors
});
```

Za pomocą SDK koncentratory powiadomień do wysyłania powiadomień wypychanych z serwera.  Nigdy nie wysyłaj powiadomienia wypychane bezpośrednio z klientami. Wykonanie tej czynności mogą służyć wyzwalać odmowa usługi atakiem przed koncentratory powiadomienie lub obwodowym układzie Nerwowym.  Obwodowym układzie Nerwowym może zakaz ruchu w wyniku takich atakami.

<!-- URLs. -->
[Azure portal]: https://portal.azure.com
[Azure urządzeń przenośnych aplikacji Szybki Start]: app-service-mobile-cordova-get-started.md
[Wprowadzenie do uwierzytelniania]: app-service-mobile-cordova-get-started-users.md
[Add authentication to your app]: app-service-mobile-cordova-get-started-users.md

[Dodatek Cordova Apache dla Azure aplikacji dla urządzeń przenośnych]: https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-apps
[pierwszy aplikacji Apache Cordova]: http://cordova.apache.org/#getstarted
[phonegap-facebook-plugin]: https://github.com/wizcorp/phonegap-facebook-plugin
[phonegap — dodatek push]: https://www.npmjs.com/package/phonegap-plugin-push
[cordova dodatek urządzeń]: https://www.npmjs.com/package/cordova-plugin-device
[cordova — dodatek inappbrowser]: https://www.npmjs.com/package/cordova-plugin-inappbrowser
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx
