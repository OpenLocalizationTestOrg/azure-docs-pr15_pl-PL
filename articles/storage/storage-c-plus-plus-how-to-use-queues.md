<properties
    pageTitle="Jak używać magazyn kolejek (C++) | Microsoft Azure"
    description="Dowiedz się, jak korzystać z usługi Magazyn kolejki w Azure. Przykłady są zapisywane w języku C++."
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

# <a name="how-to-use-queue-storage-from-c"></a>Jak używać magazyn kolejek z C++  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Omówienie
Ten przewodnik pojawi się, jak wykonywać typowe scenariusze przy użyciu usługi Azure kolejki miejsca do magazynowania. Przykłady są zapisywane w języku C++ i [Biblioteka klienta Azure miejsca do magazynowania dla C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). Scenariusze, w których zawierać **wstawiania**, **Wgląd**, **Pobieranie**i **Usuwanie** wiadomości w kolejce, a także **Tworzenie i usuwanie kolejek**.

>[AZURE.NOTE] Ten przewodnik jest przeznaczony dla biblioteki klienta miejsca do magazynowania Azure C++ w wersji 1.0.0 i nowszych. Zalecane wersja jest biblioteka klienta miejsca do magazynowania 2.2.0, która jest dostępna za pośrednictwem [NuGet](http://www.nuget.org/packages/wastorage) lub [GitHub](http://github.com/Azure/azure-storage-cpp/).

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Tworzenie aplikacji C++  
W tym przewodniku należy użyć funkcji miejsca do magazynowania, które mogą być uruchamiane w aplikacji C++.

W tym celu należy zainstalować Biblioteka klienta Azure miejsca do magazynowania dla C++ i utworzyć konto Azure miejsca do magazynowania w ramach subskrypcji Azure.

Aby zainstalować Biblioteka klienta Azure miejsca do magazynowania dla C++, można skorzystać następujących metod:

-   **Linux:** Postępuj zgodnie z instrukcjami na stronie [Biblioteka klienta Azure miejsca do magazynowania dla C++ — Plik README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .
-   **Systemu Windows:** W programie Visual Studio, kliknij przycisk **Narzędzia > Menedżer pakietów NuGet > Menedżer pakietów konsoli**. Wpisz następujące polecenie w [konsoli Menedżera pakietów NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , a następnie naciśnij klawisz **ENTER**.

        Install-Package wastorage

## <a name="configure-your-application-to-access-queue-storage"></a>Konfigurowanie aplikacji, aby uzyskać dostęp do magazynowania kolejki
Dodaj następujące instrukcje na początek pliku C++, w której chcesz uzyskać dostęp do kolejek za pomocą Azure magazynowania interfejsy API obejmują:  

    #include "was/storage_account.h"
    #include "was/queue.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Konfigurowanie parametrów połączenia Azure miejsca do magazynowania

Klient Azure magazynu używa parametrów połączenia miejsca do magazynowania do przechowywania punkty końcowe i poświadczenia dostępu do usług zarządzania danymi. Podczas działa w aplikacji klienckiej, należy podać parametry połączenia miejsca do magazynowania w następującym formacie, przy użyciu nazwy konta miejsca do magazynowania i klawisz dostępu miejsca do magazynowania dla konta miejsca do magazynowania w [Azure Portal](https://portal.azure.com) wartości *Nazwa konta* i *AccountKey* na liście. Aby uzyskać informacje na kontach miejsca do magazynowania i klawisze dostępu Zobacz [O Azure miejsca do magazynowania konta](storage-create-storage-account.md). W tym przykładzie pokazano, jak można deklarować statyczne pole do przechowywania parametry połączenia:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Aby przetestować aplikację w lokalnym komputerze z systemem Windows, możesz użyć Microsoft Azure [emulatora miejsca do magazynowania](storage-use-emulator.md) jest zainstalowany wraz z [Azure SDK](https://azure.microsoft.com/downloads/). Emulator miejsca do magazynowania jest narzędziem symuluje dostępne platformy Azure na komputer lokalny rozwój usługi obiektów Blob, kolejki i tabeli. W poniższym przykładzie pokazano, jak można deklarować statyczne pole do przechowywania parametry połączenia do swojego emulatora magazynu lokalnego:  

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Aby rozpocząć emulatora Azure miejsca do magazynowania, wybierz przycisk **Start** , lub naciśnij klawisz **systemu Windows** . Zacznij wpisywać **Azure emulatora miejsca do magazynowania**, a z listy aplikacji wybierz pozycję **Microsoft Azure miejsca do magazynowania emulatora** .

Następujące próbki przyjęto założenie, że używasz jednej z tych dwóch metod uzyskiwania parametrów połączenia miejsca do magazynowania.

## <a name="retrieve-your-connection-string"></a>Pobieranie ciągu połączenia
Za pomocą klasy **cloud_storage_account** reprezentować informacji o koncie miejsca do magazynowania. Aby pobrać informacji o koncie miejsca do magazynowania z parametrów połączenia miejsca do magazynowania, można użyj metody **analizy** .

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-queue"></a>Jak: Tworzenie kolejki
Obiekt **cloud_queue_client** pozwala uzyskać obiekty odniesienia dla kolejki. Poniższy kod tworzy obiekt **cloud_queue_client** .

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

Aby odwołać się do kolejki, który ma być wyświetlany za pomocą obiektu **cloud_queue_client** . Jeśli nie istnieje, można utworzyć kolejki.

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## <a name="how-to-insert-a-message-into-a-queue"></a>Jak: wstawianie wiadomości w kolejce
Aby wstawić wiadomości do istniejącej kolejki, najpierw należy utworzyć nowy **cloud_queue_message**. Następnie wywołać metodę **add_message** . **Cloud_queue_message** można utworzyć w ciągu lub tablicy **bajtów** . Poniżej przedstawiono kod, który tworzy kolejki (Jeśli nie istnieje) i wstawia komunikat "Witaj świecie":

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## <a name="how-to-peek-at-the-next-message"></a>Jak: wglądu do następnej wiadomości
Czy wglądu do wiadomości pierwszych stronach kolejki, bez usuwania go z kolejki przez wywołanie metody **peek_message** .

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Jak: Zmienianie zawartości wiadomość w kolejce
Możesz zmienić zawartość wiadomości w miejscu w kolejce. Jeśli wiadomość reprezentuje zadania, można tę funkcję aktualizacji stanu zadania. Poniższy kod aktualizuje wiadomości kolejki nowej zawartości i ustawia limit czasu widoczność, aby rozszerzyć innego 60 sekund. Zapisuje stan pracy związanej z tą wiadomością i udostępnia klienta innego minutę, aby kontynuować pracę w oknie komunikatu. Można użyć tej techniki do śledzenia przepływów pracy wielokrotnego krok w odniesieniu do wiadomości kolejki bez konieczności rozpoczęcie od początku, jeśli krok przetwarzania kończy się niepowodzeniem z powodu błędu sprzętu i oprogramowania. Zazwyczaj chcesz zachować to licznik ponów próbę, a jeśli wiadomość jest próby wykonania więcej niż n razy chcesz go usunąć. Chroni to przed powodujące błąd aplikacji zawsze, gdy jest przetwarzana w wiadomości.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## <a name="how-to-de-queue-the-next-message"></a>Jak: Anuluj w kolejce następnej wiadomości
Kod kolejek usuwania wiadomości z kolejki w dwóch krokach. Podczas połączenia **get_message**zostanie wyświetlony następnej wiadomości w kolejce. Komunikat zwrócone przez **get_message** staje się niewidoczne dla innych kodu do czytania wiadomości z tej kolejki. Aby zakończyć, usuwając wiadomość z kolejki, możesz również wywołać **delete_message**. Opisanego niżej procesu usuwania wiadomości gwarantuje, że jeśli kodu przetwarzania wiadomości ze względu na błąd sprzętu i oprogramowania nie powiedzie się, inne wystąpienie kodu można wyświetlenie tego samego komunikatu i spróbuj ponownie. Kod wywołuje prawo **delete_message** po przetworzeniu wiadomości.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message);

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Jak: korzystać z dodatkowych opcji dla Anuluj Kolejkowanie wiadomości
Istnieją dwa sposoby można dostosować pobieranie wiadomości z kolejki. Najpierw zostanie wyświetlony partię wiadomości (32). Drugi można ustawić invisibility wydłużyć lub skrócić przekroczenie limitu czasu, umożliwiając kodzie więcej lub mniej czasu na w pełni procesu każdej wiadomości. W poniższym przykładzie użyto metody **get_messages** do 20 wiadomości są pobierane za jednym połączenia. Przetwarza następnie każdej wiadomości przy użyciu pętli **for** . Ustawia również invisibility przekroczenia limitu czasu, aby pięć minut dla każdej wiadomości. Należy zauważyć, że 5 minut, zostanie uruchomiony dla wszystkich wiadomości w tym samym czasie, dlatego po 5 minut jest przekazywane, ponieważ wywołanie **get_messages**, wszystkie wiadomości, które nie zostały usunięte stają się widoczne ponownie.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## <a name="how-to-get-the-queue-length"></a>Jak: uzyskiwanie długość kolejki
Możesz uzyskać szacowana liczba wiadomości w kolejce. Metoda **download_attributes** zwróci się do usługi kolejki w celu pobrania atrybutów kolejki, w tym liczbę wiadomości. Metoda **approximate_message_count** pobiera przybliżonego liczba wiadomości w kolejce.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## <a name="how-to-delete-a-queue"></a>Jak: Usuwanie kolejki
Aby usunąć kolejkę i wszystkie zawarte w niej wiadomości, wywołać metody **delete_queue_if_exists** obiektu kolejki.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy miejsca w kolejce, wykonaj te łącza, aby dowiedzieć się więcej o magazyn Azure.

-   [Jak używać magazyn obiektów Blob z C++](storage-c-plus-plus-how-to-use-blobs.md)
-   [Jak używać magazyn tabel z C++](storage-c-plus-plus-how-to-use-tables.md)
-   [Zasoby dla listy Azure miejsca do magazynowania w języku C++](storage-c-plus-plus-enumeration.md)
-   [Biblioteka klienta miejsca do magazynowania dla odwołania C++](http://azure.github.io/azure-storage-cpp)
-   [Dokumentacja Azure miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/)

