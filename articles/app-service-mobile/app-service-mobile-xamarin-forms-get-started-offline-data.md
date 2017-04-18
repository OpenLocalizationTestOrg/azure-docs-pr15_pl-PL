<properties
    pageTitle="Włączanie synchronizacja w trybie offline dla aplikacji Mobile Azure (Xamarin.Forms) | Microsoft Azure"
    description="Dowiedz się, jak używać aplikacji Mobile usługi aplikacji do pamięci podręcznej i synchronizacja danych w trybie offline w aplikacji Xamarin.Forms"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Włączanie synchronizacja w trybie offline dla swojego Xamarin.Forms aplikacji dla urządzeń przenośnych

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Omówienie
Ten samouczek wprowadzono funkcję synchronizacja w trybie offline w aplikacji Mobile Azure Xamarin.Forms. Synchronizacja w trybie offline umożliwia użytkownikom końcowym korzystać z aplikacji dla urządzeń przenośnych — wyświetlanie, dodawanie lub modyfikowanie danych — nawet wtedy, gdy jest brak połączenia z siecią. Zmiany są przechowywane w lokalnej bazy danych. Po powrocie do trybu online urządzenia te zmiany są synchronizowane z usługą zdalny.

Ten samouczek jest oparty na rozwiązanie Xamarin.Forms Szybki Start dla aplikacji Mobile, które są tworzone po zakończeniu samouczek [tworzenie aplikacji w systemie iOS Xamarin]. Rozwiązanie Szybki Start dla Xamarin.Forms zawiera kod do pomocy technicznej synchronizacja w trybie offline, który właśnie musi być włączona. W tym samouczku możesz zaktualizować rozwiązanie Szybki Start, aby włączyć trybu offline funkcje aplikacji Mobile Azure. Wyróżnianie możemy również kod specyficzny dla trybu offline w aplikacji. Jeśli nie używasz rozwiązanie pobrany Szybki Start, możesz dodać pakietów rozszerzenia danych programu access do projektu. Aby uzyskać więcej informacji na temat pakietów rozszerzenia serwera, zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure][1].

