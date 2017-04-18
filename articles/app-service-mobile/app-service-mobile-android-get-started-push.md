<properties
    pageTitle="Dodawanie powiadomienia wypychane do Android aplikacji z Azure aplikacji dla urządzeń przenośnych"
    description="Dowiedz się, jak używać aplikacji Mobile Azure do wysyłania powiadomień wypychanych do Android aplikacji."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Dodawanie powiadomienia wypychane do aplikacji systemu Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Omówienie
W tym samouczku możesz dodać powiadomienia wypychane projektu [Android szybki start] , tak, aby wysłać powiadomienia wypychane na urządzeniu każdorazowo po wstawieniu rekordu.

Jeśli nie korzystasz z programu project server pobrany szybki start, konieczne jest pakiet rozszerzenia powiadomień wypychanych. Aby uzyskać więcej informacji zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Wymagania wstępne

Są potrzebne następujące elementy:

* IDE w zależności od projektu wewnętrznej bazy danych:

    * [Android Studio](https://developer.android.com/sdk/index.html) , jeśli ta aplikacja ma Node.js wewnętrznej bazie danych.

    * [Visual Studio społeczności 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) lub później, jeśli ta aplikacja ma wewnętrznej bazie danych programu .net.

* Android 2.3 lub nowszy, poprawki repozytorium Google 27 lub nowszy oraz usługi Google Play 9.0.2 lub nowszej Firebase chmury wiadomości.

* Wykonaj [Szybki start dla systemu Android].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Tworzenie projektu obsługującego Firebase chmury wiadomości

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Konfigurowanie koncentratora powiadomień

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurowanie Azure do wysyłania powiadomień wypychanych

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Włączanie powiadomień wypychanych dla programu project server

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Dodawanie powiadomień wypychanych do aplikacji

W tej sekcji możesz zaktualizować klienta Android aplikacji do obsługi powiadomienia wypychane.

### <a name="verify-android-sdk-version"></a>Sprawdzić wersję SDK systemu Android

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Następnym krokiem jest zainstalowanie usługi Google Play. Google Cloud wiadomości ma pewne interfejsu API poziomu wymagania minimalne rozwoju i badania, które właściwości **minSdkVersion** w manifeście muszą być zgodne z.

Jeśli testujesz starsze urządzenia, zwróć się [Ustawić konto Google odtwarzanie usługi SDK] można sprawdzić, jak małych można ustawić tę wartość i odpowiednio ustawić.

### <a name="add-google-play-services-to-the-project"></a>Dodawanie usługi Google Play do projektu

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Dodawanie kodu

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Testowanie aplikacji przed opublikowanych usług mobilnych

Istnieje możliwość przetestowania aplikacji, dołączając bezpośrednio telefonie z systemem Android za pomocą kabla USB lub przy użyciu urządzenia wirtualne w emulatorze.

## <a name="more"></a>Więcej

<!-- URLs -->
[Szybki start dla systemu android]: app-service-mobile-android-get-started.md

[Konfigurowanie usługi Google Play SDK]:https://developers.google.com/android/guides/setup
