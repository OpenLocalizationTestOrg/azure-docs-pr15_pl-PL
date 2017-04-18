<properties
    pageTitle="Włączanie synchronizacja w trybie offline dla aplikacji Mobile Azure (system iOS)"
    description="Dowiedz się, jak korzystać z aplikacji usługi Mobile aplikacji do pamięci podręcznej i synchronizacja danych w trybie offline w aplikacji systemu iOS"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>Włączanie synchronizacja w trybie offline dla aplikacji dla urządzeń przenośnych z systemem iOS

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Omówienie

Ten samouczek obejmuje funkcję synchronizacja w trybie offline aplikacji Azure Mobile dla systemu iOS. Synchronizacja w trybie offline umożliwia użytkownikom końcowym współdziałanie z aplikacji dla urządzeń przenośnych&mdash;wyświetlania, dodawania i modyfikowania danych&mdash;nawet wtedy, gdy jest brak połączenia z siecią. Zmiany są zapisywane w lokalnej bazy danych; Po powrocie do trybu online urządzenia te zmiany są synchronizowane z zdalnego wewnętrznej bazy danych.

Jeśli jest to pierwszy pracę w programie aplikacji Mobile Azure, najpierw należy wykonać samouczek [Tworzenie iOS aplikacji]. Jeśli nie korzystasz z programu project server pobrany szybki start, możesz dodać pakietów rozszerzenia danych programu access do projektu. Aby uzyskać więcej informacji na temat pakietów rozszerzenia serwera zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Aby dowiedzieć się więcej na temat funkcji synchronizacja w trybie offline, zobacz temat [Synchronizacja danych w trybie Offline w aplikacji Mobile Azure].

## <a name="review-sync"></a>Przejrzyj kod klienta synchronizacji

Projekt klienta, pobranego samouczek [Tworzenie iOS aplikacji] już zawiera kod obsługi synchronizacji w trybie offline za pomocą lokalnej bazy danych opartych na podstawowych danych. W tej sekcji przedstawiono podsumowanie już zawartość samouczka kod. Aby uzyskać omówienie funkcji zobacz [w trybie Offline synchronizacja danych w aplikacji Mobile Azure].

Funkcja synchronizacji synchronizacji danych w trybie offline aplikacji Mobile Azure umożliwia użytkownikom końcowym interakcję z lokalną bazą danych, gdy sieć nie jest dostępna. Aby korzystać z tych funkcji w aplikacji, zainicjować kontekście synchronizacji `MSClient` i odwołanie magazynu lokalnego. Następnie odwołać się do tabeli za pomocą `MSSyncTable` interfejsu.

1. **QSTodoService.m** (cel-C) lub **ToDoTableViewController.swift** (Swift), zwróć uwagę, typu członka `syncTable` jest `MSSyncTable`. Synchronizacja w trybie offline korzysta z tego interfejsu tabeli synchronizacji zamiast `MSTable`. W przypadku tabeli synchronizacji, wszystkie operacje przejdź do magazynu lokalnego i tylko są synchronizowane z zdalnego wewnętrznej bazy danych przy użyciu jawnych i wypychania operacji.

    Aby odwołać się do tabeli synchronizacji, użyj metody `syncTableWithName` na `MSClient`. Aby usunąć funkcji synchronizacja w trybie offline, użyj `tableWithName` zamiast tego.

2. Zanim będzie można dokonać wszystkie operacje tabeli, musi być zainicjowany magazynu lokalnego. Poniżej przedstawiono odpowiedni kod. 
    
    **Cel C**:
    
    W `QSTodoService.init` metody:
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Swift**:
    
    W `ToDoTableViewController.viewDidLoad` metody:
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    Spowoduje to utworzenie magazynu lokalnego przy użyciu interfejsu `MSCoreDataStore`, której znajduje się w SDK aplikacje Mobile. Możesz zamiast tego zawiera różne magazynu lokalnego z zastosowaniem `MSSyncContextDataSource` Protocol (protokół). 
    
    Ponadto pierwszy parametr `MSSyncContext` służy do określania obsługi konfliktu. Ponieważ firma Microsoft zdać `nil`, możemy otrzymają obsługi konfliktu domyślne, które kończy się niepowodzeniem w przypadku dowolnej konfliktu.
    
