<properties
    pageTitle="Włączanie synchronizacja w trybie offline dla aplikacji Mobile Azure (Xamarin iOS)"
    description="Dowiedz się, jak używać aplikacji Mobile usługi aplikacji do pamięci podręcznej i synchronizacja danych w trybie offline w aplikacji systemu iOS Xamarin"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Włączanie synchronizacja w trybie offline dla swojego Xamarin.iOS aplikacji dla urządzeń przenośnych

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Omówienie

Ten samouczek wprowadzono funkcję synchronizacja w trybie offline w aplikacji Mobile Azure Xamarin.iOS. Synchronizacja w trybie offline umożliwia użytkownikom końcowym korzystać z aplikacji dla urządzeń przenośnych — wyświetlanie, dodawanie lub modyfikowanie danych — nawet wtedy, gdy jest brak połączenia z siecią. Zmiany są przechowywane w lokalnej bazy danych. Po powrocie do trybu online urządzenia te zmiany są synchronizowane z usługą zdalny.

W tym samouczku aktualizowanie projektu aplikacji Xamarin.iOS w obszarze [Tworzenie aplikacji iOS Xamarin] do obsługi funkcji offline aplikacji Mobile Azure. Jeśli nie korzystasz z programu project server pobrany szybki start, możesz dodać pakietów rozszerzenia danych programu access do projektu. Aby uzyskać więcej informacji na temat pakietów rozszerzenia serwera zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Aby dowiedzieć się więcej na temat funkcji synchronizacja w trybie offline, zobacz temat [Synchronizacja danych w trybie Offline w aplikacji Mobile Azure].

## <a name="update-the-client-app-to-support-offline-features"></a>Aktualizowanie aplikacji klienta do obsługi funkcji trybu offline

Azure funkcje trybu offline w aplikacji Mobile umożliwia interakcję z lokalną bazą danych podczas pracy w trybie offline scenariuszu. Aby użyć tych funkcji w aplikacji, zainicjować [SyncContext] do magazynu lokalnego. Odwołanie do tabeli za pomocą interfejsu [IMobileServiceSyncTable]. SQLite jest używany jako magazynu lokalnego na tym urządzeniu.

1. Otwórz Menedżera pakietów NuGet w projekcie, które wykonane w samouczku [Tworzenie aplikacji iOS Xamarin] , a następnie wyszukaj i zainstaluj pakiet **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet.

2. Otwórz plik QSTodoService.cs i Usuń komentarze `#define OFFLINE_SYNC_ENABLED` definicji.

3. Odbudowywanie i uruchom aplikację klienta. Aplikacja działa tak samo, jak przed włączeniem synchronizacja w trybie offline. Jednak lokalnej bazy danych jest teraz wypełniona dane, które mogą być używane w scenariusz w trybie offline.

## <a name="update-sync"></a>Aktualizowanie aplikacji do odłączyć od wewnętrznej bazy danych

W tej sekcji przerwaniu połączenia do wewnętrznej bazy danych usługi aplikacji Mobile celu zasymulowania offline sytuacji. Po dodaniu elementów danych do obsługi wyjątków informuje, że aplikacja jest w trybie offline. W tym stanie nowe elementy dodawane w magazynie lokalnym i będą synchronizowane z wewnętrznej bazy danych aplikacji dla urządzeń przenośnych przy następnym uruchomieniu wypychanych w stanie połączenia.

1. Edytowanie QSToDoService.cs udostępnionego projektu. Zmienianie **applicationURL** , aby wskazywały nieprawidłowego adresu URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Zachowanie trybu offline może również wykazać przez wyłączenie sieci Wi-Fi i sieci komórkowej na urządzeniu lub przy użyciu trybu samolotem.

2. Tworzenie i uruchamianie aplikacji. Zwróć uwagę, synchronizacji nie powiodła się podczas odświeżania podczas uruchamiania tej aplikacji.

3. Wprowadź nowe elementy i zwróć uwagę, że wypychane nie powiodła się ze stanem [CancelledByNetworkError] za każdym razem, kliknij przycisk **Zapisz**. Jednak nowych zadań do wykonania istnieje w magazynie lokalnym do momentu może zostać przeniesiony do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych.  W aplikacji produkcji Jeśli Pomiń te wyjątki aplikacji klienckiej zachowuje się tak, jakby nadal jest podłączone do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych.

4. Zamknij aplikację i ponownie go, aby sprawdzić, czy nowe elementy utworzone są zachowywane do magazynu lokalnego.

5. (Opcjonalnie) Jeśli masz Visual Studio zainstalowany na komputerze, otwórz **Eksploratora serwera**. Przejdź do bazy danych platformy **Azure**-> **Bazy danych programu SQL**. Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz polecenie **Otwórz w Eksploratorze obiektów SQL Server**. Teraz można przejść do tabeli bazy danych SQL i jego zawartość. Upewnij się, że dane w bazie danych wewnętrznej bazy danych nie zostały zmienione.