Aby dowiedzieć się więcej na temat funkcji synchronizacja w trybie offline, zobacz temat [Synchronizacja danych w trybie Offline w aplikacji Mobile Azure][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Włączanie funkcji synchronizacja w trybie offline w rozwiązaniu Szybki Start

Kod synchronizacja w trybie offline będzie obejmować projektu przy użyciu dyrektywy preprocesora C#. Gdy **OFFLINE\_SYNCHRONIZACJI\_włączone** symbol jest definiowana, te ścieżki kodu są wyświetlane w oknie Tworzenie. W przypadku aplikacji systemu Windows należy zainstalować platformy SQLite.

1. W programie Visual Studio, kliknij prawym przyciskiem myszy rozwiązanie > **Zarządzanie pakietów NuGet rozwiązania...**, a następnie wyszukaj i zainstaluj pakiet NuGet **Microsoft.Azure.Mobile.Client.SQLiteStore** dla wszystkich projektów w rozwiązaniu.

2. W Eksploratorze rozwiązań Otwórz plik TodoItemManager.cs z projektem z **Portable** w nazwie, który jest przenośny Biblioteka klas projektu, następnie usuń komentarze dyrektywy preprocesora następujące czynności:

        #define OFFLINE_SYNC_ENABLED

4. (Opcjonalnie) Do obsługi urządzeń z systemem Windows, zainstaluj jedną z następujących pakietach środowisko uruchomieniowe SQLite:

    * **Obsługi Windows 8.1:** Instalowanie [SQLite dla systemu Windows 8.1][3].
    * **Windows Phone 8.1:** Instalowanie [SQLite dla Windows Phone 8.1][4].
    * **Platformy Windows uniwersalny** Instalowanie [SQLite dla uniwersalny uniwersalny Windows][5].

    Mimo że Szybki Start nie zawiera projektu uniwersalny systemu Windows, platformy Windows uniwersalny jest obsługiwana w formularzach Xamarin.

5. (Opcjonalnie) W każdym projekcie aplikacji systemu Windows, kliknij prawym przyciskiem myszy **odwołania** > **Dodaj odwołanie**, rozwiń **Windows** folder > **rozszerzenia**.
    Włącz odpowiednie **SQLite dla systemu Windows** SDK wraz z **Visual C++ 2013 Runtime for Windows** SDK.
    Nazwy SQLite SDK się nieco różnić z każdego platformy systemu Windows.

## <a name="review-the-client-sync-code"></a>Przejrzyj kod klienta synchronizacji

Oto krótkie omówienie już zawartość samouczka kod wewnątrz `#if OFFLINE_SYNC_ENABLED` dyrektywy. Funkcja synchronizacja w trybie offline jest TodoItemManager.cs pliku projektu w programie project Portable Biblioteka klas. Aby uzyskać omówienie funkcji, zobacz [w trybie Offline synchronizacja danych w aplikacji Mobile Azure][2].

* Zanim będzie można dokonać wszystkie operacje tabeli, musi być zainicjowany magazynu lokalnego. Baza danych magazynu lokalnego jest zainicjowana w konstruktorze klasy **TodoItemManager** przy użyciu następującego kodu:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Kod ten tworzy nową lokalną bazę SQLite za pomocą klasy **MobileServiceSQLiteStore** .

    Metoda **DefineTable** tworzy tabelę w pasującego pola podany typ magazynu lokalnego.  Aby uwzględnić wszystkie kolumny, które znajdują się w zdalna baza danych nie ma typu. Istnieje możliwość przechowywania podzbiór kolumn.

* Pole **todoTable** w **TodoItemManager** jest typem **IMobileServiceSyncTable** zamiast **IMobileServiceTable**. Ten zajęć przy użyciu lokalnej bazy danych dla wszystkich tworzenia, odczytu, aktualizowanie i usuwanie operacje tabeli (OBSŁUGIWAŁ). Możesz określić, kiedy te zmiany są przenoszone do wewnętrznej bazy danych aplikacji Mobile, dzwoniąc **PushAsync** na **IMobileServiceSyncContext**. Kontekst synchronizacji pomaga zachować relacji między tabelami, śledzenia i naciśnięcie zmian we wszystkich tabelach, które aplikacji klienckiej zmodyfikował po **PushAsync** .

    Następujące metody **SyncAsync** do synchronizacji z wewnętrznej bazy danych aplikacji Mobile:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    W tym przykładzie użyto prostej błąd obsługi z domyślnym programem obsługi synchronizacji. Rzeczywistej aplikacji może obsługiwać różne błędy, takie jak parametry sieci i rozwiązywanie konfliktów serwera przy użyciu niestandardowej implementacji **IMobileServiceSyncHandler** .

## <a name="offline-sync-considerations"></a>Zagadnienia dotyczące synchronizacja w trybie offline

W próbce metoda **SyncAsync** jest wywoływana tylko przy uruchamianiu i w przypadku zażądania synchronizacji.  Aby rozpocząć synchronizowanie aplikacji Android lub iOS, rozwijane w dół na liście elementów; dla systemu Windows użyj przycisku **Synchronizuj** . W aplikacji rzeczywistych można również wykorzystać wyzwalacza synchronizacji po zmianie stan sieci.

Podczas wykonywania pobieraj przed tabelę zawierającą oczekiwanie lokalne aktualizacje śledzona przez kontekstu, które pobierają operacji automatycznie wyzwalaczy poprzedniego wypychanych kontekstu. Podczas odświeżania, dodawanie i kończenie pracy elementów w tym przykładzie, można pominąć jawne połączenia **PushAsync** .

W kodzie dostarczonych wszystkie rekordy w tabeli zdalnej TodoItem są proszeni, ale jest również można filtrować rekordy przekazując identyfikator kwerendy i kwerendy do **PushAsync**. Aby uzyskać więcej informacji, zobacz sekcję *Przyrostowe synchronizacji* w [Trybie Offline synchronizacja danych w aplikacji Mobile Azure][2].

## <a name="run-the-client-app"></a>Uruchom aplikację klienta

Teraz włączona synchronizacja w trybie offline uruchamianie aplikacji klienckiej przynajmniej raz na każdej z tych platform do wypełniania bazy danych magazynu lokalnego. Później symulowania scenariusz w trybie offline i zmodyfikować dane w magazynie lokalnej, gdy aplikacja jest w trybie offline.

## <a name="update-the-sync-behavior-of-the-client-app"></a>Aktualizowanie zachowanie synchronizacji aplikacji klienta

W tej sekcji modyfikowanie projektu klienta w celu zasymulowania scenariusz w trybie offline, używając nieprawidłowy adres URL aplikacji do wewnętrznej bazy danych. Alternatywnie możesz wyłączyć połączeń sieciowych, przenosząc urządzenia "Tryb Samolot".  Gdy dodajesz lub zmieniasz elementów danych, te zmiany są przechowywane w magazynie lokalnej, ale nie zostały zsynchronizowane z magazynu danych wewnętrznej bazy danych, dopóki ponownym nawiązaniu połączenia.

1. W Eksploratorze rozwiązań Otwieranie pliku projektu Constants.cs z **Portable** projektu i zmień wartość `ApplicationURL` , aby wskazywały nieprawidłowego adresu URL:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";

2. Otwórz plik TodoItemManager.cs z **Portable** projektu, a następnie dodaj **efektywnej** klasy podstawowej **wyjątku** do bloku **Spróbuj... efektywnej** w **SyncAsync**. Ten blok **efektywnej** zapisuje komunikat wyjątku konsoli w następujący sposób:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. Tworzenie i uruchamianie aplikacji klienckich.  Dodaj kilka nowych elementów. Zwróć uwagę, że wyjątek jest rejestrowany w konsoli dla każdej próbie zsynchronizować z wewnętrznej bazy danych. Tych nowych elementów istnieje tylko w magazynie lokalnym, dopóki nie może zostać przeniesiony do urządzeń przenośnych wewnętrznej bazy danych. Aplikacja klienta zachowuje się tak, jakby jest podłączone do wewnętrznej bazy danych, pomocnicze utworzone przez wszystkich, odczytu, aktualizacji, operacji usuwania.

4. Zamknij aplikację i ponownie go, aby sprawdzić, czy nowe elementy utworzone są zachowywane do magazynu lokalnego.

5. (Opcjonalnie) Aby wyświetlić tabelę bazy danych SQL Azure, aby sprawdzić, czy nie zmieniono danych w bazie danych wewnętrznej bazy danych za pomocą programu Visual Studio.

    W programie Visual Studio Otwórz **Eksploratora serwera**. Przejdź do bazy danych platformy **Azure**->**Bazy danych programu SQL**. Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz polecenie **Otwórz w Eksploratorze obiektów SQL Server**. Teraz można przejść do tabeli bazy danych SQL i jego zawartość.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Aktualizowanie aplikacji klienta, aby ponownie nawiązać połączenie z urządzeń przenośnych wewnętrznej bazy danych

W tej sekcji ponownie aplikację, aby przenośnych wewnętrznej bazy danych, które symuluje aplikacji pochodzące z powrotem do trybu online. Podczas wykonywania gest odświeżania danych jest synchronizowane z wewnętrznej bazy danych z urządzeń przenośnych.

1. Otwórz ponownie Constants.cs. Poprawianie `applicationURL` , aby wskazywały poprawny adres URL.

2. Odbudowywanie i uruchom aplikację klienta. Pozwala podjąć próbę zsynchronizować z aplikacji dla urządzeń przenośnych wewnętrznej bazy danych po uruchomieniu aplikacji. Upewnij się, że bez wyjątków są rejestrowane w konsoli debugowania.

3. (Opcjonalnie) Wyświetlanie zaktualizowane dane przy użyciu Eksploratora obiektów programu SQL Server lub narzędzie pozostałych, takie jak Fiddler lub [Postman][6]. Zwróć uwagę, że dane zostały synchronizowane między wewnętrzną bazę danych i magazynu lokalnego.

    Zwróć uwagę, dane zostały zsynchronizowane między bazy danych i magazynu lokalnego i zawiera elementy, które zostały dodane odłączeniu aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure][2]
* [INSTRUKCJE SDK .NET Azure aplikacji dla urządzeń przenośnych][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md