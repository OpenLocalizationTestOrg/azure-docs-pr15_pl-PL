<properties
    pageTitle="Rozpoczynanie pracy z magazynem tabel platformy Azure za pomocą .NET | Microsoft Azure"
    description="Przechowywanie danych strukturalnych w chmurze przy użyciu magazyn tabel platformy Azure, magazynu danych NoSQL."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-table-storage-using-net"></a>Rozpoczynanie pracy z magazynem tabel platformy Azure za pomocą .NET

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Omówienie

Magazyn tabel platformy Azure to usługa przechowującego dane strukturalne NoSQL w chmurze. Magazyn tabel jest magazynem atrybutu klucza schemaless projektu. Ponieważ Magazyn tabel jest schemaless, jest łatwe do dostosowania danych jako na potrzeby usługi evolve aplikacji. Dostęp do danych jest szybkie i efektywne pod względem kosztów dla wszystkich typów aplikacji. Magazyn tabel jest zazwyczaj znacząco niższe w polu Koszt tradycyjnych SQL podobne ilości danych.

Magazyn tabel służy do przechowywania elastyczne zestawy danych, takich jak dane użytkownika dla aplikacji sieci web, książki adresowe, informacje o urządzeniach i inny rodzaj metadanych, która wymaga usługi. Mogą zawierać dowolną liczbę obiektów w tabeli, a konto miejsca do magazynowania może zawierać dowolną liczbę tabel, aż do limitem konta miejsca do magazynowania.

### <a name="about-this-tutorial"></a>Informacje o tym samouczku

Ten samouczek pokazano, jak pisać kod .NET kilka typowych scenariuszy przy użyciu magazyn tabel platformy Azure, w tym tworzenie i usuwanie tabeli i wstawianie, aktualizowanie, usuwanie i badania danych tabeli.

**Szacowany czas:** 45 minut

**Prerequisities:**

- [Program Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Biblioteka klienta Azure miejsca do magazynowania dla środowiska .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure Menedżer konfiguracji programu .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Konto Azure miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account)

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Więcej przykładów

Aby uzyskać dodatkowe przykłady przy użyciu magazyn tabel zobacz [Wprowadzenie do magazyn tabel platformy Azure w .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/). Możesz pobrać aplikację próbki i uruchom go lub przeglądanie kodu na GitHub.


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Dodawanie deklaracje przestrzeni nazw

Dodaj następujący `using` instrukcje na początek `program.cs` pliku:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types

### <a name="parse-the-connection-string"></a>Analizowanie parametry połączenia

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a>Tworzenie tabeli klienta usługi

Klasa **CloudTableClient** umożliwia pobieranie tabel i jednostki przechowywane w magazynie tabel. W tym miejscu jest jednym ze sposobów tworzenia klienta usługi:

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

Teraz możesz przystąpić do pisania kodu, który odczytuje i zapisuje dane z magazynem tabel.

## <a name="create-a-table"></a>Tworzenie tabeli

W tym przykładzie przedstawiono sposób tworzenia tabeli, jeśli jeszcze nie istnieje:

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Retrieve a reference to the table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table if it doesn't exist.
    table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Dodawanie obiektu do tabeli

Mapowanie jednostki C\# obiektów przy użyciu niestandardowej klasy pochodzące z **TableEntity**. Aby dodać obiekt do tabeli, Utwórz klasa, która definiuje właściwości jednostka. Poniższy kod definiuje klasę jednostki, która używa imię klienta jako klucz wiersza i nazwiska jako klucz partycją. Razem partycją jednostki i klucza wiersza jednoznacznie identyfikować jednostki w tabeli. Jednostki z tym samym kluczem partition można wyszukiwać szybciej niż za pomocą klawiszy ENTER, ale za pomocą klawiszy różnorodną partition umożliwia większa skalowalność równoległych operacji.  Wszystkich właściwości, które mają być przechowywane w usłudze tabeli, właściwość musi być publiczna obsługiwanego typu, który udostępnia zarówno `get` i `set`.
Ponadto do typu *musi* uwidaczniają konstruktor bez parametrów.

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

