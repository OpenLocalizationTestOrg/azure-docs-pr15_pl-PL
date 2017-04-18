<properties
    pageTitle="Azure procedury uaktualniania SDK Web zaangażowania Mobile | Microsoft Azure"
    description="Najnowsze aktualizacje i procedury SDK sieci Web dla zaangażowania Mobile Azure"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Azure procedury uaktualniania SDK Web zaangażowania Mobile

Jeśli masz już zintegrowany starszej wersji programu SDK Web zaangażowania Mobile Azure w aplikacji sieci web, należy wziąć pod uwagę następujące punkty po uaktualnieniu zestawu SDK.

Jeśli została pominięta kilku wersji pakietu SDK Web zaangażowania Mobile, może być konieczne wykonywanie kilku procedur w trakcie uaktualniania. Na przykład jeśli migrację z 1.4.0 do 1.6.0, najpierw wykonać procedury uaktualnienia z 1.4.0 do 1.5.0. Następnie wykonaj procedury uaktualnienia z 1.5.0 do 1.6.0.

Niezależnie od wersji uaktualnienie, Zamień dowolnej starszej wersji pliku azure-engagement.js z najnowszą wersją pliku.

## <a name="upgrade-from-121-to-200"></a>Uaktualnianie z 1.2.1 do 2.0.0

W tej sekcji opisano, jak przeprowadzić migrację integracji SDK Web zaangażowania Mobile w usłudze Capptain oferowanych przez skojarzeń zabezpieczeń Capptain aplikację zaangażowania Mobile Azure. Jeśli są migrowane z wcześniejszej wersji, zapoznaj się najpierw przeprowadzić migrację do 1.2.1 witrynie sieci Web Capptain, a następnie Zastosuj poniższych procedur.

Ta wersja zestawu SDK Web zaangażowania Mobile nie obsługuje telewizji inteligentne Samsung, telewizji Opera, webOS lub funkcji Reach.

>[AZURE.IMPORTANT] Capptain i zaangażowania Mobile Azure są tej samej usługi. Poniższa procedura umożliwia wyróżnienie tylko jak przeprowadzić migrację aplikacji klienta. Migrowanie SDK Mobile zaangażowania sieci Web w aplikacji będą migrowane danych z serwera Capptain na serwerze zaangażowania Mobile.

### <a name="javascript-files"></a>Pliki JavaScript

Zamień plik azure-engagement.js pliku capptain-sdk.js, a następnie zaktualizuj odpowiednio do importowanie skrypt.

### <a name="remove-capptain-reach"></a>Usuwanie Capptain Reach

Ta wersja zestawu SDK Mobile zaangażowania sieci Web nie obsługuje funkcji Reach. Jeśli Capptain Reach zintegrowany aplikacji, należy ją usunąć.

Usuwanie importowanie osiągnięcia CSS z Twojej strony i usuń plik CSS powiązanych (capptain-reach.css, domyślnie).

Usuń następujące zasoby Reach: Zamknij obraz (capptain-close.png, domyślnie) i ikona marki (capptain powiadomienie ikony, domyślnie).

Usuwanie osiągnięcia interfejsu użytkownika w aplikacji app powiadomienia. Domyślny układ wygląda następująco:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Usuwanie osiągnięcia interfejsu użytkownika dla tekstu i anonsów sieci web i ankiety. Domyślny układ wygląda następująco:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Usuwanie `reach` obiekt z konfiguracji, jeśli istnieje. Wygląda następująco:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Usuń wszelkie inne dostosowania Reach, takich jak kategorie.

### <a name="remove-deprecated-apis"></a>Usuń przestarzałe interfejsy API

Niektóre funkcje interfejsu API z Capptain jest niedostępny w SDK Web zaangażowania Mobile.

Usuń wszelkie wywołania API: `agent.connect`, `agent.disconnect`, `agent.pause`, i `agent.sendMessageToDevice`.

Usuń wszystkie wystąpienia następujących zwrotne w konfiguracji Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, i `onPushMessageReceived`.

### <a name="configuration"></a>Konfiguracja

Telefon komórkowy zaangażowania używa parametrów połączenia do konfigurowania SDK identyfikatorów, na przykład identyfikator aplikacji.

Identyfikator aplikacji należy zastąpić ciąg połączenia. Należy zauważyć, że obiekt globalny konfiguracji SDK zmian ze `capptain` do `azureEngagement`.

Przed migracją:

    window.capptain = {
      appId: ...,
      [...]
    };

Po zakończeniu migracji:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

Parametry połączenia dla aplikacji jest wyświetlany w Azure Portal.

### <a name="javascript-apis"></a>Interfejsy API języka JavaScript

Obiekt globalny JavaScript `window.capptain` została zmieniona `window.azureEngagement` , ale możesz użyć `window.engagement` alias dla interfejsu API. Alias nie umożliwia określenie konfiguracji SDK.

Na przykład `capptain.deviceId` staje się `engagement.deviceId`, `capptain.agent.startActivity` staje się `engagement.agent.startActivity`i tak dalej.
