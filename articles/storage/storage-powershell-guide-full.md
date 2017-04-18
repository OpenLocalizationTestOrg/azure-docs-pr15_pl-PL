<properties
    pageTitle="Przy użyciu programu PowerShell Azure z nośnikami Azure | Microsoft Azure"
    description="Dowiedz się, jak użyć poleceń cmdlet programu PowerShell Azure do przechowywania Azure tworzyć i zarządzać kontami miejsca do magazynowania; Praca z obiektami blob, tabele, kolejkach i plików. Konfigurowanie kwerendy analizy magazynowania i tworzenie podpisów udostępniania."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="using-azure-powershell-with-azure-storage"></a>Przy użyciu programu PowerShell Azure z nośnikami Azure

## <a name="overview"></a>Omówienie

Azure PowerShell to modułu, który zawiera polecenia cmdlet, aby zarządzać Azure za pomocą programu Windows PowerShell. To opartym na zadaniu powłoka wiersza polecenia i języka skryptowego zaprojektowane specjalnie do administrowania systemem. Przy użyciu programu PowerShell można łatwo kontrolować i automatyzowanie administracji usługi Azure usług i aplikacji. Na przykład służy poleceń cmdlet do wykonania tych samych czynności, które można wykonywać za pomocą [Azure Portal](https://portal.azure.com).

W tym przewodniku omówimy się, jak wykonywać różne zadania administracyjne i rozwoju z magazyn Azure za pomocą [Poleceń cmdlet miejsca do magazynowania Azure](https://msdn.microsoft.com/library/azure/mt269418.aspx) .

Ten przewodnik przyjęto założenie, że masz doświadczenia przy użyciu [Programu Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx)i [Magazynowania Azure](https://azure.microsoft.com/documentation/services/storage/) . Przewodnik udostępnia wiele skryptów w celu zademonstrowania zastosowania programu PowerShell z nośnikami Azure. Należy zaktualizować zmienne skrypt na podstawie konfiguracji przed uruchomieniem każdego skryptu.

Pierwsza sekcja w tym przewodniku zawiera krótki przegląd programu PowerShell i magazynowania Azure. Aby uzyskać szczegółowe informacje i instrukcje Rozpoczynanie od [wymagania wstępne dotyczące korzystania z programu PowerShell Azure z nośnikami Azure](#prerequisites-for-using-azure-powershell-with-azure-storage).


## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Wprowadzenie do programu PowerShell i magazynowania Azure w 5 minut

W tej sekcji pokazano, jak uzyskać dostęp do magazynowania Azure za pomocą programu PowerShell za 5 minut.

**Nowe Azure:** Uzyskaj subskrypcji Microsoft Azure i konto Microsoft skojarzone z tej subskrypcji. Aby uzyskać informacje na temat opcji zakupu Azure zobacz [Bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/), [Opcje zakupu](https://azure.microsoft.com/pricing/purchase-options/)i [Oferuje członka](https://azure.microsoft.com/pricing/member-offers/) (w przypadku Członkowie w witrynie MSDN, Microsoft Partner Network i BizSpark oraz innych programów firmy Microsoft).

Aby uzyskać więcej informacji na temat Azure subskrypcji, zobacz [Przypisywanie ról administratora w usłudze Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Po utworzeniu subskrypcji Microsoft Azure i konta:**

1.  Pobierz i zainstaluj [Azure programu PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409).
2.  Start systemu Windows PowerShell zintegrowanego skryptów środowiska (ISE): W komputerze, przejdź do **Start** menu. Wpisz **Narzędzia administracyjne** i kliknij przycisk, aby go uruchomić. W oknie **Narzędzia administracyjne** kliknij prawym przyciskiem myszy **Windows PowerShell ISE**, kliknij polecenie **Uruchom jako Administrator**.
3.  W **Systemie Windows PowerShell ISE**kliknij **plik** > **Nowy** , aby utworzyć nowy plik skryptu.
4.  Teraz przedstawimy prosty skrypt zawierający podstawowe polecenia programu PowerShell, aby uzyskać dostęp do magazynowania Azure. Skrypt najpierw zostanie wyświetlone pytanie, usługi Azure poświadczenia konta, aby dodać usługi Azure konta lokalnego środowiska PowerShell. Następnie skrypt Ustawianie domyślnego Azure subskrypcji i tworzenie nowego konta magazynu platformy Azure. Następnie skrypt utworzyć nowy kontener w tym nowego konta z miejsca do magazynowania i przekazywanie istniejącego pliku obrazu (blob) do kontenera. Po skrypt zawiera listę wszystkich obiektów blob w danym kontenerze, go utworzy nowy katalog docelowy na komputerze lokalnym i pobieranie pliku obrazu.
5.  W poniższej sekcji kodu, wybierz polecenie skrypt między uwagi **#begin** i **#end**. Naciśnij klawisze CTRL + C, aby skopiować go do Schowka.

        #begin
        # Update with the name of your subscription.
        $SubscriptionName = "YourSubscriptionName"

        # Give a name to your new storage account. It must be lowercase!
        $StorageAccountName = "yourstorageaccountname"

        # Choose "West US" as an example.
        $Location = "West US"

        # Give a name to your new container.
        $ContainerName = "imagecontainer"

        # Have an image file and a source directory in your local computer.
        $ImageToUpload = "C:\Images\HelloWorld.png"

        # A destination directory in your local computer.
        $DestinationFolder = "C:\DownloadImages"

        # Add your Azure account to the local PowerShell environment.
        Add-AzureAccount

        # Set a default Azure subscription.
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

        # Create a new storage account.
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location

        # Set a default storage account.
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

        # Create a new container.
        New-AzureStorageContainer -Name $ContainerName -Permission Off

        # Upload a blob into a container.
        Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

        # List all blobs in a container.
        Get-AzureStorageBlob -Container $ContainerName

        # Download blobs from the container:
        # Get a reference to a list of all blobs in a container.
        $blobs = Get-AzureStorageBlob -Container $ContainerName

        # Create the destination directory.
        New-Item -Path $DestinationFolder -ItemType Directory -Force  

        # Download blobs into the local destination directory.
        $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
        #end

5.  W **Systemie Windows PowerShell ISE**naciśnij klawisze CTRL + V, aby skopiować skrypt. Kliknij pozycję **plik** > **Zapisywanie**. W oknie dialogowym **Zapisywanie jako** wpisz nazwę pliku skryptu, takie jak "mystoragescript". Kliknij przycisk **Zapisz**.

6.  Teraz musisz zaktualizować zmienne skrypt, w oparciu o ustawieniach konfiguracji. Zmienna **$SubscriptionName** musisz zaktualizować przy użyciu własnej subskrypcji. Możesz zachować innych czynników zgodnie z skrypt lub je zaktualizować, zgodnie z potrzebami.

    - **$SubscriptionName:** Należy zaktualizować tej zmiennej z nazwą subskrypcji. Wykonaj jedną z następujących trzech sposobów, aby zlokalizować nazwę subskrypcji:

        . W **Systemie Windows PowerShell ISE**kliknij **plik** > **Nowy** , aby utworzyć nowy plik skryptu. Skopiuj poniższy skrypt do nowego pliku skryptu, a następnie kliknij pozycję **Debugowanie** > **Uruchamianie**. Poniższy skrypt najpierw zostanie wyświetlone pytanie, usługi Azure poświadczenia konta, aby dodać usługi Azure konta lokalnego środowiska PowerShell, a następnie Pokaż wszystkie subskrypcje podłączonych do sesji programu PowerShell lokalnej. Zanotuj nazwę subskrypcji, która ma być używany podczas tego samouczka:

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        b. Znajdź i skopiuj nazwę subskrypcji w [Azure Portal](https://portal.azure.com), w menu Centrum po lewej stronie kliknij pozycję **Subskrypcje**. Skopiuj nazwę subskrypcji, która ma być używany podczas uruchamiania skryptów w tym przewodniku.

        ![Azure Portal][Image2]

        c. Aby znaleźć i skopiuj nazwę subskrypcji w [Klasycznym Portal Azure](https://manage.windowsazure.com/), przewiń w dół, a następnie kliknij pozycję **Ustawienia** po lewej stronie portalu. Kliknij pozycję **Subskrypcje** , aby wyświetlić listę subskrypcji. Skopiuj nazwę subskrypcji, która ma być używany podczas uruchamiania skryptów podane w tym przewodniku.

        ![Portal Azure klasyczny][Image1]

    - **$StorageAccountName:** Użyj podanej nazwie skryptu lub wprowadź nową nazwę konta magazynu. **Ważne:** Nazwę konta magazynu musi być unikatowa w Azure. Musi to być małe litery, zbyt!

    - **$Location:** Danego "Zachód USA" za pomocą skryptu lub wybierz inne Azure lokalizacjach, na przykład US Wschodniej, północnej Europie i tak dalej.

    - **$ContainerName:** Użyj podanej nazwie skryptu lub wprowadź nową nazwę dla swojego kontenera.

    - **$ImageToUpload:** Wprowadź ścieżkę do obrazu na komputerze lokalnym, takich jak: "C:\Images\HelloWorld.png".

    - **$DestinationFolder:** Wprowadź ścieżkę do katalogu lokalnego przechowywania pliki pobrane z miejscem do magazynowania Azure, takich jak: "C:\DownloadImages".

7.  Po zaktualizowaniu zmienne skryptów w pliku "mystoragescript.ps1", kliknij pozycję **plik** > **Zapisywanie**. Następnie kliknij przycisk **Debugowanie** > **uruchomić** lub naciśnij klawisz **F5** , aby uruchomić skrypt.  

Po uruchomieniu skrypt powinien mieć folder docelowy lokalne zawierającego plik pobrany obraz. Następujące zrzut ekranu przedstawia przykładowe dane wyjściowe:

![Pobieranie obiektów blob][Image3]


> [AZURE.NOTE] Sekcja "Wprowadzenie do programu PowerShell i magazynowania Azure za 5 minut" udostępniana krótkie wprowadzenie dotyczące korzystania z magazynu Azure Azure programu PowerShell. Aby uzyskać szczegółowe informacje i instrukcje zachęcamy do czytania w poniższych sekcjach.

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Wymagania wstępne dotyczące korzystania z programu PowerShell Azure z magazynu platformy Azure
Potrzebujesz Azure subskrypcji i konta, aby uruchomić polecenia cmdlet programu PowerShell podanych w tym przewodniku, zgodnie z powyższym opisem.

Azure PowerShell to modułu, który zawiera polecenia cmdlet, aby zarządzać Azure za pomocą programu Windows PowerShell. Aby uzyskać informacje o instalowaniu i konfigurowaniu Azure programu PowerShell zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md). Zalecamy, aby pobrać i zainstalować lub uaktualnić do najnowszej modułu programu PowerShell usługi Azure przed rozpoczęciem korzystania z tego przewodnika.

Polecenia cmdlet można uruchamiać w standardowej konsoli programu Windows PowerShell lub Windows PowerShell zintegrowane skrypty środowiska (ISE). Na przykład aby otworzyć program **Windows PowerShell ISE**, przejdź do Start menu, wpisz narzędzia administracyjne i kliknij przycisk, aby go uruchomić. W oknie Narzędzia administracyjne kliknij prawym przyciskiem myszy Windows PowerShell ISE, kliknij polecenie Uruchom jako Administrator.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Jak zarządzać konta magazynu platformy Azure

### <a name="how-to-set-a-default-azure-subscription"></a>Jak skonfigurować domyślne Azure subskrypcji
Aby zarządzać Magazyn Azure przy użyciu programu PowerShell Azure, musisz uwierzytelnienia środowiska klienta z Azure za pomocą uwierzytelniania usługi Azure Active Directory lub na podstawie certyfikatu. Aby uzyskać szczegółowe informacje Zobacz samouczek [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) . Ten przewodnik używane uwierzytelnianie usługi Azure Active Directory.

1.  W systemie Windows PowerShell ISE wpisz następujące polecenie, aby dodać usługi Azure konta lokalnego środowiska PowerShell:

    `Add-AzureAccount`

2.  W oknie "Logowania w celu platformy Microsoft Azure" wpisz adres e-mail i hasło skojarzone z kontem. Azure uwierzytelnia i zapisuje informacje poświadczeń, a następnie zamyka okno.

3.  Następnie uruchom następujące polecenie, aby wyświetlić Azure konta w lokalnym środowisku programu PowerShell i sprawdź, czy Twoje konto znajduje:

    `Get-AzureAccount`

4.  Następnie uruchom następujące polecenie cmdlet, aby wyświetlić wszystkie subskrypcje, połączonych z lokalnym sesji programu PowerShell i sprawdź, czy subskrypcja jest:

    `Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`

5.  Aby ustawić domyślny Azure subskrypcji, uruchom polecenie cmdlet AzureSubscription wybierz pozycję:

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Sprawdź nazwę subskrypcji, domyślny, uruchamiając polecenia cmdlet Get-AzureSubscription:

    `Get-AzureSubscription -Default`

7.  Aby wyświetlić wszystkie dostępne polecenia programu PowerShell cmdlet magazyn Azure, uruchom polecenie:

    `Get-Command -Module Azure -Noun *Storage*`

### <a name="how-to-create-a-new-azure-storage-account"></a>Jak utworzyć nowe konto Azure miejsca do magazynowania
Aby użyć Azure przestrzeni dyskowej, konieczne będzie konto przestrzeni dyskowej. Po skonfigurowaniu na komputerze, aby nawiązać połączenie subskrypcji, możesz utworzyć nowe konto Azure miejsca do magazynowania.

1.  Uruchom polecenie cmdlet Get-AzureLocation, aby znaleźć wszystkie lokalizacje dostępne centrum danych:

    `Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes`

2.  Następnie należy uruchomić polecenia cmdlet AzureStorageAccount nowy, aby utworzyć nowe konto miejsca do magazynowania. Poniższy przykład tworzy nowe konto miejsca do magazynowania w centrum danych "Zachód USA".

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location

> [AZURE.IMPORTANT] Nazwa konta miejsca do magazynowania musi być unikatowa w Azure i musi być mała. Konwencje nazewnictwa i ograniczenia zobacz [Temat miejsca do magazynowania konta usługi Azure](storage-create-storage-account.md) i [nazw i odwoływanie się do kontenerów, obiektów blob i metadanych](http://msdn.microsoft.com/library/azure/dd135715.aspx).

### <a name="how-to-set-a-default-azure-storage-account"></a>Jak ustawić domyślne konto Azure miejsca do magazynowania
Czy masz wiele kont miejsca do magazynowania w ramach subskrypcji. Możesz wybrać jedną z nich i ustaw go jako domyślnego konta miejsca do magazynowania dla wszystkich poleceń miejsca do magazynowania w tej samej sesji programu PowerShell. Dzięki temu będzie można uruchomić polecenia programu PowerShell Azure miejsca do magazynowania bez jawnie Określanie kontekstu miejsca do magazynowania.

1.  Aby ustawić domyślne konto miejsca do magazynowania dla subskrypcji, możesz uruchomić polecenia cmdlet Set-AzureSubscription.

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  Następnie uruchom polecenie cmdlet Get-AzureSubscription, aby sprawdzić, czy konto magazynowania jest skojarzone z domyślnego konta subskrypcji. To polecenie zwraca właściwości subskrypcji na bieżącej subskrypcji, łącznie z jego bieżącym koncie miejsca do magazynowania.

        Get-AzureSubscription –Current

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Jak wyświetlić wszystkie konta Azure miejsca do magazynowania w subskrypcji
Każdej subskrypcji Azure mogą być do 100 konta miejsca do magazynowania. Aby uzyskać najbardziej aktualne informacje dotyczące ograniczenia zobacz [subskrypcji Azure i limity dotyczące usługi, przydziałów i ograniczeń](../azure-subscription-service-limits.md).

Uruchom następujące polecenie cmdlet, aby dowiedzieć się, nazwę i stan kont miejsca do magazynowania w bieżącej subskrypcji:

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### <a name="how-to-create-an-azure-storage-context"></a>Jak utworzyć kontekście Azure miejsca do magazynowania
Kontekst Azure miejsca do magazynowania jest obiektu w programie PowerShell do umieszczać poświadczeń miejsca do magazynowania. Za pomocą kontekstu miejsca do magazynowania, podczas uruchamiania wszystkie kolejne polecenia cmdlet umożliwia uwierzytelniania żądania bez określania konta miejsca do magazynowania i klucza dostępu jawnie. Możesz utworzyć kontekstu miejsca do magazynowania na różne sposoby, takie jak przy użyciu klucza nazwy i dostępu do konta magazynowania, udostępniania token podpisu (SA), parametry połączenia lub anonimowych. Aby uzyskać więcej informacji zobacz [AzureStorageContext nowy](http://msdn.microsoft.com/library/azure/dn806380.aspx).  

Użyj jednej z następujących trzech sposobów utworzyć kontekstu miejsca do magazynowania:

- Uruchom polecenie cmdlet [Get-AzureStorageKey](http://msdn.microsoft.com/library/azure/dn495235.aspx) , aby dowiedzieć się, klucz podstawowy dostępu do konta Azure miejsca do magazynowania. Następnie zadzwonić do polecenia cmdlet [New-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx) utworzyć kontekstu miejsca do magazynowania:

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary


- Generowanie tokenu podpisu udostępnienia kontenera magazynu Azure i za jej pomocą tworzyć kontekstu miejsca do magazynowania:

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    Aby uzyskać więcej informacji zobacz [Nowy AzureStorageContainerSASToken](http://msdn.microsoft.com/library/azure/dn806416.aspx) i [Za pomocą udostępnionego dostępu podpisów (SA)](storage-dotnet-shared-access-signature-part-1.md).

- W niektórych przypadkach można określić punkty końcowe usługi, podczas tworzenia nowego kontekstu miejsca do magazynowania. Może to być konieczne, po zarejestrowaniu nazwy domeny niestandardowej dla konta miejsca do magazynowania z usługą obiektów Blob lub chcesz używać podpisu udostępnienia do uzyskiwania dostępu do zasobów magazynowania. Ustawianie usługi punkty końcowe w parametrach połączenia i za jej pomocą tworzyć nowy kontekst miejsca do magazynowania, tak jak pokazano poniżej:

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString

Aby uzyskać więcej informacji na temat konfigurowania parametrów połączenia miejsca do magazynowania zobacz [Konfigurowanie parametry połączenia](storage-configure-connection-string.md).

Teraz, gdy masz Skonfiguruj komputer i materiału jak zarządzać kont miejsca do magazynowania przy użyciu programu PowerShell Azure i subskrypcje, przejdź do następnej sekcji, aby dowiedzieć się, jak i zarządzanie nimi obiektów blob platformy Azure blob migawek.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Jak pobrać i ponownie wygenerować klucze Azure miejsca do magazynowania

Konta magazynu platformy Azure pochodzi z dwóch klawiszy konta. Następujące polecenie cmdlet umożliwia pobieranie kluczy.

    Get-AzureStorageKey -StorageAccountName "yourstorageaccount"

Użyj następującego polecenia cmdlet, aby pobrać klucza. Prawidłowe wartości to głównego i pomocniczego.  

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary

Jeśli chcesz ponownie wygenerować kluczy, użyj następującego polecenia cmdlet. Prawidłowe wartości - KeyType są "Głównego" i "Pomocniczego"

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Primary”

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Secondary”

## <a name="how-to-manage-azure-blobs"></a>Jak zarządzać obiektami blob platformy Azure
Magazyn obiektów Blob platformy Azure to usługa do przechowywania dużych ilości danych niestrukturalne, takie jak dane tekstowe lub binarne, które są dostępne z dowolnego miejsca na świecie za pośrednictwem protokołu HTTP lub HTTPS. W tej sekcji przyjęto założenie, że znasz już pojęcia usługi Magazyn obiektów Blob platformy Azure. Aby uzyskać szczegółowe informacje zobacz [Rozpoczynanie pracy z magazynem obiektów Blob przy użyciu programu .NET](storage-dotnet-how-to-use-blobs.md) i [Pojęć związanych z obiektów Blob usługi](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-to-create-a-container"></a>Jak utworzyć kontener
Co obiektów blob w magazynie Azure muszą znajdować się w kontenerze. Możesz utworzyć kontener prywatny przy użyciu polecenia cmdlet New-AzureStorageContainer:

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] Istnieją trzy poziomy dostępu anonimowego odczytu: **Wyłączanie** **obiektów Blob**i **kontener**. Aby uniemożliwić dostęp anonimowy do obiektów blob, ustaw parametr uprawnień **off**. Domyślnie nowego kontenera jest prywatna i jest możliwy tylko przez właściciela konta. Aby umożliwić dostęp anonimowy publicznej odczytu blob zasobów, ale nie do kontenera metadanych lub na liście obiektów blob w kontenerze, ustaw parametr uprawnień do **obiektów Blob**. Aby umożliwić publicznej odczyt obiektów blob zasobów, kontener metadanych i na liście obiektów blob w kontenerze, ustaw parametr uprawnień do **kontenera**. Aby uzyskać więcej informacji zobacz [Zarządzanie anonimowe dostęp do odczytu kontenerów i obiektów blob](storage-manage-access-to-resources.md).

### <a name="how-to-upload-a-blob-into-a-container"></a>Jak przekazywać obiektów blob do kontenera
Magazyn obiektów Blob platformy Azure obsługuje blob blok i obiektów blob strony. Aby uzyskać więcej informacji zobacz [opis bloku blob, Dołącz obiektów blob i blob strony](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Aby przekazać BLOB w do kontenera, można użyć polecenia cmdlet [AzureStorageBlobContent zestawu](http://msdn.microsoft.com/library/azure/dn806379.aspx) . Domyślnie tego polecenia spowoduje przekazanie lokalnych plików do blob blok. Aby określić typ obiektu blob, można użyć parametru - BlobType.

W poniższym przykładzie uruchamia polecenia cmdlet [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) , aby pobrać wszystkie pliki w określonym folderze, a następnie przekazuje je do następnego polecenia cmdlet przy użyciu operatora potoku. Polecenie cmdlet [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) przekazywania plików lokalnych do swojego kontenera:

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### <a name="how-to-download-blobs-from-a-container"></a>Jak pobrać obiektów blob z kontenera
Poniższy przykład ilustruje sposób pobierania obiektów blob z kontenera. Przykład najpierw nawiąże połączenie z miejsca do magazynowania Azure za pomocą kontekst konta miejsca do magazynowania, który zawiera nazwę konta magazynu i klucza podstawowego programu access. Następnie w przykładzie pobiera odwołanie obiektów blob przy użyciu polecenia cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) . Następnie w przykładzie użyto polecenia cmdlet [Get-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806418.aspx) Aby pobrać obiektów blob do folderu docelowego lokalnego.

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>Jak skopiować obiektów blob z jednego miejsca do magazynowania kontenera do drugiego
Można kopiować obiektów blob asynchroniczne różnych kont miejsca do magazynowania i regionów. Poniższy przykład pokazuje, jak można kopiować obiektów blob z jednego miejsca do magazynowania kontenera do innej w dwóch magazynu innego konta. Przykład najpierw ustawia zmienne w przypadku kont magazynowania źródłowej i docelowej, a następnie tworzy kontekście miejsca do magazynowania dla każdego konta. Następnie przykład kopiuje obiektów blob z kontenera źródła do kontenera docelowego przy użyciu polecenia cmdlet [Start AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) . W przykładzie założono, że źródłowa i docelowa kont magazynowania i kontenerów już istnieje.

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

Należy zauważyć, że w tym przykładzie jest wykonywane asynchroniczne Kopiuj. Stan każdej kopii można monitorować za pomocą polecenia cmdlet [Get-AzureStorageBlobCopyState](http://msdn.microsoft.com/library/azure/dn806406.aspx) .

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>Jak skopiować obiektów blob z lokalizacji pomocniczej
Możesz skopiować obiektów blob z lokalizacji pomocniczej konta włączono GRS pomoc Zdalna.

    #define secondary storage context using a connection string constructed from secondary endpoints.
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### <a name="how-to-delete-a-blob"></a>Jak usunąć obiektów blob
Aby usunąć obiektów blob, najpierw odwołać obiektów blob, a następnie wywołać polecenia cmdlet AzureStorageBlob Usuń nad nim. W następującym przykładzie wszystkie obiekty BLOB w danym kontenerze. Przykład najpierw ustawia zmienne dla konta miejsca do magazynowania, a następnie tworzy kontekst miejsca do magazynowania. Następnie przykład pobiera odwołanie obiektów blob przy użyciu polecenia cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) i uruchamia polecenia cmdlet [AzureStorageBlob Usuń](http://msdn.microsoft.com/library/azure/dn806399.aspx) , aby usunąć obiektów blob z kontenera w magazynie Azure.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## <a name="how-to-manage-azure-blob-snapshots"></a>Jak zarządzać migawek obiektów blob platformy Azure
Azure umożliwia utworzenie migawki obiektów blob. Migawki jest tylko do odczytu wersji blob, w którym jest przyjmowana w punkcie w czasie. Po utworzeniu migawki go można przeczytać, skopiowane, lub usunięte, ale nie można ich modyfikować. Migawki umożliwiają wykonywanie kopii zapasowej obiektów blob wyświetlaną w momencie w czasie. Aby uzyskać więcej informacji zobacz [Tworzenie migawki obiektów Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-to-create-a-blob-snapshot"></a>Jak utworzyć migawkę obiektów blob
Aby utworzyć snaphot obiektów blob, najpierw odwołać obiektów blob, a następnie zadzwoń `ICloudBlob.CreateSnapshot` metody nad nim. Poniższy przykład najpierw ustawia zmienne dla konta miejsca do magazynowania, a następnie tworzy kontekst miejsca do magazynowania. Następnie przykład pobiera odwołanie obiektów blob przy użyciu polecenia cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) i uruchamia metodę [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) w celu utworzenia migawki.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### <a name="how-to-list-a-blobs-snapshots"></a>Jak listy migawek obiektów blob
Możesz utworzyć dowolną liczbę migawek wymaganą dla obiektów blob. Można wyświetlić listę migawek skojarzony z obiektów blob do śledzenia Twojej bieżącej migawki. W poniższym przykładzie użyto wstępnie zdefiniowanych obiektów blob i połączeń polecenia cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) do wyświetlania listy migawek dany obiekt blob.  

    #Define the blob name.
    $BlobName = "yourblobname"

    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Jak skopiować migawkę obiektów blob
Możesz skopiować migawkę obiektów blob przywrócenie migawki. Aby uzyskać szczegółowe informacje i ograniczeń zobacz [Tworzenie migawki obiektów Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). Poniższy przykład najpierw ustawia zmienne dla konta miejsca do magazynowania, a następnie tworzy kontekst miejsca do magazynowania. Następnie w przykładzie zdefiniowano zmienne nazwę kontenera i obiektów blob. Przykład pobiera odwołanie obiektów blob przy użyciu polecenia cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) i uruchamia metodę [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) w celu utworzenia migawki. Następnie przykład działa polecenia cmdlet [Start AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) skopiować migawkę obiektów blob przy użyciu obiektu ICloudBlob dla obiektów blob źródła. Pamiętaj zaktualizować zmiennych na podstawie konfiguracji przed uruchomieniem w przykładzie. Należy zauważyć, że w poniższym przykładzie założono, że źródła i miejsca docelowego kontenerów i obiektów blob źródło już istnieje.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

Teraz, gdy znasz jak zarządzać obiektami blob Azure i blob migawki przy użyciu programu PowerShell Azure, przejdź do następnej sekcji, aby dowiedzieć się, jak zarządzać tabel, kolejkach i pliki.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Jak zarządzać Azure tabel i obiektów do tabeli
Usługa magazynu w usłudze Azure tabeli jest magazynu danych NoSQL, które służy do przechowywania i kwerendy dużego zestawów danych strukturalnych, -relacyjnych. Główne składniki usługi są tabel, obiektów i właściwości. Tabela jest kolekcję obiektów. Obiekt jest pewien zestaw właściwości. Każdy obiekt może mieć maksymalnie 252 właściwości, które są wszystkich par wartości z pola Nazwa. W tej sekcji przyjęto założenie, że znasz już pojęcia usługi Magazyn tabel platformy Azure. Aby uzyskać szczegółowe informacje zobacz [Opis modelu danych z tabeli](http://msdn.microsoft.com/library/azure/dd179338.aspx) i [rozpocząć pracę z magazynem tabel platformy Azure za pomocą .NET](storage-dotnet-how-to-use-tables.md).

W podane niżej podsekcje dowiesz się, jak zarządzać usługi Magazyn tabel platformy Azure przy użyciu programu PowerShell Azure. Scenariusze, w których obejmują **Tworzenie**, **Usuwanie**i **Pobieranie** **tabel**, a także **dodawania**, **Wyszukiwanie**i **Usuwanie tabeli jednostek**.

### <a name="how-to-create-a-table"></a>Jak utworzyć tabelę
Każda tabela musi znajdować się na koncie Azure miejsca do magazynowania. Poniższy przykład przedstawia sposób tworzenia tabeli w magazynie Azure. Przykład najpierw nawiąże połączenie z miejsca do magazynowania Azure za pomocą kontekst konta miejsca do magazynowania, który zawiera nazwę konta magazynu i jego klawisz dostępu. Następnie używa polecenia cmdlet [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) , aby utworzyć tabelę w magazynie Azure.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-retrieve-a-table"></a>Jak pobrać tabelę
Możesz kwerendy i pobrać jednej lub wszystkich tabel z konta miejsca do magazynowania. Poniższy przykład pokazuje, jak pobrać danej tabeli przy użyciu polecenia cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) .

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

Jeśli dzwonisz polecenia cmdlet Get-AzureStorageTable bez parametrów pobiera wszystkie tabele miejsca do magazynowania dla konta miejsca do magazynowania.

### <a name="how-to-delete-a-table"></a>Usuwanie tabeli
Możesz usunąć tabelę z konta miejsca do magazynowania przy użyciu polecenia cmdlet [AzureStorageTable Usuń](http://msdn.microsoft.com/library/azure/dn806393.aspx) .  

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-manage-table-entities"></a>Jak zarządzać podmioty tabeli
Obecnie Azure programu PowerShell nie umożliwia zarządzanie podmioty tabeli bezpośrednio poleceń cmdlet. Do wykonywania operacji na podmioty tabeli, używając rodzajami podanymi w [Azure Biblioteka klienta miejsca do magazynowania dla środowiska .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx).

#### <a name="how-to-add-table-entities"></a>Dodawanie obiektów do tabeli
Aby dodać obiekt do tabeli, należy najpierw utworzyć obiektu, który definiuje właściwości obiektu. Jednostka może mieć do 255 właściwości, w tym 3 Właściwości systemu: **PartitionKey**, **RowKey**i **Sygnatura czasowa**. Odpowiadają do wstawiania i aktualizowanie wartości **PartitionKey** i **RowKey**. Serwer zarządza wartość **sygnatury czasowej**, którego nie można modyfikować. Współpraca **PartitionKey** i **RowKey** jednoznacznie identyfikować każdy podmiot w tabeli.

-   **PartitionKey**: Określa partycją jednostki są przechowywane w.
-   **RowKey**: jednoznacznie identyfikować podmiotem w partycją.

Można zdefiniować maksymalnie 252 niestandardowe właściwości dla obiektu. Aby uzyskać więcej informacji zobacz [Opis modelu danych z tabeli](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Poniższy przykład pokazuje, jak dodać obiektów do tabeli. W przykładzie pokazano sposób pobrać tabeli pracowników i dodać kilka podmiotów do niej. Najpierw nawiąże połączenie z magazynem Azure za pomocą kontekst konta miejsca do magazynowania, który zawiera nazwę konta magazynu i jego klawisz dostępu. Następnie pobiera danej tabeli przy użyciu polecenia cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . Jeśli tabela nie istnieje, polecenia cmdlet [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) służy do tworzenia tabeli w magazynie Azure. Następnie w przykładzie zdefiniowano funkcji niestandardowej jednostki Dodaj Dodawanie obiektów do tabeli określając partition każdej jednostki i klucz wiersza. Funkcja Dodaj jednostki wywołuje polecenia cmdlet [Nowy obiekt](http://technet.microsoft.com/library/hh849885.aspx) w klasie [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) , aby utworzyć obiekt jednostki. Później przykład wywołuje metodę [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) dla tego obiektu jednostki, aby dodać go do tabeli.

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )

      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)

      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }

    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4

#### <a name="how-to-query-table-entities"></a>Jak wyszukiwać w tabeli obiektów
Aby wykonać kwerendę tabeli, użyj klasy [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) . W poniższym przykładzie założono został już uruchomiony skrypt podany w jak dodać podmioty części tego przewodnika. Przykład najpierw nawiąże połączenie z miejsca do magazynowania Azure za pomocą kontekst miejsca do magazynowania, który zawiera nazwę konta magazynu i jego klawisz dostępu. Następnie próbuje pobrać tabeli "Pracownicy" poprzednio utworzone przy użyciu polecenia cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . Wywoływanie polecenia cmdlet [Nowy obiekt](http://technet.microsoft.com/library/hh849885.aspx) w klasie Microsoft.WindowsAzure.Storage.Table.TableQuery tworzy nowy obiekt kwerendy. Przykład wyszukuje obiektów, które mają kolumny "Identyfikator", których wartość jest równa 1, zgodnie z ustawieniami w filtrze ciągu. Aby uzyskać szczegółowe informacje zobacz [tabele kwerend i jednostki](http://msdn.microsoft.com/library/azure/dd894031.aspx). Po uruchomieniu ta kwerenda zwraca wszystkie obiekty, które spełniają kryteria filtru.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx

    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")

    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20

    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)

    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize

#### <a name="how-to-delete-table-entities"></a>Sposób usuwania jednostek tabeli
Możesz usunąć jednostki przy użyciu klawiszy jego partycją i wiersza. W poniższym przykładzie założono został już uruchomiony skrypt podany w jak dodać podmioty części tego przewodnika. Przykład najpierw nawiąże połączenie z miejsca do magazynowania Azure za pomocą kontekst miejsca do magazynowania, który zawiera nazwę konta magazynu i jego klawisz dostępu. Następnie próbuje pobrać tabeli "Pracownicy" poprzednio utworzone przy użyciu polecenia cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) . Jeśli tabela istnieje, przykład wywołuje metodę [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) do pobierania obiektu na podstawie jego wartości klucza partycją i wiersza. Następnie przekazać jednostki do metody [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) do usunięcia.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
       $entity = $tableResult.Result
    if ($entity -ne $null)
    {
       #Delete the entity.
       $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Jak zarządzać Azure kolejki i wiadomości w kolejce
Magazyn kolejek Azure to usługa do przechowywania dużej liczby wiadomości, które są dostępne z dowolnego miejsca na świecie za pośrednictwem uwierzytelnionego połączenia przy użyciu protokołu HTTP lub HTTPS. W tej sekcji przyjęto założenie, że znasz już pojęcia Usługa magazynu kolejki Azure. Aby uzyskać szczegółowe informacje zobacz [Wprowadzenie do magazynowania kolejki Azure za pomocą .NET](storage-dotnet-how-to-use-queues.md).

W tej sekcji procedurach pokazano, jak zarządzać Usługa magazynu kolejki Azure przy użyciu programu PowerShell Azure. Scenariusze, w których zawierać **wstawiania** i **usuwania** wiadomości kolejki, a także **Tworzenie**, **Usuwanie**i **pobierania kolejek**.

### <a name="how-to-create-a-queue"></a>Jak utworzyć kolejki
Poniższy przykład najpierw nawiąże połączenie z miejsca do magazynowania Azure za pomocą kontekst konta miejsca do magazynowania, który zawiera nazwę konta magazynu i jego klawisz dostępu. Następnie wywołuje [AzureStorageQueue nowy](http://msdn.microsoft.com/library/azure/dn806382.aspx) polecenia cmdlet, aby utworzyć kolejkę o nazwie "NazwaKolejki".

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Uzyskać informacji o konwencje nazewnictwa usługi kolejki Azure zobacz [nazewnictwa kolejek i metadanych](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Jak pobrać kolejki
Możesz kwerendy i pobrać określonej kolejki lub listy wszystkich kolejek na koncie miejsca do magazynowania. Poniższy przykład pokazuje, jak pobrać określonej kolejki przy użyciu polecenia cmdlet [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) .

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

Jeśli zadzwonisz do polecenia cmdlet [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) bez parametrów, otrzymuje listę wszystkich kolejek.

### <a name="how-to-delete-a-queue"></a>Jak usunąć kolejki
Aby usunąć kolejkę i wszystkie zawarte w niej wiadomości, zadzwoń do polecenia cmdlet AzureStorageQueue Usuń. W poniższym przykładzie pokazano, jak usunąć określonej kolejki przy użyciu polecenia cmdlet AzureStorageQueue Usuń.

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### <a name="how-to-insert-a-message-into-a-queue"></a>Jak wstawić wiadomości w kolejce
Aby wstawić wiadomości do istniejącej kolejki, najpierw należy utworzyć nowego wystąpienia klasy [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . Następnie wywołać metodę [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) . CloudQueueMessage można utworzyć w ciągu (w formacie UTF-8) lub tablicy bajtów.

Poniższy przykład pokazuje, jak dodać wiadomość do kolejki. Przykład najpierw nawiąże połączenie z miejsca do magazynowania Azure za pomocą kontekst konta miejsca do magazynowania, który zawiera nazwę konta magazynu i jego klawisz dostępu. Następnie pobiera określonej kolejki przy użyciu polecenia cmdlet [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) . Jeśli istnieje kolejki, polecenia cmdlet [Nowy obiekt](http://technet.microsoft.com/library/hh849885.aspx) służy do tworzenia wystąpienia klasy [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) . Później przykład wywołuje metodę [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) dla tego obiektu wiadomości, aby dodać ją do kolejki. Poniżej przedstawiono kod, który pobiera kolejki i wstawia komunikat "MessageInfo":

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }


#### <a name="how-to-de-queue-at-the-next-message"></a>Jak wyłączyć kolejki w następnej wiadomości
Kod kolejek usuwania wiadomości z kolejki w dwóch krokach. Podczas połączenia metody [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) zostanie wyświetlony następnej wiadomości w kolejce. Komunikat zwrócony przez **GetMessage** staje się niewidoczne dla innych kodu do czytania wiadomości z tej kolejki. Aby zakończyć, usuwając wiadomość z kolejki, możesz również wywołać metodę [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) . Opisanego niżej procesu usuwania wiadomości gwarantuje, że jeśli kodu przetwarzania wiadomości ze względu na błąd sprzętu i oprogramowania nie powiedzie się, inne wystąpienie kodu można wyświetlenie tego samego komunikatu i spróbuj ponownie. Kod wywołuje prawo **DeleteMessage** po przetworzeniu wiadomości.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## <a name="how-to-manage-azure-file-shares-and-files"></a>Jak zarządzać udziały plików Azure i plików
Azure magazyn plików udostępnia udostępnionych miejsca do magazynowania dla aplikacji przy użyciu protokołu SMB standardowy. Środowisku maszyn wirtualnych systemu Microsoft Azure i usług w chmurze udostępnić plik danych przez składniki aplikacji za pomocą akcji zainstalowanego i lokalnego aplikacje mogą uzyskiwać dostęp do danych pliku w udziale za pośrednictwem interfejsu API magazynu pliku lub Azure programu PowerShell.

Aby uzyskać więcej informacji na magazyn plików Azure zobacz [Wprowadzenie do przechowywania plików Azure w systemie Windows](storage-dotnet-how-to-use-files.md) i [Interfejsu API usługi REST usługi pliku](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Jak ustawić i analizy miejsca do magazynowania zapytania
[Azure analizy miejsca do magazynowania](storage-analytics.md) służy do zbierania metryki magazynowania Azure konta i dane o żądania wysyłane na konto miejsca do magazynowania. Za pomocą metryki magazynowania monitorowanie kondycji konto miejsca do magazynowania i miejsca do magazynowania rejestrowanie diagnozowanie i rozwiązywanie problemów za pomocą konta miejsca do magazynowania.
Domyślnie metryki magazynowania nie włączono dla usług miejsca do magazynowania. Możesz włączyć monitorowania przy użyciu Azure Portal lub środowiska Windows PowerShell lub programowo przy użyciu Biblioteka klienta przestrzeni dyskowej. Rejestrowanie miejsca do magazynowania się dzieje po stronie serwera, umożliwiając do rejestrowania szczegółów żądania zarówno pomyślnego i nie powiodło się na Twoim koncie miejsca do magazynowania. Dzienniki te umożliwiają szczegóły czytanie, pisanie i usuwanie operacji przed tabel, kolejek i obiektów blob, a także przyczyny żądania nie powiodło się.

Aby dowiedzieć się, jak włączyć i wyświetlanie metryki magazynowania danych przy użyciu programu PowerShell, zobacz [jak włączyć metryki magazynowania przy użyciu programu PowerShell](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Aby dowiedzieć się, jak włączyć i pobierania rejestrowania miejsca do magazynowania danych przy użyciu programu PowerShell, zobacz [jak włączyć rejestrowanie miejsca do magazynowania przy użyciu programu PowerShell](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) i [Znajdowanie danych dziennika rejestrowania miejsca do magazynowania](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Aby uzyskać szczegółowe informacje na temat korzystania z metryki magazynowania i rejestrowanie miejsca do magazynowania rozwiązywać problemy z miejsca do magazynowania zobacz [Monitorowanie, diagnozowanie i rozwiązywanie problemów z magazynem tabel platformy Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Jak zarządzać udostępnione podpis programu Access (SA) i przechowywane zasady dostępu
Podpisy udostępniania są ważnym elementem model zabezpieczeń dla każdej aplikacji przy użyciu magazynu Azure. Są przydatne w przypadku udostępniania ograniczone uprawnienia do Twojego konta miejsca do magazynowania dla klientów, które nie powinny mieć klucz konta. Tylko właściciel konta miejsca do magazynowania może mają domyślnie dostępu do obiektów blob, tabel i kolejek w obrębie tego konta. Jeśli usłudze lub aplikacji musi udostępnić te zasoby innych klientach bez konieczności udostępniania klucza dostępu, masz trzy opcje:

- Ustawianie uprawnień kontenera umożliwiające anonimowe dostęp do odczytu kontenera i jego obiektów blob. To jest niedozwolone dla tabel lub kolejek.
- Za pomocą podpisu udostępnienia czy dotacje ograniczone uprawnienia do kontenerów, obiektów blob kolejek i tabele dla określonych interwał.
- Zasady przechowywane dostępu umożliwia uzyskanie dodatkowy poziom kontroli nad udostępnienia podpisów dla kontenera lub jego obiektów blob, kolejkę lub tabeli. Zasady dostępu przechowywaną pozwala zmienić czas rozpoczęcia, czas wygaśnięcia lub uprawnień do podpisu lub Aby odwołać go po wydaniu.

Podpis udostępnienia może być w jednym z dwóch formularzy:

- **Spontaniczne skojarzeń zabezpieczeń**: podczas tworzenia spontanicznych skojarzeń zabezpieczeń, czas rozpoczęcia, czas wygaśnięcia, a uprawnienia dla skojarzeń zabezpieczeń są określane URI skojarzeń zabezpieczeń. Ten rodzaj skojarzenia zabezpieczeń mogą zostać utworzone w kontenerze, obiektów blob, tabeli lub kolejki i jest wartością revokable.
- **Skojarzenia zabezpieczeń z zasady dostępu przechowywaną**: zasadę dostępu przechowywane jest zdefiniowana na kontenerze zasobów kontenera obiektów blob, tabeli lub kolejki - i służy do zarządzania ograniczenia dla jednego lub kilku podpisów udostępniania. Skojarzenia zabezpieczeń skojarzyć zasadę dostępu przechowywane, skojarzeń zabezpieczeń dziedziczy ograniczeń - czas rozpoczęcia, czas wygaśnięcia i uprawnienia - zdefiniowane zasady dostępu przechowywane. Ten typ skojarzenia zabezpieczeń jest revokable.

Aby uzyskać więcej informacji zobacz [Za pomocą udostępnionego dostępu podpisów (SA)](storage-dotnet-shared-access-signature-part-1.md) i [Zarządzanie anonimowe dostęp do odczytu kontenerów i obiektów blob](storage-manage-access-to-resources.md).

W następnej sekcji można będzie Dowiedz się, jak utworzyć zasadę dostępu udostępnione podpisu przechowywane i token dostępu dla tabel Azure. Azure programu PowerShell zawiera podobne polecenia cmdlet dla kontenerów, obiektów blob i także kolejek. Aby uruchomić skrypty w tej sekcji, należy pobrać [Azure programu PowerShell wersji 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) lub nowszej.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>Jak utworzyć tokenu udostępnione podpis programu Access na podstawie zasad
Aby utworzyć nową zasadę dostępu przechowywane, należy użyć polecenia cmdlet AzureStorageTableStoredAccessPolicy nowy. Następnie zadzwoń do polecenia cmdlet [AzureStorageTableSASToken nowy](http://msdn.microsoft.com/library/azure/dn806400.aspx) , aby utworzyć nowy token podpisu na podstawie zasad udostępniania dla tabeli magazyn Azure.

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### <a name="how-to-create-an-ad-hoc-non-revokable-shared-access-signature-token"></a>Jak utworzyć token spontanicznych udostępnione podpis programu Access (non-revokable)
Polecenia cmdlet [New AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) umożliwia utworzenie nowego ad hoc (non-revokable) podpis dostępu do udostępnionych tokenu dla tabeli magazyn Azure:

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### <a name="how-to-create-a-stored-access-policy"></a>Jak utworzyć zasady przechowywane programu access
Aby utworzyć nową zasadę dostępu przechowywanych w tabeli magazyn Azure, należy użyć polecenia cmdlet New-AzureStorageTableStoredAccessPolicy:

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### <a name="how-to-update-a-stored-access-policy"></a>Jak zaktualizować zasady przechowywane programu access
Aby zaktualizować istniejące zasady dostępu do przechowywanej tabeli magazyn Azure, należy użyć polecenia cmdlet Set-AzureStorageTableStoredAccessPolicy:

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### <a name="how-to-delete-a-stored-access-policy"></a>Jak usunąć zasadę dostępu przechowywane
Aby usunąć zasadę dostępu przechowywanych w tabeli magazyn Azure, należy użyć polecenia cmdlet Usuń AzureStorageTableStoredAccessPolicy:

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx


## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Jak używać Azure miejsca do magazynowania dla Rządu Stanów Zjednoczonych i Chinach Azure
Azure środowisko jest niezależnych wdrożenia programu Microsoft Azure, takich jak [Azure dla instytucji rządowych dla Rządu Stanów Zjednoczonych](https://azure.microsoft.com/features/gov/), [AzureCloud dla globalnej Azure](https://portal.azure.com)i [AzureChinaCloud dla Azure obsługiwana przez firmę 21Vianet w Chinach](http://www.windowsazure.cn/). Można wdrażać nowego środowiska Azure dla instytucji rządowych USA i Chinach Azure.

Aby użyć magazyn Azure z AzureChinaCloud, należy utworzyć kontekstu miejsca do magazynowania, który jest skojarzony z AzureChinaCloud. Wykonaj poniższe czynności, aby rozpoczęcie pracy:

1.  Uruchom polecenie cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) , aby wyświetlić dostępne środowiskach Azure:

    `Get-AzureEnvironment`

2.  Dodawanie konta Chinach Azure do programu Windows PowerShell:

    `Add-AzureAccount –Environment AzureChinaCloud`

3.  Utworzyć kontekstu miejsca do magazynowania dla konta AzureChinaCloud:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud

Do magazynowania Azure za pomocą [Rząd Azure Stanów Zjednoczonych](https://azure.microsoft.com/features/gov/), należy Definiowanie nowego środowiska, a następnie utworzyć nowy kontekst miejsca do magazynowania z tego środowiska:

1.  Uruchom polecenie cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) , aby wyświetlić dostępne środowiskach Azure:

    `Get-AzureEnvironment`

2.  Dodaj konto Azure nam dla instytucji rządowych do programu Windows PowerShell:

    `Add-AzureAccount –Environment AzureUSGovernment`

3.  Utworzyć kontekstu miejsca do magazynowania dla konta AzureUSGovernment:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment

Aby uzyskać więcej informacji zobacz:

- [Przewodnik dewelopera dla instytucji rządowych platformy Microsoft Azure](../azure-government-developer-guide.md).
- [Omówienie różnic między podczas tworzenia aplikacji w Chinach usługi](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Następne kroki
W tym przewodniku znasz się, jak zarządzać Magazyn Azure przy użyciu programu PowerShell Azure. Poniżej przedstawiono kilka powiązanych artykułów i materiały szkoleniowe więcej informacji na temat ich.

- [Dokumentacja Azure miejsca do magazynowania](https://azure.microsoft.com/documentation/services/storage/)
- [Polecenia cmdlet programu PowerShell Azure miejsca do magazynowania](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [Informacje dotyczące systemu Windows PowerShell](https://msdn.microsoft.com/library/ms714469.aspx)

[Image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png
[Image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png
[Image3]: ./media/storage-powershell-guide-full/Blobdownload.png


[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
