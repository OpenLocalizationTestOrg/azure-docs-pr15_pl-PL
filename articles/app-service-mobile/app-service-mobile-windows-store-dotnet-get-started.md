<properties
    pageTitle="Tworzenie uniwersalny Windows platformy (UWP) używaną w aplikacji Mobile | Microsoft Azure"
    description="Skorzystać z tego samouczka, aby rozpocząć pracę z przy użyciu aplikacji dla urządzeń przenośnych Azure wewnętrznych bazach danych dla opracowywania aplikacji uniwersalny platformy systemu Windows (UWP) w C#, Visual Basic lub języka JavaScript."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-windows-app"></a>Tworzenie aplikacji dla systemu Windows

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Omówienie

Ten samouczek pokazano, jak dodać usługi opartej na chmurze wewnętrznej bazy danych do aplikacji Universal platformy systemu Windows (UWP). Aby uzyskać więcej informacji zobacz [Co to są aplikacje Mobile](app-service-mobile-value-prop.md). Poniżej przedstawiono zrzutów ekranu w złożonym aplikacji:

![Ukończony aplikacji klasycznej](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Zainstalowany na pulpicie. 

![Aplikacja phone wykonanych](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Działa na telefonie

Ten samouczek jest wymagane w przypadku wszystkich innych samouczków aplikacji Mobile w przypadku aplikacji UWP. 

##<a name="prerequisites"></a>Wymagania wstępne

Aby użyć tego samouczka, są potrzebne następujące elementy:

* Konto Azure active. Jeśli nie masz konta, możesz utworzyć konto Azure wersji próbnej i uzyskać maksymalnie 10 bezpłatne aplikacje mobilne, których możesz korzystać nawet po zakończeniu okresu próbnego. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio społeczności 2015] lub nowsza wersja.

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi, aby utworzyć konto Azure, przejdź do [Spróbuj aplikacji usługi](https://tryappservice.azure.com/?appServiceName=mobile). Możesz od razu utworzyć aplikacji dla urządzeń przenośnych krótkotrwałe starter w aplikacji usługi — karty kredytowej wymagane i nie zobowiązań.

##<a name="create-a-new-azure-mobile-app-backend"></a>Tworzenie nowej aplikacji Mobile Azure wewnętrznej bazie danych

Wykonaj poniższe czynności w celu utworzenia nowej aplikacji Mobile wewnętrznej bazie danych.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Teraz masz obsługi administracyjnej wewnętrznej bazy danych aplikacji Mobile Azure, który może być używany przez aplikacje klientów przenośnych. Następnie pobierze projektu server do prostej "list zadania" wewnętrznej bazy danych w celu opublikowania go w Azure.

## <a name="configure-the-server-project"></a>Konfigurowanie programu project server

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-client-project"></a>Pobierz i uruchom projektu klienta

Po skonfigurowaniu sieci wewnętrznej bazy danych aplikacji Mobile możesz utworzenia nowej aplikacji klienckich lub zmodyfikować istniejącej aplikacji, aby nawiązać połączenie Azure. W tej sekcji możesz pobrać UWP aplikacji szablon projektu, który jest dostosować tak, aby połączyć się z aplikacji Mobile wewnętrznej bazy danych.

1. Po powrocie do karta **Szybki start** dla programu aplikacji Mobile wewnętrznej bazy danych, kliknij pozycję **Utwórz nową aplikację** > **Pobierz**, a następnie Wyodrębnij skompresowany projektu plików na komputerze lokalnym.

    ![Pobieranie programu project Szybki Start systemu Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. (Opcjonalnie) Dodaj projekt aplikacji UWP do tego samego rozwiązania jako programu project server. Ułatwia debugowanie i przetestuj aplikację i wewnętrznej bazy danych w tym samym rozwiązaniu Visual Studio, jeśli wybierzesz to zrobić. Aby dodać projekt aplikacji UWP rozwiązanie, możesz korzystać z programu Visual Studio 2015 i w nowszych wersjach.

4. Aplikacja UWP jako projekt startowy naciśnij klawisz F5, aby wdrażanie i uruchamianie aplikacji.

5. W aplikacji wpisz opisową tekstu, takie jak *Wykonano samouczka*, w polu tekstowym **wstawić TodoItem** , a następnie kliknij przycisk **Zapisz**.

    ![Pulpit wykonano Szybki Start systemu Windows](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    To przesyła żądanie POST do nowej aplikacji dla urządzeń przenośnych wewnętrznej bazy danych znajdującej się w Azure.

6. (Opcjonalnie) Zatrzymaj aplikację i uruchom go na inne urządzenie lub emulatora urządzeń przenośnych.

    ![Systemu Windows phone wykonano Szybki Start](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Zwróć uwagę, że dane zapisane w poprzednim kroku zostanie załadowana z Azure po uruchomieniu aplikacji UWP. 

##<a name="next-steps"></a>Następne kroki

* [Dodawanie uwierzytelniania do aplikacji](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Dowiedz się, jak uwierzytelniania użytkowników aplikacji z dostawcą tożsamości.

* [Dodawanie powiadomień wypychanych do aplikacji](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Dowiedz się, jak dodać wypychanych powiadomień wsparcie dla aplikacji i konfigurowanie usługi wewnętrznej bazy danych aplikacji Mobile umożliwia koncentratory powiadomienie Azure do wysyłania powiadomień wypychanych.

* [Włączanie synchronizacja w trybie offline dla aplikacji](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Dowiedz się, jak dodać Obsługa w trybie offline aplikacji przy użyciu aplikacji Mobile wewnętrznej bazy danych. Synchronizacja w trybie offline umożliwia użytkownikom końcowym współdziałanie z aplikacji dla urządzeń przenośnych&mdash;wyświetlania, dodawania i modyfikowania danych&mdash;nawet wtedy, gdy jest brak połączenia z siecią.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Społeczność programu Visual Studio 2015 r.]: https://go.microsoft.com/fwLink/p/?LinkID=534203