3. Teraz dodamy operacji synchronizacji rzeczywiste i pobieranie danych z zdalnego wewnętrznej bazy danych.

    **Cel C**:
    
    `syncData`najpierw umieszcza nowe zmiany, następnie wywołuje `pullData` mają zostać pobrane dane z zdalnego wewnętrznej bazy danych. Z kolei, metodę `pullData` otrzymuje nowych danych, która jest zgodna z kwerendy:
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Swift**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    W wersji celem-C w `syncData`, najpierw nazywamy `pushWithCompletion` w kontekście synchronizacji. Ta metoda jest członkiem `MSSyncContext` (zamiast synchronizacji tabela samej) ponieważ przesunie zmiany na wszystkich tabel. Tylko te rekordy, które zostały zmodyfikowane w jakiś sposób lokalnie (za pomocą operacji CUD) będą wysyłane na serwerze. Następnie Pomocnik `pullData` nosi nazwę, która połączeń `MSSyncTable.pullWithQuery` do pobierania danych zdalnego i przechowywania lokalnej bazy danych.
    
    W wersji szybkiej jest nie połączenia do `pushWithCompletion`. Jest to spowodowane operacji push nie jest niezbędne. W przypadku zmiany oczekujące w kontekście synchronizacji dla tabeli, która jest wykonanie operacji push, przeciągnij zawsze problemów wypychanych najpierw. Jeśli masz więcej niż jedną tabelę synchronizacji jest jednak najlepiej jawnie wywołać wypychanych, aby upewnić się, że wszystko jest spójna wielu powiązanych tabel.
    
    W celu C i Swift wersjach, metodę `pullWithQuery` pozwala określić kwerendę, aby filtrować rekordy mają zostać pobrane. W tym przykładzie kwerenda po prostu pobiera wszystkie rekordy zdalnego `TodoItem` tabeli.
    
    Drugi parametr `pullWithQuery` jest używany na potrzeby *Synchronizacja przyrostowa*identyfikator kwerendy. Synchronizacja przyrostowa pobiera tylko te rekordy, które zostały zmodyfikowane od ostatniej synchronizacji przy użyciu rekordu `UpdatedAt` sygnatury czasowej (o nazwie `updatedAt` w magazynie lokalnym.) Identyfikator kwerendy powinien być opisowy ciąg, który jest unikatowy dla każdej kwerendy logicznych w aplikacji. Aby zrezygnować z synchronizacja przyrostowa, należy przekazać `nil` jako identyfikator kwerendy. Należy zauważyć, że może to być potencjalnie nieefektywne, ponieważ pobierze wszystkie rekordy na każdej operacji pobieraj.

5. Synchronizuje aplikacji celem-C, gdy mamy modyfikowanie lub dodawanie danych, użytkownik wykonuje gest odświeżania i na uruchamianie. Szybkie aplikacji synchronizuje, gdy użytkownik wykonuje gest odświeżania i na uruchamianie. 

Ponieważ synchronizuje aplikacji zawsze, gdy dane są zmodyfikowany (cel-C) lub zawsze, gdy aplikacja zostanie uruchomiony (cel C i Swift), aplikacji założono, że użytkownik jest w trybie online. W innej sekcji zostanie odpowiednio zaktualizowana aplikacji tak, aby użytkownicy będą mogli edytować nawet wtedy, gdy są w trybie offline.

## <a name="review-core-data"></a>Przeglądanie modelu danych podstawowych

