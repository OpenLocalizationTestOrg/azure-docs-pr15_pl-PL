<properties
    pageTitle="Włączanie synchronizacja w trybie offline dla aplikacji Mobile Azure (Cordova) | Microsoft Azure"
    description="Dowiedz się, jak używać aplikacji Mobile usługi aplikacji do pamięci podręcznej i synchronizacja danych w trybie offline w aplikacji Cordova"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Włączanie synchronizacja w trybie offline dla swojego Cordova aplikacji dla urządzeń przenośnych

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Ten samouczek wprowadzono funkcję synchronizacja w trybie offline w aplikacji Mobile Azure Cordova. Synchronizacja w trybie offline umożliwia użytkownikom końcowym współdziałanie z aplikacji dla urządzeń przenośnych&mdash;wyświetlania, dodawania i modyfikowania danych&mdash;nawet wtedy, gdy jest brak połączenia z siecią. Zmiany są zapisywane w lokalnej bazy danych; Po powrocie do trybu online urządzenia te zmiany są synchronizowane z usługą zdalny.

Ten samouczek jest oparty na rozwiązanie Cordova Szybki Start dla aplikacji Mobile, utworzony po zakończeniu samouczek [Apache Cordova szybki start]. W tym samouczku zostanie zaktualizowany rozwiązanie Szybki Start, aby dodać offline funkcje aplikacji Mobile Azure. Firma Microsoft wyróżni również kod specyficzny dla trybu offline w aplikacji.

Aby dowiedzieć się więcej na temat funkcji synchronizacja w trybie offline, zobacz temat [Synchronizacja danych w trybie Offline w aplikacji Mobile Azure]. Aby uzyskać szczegółowe informacje o użycie interfejsu API Zobacz pliku [README] w dodatku.

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Dodawanie synchronizacja w trybie offline na rozwiązanie Szybki Start

Kod synchronizacja w trybie offline musi zostać dodana do aplikacji. Synchronizacja w trybie offline wymaga wtyczki cordova sqlite magazynowania otrzymuje podczas dodawane automatycznie do aplikacji wtyczkę aplikacji Mobile Azure znajduje się w projekcie. Projekt Szybki Start zawiera zarówno te dodatki.

1. W oknie Eksplorator rozwiązań Visual Studio Otwórz index.js i zamień następujący kod

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    Kod:

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. Następnie należy zamienić następujący kod:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    Kod:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    Dodatki z poprzedniego kodu zainicjować magazynu lokalnego i zdefiniować tabelę lokalną odpowiadającą wartości kolumn używanych w usługi Azure wewnętrzna baza danych. (Nie musisz uwzględnić wszystkie wartości kolumny w tym kodu).

    Możesz odwołać się do kontekstu synchronizacji, dzwoniąc **getSyncContext**. Kontekst synchronizacji pomaga zachować relacji między tabelami, śledzenia i naciśnięcie zmian we wszystkich tabelach, które aplikacji klienckiej zmodyfikował po **push** .

3. Zaktualizuj adres URL aplikacji do adresu URL aplikacji w aplikacji Mobile.

4. Następnie należy zamienić ten kod:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    Kod:

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Poprzednim kodzie inicjuje kontekstu synchronizacji, a następnie wywołuje getSyncTable (zamiast getTable), aby odwołać się do lokalnej tabeli.

    Ten kod przy użyciu lokalnej bazy danych dla wszystkich tworzenia, odczytu, aktualizowanie i usuwanie operacje tabeli (OBSŁUGIWAŁ).

    W tym przykładzie wykonuje proste obsługi na konfliktów synchronizacji błędów. Rzeczywistej aplikacji może obsługiwać różne błędy takie jak parametry sieci, konflikty serwera i innych osób. Aby uzyskać przykłady kodu zobacz [Przykładowe synchronizacja w trybie offline].

5. Następnie dodaj ta funkcja przeprowadzić synchronizacji rzeczywiste.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Możesz określić, kiedy należy przekazać zmiany do wewnętrznej bazy danych aplikacji Mobile, dzwoniąc **wypychanych** w obiekcie **syncContext** używany przez klienta. Na przykład numer telefonu, aby **syncBackend** można dodać do przycisku obsługi zdarzenia w aplikacji, takiej jak nowy przycisk Synchronizuj lub można dodać wywołanie funkcji **addItemHandler** zsynchronizować zawsze, gdy zostanie dodany nowy element.

##<a name="offline-sync-considerations"></a>Zagadnienia dotyczące synchronizacja w trybie offline

W próbce metodę **wypychanych** **syncContext** nazywa się tylko podczas uruchamiania aplikacji w funkcji zwrotnego logowania.  W aplikacji rzeczywistych można również wykorzystać tej funkcji synchronizacji wywołane ręcznie lub zmiany stan sieci.

Podczas wykonywania pobieraj przed tabelę, która ma oczekujące aktualizacje lokalne śledzone według kontekstu tej operacji pobieraj automatycznie spowoduje poprzedniego wypychanych kontekstu. Podczas odświeżania, dodając i wykonywania elementów w tym przykładzie można pominąć połączenia jawne **wypychanych** czasu może być zbędne (pierwszy sprawdzanie [— Plik README] dla bieżącego stanu funkcji).

W kodzie dostarczonych wszystkie rekordy w tabeli todoItem zdalny są proszeni, ale jest również można filtrować rekordy przekazując identyfikator kwerendy i kwerendy do **wypychane**. Aby uzyskać więcej informacji zobacz sekcję *Przyrostowe synchronizacji* w [Trybie Offline synchronizacja danych w aplikacji Mobile Azure].

