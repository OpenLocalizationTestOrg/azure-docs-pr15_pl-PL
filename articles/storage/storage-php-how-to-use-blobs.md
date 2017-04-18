<properties
    pageTitle="Jak używać magazyn obiektów blob (miejsca przechowywania obiektu) z PHP | Microsoft Azure"
    description="Przechowywanie danych niestrukturalne w chmurze z magazynem obiektów Blob platformy Azure (miejsca przechowywania obiektu)."
    documentationCenter="php"
    services="storage"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-php"></a>Jak używać magazyn obiektów blob z PHP

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie

Magazyn obiektów Blob platformy Azure to usługa, która przechowuje niestrukturalne dane w chmurze jako obiekty i obiektów blob. Magazyn obiektów blob mogą zawierać dowolnego typu tekst lub dane binarne, takich jak dokument, plik lub Instalator aplikacji. Magazyn obiektów blob jest również określane jako miejsca przechowywania obiektu.

Ten przewodnik zawiera się, jak wykonywać typowe scenariusze korzystania z usługi obiektów blob platformy Azure. Przykłady są zapisane w PHP i używanie [SDK Azure dla PHP] [download]. Scenariusze, w których zawierać **przekazywanie**, **Wyświetlanie**, **Pobieranie**i **Usuwanie** obiektów blob. Aby uzyskać więcej informacji dotyczących obiektów blob zobacz sekcję [Następne kroki](#next-steps) .

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>Tworzenie aplikacji PHP

Tylko wymagania dotyczące tworzenia aplikacji PHP, który uzyskuje dostęp do usługi obiektów blob platformy Azure jest odwołujących się klas w Azure SDK dla PHP z w kodzie. Za pomocą dowolnego narzędzia programistyczne do tworzenia aplikacji, w tym Notatnik.

W tym przewodniku można użyć funkcji usług, które mogą być wywoływane w aplikacji PHP, lokalnie lub w kodzie uruchomionych w roli Azure sieci web, roli Pracownik lub witryny sieci Web.

## <a name="get-the-azure-client-libraries"></a>Pobieranie bibliotek Azure klienta

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Konfigurowanie aplikacji do uzyskania dostępu do usługi obiektów blob

Aby korzystać z usługi obiektów blob platformy Azure interfejsy API, należy:

1. Dokumentacja dotycząca pliku automatyczna ładowarka za pomocą instrukcji [require_once] i
2. Dokumentacja dotycząca wszystkich klas, których można użyć.

W poniższym przykładzie pokazano, jak dołączyć plik automatyczna ładowarka i odwołuje się klasy **ServicesBuilder** .

> [AZURE.NOTE] W tym przykładzie (i inne przykłady w tym artykule) przyjęto założenie, że zainstalowano bibliotek klienta PHP dla Azure za pośrednictwem kompozytor. Jeśli zainstalowano biblioteki ręcznie, muszą odwoływać się `WindowsAzure.php` automatyczna ładowarka pliku.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


W poniższych przykładach `require_once` instrukcji będzie zawsze wyświetlana, ale odwołuje się to konieczne, na przykład do wykonywania zajęcia.

## <a name="set-up-an-azure-storage-connection"></a>Konfigurowanie połączenia Azure przestrzeni dyskowej

Wystąpienia obiektów blob platformy Azure klienta usługi, musisz najpierw włączyć prawidłowe parametry połączenia. Format ciągu połączenia obiektów blob usługi jest:

Aby uzyskać dostęp do usługi live:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Do uzyskiwania dostępu do emulatora miejsca do magazynowania:

    UseDevelopmentStorage=true


Aby utworzyć dowolnego klienta usługi Azure, musisz użyć klasy **ServicesBuilder** . Można:

* Przekazywane parametry połączenia bezpośrednio do niego lub
* **CloudConfigurationManager (CCM)** umożliwiają sprawdzanie wielu źródeł zewnętrznych ciągu połączenia:
    * Domyślnie pochodzi z obsługą jedno źródło zewnętrzne — zmienne środowiska.
    * Możesz dodać nowe źródła, wydłużając klasy **ConnectionStringSource** .

Przykłady przedstawionych poniżej parametry połączenia zostanie przekazany bezpośrednio.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## <a name="create-a-container"></a>Tworzenie kontenera

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Obiekt **BlobRestProxy** umożliwia tworzenie kontenera obiektów blob przy użyciu metody **tworzony kontener** . Podczas tworzenia kontenera, możesz ustawić opcje w kontenerze, ale jest to nie wymagane. (W poniższym przykładzie pokazano sposób Ustawianie kontenera listy kontroli dostępu (ACL) i metadanych kontenera).

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
    use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Wywoływanie **setPublicAccess (PublicAccessType::CONTAINER\_i\_obiektów blob)** powoduje, że dane kontener oraz obiektów blob jest dostępny za pośrednictwem anonimowe żądania. Wywoływanie **setPublicAccess(PublicAccessType::BLOBS_ONLY)** sprawia, że tylko dane obiektów blob dostępny za pośrednictwem anonimowe żądania. Aby uzyskać więcej informacji na temat kontenera ACL zobacz [kontener zestawu list ACL (interfejsu API usługi REST)][container-acl].

Aby uzyskać więcej informacji na temat kodów błędów obiektów Blob usługi, zobacz [Kody błędów usługi obiektów Blob][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektów blob do kontenera

Aby przekazać plik jako obiektów blob, użyj metody **BlobRestProxy -> createBlockBlob** . Operacja tworzy to, jeśli nie istnieje lub zastępuje go, jeśli tak. W poniższym przykładzie kodu założono, że kontener został już utworzony i używa [fopen] [ fopen] do otwarcia pliku jako strumienia.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Należy zauważyć, że poprzedni przykład przekazywanie obiektów blob jako strumienia. Jednak obiektów blob również można przekazywać jako przy użyciu ciągu, na przykład [pliku\_uzyskać\_zawartość] [ file_get_contents] funkcji. Aby to zrobić przy użyciu poprzedniej próbki, zmień `$content = fopen("c:\myfile.txt", "r");` do `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Lista obiektów blob w kontenerze

Aby wyświetlić listę obiektów blob w kontenerze, użyj metody **BlobRestProxy -> listBlobs** z **foreach** pętli do pętli między wynik. Poniższy kod zawiera nazwę każdego obiektów blob jako wynik w kontenerze i wyświetla jego identyfikatora URI do przeglądarki.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="download-a-blob"></a>Pobieranie obiektów blob

Aby pobrać obiektów blob, wywołaj metodę **BlobRestProxy -> getBlob** , a następnie wywołać metodę **getContentStream** w wyniku obiektu **GetBlobResult** .

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Należy zauważyć, że w powyższym przykładzie otrzymuje obiektów blob jako zasób strumienia (zachowanie domyślne). Jednak może zawierać [strumienia\_uzyskać\_zawartość] [ stream-get-contents] funkcji, aby przekonwertować strumienia zwracany ciąg.

## <a name="delete-a-blob"></a>Usuwanie obiektów blob

Aby usunąć obiektów blob, przekazać nazwę kontenera i nazwy obiektów blob do **BlobRestProxy -> deleteBlob**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete blob.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-a-blob-container"></a>Usuwanie kontenera obiektów blob

Na koniec aby usunąć kontenera obiektów blob, przekazać nazwa kontenera do **BlobRestProxy -> deleteContainer**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już podstawy pracy obiektów blob platformy Azure, skorzystaj z poniższych łączy, aby uzyskać informacje o bardziej złożone zadania miejsca do magazynowania.

- Odwiedź [blog zespołu magazyn Azure](http://blogs.msdn.com/b/windowsazurestorage/)
- Zobacz [przykład obiektów blob blok PHP](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
- Zobacz [przykład obiektów blob strony PHP](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
- [Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)
 
Aby uzyskać więcej informacji zobacz też [Centrum deweloperów PHP](/develop/php/).


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