W przypadku korzystania z pamięci trybu offline podstawowych danych, należy zdefiniować określonych tabel i pól w modelu danych. Aplikacja przykładowa już zawiera modelu danych przy użyciu odpowiedni format. W tej sekcji przeprowadzimy za pośrednictwem te tabele i jak są one używane.

- Otwórz **QSDataModel.xcdatamodeld**. Istnieją cztery tabele zdefiniowane — trzy, które są używane przez SDK, i jedną tabelę dla zadania elementy się:     *MS_TableOperations: do śledzenia elementów, które muszą być synchronizowane z serwerem    * MS_TableOperationErrors: do śledzenia wszystkie błędy, które występują podczas synchronizacji w trybie offline     *MS_TableConfig: do śledzenia ostatnio zaktualizowane czas ostatniej operacji synchronizacji dla wszystkich operacji pobieraj    * TodoItem : Do przechowywania zadań do wykonania. System kolumny **createdAt**, **updatedAt**i **wersji** jest opcjonalny system właściwości.

>[AZURE.NOTE] Nazwy kolumn zastrzega sobie SDK aplikacje Azure Mobile jest z "**``**". Nie należy używać tego prefiksu na nic innego niż kolumny systemowe, w przeciwnym razie modyfikacji nazwy kolumn podczas korzystania z zdalnego wewnętrznej bazy danych.

- Podczas korzystania z funkcji synchronizacja w trybie offline, należy zdefiniować tabele systemowe, tak jak pokazano poniżej.

    ### <a name="system-tables"></a>Tabele systemowe

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| Atrybut  |    Typ     |
  	|----------- |   ------    |
  	| Identyfikator         | Liczba całkowita 64  |
  	| Identyfikator elementu     | Ciąg      |
  	| właściwości | Dane binarne |
  	| Tabela      | Ciąg      |
  	| tableKind  | Liczba całkowita 16  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| Atrybut  |    Typ     |
  	|----------- |   ------    |
  	| Identyfikator         | Ciąg      |
  	| operationId | Liczba całkowita 64 |
  	| właściwości | Dane binarne |
  	| tableKind  | Liczba całkowita 16  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| Atrybut  |    Typ     |
  	|----------- |   ------    |
  	| Identyfikator         | Ciąg      |
  	| klawisz        | Ciąg      |
  	| keyType    | Liczba całkowita 64  |
  	| Tabela      | Ciąg      |
  	| wartość      | Ciąg      |

    ### <a name="data-table"></a>Tabela danych

    **TodoItem**

  	| Atrybut    |  Typ   | Uwaga                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| Identyfikator           | Ciąg oznaczona jako wymagana  | klucz podstawowy w magazynie zdalnym                            |
  	| Kończenie     | Wartość logiczna | pola elementu zadania                                        |
  	| tekst         | Ciąg  | pola elementu zadania                                        |
  	| createdAt | Data    | (opcjonalnie) mapy właściwość systemu createdAt         |
  	| updatedAt | Data    | (opcjonalnie) mapy właściwość systemu updatedAt         |
  	| Wersja   | Ciąg  | (opcjonalnie) używane do wykrywania konfliktów, mapy do wersji |


## <a name="setup-sync"></a>Zmienianie zachowania synchronizacji aplikacji

W tej sekcji należy zmodyfikować aplikację tak, aby nie synchronizuje po uruchomieniu aplikacji lub podczas wstawiania i aktualizowanie elementów, ale tylko wtedy, gdy jest wykonywane gest przycisk Odśwież.

**Cel C**:

1. W **QSTodoListViewController.m**zmienić metodę **viewDidLoad** , aby usunąć połączenie `[self refresh]` na końcu metody. Teraz dane nie będą synchronizowane z serwerem po uruchomieniu aplikacji, ale zamiast niego będzie zawartość magazynu lokalnego.

