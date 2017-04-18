<properties
    pageTitle="Jak używać magazyn tabel z PHP | Microsoft Azure"
    description="Dowiedz się, jak skorzystać z usług tabeli PHP do tworzenia i usuwania tabeli i wstawianie, usuwanie i kwerenda tabeli."
    services="storage"
    documentationCenter="php"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-php"></a>Jak używać magazyn tabel z PHP

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Omówienie

Ten przewodnik zawiera się, jak wykonywać typowe scenariusze korzystania z usługi Azure tabeli. Przykłady są zapisane w PHP i używanie [SDK Azure dla PHP][download]. Scenariusze, w których zawierać **tworzenia i usuwania tabeli i wstawianie, usuwanie i wykonywanie zapytań za podmioty w tabeli**. Aby uzyskać więcej informacji o usłudze Azure tabeli zobacz sekcję [Następne kroki](#next-steps) .

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Tworzenie aplikacji PHP

Tylko wymagania dotyczące tworzenia aplikacji PHP, który uzyskuje dostęp do usługi Azure tabel jest odwołujących się klas w Azure SDK dla PHP z w kodzie. Za pomocą dowolnego narzędzia programistyczne do tworzenia aplikacji, w tym Notatnik.

W niniejszym przewodniku można użyć funkcji usługi tabeli, które mogą być wywoływane w aplikacji PHP lokalnie lub w kodzie uruchomionych w roli Azure sieci web, roli Pracownik lub witryny sieci Web.

## <a name="get-the-azure-client-libraries"></a>Pobieranie bibliotek Azure klienta

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Konfigurowanie aplikacji do uzyskania dostępu do usługi tabeli

Aby korzystać z usługi Azure tabeli interfejsy API, należy:

1. Dokumentacja dotycząca plik automatyczna ładowarka za pomocą [require_once] [ require_once] instrukcji i
2. Dokumentacja dotycząca wszystkich klas, których można użyć.

W poniższym przykładzie pokazano, jak dołączyć plik automatyczna ładowarka i odwołuje się klasy **ServicesBuilder** .

> [AZURE.NOTE] W tym przykładzie (i inne przykłady w tym artykule) przyjęto założenie, że zainstalowano bibliotek klienta PHP dla Azure za pośrednictwem kompozytor. Jeśli zainstalowano biblioteki ręcznie, muszą odwoływać się <code>WindowsAzure.php</code> automatyczna ładowarka pliku.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


W poniższych przykładach `require_once` instrukcja jest zawsze wyświetlany, ale odwołuje się to konieczne, na przykład do wykonywania zajęcia.

## <a name="set-up-an-azure-storage-connection"></a>Konfigurowanie połączenia Azure miejsca do magazynowania

Uruchamianie klienta usługi Azure tabeli, musisz najpierw włączyć prawidłowe parametry połączenia. Format ciągu połączenia usługi tabeli jest:

Aby uzyskać dostęp do usługi live:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Aby uzyskać dostęp do magazynu emulatora:

    UseDevelopmentStorage=true


Aby utworzyć dowolnego klienta usługi Azure, musisz użyć klasy **ServicesBuilder** . Można:

* przekazywane parametry połączenia bezpośrednio do niego lub
* **CloudConfigurationManager (CCM)** umożliwiają sprawdzanie wielu źródeł zewnętrznych ciągu połączenia:
    * Domyślnie pochodzi z obsługą jedno źródło zewnętrzne — zmienne środowiska
    * Możesz dodać nowe źródła, wydłużając klasy **ConnectionStringSource**

Przykłady przedstawionych poniżej parametry połączenia zostanie przekazany bezpośrednio.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## <a name="create-a-table"></a>Tworzenie tabeli

Obiekt **TableRestProxy** pozwala utworzyć tabelę przy użyciu metody **createTable** . Podczas tworzenia tabeli, można ustawić limit czasu usługi tabeli. (Aby uzyskać więcej informacji na temat usługi tabeli przekroczenia limitu czasu, zobacz [Ustawienia przekroczenia limitu czasu dla operacji usługi tabeli][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

Aby uzyskać informacji na temat ograniczeń dotyczących nazw tabel, zobacz [Opis modelu danych z tabeli][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Dodawanie obiektu do tabeli

Aby dodać obiekt do tabeli, Utwórz nowy obiekt **jednostki** i przekazać je do **TableRestProxy -> insertEntity**. Należy zauważyć, że podczas tworzenia obiektu, musisz określić `PartitionKey` i `RowKey`. Te są unikatowe identyfikatory obiektu i wartości, które można wyszukiwać znacznie szybciej niż inne właściwości obiektu. System używa `PartitionKey` Aby automatycznie przekazywać podmioty tabeli przez wiele węzłów magazynu. Jednostki o tej samej `PartitionKey` są przechowywane w tym samym węźle. (Wykonywanie operacji na wiele jednostek przechowywanych w tym samym węźle lepiej niż na obiektów przechowywanych w różnych węzłach.) `RowKey` Jest to unikatowy identyfikator obiektu w obrębie partycją.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

Aby uzyskać informacji na temat typów i właściwości tabeli, zobacz [Opis modelu danych z tabeli][table-data-model].

Klasy **TableRestProxy** oferuje dwie alternatywne metody służące do wstawiania obiektów: **insertOrMergeEntity** i **insertOrReplaceEntity**. Aby korzystać z tych metod, Utwórz nowy **obiekt** , a następnie przekazać go jako parametr do obu tych metod. Każda z metod wstawi jednostki, jeśli nie istnieje. Jeśli jednostka już istnieje, **insertOrMergeEntity** aktualizuje wartości właściwości, jeśli istnieje właściwości i dodaje nowe właściwości, jeśli nie istnieje, podczas gdy **insertOrReplaceEntity** całkowicie powoduje zastąpienie istniejącego obiektu. W poniższym przykładzie pokazano, jak za pomocą **insertOrMergeEntity**. Jeśli jednostka o `PartitionKey` "tasksSeattle" i `RowKey` "1" jeszcze nie istnieje, zostanie on wstawiony. Jednak jeśli wcześniej wstawione (jak pokazano w powyższym przykładzie), `DueDate` zostanie zaktualizowana oraz `Status` właściwości zostaną dodane. `Description` i `Location` właściwości również są aktualizowane, ale z wartościami który skutecznie pozostawić je bez zmian. Jeśli te ostatnie dwie właściwości nie zostały dodane, jak pokazano w przykładzie, ale istnieje w docelowej jednostce, ich bieżące wartości pozostaje bez zmian.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="retrieve-a-single-entity"></a>Pobieranie całość

Metoda **TableRestProxy -> getEntity** pozwala na pobieranie całość przez wyszukiwanie jej `PartitionKey` i `RowKey`. W przykładzie poniżej klucza partition `tasksSeattle` i klucza wiersza `1` są przekazywane do metody **getEntity** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## <a name="retrieve-all-entities-in-a-partition"></a>Pobieranie wszystkich obiektów w partycją

Kwerendy jednostki są wykonane przy użyciu filtrów (Aby uzyskać więcej informacji, zobacz [tabele kwerend i jednostki][filters]). Aby pobrać wszystkie jednostki w partition, użyj filtru "PartitionKey eq *nazwa_partycji*". W poniższym przykładzie pokazano sposób pobierania wszystkich obiektów w `tasksSeattle` partition przekazując filtru do metody **queryEntities** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Pobieranie podzbiór jednostki partycją

Takim samym wzorcem używane w poprzednim przykładzie może służyć do pobierania dowolny podzbiór jednostki partycją. Podzestaw jednostek pobierania są uzależnione od filtrowania (Aby uzyskać więcej informacji, zobacz [tabele kwerend i jednostki][filters]). W poniższym przykładzie pokazano, jak stosowanie filtru w celu pobrania wszystkich obiektów z określonym `Location` i `DueDate` mniej niż określona data.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## <a name="retrieve-a-subset-of-entity-properties"></a>Pobieranie podzbiór właściwości obiektu

Kwerendy można pobrać podzbiór właściwości obiektu. Ta metoda, o nazwie *rzut*zmniejsza przepustowość i zwiększyć wydajność kwerendy, szczególnie w przypadku dużych obiektów. Aby określić właściwości mają być pobierane, należy przekazać nazwę właściwości do metody **zapytania -> addSelectField** . Aby dodać więcej właściwości można wywołać wielokrotnie tej metody. Po wykonaniu **TableRestProxy -> queryEntities**, zwracany podmioty będzie miał tylko wybrane właściwości. (Jeśli chcesz przywrócić podzbiór jednostek tabeli, jak pokazano w kwerendach powyżej zastosować filtr.)

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## <a name="update-an-entity"></a>Aktualizowanie obiektu

Istniejącego obiektu można aktualizować przy użyciu metody **setProperty -> jednostki** i **jednostki -> addProperty** dotyczące obiektu, a następnie nawiązywania połączeń z **TableRestProxy -> updateEntity**. Poniższy przykład pobiera jednostki, modyfikuje jedną właściwość usuwa innej właściwości i dodaje nową właściwość. Zauważ, że możesz usunąć właściwość, ustawiając wartość **Null**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-an-entity"></a>Usuwanie obiektu

Aby usunąć jednostkę, należy przekazać nazwę tej tabeli i jednostki `PartitionKey` i `RowKey` do metody **TableRestProxy -> deleteEntity** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Należy zauważyć, że testy współbieżności, możesz ustawić Etag obiektu zostanie usunięta przy użyciu metody **DeleteEntityOptions -> setEtag** a przekazywania obiektu **DeleteEntityOptions** do **deleteEntity** jako czwarty parametr.

## <a name="batch-table-operations"></a>Operacje tabeli partii

Metoda **TableRestProxy -> partii** umożliwia wykonywanie wielu operacji w pojedyncze żądanie. Poniżej wzorca polega na dodanie operacje **BatchRequest** obiektu, a następnie przekazanie obiektu **BatchRequest** do metody **TableRestProxy -> partii** . Aby dodać operację do obiektu **BatchRequest** , można zadzwonić do dowolnej z następujących metod wielokrotnie:

* **addInsertEntity** (dodaje operacji insertEntity)
* **addUpdateEntity** (dodaje operacji updateEntity)
* **addMergeEntity** (dodaje operacji mergeEntity)
* **addInsertOrReplaceEntity** (dodaje operacji insertOrReplaceEntity)
* **addInsertOrMergeEntity** (dodaje operacji insertOrMergeEntity)
* **addDeleteEntity** (dodaje operacji deleteEntity)

W poniższym przykładzie pokazano, jak wykonywać operacje **insertEntity** i **deleteEntity** w pojedynczej żądaniu:

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;
    use MicrosoftAzure\Storage\Table\Models\Entity;
    use MicrosoftAzure\Storage\Table\Models\EdmType;
    use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Aby uzyskać więcej informacji o tworzeniu partii operacje tabeli, zobacz [Przeprowadzanie transakcji grupy jednostek][entity-group-transactions].

## <a name="delete-a-table"></a>Usuwanie tabeli

Na koniec: Aby usunąć tabelę, można przekazać do metody **TableRestProxy -> deleteTable** nazwy tabeli.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy usłudze Azure tabeli, skorzystaj z poniższych łączy, aby uzyskać informacje o bardziej złożone zadania miejsca do magazynowania.

- Odwiedź [blog zespołu magazyn Azure](http://blogs.msdn.com/b/windowsazurestorage/)

Aby uzyskać więcej informacji zobacz też [Centrum deweloperów PHP](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
