<properties
    pageTitle="Lokalnej aplikacji z magazynem obiektów blob (Java) | Microsoft Azure"
    description="Dowiedz się, jak utworzyć aplikację konsoli przekazywanie obrazu Azure, a następnie jest wyświetlana w przeglądarce. Przykłady kodu w języku Java."
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

# <a name="on-premises-application-with-blob-storage"></a>Aplikacja lokalnego z magazynem obiektów blob

## <a name="overview"></a>Omówienie

W poniższym przykładzie pokazano, jak magazynowania Azure umożliwia przechowywanie obrazów w Azure. Kod w tym artykule dotyczy aplikacji konsoli przekazywanie obrazu Azure, a następnie tworzy plik HTML, która jest wyświetlana w przeglądarce.

## <a name="prerequisites"></a>Wymagania wstępne

- Zainstalowano Java Developer Kit (JDK), 1,6 lub nowszego.
- Azure SDK jest zainstalowany.
- SŁOIK bibliotek Azure Java i wszelkie słoików dotyczy zależności są zainstalowane i w ścieżce kompilacji używane przez kompilujący Java. Aby uzyskać informacje o instalowaniu bibliotek Azure dla języka Java zobacz [Pobieranie SDK Azure dla języka Java](java-download-azure-sdk.md).
- Konto Azure magazynowania została skonfigurowana. Nazwa konta i klucz konta dla konta miejsca do magazynowania będą używane przez kod w tym artykule. Zobacz, [jak tworzenie konta miejsca do magazynowania](storage-create-storage-account.md#create-a-storage-account) dla informacji na temat tworzenia konta miejsca do magazynowania i [Wyświetlanie i kopiowanie klawiszy dostępu miejsca do magazynowania](storage-create-storage-account.md#view-and-copy-storage-access-keys) dla informacji na temat pobierania klucz konta.

- Plik obrazu lokalne o nazwie został utworzony przechowywane w c: ścieżka\\myimages\\image1.jpg. Możesz również zmienić konstruktora   **FileInputStream** w tym przykładzie, aby użyć innego obrazu ścieżkę i nazwę pliku.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="to-use-azure-blob-storage-to-upload-a-file"></a>Za pomocą magazynem obiektów blob Azure przekazywanie pliku

Poniżej przedstawiono procedurę krok po kroku. Jeśli chcesz od razu przejść, cały kod znajduje się w dalszej części tego artykułu.

Rozpocznij kodu, dołączając importowanie Azure główne klasy miejsca do magazynowania, klas klienta obiektów blob platformy Azure, klasy języka Java Jo i klasy **URISyntaxException** .

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

Zadeklaruj klasę o nazwie **StorageSample**i zawierać otwierający **{**.

    public class StorageSample {

W klasie **StorageSample** deklarować Zmienna ciągu, która będzie zawierać domyślny protokół punktu końcowego, nazwę konta magazynu i miejsca do magazynowania klawisz dostępu, zgodnie z ustawieniami na koncie Azure miejsca do magazynowania. Zamienianie wartości symbol zastępczy **usługi\_konta\_nazwy** i **z\_konta\_klucza** swojego konta klucz konta i nazwę, odpowiednio.

    public static final String storageConnectionString =
           "DefaultEndpointsProtocol=http;" +
               "AccountName=your_account_name;" +
               "AccountKey=your_account_name";

Dodawanie w swojej deklaracji **głównej**, **Spróbuj** bloku, a niezbędne nawiasy otwarte **{**.

    public static void main(String[] args)
    {
        try
        {

Deklarowanie zmiennych typu (opisy są dla jak są używane w tym przykładzie):

-   **CloudStorageAccount**: używane zainicjować obiekt konta z nazwę konta magazynu platformy Azure i klawisz i, aby utworzyć obiekt klienta obiektów blob.
-   **CloudBlobClient**: umożliwia dostęp do usługi obiektów blob.
-   **CloudBlobContainer**: umożliwia tworzenie kontenera obiektów blob, lista obiektów blob w kontenerze i usuń kontener.
-   **CloudBlockBlob**: umożliwia przekazywanie pliku obrazu lokalnych w kontenerze.

<!-- -->

    CloudStorageAccount account;
    CloudBlobClient serviceClient;
    CloudBlobContainer container;
    CloudBlockBlob blob;

Przypisz wartość zmiennej **konta** .

    account = CloudStorageAccount.parse(storageConnectionString);

Przypisz wartość zmiennej **serviceClient** .

    serviceClient = account.createCloudBlobClient();

Przypisz wartość zmiennej **kontener** . Firma Microsoft wyświetlony odwołanie do kontenera o nazwie **gettingstarted**.

    // Container name must be lower case.
    container = serviceClient.getContainerReference("gettingstarted");

Tworzenie kontenera. Ta metoda utworzy kontenera, jeśli go nie istnieje (i zwraca **wartość true**). Jeśli istnieje kontenera, zwróci **wartość false**. Innym sposobem **createIfNotExists** jest **Tworzenie** metodę (który zwróci błąd, jeśli już istnieje kontener).

    container.createIfNotExists();

Ustaw dostęp anonimowy kontenera.

    // Set anonymous access on the container.
    BlobContainerPermissions containerPermissions;
    containerPermissions = new BlobContainerPermissions();
    containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
    container.uploadPermissions(containerPermissions);

Odwołać się do obiektów blob blok reprezentujące obiektów blob w magazynie Azure.

    blob = container.getBlockBlobReference("image1.jpg");

Aby odwołać się do lokalnie utworzony plik, który będzie przekazać za pomocą konstruktora **pliku** . Upewnij się, że ten plik został utworzony przed uruchomieniem kodu.

    File fileReference = new File ("c:\\myimages\\image1.jpg");

Przekaż plik lokalny za pośrednictwem połączenia do metody **CloudBlockBlob.upload** . Pierwszy parametr metody **CloudBlockBlob.upload** to obiekt **FileInputStream** reprezentuje plik lokalny, które zostaną przekazane do magazynu Azure. Drugi parametr jest, w bajtach rozmiar pliku.

    blob.upload(new FileInputStream(fileReference), fileReference.length());

Wywołaj Pomocnik o nazwie **MakeHTMLPage**, aby skonfigurować podstawowe stronę HTML zawierający ** &lt;obraz&gt; ** elementu ze źródłem Ustaw blob, w którym znajduje się teraz w konto Azure miejsca do magazynowania. Kod **MakeHTMLPage** zostaną omówione w dalszej części tego artykułu.

    MakeHTMLPage(container);

Wydrukuj komunikat o stanie i informacje o utworzonej strony HTML.

    System.out.println("Processing complete.");
    System.out.println("Open index.html to see the images stored in your storage account.");

Zamknij bloku **Spróbuj** , wstawiając Zamknij nawias: **}**

Obsługa następujące wyjątki:

-   **FileNotFoundException**: może zostać wygenerowany przez konstruktorów **FileInputStream** lub **FileOutputStream** .
-   **StorageException**: może zostać wygenerowany przez Biblioteka miejsca do magazynowania Azure klienta.
-   **URISyntaxException**: może zostać wygenerowany przez metodę **ListBlobItem.getUri** .
-   **Wyjątek**: obsługa wyjątków ogólne.

<!-- -->

    catch (FileNotFoundException fileNotFoundException)
    {
        System.out.print("FileNotFoundException encountered: ");
        System.out.println(fileNotFoundException.getMessage());
        System.exit(-1);
    }
    catch (StorageException storageException)
    {
        System.out.print("StorageException encountered: ");
        System.out.println(storageException.getMessage());
        System.exit(-1);
    }
    catch (URISyntaxException uriSyntaxException)
    {
        System.out.print("URISyntaxException encountered: ");
        System.out.println(uriSyntaxException.getMessage());
        System.exit(-1);
    }
    catch (Exception e)
    {
        System.out.print("Exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Zamknij **głównym** , wstawiając Zamknij nawias: **}**

Utwórz metodę o nazwie **MakeHTMLPage** Tworzenie strony podstawowej HTML. Ta metoda wymaga parametru typu **CloudBlobContainer**, która będzie używana do iteracji na liście przekazane obiektów blob. Ta metoda będzie generują wyjątki typu **FileNotFoundException**, który może zostać wygenerowany konstruktora **FileOutputStream** , a **URISyntaxException**, który może zostać wygenerowany przez metodę **ListBlobItem.getUri** . Dołączanie nawiasem otwierającym **{**.

    public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
    {

Utwórz plik lokalny o nazwie **index.html**.

    PrintStream stream;
    stream = new PrintStream(new FileOutputStream("index.html"));

Zapisywać do pliku lokalnego, dodając w ** &lt;html&gt;**, ** &lt;nagłówka&gt;**, i ** &lt;treści&gt; ** elementy.

    stream.println("<html>");
    stream.println("<header/>");
    stream.println("<body>");

Iterację na liście przekazane obiektów blob. Dla każdego blob na stronie HTML Utwórz ** &lt;img&gt; ** elementu, który ma jego atrybut **src** wysyłane do identyfikator URI to, ponieważ znajduje się na Twoim koncie Azure miejsca do magazynowania.
Mimo że został dodany tylko jeden obraz w tym przykładzie, jeśli została dodana więcej, kod chcesz przejść z nich.

Dla uproszczenia w tym przykładzie założono, że każdy przekazane obiektów blob to obraz. Jeśli zostały zaktualizowane blob niż obrazów lub obiektów blob strony zamiast blob blok, Ustaw kod, stosownie do potrzeb.

    // Enumerate the uploaded blobs.
    for (ListBlobItem blobItem : container.listBlobs()) {
    // List each blob as an <img> element in the HTML body.
    stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
    }

Zamknij ** &lt;treści&gt; ** elementu i ** &lt;html&gt; ** elementu.

    stream.println("</body>");
    stream.println("</html>");

Zamknij plik lokalny.

    stream.close();

Zamknij **MakeHTMLPage** , wstawiając Zamknij nawias: **}**

Zamknij **StorageSample** , wstawiając Zamknij nawias: **}**

Poniżej przedstawiono pełną kod w tym przykładzie. Pamiętaj, aby zmodyfikować wartości symbol zastępczy **z\_konta\_nazwę** i **z\_konta\_klucza** być używane odpowiednio nazwę konta i klucz konta.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import java.io.*;
    import java.net.URISyntaxException;

    // Create an image, c:\myimages\image1.jpg, prior to running this sample.
    // Alternatively, change the value used by the FileInputStream constructor
    // to use a different image path and file that you have already created.
    public class StorageSample {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                       "AccountName=your_account_name;" +
                       "AccountKey=your_account_name";

        public static void main(String[] args) {
            try {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;
                CloudBlockBlob blob;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.createIfNotExists();

                // Set anonymous access on the container.
                BlobContainerPermissions containerPermissions;
                containerPermissions = new BlobContainerPermissions();
                containerPermissions.setPublicAccess(BlobContainerPublicAccessType.CONTAINER);
                container.uploadPermissions(containerPermissions);

                // Upload an image file.
                blob = container.getBlockBlobReference("image1.jpg");

                File fileReference = new File("c:\\myimages\\image1.jpg");
                blob.upload(new FileInputStream(fileReference), fileReference.length());

                // At this point the image is uploaded.
                // Next, create an HTML page that lists all of the uploaded images.
                MakeHTMLPage(container);

                System.out.println("Processing complete.");
                System.out.println("Open index.html to see the images stored in your storage account.");

            } catch (FileNotFoundException fileNotFoundException) {
                System.out.print("FileNotFoundException encountered: ");
                System.out.println(fileNotFoundException.getMessage());
                System.exit(-1);
            } catch (StorageException storageException) {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            } catch (URISyntaxException uriSyntaxException) {
                System.out.print("URISyntaxException encountered: ");
                System.out.println(uriSyntaxException.getMessage());
                System.exit(-1);
            } catch (Exception e) {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }

        // Create an HTML page that can be used to display the uploaded images.
        // This example assumes all of the blobs are for images.
        public static void MakeHTMLPage(CloudBlobContainer container) throws FileNotFoundException, URISyntaxException
        {
            PrintStream stream;
            stream = new PrintStream(new FileOutputStream("index.html"));

            // Create the opening <html>, <header>, and <body> elements.
            stream.println("<html>");
            stream.println("<header/>");
            stream.println("<body>");

            // Enumerate the uploaded blobs.
            for (ListBlobItem blobItem : container.listBlobs()) {
                // List each blob as an <img> element in the HTML body.
                stream.println("<img src='" + blobItem.getUri() + "'/><br/>");
            }

            stream.println("</body>");

            // Complete the <html> element and close the file.
            stream.println("</html>");
            stream.close();
        }
    }

Oprócz przekazać plik obrazu lokalnego magazynu Azure, przykładowy kod tworzy namedindex.html plik lokalny, który można otworzyć w przeglądarce, aby wyświetlić przekazane obrazu.

Ponieważ kod zawiera nazwę konta i klucz konta, należy się upewnić, że kod źródłowy jest bezpieczny.

## <a name="to-delete-a-container"></a>Aby usunąć kontenera

Ponieważ jest naliczany do przechowywania, warto usunąć kontenera **gettingstarted** po zakończeniu eksperymentowanie z w tym przykładzie. Aby usunąć kontenera, użyj metody **CloudBlobContainer.delete** .

    container = serviceClient.getContainerReference("gettingstarted");
    container.delete();

Wywołać metody **CloudBlobContainer.delete** , proces inicjowania obiekty **CloudStorageAccount**, **ClodBlobClient**i **CloudBlobContainer** jest taka sama, jak pokazano w metodzie **createIfNotExist** . Poniżej przedstawiono przykład wykonane, umożliwiający usunięcie kontenera o nazwie **gettingstarted**.

    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;

    public class DeleteContainer {

        public static final String storageConnectionString =
                "DefaultEndpointsProtocol=http;" +
                   "AccountName=your_account_name;" +
                   "AccountKey=your_account_key";

        public static void main(String[] args)
        {
            try
            {
                CloudStorageAccount account;
                CloudBlobClient serviceClient;
                CloudBlobContainer container;

                account = CloudStorageAccount.parse(storageConnectionString);
                serviceClient = account.createCloudBlobClient();
                // Container name must be lower case.
                container = serviceClient.getContainerReference("gettingstarted");
                container.delete();

                System.out.println("Container deleted.");

            }
            catch (StorageException storageException)
            {
                System.out.print("StorageException encountered: ");
                System.out.println(storageException.getMessage());
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.print("Exception encountered: ");
                System.out.println(e.getMessage());
                System.exit(-1);
            }
        }
    }

Aby uzyskać omówienie innych klas magazyn obiektów blob i metod Zobacz, [jak używać magazyn obiektów Blob z języka Java](storage-java-how-to-use-blob-storage.md).

## <a name="next-steps"></a>Następne kroki

Wykonaj te łącza, aby dowiedzieć się więcej o bardziej złożone zadania miejsca do magazynowania.

- [Azure miejsca do magazynowania SDK dla języka Java](https://github.com/azure/azure-storage-java)
- [Odwołanie SDK klienta Azure miejsca do magazynowania](http://dl.windowsazure.com/storage/javadoc/)
- [Usługi Azure magazyn interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Blog zespołu Azure miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage/)