2. W **QSTodoService.m**modyfikowania definicji `addItem` tak, aby go nie można zsynchronizować po wstawieniu elementu. Usuwanie `self syncData` zablokować, a następnie Zamień na następujące czynności:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. Modyfikowanie definicji `completeItem` co powyżej; Usuwanie bloku dla `self syncData` i zamienić na następujące czynności:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Swift**:

1. W `viewDidLoad` **ToDoTableViewController.swift**skomentować te dwa wiersze, aby zatrzymać synchronizację po uruchomieniu aplikacji. Podczas pisania w tym artykule aplikacji Swift zadania nie jest aktualizowany usługę, gdy ktoś doda lub wykonuje elementu tylko po uruchomieniu aplikacji.

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="test-app"></a>Testowanie aplikacji

W tej sekcji połączy do nieprawidłowego adresu URL w celu zasymulowania scenariusz w trybie offline. Po dodaniu elementów danych one będą przechowywane w magazynie lokalnej podstawowych danych, ale nie zostały zsynchronizowane z urządzeń przenośnych wewnętrznej bazy danych.

1. Zmienianie adresu URL aplikacji Mobile w **QSTodoService.m** do nieprawidłowego adresu URL, a następnie ponownie uruchom aplikację:

    **Cel C** QSTodoService.m:
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    **Swift** w ToDoTableViewController.swift:

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Dodawanie niektórych zadań do wykonania. Zamykanie simulator (lub wymusić zamknięcie aplikacji) i uruchom ponownie. Upewnij się, że trwają zostały wprowadzone zmiany.

3. Wyświetlanie zawartości tabeli zdalnej TodoItem:

    + Dla Node.js wewnętrznej bazie danych, przejdź do [portalu Azure](https://portal.azure.com/)i w aplikacji Mobile wewnętrznej bazy danych kliknij pozycję **Łatwe tabele** > **TodoItem** , aby wyświetlić zawartość `TodoItem` tabeli.
    + Służy do wyświetlania spisu treści jako narzędzie SQL, takie jak SQL Server Management Studio lub klienta pozostałych, takich jak Fiddler lub Postman dla .NET wewnętrznej bazy danych.

    Sprawdź, czy nowe elementy *nie* zostały zsynchronizowane z serwerem:

4. Zmienianie adresu URL do odpowiedniego na w **QSTodoService.m** i ponownie uruchom aplikację. Wykonywanie gest odświeżania, przenosząc je w dół na liście elementów. Zostanie wyświetlona pokrętła postępu.

5. Wyświetlanie danych TodoItem ponownie. TodoItems nowych i zmienionych powinna zostać wyświetlona.

## <a name="summary"></a>Podsumowanie

Aby można było obsługuje funkcję synchronizacja w trybie offline, użyliśmy `MSSyncTable` interfejs i zainicjować `MSClient.syncContext` z magazynu lokalnego. W tym przypadku magazynu lokalnego było oparte na podstawowych danych bazy danych.

Podczas korzystania z magazynu lokalnego podstawowych danych, należy zdefiniować kilka tabel z [Właściwości systemu poprawne](#review-core-data).

Normalny operacji OBSŁUGIWAŁ aplikacje Mobile Azure działa tak, jakby aplikacji nadal jest połączony, ale wszystkie operacje wystąpić przed magazynu lokalnego.

Gdy trzeba magazynu lokalnego są synchronizowane z serwerem, użyliśmy `MSSyncTable.pullWithQuery`metody.


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]

* [Motywacyjnego w chmurze: synchronizacja w trybie Offline w Azure usług mobilnych] \(Uwaga: klip wideo jest na telefon komórkowy usług, ale działa synchronizacja w trybie offline w podobny sposób jak w aplikacji Mobile Azure\)

<!-- URLs. -->


[Tworzenie aplikacji systemu iOS]: app-service-mobile-ios-get-started.md
[Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Strona tytułowa chmury: Synchronizacja w trybie Offline w Azure usług mobilnych]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
