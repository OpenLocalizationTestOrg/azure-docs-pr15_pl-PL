<properties
    pageTitle="Włączanie synchronizacja w trybie offline dla aplikacji Mobile Azure (system Xamarin Android)"
    description="Dowiedz się, jak używać aplikacji Mobile usługi aplikacji do pamięci podręcznej i synchronizacja danych w trybie offline w aplikacji Xamarin Android"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Włączanie synchronizacja w trybie offline dla swojego Xamarin.Android aplikacji dla urządzeń przenośnych

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Omówienie

Ten samouczek wprowadzono funkcję synchronizacja w trybie offline w aplikacji Mobile Azure Xamarin.Android. Synchronizacja w trybie offline umożliwia użytkownikom końcowym korzystać z aplikacji dla urządzeń przenośnych — wyświetlanie, dodawanie lub modyfikowanie danych — nawet wtedy, gdy jest brak połączenia z siecią. Zmiany są przechowywane w lokalnej bazy danych.
Po powrocie do trybu online urządzenia te zmiany są synchronizowane z usługą zdalny.

W tym samouczku możesz zaktualizować projektu klienta z samouczka [Tworzenie aplikacji Xamarin Android] do obsługi funkcji offline aplikacji Mobile Azure. Jeśli nie korzystasz z programu project server pobrany szybki start, możesz dodać pakietów rozszerzenia danych programu access do projektu. Aby uzyskać więcej informacji na temat pakietów rozszerzenia serwera zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Aby dowiedzieć się więcej na temat funkcji synchronizacja w trybie offline, zobacz temat [Synchronizacja danych w trybie Offline w aplikacji Mobile Azure].

## <a name="update-the-client-app-to-support-offline-features"></a>Aktualizowanie aplikacji klienta do obsługi funkcji trybu offline

Azure funkcje trybu offline w aplikacji Mobile umożliwia interakcję z lokalną bazą danych podczas pracy w trybie offline scenariuszu. Aby użyć tych funkcji w aplikacji, należy zainicjować [SyncContext] do magazynu lokalnego. Następnie odwołać się do tabeli za pomocą interfejsu [IMobileServiceSyncTable] [IMobileServiceSyncTable]. SQLite jest używany jako magazynu lokalnego na tym urządzeniu.

1. W programie Visual Studio Otwórz Menedżera pakietów NuGet w projekcie, które wykonane w samouczku [Tworzenie aplikacji Xamarin Android] .  Wyszukaj i zainstaluj pakiet NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** .

2. Otwórz plik ToDoActivity.cs i Usuń komentarze `#define OFFLINE_SYNC_ENABLED` definicji.

3. W programie Visual Studio naciśnij klawisz **F5** , aby odbudować i uruchamianie aplikacji klienta. Aplikacja działa tak samo, jak przed włączeniem synchronizacja w trybie offline. Jednak lokalnej bazy danych jest teraz wypełniona dane, które mogą być używane w scenariusz w trybie offline.

## <a name="update-sync"></a>Aktualizowanie aplikacji do odłączyć od wewnętrznej bazy danych

W tej sekcji przerwaniu połączenia do wewnętrznej bazy danych usługi aplikacji Mobile celu zasymulowania offline sytuacji. Po dodaniu elementów danych do obsługi wyjątków informuje, że aplikacja jest w trybie offline. W tym stanie nowe elementy dodawane w magazynie lokalnym i są synchronizowane z wewnętrznej bazy danych aplikacji dla urządzeń przenośnych, gdy wypychanych jest wykonywane w stanie połączenia.

1. Edytowanie ToDoActivity.cs udostępnionego projektu. Zmienianie **applicationURL** , aby wskazywały nieprawidłowego adresu URL:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Zachowanie trybu offline może również wykazać przez wyłączenie sieci Wi-Fi i sieci komórkowej na urządzeniu lub przy użyciu trybu samolotem.

2. Naciśnij klawisz **F5** , aby skompilować i uruchomić aplikację. Zwróć uwagę, synchronizacji nie powiodła się podczas odświeżania podczas uruchamiania tej aplikacji.

3. Wprowadź nowe elementy i zwróć uwagę, że wypychane nie powiodła się ze stanem [CancelledByNetworkError] za każdym razem, kliknij przycisk **Zapisz**. Jednak nowych zadań do wykonania istnieje w magazynie lokalnym do momentu może zostać przeniesiony do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych.  W aplikacji produkcji Jeśli Pomiń te wyjątki aplikacji klienckiej zachowuje się tak, jakby nadal jest podłączone do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych.

4. Zamknij aplikację i ponownie go, aby sprawdzić, czy nowe elementy utworzone są zachowywane do magazynu lokalnego.

5. (Opcjonalnie) W programie Visual Studio Otwórz **Eksploratora serwera**. Przejdź do bazy danych platformy **Azure**->**Bazy danych programu SQL**. Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz polecenie **Otwórz w Eksploratorze obiektów SQL Server**. Teraz można przejść do tabeli bazy danych SQL i jego zawartość. Upewnij się, że dane w bazie danych wewnętrznej bazy danych nie zostały zmienione.

