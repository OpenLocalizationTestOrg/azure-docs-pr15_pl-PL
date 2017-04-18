<properties
    pageTitle="Tworzenie migawki tylko do odczytu obiektów blob | Microsoft Azure"
    description="Dowiedz się, jak utworzyć migawkę obiektów blob kopie zapasowe danych obiektów blob w danym momencie w czasie. Zrozumieć, jak wystawiona migawek i sposobach ich używania, aby zminimalizować opłaty wydajność."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="create-a-blob-snapshot"></a>Tworzenie migawki obiektów blob

## <a name="overview"></a>Omówienie

Migawki jest tylko do odczytu wersji blob, w którym jest przyjmowana w punkcie w czasie. Migawki są przydatne do tworzenia kopii zapasowych obiektów blob. Po utworzeniu migawki czytanie, skopiować lub usunąć go, ale nie można zmodyfikować.

Migawki obiektów blob jest identyczny z jego podstawowej obiektów blob, ale obiektów blob URI ma wartość **DateTime** dołączane do obiektów blob identyfikatora URI w celu wskazania czasu, w którym wykonano migawki. Na przykład jeśli strony blob URI jest `http://storagesample.core.blob.windows.net/mydrives/myvhd`, migawki identyfikator URI jest podobne do `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`. 

> [AZURE.NOTE] Wszystkich migawek udostępnianie podstawowej obiektów blob identyfikatora URI. Tylko rozróżnianie podstawowej obiektów blob i migawki jest dołączonych wartość **daty/godziny** .

Obiektów blob może mieć dowolną liczbę migawek. Migawki zachowywane, dopóki nie zostaną jawnie usunięte. Migawki nie outlive jego podstawie obiektów blob. Można wyliczyć migawek skojarzone z podstawowych obiektów blob do śledzenia Twojej bieżącej migawki.

Podczas tworzenia migawki obiektów blob właściwości obiektów blob systemu są kopiowane do migawki o tych samych wartościach. Podstawa obiektów blob metadanych, zostanie skopiowana również do migawki, o ile nie wybierzesz osobnych metadanych dla migawki podczas jej tworzenia.

Wszelkie dzierżawy skojarzone z podstawowych obiektów blob nie wpływają na migawki. Nie można pobrać dzierżawę na migawkę.

## <a name="create-a-snapshot"></a>Tworzenie migawki

W poniższym przykładzie pokazano sposób tworzenia migawki w .NET. W tym przykładzie określa osobne metadanych dla migawki po jego utworzeniu.

    private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
    {
        // Create a new block blob in the container.
        CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

        // Add blob metadata.
        baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

        try
        {
            // Upload the blob to create it, with its metadata.
            await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

            // Sleep 5 seconds.
            System.Threading.Thread.Sleep(5000);

            // Create a snapshot of the base blob.
            // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
            // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
            Dictionary<string, string> metadata = new Dictionary<string, string>();
            metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
            await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        }
        catch (StorageException e)
        {
            Console.WriteLine(e.Message);
            Console.ReadLine();
            throw;
        }
    }
 

## <a name="copy-snapshots"></a>Kopiowanie migawki

Kopiowanie obejmującego obiektów blob i migawek, przestrzegaj następujących reguł:

- Możesz skopiować migawkę na jego podstawie obiektów blob. Promowanie migawkę do pozycji Podstawa obiektów blob, można przywrócić starszej wersji programu obiektów blob. Pozostaje migawki, ale podstawy obiektów blob jest zastępowany zapisywalny dysk kopii migawki.

- Migawkę można kopiować do obiektów blob docelowego pod inną nazwą. Wynikowa blob miejsce docelowe jest zapisywalny dysk obiektów blob i nie migawkę.

- Po skopiowaniu blob źródła dowolnego migawek obiektów blob źródła nie są kopiowane do miejsca docelowego. Gdy blob miejsce docelowe jest zastępowany kopię, wszelkie migawek skojarzone z oryginalnego blob docelowego pozostaną niezmienione.

- Podczas tworzenia migawki blob blok listy zablokowanych zatwierdzony obiektów blob zostanie skopiowana również do migawki. Niezatwierdzonym bloków nie są kopiowane.

## <a name="specify-an-access-condition"></a>Określ warunek programu access

