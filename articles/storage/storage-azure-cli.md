<properties
    pageTitle="Polecenie Azure za pomocą Azure magazynowania | Microsoft Azure"
    description="Dowiedz się, jak za pomocą interfejsu wiersza polecenia Azure (polecenie Azure) z nośnikami Azure do tworzenia i zarządzać kontami miejsca do magazynowania i Praca z obiektami blob Azure i pliki. Polecenie Azure to narzędzie między platformami "
    services="storage"
    documentationCenter="na"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="using-the-azure-cli-with-azure-storage"></a>Polecenie Azure za pomocą Azure miejsca do magazynowania

## <a name="overview"></a>Omówienie

Polecenie Azure udostępnia zestaw Otwórz źródło między platformami polecenia dotyczące pracy z platformy Azure. Udostępnia wiele funkcji znaleziony w [Azure portal](https://portal.azure.com) , a także funkcji dostępu do danych sformatowanych.

W tym przewodniku omówimy się, jak wykonywać różne zadania administracyjne i rozwoju z magazyn Azure za pomocą [interfejsu wiersza polecenia Azure (polecenie Azure)](../xplat-cli-install.md) . Zalecamy, aby pobrać i zainstalować lub uaktualnić do najnowszej polecenie Azure przed rozpoczęciem korzystania z tego przewodnika.

Ten przewodnik założono, aby zrozumieć podstawowe pojęcia magazyn Azure. Przewodnik udostępnia wiele skryptów w celu zademonstrowania zastosowania polecenie Azure z nośnikami Azure. Pamiętaj zaktualizować zmienne skrypt na podstawie konfiguracji przed uruchomieniem każdego skryptu.

> [AZURE.NOTE] Przewodnik zawiera przykłady polecenie i skrypt polecenie Azure w przypadku kont klasyczny miejsca do magazynowania. Zobacz [za pomocą interfejsu wiersza polecenia Azure dla komputerów Mac, Linux i systemu Windows z zarządzania zasobami Azure](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) w celu polecenie Azure polecenia w przypadku kont miejsca do magazynowania Menedżera zasobów.

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Wprowadzenie do przechowywania Azure i polecenie Azure za 5 minut

Ten przewodnik używa Ubuntu przykłady, ale inne platformy systemu operacyjnego należy wykonać w podobny sposób.

**Nowe Azure:** Uzyskaj subskrypcji Microsoft Azure i konto Microsoft skojarzone z tej subskrypcji. Aby uzyskać informacje na temat opcji zakupu Azure zobacz [Bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/), [Opcje zakupu](https://azure.microsoft.com/pricing/purchase-options/)i [Oferuje członka](https://azure.microsoft.com/pricing/member-offers/) (w przypadku Członkowie w witrynie MSDN, Microsoft Partner Network i BizSpark oraz innych programów firmy Microsoft).

Aby uzyskać więcej informacji na temat Azure subskrypcji, zobacz [Przypisywanie ról administratora w usłudze Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Po utworzeniu subskrypcji Microsoft Azure i konta:**

1. Pobierz i zainstaluj polecenie Azure, postępując zgodnie z instrukcjami przedstawionych w [instalacji polecenie Azure](../xplat-cli-install.md).
2. Po zainstalowaniu polecenie Azure można uzyskać dostęp do poleceń Azure interfejsu wiersza polecenia przy użyciu polecenia azure z interfejsu użytkownika wiersza polecenia (imprezie, końcowych, wiersz polecenia). Typ `azure` polecenia i powinny być widoczne następujące wyniki.

    ![Dane wyjściowe polecenia Azure][Image1]

3. W interfejsie wiersza polecenia wpisz `azure storage` listy wszystkich poleceń azure miejsca do magazynowania i pobieranie pierwszej wrażenie funkcji polecenie Azure udostępnia. Możesz wpisać nazwę polecenia z parametrem **-h** (na przykład `azure storage share create -h`) aby wyświetlić szczegółowe informacje o składni polecenia.
4. Teraz przedstawimy prosty skrypt zawierający podstawowe polecenia polecenie Azure uzyskiwanie dostępu do magazynu Azure. Najpierw skrypt poprosi ustawić dwie zmienne dla konta miejsca do magazynowania i klucza. Następnie skrypt utworzyć nowy kontener w tym nowego konta z miejsca do magazynowania i przekazywanie istniejącego pliku obrazu (blob) do kontenera. Po skrypt zawiera listę wszystkich obiektów blob w danym kontenerze, pobierze plik obrazu do katalogu docelowego, który znajduje się na komputerze lokalnym.

        #!/bin/bash
        # A simple Azure storage example

        export AZURE_STORAGE_ACCOUNT=<storage_account_name>
        export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

        export container_name=<container_name>
        export blob_name=<blob_name>
        export image_to_upload=<image_to_upload>
        export destination_folder=<destination_folder>

        echo "Creating the container..."
        azure storage container create $container_name

        echo "Uploading the image..."
        azure storage blob upload $image_to_upload $container_name $blob_name

        echo "Listing the blobs..."
        azure storage blob list $container_name

        echo "Downloading the image..."
        azure storage blob download $container_name $blob_name $destination_folder

        echo "Done"

5. Na komputerze lokalnym Otwórz Edytor tekstu preferowany (vim, na przykład). Wpisz powyższy skrypt do edytora tekstu.

6. Teraz musisz zaktualizować zmienne skrypt, w oparciu o ustawieniach konfiguracji.

    - **< storage_account_name >** Użyj podanej nazwie skryptu lub wprowadź nową nazwę konta magazynu. **Ważne:** Nazwę konta magazynu musi być unikatowa w Azure. Musi to być małe litery, zbyt!

    - **< storage_account_key >** Klawisz dostępu konta miejsca do magazynowania.

    - **< container_name >** Użyj podanej nazwie skryptu lub wprowadź nową nazwę dla swojego kontenera.

    - **< image_to_upload >** Wprowadź ścieżkę do obrazu na komputerze lokalnym, takich jak: "~ / images/HelloWorld.png".

    - **< destination_folder >** Wprowadź ścieżkę do katalogu lokalnego przechowywania pliki pobrane z miejscem do magazynowania Azure, takich jak: "~/downloadImages".

7. Po zaktualizowaniu niezbędne zmienne vim Naciskaj kombinacje klawiszy "Esc,:, wq!" Aby zapisać skrypt.

8. Aby uruchomić ten skrypt, po prostu wpisz nazwę pliku skryptu w konsoli imprezie. Po uruchomieniu tego skryptu powinien mieć folder docelowy lokalne zawierającego plik pobrany obraz. Następujące zrzut ekranu przedstawia przykładowe dane wyjściowe:

Po uruchomieniu skrypt powinien mieć folder docelowy lokalne zawierającego plik pobrany obraz.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Zarządzanie kontami miejsca do magazynowania z polecenie Azure

### <a name="connect-to-your-azure-subscription"></a>Nawiązywanie połączenia z subskrypcji usługi Azure

Gdy większość poleceń miejsca do magazynowania będzie pracować bez subskrypcji usługi Azure, zalecamy nawiązywanie połączenia z subskrypcji z polecenie Azure. Aby skonfigurować polecenie Azure do pracy z subskrypcji, wykonaj czynności opisane w [Nawiązywanie połączenia z Azure subskrypcję polecenie Azure](../xplat-cli-connect.md).

### <a name="create-a-new-storage-account"></a>Utwórz nowe konto miejsca do magazynowania

Aby użyć Azure miejsca do magazynowania, konieczne będzie konto miejsca do magazynowania. Po skonfigurowaniu komputerowi nawiązywanie połączenia z subskrypcji, możesz utworzyć nowe konto Azure miejsca do magazynowania.

        azure storage account create <account_name>

Nazwę konta magazynu musi należeć do przedziału od 3 do 24 znaków i używanie tylko małe litery i cyfry.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Ustawianie domyślnego konta Azure miejsca do magazynowania w środowisku zmiennych

Masz wiele kont miejsca do magazynowania w ramach subskrypcji. Możesz wybrać jedną z nich i ustawić ją zmienne środowiska dla wszystkich poleceń miejsca do magazynowania w tej samej sesji. Pozwala na uruchomienie polecenia magazynu polecenie Azure bez określania konta miejsca do magazynowania i kluczowe jawnie.

        export AZURE_STORAGE_ACCOUNT=<account_name>
        export AZURE_STORAGE_ACCESS_KEY=<key>

Ustawianie domyślnego konta miejsca do magazynowania w inny sposób korzysta z parametrów połączenia. Po pierwsze pobieranie parametrów połączenia przy użyciu polecenia:

        azure storage account connectionstring show <account_name>

Następnie skopiuj parametry połączenia danych wyjściowych i ustaw zmienną środowiska:

        export AZURE_STORAGE_CONNECTION_STRING=<connection_string>

## <a name="create-and-manage-blobs"></a>Tworzenie i zarządzanie obiektami blob

Magazyn obiektów Blob platformy Azure to usługa do przechowywania dużych ilości danych niestrukturalne, takie jak dane tekstowe lub binarne, które są dostępne z dowolnego miejsca na świecie za pośrednictwem protokołu HTTP lub HTTPS. W tej sekcji przyjęto założenie, że znasz już pojęcia magazyn obiektów Blob platformy Azure. Aby uzyskać szczegółowe informacje zobacz [Obiektów Blob usługi pojęcia](http://msdn.microsoft.com/library/azure/dd179376.aspx)i [rozpocząć pracę z magazynem obiektów Blob platformy Azure za pomocą .NET](storage-dotnet-how-to-use-blobs.md) .

### <a name="create-a-container"></a>Tworzenie kontenera

Co obiektów blob w magazynie Azure muszą znajdować się w kontenerze. Możesz utworzyć kontener prywatny za pomocą `azure storage container create` polecenie:

        azure storage container create mycontainer

> [AZURE.NOTE] Istnieją trzy poziomy dostępu anonimowego odczytu: **Wyłączanie** **obiektów Blob**i **kontener**. Aby uniemożliwić dostęp anonimowy do obiektów blob, ustaw parametr uprawnień, aby **wyłączyć**. Domyślnie nowego kontenera jest prywatna i jest możliwy tylko przez właściciela konta. Aby umożliwić dostęp anonimowy publicznej odczytu blob zasobów, ale nie do kontenera metadanych lub na liście obiektów blob w kontenerze, ustaw parametr uprawnień do **obiektów Blob**. Aby umożliwić publicznej odczyt obiektów blob zasobów, kontener metadanych i na liście obiektów blob w kontenerze, ustaw parametr uprawnień do **kontenera**. Aby uzyskać więcej informacji zobacz [Zarządzanie anonimowe dostęp do odczytu kontenerów i obiektów blob](storage-manage-access-to-resources.md).

### <a name="upload-a-blob-into-a-container"></a>Przekazywanie obiektów blob do kontenera

Magazyn obiektów Blob platformy Azure obsługuje blob blok i obiektów blob strony. Aby uzyskać więcej informacji zobacz [opis bloku blob, Dołącz obiektów blob i blob strony](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Aby przekazać BLOB w do kontenera, można użyć `azure storage blob upload`. Domyślnie tego polecenia spowoduje przekazanie lokalnych plików do blob blok. Aby określić typ obiektu blob, możesz użyć `--blobtype` parametru.

        azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob

### <a name="download-blobs-from-a-container"></a>Pobieranie obiektów blob z kontenera

Poniższy przykład ilustruje sposób pobierania obiektów blob z kontenera.

        azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'

### <a name="copy-blobs"></a>Kopiowanie obiektów blob

Możesz skopiować obiektów blob w jednym lub wielu kont miejsca do magazynowania i regiony asynchroniczne.

Poniższy przykład pokazuje, jak można kopiować obiektów blob z jednego miejsca do magazynowania konta do innej. W tym przykładzie tworzymy kontenera publicznie, których obiektów blob anonimowo dostępne.

    azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

    azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

    azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer

W tym przykładzie wykonuje asynchroniczne Kopiuj. Stan każdej operacji kopiowania można monitorować za pomocą `azure storage blob copy show` operacji.

Należy zauważyć, że adres URL źródła przewidziane operacji kopiowania musi być publicznie dostępne albo zawierać token skojarzeń zabezpieczeń (udostępnienia podpis).

### <a name="delete-a-blob"></a>Usuwanie obiektów blob

Aby usunąć obiektów blob, użyj poniżej polecenia:

        azure storage blob delete mycontainer myBlockBlob2

## <a name="create-and-manage-file-shares"></a>Tworzenie i zarządzanie nimi udziały plików

Azure magazyn plików udostępnia udostępnionych miejsca do magazynowania dla aplikacji przy użyciu protokołu SMB standardowy. Środowisku maszyn wirtualnych systemu Microsoft Azure i usług w chmurze, a także aplikacji lokalnej udostępnić plik danych za pomocą akcji zainstalowany. Możesz zarządzać udziały plików i plików danych za pośrednictwem interfejsu wiersza polecenia Azure. Aby uzyskać więcej informacji na magazyn plików Azure zobacz [Wprowadzenie do przechowywania plików Azure w systemie Windows](storage-dotnet-how-to-use-files.md) lub [sposobu używania magazyn plików Azure z systemem Linux](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Tworzenie pliku w udziale plików

Udostępnij plik Azure jest udziale plików SMB platformy Azure. Wszystkie pliki i katalogi muszą zostać utworzone w udziale plików. Konta może zawierać dowolną liczbę akcji, a udział mogą zawierać dowolną liczbę plików, w granicach możliwości konta miejsca do magazynowania. Poniższy przykład tworzy udziału plików o nazwie **moj_udzial**.

        azure storage share create myshare

### <a name="create-a-directory"></a>Tworzenie katalogu

Katalog przewiduje opcjonalne strukturę hierarchiczną udziale plików Azure. Poniższy przykład tworzy katalogu o nazwie **myDir** w udziale plików.

        azure storage directory create myshare myDir

Uwaga Ta ścieżka katalogu może zawierać wiele poziomów, *np.* **-b**. Należy jednak upewnij się, że istnieje wszystkich katalogach nadrzędnych. Na przykład dla ścieżki **-b**, należy utworzyć katalogu **** najpierw, a następnie utworzyć katalogu **b**.

### <a name="upload-a-local-file-to-directory"></a>Przekaż plik lokalny do katalogu

Poniższy przykład przekazywanie pliku z **~/temp/samplefile.txt** do katalogu **myDir** . Edytować ścieżkę pliku, tak aby wskazywała prawidłowy plik na komputerze lokalnym:

        azure storage file upload '~/temp/samplefile.txt' myshare myDir

Należy zauważyć, że pliku w udziale mogą być w rozmiarze do 1 TB.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Lista plików w katalogu lub udostępnianie główny

Można wyświetlić listę plików i podkatalogów w katalogu głównego udziału lub katalogu przy użyciu następującego polecenia:

        azure storage file list myshare myDir

Zauważ, że nazwa katalogu jest opcjonalne dla operacji pozycję na liście. Jeśli pominięty, to polecenie wyświetla zawartość katalogu głównego udziału.

### <a name="copy-files"></a>Kopiowanie plików

Począwszy od wersji 0.9.8 polecenie Azure, można kopiować pliku do innego pliku, pliku do obiektów blob lub obiektów blob do pliku. Poniżej pokazano możemy sposób do wykonania tych operacji kopiowania za pomocą poleceń interfejsu wiersza polecenia. Aby skopiować pliku do nowego katalogu:

    azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

Aby skopiować obiektów blob do katalogu plików:

    azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

## <a name="next-steps"></a>Następne kroki

Poniżej przedstawiono kilka powiązanych artykułów i materiały szkoleniowe więcej informacji na temat przechowywania Azure.

- [Dokumentacja Azure miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/)
- [Odwołanie interfejsu API pozostałych Azure miejsca do magazynowania](https://msdn.microsoft.com/library/azure/dd179355.aspx)


[Image1]: ./media/storage-azure-cli/azure_command.png
