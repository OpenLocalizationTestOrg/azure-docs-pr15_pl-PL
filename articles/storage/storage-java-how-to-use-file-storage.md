<properties
    pageTitle="Jak używać magazyn plików z języka Java | Microsoft Azure"
    description="Dowiedz się, jak przekazać, pobieranie listy i usuwanie plików za pomocą usługi Azure pliku. Próbki napisany w języku Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-file-storage-from-java"></a>Jak używać magazyn plików z języka Java

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Omówienie

W tym przewodniku dowiesz się, jak umożliwiają wykonywanie podstawowych operacji w usłudze Microsoft Azure plik miejsca do magazynowania. Za pośrednictwem próbki napisany w języku Java dowiesz się, jak utworzyć udział i katalogów, przekazywanie, listy i usuwanie plików. Jeśli jesteś nowym użytkownikiem usługi przechowywania plików Microsoft Azure, przechodzące przez pojęcia w sekcjach będzie zrozumieć próbki bardzo przydatne.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Tworzenie aplikacji Java

Aby utworzyć próbki, konieczne będzie Java Development Kit (JDK) i [[Azure SDK miejsca do magazynowania dla języka Java]]. Należy również utworzono konto Azure miejsca do magazynowania.

## <a name="setup-your-application-to-use-file-storage"></a>Konfigurowanie aplikacji do przechowywania plików za pomocą

Aby korzystać z magazynu Azure interfejsy API, dodaj następującą instrukcję na początek plik języka Java, gdy chcesz uzyskać dostęp do usług miejsca do magazynowania z.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.file.*;

## <a name="setup-an-azure-storage-connection-string"></a>Konfigurowanie parametrów połączenia Azure miejsca do magazynowania

Aby użyć przechowywania plików, musisz połączyć się z kontem Azure miejsca do magazynowania. Pierwszym krokiem jest skonfigurować parametry połączenia, które będą używane do łączenia się z kontem miejsca do magazynowania. Załóżmy Zdefiniuj zmienną statyczne to zrobić.

    // Configure the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account_name;" +
        "AccountKey=your_storage_account_key";

> [AZURE.NOTE] Zamień your_storage_account_name i your_storage_account_key rzeczywistych wartości dla Twojego konta miejsca do magazynowania.

## <a name="connecting-to-an-azure-storage-account"></a>Łączenie się z kontem Azure miejsca do magazynowania

Do łączenia się z kontem miejsca do magazynowania, należy użyć obiektu **CloudStorageAccount** przekazywania parametrów połączenia do metody **analizy** .

    // Use the CloudStorageAccount object to connect to your storage account
    try {
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
    } catch (InvalidKeyException invalidKey) {
        // Handle the exception
    }

**CloudStorageAccount.parse** generuje InvalidKeyException, więc musisz je umieścić wewnątrz bloku spróbuj efektywnej.

## <a name="how-to-create-a-share"></a>Jak: utworzyć udział

Wszystkie pliki i katalogi w magazynie pliku znajdują się w kontenerze o nazwie **Udostępnianie**. Konta miejsca do magazynowania może mieć tyle akcji, jak pozwala na wydajność konta. Aby uzyskać dostęp do udziału oraz jego zawartość, musisz za pomocą klienta miejsca do magazynowania plików.

    // Create the file storage client.
    CloudFileClient fileClient = storageAccount.createCloudFileClient();

Przy użyciu klienta miejsca do magazynowania plików, możesz następnie uzyskać odwołanie do udziału.

    // Get a reference to the file share
    CloudFileShare share = fileClient.getShareReference("sampleshare");

Aby utworzyć faktycznie Udostępnij, użyj metody **createIfNotExists** obiektu CloudFileShare.

    if (share.createIfNotExists()) {
        System.out.println("New share created");
    }

W tym momencie **Udostępnianie** zawiera odwołanie do udziału o nazwie **sampleshare**.

## <a name="how-to-upload-a-file"></a>Jak: przekazywanie pliku

Udostępnianie miejsca do magazynowania plików Azure zawiera co najmniej, katalogu, w którym mogą znajdować się pliki. W tej sekcji dowiesz się, jak przekazywać plików z magazynu lokalnego do katalogu głównego udziału.

