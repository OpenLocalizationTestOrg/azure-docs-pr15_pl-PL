<properties
    pageTitle="Jak używać magazyn obiektów Blob platformy Azure z systemem iOS | Microsoft Azure"
    description="Przechowywanie danych niestrukturalne w chmurze z magazynem obiektów Blob platformy Azure (miejsca przechowywania obiektu)."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-ios"></a>Jak używać magazyn obiektów Blob z systemem iOS

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie

W tym artykule pojawi się, jak wykonywać typowe scenariusze przy użyciu magazyn obiektów Blob platformy Azure firmy Microsoft. Próbki są zapisywane w celu C i używania [Biblioteki klienta Azure miejsca do magazynowania dla systemu iOS](https://github.com/Azure/azure-storage-ios). Scenariusze, w których zawierać **przekazywanie**, **Wyświetlanie**, **Pobieranie**i **Usuwanie** obiektów blob. Aby uzyskać więcej informacji dotyczących obiektów blob zobacz sekcję [Następne kroki](#next-steps) . Możesz również pobrać [aplikacji przykładowych](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) , aby szybko wyświetlić wykorzystanie miejsca do magazynowania Azure w aplikacji systemu iOS.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Importowanie biblioteki iOS magazyn Azure do aplikacji

Biblioteka iOS magazyn Azure można zaimportować do aplikacji przy użyciu [CocoaPod miejsca do magazynowania Azure](https://cocoapods.org/pods/AZSClient) lub importując plik **Framework** .

## <a name="cocoapod"></a>CocoaPod

1. Jeśli nie zrobiono tego wcześniej [CocoaPods zainstalować](https://guides.cocoapods.org/using/getting-started.html#toc_3) na komputerze, otwierając okno końcowych i uruchamiając następujące polecenie

        sudo gem install cocoapods

2. Następny w katalogu projektu (zawierające katalogu usługi `.xcodeproj` pliku), utworzenie nowego pliku o nazwie `Podfile`(bez rozszerzenia pliku). Dodaj poniższe czynności, aby `Podfile` i zapisywanie

        pod 'AZSClient'

3. W oknie końcowych przejdź do katalogu projektu, a następnie uruchom następujące polecenie

        pod install

4. Jeśli do `.xcodeproj` jest otwarty w Xcode, zamknij go. W katalogu projektu otwórz plik nowo utworzonego projektu, który ma `.xcworkspace` rozszerzenia. To jest plik, który będzie pracujesz z dla teraz na.

## <a name="framework"></a>Struktura
Aby użyć biblioteki iOS magazyn Azure, zostanie najpierw należy utworzyć plik framework.

1. Najpierw pobierz lub klonowanie [repo azure-miejsca do magazynowania — ios](https://github.com/azure/azure-storage-ios).

2. Przejdź do *azure-miejsca do magazynowania — ios* -> *Biblioteka* -> *Biblioteka klienta miejsca do magazynowania Azure*i Otwórz `AZSClient.xcodeproj` w Xcode.

3. W lewym górnym rogu Xcode Zmień schemat active "Biblioteka klienta miejsca do magazynowania Azure" do "Struktury".

4. Tworzenie projektu (klawisze ⌘ + B). Spowoduje to utworzenie `AZSClient.framework` plik na pulpicie.

Można następnie zaimportuj plik framework do aplikacji, wykonując następujące czynności:

1. Tworzenie nowego projektu lub Otwórz istniejący projektu w Xcode.

2. Kliknij nad projektem w nawigacji po lewej stronie, a następnie kliknij kartę *Ogólne* w górnej części edytora projektu.

3. W sekcji *połączonych struktury i biblioteki* kliknij przycisk Dodaj (+).

4. Kliknij przycisk *Dodaj inne...*. Przejdź do i dodawanie `AZSClient.framework` właśnie utworzony plik.

5. W sekcji *połączonych struktury i bibliotek* ponownie kliknij przycisk Dodaj (+).

6. Na liście bibliotek już podany wyszukiwanie `libxml2.2.dylib` i dodać je do projektu.

7. Kliknij kartę *Tworzenie ustawienia* w górnej części edytora projektu.

8. W sekcji *Ścieżki wyszukiwania* kliknij dwukrotnie *Framework ścieżki wyszukiwania* , a następnie dodaj ścieżkę do swojego `AZSClient.framework` pliku.

## <a name="import-statement"></a>Importowanie poufności informacji
Należy uwzględnić następujące instrukcji import w pliku, w której chcesz wywołania interfejsu API magazynu Azure.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Operacje asynchroniczne
> [AZURE.NOTE] Operacje asynchroniczne są wszystkie metody, które wykonują żądanie usługi. W przykładach kodu znajdziesz, że te metody obsługi zakończenia. **Po** zakończeniu żądanie, spowoduje uruchomienie kodu wewnątrz obsługi zakończenia. Kod po obsługi zakończenia spowoduje uruchomienie **podczas** żądania jest dokonywana.

## <a name="create-a-container"></a>Tworzenie kontenera
Co obiektów blob w magazynie Azure musi znajdować się w kontenerze. W poniższym przykładzie pokazano, jak utworzyć kontener o nazwie *newcontainer*na Twoim koncie miejsca do magazynowania, jeśli jeszcze nie istnieje. Wybierając nazwę dla swojego kontenera, należy zachować ostrożność reguł nazewnictwa wymienionych powyżej.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

Aby potwierdzić, że to polega na przeglądająca [Eksplorator magazynu usługi Microsoft Azure](http://storageexplorer.com) i zweryfikowania, że tej *newcontainer* znajduje się na liście kontenerów dla Twojego konta miejsca do magazynowania.

## <a name="set-container-permissions"></a>Ustawianie uprawnień do kontenera
Domyślnie dostępu **prywatne** są konfigurowane uprawnień kontenera. Kontenery zapewnia jednak kilka różnych opcji dostępu kontenera:

- **Prywatne**: kontener i obiektów blob danych mogą być odczytywane przez tylko właściciel konta.

- **Obiektów blob**: obiektów Blob danych w tym kontenerze można odczytywać przy użyciu anonimowego żądania, ale kontenera dane nie są dostępne. Klienci nie można wyliczyć obiektów blob w kontenerze przy użyciu anonimowego żądania.

- **Kontener**: kontener i obiektów blob danych można odczytywać przy użyciu anonimowego żądania. Klientów można wyliczyć obiektów blob w kontenerze przy użyciu anonimowego żądania, ale nie można wyliczyć kontenerów w ramach konta miejsca do magazynowania.

W poniższym przykładzie pokazano, jak utworzyć kontener z uprawnieniami dostępu **kontenera** , umożliwiającym dostęp publicznej, tylko do odczytu dla wszystkich użytkowników w Internecie:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektów blob do kontenera
Jak wskazano w sekcji [obiektów Blob usługi pojęcia](#blob-service-concepts) , magazyn obiektów Blob oferuje trzy różne typy obiektów blob: blokowanie obiektów blob, Dołącz obiektów blob i strony obiektów blob. W tym momencie biblioteki iOS magazyn Azure obsługuje tylko blob blok. W większości przypadków obiektów blob bloku jest zalecane typ ma być używany.

W poniższym przykładzie pokazano, jak przekazać obiektów blob blok z NSString. Jeśli obiektów blob o takiej samej nazwie już istnieje w tym kontenerze, zawartość tego obiektów blob zostaną zastąpione.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

Aby potwierdzić, że to polega na [Eksplorator magazynu usługi Microsoft Azure](http://storageexplorer.com) i sprawdzanie, czy kontenerze *containerpublic*zawiera obiektów blob, *sampleblob*. W tym przykładzie użyliśmy publicznej kontenera, można też sprawdzić, czy to zadziałała, przechodząc do obiektów blob identyfikatora URI:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Oprócz przekazywania obiektów blob blok z NSString podobnych metod istnieje NSData, NSInputStream lub plik lokalny.

## <a name="list-the-blobs-in-a-container"></a>Lista obiektów blob w kontenerze
W poniższym przykładzie pokazano sposób wyświetlić listę wszystkich obiektów blob w kontenerze. Po tej operacji należy zachować ostrożność, z następujących parametrów:     

- **continuationToken** — token kontynuacji reprezentuje początku operacja pozycję na liście. Jeśli żaden token jest dostępne, spowoduje wyświetlenie listy obiektów BLOB od początku. Dowolna liczba obiektów blob można podawać od 0 do zestawu maksymalna. Nawet jeśli ta metoda zwraca wyników, jeśli `results.continuationToken` nie wynosi zero, może istnieć więcej obiektów blob usługi, które nie zostały wymienione.
- **Prefiks** - można określić prefiks lista obiektów blob. Zostaną wyświetlone tylko obiektów blob, które zaczynają się od tego prefiksu.
- **useFlatBlobListing** - wymieniony w sekcji [nazw i odwołujący się kontenerów i obiektów blob](#naming-and-referencing-containers-and-blobs) , mimo że usługa obiektów Blob jest schemat prostym miejsca do magazynowania, można utworzyć hierarchię wirtualnych według nazw obiektów blob z informacjami o ścieżce. Jednak nie płaskie lista nie jest obecnie obsługiwana; jest to już wkrótce. Teraz ta wartość powinna być`YES`
- **blobListingDetails** — możesz określić elementy, które mają być uwzględniane podczas pozycje obiektów blob
    - `AZSBlobListingDetailsNone`: Listy tylko zatwierdzony blob, a nie zwracają metadanych obiektów blob.
    - `AZSBlobListingDetailsSnapshots`: Lista Projekt zatwierdzony obiektów blob i migawek obiektów blob.
    - `AZSBlobListingDetailsMetadata`: Pobieranie obiektów blob metadanych dla poszczególnych obiektów blob zwracana wartość na liście.
    - `AZSBlobListingDetailsUncommittedBlobs`: Listy gwarantowanej i niezatwierdzonym obiektów blob.
    - `AZSBlobListingDetailsCopy`: Zawierać Kopiuj właściwości na liście.
    - `AZSBlobListingDetailsAll`: Listy wszystkich dostępnych zatwierdzony obiektów blob, niezatwierdzonym obiektów blob i migawki, a wszystkie metadanych i Kopiuj stan zwrotu dla tych obiektów blob.
- **maxResults** - maksymalna liczba wyników do zwrócenia dla tej operacji. Nie ustawiono limit za pomocą -1.
- **completionHandler** - blok kodu do wykonania z wyniki operacji pozycję na liście.

W tym przykładzie jest używany do metody Pomocnik blob lokalizacji połączenie listy metody zawsze zwraca token kontynuacji.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## <a name="download-a-blob"></a>Pobieranie obiektów blob

W poniższym przykładzie pokazano, jak pobrać obiektów blob do obiektu NSString.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## <a name="delete-a-blob"></a>Usuwanie obiektów blob

W poniższym przykładzie pokazano, jak usunąć obiektów blob.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## <a name="delete-a-blob-container"></a>Usuwanie kontenera obiektów blob

W poniższym przykładzie pokazano, jak usunąć kontenera.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz już sposobu używania magazyn obiektów Blob z systemem iOS, wykonaj te łącza, aby dowiedzieć się więcej o bibliotece iOS i Usługa magazynu.

- [Azure Biblioteka klienta miejsca do magazynowania dla systemu iOS](https://github.com/azure/azure-storage-ios)
- [Azure iOS przechowywania dokumentacji](http://azure.github.io/azure-storage-ios/)
- [Usługi Azure magazyn interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Blog zespołu Azure miejsca do magazynowania](http://blogs.msdn.com/b/windowsazurestorage)

Jeśli masz pytania dotyczące tej biblioteki zachęcamy publikować wiadomości w naszym [forum w witrynie MSDN Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) lub [Przepełnienie stosu](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Jeśli masz funkcję sugestie dotyczące miejsca do magazynowania Azure, Opublikuj [Opinię miejsca do magazynowania Azure](https://feedback.azure.com/forums/217298-storage/).