6. (Opcjonalnie) Kwerendy z urządzeń przenośnych wewnętrznej bazy danych, za pomocą kwerendy Pobierz w formularzu za pomocą narzędzia pozostałych, takich jak Fiddler lub Postman `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Aktualizowanie aplikacji, aby ponownie nawiązać połączenie z aplikacji Mobile wewnętrznej bazy danych

W tej sekcji ponownie aplikacji do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych. To symuluje aplikacji przechodzenie z trybu offline do trybu online przy użyciu aplikacji dla urządzeń przenośnych wewnętrznej bazy danych.   Jeśli uszkodzenie sieci jest symulowany przez wyłączenie łączność sieciowa, potrzebne są żadne zmiany kodu.
Ponownie włączyć sieci.  Przy pierwszym uruchomieniu aplikacji, `RefreshDataAsync` metoda jest wywoływana. Z kolei wymaga `SyncAsync` do synchronizacji magazynu lokalnego z wewnętrzną bazę danych.

1. Otwórz QSToDoService.cs udostępnionego projektu i przywrócić zmiany właściwości **applicationURL** .

2. Odbudowywanie i uruchom aplikację. Aplikacja synchronizuje lokalne zmiany z wewnętrznej bazy danych aplikacji Mobile Azure za pomocą operacji i wypychania po `OnRefreshItemsSelected` metoda.

3. (Opcjonalnie) Wyświetl zaktualizowane dane przy użyciu Eksploratora obiektów programu SQL Server lub narzędzie pozostałych, takie jak Fiddler. Powiadomienie o danych został zsynchronizowany między wewnętrzną bazę danych w aplikacji Mobile Azure i magazynu lokalnego.

4. W aplikacji kliknij pole wyboru obok kilka elementów do wykonania ich w magazynie lokalnym.

  `CompleteItemAsync`połączenia `SyncAsync` do elementu synchronizacja każdej z aplikacji Mobile wewnętrznej bazy danych. `SyncAsync`połączeń zarówno i wypychania.
  **Przy każdym wykonaniu pobieraj tabeli, w której klient wprowadził zmiany, wypychanych w kontekście klienta synchronizacji jest zawsze wykonywana najpierw automatycznie**. Niejawne wypychanych gwarantuje, że wszystkie tabele w magazynie lokalnej wraz z relacjami zachowania zgodności. Aby uzyskać więcej informacji na ten problem zobacz [w trybie Offline synchronizacja danych w aplikacji Mobile Azure].

## <a name="review-the-client-sync-code"></a>Przejrzyj kod klienta synchronizacji

Projekt klienta Xamarin pobranego po wykonaniu samouczek [Tworzenie aplikacji iOS Xamarin] już zawiera kod obsługi synchronizacji w trybie offline za pomocą lokalnej bazy danych SQLite. Oto krótkie omówienie już zawartość samouczka kod. Aby uzyskać omówienie funkcji zobacz [w trybie Offline synchronizacja danych w aplikacji Mobile Azure].

* Zanim będzie można dokonać wszystkie operacje tabeli, musi być zainicjowany magazynu lokalnego. Baza danych magazynu lokalnego jest zainicjowana po `QSTodoListViewController.ViewDidLoad()` wykonuje `QSTodoService.InitializeStoreAsync()`. Ta metoda umożliwia utworzenie nowej lokalnej SQLite bazy danych za pomocą `MobileServiceSQLiteStore` klasy przez klienta aplikacji Mobile Azure SDK.

    `DefineTable` Metody tworzy tabelę w magazynie lokalnym odpowiadającego polom w typie dostarczonych `ToDoItem` w tym przypadku. Aby uwzględnić wszystkie kolumny, które znajdują się w zdalna baza danych nie ma typu. Istnieje możliwość przechowywania tylko podzestawu danych kolumn.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }


* `todoTable` Członkiem `QSTodoService` jest `IMobileServiceSyncTable` wpisz zamiast `IMobileServiceTable`. IMobileServiceSyncTable kieruje wszystkie tworzenie, odczytywanie, aktualizowanie i usuwanie operacje tabeli (OBSŁUGIWAŁ) z bazą danych magazynu lokalnego.

    Określ, kiedy te zmiany są przenoszone do wewnętrznej bazy danych aplikacji Mobile Azure, dzwoniąc `IMobileServiceSyncContext.PushAsync()`. Kontekst synchronizacji pomaga zachować relacji między tabelami, śledzenia i naciśnięcie zmian we wszystkich tabelach aplikacji klienckiej zmodyfikował podczas `PushAsync` nosi nazwę.

    Połączenia dostarczony kod `QSTodoService.SyncAsync()` do synchronizacji po odświeżeniu listy todoitem lub todoitem zostanie dodany bądź wykonane. Synchronizuje aplikacji po każdej zmianie lokalny. Jeśli pobieraj jest wykonywane względem tabelę, która ma oczekujące aktualizacje lokalne śledzone według kontekstu, tej operacji pobieraj automatycznie spowoduje wypychanych kontekstu najpierw.

    W kodzie dostarczonych wszystkie rekordy zdalnego `TodoItem` tabeli są proszeni, ale jest również można filtrować rekordy przekazując identyfikator kwerendy i kwerendy do `PushAsync`. Aby uzyskać więcej informacji zobacz sekcję *Przyrostowe synchronizacji* w [Trybie Offline synchronizacja danych w aplikacji Mobile Azure].

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]
* [INSTRUKCJE SDK .NET Azure aplikacji dla urządzeń przenośnych][8]

<!-- Images -->

<!-- URLs. -->
[Tworzenie aplikacji w systemie iOS Xamarin]: app-service-mobile-xamarin-ios-get-started.md
[Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md