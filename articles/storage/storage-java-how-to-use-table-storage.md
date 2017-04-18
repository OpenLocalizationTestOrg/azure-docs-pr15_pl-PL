<properties
    pageTitle="Jak używać magazyn tabel z języka Java | Microsoft Azure"
    description="Przechowywanie danych strukturalnych w chmurze przy użyciu magazyn tabel platformy Azure, magazynu danych NoSQL."
    services="storage"
    documentationCenter="java"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="how-to-use-table-storage-from-java"></a>Jak używać magazyn tabel z języka Java

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Omówienie

Ten przewodnik pojawi się, jak wykonywać typowe scenariusze przy użyciu usługi Azure tabeli miejsca do magazynowania. Przykłady są zapisywane w języku Java i używanie [Azure SDK miejsca do magazynowania dla języka Java][]. Omówione są następujące scenariusze **Tworzenie**, **Wyświetlanie**i **Usuwanie** tabel, a także **Wstawianie**, **Wyszukiwanie**, **Modyfikowanie**i **Usuwanie** obiektów w tabeli. Aby uzyskać więcej informacji o tabelach zobacz sekcję [Następne kroki](#Next-Steps) .

Uwaga: SDK jest dostępna dla deweloperów, którzy używają magazyn Azure na urządzeniach z systemem Android. Aby uzyskać więcej informacji zobacz [Zestaw SDK Azure miejsca do magazynowania dla systemu Android][].

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Tworzenie aplikacji Java

W tym przewodniku należy użyć funkcji miejsca do magazynowania, które mogą być uruchamiane w aplikacji Java lokalnie lub w kodzie uruchomiony w sieci web rola lub pracownika platformy Azure.

W tym celu należy zainstalować Java Development Kit (JDK) i utworzyć konto Azure miejsca do magazynowania w subskrypcji usługi Azure. Po wykonaniu tego należy sprawdzić, czy systemu dla deweloperów spełnia minimalne wymagania i zależności, które są wymienione w repozytorium [Azure SDK miejsca do magazynowania dla języka Java][] w GitHub. Jeśli komputer spełnia tych wymagań, możesz postępuj zgodnie z instrukcjami pobraniu i zainstalowaniu Azure bibliotek miejsca do magazynowania dla języka Java na komputerze z tego repozytorium. Po zakończeniu tych zadań, można utworzyć aplikację języka Java, używanym w przykładach w tym artykule.

## <a name="configure-your-application-to-access-table-storage"></a>Konfigurowanie aplikacji, aby uzyskać dostęp do magazynowania tabeli

Dodawanie poniższych instrukcji importu do górnej części pliku Java, której chcesz używać magazynu Microsoft Azure interfejsy API dostęp do tabel:

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## <a name="setup-an-azure-storage-connection-string"></a>Konfigurowanie parametrów połączenia Azure miejsca do magazynowania

Klient Azure magazynu używa parametrów połączenia miejsca do magazynowania do przechowywania punkty końcowe i poświadczenia dostępu do usług zarządzania danymi. Podczas działa w aplikacji klienckiej, należy podać parametry połączenia miejsca do magazynowania w następującym formacie, przy użyciu nazwy konta miejsca do magazynowania i klucza podstawowego dostępu dla konta miejsca do magazynowania w [Azure Portal](https://portal.azure.com) wartości *Nazwa konta* i *AccountKey* na liście. W tym przykładzie pokazano, jak można deklarować statyczne pole do przechowywania parametry połączenia:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

W aplikacji uruchomionych w roli platformy Microsoft Azure ten ciąg mogą być przechowywane w pliku konfiguracji usługi *ServiceConfiguration.cscfg*i można uzyskać do nich dostęp przy użyciu metody **RoleEnvironment.getConfigurationSettings** . Oto przykład pobieranie parametrów połączenia z element **Ustawienie** o nazwie *StorageConnectionString* w pliku konfiguracji usługi:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Następujące próbki przyjęto założenie, że używasz jednej z tych dwóch metod uzyskiwania parametrów połączenia miejsca do magazynowania.

## <a name="how-to-create-a-table"></a>Jak: Tworzenie tabeli

Obiekt **CloudTableClient** pozwala uzyskać obiekty odwołań do tabel i obiektów. Poniższy kod tworzy obiekt **CloudTableClient** i używa jej do utworzenia nowego obiektu **CloudTable** , która reprezentuje tabeli o nazwie "osoby". (Uwaga: istnieją dodatkowe sposoby tworzenia obiektów **CloudStorageAccount** ; uzyskać więcej informacji, zobacz **CloudStorageAccount** [Azure miejsca do magazynowania klienta SDK odwołanie].)

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = tableClient.getTableReference(tableName);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-tables"></a>Jak: listy tabel

Aby uzyskać listę tabel, zadzwoń do metody **CloudTableClient.listTables()** , aby pobrać iterable listę nazw tabel.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-an-entity-to-a-table"></a>Jak: Dodawanie obiektu do tabeli

Mapa jednostki do obiektów Java przy użyciu niestandardowej klasy implementacji **TableEntity**. Dla wygody klasy **TableServiceEntity** wykonuje **TableEntity** i używa odbicia do mapowania właściwości metody pobierające i ustawiające o nazwie właściwości. Aby dodać obiekt do tabeli, należy najpierw utworzyć klasa, która definiuje właściwości jednostka. Poniższy kod definiuje klasy jednostki, która używa imię klienta jako klucz wiersza i nazwiska jako klucz partycją. Razem partycją jednostki i klucza wiersza jednoznacznie identyfikować jednostki w tabeli. Jednostki z tym samym kluczem partition można wyszukiwać szybciej niż za pomocą klawiszy ENTER.

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;

        public String getEmail() {
            return this.email;
        }

        public void setEmail(String email) {
            this.email = email;
        }

        public String getPhoneNumber() {
            return this.phoneNumber;
        }

        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

Tabela obejmującego podmioty wymagają obiektu **TableOperation** . Ten obiekt określa operacji wykonywanych na jednostki, które mogą być wykonywane za pomocą obiektu **CloudTable** . Poniższy kod tworzy nowe wystąpienie klasy **CustomerEntity** zawierającego dane klienta mają być przechowywane. Kod następnie połączenia **TableOperation.insertOrReplace** , aby utworzyć obiekt **TableOperation** , aby wstawić obiekt do tabeli i przypisuje nowy **CustomerEntity** go. Na koniec kod wywołuje metoda **wykonania** na obiekcie **CloudTable** , określając tabeli "osoby" oraz nowe **TableOperation**, następnie wysłać żądanie z usługą Magazyn, aby wstawić nowy obiekt klienta do tabeli "osoby" lub zamienianie jednostki, jeśli już istnieje.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-a-batch-of-entities"></a>Jak: wstawianie partię jednostek

W jednej operacji zapisu można wstawiać partię jednostek z usługą tabeli. Poniższy kod tworzy obiekt **TableBatchOperation** , a następnie dodaje trzy Wstawianie operacje, aby go. Kolejne operacje wstawiania jest dodawany przez utworzenie nowego obiektu jednostki, ustawienie wartości i następnie wywoływanie metody **Wstawianie** obiektu **TableBatchOperation** chcesz skojarzyć jednostki nowych operacji wstawiania. Następnie kod wywołuje **Wykonywanie** na obiekcie **CloudTable** , określając tabeli "osoby" i obiektu **TableBatchOperation** , otrzyma partii operacje tabeli z usługą magazynu w pojedynczej żądaniu.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Operacje wsadowe kilka uwag na:

- Można wykonywać insert do 100, usuwanie, korespondencji seryjnej, Zamień, wstawianie lub scalić i wstawianie lub zamienianie operacje w dowolną kombinację w ramach jednej partii.
- Operacja partii może mieć operacji pobierania, jeśli jest tylko operacji w partii.
- Wszystkie obiekty w operacji jednej partii musi być tego samego klucza partycją.
- Operacja partii jest ograniczone do ładunek danych 4MB.

## <a name="how-to-retrieve-all-entities-in-a-partition"></a>Jak: pobrać wszystkie jednostki w partycją

Aby wykonać kwerendę tabeli dla obiektów w partycją, możesz użyć **TableQuery**. Zadzwoń do **TableQuery.from** umożliwia tworzenie zapytań dla określonego tabeli, która zwraca typ określonych wyników. Poniższy kod określa filtr dla obiektów "Kit" w przypadku klucz partycją. **TableQuery.generateFilterCondition** jest to metoda Pomocnik tworzenia filtrów dla kwerend. Nawiązywanie połączenia **miejsce, w którym** Odwołanie zwrócone przez metodę **TableQuery.from** , aby zastosować filtr do danej kwerendy. Po wykonaniu kwerendy z połączenia do **wykonania** na obiekcie **CloudTable** zwraca **iteracyjne** określonego typu wyniku **CustomerEntity** . Możesz użyć **iteracyjne** zwrócone w dla każdej pętli do korzystania z wyników. Kod drukowany pola każdego obiektu w wynikach kwerendy do konsoli.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a>Jak: pobrać zakresu jednostki partycją

Jeśli nie chcesz, aby wszystkie jednostki w partycją kwerendy, można określić zakres przy użyciu operatorów porównania w filtrze. Poniższy kod łączy dwa filtry do pobierania wszystkich obiektów w partition "Kit" miejsce, w którym klucz wiersza (imię) zaczyna się od litery do "E" alfabetu. Następnie zostanie wydrukowany wyników kwerendy. Jeśli korzystasz z jednostki dodane do tabeli w partii wstawić sekcję w tym przewodniku, tylko dwa jednostki są zwracane tym razem (Jan i Rumian); Marcin Jankowski nie jest włączony.

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY,
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY,
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter,
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-retrieve-a-single-entity"></a>Jak: pobrać całość

Można napisać kwerendę, aby pobrać podmiot jednym, określonym. Poniższy kod nawiąże połączenie z **TableOperation.retrieve** kluczem partycją i wiersz klawiszy w celu określenia klienta "Marcin Jankowski" zamiast tworzenia **TableQuery** i wykonaj te same kroki przy użyciu filtrów. Podczas wykonywania operacji pobierania zwraca tylko jednego obiektu, a nie kolekcji. Metoda **getResultAsType** rzutuje wynik typ obiektu docelowego przydziału obiektu **CustomerEntity** . Jeśli ten typ nie jest zgodny z typem określone dla kwerendy, zostanie zgłoszony wyjątek. Jest zwracana wartość null, jeśli brak jednostki ma dokładnego partycją i klucz wiersza zgodne. Określanie kluczy zarówno partycją i wierszy w zapytaniu jest najszybszy sposób pobrać jedną osobę z usługi tabeli.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-modify-an-entity"></a>Jak: zmodyfikować jednostkę

Aby zmodyfikować jednostki, pobranie ich z usługi tabel, zmiany obiektu jednostki i zapisać zmiany usłudze tabeli z operacji Zamień lub korespondencji seryjnej. Poniższy kod zmienia numer telefonu istniejącego klienta. Zamiast połączenia audio **TableOperation.insert** , jak to zostało możemy Aby wstawić ten kod wywołuje **TableOperation.replace**. Metoda **CloudTable.execute** połączeń usłudze tabeli i jednostki są zastępowane, chyba że innej aplikacji zmienione go w czasie od pobrany tej aplikacji. W takiej sytuacji wyjątek, a podmiot musi być pobierane, zmodyfikowany i zapisany ponownie. Ten wzorzec ponów próbę optymistyczny współbieżności jest typowe w systemie rozłożone magazynu.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff =
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-query-a-subset-of-entity-properties"></a>Jak: kwerendy podzbiór właściwości obiektu

Kwerendy do tabeli można pobrać kilka właściwości z obiektu. Ta metoda, o nazwie rzut zmniejsza przepustowość i zwiększyć wydajność kwerendy, szczególnie w przypadku dużych obiektów. Kwerenda poniższy kod użyto **Wybierz** metodę, aby zwrócić tylko adresy e-mail osób w tabeli. Wyniki są przewidywane do kolekcji **ciągu** za pomocą **EntityResolver**, która konwersji typów na zwróconych z serwera. Dowiedz się więcej o rzut w [Azure tabel: wprowadzenie do Upsert i rzut kwerendy][]. Należy zauważyć, że rzut nie są obsługiwane na emulatorze magazynu lokalnego, więc kod działa tylko wtedy, gdy za pomocą konta w usłudze tabeli.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery =
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString :
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-insert-or-replace-an-entity"></a>Jak: wstawianie lub zamienianie jednostki

Często chcesz dodać obiekt do tabeli bez znajomości, jeśli już istnieje w tabeli. Wstawianie lub zamienianie operacji umożliwia nawiązywanie pojedyncze żądanie, której zostanie wstawiona jednostki, jeśli nie istnieje lub nie zamienić istniejący, jeśli tak. Opierając się na poprzednich przykładach, poniższy kod zostanie wstawiony lub zastąpi jednostki dla "Walter Harp". Po utworzeniu nowego obiektu, kod wywołuje metodę **TableOperation.insertOrReplace** . Kod następnie wywołuje **Wykonywanie** obiektu **CloudTable** z tabeli i Wstaw lub zamienianie operacja tabeli jako parametry. Aby zaktualizować tylko część obiektu, można w zamian użyć metody **TableOperation.insertOrMerge** . Należy zauważyć, że tego Wstaw lub zamieniania nie są obsługiwane na emulatorze magazynu lokalnego, kod działa tylko wtedy, gdy za pomocą konta w usłudze tabeli. Możesz dowiedzieć się więcej o Wstawianie lub zamienianie i Wstaw lub Scal w tym [Azure tabel: wprowadzenie do Upsert i rzut kwerendy][].

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-an-entity"></a>Jak: usuwanie obiektu

Możesz łatwo usunąć obiekt, po pobraniu go. Po pobraniu jednostki połączeń **TableOperation.delete** z jednostką do usunięcia. Następnie zadzwoń do **wykonania** na obiekcie **CloudTable** . Poniższy kod pobiera i usuwa jednostka Klient.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-table"></a>Jak: usuwanie tabeli

Na koniec poniższy kod powoduje usunięcie tabeli z konta miejsca do magazynowania. Tabela, który został usunięty będą niedostępne do odtworzenia okres, po usunięciu, zwykle mniej niż 40 sekund.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy miejsca w tabeli, wykonaj te łącza, aby dowiedzieć się, jak wykonywać bardziej złożone zadania miejsca do magazynowania.

- [Azure miejsca do magazynowania SDK dla języka Java][]
- [Odwołanie SDK klienta Azure miejsca do magazynowania][]
- [Interfejsu API usługi REST Azure miejsca do magazynowania][]
- [Blog zespołu Azure miejsca do magazynowania][]

Aby uzyskać więcej informacji zobacz też [Centrum deweloperów języka Java](/develop/java/).


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure miejsca do magazynowania SDK dla języka Java]: https://github.com/azure/azure-storage-java
[Azure miejsca do magazynowania SDK dla systemu Android]: https://github.com/azure/azure-storage-android
[Odwołanie SDK klienta Azure miejsca do magazynowania]: http://dl.windowsazure.com/storage/javadoc/
[Interfejsu API usługi REST Azure miejsca do magazynowania]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Blog zespołu Azure miejsca do magazynowania]: http://blogs.msdn.com/b/windowsazurestorage/
[Tabele Azure: Wprowadzenie do Upsert i rzut kwerendy]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
