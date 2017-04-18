<properties
    pageTitle="Włączanie synchronizacja w trybie offline dla aplikacji Mobile Azure (system Android)"
    description="Dowiedz się, jak korzystać z aplikacji usługi Mobile aplikacji do pamięci podręcznej i synchronizacja danych w trybie offline w aplikacji systemu Android"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Włączanie synchronizacja w trybie offline dla aplikacji dla urządzeń przenośnych z systemem Android

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Omówienie

Ten samouczek obejmuje funkcję synchronizacja w trybie offline w aplikacji Mobile Azure dla systemu Android. Synchronizacja w trybie offline umożliwia użytkownikom końcowym korzystać z aplikacji dla urządzeń przenośnych&mdash;wyświetlania, dodawania i modyfikowania danych&mdash;nawet wtedy, gdy jest brak połączenia z siecią. Zmiany są przechowywane w lokalnej bazy danych. Po powrocie do trybu online urządzenia te zmiany są synchronizowane z zdalnego wewnętrznej bazy danych.

Jeśli jest to pierwszy pracę w programie aplikacji Mobile Azure, najpierw należy wykonać samouczek [utworzyć aplikację Android]. Jeśli nie korzystasz z programu project server pobrany szybki start, możesz dodać pakietów rozszerzenia danych programu access do projektu. Aby uzyskać więcej informacji na temat pakietów rozszerzenia serwera zobacz [Praca z serwera wewnętrznej bazy danych programu .NET SDK dla aplikacji Mobile Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Aby dowiedzieć się więcej na temat funkcji synchronizacja w trybie offline, zobacz temat [Synchronizacja danych w trybie Offline w aplikacji Mobile Azure].

## <a name="update-the-app-to-support-offline-sync"></a>Aktualizowanie aplikacji do pomocy technicznej synchronizacja w trybie offline

Z synchronizacja w trybie offline do odczytu i zapisu *synchronizacji tabeli* (za pomocą interfejsu *IMobileServiceSyncTable* ), która stanowi część **SQLite** bazy danych na urządzeniu.

Aby i wypychania zmiany między urządzenia i usługi Azure Mobile, należy użyć *kontekstu synchronizacji* (*MobileServiceClient.SyncContext*), który zainicjować z lokalnej bazy danych, przechowywanie danych lokalnie.

1. W `TodoActivity.java`, komentarz się definicja `mToDoTable` i Usuń komentarze wersja tabeli synchronizacji:

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. W `onCreate` metoda komentarz się istniejących inicjowanie `mToDoTable` i Usuń komentarze tej definicji:

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. W `refreshItemsFromTable` komentarz się definicji `results` i Usuń komentarze tej definicji:

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. Komentarz się definicji `refreshItemsFromMobileServiceTable`.

5. Usuń komentarze definicji `refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. Usuń komentarze definicji `sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Testowanie aplikacji

W tej sekcji Testowanie zachowanie przy użyciu sieci Wi-Fi na, a następnie wyłącz opcję sieci Wi-Fi, aby utworzyć scenariusz w trybie offline.

Po dodaniu elementów danych są przechowywane w magazynie SQLite lokalnej, ale nie zostały zsynchronizowane z usług mobilnych, aż naciśnij przycisk **Odśwież** . Inne aplikacje mogą mieć inne wymagania dotyczące gdy dane mają być synchronizowane, ale w celach pokaz ten samouczek ma jawne żądanie użytkownika.

Po naciśnięciu przycisku uruchamia nowego zadania w tle. Umieszcza go najpierw wszystkie zmiany wprowadzone do magazynu lokalnego przy użyciu kontekstu synchronizacji, a następnie ściąga zmienione dane z platformy Azure lokalnej tabeli.

### <a name="offline-testing"></a>Testowanie trybu offline

1. Umieść urządzenie lub simulator w *Trybie samolotem*. Spowoduje to utworzenie scenariusz w trybie offline.

2. Dodawanie niektórych *zadań do* wykonania lub oznaczyć niektóre elementy jako ukończone. Zamykanie urządzenia lub simulator (lub wymusić zamknięcie aplikacji) i uruchom ponownie. Sprawdź, czy zmiany zostały umieszczone na tym urządzeniu, ponieważ są one przechowywane w magazynie SQLite lokalnej.

3. Wyświetlanie zawartości tabeli Azure *TodoItem* albo SQL narzędzia, takie jak *SQL Server Management Studio*lub klienta pozostałych, takich jak *Fiddler* lub *Postman*. Sprawdź, czy nowe elementy _nie_ zostały zsynchronizowane na serwerze

    + Dla Node.js wewnętrznej bazie danych, przejdź do [portalu Azure](https://portal.azure.com/)i w aplikacji Mobile wewnętrznej bazy danych kliknij pozycję **Łatwe tabele** > **TodoItem** , aby wyświetlić zawartość `TodoItem` tabeli.
    + Służy do wyświetlania spisu treści jako narzędzie SQL, takie jak *SQL Server Management Studio*lub klienta pozostałych, takich jak *Fiddler* lub *Postman*dla .NET wewnętrznej bazy danych.

4. Włącz funkcję sieci Wi-Fi na urządzeniu lub simulator. Następnie kliknij przycisk **Odśwież** .

5. Wyświetlanie danych TodoItem ponownie Azure portal. TodoItems nowych i zmienionych powinna zostać wyświetlona.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]

* [Motywacyjnego w chmurze: synchronizacja w trybie Offline w Azure usług mobilnych] \(Uwaga: klip wideo jest na telefon komórkowy usług, ale działa synchronizacja w trybie offline w podobny sposób jak w aplikacji Mobile Azure\)


<!-- URLs. -->

[Synchronizowanie danych w trybie offline w aplikacji dla urządzeń przenośnych Azure]: app-service-mobile-offline-data-sync.md

[Tworzenie aplikacji programu systemu Android]: app-service-mobile-android-get-started.md

[Strona tytułowa chmury: Synchronizacja w trybie Offline w Azure usług mobilnych]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