Warunek programu access można określić, tak aby tworzenia tylko wtedy, gdy warunek jest spełniony. Aby określić warunek programu access, należy użyć właściwości **AccessCondition** . Jeśli określony warunek nie jest spełniony, nie utworzono migawki i usługa obiektów Blob zwraca kodu stanu HTTPStatusCode.PreconditionFailed.

## <a name="delete-snapshots"></a>Usuwanie migawki

Nie można usunąć obiektów blob z migawki, chyba że usuwane są również migawki. Możesz usunąć migawkę pojedynczo lub określić, że migawki wszystkie usunięte po usunięciu obiektów blob źródła. Przy próbie usuwanie obiektów blob, która nadal ma migawek powoduje to wystąpienie błędu.

W poniższym przykładzie pokazano, jak usunąć obiektów blob i jego migawki w .NET miejsce, w którym `blockBlob` jest zmienną typu **CloudBlockBlob**:

    await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);

## <a name="snapshots-with-azure-premium-storage"></a>Migawki z nośnikami Azure Premium

Za pomocą migawek Obserwuj magazynowania Premium następujących reguł:

- Maksymalna liczba migawek na obiektów blob strony na koncie miejsca do magazynowania premium wynosi 100. Jeśli ten limit zostanie przekroczony, operacja obiektów Blob migawkę zwraca kod błędu 409 (**SnapshotCountExceeded**).

- Czy zrób migawkę blob strony na koncie miejsca do magazynowania premium co 10 minut. Jeśli tego kursu zostanie przekroczony, operacja obiektów Blob migawkę zwraca kod błędu 409 (**SnaphotOperationRateExceeded**).

- Nie można wywołać uzyskiwanie obiektów Blob czytanie migawki obiektów blob strony na koncie miejsca do magazynowania premium. Wywoływanie uzyskiwanie obiektów Blob na migawkę na koncie miejsca do magazynowania premium zwraca kod błędu 400 (**InvalidOperation**). Jednak możesz zadzwonić Uzyskaj właściwości obiektów Blob i uzyskiwanie metadanych obiektów Blob przed migawkę na koncie miejsca do magazynowania premium.

- Aby odczytać migawki, operacja kopiowania obiektów Blob umożliwia kopiowanie migawki do innej strony obiektów blob w oknie konta. Blob miejsca docelowego dla operacji kopiowania nie może mieć dowolne istniejące migawek. Mając obiektów blob docelowego migawek, operacja kopiowania obiektów Blob zwraca kod błędu 409 (**SnapshotsPresent**).

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Przywrócenie bezwzględnego identyfikatora URI migawki

W tym przykładzie kodu C# tworzy migawkę i zapisuje się bezwzględnego identyfikatora URI dla lokalizacji podstawowej.

    //Create the blob service client object.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();

    //Get a reference to a blob.
    CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
    blob.UploadText("This is a blob.");

    //Create a snapshot of the blob and write out its primary URI.
    CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
    Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);

## <a name="understand-how-snapshots-accrue-charges"></a>Opis sposobu migawek naliczania opłat

Tworzenie migawki, czyli kopię obiektów blob tylko do odczytu, może spowodować opłat za przesyłanie danych dodatkowego miejsca do magazynowania do swojego konta. Podczas projektowania aplikacji, jest należy pamiętać sposobu opłaty te mogą naliczania tak, aby zminimalizować niepotrzebnych kosztów.

### <a name="important-billing-considerations"></a>Ważne zagadnienia rozliczeń

Poniższa lista zawiera kluczowe kwestie do rozważenia podczas tworzenia migawki.

- Konto miejsca do magazynowania z opłatami dla bloków unikatowe lub strony, czy są one w obiekcie blob lub migawki. Twoje konto nie spowodować dodatkowe opłaty za migawek skojarzone z obiektów blob, dopóki nie zostanie zaktualizowana obiektów blob, na którym są oparte. Po zaktualizowaniu podstawowej obiektów blob rozbieżność z jego migawek. W takim przypadku są naliczane dla bloków unikatowe lub stron w poszczególnych obiektów blob lub migawki.

