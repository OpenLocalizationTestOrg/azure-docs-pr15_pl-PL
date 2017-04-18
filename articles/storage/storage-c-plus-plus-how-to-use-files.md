<properties
    pageTitle="Jak używać magazyn plików z C++ | Microsoft Azure"
    description="Przechowywanie danych pliku w chmurze z magazyn plików Azure."
    services="storage"
    documentationCenter=".net"
    authors="seguler"
    manager="jahogg"
    editor="tysonn" />

<tags ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="seguler" />

# <a name="how-to-use-file-storage-from-c"></a>Jak używać magazyn plików z C++

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]
[AZURE.INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>Informacje o tym samouczku

W tym samouczku dowiesz się, jak umożliwiają wykonywanie podstawowych operacji w usłudze Microsoft Azure plik miejsca do magazynowania. Za pomocą przykładów w języku C++ dowiesz się, jak utworzyć udział i katalogów, przekazywanie, listy i usuwanie plików. Jeśli jesteś nowym użytkownikiem usługi przechowywania plików Microsoft Azure, przechodzące przez pojęcia w sekcjach będzie zrozumieć próbki bardzo przydatne.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Tworzenie aplikacji C++

Do utworzenia próbki, należy zainstalować Biblioteka klienta miejsca do magazynowania Azure 2.4.0 dla C++. Należy również utworzono konto Azure miejsca do magazynowania.

Aby zainstalować klienta miejsca do magazynowania Azure 2.4.0 C++, użyj jednej z następujących metod:

-   **Linux:** Postępuj zgodnie z instrukcjami na stronie [Biblioteka klienta Azure miejsca do magazynowania dla C++ — Plik README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .

-   **Systemu Windows:** W programie Visual Studio, kliknij przycisk **narzędzia &gt; Menedżera pakietów NuGet &gt; konsoli Menedżera pakietów**. Wpisz następujące polecenie w [konsoli Menedżera pakietów NuGet](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , a następnie naciśnij klawisz **ENTER**.

    Pakiet instalacyjny wastorage

## <a name="set-up-your-application-to-use-file-storage"></a>Konfigurowanie aplikacji do przechowywania plików za pomocą

Dodaj następujące zawiera instrukcje na początek pliku C++, w której chcesz uzyskać dostęp do plików za pomocą Azure magazynowania interfejsy API:

    #include "was/storage_account.h"
    #include "was/file.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Konfigurowanie parametrów połączenia Azure miejsca do magazynowania

Aby użyć przechowywania plików, musisz połączyć się z kontem Azure miejsca do magazynowania. Pierwszym krokiem jest skonfigurować parametry połączenia, które będą używane do łączenia się z kontem miejsca do magazynowania. Załóżmy Zdefiniuj zmienną statyczne to zrobić.

    // Define the connection-string with your values.
    const utility::string_t 
    storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

## <a name="connecting-to-an-azure-storage-account"></a>Łączenie się z kontem Azure miejsca do magazynowania

Za pomocą klasy **cloud_storage_account** reprezentować informacji o koncie miejsca do magazynowania. Aby pobrać informacji o koncie miejsca do magazynowania z parametrów połączenia miejsca do magazynowania, można użyj metody **analizy** .

    // Retrieve storage account from connection string. 
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-share"></a>Jak: utworzyć udział

Wszystkie pliki i katalogi w magazynie pliku znajdują się w kontenerze o nazwie **Udostępnianie**. Konta miejsca do magazynowania może mieć dowolną liczbę akcji, jak pozwala na wydajność konta. Aby uzyskać dostęp do udziału oraz jego zawartość, musisz za pomocą klienta miejsca do magazynowania plików.

    // Create the file storage client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();

Przy użyciu klienta miejsca do magazynowania plików, możesz następnie uzyskać odwołanie do udziału.

    // Get a reference to the file share
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));

Aby utworzyć udział, użyj metody **create_if_not_exists** obiektu **cloud_file_share** .

    if (share.create_if_not_exists()) { 
        std::wcout << U("New share created") << std::endl;  
    }

W tym momencie **Udostępnianie** zawiera odwołanie do udziału o nazwie **Moje Udostępnij próbki**.

## <a name="how-to-upload-a-file"></a>Jak: przekazywanie pliku

Co najmniej udział miejsca do magazynowania plików Azure zawiera katalogu, w którym mogą znajdować się pliki. W tej sekcji dowiesz się, jak przekazywać plików z magazynu lokalnego do katalogu głównego udziału.

Pierwszym krokiem podczas przekazywania pliku jest uzyskanie odwołanie do katalogu, w którym powinien znajdować się. W tym celu nawiązywania połączeń z metody **get_root_directory_reference** obiektu Udostępnij.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();

Teraz, gdy masz odwołanie do katalogu głównego udziału, możesz przekazać plik na go. W tym przykładzie spowoduje przekazanie z pliku, tekstu i strumienia.

    // Upload a file from a stream.
    concurrency::streams::istream input_stream = 
      concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

    azure::storage::cloud_file file1 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
    file1.upload_from_stream(input_stream);

    // Upload some files from text.
    azure::storage::cloud_file file2 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    file2.upload_text(_XPLATSTR("more text"));

    // Upload a file from a file.
    azure::storage::cloud_file file4 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    file4.upload_from_file(_XPLATSTR("DataFile.txt"));  

## <a name="how-to-create-a-directory"></a>Jak: Tworzenie katalogu

Można również uporządkować miejsca do magazynowania, wysyłanie plików wewnątrz podkatalogów zamiast je wszystkie w katalogu głównym. Usługa magazynu pliku Azure umożliwia tworzenie tyle katalogów, aby umożliwić Twojego konta. Poniższy kod spowoduje utworzenie katalogu o nazwie **Mój katalog próbki** w katalogu głównym, a także podkatalogów o nazwie **Moje podkatalogów próbki**.
    
    // Retrieve a reference to a directory
    azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Return value is true if the share did not exist and was successfully created.
    directory.create_if_not_exists();
    
    // Create a subdirectory.
    azure::storage::cloud_file_directory subdirectory = 
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    subdirectory.create_if_not_exists();

## <a name="how-to-list-files-and-directories-in-a-share"></a>Jak: Lista pliki i katalogi w udziale

Uzyskiwanie listy pliki i katalogi w udziale łatwo jest wykonywane przez wywołanie **list_files_and_directories** w odwołaniu **cloud_file_directory** . Aby uzyskać dostęp z bogatego zestawu właściwości i metody zwracane **list_file_and_directory_item**, musisz wywołać metodę **list_file_and_directory_item.as_file** uzyskać obiektu **cloud_file** lub metody **list_file_and_directory_item.as_directory** uzyskać obiektu **cloud_file_directory** .

Poniższy kod ilustruje sposób pobierania i wyjściowy identyfikator URI każdego elementu w katalogu głównym udziału.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    // Output URI of each item.
    azure::storage::list_file_and_diretory_result_iterator end_of_results;
    
    for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
    {
        if(it->is_directory())
        {
            ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
        else if (it->is_file())
        {
            ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
        }       
    }
    

## <a name="how-to-download-a-file"></a>Jak: pobieranie pliku

Aby pobrać pliki, najpierw pobrać odwołanie do pliku, a następnie wywołać metodę **download_to_stream** , aby przenieść zawartość pliku do obiektu strumienia, który można następnie utrzymują się do pliku lokalnego. Za pomocą metody **download_to_file** można także pobrać zawartość pliku do pliku lokalnego. Metoda **download_text** umożliwia pobieranie zawartości pliku w postaci ciągu tekstowego.

W poniższym przykładzie użyto metod **download_to_stream** i **download_text** w celu zademonstrowania pobierania plików, które zostały utworzone w poprzedniej sekcji.

    // Download as text
    azure::storage::cloud_file text_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    utility::string_t text = text_file.download_text();
    ucout << "File Text: " << text << std::endl;
    
    // Download as a stream.
    azure::storage::cloud_file stream_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    stream_file.download_to_stream(output_stream);
    std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();

## <a name="how-to-delete-a-file"></a>Jak: usuwanie pliku

Inny typowych operacji miejsca do magazynowania plików jest usunięcie plików. Poniższy kod usuwa plik o nazwie Moje próbki pliku-3 przechowywany w katalogu głównym.

    // Get a reference to the root directory for the share. 
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    file.delete_file_if_exists();

## <a name="how-to-delete-a-directory"></a>Jak: Usuwanie katalogu

Usunięcie katalogu jest zadaniem proste, jednak należy zauważyć, że nie można usunąć katalogu zawierającego nadal plików ani innych katalogów.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // Get a reference to the directory.
    azure::storage::cloud_file_directory directory = 
      share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Get a reference to the subdirectory you want to delete.
    azure::storage::cloud_file_directory sub_directory =
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    
    // Delete the subdirectory and the sample directory.
    sub_directory.delete_directory_if_exists();
    
    directory.delete_directory_if_exists();

## <a name="how-to-delete-a-share"></a>Jak: usunąć udział

Usuwanie udziału jest wykonywane przez wywołującego metodę **delete_if_exists** w obiekcie cloud_file_share. Poniżej przedstawiono przykładowy kod, który oznacza, że.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // delete the share if exists
    share.delete_share_if_exists();

## <a name="set-the-maximum-size-for-a-file-share"></a>Ustaw maksymalny rozmiar pliku w udziale plików

Przydział (lub maksymalny rozmiar) można ustawić dla udziału plików w gigabajtów. Możesz również sprawdzić aby zobaczyć, ile danych przechowywanych w udziale.

Ustawiając przydziału dla udziału, można ograniczyć całkowity rozmiar plików przechowywanych w udziale. Jeśli całkowity rozmiar plików w udziale Ustaw w udziale, następnie klienci będą nie można zwiększyć rozmiar istniejących plików lub tworzenie nowych plików, chyba że te pliki są puste.

W poniższym przykładzie pokazano, jak sprawdzić bieżące użycie udział oraz ustawiania przydziału dla Udostępnij.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    if (share.exists())
    {
        std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();
    
        //This line sets the quota to 2560GB
        share.resize(2560);
    
        std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();
    
    }

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Generowanie podpisu udostępnienia pliku lub udziale plików

Istnieje możliwość wygenerowania podpisu udostępniania (SA) dla pliku w udziale plików lub osobne pliki. Można także tworzyć zasady udostępniania w udziale plików do zarządzania udostępnienia podpisów. Tworzenie zasady dostępu udostępnione jest zalecana, ponieważ zapewnia środka odwoływanie skojarzeń zabezpieczeń, jeśli powinna zostać złamane.

Poniższy przykład tworzy zasadę dostępu udostępnione w udziale, a następnie używa tych zasad w celu włączenia ograniczeń dla skojarzeń zabezpieczeń dla pliku w udziale.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client and get a reference to the share
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    if (share.exists())
    {
        // Create and assign a policy
        utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

        azure::storage::file_shared_access_policy sharedPolicy = 
          azure::storage::file_shared_access_policy();

        //set permissions to expire in 90 minutes
        sharedPolicy.set_expiry(utility::datetime::utc_now() + 
          utility::datetime::from_minutes(90));

        //give read and write permissions
        sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

        //set permissions for the share
        azure::storage::file_share_permissions permissions; 

        //retrieve the current list of shared access policies
        azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;
        
        //add the new shared policy
        policies.insert(std::make_pair(policy_name, sharedPolicy));

        //save the updated policy list
        permissions.set_policies(policies);
        share.upload_permissions(permissions);

        //Retrieve the root directory and file references
        azure::storage::cloud_file_directory root_dir = 
          share.get_root_directory_reference();
        azure::storage::cloud_file file = 
          root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

        // Generate a SAS for a file in the share 
        //  and associate this access policy with it.       
        utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);
        
        // Create a new CloudFile object from the SAS, and write some text to the file.     
        azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
        utility::string_t text = _XPLATSTR("My sample content");        
        file_with_sas.upload_text(text);        
        
        //Download and print URL with SAS.
        utility::string_t downloaded_text = file_with_sas.download_text();      
        ucout << downloaded_text << std::endl;      
        ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;
    
    }

Aby uzyskać więcej informacji na temat tworzenia i używania podpisów udostępniania zobacz [Przy użyciu podpisów dostępu do udostępnionej (SA)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat magazyn Azure, zapoznaj się z następujących zasobów:

-   [Biblioteka klienta miejsca do magazynowania dla C++](https://github.com/Azure/azure-storage-cpp)

-   [Eksplorator magazynu platformy Azure](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)

-   [Dokumentacja Azure miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/)
