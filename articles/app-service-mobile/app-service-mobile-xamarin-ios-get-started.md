<properties
    pageTitle="Wprowadzenie do programu Mobile Azure aplikacji usług dla aplikacji Xamarin.iOS | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć pracę z przy użyciu aplikacji Mobile rozwoju Xamarin.iOS."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>Tworzenie aplikacji Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Omówienie

Tym samouczku pokazano, jak dodawać usługi opartej na chmurze wewnętrznej bazy danych do aplikacji dla urządzeń przenośnych Xamarin.iOS za pomocą aplikacji dla urządzeń przenośnych Azure wewnętrznej bazy danych.  Możesz utworzyć nowy wewnętrznej bazy danych aplikacji dla urządzeń przenośnych jak prosta _Lista zadania_ aplikacji Xamarin.iOS, w którym są przechowywane dane aplikacji platformy Azure.

Ten samouczek jest wymagane w przypadku wszystkich innych samouczków Xamarin.iOS o korzystaniu z funkcji aplikacji Mobile w Azure aplikacji usługi.

##<a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, potrzebne następujące wymagania wstępne:

* Konto Azure active. Jeśli nie masz konta, Utwórz konto w usłudze Azure wersji próbnej i przejść do 10 bezpłatnej aplikacji dla urządzeń przenośnych, które możesz korzystać nawet po zakończeniu okresu próbnego. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

* Program Visual Studio z Xamarin. Aby uzyskać instrukcje, zobacz [Instalacja i zainstaluj program Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

* Mac z Xcode 7.0 lub nowszy i Xamarin Studio społeczności zainstalowany. Zobacz [Instalacja i zainstaluj program Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) i [konfiguracji, instalowanie i weryfikacji użytkownicy komputerów Mac](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE]Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi, aby utworzyć konto Azure, przejdź do [Spróbuj aplikacji usługi](https://tryappservice.azure.com/?appServiceName=mobile). Możesz od razu utworzyć aplikacji dla urządzeń przenośnych krótkotrwałe starter w aplikacji usługi — karty kredytowej wymagane i nie zobowiązań.

## <a name="create-an-azure-mobile-app-backend"></a>Tworzenie aplikacji Mobile Azure wewnętrznej bazy danych

Wykonaj poniższe czynności, aby utworzyć aplikacji Mobile wewnętrznej bazie danych.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Konfigurowanie programu project server

Teraz masz obsługi administracyjnej wewnętrznej bazy danych aplikacji Mobile Azure, który może być używany przez aplikacje klientów przenośnych. Następnie pobrać projektu server do prostej "list zadania" wewnętrznej bazy danych w celu opublikowania go w Azure.

Wykonaj poniższe czynności, aby skonfigurować project server umożliwia Node.js albo .NET wewnętrznej bazy danych.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Pobierz i uruchom aplikację Xamarin.iOS

1. Otwórz [Azure portal] w oknie przeglądarki.

2. Karta Ustawienia dla aplikacji Mobile, wybierz polecenie **Wprowadzenie** > **Xamarin.iOS**. W kroku 3 kliknij przycisk **Utwórz nową aplikację** , jeśli jeszcze nie jest wybrana.  Następnie kliknij przycisk **Pobierz** .

    Aplikacja klienta, który łączy się z urządzeń przenośnych wewnętrznej bazy danych są pobierane. Zapisz plik skompresowany projektu na komputerze lokalnym i zanotuj miejsce, w którym można go zapisać.

3. Wyodrębnianie projekt, który został pobrany, a następnie otwórz go w Xamarin Studio (lub Visual Studio).

    ![][9]

    ![][8]

4. Naciśnij klawisz F5, aby utworzyć projektu i uruchamianie aplikacji w emulatorze iPhone.

5. W aplikacji, wpisz opisową tekstu, takie jak _Informacje Xamarin_, a następnie kliknij pozycję **+** przycisk.

    ![][10]

    Dane z żądania zostanie wstawiona do tabeli TodoItem. Elementy przechowywane w tabeli są zwracane przez wewnętrznej bazy danych aplikacji dla urządzeń przenośnych, a dane są wyświetlane na liście.

>[AZURE.NOTE]Możesz przejrzeć kod, który uzyskuje dostęp do wewnętrznej bazy danych usługi aplikacji dla urządzeń przenośnych do kwerendy, a następnie Wstaw dane w pliku QSTodoService.cs C#.

##<a name="next-steps"></a>Następne kroki

* [Dodawanie synchronizacja w trybie Offline do aplikacji](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Dodawanie uwierzytelniania do aplikacji](app-service-mobile-xamarin-ios-get-started-users.md)
* [Dodawanie powiadomienia wypychane do aplikacji Xamarin.Android](app-service-mobile-xamarin-ios-get-started-push.md)
* [Jak w przypadku aplikacji Mobile Azure za pomocą klienta usługi zarządzanych](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure portal]: https://portal.azure.com/