Operacje tabeli, obejmujące jednostki są wykonywane za pośrednictwem obiektu **CloudTable** , który został utworzony wcześniej w sekcji "Tworzenie tabeli". Operacja do wykonania jest reprezentowany przez obiekt **TableOperation** .  W poniższym przykładzie pokazano tworzenie obiektu **CloudTable** , a następnie obiektu **CustomerEntity** .  Aby przygotować operację, zostanie utworzony obiekt **TableOperation** Wstawianie obiektu klienta do tabeli.  Na koniec wykonywania operacji, dzwoniąc **CloudTable.Execute**.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    table.Execute(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Wstawianie partię jednostek

Partię jednostek można wstawić do tabeli w jednej operacji zapisu. Niektóre inne uwagi na operacje partii:

-  Można wykonywać aktualizacji, usuwa i zostanie wstawiony w tej samej operacji jednej partii.
-  Operacja jednej partii może zawierać maksymalnie 100 jednostek.
-  Wszystkie obiekty w operacji jednej partii musi być tego samego klucza partycją.
-  Gdy użytkownik może wykonać kwerendy jako operacja partii musi być operację wyłącznie w partii.

<!-- -->
W poniższym przykładzie tworzy dwa obiekty obiektów i dodaje każde **TableBatchOperation** przy użyciu metody **Wstawianie** . Następnie **CloudTable.Execute** nosi nazwę do wykonywania operacji.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

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
    table.ExecuteBatch(batchOperation);

## <a name="retrieve-all-entities-in-a-partition"></a>Pobieranie wszystkich obiektów w partycją

Aby wykonać kwerendę tabeli dla wszystkich obiektów w partycją, użyj obiektu **TableQuery** .
W poniższym przykładzie określa filtr dla obiektów "Kit" w przypadku klucz partycją. W tym przykładzie jest drukowany pola każdego obiektu w wynikach kwerendy do konsoli.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Pobieranie zakresu jednostki partycją

Jeśli nie chcesz, aby wszystkie jednostki w partycją kwerendy, możesz określić zakresie łącząc filtru kluczowego partition przy użyciu filtru klucza wiersza. W poniższym przykładzie użyto dwóch filtrów uzyskiwania wszystkich obiektów w partition "Kit", gdzie klucz wiersza (imię) wcześniejszych niż "E" w alfabecie zaczyna się od litery, a następnie wydrukowanie wyników kwerendy.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## <a name="retrieve-a-single-entity"></a>Pobieranie całość

Można napisać kwerendę, aby pobrać podmiot jednym, określonym. Poniższy kod użyto **TableOperation** do określenia klienta "Jan Kowalski".
Ta metoda zwraca tylko jeden obiekt zamiast zbioru i zwracana wartość w **TableResult.Result** jest obiektem **CustomerEntity** .
Określanie kluczy zarówno partycją i wierszy w zapytaniu jest najszybszy sposób pobrać jedną osobę z usługi tabeli.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="replace-an-entity"></a>Zamień obiekt

Aby zaktualizować jednostki, pobranie ich z usługi tabel, zmodyfikować obiektu jednostki, a następnie zapisz zmiany do usługi tabeli. Poniższy kod zmienia numer telefonu istniejącego klienta. Zamiast połączenia audio, **Wstawianie**ten kod zawiera **Zamień**. Ta opcja powoduje obiekt, który ma w pełni zastąpić na serwerze, chyba że zmienił podmiotu na serwerze, ponieważ zostało pobrane, w tym przypadku operacji nie powiedzie się.  Ten błąd jest uniemożliwiają utworzenie aplikacji niezamierzonemu zastąpieniu zmian między pobierania i aktualizacji przez inne składniki aplikacji.  Prawidłowe obchodzenie tego błędu jest ponownie pobrać jednostki, wprowadź zmiany (jeśli jest nadal ważna), a następnie wykonaj innej operacji **Zamień** .  Następnej sekcji procedurach pokazano, jak zastąpić to zachowanie.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";

       // Create the Replace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);

       // Execute the operation.
       table.Execute(updateOperation);

       Console.WriteLine("Entity updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="insert-or-replace-an-entity"></a>Wstawianie lub zamieniania jednostki

**Zamienianie** operacji zakończy się niepowodzeniem, jeśli zmieniono jednostki, ponieważ zostały pobrane z serwera.  Ponadto należy pobrać podmiotu na serwerze, najpierw w kolejności operacji **Zamień** się pomyślnie.
Czasem jednak nie wiesz, jeśli jednostka nie istnieje na serwerze i bieżące wartości znajdujące się w niej są one zbędne. Aktualizacja należy zastąpić je wszystkie.  W tym celu należy użyć operacji **InsertOrReplace** .  Operacja wstawia jednostki, jeśli nie istnieje lub zamieni go, jeśli tak, niezależnie od tego, gdy jest realizowana ostatniej aktualizacji.  W poniższym przykładzie jednostce Klient Jan Nowak nadal jest pobierana, ale jest następnie zapisywany na serwerze za pośrednictwem **InsertOrReplace**.  Zostaną zastąpione wszelkie aktualizacje wprowadzone do jednostki między operacji pobierania i aktualizacji.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";

       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);

       // Execute the operation.
       table.Execute(insertOrReplaceOperation);

       Console.WriteLine("Entity was updated.");
    }

    else
       Console.WriteLine("Entity could not be retrieved.");

