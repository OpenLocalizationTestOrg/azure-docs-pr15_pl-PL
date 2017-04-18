<properties
    pageTitle="Włączanie synchronizacja w trybie offline dla aplikacji platformy Windows uniwersalny (UWP) przy użyciu aplikacji Mobile | Azure aplikacji usługi"
    description="Dowiedz się, jak używać aplikacji Mobile Azure pamięci podręcznej i synchronizacja danych w trybie offline w aplikacji platformy Windows uniwersalny (UWP)."
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-windows-app"></a>Włączanie synchronizacja w trybie offline dla aplikacji systemu Windows

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Omówienie

Ten samouczek pokazano, jak dodać Obsługa w trybie offline do aplikacji platformy Windows uniwersalny (UWP) przy użyciu aplikacji Mobile Azure wewnętrznej bazy danych. Synchronizacja w trybie offline umożliwia użytkownikom końcowym korzystać z aplikacji dla urządzeń przenośnych — wyświetlanie, dodawanie lub modyfikowanie danych — nawet wtedy, gdy istnieje połączenie sieciowe. Zmiany są przechowywane w lokalnej bazy danych. Po powrocie do trybu online urządzenia te zmiany są synchronizowane z zdalnego wewnętrznej bazy danych.

W tym samouczku możesz zaktualizować projektu aplikacji UWP z samouczka [Tworzenie aplikacji systemu Windows] do obsługi funkcji offline aplikacji Mobile Azure. Jeśli nie korzystasz z programu project server pobrany szybki start, możesz dodać pakietów rozszerzenia danych programu access do projektu. Aby uzyskać więcej informacji na temat pakietów rozszerzenia serwera zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Aby dowiedzieć się więcej na temat funkcji synchronizacja w trybie offline, zobacz temat [Synchronizacja danych w trybie Offline w aplikacji Mobile Azure].

## <a name="requirements"></a>Wymagania dotyczące

Ten samouczek wymaga następujące wymagania wstępne:

* Visual Studio 2013 działa w systemie Windows 8.1 lub nowszy.
* Zakończenie [Tworzenie aplikacji systemu Windows],[Tworzenie aplikacji dla systemu windows].
* [Azure Mobile usług SQLite magazynu][sqlite store nuget]
* [SQLite rozwoju uniwersalny platformy systemu Windows](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>Aktualizowanie aplikacji klienta do obsługi funkcji trybu offline

Azure funkcje trybu offline w aplikacji Mobile umożliwia interakcję z lokalną bazą danych podczas pracy w trybie offline scenariuszu. Aby użyć tych funkcji w aplikacji, zainicjować [SyncContext] [ synccontext] do magazynu lokalnego. Następnie odwołać się do tabeli za pomocą interfejsu[IMobileServiceSyncTable] [IMobileServiceSyncTable]. SQLite jest używany jako magazynu lokalnego na tym urządzeniu.

1. Zainstaluj [środowisko uruchomieniowe SQLite uniwersalny platformy systemu Windows](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).

2. W programie Visual Studio Otwórz Menedżera pakietów NuGet projektu aplikacji UWP zakończenia w samouczku [Tworzenie aplikacji systemu Windows] .
    Wyszukaj i zainstaluj pakiet NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** .

4. W Eksploratorze rozwiązań, kliknij prawym przyciskiem myszy **odwołania** > **Dodaj odwołanie**  >  **Uniwersalny Windows** > **rozszerzenia**, następnie włączyć zarówno **SQLite uniwersalny platformy systemu Windows** i **Visual C++ Runtime 2015 aplikacje uniwersalny platformy systemu Windows**.

    ![Dodawanie SQLite UWP odwołania][1]

5. Otwórz plik MainPage.xaml.cs i Usuń komentarze `#define OFFLINE_SYNC_ENABLED` definicji.

6. W programie Visual Studio naciśnij klawisz **F5** , aby odbudować i uruchamianie aplikacji klienta. Aplikacja działa tak samo, jak przed włączeniem synchronizacja w trybie offline. Jednak lokalnej bazy danych jest teraz wypełniona dane, które mogą być używane w scenariusz w trybie offline.

## <a name="update-sync"></a>Aktualizowanie aplikacji, aby rozłączyć wewnętrznej bazy danych

W tej sekcji przerwaniu połączenia do wewnętrznej bazy danych usługi aplikacji Mobile celu zasymulowania offline sytuacji. Po dodaniu elementów danych do obsługi wyjątków informuje, że aplikacja jest w trybie offline. W tym stanie nowe elementy dodawane w magazynie lokalnym i będą synchronizowane z wewnętrznej bazy danych aplikacji dla urządzeń przenośnych przy następnym uruchomieniu wypychanych w stanie połączenia.

1. Edytowanie App.xaml.cs udostępnionego projektu. Inicjowanie **MobileServiceClient** komentarz, a następnie dodaj następujący wiersz, który używa adresu URL nieprawidłowe aplikacji dla urządzeń przenośnych:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Można również pokazano zachowanie trybu offline, wyłączając sieci Wi-Fi i sieci komórkowej na urządzeniu lub w trybie samolotem.

2. Naciśnij klawisz **F5** , aby skompilować i uruchomić aplikację. Zwróć uwagę, synchronizacji nie powiodła się podczas odświeżania podczas uruchamiania tej aplikacji.

3. Wprowadź nowe elementy i zwróć uwagę, że wypychane nie powiodła się ze stanem [CancelledByNetworkError] za każdym razem, kliknij przycisk **Zapisz**. Jednak nowych zadań do wykonania istnieje w magazynie lokalnym do momentu może zostać przeniesiony do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych.  W aplikacji produkcji Jeśli Pomiń te wyjątki aplikacji klienckiej zachowuje się tak, jakby nadal jest podłączone do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych.

4. Zamknij aplikację i ponownie go, aby sprawdzić, czy nowe elementy utworzone są zachowywane do magazynu lokalnego.

5. (Opcjonalnie) W programie Visual Studio Otwórz **Eksploratora serwera**. Przejdź do bazy danych platformy **Azure**->**Bazy danych programu SQL**. Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz polecenie **Otwórz w Eksploratorze obiektów SQL Server**. Teraz można przejść do tabeli bazy danych SQL i jego zawartość. Upewnij się, że dane w bazie danych wewnętrznej bazy danych nie zostały zmienione.

6. (Opcjonalnie) Kwerendy z urządzeń przenośnych wewnętrznej bazy danych, za pomocą kwerendy GET w formularzu za pomocą narzędzia pozostałych, takich jak Fiddler lub Postman `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Aktualizowanie aplikacji, aby ponownie nawiązać połączenie z aplikacji Mobile wewnętrznej bazy danych

W tej sekcji ponownym aplikacji do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych. Te zmiany symuluje ponowne połączenie sieciowe na aplikację.

Przy pierwszym uruchomieniu aplikacji, `OnNavigatedTo` wywołań obsługi zdarzeń `InitLocalStoreAsync`. Ta metoda z kolei wywołuje `SyncAsync` do synchronizacji magazynu lokalnego z wewnętrzną bazę danych. Pozwala podjąć próbę synchronizacji podczas uruchamiania tej aplikacji.

1. Otwórz App.xaml.cs udostępnionego projektu, a następnie usuń komentarze z poprzedniego inicjowanie `MobileServiceClient` umożliwia prawidłowy adres URL aplikacji dla urządzeń przenośnych.

2. Naciśnij klawisz **F5** , aby ponownie utworzyć i uruchomić aplikację. Aplikacja synchronizuje lokalne zmiany z wewnętrznej bazy danych aplikacji Mobile Azure za pomocą operacji i wypychania podczas `OnNavigatedTo` wykonuje zdarzeń.

3. (Opcjonalnie) Wyświetl zaktualizowane dane przy użyciu Eksploratora obiektów programu SQL Server lub narzędzie pozostałych, takie jak Fiddler. Powiadomienie o danych został zsynchronizowany między wewnętrzną bazę danych w aplikacji Mobile Azure i magazynu lokalnego.

4. W aplikacji kliknij pole wyboru obok kilka elementów do wykonania ich w magazynie lokalnym.

  `UpdateCheckedTodoItem`połączenia `SyncAsync` do elementu synchronizacja każdej z aplikacji Mobile wewnętrznej bazy danych. `SyncAsync`połączeń zarówno i wypychania. Jednak **przy każdym wykonaniu pobieraj tabeli, w której klient wprowadził zmiany, wypychanych jest zawsze wykonywana automatycznie**. To zachowanie gwarantuje, że wszystkie tabele w magazynie lokalnej wraz z relacjami zachowania zgodności. Ten problem może powodować nieoczekiwane wypychane.  Aby uzyskać więcej informacji na ten problem zobacz [w trybie Offline synchronizacja danych w aplikacji Mobile Azure].


##<a name="api-summary"></a>Podsumowanie interfejsu API

Do obsługi funkcji offline usług mobilnych, możemy używana interfejsu [IMobileServiceSyncTable] i zainicjowany [MobileServiceClient.SyncContext] [ synccontext] z lokalną bazą danych SQLite. W trybie offline, normalny operacji OBSŁUGIWAŁ aplikacji Mobile działają tak, jakby aplikacji jest nadal połączony podczas operacji występuje przed magazynu lokalnego. Następujące metody są używane do magazynu lokalnego są synchronizowane z serwerem:

*  **[PushAsync]** Ponieważ ta metoda jest członkiem [IMobileServicesSyncContext], zmiany przez wszystkie tabele są przenoszone do wewnętrznej bazy danych. Tylko rekordy o lokalne zmiany są wysyłane do serwera.

* **[PullAsync] ** 
   pobieraj została uruchomiona z [IMobileServiceSyncTable]. Istnienia prześledzonych zmian w tabeli, aby upewnić się, że wszystkie tabele w magazynie lokalnej wraz z relacjami zachowania zgodności jest uruchamiany niejawne push. Parametr *pushOtherTables* Określa, czy innych tabel w kontekście są usuwane w niejawne push. Parametry *kwerendy* trwa [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] 
   lub OData ciągu kwerendy do filtrowania danych zwróconych. Parametr *queryId* jest używany do definiowania synchronizacja przyrostowa. Aby uzyskać więcej informacji zobacz  [w trybie Offline synchronizacja danych w aplikacji Mobile Azure](app-service-mobile-offline-data-sync.md#how-sync-works).

* **[PurgeAsync]** Aplikacji powinna biurami tej metody, aby wyczyścić dane stare z magazynu lokalnego. Użyty parametr *wymusić* , gdy trzeba usunąć wszelkie zmiany, które nie zostały zsynchronizowane.

Aby uzyskać więcej informacji o tych pojęć zobacz [w trybie Offline synchronizacja danych w aplikacji Mobile Azure](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Więcej informacji

Poniższe tematy zawierają dodatkowe informacje na temat funkcji synchronizacja w trybie offline w aplikacji Mobile:

* [Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]
* [INSTRUKCJE SDK .NET Azure aplikacji dla urządzeń przenośnych][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]: app-service-mobile-offline-data-sync.md
[Tworzenie aplikacji dla systemu windows]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
