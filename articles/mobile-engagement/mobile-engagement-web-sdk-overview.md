<properties
    pageTitle="Omówienie SDK sieci Web Azure zaangażowania przenośnego | Microsoft Azure"
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
    ms.date="10/18/2016"
    ms.author="piyushjo" />


# <a name="azure-mobile-engagement-web-sdk"></a>Azure zaangażowania przenośnych Web SDK

Rozpocznij tutaj, szczegółowe informacje o integrowaniu zaangażowania Mobile Azure w aplikacji sieci web. Jeśli chcesz spróbuj przed wprowadzenie do aplikacji sieci web, zobacz nasz [Samouczek 15-minutowe](mobile-engagement-web-app-get-started.md).

## <a name="integration-procedures"></a>Procedury integracji
1. Dowiedz się, [jak zintegrować zaangażowania Mobile w aplikacji sieci web](mobile-engagement-web-integrate-engagement.md).

2. Znacznik plan wykonania Dowiedz się, [jak za pomocą zaawansowanych zaangażowania Mobile znakowania interfejsu API w aplikacji sieci web](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Informacje o wersji

### <a name="202-10182016"></a>2.0.2 (2016-10-18)

-   Stały awarię na przeglądanie w trybie prywatności (Safari).
-   Stały awarii w przeglądarkach plików cookie wyłączone.

Dla wszystkich wersji, zobacz [informacje o wersji wykonania](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Procedury uaktualniania

### <a name="upgrade-from-121-to-200"></a>Uaktualnianie z 1.2.1 do 2.0.0

W poniższych sekcjach opisano, jak przeprowadzić migrację integracji SDK Web zaangażowania Mobile w usłudze Capptain oferowanych przez skojarzeń zabezpieczeń Capptain aplikację zaangażowania Mobile Azure. Podczas migrowania z wersji wcześniejszej niż 1.2.1, można znaleźć w sieci Web Capptain, aby przeprowadzić migrację do 1.2.1, a następnie Zastosuj poniższych procedur.

Ta wersja zestawu SDK Web zaangażowania Mobile nie obsługuje telewizji inteligentne Samsung, telewizji Opera, webOS lub funkcji Reach.

>[AZURE.IMPORTANT] Capptain i zaangażowania Mobile Azure są tej samej usługi i procedur wyróżnianie tylko jak przeprowadzić migrację aplikacji klienta. Migrowanie SDK Mobile zaangażowania sieci Web w aplikacji będą migrowane danych z serwera Capptain na serwerze zaangażowania Mobile.

#### <a name="javascript-files"></a>Pliki JavaScript

Zamień plik azure-engagement.js pliku capptain-sdk.js, a następnie zaktualizuj odpowiednio do importowanie skrypt.

#### <a name="remove-capptain-reach"></a>Usuwanie Capptain Reach

Ta wersja zestawu SDK Mobile zaangażowania sieci Web nie obsługuje funkcji Reach. Jeśli jest zintegrowany Capptain Reach do aplikacji, należy ją usunąć.

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

#### <a name="remove-deprecated-apis"></a>Usuń przestarzałe interfejsy API

Niektóre funkcje interfejsu API z Capptain jest niedostępny w SDK Web zaangażowania Mobile.

Usuń wszelkie wywołania API: `agent.connect`, `agent.disconnect`, `agent.pause`, i `agent.sendMessageToDevice`.

Usuwanie dowolnego z następujących zwrotne z konfiguracji Capptain: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, i `onPushMessageReceived`.

#### <a name="configuration"></a>Konfiguracja

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

Parametry połączenia dla aplikacji jest wyświetlana w portalu Azure.

#### <a name="javascript-apis"></a>Interfejsy API języka JavaScript

Globalny obiektu JavaScript `window.capptain` została zmieniona `window.azureEngagement`, ale możesz użyć `window.engagement` alias dla interfejsu API. Za pomocą tego aliasu nie można określić konfigurację SDK.

Na przykład `capptain.deviceId` staje się `engagement.deviceId`, `capptain.agent.startActivity` staje się `engagement.agent.startActivity`i tak dalej.

Jeśli masz już zintegrowany starszej wersji programu SDK Web zaangażowania Mobile Azure do aplikacji, przeczytaj informacje o [uaktualnieniu procedur](mobile-engagement-web-upgrade-procedure.md).