## <a name="query-a-subset-of-entity-properties"></a>Kwerendy podzbiór właściwości obiektu

Kwerendy tabeli można pobrać kilka właściwości z jednostka zamiast wszystkich właściwości obiektu. Ta metoda, o nazwie rzut zmniejsza przepustowość i zwiększyć wydajność kwerendy, szczególnie w przypadku dużych obiektów. Kwerenda poniższy kod zwraca tylko adresy e-mail osób, w tabeli. Jest to zrobić przy użyciu kwerendy **DynamicTableEntity** , a także **EntityResolver**. Więcej informacji temat rzut na [wprowadzenie Upsert i blogu rzut kwerendy ogłoszeń][]. Należy zauważyć, że rzut nie są obsługiwane na emulatorze magazynu lokalnego, więc kod działa tylko wtedy, gdy korzystasz z konta w usłudze tabeli.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## <a name="delete-an-entity"></a>Usuwanie obiektu

Możesz łatwo usunąć obiekt, po pobraniu, za pomocą samego wzorca wyświetlane w celu zaktualizowania obiektu.  Poniższy kod pobiera i usuwa jednostka Klient.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       table.Execute(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Could not retrieve the entity.");

## <a name="delete-a-table"></a>Usuwanie tabeli

Na koniec w poniższym przykładzie powoduje usunięcie tabeli z konta przestrzeni dyskowej. Tabela, która została usunięta będzie mógł on odtworzony okres, po usunięciu.

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");

    // Delete the table it if exists.
    table.DeleteIfExists();

## <a name="retrieve-entities-in-pages-asynchronously"></a>Pobieranie obiektów na stronach asynchroniczne

Jeśli podczas czytania dużej liczby jednostek, a chcesz proces i wyświetlania obiektów zgodnie z ich pobraniu zamiast oczekiwanie na ich wszystkich zwraca, można pobrać jednostki przy użyciu segmentowany kwerend. W tym przykładzie przedstawiono sposób zwracało wyniki na stronach przy użyciu poczekać na asynchroniczne deseniu, tak aby wykonanie nie jest zablokowane, gdy czekasz dla dużego zestawu wyników do zwrócenia. Aby uzyskać więcej informacji na temat korzystania ze wzorcem Await asynchroniczne w .NET zobacz [asynchroniczne programowania przy użyciu asynchroniczne i Await (C# i Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;

    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;

        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy magazyn tabel, skorzystaj z poniższych łączy, aby uzyskać informacje o bardziej złożonych zadań magazynu:

- Zobacz więcej przykładów miejsca do magazynowania tabeli [rozpoczynania](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) pracy z magazynem tabel platformy Azure w .NET
- Widok usługi tabeli dokumentacji dotyczących pełne szczegóły dotyczące dostępnych interfejsów API:
    - [Biblioteka klienta miejsca do magazynowania dla odwołania .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [Odwołanie interfejsu API usługi REST](http://msdn.microsoft.com/library/azure/dd179355)
- Dowiedz się, jak w celu uproszczenia kodu, którą piszesz do pracy z magazyn Azure za pomocą [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- Wyświetl więcej prowadnic funkcji, aby uzyskać informacje o dodatkowych opcji do przechowywania danych w Azure.
    - [Rozpoczynanie pracy z magazynem obiektów Blob platformy Azure za pomocą .NET](storage-dotnet-how-to-use-blobs.md) do przechowywania danych niestrukturalne.
    - [Nawiązywanie połączenia z bazą danych SQL przy użyciu .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) do przechowywania danych relacyjnych.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

  [Blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png

  [Wpis w blogu Upsert wprowadzające i rzut kwerendy]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
  [.NET Client Library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configure Azure Storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
  [How to: Programmatically access Table storage]: #tablestorage
