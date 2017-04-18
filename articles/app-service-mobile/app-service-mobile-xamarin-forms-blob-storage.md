<properties
    pageTitle="Nawiązywanie połączenia z magazynem Azure w aplikacji Xamarin.Forms"
    description="Dodawanie obrazów do zadania listy Xamarin.Forms aplikacji dla urządzeń przenośnych za pomocą połączenia z magazynem obiektów blob platformy Azure"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
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

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>Nawiązywanie połączenia z magazynem Azure w aplikacji Xamarin.Forms

## <a name="overview"></a>Omówienie

Aplikacje Mobile Azure klienta i serwera SDK pomocy technicznej synchronizacja w trybie offline dane strukturalne z OBSŁUGIWAŁ operacje punkt końcowy /tables. Zazwyczaj ten dane są przechowywane w bazie danych lub podobne ze sklepu i zazwyczaj te Sklepy danych nie można przechowywać dużych danych binarnych wydajność. Ponadto niektóre aplikacje pokrewne danych przechowywanych w innej lokalizacji (na przykład, magazyn obiektów blob, udziały plików), która jest przydatne można było utworzyć skojarzenia między rekordami w punkt końcowy /tables i innych danych.

W tym temacie pokazano, jak dodać obsługę obrazów do Szybki Start listy zadania aplikacji Mobile. Najpierw musisz wykonać samouczek [Tworzenie aplikacji Xamarin.Forms].

W tym samouczku utworzysz konto miejsca do magazynowania i dodać parametry połączenia do Twojej aplikacji Mobile wewnętrznej bazy danych. Następnie dodasz nowe dziedziczenie nowy typ aplikacji Mobile `StorageController<T>` do programu project server.

>[AZURE.TIP] Ten samouczek zawiera [Przykładowe companion](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/) dostępna, który można wdrożyć do Azure konta. 

## <a name="prerequisites"></a>Wymagania wstępne

* Wykonywanie samouczka [Tworzenie aplikacji Xamarin.Forms] listy wstępne. W tym artykule używa złożonym aplikację z tego samouczka.