Pierwszym krokiem podczas przekazywania pliku jest uzyskanie odwołanie do katalogu, w którym powinien znajdować się. W tym celu nawiązywania połączeń z metody **getRootDirectoryReference** obiektu Udostępnij.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

Teraz, gdy masz odwołanie do katalogu głównego udziału, możesz przekazać plik na za pomocą następującego kodu.

    // Define the path to a local file.
    final String filePath = "C:\\temp\\Readme.txt";

    CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
    cloudFile.uploadFromFile(filePath);

## <a name="how-to-create-a-directory"></a>Jak: Tworzenie katalogu

Można również uporządkować miejsca do magazynowania, wysyłanie plików wewnątrz podkatalogów zamiast je wszystkie w katalogu głównym. Usługa magazynu pliku Azure umożliwia tworzenie tyle katalogów, aby umożliwić Twojego konta. Kod poniżej utworzy podrzędne katalogu o nazwie **sampledir** w katalogu głównym.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the sampledir directory
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    if (sampleDir.createIfNotExists()) {
        System.out.println("sampledir created");
    } else {
        System.out.println("sampledir already exists");
    }

## <a name="how-to-list-files-and-directories-in-a-share"></a>Jak: Lista pliki i katalogi w udziale

Uzyskiwanie listy pliki i katalogi w udziale łatwo jest wykonywane przez wywołanie **listFilesAndDirectories** w odwołaniu CloudFileDirectory. Metoda zwraca listę ListFileItem obiekty, które można przejść na. Na przykład poniższy kod spowoduje wyświetlenie listy pliki i katalogi w katalogu głównym.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
        System.out.println(fileItem.getUri());
    }


## <a name="how-to-download-a-file"></a>Jak: pobieranie pliku

Jednym częściej czynności wykonywane przed magazyn plików jest pobieranie plików. W poniższym przykładzie kodu do pobrania SampleFile.txt i wyświetla jego zawartość.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the directory that contains the file
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    //Get a reference to the file you want to download
    CloudFile file = sampleDir.getFileReference("SampleFile.txt");

    //Write the contents of the file to the console.
    System.out.println(file.downloadText());

## <a name="how-to-delete-a-file"></a>Jak: usuwanie pliku

Inny typowych operacji miejsca do magazynowania plików jest usunięcie plików. Poniższy kod usuwa plik o nazwie SampleFile.txt przechowywany w katalogu o nazwie **sampledir**.


    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory where the file to be deleted is in
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    String filename = "SampleFile.txt"
    CloudFile file;

    file = containerDir.getFileReference(filename)
    if ( file.deleteIfExists() ) {
        System.out.println(filename + " was deleted");
    }


## <a name="how-to-delete-a-directory"></a>Jak: Usuwanie katalogu

Usuwanie katalogu jest zadaniem dość proste, jednak należy zauważyć, że nie można usunąć katalogu zawierającego nadal plików ani innych katalogów.

    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory you want to delete
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    // Delete the directory
    if ( containerDir.deleteIfExists() ) {
        System.out.println("Directory deleted");
    }


## <a name="how-to-delete-a-share"></a>Jak: usunąć udział

Usuwanie udziału jest wykonywane przez wywołującego metodę **deleteIfExists** w obiekcie CloudFileShare. Poniżej przedstawiono przykładowy kod, który oznacza, że.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the file client.
       CloudFileClient fileClient = storageAccount.createCloudFileClient();

       // Get a reference to the file share
       CloudFileShare share = fileClient.getShareReference("sampleshare");

       if (share.deleteIfExists()) {
           System.out.println("sampleshare deleted");
       }
    } catch (Exception e) {
        e.printStackTrace();
    }

## <a name="next-steps"></a>Następne kroki

Jeśli chcesz dowiedzieć się więcej o innych Azure miejsca do magazynowania interfejsy API, skorzystaj z poniższych łączy.

- [Centrum deweloperów języka Java](http://azure.microsoft.com/develop/java/)
- [Azure miejsca do magazynowania SDK dla języka Java](https://github.com/azure/azure-storage-java)
- [Azure miejsca do magazynowania SDK dla systemu Android](https://github.com/azure/azure-storage-android)
- [Odwołanie SDK klienta Azure miejsca do magazynowania](http://dl.windowsazure.com/storage/javadoc/)
- [Usługi Azure magazyn interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Blog zespołu Azure miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage/)
- [Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md)
