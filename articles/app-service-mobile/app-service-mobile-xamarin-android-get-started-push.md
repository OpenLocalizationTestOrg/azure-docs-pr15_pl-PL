<properties
    pageTitle="Dodawanie powiadomień wypychanych do aplikacji Xamarin.Android | Azure aplikacji usługi"
    description="Dowiedz się, jak używać Azure aplikacji usługi i koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych do aplikacji Xamarin.Android"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Dodawanie powiadomienia wypychane do aplikacji Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Omówienie


W tym samouczku możesz dodać powiadomienia wypychane [Xamarin.Android Szybkie rozpoczynanie](app-service-mobile-windows-store-dotnet-get-started.md) projektu, tak, aby wysłać powiadomienia wypychane na urządzeniu każdorazowo po wstawieniu rekordu.

Jeśli nie korzystasz z programu project server pobrany szybki start, konieczne będzie pakiet rozszerzenia powiadomień wypychanych. Aby uzyskać więcej informacji, zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .


##<a name="prerequisites"></a>Wymagania wstępne

Ten samouczek ma następujące wymagania:

+ Aktywne konto Google. Możesz zalogować konto Google u [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
+ [Usługa Google Cloud wiadomości składnik klienta](http://components.xamarin.com/view/GCMClient/).

##<a name="configure-hub"></a>Konfigurowanie koncentratora powiadomień

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a id="register"></a>Włączanie Firebase wiadomości w chmurze

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

##<a name="configure-azure-to-send-push-requests"></a>Konfigurowanie Azure na wysyłanie wezwań na wypychane

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

##<a id="update-server"></a>Aktualizacja programu project server do wysyłania powiadomień wypychanych

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>Konfigurowanie projektu klienta do powiadomienia wypychane

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Dodawanie kodu powiadomienia wypychane do aplikacji

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Powiadomienia wypychane test w aplikacji

Istnieje możliwość przetestowania aplikacji przy użyciu urządzenia wirtualnego w emulatorze. Istnieją dodatkowe czynności konfiguracyjne wymagane przy pracy z emulatora.

1. Upewnij się, że wdrożenie lub debugowania na urządzeniu wirtualny zawierający interfejsy API Google Ustaw jako miejsce docelowe, jak pokazano poniżej w Menedżerze wirtualne urządzeniu z systemem Android (AVD).

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Dodawanie konta Google do urządzenie z systemem Android, klikając pozycję **aplikacje** > **Ustawienia** > **Dodawanie konta**, postępuj zgodnie z instrukcjami.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Uruchom aplikację listy zadań jako przed i wstawić nowy element zadania. Tym razem w obszarze powiadomień jest wyświetlana ikona powiadomienia. Możesz otworzyć szuflada powiadomienie wyświetlanie pełnego tekstu powiadomienia.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