>[AZURE.NOTE] Jeśli chcesz rozpocząć pracę z Azure aplikacji usługi, aby utworzyć konto Azure, przejdź do [Spróbuj aplikacji usługi](https://tryappservice.azure.com/?appServiceName=mobile). Możesz od razu utworzyć aplikacji dla urządzeń przenośnych krótkotrwałe starter w aplikacji usługi — karty kredytowej wymagane i nie zobowiązań.

## <a name="create-a-storage-account"></a>Tworzenie konta miejsca do magazynowania

1. Utwórz konto miejsca do magazynowania, wykonując samouczek [utworzyć konto Azure miejsca do magazynowania]. 

2. W portalu Azure przejdź do swojego konta nowo utworzonego miejsca do magazynowania i kliknij ikonę **klawiszy** . Skopiuj **Parametry połączenia podstawowego**.

3. Przejdź do swojej wewnętrznej bazy danych aplikacji dla urządzeń przenośnych. W obszarze **Wszystkie ustawienia** -> **Ustawienia aplikacji** -> **Parametry połączenia**, Utwórz nowy klucz o nazwie `MS_AzureStorageAccountConnectionString` i użyj wartości z pola skopiowany z Twojego konta miejsca do magazynowania. Użyj **niestandardowych** jako typ klucza.

## <a name="add-a-storage-controller-to-the-server"></a>Dodawanie kontrolera miejsca do magazynowania na serwerze

Musisz dodać nowy kontroler do Projekt serwera, który będzie odpowiadania na wezwania do tokenu skojarzeń zabezpieczeń magazyn Azure, a także zwraca listę plików, które odpowiadają do rekordu:

- [Dodawanie kontroler magazynu do programu project server](#add-controller-code)
- [Trasy zarejestrowane przez kontrolera magazynu](#routes-registered)
- [Komunikacja klienta i serwera](#client-communication)

###<a name="add-controller-code"></a>Dodawanie kontroler magazynu do programu project server

1. W programie Visual Studio Otwórz projekt serwera .NET. Dodawanie pakietu Nuget [Microsoft.Azure.Mobile.Server.Files]. Pamiętaj zaznaczyć **obejmują wstępną**.

2. W programie Visual Studio Otwórz projekt serwera .NET. Kliknij prawym przyciskiem myszy folder **kontrolerów** i wybierz polecenie **Dodaj** -> **Kontroler** -> **Kontroler 2 interfejsu API sieci Web — puste**. Nadaj nazwę administratora `TodoItemStorageController`.

3. Dodaj następujący przy użyciu instrukcji:

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. Zmiana klasy podstawowej do `StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. Do klasy, należy dodać następujących metod:

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. Aktualizowanie Konfiguracja interfejs API sieci Web, aby skonfigurować atrybut routing. W **Startup.MobileApp.cs**, Dodaj poniższy wiersz, aby `ConfigureMobileApp()` metody po definicji `config` zmiennej:

        config.MapHttpAttributeRoutes();

7. Publikowanie projektu server do wewnętrznej bazy danych aplikacji dla urządzeń przenośnych.

###<a name="routes-registered"></a>Trasy zarejestrowane przez kontrolera magazynu

Nowy `TodoItemStorageController` udostępnia dwa zasoby podrzędnego w rekordzie zarządza:

- StorageToken

    + HTTP POST: Tworzy token miejsca do magazynowania
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP GET: Pobiera listę plików skojarzone z rekordem
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + Usuń HTTP: Usuwa pliku określonego w pliku identyfikator zasobu
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="client-communication"></a>Komunikacja klienta i serwera

Należy zauważyć, że `TodoItemStorageController` czy *nie* mają trasy do przekazywania lub pobierania obiektów blob. Wynika to z klienta przenośnego interakcji z blob miejsca do magazynowania *bezpośrednio* w celu wykonania tych operacji po pierwszym wprowadzenie token skojarzeń zabezpieczeń (udostępnione dostępu podpis) do uzyskiwania bezpiecznego dostępu do określonego blob lub kontener. Jest to ważne architektura projektu w inny sposób dostępu do magazynu może być ograniczone przez skalowalność i dostępność przenośnych wewnętrznej bazy danych. Zamiast tego za pomocą połączenia bezpośrednio do magazynu Azure, klienta przenośnego można korzystać z jej funkcje, takie jak podziału automatyczne i geo rozkładu.

Podpis udostępnienia udostępnia delegowanych zasobów na koncie miejsca do magazynowania. Oznacza to, że możesz przyznać że klient ograniczone uprawnienia do obiektów na koncie miejsca do magazynowania przez określony czas i określony zestaw uprawnień, bez konieczności udostępniania kluczy dostępu do konta. Aby uzyskać więcej informacji, zobacz [Opis udostępnionych dostępu podpisy].

Na poniższym diagramie pokazano interakcji klienta i serwera. Przed przekazaniem pliku klient żąda tokenu skojarzenia zabezpieczeń z usługi. Usługi używane miejsca do magazynowania parametry połączenia w celu wygenerowania nowe skojarzenia zabezpieczeń, która następnie zwraca do klienta. Skojarzenia zabezpieczeń jest czas i ogranicza uprawnienia do tylko określonego pliku lub kontener. Klienta przenośnego następnie używa tej skojarzeń zabezpieczeń i client magazyn Azure SDK przekazać plik z magazynem obiektów blob.

![Żądanie tokenu skojarzenia zabezpieczeń](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>Aktualizowanie aplikacji klienckich Dodawanie obsługi obrazów

Otwórz projekt Szybki Start Xamarin.Forms w programie Visual Studio lub Xamarin Studio. Zainstalujesz Nuget pakietów i aktualizowanie projektu portable biblioteki oraz iOS, Android i okien projekty klientów:

- [Dodawanie pakietów Nuget](#add-nuget)
- [Dodawanie interfejsu IPlatform](#add-iplatform)
- [Dodawanie klasy FileHelper](#add-filehelper)
- [Dodawanie obsługi synchronizacji plików](#file-sync-handler)
- [Aktualizowanie TodoItemManager](#update-todoitemmanager)
- [Dodawanie widoku szczegółów](#add-details-view)
- [Aktualizowanie Widok główny](#update-main-view)
- [Aktualizowanie projektu systemu Android](#update-android), [iOS projektu](#update-ios), [Projekt systemu Windows](#update-windows)

>[AZURE.NOTE] Ten samouczek zawiera tylko instrukcji dla systemu Android, iOS i platform ze Sklepu Windows, nie Windows Phone.

###<a name="add-nuget"></a>Dodawanie pakietów Nuget

Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Zarządzanie Nuget pakietów rozwiązania**. Dodaj następujące pakiety Nuget dla **wszystkich** projektów w rozwiązaniu. Pamiętaj sprawdzić **obejmują wstępną**.

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

Dla wygody w przykładzie użyto biblioteki [PCLStorage] , ale nie jest wymagane przez klienta aplikacji Mobile Azure SDK.

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="add-iplatform"></a>Dodawanie interfejsu IPlatform

Tworzenie nowego interfejsu `IPlatform` w projekcie głównym portable biblioteki. Wynika to ze wzorcem [Xamarin.Forms DependencyService] załadować klasy prawo specyficzne dla platformy w czasie rzeczywistym. Później dodasz implementacji specyficzne dla platformy w każdej z projektami klienta.

1. Dodaj następujący przy użyciu instrukcji:

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. Zastąp wykonania następujących czynności:

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="add-filehelper"></a>Dodawanie klasy FileHelper

1. Tworzenie nowej klasy `FileHelper` w projekcie głównym portable biblioteki. Dodaj następujący przy użyciu instrukcji:

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. Dodawanie definicji klasy:

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="file-sync-handler"></a>Dodawanie obsługi synchronizacji plików

Tworzenie nowej klasy `TodoItemFileSyncHandler` w projekcie głównym portable biblioteki. Ta klasa zawiera zwrotne z SDK Azure w celu powiadamiania o kodzie, gdy plik zostanie dodany lub usunięty.

Azure Mobile Client SDK przechowuje wszelkie dane pliku: klient SDK wywoła implementacji z `IFileSyncHandler` która z kolei określa czy i w jaki sposób pliki są przechowywane na urządzeniem lokalnym.

1. Dodaj następujący przy użyciu instrukcji:

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. Zastąp definicji klasy z następujących czynności: 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="update-todoitemmanager"></a>Aktualizowanie TodoItemManager

1. W **TodoItemManager.cs**, Usuń komentarze linię `#define OFFLINE_SYNC_ENABLED`.

2. W **TodoItemManager.cs**, Dodaj następujący przy użyciu instrukcji:

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. W Konstruktorze `TodoItemManager`, Dodaj następujący po połączenie `DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. W Konstruktorze Zamień połączenie `InitializeAsync` wpisem. Temu będziesz mieć pewność, że istnieje zwrotne modyfikacji rekordów w magazynie lokalnym. Funkcja synchronizacji plików używa tych zwrotne wyzwalać do obsługi synchronizacji plików.

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. W `SyncAsync()`, Dodaj następujący po połączenie `PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. Dodaj następujące metody `TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="add-details-view"></a>Dodawanie widoku szczegółów

W tej sekcji należy dodać nowy widok Szczegóły dla pozycji zadania. Widok jest tworzona po wybraniu pozycji zadania i pozwala nowe obrazy, które mają zostać dodane do elementu.

1. Dodawanie nowej klasy **TodoItemImage** do projektu portable biblioteki wykonania następujących czynności:

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. Edytowanie **App.cs**. Zamienianie inicjowania `MainPage` z następujących czynności:
    
        MainPage = new NavigationPage(new TodoList());

3. W **App.cs**Dodaj następujące właściwości:

        public static object UIContext { get; set; }

4. Kliknij prawym przyciskiem myszy projektu portable biblioteki i wybierz polecenie **Dodaj** -> **Nowy element** -> **między platformami** -> **Strony Xaml formularzy**. Nadaj nazwę widoku `TodoItemDetailsView`.

5. Otwórz **TodoItemDetailsView.xaml** i zastąpić treści wartość ContentPage następujące czynności:

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. Edytowanie **TodoItemDetailsView.xaml.cs** i Dodaj następujący przy użyciu instrukcji:

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. Zamienianie stosowania `TodoItemDetailsView` z następujących czynności:

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="update-main-view"></a>Aktualizowanie Widok główny 

Aktualizowanie głównym widoku, aby otworzyć widok Szczegóły po wybraniu pozycji zadania.

W **TodoList.xaml.cs**Zamień stosowania `OnSelected` z następujących czynności:

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="update-android"></a>Aktualizowanie projektu systemu Android

Dodaj do Android projektu, łącznie z kodem pobierania pliku i przy użyciu aparatu fotograficznego, aby przechwycić obraz nowej kod specyficzny dla platformy. 

Ten kod zawiera Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/) załadować klasy prawo specyficzne dla platformy w czasie rzeczywistym.

1. Dodaj składnik **Xamarin.Mobile** Android projektu.

2. Dodawanie nowej klasy `DroidPlatform` wykonania następujących czynności. Zamień "YourNamespace" nazw głównym projektu.

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Edytowanie **MainActivity.cs**. W `OnCreate`, Dodaj następujący przed połączenie `LoadApplication()`:

        App.UIContext = this;

###<a name="update-ios"></a>Aktualizowanie projektu systemu iOS

Dodaj do projektu iOS kod specyficzny dla platformy.

1. Dodaj składnik **Xamarin.Mobile** projektu iOS.

2. Dodawanie nowej klasy `TouchPlatform` wykonania następujących czynności. Zamień "YourNamespace" nazw głównym projektu.

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Edytowanie **AppDelegate.cs** i Usuń komentarze połączenie `SQLitePCL.CurrentPlatform.Init()`.

###<a name="update-windows"></a>Aktualizowanie projektu systemu Windows

1. Zainstaluj rozszerzenie programu Visual Studio [SQLite dla systemu Windows 8.1](http://go.microsoft.com/fwlink/?LinkID=716919). Aby uzyskać więcej informacji zobacz samouczek [Włącz synchronizacja w trybie offline dla aplikacji systemu Windows](app-service-mobile-windows-store-dotnet-get-started-offline-data.md). 

2. Edytowanie **Package.appxmanifest** i sprawdzanie możliwości **kamery** .

3. Dodawanie nowej klasy `WindowsStorePlatform` wykonania następujących czynności. Zamień "YourNamespace" nazw głównym projektu.

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>Podsumowanie

W tym artykule opisano jak pracować z magazyn Azure za pomocą nowego Obsługa plików w Azure Mobile klienta i serwera SDK. 

- Tworzenie konta miejsca do magazynowania i Dodaj parametry połączenia do sieci wewnętrznej bazy danych aplikacji dla urządzeń przenośnych. Tylko do wewnętrznej bazy danych ma odpowiedni klucz do magazynu Azure: klienta przenośnego żądania tokenu skojarzeń zabezpieczeń (udostępnione podpis programu Access), zawsze, gdy trzeba ją uzyskać dostęp do magazynowania Azure. Aby dowiedzieć się więcej na temat tokeny skojarzeń zabezpieczeń w magazynie Azure, zobacz [Opis udostępnionych dostępu podpisy].

- Utworzyć kontroler tej podklasy `StorageController` w celu obsługi żądań token skojarzeń zabezpieczeń i pobieranie plików, które są skojarzone z rekordem. Domyślnie pliki są skojarzone z rekordem przy użyciu Identyfikatora rekordu jako część nazwy kontenera; zachowanie można dostosować, określając implementacja `IContainerNameResolver`. Można również dostosować zasady token skojarzeń zabezpieczeń.

- Nie są zapisywane SDK klienta Mobile Azure przechowuje dowolny plik danych. Zamiast klienta SDK wywołuje usługi `IFileSyncHandler`, co decyduje o treści jak (i jeżeli) pliki są przechowywane na tym urządzeniu lokalnym. Obsługa synchronizacji jest zarejestrowany w następujący sposób:

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`jest określana mianem Azure Mobile Client SDK reagować na plik danych (np. w ramach procesu przekazywania). Uzyskasz możliwość zarządzania jak (i jeżeli) pliki są przechowywane na tym urządzeniu lokalnym i zwrócić te informacje w razie potrzeby.

      + `IFileSyncHandler.ProcessFileSynchronizationAction`jest wywoływane w ramach przepływu synchronizacji pliku. Odwołanie do pliku i wartość wyliczenia FileSynchronizationAction są dostarczane, można określić sposób obsługi aplikacji to zdarzenie (np. automatyczne pobieranie pliku, zostanie utworzona lub zaktualizowana, usunięcie pliku z urządzeniem lokalnym, usunięcie tego pliku na serwerze).

- A `MobileServiceFile` może być używany w trybie online lub offline, używając `IMobileServiceTable` lub `IMobileServiceSyncTable`odpowiednio. W scenariusz w trybie offline, Przekaż wystąpi, gdy aplikacja `PushFileChangesAsync`. Ta opcja powoduje przetwarzanie; kolejki operacji w trybie offline dla każdej operacji pliku klienta Azure Mobile SDK wywoła `GetDataSource` metoda `IFileSyncHandler` wystąpienie pobrać zawartość pliku do przekazania.

- Aby można było pobrać plików elementów, połączenie "GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>` wystąpienie. Ta metoda zwraca listę plików skojarzoną z elementem danych dostępne. (Uwaga: to jest *lokalnych* operacji i zwróci pliki na podstawie stanu obiektu podczas był ostatnio synchronizowany. Aby uzyskać zaktualizowaną listę plików na serwerze, dlatego należy inicjować operacji synchronizacji najpierw.)

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- Funkcja synchronizacji pliku jest używana powiadomień o zmianach rekordu magazynu lokalnego było pobrać rekordy, które klienta otrzymano w ramach operacji lub wypychania. W tym celu należy, włączając lokalnych i serwerowych powiadomienia o przy użyciu kontekstu synchronizacji `StoreTrackingOptions` parametru. 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + Inne opcje śledzenia magazynu są dostępne, takie jak powiadomienia tylko lokalne lub tylko do serwera. Można dodawać lub właścicielem przy użyciu niestandardowej zwrotnego `EventManager` właściwości `IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- Istnieje możliwość dodania lub usunięcia plików z rekordu, modyfikując magazyn obiektów blob bezpośrednio, ponieważ zapewnia skojarzenia konwencji nazewnictwa. Jednak w tym przypadku należy zawsze **uaktualnić sygnaturę czasową rekordu modyfikacji skojarzone obiektów blob**. Klient Azure Mobile SDK zawsze aktualizuje rekord podczas dodawania lub usuwania pliku. 

    Powód to wymaganie jest niektórych klientów przenośnych będzie już rekordu w magazynu lokalnego. Po tych klientów przeprowadzić przyrostowe pobieraj, ten rekord nie zostaną zwrócone, a klient nie zwrócą dla nowych skojarzonych plików. Aby uniknąć tego problemu, zaleca się aktualizowanie sygnatury czasowej rekordu, podczas wykonywania jakiejkolwiek zmiany magazyn obiektów blob, która nie korzysta z klienta Azure Mobile SDK.

- Wzór [Xamarin.Forms DependencyService] jest używane w programie project klienta załadować klasy prawo specyficzne dla platformy w czasie wykonywania. W tym przykładzie możemy zdefiniowane interfejs `IPlatform` z implementacji w każdej z projektami specyficzny dla platformy.

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[Tworzenie aplikacji Xamarin.Forms]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[Opis udostępnionych podpisów w programie Access]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[Utwórz konto Azure miejsca do magazynowania]:  ../storage/storage-create-storage-account.md#create-a-storage-account
