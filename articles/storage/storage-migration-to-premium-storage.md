<properties
    pageTitle="Migrowanie do magazynowania Azure Premium | Microsoft Azure"
    description="Przeprowadź migrację istniejącej maszyn wirtualnych do magazyn Premium Azure. Magazyn Premium oferuje dysku wysokiej wydajności i małych opóźnień obsługę obciążenia I-O-intensywną uruchomione na maszyn wirtualnych Azure."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="yuemlu"/>


# <a name="migrating-to-azure-premium-storage"></a>Migrowanie do magazynowania Azure Premium

## <a name="overview"></a>Omówienie

Azure magazynowania Premium udostępnia obsługę dysku wysokiej wydajności i małych opóźnień maszyn wirtualnych z systemem obciążenia I-O-intensywną. Zaletą szybkości i wydajności tych dysków może potrwać przez migrowanie aplikacji maszyn wirtualnych dysków do przechowywania Premium Azure.

Cel ten przewodnik jest pomoc nowych użytkowników Azure Premium przestrzeni dyskowej lepiej przygotować nawiązać płynnego przejścia z bieżącego systemu do magazynu Premium. Przewodnik dotyczy trzy główne składniki tego procesu: 

  - [Planowanie migracji do magazynu Premium](#plan-the-migration-to-premium-storage)
  - [Przygotowywanie i skopiuj wirtualnych dyskach twardych (VHD) do miejsca do magazynowania Premium](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
  - [Tworzenie maszyn wirtualnych Azure przy użyciu magazynu Premium](#create-azure-virtual-machine-using-premium-storage)

Możesz Migrowanie maszyn wirtualnych na innych platformach do magazynu Premium Azure lub Migrowanie dnia istniejących Azure maszyn wirtualnych ze standardowego magazynu do magazynu Premium. Ten przewodnik obejmuje instrukcje dotyczące obu dwa scenariusze. Postępuj zgodnie z instrukcjami w odpowiedniej sekcji w zależności od aktualnego scenariusza.

>[AZURE.NOTE] Omówienie funkcji i ceny Premium miejsca w magazynie Premium można znaleźć: [Wysokiej wydajności miejsca do magazynowania dla obciążenia maszyn wirtualnych Azure](storage-premium-storage.md). Zaleca się migrowanie dysku maszyn wirtualnych wymagających wysokiej operacji i/o na SEKUNDĘ do magazynu Premium Azure celu uzyskania najlepszej wydajności aplikacji. Jeśli dysk nie wymaga operacji i/o na SEKUNDĘ wysoki, można ograniczyć koszty przy zachowaniu go w magazynie standardowy, w którym są przechowywane dane dysku maszyn wirtualnych na dysków twardych (dysków twardych) zamiast twarde SSD.

Kończenie procesu migracji w całości może wymagać dodatkowe akcje przed i po kroki opisane w tym przewodniku. Jako przykład można wymienić konfigurowania wirtualnych sieci lub punktów końcowych i zmian kod samej aplikacji wymagające niektórych przestoje w aplikacji. Te akcje są unikatowe dla każdej aplikacji, a następnie należy wykonać je wraz z kroki opisane w tym przewodniku do pełnego przechodzenia do magazynu Premium Bezproblemowa w jak najszerszym zakresie.


## <a name="plan-the-migration-to-premium-storage"></a>Planowanie migracji do magazynu Premium

W tej sekcji zapewnia, że są gotowe do wykonania tych kroków migracji, w tym artykule i ułatwia podejmowania decyzji zalecane dla typów maszyn wirtualnych i dysk.

### <a name="prerequisites"></a>Wymagania wstępne
- Konieczne będzie Azure subskrypcji. Jeśli nie masz, można utworzyć subskrypcji miesięcznego [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/) lub odwiedź stronę [Ceny Azure](https://azure.microsoft.com/pricing/) , aby uzyskać więcej opcji.
- Aby wykonać polecenia cmdlet programu PowerShell, konieczne będzie modułu programu PowerShell usługi Microsoft Azure. Zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) dla instrukcji instalacji punktu i instalacji.
- Jeśli planujesz używać maszyny wirtualne Azure uruchomionych Premium miejsca do magazynowania, należy użyć maszyny wirtualne do magazynowania Premium. Za pomocą dysków zarówno standardowe, jak i miejsca do magazynowania Premium z maszyny wirtualne do magazynowania Premium. Dysków w magazynie Premium będzie dostępny więcej typów maszyn wirtualnych w przyszłości. Aby uzyskać więcej informacji na wszystkich dostępnych typów dysku maszyn wirtualnych Azure i rozmiarów zobacz [rozmiarów maszyn wirtualnych](../virtual-machines/virtual-machines-windows-sizes.md) i [rozmiarów usług w chmurze](../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Zagadnienia dotyczące

Maszyn wirtualnych Azure obsługuje dołączanie kilka dysków magazynu Premium, dzięki czemu aplikacji może mieć maksymalnie 64 TB miejsca na maszyn wirtualnych. Z miejscem do magazynowania Premium aplikacji można uzyskać 80 000 operacji i/o na SEKUNDĘ (wejścia i wyjścia operacji na sekundę) na maszyn wirtualnych i 2000 MB na drugim przepustowość dysków na maszyn wirtualnych z bardzo niskie opóźnienia operacji odczytu. Dostępne opcje w różnych maszyny wirtualne i dyski. Ta sekcja jest ułatwiające znajdowanie opcję, która najlepiej pasuje do obciążenie pracą.

#### <a name="vm-sizes"></a>Rozmiary maszyn wirtualnych
Specyfikacje rozmiar maszyn wirtualnych Azure są widoczne w [rozmiarów maszyn wirtualnych](../virtual-machines/virtual-machines-windows-sizes.md). Przeglądanie parametrów maszyn wirtualnych, które Praca z magazynu Premium i wybierz odpowiedni rozmiar maszyn wirtualnych, który najlepiej pasuje do obciążenie pracą. Upewnij się, przepustowości wystarczającej dostępne jest w Twojej maszyn wirtualnych do kierowania ruchu dysku.


#### <a name="disk-sizes"></a>Rozmiary dysku
Istnieją trzy typy dysków, których można używać z Twojej maszyn wirtualnych i każda ma określonych operacji i/o na sekundę i przepustowość ograniczenia. Wziąć pod uwagę następujące ograniczenia po Wybieranie typu dysku dla Twojej maszyn wirtualnych oparte na potrzeby aplikacji pod względem zdolności, wydajność i skalowalność i ładowania Szczyt.

|Typ dysku magazynu Premium|P 10|P20|P30|
|:---:|:---:|:---:|:---:|
|Rozmiar dysku|128 GB|512 GB|1024 GB (1 TB)|
|Operacji i/o na SEKUNDĘ na dysku|500|2300|5000|
|Przepustowość na dysku|100 MB na sekundę|150 MB na sekundę|200 MB na sekundę|

W zależności od usługi Obciążenie pracą należy sprawdzić, czy dyski dodatkowe dane są niezbędne do swojego maszyn wirtualnych. Kilka dysków trwałych danych można dołączać do swojego maszyn wirtualnych. W razie potrzeby można paskowych na dyskach, aby zwiększyć wydajność i wydajności głośność. (Zobacz Co to jest rozkładanie [tutaj](storage-premium-storage-performance.md#disk-striping).) Jeśli paskowych magazynowania Premium dyski danych przy użyciu [Spacji miejsca do magazynowania][4], należy skonfigurować ją z jednej kolumny dla każdego dysku, który jest używany. W przeciwnym razie ogólnej wydajności wielkość rozłożone może być mniejsza niż oczekiwane ze względu na rozkład normalny ruchu na dyskach. Za pomocą narzędzia *mdadm* do osiągnięcia tak samo, dla maszyny wirtualne Linux. Zobacz artykuł [Konfigurowanie RAID oprogramowania w systemie Linux](../virtual-machines/virtual-machines-linux-configure-raid.md) , aby uzyskać szczegółowe informacje.

#### <a name="storage-account-scalability-targets"></a>Cele skalowalność konta miejsca do magazynowania
W przypadku kont miejsca do magazynowania Premium są następujących docelowych skalowalność oprócz [skalowalność miejsca do magazynowania Azure i cele](storage-scalability-targets.md). Jeśli z wymaganiami aplikacji przekracza celów skalowalność konta jednego miejsca do magazynowania, tworzenie aplikacji używać wielu kont miejsca do magazynowania i dzielenia danych na rachunkach miejsca do magazynowania.

|Wydajność konta sum|Przepustowości dla konta lokalnie zbędne miejsca do magazynowania|
|:--|:---|
|Wydajność dysku: 35TB<br />Migawka pojemności: 10 TB|Do 50 GB na sekundę przychodzące + wychodzące|

Aby uzyskać więcej informacji dotyczących specyfikacji magazynowania Premium zapoznaj się z [skalowalność i cele wydajności podczas korzystania z miejsca do magazynowania Premium](storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

#### <a name="disk-caching-policy"></a>Zasady buforowania na dysku
Domyślnie dyskowej pamięci podręcznej zasad jest *Tylko do odczytu* dla wszystkich danych Premium dysków i *Odczytu i zapisu* dysku system operacyjny Premium dołączone do maszyn wirtualnych. To ustawienie konfiguracji zaleca się uzyskanie optymalnej wydajności dla systemu IOs aplikacji. W przypadku dysków dużej zapisu lub tylko do zapisu danych (na przykład plików dziennika programu SQL Server) wyłączenie buforowania dysku, tak aby można uzyskać lepszą wydajność aplikacji. Ustawienia pamięci podręcznej dla istniejących dyski danych mogą być aktualizowane przy użyciu parametru *- HostCaching* polecenia cmdlet *Set-AzureDataDisk* lub [Azure Portal](https://portal.azure.com) .

#### <a name="location"></a>Lokalizacja
Wybierz lokalizację, w której znajduje się magazyn Premium Azure. Aby uzyskać aktualne informacje dotyczące dostępnych lokalizacji, zobacz [Usługi Azure według regionów](https://azure.microsoft.com/regions/#services) . Maszyny wirtualne znajdujące się w tym samym regionie jako konto miejsca do magazynowania, że sklepy dysków w poszukiwaniu maszyn wirtualnych nadaje dużo lepszą wydajność niż jeśli znajdują się w oddzielnym regionów.

#### <a name="other-azure-vm-configuration-settings"></a>Inne ustawienia konfiguracji maszyn wirtualnych Azure
Podczas tworzenia maszyn wirtualnych Azure, zostanie wyświetlony monit, aby skonfigurować pewne ustawienia maszyn wirtualnych. Należy pamiętać, że w kilku ustawień rozwiązane dla ważności Głosowa, gdy można modyfikować lub dodać inne później. Przejrzyj te ustawienia konfiguracji maszyn wirtualnych Azure i upewnij się, że te są odpowiednio skonfigurowane zgodnie z wymaganiami obciążenie pracą.

### <a name="optimization"></a>Optymalizacja

[Magazyn Premium azure: Projektowanie dla wysokiej wydajności](storage-premium-storage-performance.md) zawiera wskazówki dotyczące tworzenia aplikacji wysokiej wydajności przy użyciu magazynu Premium Azure. Możesz wykonać wskazówki połączone z wydajnością najważniejsze wskazówki dotyczące technologii używanych przez aplikację.


## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Przygotowywanie i skopiuj wirtualnych dyskach twardych (VHD) do miejsca do magazynowania Premium

Następująca sekcja zawiera wskazówki dotyczące przygotowywania wirtualnych dysków twardych z Twojej maszyn wirtualnych i kopiowania wirtualnych dysków twardych do magazynu Azure.

- [Scenariusz 1: "I jest Migrowanie istniejącej Azure maszyny wirtualne Azure magazynem Premium."](#scenario1)
- [Scenariusz 2: "I jestem migracji maszyny wirtualne na innych platformach Azure magazynem Premium."](#scenario2)

### <a name="prerequisites"></a>Wymagania wstępne

Aby przygotować wirtualnych dysków twardych migracji, musisz:

- Subskrypcję usługi Azure, konto miejsca do magazynowania i kontenera tego konta miejsca do magazynowania, do którego zostały skopiowane do wirtualnego dysku twardego. Należy zauważyć, że konta docelowego miejsca do magazynowania mogą być standardowy lub magazynowania Premium konto w zależności od z wymaganiami.
- Narzędzie uogólniać wirtualny dysk twardy, jeśli zamierzasz utworzyć wielu wystąpień maszyn wirtualnych z niego. Na przykład sysprep dla systemu Windows lub narzędzia sysprep virt dla Ubuntu.
- Narzędzie przekazać plik wirtualnego dysku twardego na koncie przestrzeni dyskowej. Zobacz [Przenoszenie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md) lub za pomocą [Eksploratora magazynu Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). W tym przewodniku opisano kopiowania do wirtualnego dysku twardego przy użyciu narzędzia AzCopy.

> [AZURE.NOTE] Po wybraniu opcji Kopiuj obraz z AzCopy, aby uzyskać optymalną wydajność, skopiuj do wirtualnego dysku twardego, uruchamiając jednego z tych narzędzi z maszyn wirtualnych Azure, który znajduje się w tym samym regionie jako konto docelowego miejsca do magazynowania. Jeśli kopiujesz wirtualnego dysku twardego maszyn wirtualnych Azure w innym regionie, wydajność może być mniejsza.
>
> Do kopiowania dużych ilości danych na ograniczonej przepustowości, należy rozważyć, czy [przy użyciu usługi Azure Importuj/Eksportuj do przesyłania danych z magazynem obiektów Blob](storage-import-export-service.md); pozwala na transfer danych za pomocą wysyłki dysków twardych Azure centrum danych. Za pomocą usługi Azure Importuj/Eksportuj do skopiowania danych na koncie standardowego magazynu. Po zaimportowaniu danych na swoim koncie standardowego magazynu, umożliwia [Kopiowanie obiektów Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) lub AzCopy przesyłanie danych do konta premium miejsca do magazynowania.
>
> Należy zauważyć, że Microsoft Azure obsługuje tylko stały rozmiar plików wirtualnego dysku twardego. Pliki VHDX lub dynamiczne wirtualnych dysków twardych nie są obsługiwane. Jeśli masz dynamiczne wirtualny dysk twardy, można ją przekształcić stały rozmiar przy użyciu polecenia cmdlet [Wirtualnego dysku twardego Konwertuj](http://technet.microsoft.com/library/hh848454.aspx) .

### <a name="scenario1"></a>Scenariusz 1: "I jest Migrowanie istniejącej Azure maszyny wirtualne Azure magazynem Premium."

Podczas migrowania istniejących Azure maszyny wirtualne zatrzymać maszyn wirtualnych, przygotowywanie wirtualnych dysków twardych według typu ma wirtualnego dysku twardego, a następnie skopiuj wirtualny dysk twardy z AzCopy lub programu PowerShell.

Maszyn wirtualnych musi być całkowicie dół, aby przeprowadzić migrację stanu czystego. Będzie przestoje dopiero po zakończeniu migracji.

#### <a name="step-1-prepare-vhds-for-migration"></a>Krok 1. Przygotowywanie wirtualnych dysków twardych migracji

Jeśli istniejący Azure maszyny wirtualne są migrowane do magazynu Premium, do wirtualnego dysku twardego mogą być:

- Obraz uogólniony systemu operacyjnego
- Dysk unikatowe system operacyjny
- Dysk danych

Poniżej przeprowadzimy przez te 3 scenariuszy przygotowań do wirtualnego dysku twardego.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Tworzenie wielu wystąpień maszyn wirtualnych przy użyciu uniwersalny wirtualnego dysku twardego System operacyjny

Jeśli przekazujesz wirtualny dysk twardy, którego będzie można używać do tworzenia wielu wystąpień maszyn wirtualnych Azure ogólne, należy najpierw generalize wirtualnego dysku twardego przy użyciu narzędzia sysprep. Dotyczy to wirtualny dysk twardy z lokalną lub w chmurze. Sysprep usuwa wszystkie informacje dotyczące komputera z wirtualnego dysku twardego.

>[AZURE.IMPORTANT] Zrób migawkę lub kopia zapasowa z maszyn wirtualnych przed uogólnianie go. Bieżąca sysprep Zatrzymaj, a następnie cofnąć wystąpienie maszyn wirtualnych. Wykonaj kroki wymienione poniżej do sysprep wirtualnego dysku twardego systemu operacyjnego Windows. Należy zauważyć, że uruchamianie polecenia konieczne będzie zamknięcie maszyny wirtualnej. Aby uzyskać więcej informacji na temat narzędzia Sysprep Zobacz [Omówienie Sysprep](http://technet.microsoft.com/library/hh825209.aspx) lub [Techniczne dotyczące narzędzia Sysprep](http://technet.microsoft.com/library/cc766049.aspx).

1. Otwórz okno wiersza polecenia jako administrator.
2. Wpisz następujące polecenie, aby otworzyć Sysprep:

        %windir%\system32\sysprep\sysprep.exe

3. W narzędzie przygotowywania systemu, wybierz środowisko w nowym polu wprowadź System (pierwszego uruchomienia), zaznacz pole wyboru Generalize, wybierz pozycję **Zamknij**, a następnie kliknij przycisk **OK**, jak pokazano na poniższej ilustracji. Sysprep będzie system operacyjny a wyłączony systemu.

    ![][1]

Dla Głosowa Ubuntu przy użyciu narzędzia sysprep virt takie same. Zobacz [virt sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) , aby uzyskać więcej informacji. Zobacz też część Otwórz źródło [oprogramowania rozbudowy serwerów Linux oraz](http://www.cyberciti.biz/tips/server-provisioning-software.html) dla innych systemów operacyjnych Linux.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Umożliwia tworzenie jednego wystąpienia maszyn wirtualnych unikatowe wirtualnego dysku twardego System operacyjny

Jeśli masz uruchamiania aplikacji na maszyn wirtualnych, która wymaga komputera określonych danych nie generalize wirtualnego dysku twardego. Wirtualnego dysku twardego — uogólniony można utworzyć unikatowe wystąpienia maszyn wirtualnych Azure. Na przykład jeśli masz kontrolera domeny w swojej wirtualnego dysku twardego wykonywanie sysprep spowoduje to nieskuteczne jako kontrolera domeny. Przejrzyj aplikacje na swojej maszyn wirtualnych i wpływu na nich uruchomiona sysprep przed uogólnianie wirtualnego dysku twardego.

##### <a name="register-data-disk-vhd"></a>Zarejestruj się w danych dysku wirtualnego dysku twardego

Jeśli masz dyski danych platformy Azure do migracji, należy się upewnić, że maszyny wirtualne, które używają tych dysków danych są zamknięte.

Wykonaj kroki opisane poniżej, aby skopiować wirtualnego dysku twardego do magazyn Premium Azure i zarejestrować ją jako dyskiem ustanawianie danych.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Krok 2. Tworzenie miejsca docelowego dla swojego wirtualnego dysku twardego

Utwórz konto miejsca do magazynowania obsługę usługi wirtualnych dysków twardych. Podczas planowania miejsca przechowywania usługi wirtualnych dysków twardych, rozważ następujące wskazówki:

- Wartość docelowa nominalnej miejsca do magazynowania.
- Miejsce przechowywania konta musi być taki sam jak magazyn Premium stanie Azure maszyny wirtualne ma zostać utworzony w ostatnim etapie. Może skopiować do nowego konta miejsca do magazynowania lub zamierzasz używać tego samego konta miejsca do magazynowania, w zależności od potrzeb.
- Skopiuj i Zapisz klucz konta magazynowania konta docelowego miejsca do magazynowania do następnego etapu.

W przypadku dysków danych możesz zachować niektóre dyski danych z konta standardowego magazynu (na przykład dysków, które mają zapewnia miejsca do magazynowania), ale Stanowczo zalecamy przenoszenie wszystkie dane dla produkcji obciążenie pracą korzystania z magazynu premium.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Krok 3. Skopiuj wirtualny dysk twardy z AzCopy lub programu PowerShell

Będzie trzeba znaleźć kontenera ścieżkę i miejsca do magazynowania klucz konta podczas jednej z tych dwóch opcji. Kontener ścieżkę i miejsca do magazynowania klucz konta można znaleźć w **Azure Portal** > **miejsca do magazynowania**. Adres URL kontenera będą takie jak "https://myaccount.blob.core.windows.net/mycontainer/".

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Opcja 1: Skopiuj wirtualny dysk twardy z AzCopy (asynchroniczne kopia)

Przy użyciu AzCopy, możesz łatwo przekazać wirtualnego dysku twardego przez Internet. W zależności od rozmiaru wirtualnych dysków twardych to może trochę potrwać. Pamiętaj sprawdzić limitu ingress i wyjściowego konta przy użyciu tej opcji. Aby uzyskać szczegółowe informacje, zobacz [skalowalność miejsca do magazynowania Azure i cele](storage-scalability-targets.md) .

1. Pobieranie i instalowanie AzCopy tutaj: [najnowszą wersję programu AzCopy](http://aka.ms/downloadazcopy)
2. Otwórz Azure programu PowerShell i przejdź do folderu, w którym zainstalowano AzCopy.
3. Użyj następującego polecenia, aby skopiować plik wirtualnego dysku twardego z "Źródłowy" do "Docelowy".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Przykład:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd

    Poniżej przedstawiono opisy parametry używane w tym poleceniu AzCopy:

 - * */Źródła: * &lt;źródła&gt;:*** lokalizację folderu lub adres URL kontenera miejsca do magazynowania, który zawiera wirtualnego dysku twardego.
 - * */SourceKey: * &lt;źródła konta klucza&gt;:*** klucz konta miejsca do magazynowania konta magazynu źródłowego.
 - * */Dest: * &lt;miejsce docelowe&gt;:*** adres URL kontenera magazynu do wirtualnego dysku twardego do skopiowania.
 - * */DestKey: * &lt;dest konta klucza&gt;:*** klucz konta miejsca do magazynowania konta docelowego miejsca do magazynowania.
 - * */Deseniu: * &lt;nazwy pliku&gt;:*** Określ nazwę pliku wirtualnego dysku twardego do skopiowania.

Aby uzyskać szczegółowe informacje na temat korzystania z narzędzia AzCopy zobacz [Przenoszenie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Opcja 2: Kopiowanie wirtualnego dysku twardego z programem PowerShell (Synchronized kopia)

Możesz również skopiować plik wirtualnego dysku twardego przy użyciu polecenia cmdlet programu PowerShell Start AzureStorageBlobCopy. Użyj następującego polecenia na Azure programu PowerShell, aby skopiować wirtualnego dysku twardego. Zamień wartości w <> odpowiadających im wartości z Twojego konta miejsca do magazynowania źródłowej i docelowej. Aby użyć tego polecenia, musisz mieć kontenera o nazwie wirtualnych dysków twardych na koncie docelowego miejsca do magazynowania. Jeśli kontener nie istnieje, utwórz ją przed uruchomieniem polecenia.

    $sourceBlobUri = <source-vhd-uri>

    $sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

    $destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

    Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext

Przykład:

    C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

    C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

    C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

    C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext

### <a name="scenario2"></a>Scenariusz 2: "I jestem migracji maszyny wirtualne na innych platformach Azure magazynem Premium."

Jeśli wirtualny dysk twardy są migrowane z wartością - Azure magazynu w chmurze Azure, należy najpierw wyeksportować wirtualny dysk twardy do katalogu lokalnego. Masz źródła pełną ścieżkę katalogu lokalnego, w której jest przechowywany przydatnego wirtualnego dysku twardego i przekaż go do magazynowania Azure za pomocą AzCopy.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Krok 1. Eksportowanie wirtualnego dysku twardego do katalogu lokalnego

##### <a name="copy-a-vhd-from-aws"></a>Skopiuj wirtualny dysk twardy z AWS

1. Jeśli używasz AWS eksportowanie wystąpienie EC2 do wirtualnego dysku twardego w kolorem Amazon S3. Wykonaj kroki opisane w dokumentacji Amazon eksportowania wystąpienia EC2 Amazon zainstalować narzędzie interfejs wiersza polecenia (polecenie) Amazon EC2 i uruchom polecenie Eksportuj zadania, Utwórz wystąpienie w-, aby wyeksportować wystąpienie EC2 do pliku wirtualnego dysku twardego. Pamiętaj użyć **wirtualnego dysku twardego** dysku & #95; obraz & #95; Zmienna FORMAT po uruchomieniu polecenia **Utwórz wystąpienie — eksportowanie — zadanie** . Wyeksportowany plik wirtualny dysk twardy jest zapisywany w kolorem Amazon S3, które wyznacza się podczas wykonywania tej czynności.

        aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
        --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX

2. Pobierz plik wirtualnego dysku twardego z kolorem S3. Wybierz plik wirtualny dysk twardy, następnie **Akcje** > **pobierania**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Skopiuj wirtualny dysk twardy z innych chmurze innych niż Azure

Jeśli wirtualny dysk twardy są migrowane z wartością - Azure magazynu w chmurze Azure, należy najpierw wyeksportować wirtualny dysk twardy do katalogu lokalnego. Kopiowanie pełną źródła ścieżkę katalogu lokalnego przechowywania wirtualnego dysku twardego.

##### <a name="copy-a-vhd-from-on-premise"></a>Skopiuj wirtualny dysk twardy z lokalnego

Jeśli wirtualny dysk twardy są migrowane z lokalnego środowiska, konieczne będzie ścieżka źródłowa pełną przechowywania wirtualnego dysku twardego. Ścieżka źródłowa może być serwera lokalizacji lub w udziale plików.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Krok 2. Tworzenie miejsca docelowego dla swojego wirtualnego dysku twardego

Utwórz konto magazynowania obsługę usługi wirtualnych dysków twardych. Podczas planowania miejsca przechowywania usługi wirtualnych dysków twardych, rozważ następujące wskazówki:

- Konta docelowego miejsca do magazynowania może być standardowy lub specjalnego miejsca do magazynowania w zależności od Twojej wymagania aplikacji.
- Region konta miejsca do magazynowania musi być taki sam jak magazyn Premium stanie Azure maszyny wirtualne ma zostać utworzony w ostatnim etapie. Może skopiować do nowego konta miejsca do magazynowania lub zamierzasz używać tego samego konta miejsca do magazynowania, w zależności od potrzeb.
- Skopiuj i Zapisz klucz konta magazynowania konta docelowego miejsca do magazynowania do następnego etapu.

Zdecydowanie zaleca się możesz przeniesienie wszystkich danych dla produkcji obciążenie pracą korzystania z magazynu premium.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Krok 3. Przekazywanie wirtualnego dysku twardego do przechowywania Azure

Teraz, gdy masz do wirtualnego dysku twardego w katalogu lokalnego, możesz przekazać plik VHD do magazynowania Azure za pomocą AzCopy lub AzurePowerShell. Obie opcje są dostępne w tym miejscu:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Opcja 1: Przy użyciu programu PowerShell Azure Dodaj-AzureVhd przekazać plik VHD

    Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>

Przykład <Uri> może być **_"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"_**. Przykład <FileInfo> może być **_"C:\path\to\upload.vhd"_**.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Opcja 2: Za pomocą AzCopy przekazać plik VHD

Przy użyciu AzCopy, możesz łatwo przekazać wirtualnego dysku twardego przez Internet. W zależności od rozmiaru wirtualnych dysków twardych to może trochę potrwać. Pamiętaj sprawdzić limitu ingress i wyjściowego konta przy użyciu tej opcji. Aby uzyskać szczegółowe informacje, zobacz [skalowalność miejsca do magazynowania Azure i cele](storage-scalability-targets.md) .

1. Pobieranie i instalowanie AzCopy tutaj: [najnowszą wersję programu AzCopy](http://aka.ms/downloadazcopy)
2. Otwórz Azure programu PowerShell i przejdź do folderu, w którym zainstalowano AzCopy.
3. Użyj następującego polecenia, aby skopiować plik wirtualnego dysku twardego z "Źródłowy" do "Docelowy".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Przykład:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd

    Poniżej przedstawiono opisy parametry używane w tym poleceniu AzCopy:

 - * */Źródła: * &lt;źródła&gt;:*** lokalizację folderu lub adres URL kontenera miejsca do magazynowania, który zawiera wirtualnego dysku twardego.
 - * */SourceKey: * &lt;źródła konta klucza&gt;:*** klucz konta miejsca do magazynowania konta magazynu źródłowego.
 - * */Dest: * &lt;miejsce docelowe&gt;:*** adres URL kontenera magazynu do wirtualnego dysku twardego do skopiowania.
 - * */DestKey: * &lt;dest konta klucza&gt;:*** klucz konta miejsca do magazynowania konta docelowego miejsca do magazynowania.
 - **/BlobType: strona:** Określa, czy miejsce docelowe jest blob strony.
 - * */Deseniu: * &lt;nazwy pliku&gt;:*** Określ nazwę pliku wirtualnego dysku twardego do skopiowania.

Aby uzyskać szczegółowe informacje na temat korzystania z narzędzia AzCopy zobacz [Przenoszenie danych za pomocą narzędzia wiersza polecenia AzCopy](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Inne opcje przekazywania wirtualnego dysku twardego

Możesz również przekazać wirtualnego dysku twardego do swojego konta miejsca do magazynowania przy użyciu jednego z następujących sposobów:

- [Azure przechowywania kopii Blob interfejsu API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Blob przekazywania Eksploratora magazynu platformy Azure](https://azurestorageexplorer.codeplex.com/)
- [Odwołanie do miejsca do magazynowania Importuj/Eksportuj usługi REST interfejsu API](https://msdn.microsoft.com/library/dn529096.aspx)

>[AZURE.NOTE] Zaleca się przy użyciu usługi Importuj/Eksportuj, jeśli szacowany, że przekazywanie czasu jest dłuższa niż 7 dni. Za pomocą [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) oszacowanie czasu podmiotu rozmiar i przesyłanie danych.
>
> Importuj/Eksportuj może służyć do skopiowania na koncie standardowego magazynu. Należy skopiować z magazynu w standardowej premium magazynowania kontem za pomocą narzędzi, takich jak AzCopy.


## <a name="create-azure-virtual-machine-using-premium-storage"></a>Tworzenie maszyny wirtualne Azure przy użyciu magazynu Premium

Po wirtualny dysk twardy jest przekazane lub kopiowane do magazynowania odpowiednie konto, postępuj zgodnie z instrukcjami podanymi w tej sekcji, aby zarejestrować wirtualnego dysku twardego jako obraz systemu operacyjnego lub na dysk z systemem operacyjnym w zależności od aktualnego scenariusza, a następnie utworzyć wystąpienie maszyn wirtualnych z niego. Dane dysku wirtualnego dysku twardego może zostać dołączony maszyn wirtualnych po jego utworzeniu. Przykładowy skrypt migracji znajduje się na końcu tej sekcji. Ten prosty skrypt nie pasuje do wszystkich scenariuszach. Może być konieczne zaktualizowanie skrypt zgodnie z określonym rozwiązania. Aby wyświetlić, jeśli ten skrypt dotyczy rozwiązania, zobacz poniżej [Przykładowy skrypt migracji](#a-sample-migration-script).

### <a name="checklist"></a>Lista kontrolna

1.  Gdy wszystkie dyski wirtualny dysk twardy, kopiując zostało wykonane.
2.  Upewnij się, że miejsca do magazynowania Premium jest dostępna w regionie, które są migrowane do.
3.  Określ nową serię maszyn wirtualnych, który będzie używany. Należy go do przechowywania Premium, a rozmiar powinien być w zależności od dostępności w regionie i zależności od potrzeb.
4.  Określ dokładnie rozmiar pamięci Wirtualnej, którego chcesz użyć. Rozmiar pamięci Wirtualnej musi być mała, aby obsługiwać liczbę dysków danych, które masz. Np. Jeśli masz 4 dysków danych maszyn wirtualnych musi być rdzenie 2 lub więcej. Rozważ też przetwarzania power, pamięci i musi przepustowość sieci.
5.  Utwórz konto Premium miejsca do magazynowania w regionie docelowej. To jest konto, dla którego chcesz użyć do nowych maszyn wirtualnych.
6.  Masz bieżącą szczegóły maszyn wirtualnych zawsze pod ręką, łącznie z listy dysków i odpowiadające im blob wirtualnego dysku twardego.

Przygotowywanie aplikacji przestojów. Aby przeprowadzić migrację Oczyść, należy zatrzymać wszystkie przetwarzania w systemie. Następnie możesz uzyskać go do spójna, które można migrować do nowej platformy. Czas trwania przestoje zależy od ilości danych w oknie dysków do zmigrowania.

>[AZURE.NOTE] Jeśli tworzysz maszyn wirtualnych Azure Menedżera zasobów z dysku wirtualnego dysku twardego specjalistyczne, zajrzyj do [tego szablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) do wdrażania maszyn wirtualnych Menedżera zasobów przy użyciu istniejącego dysku.

### <a name="register-your-vhd"></a>Zarejestruj się do wirtualnego dysku twardego

Tworzenie maszyn wirtualnych z systemem operacyjnym wirtualnego dysku twardego lub dołączanie dysku danych do nowych maszyn wirtualnych, należy je najpierw zarejestrować. Wykonaj kroki wymienione poniżej, w zależności od scenariusza z wirtualnego dysku twardego.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Uogólniony wirtualnego dysku twardego systemu operacyjnego, aby utworzyć wielu wystąpień maszyn wirtualnych Azure

Po przekazaniu uogólniony obraz systemu operacyjnego wirtualny dysk twardy na koncie magazynowania Zarejestruj go jako **Obraz maszyn wirtualnych Azure** tak, aby można tworzyć wystąpienia maszyn wirtualnych co najmniej jeden z niego. Aby zarejestrować do wirtualnego dysku twardego jako obraz systemu operacyjnego maszyn wirtualnych Azure za pomocą następujące polecenia cmdlet programu PowerShell. Podaj adres URL pełną kontenera której zostało skopiowane wirtualnego dysku twardego.

    Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows

Skopiuj i Zapisz nazwę ten nowy obraz maszyn wirtualnych Azure. W powyższym przykładzie jest *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Unikatowe wirtualnego dysku twardego System operacyjny do utworzenia jednego wystąpienia maszyn wirtualnych Azure

Po przekazaniu unikatowy wirtualny dysk twardy OS na koncie magazynowania go zarejestrować jako **Dysku OS Azure** tak, aby można utworzyć wystąpienie maszyn wirtualnych z niego. Za pomocą tych poleceń cmdlet środowiska PowerShell do rejestrowania Twojej wirtualnego dysku twardego jako dysk OS Azure. Podaj adres URL pełną kontenera której zostało skopiowane wirtualnego dysku twardego.

    Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"

Skopiuj i Zapisz nazwę nowego dysku OS Azure. W powyższym przykładzie jest *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Dane na dysku wirtualnego dysku twardego załączany do nowego wystąpienia maszyn wirtualnych Azure

Po przekazaniu do konta miejsca do magazynowania danych dysku wirtualnego dysku twardego zarejestrować go jako dysk danych Azure może zostać dołączony do nowej serii DS, DSv2 serii lub maszyn wirtualnych Azure serii GS wystąpienie.

Za pomocą tych poleceń cmdlet środowiska PowerShell do rejestrowania Twojej wirtualnego dysku twardego jako dysk danych Azure. Podaj adres URL pełną kontenera której zostało skopiowane wirtualnego dysku twardego.

    Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"

Skopiuj i Zapisz nazwę nowego dysku danych Azure. W powyższym przykładzie jest *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Tworzenie maszyny do magazynowania Premium

Raz obraz systemu operacyjnego lub zarejestrowano dysku systemu operacyjnego, utworzyć nową serię DS, DSv2 serii lub serii GS maszyn wirtualnych. Obraz systemu operacyjnego lub nazwa dysku systemu operacyjnego, zarejestrowanego, będą używane. Wybierz typ maszyn wirtualnych z poziomu Premium miejsca do magazynowania. W poniższym przykładzie użyto rozmiar pamięci Wirtualnej *Standard_DS2* .

>[AZURE.NOTE] Aktualizowanie rozmiar dysku do upewnij się, że jest on taki sam i rozmiarów wolnego Azure wydajność i wymagania dotyczące wydajności.

Wykonaj krok po kroku poleceń cmdlet środowiska PowerShell poniżej, aby utworzyć nowy maszyn wirtualnych. Najpierw należy ustawić typowych parametrach:

    $serviceName = "yourVM"
    $location = "location-name" (e.g., West US)
    $vmSize ="Standard_DS2"
    $adminUser = "youradmin"
    $adminPassword = "yourpassword"
    $vmName ="yourVM"
    $vmSize = "Standard_DS2"

Najpierw należy utworzyć w jakiej będzie hostingu swojej nowej maszyny wirtualne usługi w chmurze.

    New-AzureService -ServiceName $serviceName -Location $location

Następnie w zależności od aktualnego scenariusza, Utwórz wystąpienie Azure maszyn wirtualnych z obraz systemu operacyjnego lub dysk systemu operacyjnego, który zostanie zarejestrowany.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Uogólniony wirtualnego dysku twardego systemu operacyjnego, aby utworzyć wielu wystąpień maszyn wirtualnych Azure

Utwórz jeden lub więcej nowych maszyn wirtualnych Azure serii Zasadami wystąpienia przy użyciu **Obraz systemu operacyjnego Azure** rejestrowania. Określ nazwę ten obraz systemu operacyjnego w konfiguracji maszyn wirtualnych, podczas tworzenia nowych maszyn wirtualnych, tak jak pokazano poniżej.

    $OSImage = Get-AzureVMImage –ImageName "OSImageName"

    $vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

    Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

    New-AzureVM -ServiceName $serviceName -VM $vm

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Unikatowe wirtualnego dysku twardego System operacyjny do utworzenia jednego wystąpienia maszyn wirtualnych Azure

Utwórz nowe Zasadami serii maszyn wirtualnych Azure wystąpienie za pomocą **Dysku OS Azure** rejestrowania. Określ nazwę tego dysku z systemem operacyjnym w konfiguracji maszyn wirtualnych, podczas tworzenia nowych maszyn wirtualnych, tak jak pokazano poniżej.

    $OSDisk = Get-AzureDisk –DiskName "OSDisk"

    $vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

    New-AzureVM -ServiceName $serviceName –VM $vm

Określ inne informacje maszyn wirtualnych Azure, takich jak usługa w chmurze, regionu, konto miejsca do magazynowania, Ustaw dostępność i zasady buforowania. Należy zauważyć, że wystąpienie maszyn wirtualnych musi być znajdują się w tym skojarzonego z nim systemu operacyjnego lub dyski danych, więc chmury wybranego konta usługi, region i miejsca do magazynowania muszą być w tej samej lokalizacji jako źródłowych wirtualnych dysków twardych dysków.

### <a name="attach-data-disk"></a>Dołączanie danych dysku

Ponadto jeśli zarejestrowano dysku danych wirtualnych dysków twardych, dołączone do nowego miejsca do magazynowania Premium stanie Azure maszyn wirtualnych.

Aby dołączyć dysku danych do nowych maszyn wirtualnych i określić zasady buforowania za pomocą po polecenia cmdlet programu PowerShell. W poniższym przykładzie buforowania zasady są ustawione na *tylko do odczytu*.

    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

    Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

    Update-AzureVM  -VM $vm

>[AZURE.NOTE] Czasami może występować dodatkowe kroki niezbędne do obsługi aplikacji, która jest nie objęte tego przewodnika.

### <a name="checking-and-plan-backup"></a>Sprawdzanie i Planowanie kopii zapasowej

Po nowych maszyn wirtualnych i rozpocząć pracę, dostęp za pomocą tego samego identyfikatora logowania i hasła jest jako oryginalny maszyn wirtualnych i sprawdź, czy że wszystko działa zgodnie z oczekiwaniami. Wszystkie ustawienia, w tym wielkość rozłożone może znajdować się w nowych maszyn wirtualnych.

Ostatnim krokiem jest planowanie kopii zapasowych i konserwacji harmonogram nowych maszyn wirtualnych oparte na potrzeby aplikacji.

### <a name="a-sample-migration-script"></a>Przykładowy skrypt migracji

Jeśli masz wiele maszyny wirtualne przeprowadzić migrację automatyzacji za pomocą skryptów programu PowerShell będą przydatne. Oto przykładowy skrypt, który pozwala zautomatyzować migracji maszyn wirtualnych. Uwaga poniżej skrypt jest tylko przykładem i istnieje kilka założenia dotyczące bieżącego dysków maszyn wirtualnych. Może być konieczne zaktualizowanie skrypt zgodnie z określonym rozwiązania.

Założenia są:

- Tworzysz klasyczny Azure maszyny wirtualne.
- Usługi dyski OS źródła i źródła danych są w tym samym i tego samego konta miejsca do magazynowania. Jeśli usługi dyski systemu operacyjnego, a dane nie są w tym samym miejscu, można użyć AzCopy lub Azure programu PowerShell kopiować wirtualnych dysków twardych kont miejsca do magazynowania i kontenerów. Można znaleźć w poprzednim kroku: [Kopiowanie wirtualny dysk twardy z AzCopy lub programu PowerShell](#copy-vhd-with-azcopy-or-powershell). Edytowanie tego skryptu spotkania rozwiązania jest wybór innej, zalecamy jednak za pomocą AzCopy lub programu PowerShell, ponieważ jest łatwiejsze i szybsze.

Poniżej znajduje się skrypt automatyzacji. Zamienianie tekstu wraz z informacjami i zaktualizuj skrypt zgodnie z określonym rozwiązania.

>[AZURE.NOTE] Przy użyciu istniejącego skryptu nie zostaną zachowane konfiguracji źródła maszyn wirtualnych. Konieczne będzie ponowne konfiguracji ustawień sieciowych na swojej migracją maszyny wirtualne.

    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED “AS IS” WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location “Southeast Asia”

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you’d like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location

#### <a name="optimization"></a>Optymalizacja

Bieżącej konfiguracji maszyn wirtualnych może dostosowane specjalnie do pracy ze standardowego dysków. Na przykład aby zwiększyć wydajność przy użyciu wielu dysków wielkości rozłożony. Na przykład zamiast 4 dysków oddzielnie ilość miejsca do magazynowania Premium, można zoptymalizować koszt przez jednego dysku. Optymalizacje, takich jak potrzeba można rozwiązać na podstawie indywidualnie i wymagają wykonania kroków niestandardowe po migracji. Należy również zauważyć, że ten proces nie może również działać dla baz danych i aplikacje, które zależą od układu dysku zdefiniowany w ustawieniach.

##### <a name="preparation"></a>Przygotowanie

1.  Wykonaj proste migracji, zgodnie z opisem w sekcji wcześniejszych. Optymalizacje zostaną wykonane na nowy maszyn wirtualnych po migracji.
2.  Definiowanie nowe rozmiary dysku wymagane przez zoptymalizowane konfiguracji.
3.  Określanie mapowania wielkości bieżącego dysków i do nowych specyfikacji dysku.

##### <a name="execution-steps"></a>Wykonywanie czynności

1.  Tworzenie nowych dysków z prawej o rozmiarach na maszyn wirtualnych miejsca do magazynowania Premium.
2.  Zaloguj się do maszyn wirtualnych i skopiowanie danych z bieżącego woluminu nowy dysk, który jest mapowany do tego woluminu. Zrób to dla wszystkich wielkości bieżącego, które trzeba mapować na nowy dysk.
3.  Następnie zmień ustawienia aplikacji, aby przejść do nowych dysków i odłączanie wielkość stare.

Dostosowywanie aplikacji w celu zwiększenia wydajności dysku, można znaleźć [Optymalizowania wydajności aplikacji](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Migracja aplikacji

Baz danych i innymi aplikacjami złożone może wymagać specjalnych kroków, zdefiniowane przez dostawcę aplikacji migracji. Zapoznaj się z dokumentacją odpowiednich aplikacji. Np. Zazwyczaj baz danych, możesz przeprowadzić migrację dzięki tworzenie kopii zapasowych i przywracanie.

## <a name="next-steps"></a>Następne kroki

Zobacz następujące zasoby dla określonych scenariuszach do migracji maszyn wirtualnych:

- [Migrowanie Azure maszyn wirtualnych między kontami miejsca do magazynowania](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
- [Tworzenie i przekazywanie wirtualnego dysku twardego serwera systemu Windows Azure.](../virtual-machines/virtual-machines-windows-classic-createupload-vhd.md)
- [Tworzenie i przekazywanie wirtualnego dysku twardego z systemem operacyjnym Linux](../virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md)
- [Migrowanie maszyn wirtualnych z Amazon AWS do platformy Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Ponadto zobacz następujące zasoby, aby dowiedzieć się więcej o przechowywaniu Azure i maszyn wirtualnych Azure:

- [Azure przestrzeni dyskowej](https://azure.microsoft.com/documentation/services/storage/)
- [Azure maszyn wirtualnych](https://azure.microsoft.com/documentation/services/virtual-machines/)
- [Przechowywanie Premium: Wysokiej wydajności miejsca do magazynowania dla obciążenia Azure maszyn wirtualnych](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