6. (Opcjonalnie) Kwerendy z urządzeń przenośnych wewnętrznej bazy danych, za pomocą kwerendy Pobierz w formularzu za pomocą narzędzia pozostałych, takich jak Fiddler lub Postman `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Aktualizowanie aplikacji, aby ponownie nawiązać połączenie z aplikacji Mobile wewnętrznej bazy danych

W tej sekcji ponownie aplikacji do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych. Przy pierwszym uruchomieniu aplikacji, `OnCreate` wywołań obsługi zdarzeń `OnRefreshItemsSelected`. Ta metoda wymaga `SyncAsync` do synchronizacji magazynu lokalnego z wewnętrzną bazę danych.

1. Otwórz ToDoActivity.cs w projekcie udostępnionych i przywrócić zmiany właściwości **applicationURL** .

2. Naciśnij klawisz **F5** , aby ponownie utworzyć i uruchomić aplikację. Aplikacja synchronizuje lokalne zmiany z wewnętrznej bazy danych aplikacji Mobile Azure za pomocą operacji i wypychania po `OnRefreshItemsSelected` metoda.

3. (Opcjonalnie) Wyświetl zaktualizowane dane przy użyciu Eksploratora obiektów programu SQL Server lub narzędzie pozostałych, takie jak Fiddler. Powiadomienie o danych został zsynchronizowany między wewnętrzną bazę danych w aplikacji Mobile Azure i magazynu lokalnego.

4. W aplikacji kliknij pole wyboru obok kilka elementów do wykonania ich w magazynie lokalnym.

  `CheckItem`połączenia `SyncAsync` do elementu synchronizacja każdej z aplikacji Mobile wewnętrznej bazy danych. `SyncAsync`połączeń zarówno i wypychania. **Przy każdym wykonaniu pobieraj tabeli, w której klient wprowadził zmiany, wypychanych jest zawsze wykonywana automatycznie**. Dzięki temu wszystkie tabele w magazynie lokalnej wraz z relacji zachowania zgodności. Ten problem może powodować nieoczekiwane push. Aby uzyskać więcej informacji na ten problem zobacz [w trybie Offline synchronizacja danych w aplikacji Mobile Azure].

## <a name="review-the-client-sync-code"></a>Przejrzyj kod klienta synchronizacji

Projekt klienta Xamarin pobranego po wykonaniu samouczek [Tworzenie aplikacji Xamarin Android] już zawiera kod obsługi synchronizacji w trybie offline za pomocą lokalnej bazy danych SQLite. Oto krótkie omówienie już zawartość samouczka c nadawcy. Aby uzyskać omówienie funkcji zobacz [w trybie Offline synchronizacja danych w aplikacji Mobile Azure].

* Zanim będzie można dokonać wszystkie operacje tabeli, musi być zainicjowany magazynu lokalnego. Baza danych magazynu lokalnego jest zainicjowana po `ToDoActivity.OnCreate()` wykonuje `ToDoActivity.InitLocalStoreAsync()`. Ta metoda umożliwia utworzenie lokalnej bazy danych SQLite, za pomocą `MobileServiceSQLiteStore` klasy przez klienta aplikacji Mobile Azure SDK.

    `DefineTable` Metody tworzy tabelę w magazynie lokalnym odpowiadającego polom w typie dostarczonych `ToDoItem` w tym przypadku. Aby uwzględnić wszystkie kolumny, które znajdują się w zdalna baza danych nie ma typu. Istnieje możliwość przechowywania tylko podzestawu danych kolumn.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }


* `toDoTable` Członkiem `ToDoActivity` jest `IMobileServiceSyncTable` wpisz zamiast `IMobileServiceTable`. IMobileServiceSyncTable kieruje wszystkie tworzenie, odczytywanie, aktualizowanie i usuwanie operacje tabeli (OBSŁUGIWAŁ) z bazą danych magazynu lokalnego.

    Określ, kiedy zmiany są przenoszone do wewnętrznej bazy danych aplikacji Mobile Azure, dzwoniąc `IMobileServiceSyncContext.PushAsync()`. Kontekst synchronizacji pomaga zachować relacji między tabelami, śledzenia i naciśnięcie zmian we wszystkich tabelach aplikacji klienckiej zmodyfikował podczas `PushAsync` nosi nazwę.

    Połączenia dostarczony kod `ToDoActivity.SyncAsync()` do synchronizacji po odświeżeniu listy todoitem lub todoitem zostanie dodany bądź wykonane. Synchronizuje kod po każdej zmianie lokalny.

    W kodzie dostarczonych wszystkie rekordy zdalnego `TodoItem` tabeli są proszeni, ale jest również można filtrować rekordy przekazując identyfikator kwerendy i kwerendy do `PushAsync`. Aby uzyskać więcej informacji zobacz sekcję *Przyrostowe synchronizacji* w [Trybie Offline synchronizacja danych w aplikacji Mobile Azure].

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]
* [INSTRUKCJE SDK .NET Azure aplikacji dla urządzeń przenośnych][8]

<!-- URLs. -->
[Tworzenie aplikacji Xamarin systemu Android]: ../app-service-mobile-xamarin-android-get-started.md
[Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Tworzenie aplikacji Xamarin systemu Android]: app-service-mobile-xamarin-android-get-started.md
[Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
