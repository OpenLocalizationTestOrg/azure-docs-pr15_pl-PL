<properties 
    pageTitle="Aplikacja sieci Web z magazynem tabel (Node.js) | Microsoft Azure" 
    description="Samouczek, który tworzy w aplikacji sieci Web z samouczka Express przez dodanie usługi Magazyn Azure i moduł Azure." 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robmcm"/>

# <a name="nodejs-web-application-using-storage"></a>Node.js aplikacji sieci Web przy użyciu magazynu

## <a name="overview"></a>Omówienie

W tym samouczku będzie rozszerzanie aplikacji utworzone w samouczku [Node.js aplikacji sieci Web przy użyciu Express] przy użyciu bibliotek Microsoft Azure klienta dla Node.js do pracy z usługami zarządzania danymi. Może trwać aplikację, aby utworzyć aplikację listy zadań oparte na sieci web, którą można rozmieścić Azure. Lista zadań umożliwia użytkownikowi pobierania zadań, dodawanie nowych zadań i oznaczanie zadania jako ukończonego.

Elementy zadania są przechowywane w magazynie Azure. Magazyn Azure oferuje magazynowanie danych niestrukturalne, czyli odporność na uszkodzenia i wysokiej dostępności. Azure magazynowania zawiera kilka struktury danych, gdzie można przechowywać i danych programu access, a mogą korzystać z usług przechowywania interfejsy API, zawarte w Azure SDK dla Node.js lub za pośrednictwem interfejsów API pozostałych. Aby uzyskać więcej informacji zobacz [przechowywanie i uzyskiwanie dostępu do danych w Azure].

Tego samouczka przyjęto założenie, ukończono [Node.js aplikacji sieci Web] i [Node.js z Express]samouczki[Node.js aplikacji sieci Web przy użyciu Express] .

Dowiesz się:

-   Jak pracować z aparatu Jade szablonu
-   Jak pracować z usługami zarządzania danymi Azure

Zrzut ekranu przedstawiający wypełniony wniosek jest poniżej:

![Ukończony strony sieci web w programie internet explorer](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Ustawianie poświadczeń miejsca do magazynowania w Web.Config

Aby uzyskać dostęp do magazynowania Azure, musisz przekazać magazynu poświadczeń. Aby to zrobić, możesz korzystać z ustawień aplikacji web.config.
Te ustawienia będą przekazywane jako zmienne środowiska do węzła, które zostaną następnie odczytane przez zestaw SDK Azure.

> [AZURE.NOTE] Magazyn poświadczeń są używane tylko po wdrożeniu aplikacji Azure. Gdy działa w emulatorze, aplikacja będzie korzystać emulatora miejsca do magazynowania.

Wykonaj poniższe czynności, aby pobrać poświadczeń konta miejsca do magazynowania i dodać je do ustawień web.config:

1.  Jeśli nie jest jeszcze otwieranie, uruchom program Azure PowerShell z **Start** menu po rozwinięciu **Wszystkie programy, Azure**, kliknij prawym przyciskiem myszy **Azure programu PowerShell**, a następnie wybierz **Uruchom jako Administrator**.

2.  Zmień katalog do folderu zawierającego aplikację. Na przykład C:\\węzeł\\tasklist\\WebRole1.

3.  W oknie Azure programu Powershell wpisz następujące polecenie cmdlet do pobrania informacji o koncie miejsca do magazynowania:

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    To pobiera listę kont miejsca do magazynowania i konta klawiszy skojarzonego z usługą hostowanej.

    > [AZURE.NOTE] Ponieważ Azure SDK tworzy konto miejsca do magazynowania, podczas wdrażania usługi, konto miejsca do magazynowania powinna już istnieje z wdrażania aplikacji w poprzednim prowadnic.

4.  Otwórz plik **ServiceDefinition.csdef** zawierający ustawień środowiska, które są używane po wdrożeniu aplikacji Azure:

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  Wstaw następujący blok w obszarze elementu **środowiska** , zastępując {miejsca do magazynowania konta} i {klawisz dostępu miejsca do magazynowania} nazwą konta i klucza podstawowego dla konta miejsca do magazynowania, którego chcesz użyć do wdrożenia:

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![Zawartość pliku web.cloud.config](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  Zapisz plik, a następnie zamknij Notatnik.

### <a name="install-additional-modules"></a>Instalowanie dodatkowych modułów

2. Użyj następującego polecenia, aby zainstalować [azure], [uuid węzeł], [nconf] i modułów [asynchronicznych] lokalnie także jako można zapisać wpisu dla nich do pliku **package.json** :

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    Wynik tego polecenia powinna wyglądać podobnie do następującej:

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##<a name="using-the-table-service-in-a-node-application"></a>Za pomocą usługi tabeli w aplikacji węzeł

W tej sekcji może trwać zgłoszenie podstawowe utworzone za pomocą polecenia **ekspresowe** , dodając plik **task.js** , który zawiera modelu dla zadań. Zostaną również zmodyfikować istniejące **app.js** i utworzenie nowego pliku **tasklist.js** , która korzysta z modelu.

### <a name="create-the-model"></a>Tworzenie modelu

1. W katalogu **WebRole1** Utwórz nowy katalog o nazwie **modeli**.

2. W katalogu **modeli** Utwórz nowy plik o nazwie **task.js**. Ten plik zawiera modelu dla zadań utworzone przez aplikację.

3. Na początku pliku **task.js** , Dodaj następujący kod, aby odwołać bibliotek wymagane:

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Następnie dodasz kod, aby zdefiniować i eksportowania obiektu zadania. Ten obiekt jest odpowiedzialny za nawiązywanie połączenia z tabeli.

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Następnie dodaj następujący kod, aby zdefiniować dodatkowe metody obiektu zadania umożliwiające interakcje z danymi przechowywanymi w tabeli:

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };

            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. Zapisz i zamknij plik **task.js** .

### <a name="create-the-controller"></a>Tworzenie administratora

1. W katalogu **WebRole1 i przekierowuje** utworzenie nowego pliku o nazwie **tasklist.js** , a następnie otwórz go w edytorze tekstów.

2. Dodaj następujący kod do **tasklist.js**. Ładuje to moduły azure i asynchroniczne, które są używane przez **tasklist.js**. Definiuje również funkcji **TaskList** , który jest przekazywany wystąpienia obiektu **zadania** , możemy wcześniej zdefiniowanych przez:

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. Kontynuuj dodawanie do pliku **tasklist.js** , dodając metod **showTasks**, **addTask**i **completeTasks**:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this      
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

3. Zapisz plik **tasklist.js** .

### <a name="modify-appjs"></a>Modyfikowanie app.js

1. Otwórz plik **app.js** w edytorze tekstów w katalogu **WebRole1** . 

2. Na początku pliku, Dodaj poniższe czynności, aby załadować modułu azure i ustawianie klucza nazwy i partycją tabeli:

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. W pliku app.js przewiń w dół do miejsce, w którym zostanie wyświetlony następujący wiersz:

        app.use('/', routes);
        app.use('/users', users);

    Zamień powyżej wierszy kodu, jak pokazano poniżej. Spowoduje to zainicjować wystąpienie <strong>zadania</strong> za pomocą połączenia z kontem miejsca do magazynowania. To jest przekazywany do <strong>TaskList</strong>, które będą z niej korzystać można komunikować się z usługą tabeli:

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. Zapisz plik **app.js** .

### <a name="modify-the-index-view"></a>Modyfikowanie widoku indeksu

1. Zmienianie katalogów do katalogu **widoków** i Otwórz plik **index.jade** w edytorze tekstów.

2. Zamień zawartość pliku **index.jade** kod poniżej. Definiuje widok, aby wyświetlić istniejących zadań, a także formularza do dodawania nowych zadań i oznaczanie istniejących jako wykonane.

        extends layout

        block content
          h1= title
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if tasks != []
                tr
                  td 
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name: 
            input(name="item[name]", type="textbox")
            label Item Category: 
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. Zapisz i zamknij plik **index.jade** .

### <a name="modify-the-global-layout"></a>Modyfikowanie globalnej układu

Plik **layout.jade** w katalogu **Widoki** służy jako szablon globalny do innych plików **.jade** . W tym kroku należy zmodyfikować go, aby za pomocą [Serwisu Twitter początkowego](https://github.com/twbs/bootstrap), która jest zestaw narzędzi, który ułatwia projektowanie i witryny sieci Web wyglądzie.

1. Pobierz i Wyodrębnij pliki do [Serwisu Twitter początkowego](http://getbootstrap.com/). Skopiuj plik **bootstrap.min.css** z **uruchamiania\\dist\\css** folder, aby **publicznej\\arkusze stylów** katalogu aplikacji tasklist.

2. W folderze **Widoki** Otwórz **layout.jade** w edytorze tekstu i zastąpić zawartość z następujących czynności:

        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body.app
            nav.navbar.navbar-default
              div.navbar-header
                a.navbar-brand(href='/') My Tasks
            block content

3. Zapisz plik **layout.jade** .

### <a name="running-the-application-in-the-emulator"></a>Uruchamianie aplikacji w emulatorze

Użyj następującego polecenia, aby uruchomić aplikację w emulatorze.

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

W przeglądarce zostanie otwarty i zostanie wyświetlona strona następujące czynności:

![Sieć web stronicowanej Tytuł mojej listy zadań z tabelą zawierającą zadań i pola, aby dodać nowe zadanie.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

Dodawanie elementów za pomocą formularza lub usunąć istniejących elementów, oznaczając je jako ukończone.

## <a name="publishing-the-application-to-azure"></a>Publikowanie aplikacji Azure


W oknie programu Windows PowerShell Zadzwoń do następującego polecenia cmdlet, aby ponownie rozmieścić hostowanej usługi Azure.

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

Zamień **myuniquename** unikatową nazwę dla tej aplikacji. Zamień **datacentername** nazwę centrum danych Azure, takich jak **Zachodnia USA**.

Po zakończeniu rozmieszczania powinna być widoczna odpowiedzi podobny do następującego:

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

Jak poprzednio ponieważ określony **-Uruchamianie** opcji, zostanie otwarty w przeglądarce i wyświetla działanie aplikacji platformy Azure po zakończeniu publikowania.

![Okno przeglądarki zawierające strony Moje listy zadań. Adres URL wskazuje, że strona jest teraz obsługiwane Azure.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Zatrzymywanie i usuwanie aplikacji

Po wdrożeniu aplikacji, można ją wyłączyć, aby uniknąć kosztów lub tworzenie i wdrażanie inne aplikacje terminie bezpłatnej wersji próbnej.

Rachunki Azure web wystąpienia roli na godzinę zużyte czasu serwera.
Czas serwera jest używane po wdrożeniu aplikacji, nawet w przypadku wystąpienia nie działają i są w stanie zatrzymania.

Poniższe kroki pokazują jak zatrzymać i usuwanie aplikacji.

1.  W oknie programu Windows PowerShell zatrzymać wdrażanie usługi utworzonego w poprzedniej sekcji przy użyciu następującego polecenia cmdlet:

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    Zatrzymywanie usługi może potrwać kilka minut. Po zatrzymaniu usługi otrzymasz komunikat z informacją, że został zatrzymany.

3.  Aby usunąć usługę, zadzwoń następujące polecenie cmdlet:

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    Po wyświetleniu monitu wprowadź **Y** , aby usunąć usługę.

    Usuwanie usługi może potrwać kilka minut. Po usunięciu usługi otrzymasz komunikat z informacją, że usługa została usunięta.

  [Node.js aplikacji sieci Web przy użyciu Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [Przechowywanie i uzyskiwanie dostępu do danych platformy Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Aplikacja sieci Web node.js]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 