- Po zamianie bloku w blob blok grupowych następnie jest naliczany jako blok unikatowych. Dotyczy to nawet wtedy, gdy bloku ma taki sam identyfikator bloku i te same dane ze względu migawki. Po blok zostanie zatwierdzony ponownie go rozbieżność się od swoich odpowiedników w dowolnym migawki i zostanie naliczona dla wraz z danymi. To samo dla strony w blob strony, w którym jest zaktualizowany przy użyciu identyczne dane.

- Zastępowanie blob blok przez wywołanie metody **UploadFile**, **UploadText**, **UploadStream**lub **UploadByteArray** zastępuje wszystkie bloki w obiekcie blob. Jeśli masz migawkę skojarzone z tym obiektów blob wszystkich bloków blob podstawowego i migawki różni się teraz, a zostanie naliczona dla wszystkich bloków w obu obiektów blob. Dotyczy to nawet wtedy, gdy dane w podstawowej obiektów blob i migawki pozostaną identyczne.

- Usługa Azure obiektów Blob nie ma sposób, aby określić, czy dwa bloki zawierać identyczne dane. Każdego bloku, który jest przekazane Projekt zatwierdzony jest traktowany jako unikatowe, nawet jeśli ma te same dane i ten sam identyfikator bloku. Ponieważ naliczania opłat bloków unikatowe, jest należy rozważyć, którego aktualizacje blob, w którym ma migawkę skutkuje dodatkowe blokady unikatowych dodatkowych opłat.

> [AZURE.NOTE] Najważniejsze wskazówki określają, zarządzanie nimi migawek uważnie, aby uniknąć dodatkowych opłat. Zaleca się zarządzanie migawki w następujący sposób:

> - Usuwanie i ponowne tworzenie migawek skojarzone z obiektów blob po zaktualizowaniu obiektów blob, nawet jeśli aktualizujesz identyczny danych, chyba że projektu aplikacji wymaga, aby zachować migawek. Usuwanie i ponowne tworzenie migawek obiektów blob, daje pewność, że obiektów blob i migawek różni się.

> - Jeśli obsługujesz migawek dla obiektów blob Unikaj nawiązywania połączeń z **UploadFile**, **UploadText**, **UploadStream**lub **UploadByteArray** , aby zaktualizować obiektów blob. Jednej z tych metod Zamień wszystko bloków w obiektów blob, tak aby migawek i podstawa obiektów blob usługi różni się znacząco. Należy zaktualizować najmniejszą liczbę bloków przy użyciu metody **PutBlock** i **PutBlockList** .


### <a name="snapshot-billing-scenarios"></a>Migawka rozliczenia scenariuszy


Następujące scenariusze pokazują sposobu naliczania opłat blob blok i jego migawek.

Scenariusz 1 podstawowej obiektów blob nie zostały zaktualizowane po podjęciu migawki, aby opłaty ponoszone są tylko dla bloków unikatowe 1, 2 i 3.

![Azure zasobów magazynowania](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

Scenariusz 2 podstawowej obiektów blob został zaktualizowany, ale migawki nie. Zaktualizowano blok 3, a nawet zawiera te same dane i ten sam identyfikator, nie jest taki sam jak zablokować 3 migawki. W wyniku tego konta jest naliczany cztery bloków.

![Azure zasobów magazynowania](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

Scenariusz 3 podstawowej obiektów blob został zaktualizowany, ale migawki nie. Blok 3 został zastąpiony bloku 4 na podstawie obiektów blob, ale migawki nadal odzwierciedla blok 3. W wyniku tego konta jest naliczany cztery bloków.

![Azure zasobów magazynowania](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

Scenariusz 4 podstawowej obiektów blob została całkowicie zaktualizowana i zawiera żaden z jego oryginalnej bloków. W wyniku tego konta jest naliczany dla wszystkich osiem bloków unikatowe. W tym scenariuszu może wystąpić, jeśli używasz metody update, takich jak **UploadFile**, **UploadText**, **UploadFromStream**lub **UploadByteArray**, ponieważ te metody zastąpić całej zawartości obiektów blob.

![Azure zasobów magazynowania](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkowe przykłady przy użyciu magazyn obiektów Blob zobacz [Przykłady kodu Azure](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Możesz pobrać aplikację próbki i uruchom go lub przeglądanie kodu na GitHub. 