## <a name="optional-disable-authentication"></a>(Opcjonalnie) Wyłączanie uwierzytelniania

Jeśli nie zostało już skonfigurowane uwierzytelniania i nie chcesz, aby skonfigurować uwierzytelnianie przed testowania synchronizacja w trybie offline, komentarz takiej funkcji dla logowania, ale pozostawić kod wewnątrz funkcji zwrotnego uncommented.

Kod powinien wyglądać podobnie do następującej po komentowania wierszy logowania.

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Teraz aplikacja będzie synchronizowany z Azure wewnętrznej bazy danych po uruchomieniu aplikacji zamiast podczas logowania.

## <a name="run-the-client-app"></a>Uruchom aplikację klienta

Teraz włączona synchronizacja w trybie offline umożliwia teraz uruchamianie aplikacji klienckiej przynajmniej raz na każdej z tych platform do wypełniania bazy danych magazynu lokalnego. Później będzie symulować scenariusz w trybie offline i zmodyfikować dane w magazynie lokalnej, gdy aplikacja jest w trybie offline.

## <a name="optional-test-the-sync-behavior"></a>(Opcjonalnie) Testowanie zachowanie synchronizacji

W tej sekcji należy zmodyfikować projektu klienta w celu zasymulowania scenariusz w trybie offline, używając nieprawidłowy adres URL aplikacji do wewnętrznej bazy danych. Gdy dodajesz lub zmieniasz elementów danych, te zmiany będą przechowywane w magazynie lokalnej, ale nie zostały zsynchronizowane z magazynu danych wewnętrznej bazy danych, dopóki ponownym nawiązaniu połączenia.

1. W Eksploratorze rozwiązań Otwórz plik projektu index.js i zmień adres URL aplikacji, aby wskazywały nieprawidłowy adres URL, podobnej do następującej:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. Index.html aktualizacji dostawcy `<meta>` elementu z tego samego adresu URL nieprawidłowe.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Tworzenie i uruchamianie aplikacji klienta i zwróć uwagę, że w konsoli rejestrowane wyjątków, gdy aplikacja próbuje zsynchronizować z wewnętrznej bazy danych po logowania. Nowe elementy, które można dodać istnieje tylko w magazynie lokalnej do momentu może zostać przeniesiony do urządzeń przenośnych wewnętrznej bazy danych. Aplikacja klienta zachowuje się jak, jeśli jest podłączony do wewnętrznej bazy danych, pomocnicze utworzone przez wszystkich, odczytu, aktualizacji, operacji usuwania.

4. Zamknij aplikację i ponownie go, aby sprawdzić, czy nowe elementy utworzone są zachowywane do magazynu lokalnego.

5. (Opcjonalnie) Aby wyświetlić tabelę bazy danych SQL Azure, aby sprawdzić, czy nie zmieniono danych w bazie danych wewnętrznej bazy danych za pomocą programu Visual Studio.

    W programie Visual Studio Otwórz **Eksploratora serwera**. Przejdź do bazy danych platformy **Azure**->**Bazy danych programu SQL**. Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz polecenie **Otwórz w Eksploratorze obiektów SQL Server**. Teraz można przejść do tabeli bazy danych SQL i jego zawartość.


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Opcjonalnie) Testowanie ponowne do wewnętrznej bazy danych z urządzeń przenośnych

W tej sekcji jest opcjonalny aplikacji do urządzeń przenośnych wewnętrznej bazy danych, które symuluje aplikacji pochodzące z powrotem do trybu online. Gdy się zalogujesz, dane będą synchronizowane z wewnętrznej bazy danych z urządzeń przenośnych.

1. Otwórz ponownie index.js i poprawianie adres URL aplikacji, aby wskazywały poprawny adres URL.

2. Otwórz ponownie index.html i poprawianie adresu URL aplikacji w dostawcy `<meta>` elementu.

3. Odbudowywanie i uruchom aplikację klienta. Aplikacja próbuje zsynchronizować z aplikacji dla urządzeń przenośnych wewnętrznej bazy danych po logowania. Upewnij się, że bez wyjątków są rejestrowane w konsoli debugowania.

4. (Opcjonalnie) Wyświetl zaktualizowane dane przy użyciu Eksploratora obiektów programu SQL Server lub narzędzie pozostałych, takie jak Fiddler. Zwróć uwagę, że dane zostały synchronizowane między wewnętrzną bazę danych i magazynu lokalnego.

    Zwróć uwagę, dane zostały zsynchronizowane między bazy danych i magazynu lokalnego i zawiera elementy, które zostały dodane odłączeniu aplikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]

* [Strona tytułowa w chmurze: synchronizacja w trybie Offline w Azure usług mobilnych] \(Uwaga: klip wideo jest na telefon komórkowy usług, ale działa synchronizacja w trybie offline w podobny sposób jak w aplikacji Mobile Azure\)

* [Visual Studio Tools for Apache Cordova]

## <a name="next-steps"></a>Następne kroki

* Przyjrzyj się bardziej zaawansowanych funkcji synchronizacja w trybie offline, takich jak rozwiązywanie konfliktów w [próbce synchronizacja w trybie offline]
* Przyjrzyj się synchronizacja w trybie offline odwołanie interfejsu API na [— Plik README]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Szybki start Apache Cordova]: app-service-mobile-cordova-get-started.md
[PLIK README]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[Przykładowe synchronizacja w trybie offline]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]: app-service-mobile-offline-data-sync.md
[Strona tytułowa chmury: Synchronizacja w trybie Offline w Azure usług mobilnych]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
