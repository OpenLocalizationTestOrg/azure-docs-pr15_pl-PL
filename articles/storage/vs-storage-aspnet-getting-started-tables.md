<properties
    pageTitle="Wprowadzenie do programu Visual Studio i Magazyn tabel połączonych usług (ASP.NET) | Microsoft Azure"
    description="Jak rozpocząć pracę z magazynem tabel platformy Azure w projekcie ASP.NET w programie Visual Studio po nawiązywanie połączenia z kontem miejsca do magazynowania przy użyciu programu Visual Studio połączenia usług"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-table-storage-and-visual-studio-connected-services-aspnet"></a>Wprowadzenie do programu Visual Studio i Magazyn tabel połączonych usługi (ASP.NET)

[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Omówienie
W tym artykule opisano, jak rozpocząć korzystanie z magazyn tabel platformy Azure w programie Visual Studio, po utworzony lub odwołanie do konta Azure miejsca do magazynowania w ramach projektu programu ASP.NET przy użyciu okna dialogowego Visual Studio **Dodaj usługi połączone** . W tym artykule pokazano, jak wykonywać typowe zadania w Azure tabele, w tym tworzenie i usuwanie tabeli, a także współpracę z osobami tabeli. Przykłady są zapisywane w C\# kodu i używania [Biblioteki klienta programu Microsoft Azure miejsca do magazynowania dla środowiska .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx). Więcej ogólnych informacji o użyciu Azure tabeli miejsca do magazynowania, zobacz [Rozpoczynanie pracy z magazynem tabel platformy Azure za pomocą .NET](storage-dotnet-how-to-use-tables.md).

Magazyn tabel platformy Azure umożliwia przechowywanie dużych ilości danych strukturalnych. Usługa jest NoSQL magazynu danych, która akceptuje uwierzytelnionych połączeń z wewnątrz i na zewnątrz Azure chmury. Tabele Azure to idealne rozwiązanie w przypadku przechowywania danych strukturalnych, -relacyjnych.


## <a name="access-tables-in-code"></a>Tabele programu Access w kodzie

1. Upewnij się, że deklaracje przestrzeni nazw u góry pliku C# zawiera instrukcje te **przy użyciu** .

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. Uzyskaj obiekt **CloudStorageAccount** , reprezentujący informacji o koncie miejsca do magazynowania. Użyj następującego kodu uzyskiwania parametrów połączenia miejsca do magazynowania i informacje o koncie miejsca do magazynowania z konfigurację usługi Azure usługi.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Uwaga** — korzystać ze wszystkich powyższych kodu przed kodem w poniższych przykładach.

3. Uzyskaj obiekt **CloudTableClient** , odwołując się do obiektów tabeli na koncie miejsca do magazynowania.  

        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. Uzyskaj obiektu **CloudTable** odwołania do określonej tabeli i jednostki.

        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Tworzenie tabeli w kodzie

Aby utworzyć tabelę Azure, wystarczy dodać wywołanie **CreateIfNotExistsAsync()** do poprzedniego kodu.

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Dodawanie obiektu do tabeli

Aby dodać obiekt do tabeli możesz utworzyć klasa, która definiuje właściwości jednostka. Poniższy kod definiuje klasy obiektu o nazwie **CustomerEntity** używa imię klienta jako klucz wiersza i nazwiska jako klucz partycją.

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

Tabela obejmującego podmioty są gotowe przy użyciu **CloudTable** obiektu utworzonego wcześniej w "Tabelami programu Access w kodzie". Obiekt **TableOperation** reprezentuje operację do wykonania. W poniższym przykładzie pokazano, jak utworzyć obiektu **CloudTable** i obiektu **CustomerEntity** . Aby przygotować operację, **TableOperation** jest tworzona Wstawianie obiektu klienta do tabeli. Na koniec wykonywania operacji, dzwoniąc CloudTable.ExecuteAsync.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Wstawianie partię jednostek

Do tabeli w operacji zapisu pojedynczy, możesz wstawić wiele jednostek. W poniższym przykładzie tworzy dwa obiekty obiektów ("Marcin Jankowski" i "Jan Kowalski"), dodaje je do obiektu **TableBatchOperation** przy użyciu metody Wstaw, a następnie rozpoczyna operację, dzwoniąc **CloudTable.ExecuteBatchAsync**.

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Uzyskiwanie wszystkie jednostki partition
Aby wykonać kwerendę tabeli dla wszystkich osób w partycją, użyj obiektu **TableQuery** . W poniższym przykładzie określa filtr dla obiektów "Kit" w przypadku klucz partycją. W tym przykładzie jest drukowany pola każdego obiektu w wynikach kwerendy do konsoli.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity>
        resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
        }
        } while (token != null);

        return View();


## <a name="get-a-single-entity"></a>Uzyskiwanie całość
Można napisać kwerendę uzyskanie jednym, określonym jednostki. Poniższy kod używa obiektu **TableOperation** , aby określić klienta "Jan Kowalski". Ta metoda zwraca tylko jednego obiektu, a nie zbioru i zwracana wartość w **TableResult.Result** jest obiektem **CustomerEntity** . Określanie kluczy zarówno partycją i wierszy w zapytaniu jest najszybszy sposób pobrać jedną osobę z usługi tabeli.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);
    
    // Print the phone number of the result.
    if (retrievedResult.Result != null)
        Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Usuwanie obiektu
Po je znaleźć, można usunąć obiektu. Poniższy kod wyszukuje jednostka Klient "Jan Kowalski", a jeśli go znajdzie, usunięcie go.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]
