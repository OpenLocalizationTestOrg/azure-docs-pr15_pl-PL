<properties
    pageTitle="Jak używać magazyn tabel (C++) | Microsoft Azure"
    description="Przechowywanie danych strukturalnych w chmurze przy użyciu magazyn tabel platformy Azure, magazynu danych NoSQL."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-table-storage-from-c"></a>Jak używać magazyn tabel z C++

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Omówienie  
Ten przewodnik pojawi się, jak wykonywać typowe scenariusze za pomocą usługi Azure tabeli miejsca do magazynowania. Przykłady są zapisywane w języku C++ i [Biblioteka klienta Azure miejsca do magazynowania dla C++](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). Scenariusze, w których opisano **Tworzenie i usuwanie tabeli** lub **pracy z obiektami tabeli**.

>[AZURE.NOTE] Ten przewodnik jest przeznaczony dla biblioteki klienta miejsca do magazynowania Azure C++ w wersji 1.0.0 i nowszych. Zalecane wersja jest biblioteka klienta miejsca do magazynowania 2.2.0, która jest dostępna za pośrednictwem [NuGet](http://www.nuget.org/packages/wastorage) lub [GitHub](https://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="create-a-c-application"></a>Tworzenie aplikacji C++  
W tym przewodniku należy użyć funkcji miejsca do magazynowania, które mogą być uruchamiane w aplikacji C++. W tym celu należy zainstalować Biblioteka klienta Azure miejsca do magazynowania dla C++ i utworzyć konto Azure miejsca do magazynowania w ramach subskrypcji Azure.  

Aby zainstalować Biblioteka klienta Azure miejsca do magazynowania dla C++, można skorzystać następujących metod:

-   **Linux:** Postępuj zgodnie z instrukcjami na stronie [Biblioteka klienta Azure miejsca do magazynowania dla C++ — Plik README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Systemu Windows:** W programie Visual Studio, kliknij przycisk **Narzędzia > Menedżer pakietów NuGet > Menedżer pakietów konsoli**. Wpisz następujące polecenie w [konsoli Menedżera pakietów NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , a następnie naciśnij klawisz Enter.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-table-storage"></a>Konfigurowanie aplikacji, aby uzyskać dostęp do magazynowania tabeli  
Dodaj następujące zawiera instrukcje na początek pliku C++, której chcesz użyć na przechowywanie Azure interfejsy API dostęp do tabel:  

    #include "was/storage_account.h"
    #include "was/table.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Konfigurowanie parametrów połączenia Azure miejsca do magazynowania  
Klient Azure magazynu używa parametrów połączenia miejsca do magazynowania do przechowywania punkty końcowe i poświadczenia dostępu do usług zarządzania danymi. Podczas uruchamiania aplikacji klienckiej, musisz podać parametry połączenia miejsca do magazynowania w następującym formacie. Użyj nazwy konta miejsca do magazynowania i klawisz dostępu miejsca do magazynowania dla konta miejsca do magazynowania w [Azure Portal](https://portal.azure.com) wartości *Nazwa konta* i *AccountKey* na liście. Aby uzyskać informacje na kontach miejsca do magazynowania i klawisze dostępu Zobacz [o Azure miejsca do magazynowania konta](storage-create-storage-account.md). W tym przykładzie pokazano, jak można deklarować statyczne pole do przechowywania parametry połączenia:  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Aby przetestować aplikację na komputerze lokalnym systemem Windows, możesz użyć Azure [emulatora miejsca do magazynowania](storage-use-emulator.md) jest zainstalowany wraz z [Azure SDK](https://azure.microsoft.com/downloads/). Emulator miejsca do magazynowania jest narzędziem symuluje dostępne na komputerze lokalnym rozwoju usługi obiektów Blob platformy Azure, kolejki i tabeli. W poniższym przykładzie pokazano, jak można deklarować statyczne pole do przechowywania parametry połączenia do swojego emulatora magazynu lokalnego:  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Aby rozpocząć emulatora Azure miejsca do magazynowania, kliknij przycisk **Start** , lub naciśnij klawisz systemu Windows. Zacznij wpisywać **Azure emulatora miejsca do magazynowania**, a następnie wybierz **Microsoft Azure miejsca do magazynowania emulatora** z listy aplikacji.  

Następujące próbki przyjęto założenie, że używasz jednej z tych dwóch metod uzyskiwania parametrów połączenia miejsca do magazynowania.  

## <a name="retrieve-your-connection-string"></a>Pobieranie ciągu połączenia  
Za pomocą klasy **cloud_storage_account** reprezentować informacji o koncie miejsca do magazynowania. Aby pobrać informacji o koncie miejsca do magazynowania z parametrów połączenia miejsca do magazynowania, można użyj metody analizy.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

Następnie odwołać się do klasy **cloud_table_client** , jak pozwala uzyskać obiekty odwołań do tabel i obiektów przechowywanych w usłudze miejsca do magazynowania tabeli. Poniższy kod tworzy obiekt **cloud_table_client** przy użyciu obiektu konta miejsca do magazynowania, możemy pobierane powyżej:  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## <a name="create-a-table"></a>Tworzenie tabeli
Obiekt **cloud_table_client** pozwala uzyskać obiekty odwołań do tabel i obiektów. Poniższy kod tworzy obiekt **cloud_table_client** i umożliwia tworzenie nowej tabeli.

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## <a name="add-an-entity-to-a-table"></a>Dodawanie obiektu do tabeli
Aby dodać obiekt do tabeli, Utwórz nowy obiekt **table_entity** i przekazać je do **table_operation::insert_entity**. Poniższy kod używa imię klienta jako klucz wiersza i nazwiska jako klucz partycją. Razem partycją jednostki i klucza wiersza jednoznacznie identyfikować jednostki w tabeli. Jednostki z tym samym kluczem partition można wyszukiwać szybciej niż za pomocą klawiszy ENTER, ale za pomocą klawiszy różnorodną partition umożliwia większa skalowalność równoległych operacji. Aby uzyskać więcej informacji zobacz [Microsoft Azure miejsca do magazynowania wydajność i skalowalność — Lista kontrolna](storage-performance-checklist.md).

Poniższy kod tworzy nowe wystąpienie **table_entity** zawierającego dane klienta mają być przechowywane. Kod następnie połączenia **table_operation::insert_entity** , aby utworzyć obiekt **table_operation** , aby wstawić obiekt do tabeli i przypisuje nowy obiekt tabeli go. Na koniec kod wywołuje metodę execute na obiekcie **cloud_table** . Oraz nowe **table_operation** wysyłane żądanie do usługi tabeli, aby wstawić nowy obiekt klienta do tabeli "osoby".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## <a name="insert-a-batch-of-entities"></a>Wstawianie partię jednostek
W jednej operacji zapisu można wstawiać partię jednostek z usługą tabeli. Poniższy kod tworzy obiekt **table_batch_operation** , a następnie dodaje trzy Wstawianie operacje, aby go. Kolejne operacje wstawiania jest dodawany przez utworzenie nowego obiektu jednostki, ustawienie wartości i następnie wywoływanie metody Wstawianie obiektu **table_batch_operation** chcesz skojarzyć jednostki nowych operacji wstawiania. Następnie Aby wykonać operację nosi nazwę **cloud_table.execute** .  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

Operacje wsadowe kilka uwag na:  

-   Do 100 Wstawianie Usuń seryjna Zamień, wstawianie lub Scal i wstawianie lub zamienianie można wykonywać w dowolnej kombinacji w ramach jednej partii.  
-   Operacja partii może mieć operacji pobierania, jeśli jest tylko operacji w partii.  
-   Wszystkie obiekty w operacji jednej partii musi być tego samego klucza partycją.  
-   Operacja partii jest ograniczone do ładunek danych 4 MB.  

## <a name="retrieve-all-entities-in-a-partition"></a>Pobieranie wszystkich obiektów w partycją
Aby wykonać kwerendę tabeli dla wszystkich obiektów w partycją, użyj obiektu **table_query** . W poniższym przykładzie określa filtr dla obiektów "Kit" w przypadku klucz partycją. W tym przykładzie jest drukowany pola każdego obiektu w wynikach kwerendy do konsoli.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

Kwerendy, w tym przykładzie łączy wszystkie obiekty, które spełniają kryteria filtru. Jeśli masz dużych tabel i potrzebne do pobrania podmioty tabeli często zalecane dane są przechowywane w Azure magazyn obiektów blob zamiast tego.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Pobieranie zakresu jednostki partycją
Jeśli nie chcesz, aby wszystkie jednostki w partycją kwerendy, możesz określić zakresie łącząc filtru kluczowego partition przy użyciu filtru klucza wiersza. W poniższym przykładzie użyto dwóch filtrów uzyskiwania wszystkich obiektów w partition "Kit", gdzie klucz wiersza (imię) wcześniejszych niż "E" w alfabecie zaczyna się od litery, a następnie wydrukowanie wyników kwerendy.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## <a name="retrieve-a-single-entity"></a>Pobieranie całość
Można napisać kwerendę, aby pobrać podmiot jednym, określonym. Poniższy kod używa **table_operation::retrieve_entity** , aby określić klienta "Marcin Jankowski". Ta metoda zwraca tylko jeden obiekt zamiast zbioru i zwracana wartość jest **table_result**. Określanie kluczy zarówno partycją i wierszy w zapytaniu jest najszybszy sposób pobrać jedną osobę z usługi tabeli.  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## <a name="replace-an-entity"></a>Zamień obiekt
Aby zamienić jednostki, pobranie ich z usługi tabel, zmodyfikować obiektu jednostki, a następnie zapisz zmiany do usługi tabeli. Poniższy kod zmienia istniejący klient telefonu i adresu e-mail. Zamiast połączenia audio **table_operation::insert_entity**ten kod zawiera **table_operation::replace_entity**. Ta opcja powoduje obiekt, który ma w pełni zastąpić na serwerze, chyba że zmienił podmiotu na serwerze, ponieważ zostało pobrane, w tym przypadku operacji nie powiedzie się. Ten błąd jest uniemożliwiają utworzenie aplikacji niezamierzonemu zastąpieniu zmian między pobierania i aktualizacji przez inne składniki aplikacji. Prawidłowe obchodzenie tego błędu jest ponownie pobrać jednostki, wprowadź zmiany (jeśli jest nadal ważna), a następnie wykonaj innej operacji **table_operation::replace_entity** . Następnej sekcji procedurach pokazano, jak zastąpić to zachowanie.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## <a name="insert-or-replace-an-entity"></a>Wstawianie lub zamieniania jednostki
operacje **table_operation::replace_entity** zakończy się niepowodzeniem, jeśli zmieniono jednostki, ponieważ zostały pobrane z serwera. Ponadto należy pobrać podmiotu na serwerze w kolejności **table_operation::replace_entity** się pomyślnie. Czasem jednak nie wiesz, jeśli jednostka nie istnieje na serwerze i bieżące wartości znajdujące się w niej są one zbędne — aktualizacja należy zastąpić je wszystkie. W tym celu należy użyć operacji **table_operation::insert_or_replace_entity** . Operacja wstawia jednostki, jeśli nie istnieje lub zamieni go, jeśli tak, niezależnie od tego, gdy jest realizowana ostatniej aktualizacji. W poniższym przykładzie jednostki klienta z powodu Marcin Jankowski nadal jest pobierana, ale jest następnie zapisywany na serwerze za pośrednictwem **table_operation::insert_or_replace_entity**. Zostaną zastąpione wszelkie aktualizacje wprowadzone do jednostki między pobierania i operacji aktualizacji.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## <a name="query-a-subset-of-entity-properties"></a>Kwerendy podzbiór właściwości obiektu  
Kwerendy do tabeli można pobrać kilka właściwości z obiektu. Kwerendy poniższy kod wykorzystuje metodę **table_query::set_select_columns** zwraca tylko adresy e-mail osób w tabeli.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] Wykonywanie zapytań za kilka właściwości obiektu jest operacji efektywniejsze niż pobieranie wszystkich właściwości.

## <a name="delete-an-entity"></a>Usuwanie obiektu
Możesz łatwo usunąć obiekt, po pobraniu go. Po pobraniu jednostki połączeń **table_operation::delete_entity** z jednostką do usunięcia. Następnie wywołać metodę **cloud_table.execute** . Poniższy kod pobiera i usuwa obiekt z kluczem partition "Kit" i kluczem wiersza "Marcin".  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## <a name="delete-a-table"></a>Usuwanie tabeli
Na koniec w poniższym przykładzie powoduje usunięcie tabeli z konta miejsca do magazynowania. Tabela, która została usunięta będzie mógł on odtworzony okres, po usunięciu.  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## <a name="next-steps"></a>Następne kroki
Teraz, gdy znasz już podstawy miejsca w tabeli, wykonaj te łącza, aby dowiedzieć się więcej o magazyn Azure:  

-   [Jak używać magazyn obiektów Blob z C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Jak używać magazyn kolejek z C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Lista zasobów Azure magazynu w języku C++](storage-c-plus-plus-enumeration.md)
-   [Biblioteka klienta miejsca do magazynowania dla odwołania C++](http://azure.github.io/azure-storage-cpp)
-   [Azure dokumentacji miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/)
