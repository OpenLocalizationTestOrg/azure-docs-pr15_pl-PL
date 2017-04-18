<properties
    pageTitle="Wprowadzenie do aplikacji dla urządzeń przenośnych Azure w przypadku aplikacji Xamarin.Android"
    description="Skorzystać z tego samouczka, aby rozpocząć korzystanie z aplikacji Mobile Azure rozwoju Xamarin Android"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>Tworzenie aplikacji Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Omówienie

Ten samouczek pokazano, jak dodać usługi opartej na chmurze wewnętrznej bazy danych do aplikacji Xamarin.Android. Aby uzyskać więcej informacji zobacz [Co to są aplikacje Mobile](app-service-mobile-value-prop.md).

Zrzut ekranu z wykonanych aplikacji jest poniżej:

![][0]

Ten samouczek jest wymagane w przypadku wszystkich innych samouczków aplikacji Mobile w przypadku aplikacji Xamarin.Android.

##<a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, potrzebne następujące wymagania wstępne:

* Konto Azure active. Jeśli nie masz konta, konta w usłudze Azure wersji próbnej i pobrać maksymalnie 10 bezpłatne aplikacje Mobile. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

* Program Visual Studio z Xamarin. Aby uzyskać instrukcje, zobacz [Instalacja i zainstaluj program Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

>[AZURE.NOTE]Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi przed utworzeniem konta dla konta Azure, przejdź do [Spróbuj aplikacji usługi](https://tryappservice.azure.com/?appServiceName=mobile).  Możesz od razu utworzyć krótkotrwałe początkowego aplikacji Mobile w aplikacji usługi. Nie kart kredytowych wymagane; nie zobowiązania.

## <a name="create-an-azure-mobile-app-backend"></a>Tworzenie aplikacji Mobile Azure wewnętrznej bazy danych

Wykonaj poniższe czynności, aby utworzyć aplikacji Mobile wewnętrznej bazie danych.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Teraz masz obsługi administracyjnej wewnętrznej bazy danych aplikacji Mobile Azure, który może być używany przez aplikacje klientów przenośnych. Następnie pobrać projektu server do prostej "list zadania" wewnętrznej bazy danych w celu opublikowania go w Azure.

## <a name="configure-the-server-project"></a>Konfigurowanie programu project server

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Pobierz i uruchom aplikację Xamarin.Android

1. W obszarze **pobrać i uruchomić Xamarin.Android projektu**kliknij przycisk **Pobierz** .

    Zapisz plik skompresowany projektu na komputerze lokalnym i zanotuj miejsce, w którym można go zapisać.

2. Naciśnij klawisz **F5** , aby utworzyć projektu i uruchom aplikację.

3. W aplikacji wpisz opisową tekst, na przykład _Wykonano samouczka_ , a następnie kliknij przycisk **Dodaj** .

    ![][10]

    Dane z żądania zostanie wstawiona do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez wewnętrznej bazy danych aplikacji dla urządzeń przenośnych, a dane są wyświetlane na liście.

    > [AZURE.NOTE] Możesz przejrzeć kod, który uzyskuje dostęp do wewnętrznej bazy danych usługi aplikacji dla urządzeń przenośnych do kwerendy, a następnie Wstaw dane, który znajduje się w pliku ToDoActivity.cs C#.

##<a name="next-steps"></a>Następne kroki

* [Dodawanie synchronizacja w trybie Offline do aplikacji](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Dodawanie uwierzytelniania do aplikacji](app-service-mobile-xamarin-android-get-started-users.md)
* [Dodawanie powiadomienia wypychane do aplikacji Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md)
* [Jak w przypadku aplikacji Mobile Azure za pomocą klienta usługi zarządzanych](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
