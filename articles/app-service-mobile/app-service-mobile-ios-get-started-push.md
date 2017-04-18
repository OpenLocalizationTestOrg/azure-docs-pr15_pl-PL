<properties
    pageTitle="Dodawanie powiadomienia Push w systemie iOS aplikacji z aplikacji Mobile Azure"
    description="Dowiedz się, jak używać aplikacji Mobile Azure do wysyłania powiadomień wypychanych do aplikacji w systemie iOS."
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>Dodawanie powiadomienia Push w swojej aplikacji w systemie iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Omówienie
W tym samouczku możesz dodać powiadomienia wypychane [iOS Szybkie rozpoczynanie] projektu, tak, aby wysłać powiadomienia wypychane na urządzeniu każdorazowo po wstawieniu rekordu.

Jeśli nie korzystasz z programu project server pobrany szybki start, konieczne będzie pakiet rozszerzenia powiadomień push. Aby uzyskać więcej informacji, zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

[IOS simulator nie obsługuje powiadomienia wypychane](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Potrzebujesz urządzeniu fizycznie iOS i [członkostwa w programie Deweloper firmy Apple](https://developer.apple.com/programs/ios/).

##<a name="configure-hub"></a>Konfigurowanie powiadomień Centrum

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Zarejestruj się w aplikacji dla powiadomienia wypychane

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurowanie Azure do wysyłania powiadomień wypychanych

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>Aktualizowanie wewnętrznej bazy danych do wysyłania powiadomień wypychanych

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Dodawanie powiadomień wypychanych do aplikacji

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Powiadomienia wypychane test

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>Więcej

* Szablony umożliwiają możliwość wysyłania umieszcza i platform i umieszcza zlokalizowanym. [Jak iOS Użyj Biblioteka klienta dla aplikacji Mobile Azure](app-service-mobile-ios-how-to-use-client-library.md#templates) pokazano, jak zarejestrować szablonów.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[Szybki start systemu iOS]: app-service-mobile-ios-get-started.md
