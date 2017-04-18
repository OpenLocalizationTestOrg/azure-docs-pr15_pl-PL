<properties
    pageTitle="Kopiowanie lub przenoszenie danych do magazynowania z AzCopy | Microsoft Azure"
    description="Narzędzie AzCopy umożliwia przenoszenie lub kopiowanie danych do lub z obiektów blob, tabel i zawartości pliku. Kopiowanie danych do magazynowania Azure w plikach lokalnych lub kopiowanie danych w obrębie lub między kontami miejsca do magazynowania. Łatwe migrację danych do magazynowania Azure."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="micurd"/>

# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>Przesyłanie danych za pomocą narzędzia wiersza polecenia AzCopy

## <a name="overview"></a>Omówienie

AzCopy jest przeznaczona do kopiowania danych z magazynu obiektów Blob platformy Azure firmy Microsoft, plików i tabeli za pomocą prostych poleceń z optymalnej wydajności narzędzie wiersza polecenia systemu Windows. Dane można kopiować z jednego obiektu do innego w ramach konta miejsca do magazynowania lub między kontami miejsca do magazynowania.

> [AZURE.NOTE] Ten przewodnik przyjęto założenie, że znasz już [Magazyn Azure](https://azure.microsoft.com/services/storage/). W przeciwnym razie czytania dokumentacji [Wprowadzenie do przechowywania Azure](storage-introduction.md) będą przydatne. Najważniejsze będzie konieczne [utworzenie konta przestrzeni dyskowej](storage-create-storage-account.md#create-a-storage-account) , aby rozpocząć korzystanie z narzędzia AzCopy.

## <a name="download-and-install-azcopy"></a>Pobieranie i instalowanie AzCopy

### <a name="windows"></a>Systemu Windows

Pobierz [najnowszą wersję programu AzCopy](http://aka.ms/downloadazcopy).

### <a name="maclinux"></a>Mac i Linux

AzCopy nie jest dostępna dla komputerów Mac i Linux OSs. Polecenie Azure jest jednak odpowiednie alternatywę do kopiowania danych z magazynu Azure. W tym artykule [przy użyciu interfejsu wiersza polecenia Azure z nośnikami Azure](storage-azure-cli.md) , aby dowiedzieć się więcej.

## <a name="writing-your-first-azcopy-command"></a>Pisanie pierwszego polecenia AzCopy

Podstawowa składnia polecenia AzCopy jest:

    AzCopy /Source:<source> /Dest:<destination> [Options]

Otwórz okno poleceń i przejdź do katalogu AzCopy instalacji na komputerze — miejsce, w którym `AzCopy.exe` wykonywalny znajduje się. W razie potrzeby można dodać lokalizację AzCopy do ścieżki systemowej. Domyślnie zainstalowano AzCopy `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` lub `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

W poniższych przykładach pokazano różnych scenariuszach do kopiowania danych z programu Microsoft Azure blob, plików i tabel. Zapoznaj się z sekcją [Parametrów AzCopy](#azcopy-parameters) szczegółowy opis parametry używane w każdej próbce.

## <a name="blob-download"></a>Obiektów blob: pobieranie

### <a name="download-single-blob"></a>Pobieranie pojedynczej obiektów blob

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"

Należy zauważyć, że jeśli folder `C:\myfolder` nie istnieje, utworzy go i Pobierz AzCopy `abc.txt ` do nowego folderu.

### <a name="download-single-blob-from-secondary-region"></a>Pobieranie pojedynczej obiektów blob z pomocniczym regionu

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Należy zauważyć, że muszą mieć dostęp do odczytu zbędne geo miejsca do magazynowania włączone.

### <a name="download-all-blobs"></a>Pobierz wszystkie obiekty BLOB

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Załóżmy, że następujących obiektów blob znajdują się w określonym kontenerze:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Po zakończeniu operacji pobierania katalogu `C:\myfolder` obejmuje następujące pliki:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Jeśli nie określisz opcję `/S`, BLOB nie będą pobierane.

### <a name="download-blobs-with-specified-prefix"></a>Pobieranie obiektów blob z określonego prefiksu

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Załóżmy, że następujących obiektów blob znajdują się w określonym kontenerze. Wszystkie obiekty BLOB rozpoczynający się od prefiksu `a` będą pobierane:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Po zakończeniu operacji pobierania folderu `C:\myfolder` obejmuje następujące pliki:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Prefiks dotyczy katalogu wirtualnego stanowi pierwszą część nazwy obiektów blob. W powyższym przykładzie katalogu wirtualnego jest niezgodna określony prefiks, więc nie są pobierane. Ponadto jeśli opcja `\S` nie zostanie określony, AzCopy nie będzie pobierać dowolną obiektów blob.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Ustawianie czasu ostatniej modyfikacji wyeksportowane pliki, aby być taki sam jak blob źródła

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

Można również wykluczyć obiektów blob z operacji pobierania według czasu ich ostatniej modyfikacji. Na przykład, jeśli chcesz wykluczyć obiektów blob, którego ostatnio zmodyfikowane czasu jest dodać tej samej lub nowszy niż plik docelowy `/XN` opcję:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

Lub jeśli chcesz wykluczyć obiektów blob, którego ostatnio zmodyfikowane czasu jest tej samej lub starsze niż plik docelowy Dodawanie `/XO` opcję:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

## <a name="blob-upload"></a>Obiektów blob: przekazywanie

### <a name="upload-single-file"></a>Przekazywanie jednego pliku

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"

Jeśli kontener określone miejsce docelowe nie istnieje, AzCopy jej tworzenia i przekaż plik do niego.

### <a name="upload-single-file-to-virtual-directory"></a>Przekazywanie jednego pliku do katalogu wirtualnego

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Jeśli określony katalog wirtualny nie istnieje, AzCopy będzie przekazać plik do dołączenia wirtualnego katalogu w nazwie (*np.* `vd/abc.txt` w powyższym przykładzie).

### <a name="upload-all-files"></a>Przekazywanie wszystkich plików

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

Określenie opcji `/S` przesłane zawartość określonego katalogu do lokalizacji przechowywania obiektów Blob, co oznacza, że wszystkie podfoldery i pliki zostaną przekazane także. Na przykład założono następujące pliki znajdują się w folderze `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Po zakończeniu operacji przekazywania kontenerze obejmuje następujące pliki:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Jeśli nie określisz opcję `/S`, AzCopy nie będzie przekazać lokalizacji. Po zakończeniu operacji przekazywania kontenerze obejmuje następujące pliki:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Przekaż pliki zgodne z określonym wzorcem

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Założono następujące pliki znajdują się w folderze `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Po zakończeniu operacji przekazywania kontenerze obejmuje następujące pliki:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Jeśli nie określisz opcję `/S`, AzCopy tylko zostaną przekazywanie obiektów blob, które nie znajdują się w katalogu wirtualnego:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Określ typ zawartości MIME blob miejsce docelowe

Domyślnie AzCopy ustawia typ zawartości blob miejsce docelowe, aby `application/octet-stream`. Począwszy od wersji 3.1.0, można jawnie określić typ zawartości za pomocą opcji `/SetContentType:[content-type]`. Tej składni ustawia typu zawartości dla wszystkich obiektów blob w operacji przekazywania.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Jeśli użytkownik określi `/SetContentType` bez wartości, następnie AzCopy ustawia poszczególnych obiektów blob lub typu zawartości pliku według rozszerzenie pliku.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## <a name="blob-copy"></a>Obiektów blob: kopiowanie

### <a name="copy-single-blob-within-storage-account"></a>Kopiowanie pojedynczej obiektów blob w ramach konta miejsca do magazynowania

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt

Podczas kopiowania obiektów blob w ramach konta usługi przechowywania przeprowadzana jest operacja [kopii po stronie serwera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) .

### <a name="copy-single-blob-across-storage-accounts"></a>Kopiowanie pojedynczej obiektów blob między kontami miejsca do magazynowania

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Podczas kopiowania obiektów blob na kontach miejsca do magazynowania, przeprowadzana jest operacja [kopii po stronie serwera](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) .

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Kopiowanie pojedynczej obiektów blob z regionu pomocniczej do podstawowy obszar

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Należy zauważyć, że muszą mieć dostęp do odczytu zbędne geo miejsca do magazynowania włączone.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopiowanie pojedynczej obiektów blob i jego migawek na kontach miejsca do magazynowania

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Po wykonaniu operacji kopiowania kontenera docelowego będzie zawierać to i jego migawek. Zakładając, że obiektów blob w powyższym przykładzie występują dwa zrzuty, kontener obejmuje następujące obiektów blob i migawki:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Synchronicznie kopiowanie obiektów blob między kontami miejsca do magazynowania

AzCopy domyślnie kopiuje asynchroniczne danych między dwoma punktami końcowymi miejsca do magazynowania. W związku z tym operacja kopiowania będzie działać w tle przy użyciu zdolności zamiennych przepustowości, zawierającą Umowa dotycząca poziomu usług pod względem jak szybko obiektów blob zostaną skopiowane i AzCopy okresowo sprawdza, czy Kopiuj stan do momentu kopiowania zostanie ukończona lub nie powiodła się.

`/SyncCopy` Opcja zapewnia, że operacja kopiowania otrzymają spójne szybkość. AzCopy wykonuje Kopiuj obraz, pobierając obiektów blob, aby skopiować z określonego źródła do lokalnej pamięci i przekazywania ich do docelowego magazyn obiektów Blob.

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy`wygenerować koszt dodatkowe wyjściowego w porównaniu z kopii asynchroniczne, to zalecane jest użycie tej opcji w maszyn wirtualnych Azure, który znajduje się w tym samym regionie jako źródło konta miejsca do magazynowania w celu uniknięcia wyjściowego koszt.

## <a name="file-download"></a>Plik: pobieranie

### <a name="download-single-file"></a>Pobieranie jednego pliku

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Jeśli określone źródło jest udział Azure pliku, a następnie należy określić nazwę pliku dokładnie (*np.* `abc.txt`) pobieranie jednego pliku i określ opcję `/S` Aby pobrać wszystkie pliki w lokalizacji Udostępnij. Próba Określ wzorzec plików i opcja `/S` ze sobą spowoduje błąd.

### <a name="download-all-files"></a>Pobierz wszystkie pliki

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S

Uwaga wszelkie pustych folderów nie będą pobierane.

## <a name="file-upload"></a>Plik: przekazywanie

### <a name="upload-single-file"></a>Przekazywanie jednego pliku

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt

### <a name="upload-all-files"></a>Przekazywanie wszystkich plików

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Uwaga wszelkie pustych folderów nie będą przekazywane.

### <a name="upload-files-matching-specified-pattern"></a>Przekaż pliki zgodne z określonym wzorcem

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

## <a name="file-copy"></a>Plik: kopiowanie

### <a name="copy-across-file-shares"></a>Kopiowanie między udziały plików

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

### <a name="copy-from-file-share-to-blob"></a>Kopiowanie z udziału pliku do blob

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Należy zauważyć, że asynchroniczne kopiowany magazyn plików do obiektów Blob strony nie jest obsługiwana.

### <a name="copy-from-blob-to-file-share"></a>Kopiowanie z obiektów blob do udziału plików

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

### <a name="synchronously-copy-files"></a>Synchronicznie kopiowanie plików

Możesz określić `/SyncCopy` opcję, aby skopiować dane z magazyn plików do przechowywania plików, Magazyn plików z magazynem obiektów Blob oraz magazyn obiektów Blob do przechowywania plików synchronicznie, AzCopy powinien się tym zająć, pobierając dane źródłowe do lokalnej pamięci i przekaż go ponownie do miejsca docelowego.

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Podczas kopiowania z magazynu w pliku z magazynem obiektów Blob, domyślny typ obiektów blob jest obiektów blob blok, użytkownik może określić opcję `/BlobType:page` Aby zmienić docelowy typ obiektów blob.

Należy zauważyć, że `/SyncCopy` wygenerować dodatkowe wyjściowym kosztów porównanie kopii asynchroniczne, to zalecane jest użycie tej opcji w maszyn wirtualnych Azure, która znajduje się w tym samym regionie jako źródła konta miejsca do magazynowania w celu uniknięcia wyjściowym koszt.

## <a name="table-export"></a>Tabela: eksportowanie

### <a name="export-table"></a>Eksportowanie tabeli

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy zapisuje manifestu pliku do folderu, w określonym miejscu docelowym. Plik manifestu umożliwia importowania Znajdź niezbędne pliki danych i sprawdzana poprawność danych. Plik manifestu domyślnie używa następującej konwencji nazewnictwa:

    <account name>_<table name>_<timestamp>.manifest

Użytkownik może również określić opcję `/Manifest:<manifest file name>` ustawić nazwy pliku manifestu.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest

### <a name="split-export-into-multiple-files"></a>Dzielenie eksportu do wielu plików

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy używa *indeksu głośności* w nazwach plików danych podziału, aby odróżnić wielu plików. Wskaźnik głośności składa się z dwóch części, *partycją indeksu klucza zakres* i *Dzielenie indeksu plików*. Oba indeksy są od zera.

Partycją indeksu klucza zakres będzie równa 0, jeśli użytkownik nie określi opcja `/PKRS`.

Załóżmy, AzCopy generuje dwa pliki danych, gdy użytkownik Określa opcję `/SplitSize`. Wynikowe nazwy plików danych mogą być:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Należy zauważyć, że minimalne możliwe wartości dla opcji `/SplitSize` wynosi 32MB. Jeśli określone miejsce docelowe jest magazyn obiektów Blob, AzCopy będzie dzielenie pliku danych po jego rozmiarów osiągnie limit rozmiaru obiektów blob (200GB), niezależnie od tego, czy opcja `/SplitSize` został określony przez użytkownika.

### <a name="export-table-to-json-or-csv-data-file-format"></a>Eksportowanie tabeli do JSON lub CSV format pliku danych

AzCopy domyślnie eksportowania tabel do plików danych JSON. Możesz określić opcję `/PayloadFormat:JSON|CSV` Eksportowanie tabel jako JSON lub w formacie CSV.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Określając ładunku w formacie CSV, AzCopy spowoduje również pliku schematu z rozszerzeniem `.schema.csv` dla każdego pliku danych.

### <a name="export-table-entities-concurrently"></a>Eksportowanie tabeli obiektów jednocześnie

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

AzCopy rozpocznie się równoczesne operacje, aby wyeksportować jednostki, gdy użytkownik określi opcja `/PKRS`. Kolejne operacje eksportuje jednego zakresu klucza partycją.

Należy zauważyć, że liczby równoległych operacji jest także uzależniony opcja `/NC`. AzCopy używa liczba procesorów jako wartości domyślnej `/NC` podczas kopiowania obiektów tabeli, nawet jeśli `/NC` nie określono. Gdy użytkownik Określa opcję `/PKRS`, AzCopy używa mniejsza z dwóch wartości - partition klucza zakresów i jawnie lub określony równoczesne wykonywanie operacji — do ustalania liczby równoczesne operacje, aby rozpocząć. Aby uzyskać więcej informacji, wpisz `AzCopy /?:NC` w wierszu polecenia.

### <a name="export-table-to-blob"></a>Eksportowanie tabeli do blob

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy wygeneruje plik danych JSON w kontenerze obiektów blob z następującego konwencji nazewnictwa:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Plik danych programu wygenerowane JSON w formacie ładunku minimalnego metadanych. Aby uzyskać szczegółowe informacje na ten format ładunku zobacz [Formatowanie ładunku dla operacji usługi tabeli](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Należy zauważyć, że podczas eksportowania tabel do obiektów blob, AzCopy będzie pobrać podmioty tabeli na lokalnym tymczasowe pliki danych, a następnie przekaż podmiotów do obiektów blob. Te pliki tymczasowe dane są wprowadzane do folderu plików dziennika ze ścieżką domyślne "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", można określić opcję/Z: [ruch folder dziennik pliku], aby zmienić w dzienniku lokalizację folderu plików i więc zmiany lokalizacji plików tymczasowych danych. Rozmiar pliku danych tymczasowych decyduje rozmiar podmiotów tabeli i rozmiar określony /SplitSize opcji, mimo że plik danych tymczasowych na lokalnym dysku zostanie usunięta natychmiast po przekazaniu do obiektów blob, upewnij się, że masz wystarczającą ilość miejsca na dysku lokalnym, aby zapisać te pliki tymczasowe danych przed usunięciem.

## <a name="table-import"></a>Tabela: importowanie

### <a name="import-table"></a>Tabela importu

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace

Opcja `/EntityOperation` wskazuje, jak wstawić obiektów do tabeli. Możliwe wartości są następujące:

- `InsertOrSkip`: Pomija istniejącego obiektu lub wstawia nowy obiekt, jeśli nie istnieje w tabeli.
- `InsertOrMerge`: Scala istniejącego obiektu lub wstawia nowy obiekt, jeśli nie istnieje w tabeli.
- `InsertOrReplace`: Powoduje zastąpienie istniejącego obiektu lub wstawia nowy obiekt, jeśli nie istnieje w tabeli.

Zauważ, że nie można określić opcji `/PKRS` w scenariuszu importu. W przeciwieństwie do tego scenariusza eksportu, w którym możesz określić opcję `/PKRS` zacząć równoczesne wykonywanie operacji AzCopy domyślnie rozpocznie się równoczesne wykonywanie operacji importowania tabeli. Domyślny numer równoczesne wykonywanie operacji pracę jest równa liczbie procesory; jednak można określić inną liczbę jednocześnie z opcją `/NC`. Aby uzyskać więcej informacji, wpisz `AzCopy /?:NC` w wierszu polecenia.

Należy zauważyć, że AzCopy obsługuje tylko importowanie w celu JSON, nie CSV. AzCopy obsługuje importowanie tabel z JSON utworzone przez użytkownika i nie pojawiają się pliki. Obie te pliki muszą pochodzić z AzCopy eksportowania tabeli. Aby uniknąć błędów, nie zmodyfikuj JSON wyeksportowane lub pojawiają pliku.

### <a name="import-entities-to-table-using-blobs"></a>Importowanie obiektów do tabeli przy użyciu obiektów blob

Załóżmy kontenera obiektów Blob zawiera następujące: reprezentujące Azure tabeli i jej towarzyszące pliku manifestu pliku JSON.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Można uruchomić następujące polecenie, aby zaimportować obiektów do tabeli za pomocą pliku manifestu w danym kontenerze obiektów blob:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"

## <a name="other-azcopy-features"></a>Inne funkcje AzCopy

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Skopiować tylko dane, które nie istnieje w lokalizacji docelowej

`/XO` i `/XN` parametry umożliwiają wykluczanie zasobów źródło starszych lub nowszy z kopiowana odpowiednio. Jeśli chcesz skopiować zasoby źródła, które nie istnieje w miejscu docelowym, można określić obu parametrów w poleceniu AzCopy:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Uwaga: To nie jest obsługiwane w przypadku źródłowej lub docelowej tabeli.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Umożliwia określenie parametrów wiersza polecenia pliku odpowiedzi

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

Możesz umieścić parametry wiersza polecenia AzCopy w pliku odpowiedzi. AzCopy przetwarza parametry w pliku tak, jakby były został określony w wierszu polecenia podczas wykonywania polecenia bezpośredni podstawiania zawartością pliku.

Przyjęto założenie, o nazwie pliku odpowiedzi `copyoperation.txt`, który zawiera następujące wiersze. Każdy parametr AzCopy można określić w jednym wierszu

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

lub na oddzielne wiersze:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy zakończy się niepowodzeniem, jeśli parametr można podzielić na dwa wiersze, jak pokazano w przykładzie dla `/sourcekey` parametr:

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Korzystanie z wielu plików odpowiedzi do określenia parametrów wiersza polecenia

Załóżmy pliku odpowiedzi o nazwie `source.txt` określająca kontenera źródła:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

Plik odpowiedzi o nazwie `dest.txt` określająca folder docelowy w systemie plików:

    /Dest:C:\myfolder

Plik odpowiedzi o nazwie `options.txt` , który określa opcje AzCopy:

    /S /Y

Aby zadzwonić do AzCopy z tych plików odpowiedzi, które znajdują się w katalogu `C:\responsefiles`, użyj tego polecenia:

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy przetwarza to polecenie tak, jak gdyby zostały uwzględnione wszystkie poszczególne parametry w wierszu polecenia:

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

### <a name="specify-a-shared-access-signature-sas"></a>Określ podpis udostępniania (SA)

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

Można też określić skojarzeń zabezpieczeń w kontenerze identyfikatora URI:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

### <a name="journal-file-folder"></a>Folder plików dziennika

Zawsze, gdy wydaj polecenie do AzCopy, to sprawdza, czy istnieje w pliku dziennika w domyślnym folderze lub czy nie istnieje w folderze określone za pomocą tej opcji. Jeśli plik dziennika nie istnieje w obu miejscu, AzCopy potraktuje operacji jako nowy i generuje nowy plik dziennika.

Jeśli istnieje plik dziennika, AzCopy sprawdza, czy wiersz polecenia, który wprowadzania odpowiada wiersza polecenia w pliku dziennika. Jeśli te dwie linie polecenia są zgodne, AzCopy życiorysy operację niekompletna. Jeśli nie odpowiadają, wyświetli się monit o albo zastąpić pliku dziennika, aby rozpocząć nową operację, lub, aby anulować bieżącej operacji.

Jeśli chcesz użyć domyślnej lokalizacji dla pliku dziennika:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

W przypadku pominięcia opcji `/Z`, lub określić opcję `/Z` bez ścieżkę folderu, jak pokazano powyżej, AzCopy tworzy plik dziennika w domyślnej lokalizacji, która jest `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Jeśli istnieje już plik dziennika, AzCopy życiorysy operacji na podstawie pliku dziennika.

Jeśli chcesz określić niestandardową lokalizację pliku dziennika:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

W tym przykładzie tworzy plik dziennika, jeśli jeszcze nie istnieje. Jeśli istnieje, AzCopy życiorysy operacji na podstawie pliku dziennika.

Jeśli chcesz wznowić operacji AzCopy:

    AzCopy /Z:C:\journalfolder\

W tym przykładzie życiorysy ostatniej operacji mógł wystąpić błąd do wykonania.

### <a name="generate-a-log-file"></a>Generowanie pliku dziennika

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Po określeniu opcji `/V` bez ich ścieżka pliku w dzienniku pełne, następnie AzCopy tworzy plik dziennika w domyślnej lokalizacji, która jest `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

W przeciwnym razie można utworzyć plik dziennika w niestandardowej lokalizacji:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Należy zauważyć, że jeśli użytkownik określi ścieżkę względną po opcja `/V`, takich jak `/V:test/azcopy1.log`, a następnie pełny dziennik zostanie utworzona w bieżącego katalogu roboczego w podfolderze o nazwie `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Umożliwia określenie liczby równoczesne operacje, aby rozpocząć

Opcja `/NC` określa liczbę operacji kopiowania jednocześnie. Domyślnie AzCopy uruchamia określoną liczbę równoczesne operacje, aby zwiększyć przepustowość transfer danych. Dla operacji tabeli liczbę operacji równoczesne jest równa liczby procesorów, których masz. Operacje obiektów Blob i plik, liczbę operacji równoczesne jest równa 8 razy liczby procesorów, których masz. Jeśli korzystasz z AzCopy w niskiej przepustowości sieci, można określić mniejszą wartość dla /NC uniknąć błędu spowodowany konkurencji zasobów.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Wykonywane AzCopy emulatora Azure miejsca do magazynowania

Aby uruchomić AzCopy przed [Azure emulatora miejsca do magazynowania](storage-use-emulator.md) dla obiektów blob:

    AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

i tabel:

    AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table

## <a name="azcopy-parameters"></a>Parametry AzCopy

Poniżej zamieszczono parametry AzCopy. Można także wpisać jedno z następujących poleceń z poziomu wiersza polecenia, aby uzyskać pomoc przy użyciu AzCopy:

- Aby uzyskać szczegółową pomoc wiersza polecenia dla AzCopy:`AzCopy /?`
- Aby uzyskać szczegółową pomoc dotyczącą każdego parametru AzCopy:`AzCopy /?:SourceKey`
- Przykłady wiersza polecenia:`AzCopy /?:Samples`

### <a name="sourcesource"></a>-Źródła: "źródłowy"

Określa dane źródłowe do skopiowania. Źródła może być katalog systemu plików, kontenera obiektów blob, katalog wirtualny obiektów blob, udziału plików miejsca do magazynowania, katalogu plików miejsca do magazynowania lub Azure tabeli.

**Dotyczą:** Tabele obiektów blob, pliki

### <a name="destdestination"></a>/ Dest: "docelowy"

Określa miejsce docelowe, aby skopiować do. Miejsce docelowe może być katalogu w systemie plików, kontenera obiektów blob, katalog wirtualny obiektów blob, udziału plików przestrzeni dyskowej, miejsca do magazynowania pliku katalogu lub Azure tabeli.

**Dotyczą:** Tabele obiektów blob, pliki

### <a name="patternfile-pattern"></a>-Deseniu: "Wzorzec plików"

Określa wzorzec plik, który wskazuje plików do skopiowania. Zachowanie parametr /Pattern jest określona przez lokalizację danych źródłowych i obecność opcja trybu cykliczne. Cykliczne zostanie podany za pomocą opcji opcji/s.

W przypadku określonego źródła katalogu w systemie plików, a następnie obowiązują standardowe symboli wieloznacznych, a deseń plik udostępniony jest dopasowywane plików w katalogu. Opcja określeniu, następnie AzCopy również pasuje do określonego wzorca przed wszystkie pliki w podfolderach poniżej katalogu.

W przypadku obiektów blob kontenera lub katalogu wirtualnego określonego źródła symboli wieloznacznych nie są stosowane. Jeśli opcja określeniu, następnie AzCopy deseniu określonego pliku są interpretowane jako prefiks obiektów blob. Jeśli opcję /S nie zostanie określony, następnie AzCopy wzorcem pliku z nazwami dokładnie obiektów blob.

Jeśli określone źródło jest udział Azure pliku, a następnie musisz Określ nazwę pliku dokładnie, (np. abc.txt) aby skopiować jednego pliku, lub określić opcję /S, aby skopiować wszystkie pliki w lokalizacji Udostępnij. Próba Określ wzorzec plików i opcja /S razem spowoduje błąd.

AzCopy używa dopasowania uwzględniania wielkości liter po /Source kontenera obiektów blob, czy katalog wirtualny obiektów blob i używa dopasowania bez uwzględniania wielkości liter we wszystkich innych przypadkach.

Domyślny wzorzec plików używany, gdy określono Brak desenia pliku jest *.* lub pusta prefiks dla lokalizacji przechowywania Azure lokalizację w systemie plików. Określanie wielu wzorców pliku nie jest obsługiwane.

**Dotyczą:** Blob, pliki

### <a name="destkeystorage-key"></a>/ DestKey: "klucz magazynowania"

Określa klucz konta miejsca do magazynowania dla zasobu docelowego.

**Dotyczą:** Tabele obiektów blob, pliki

### <a name="destsassas-token"></a>/ DestSAS: "skojarzeń zabezpieczeń token"

Określa udostępniony podpis programu Access (SA) mający uprawnienia do odczytu i zapisu dla lokalizacji docelowej (jeśli dotyczy). Ująć skojarzeń zabezpieczeń w podwójne cudzysłowy proste, jak mogą zawiera znaki specjalne wiersza polecenia.

W przypadku zasobu docelowego kontenera obiektów blob, udziale plików lub tabeli, można określić tę opcję, a po nim token skojarzeń zabezpieczeń lub skojarzeń zabezpieczeń można określić jako część kontener obiektów blob docelowy, udziale plików lub identyfikator URI tabeli, jeśli ta opcja nie.

Jeśli źródłowa i docelowa są oba blob, obiektów blob miejsce docelowe musi znajdować się w obrębie tego samego konta miejsca do magazynowania jako obiektów blob źródła.

**Dotyczą:** Tabele obiektów blob, pliki

### <a name="sourcekeystorage-key"></a>/ SourceKey: "klucz magazynowania"

Określa klucz konta miejsca do magazynowania dla zasobu źródła.

**Dotyczą:** Tabele obiektów blob, pliki

### <a name="sourcesassas-token"></a>/ SourceSAS: "skojarzeń zabezpieczeń token"

Określa podpis dostępu do udostępnionego z uprawnieniami Odczyt i lista źródła (jeśli dotyczy). Ująć skojarzeń zabezpieczeń w podwójne cudzysłowy proste, jak mogą zawiera znaki specjalne wiersza polecenia.

Jeśli zasób źródła jest kontenera obiektów blob i znajduje się klucz ani skojarzeń zabezpieczeń, za pomocą dostępu anonimowego przeczytaj jest kontenera obiektów blob.

Jeśli źródłem jest w udziale plików lub tabeli, należy podać klucz lub skojarzenia zabezpieczeń.

**Dotyczą:** Tabele obiektów blob, pliki

### <a name="s"></a>/S

Określa tryb cyklicznych operacji kopiowania. W trybie cykliczne AzCopy skopiuje wszystkich obiektów blob lub pliki, które są zgodne z wzorcem określonego pliku, łącznie z tymi w podfolderach.

**Dotyczą:** Blob, pliki

### <a name="blobtypeblock--page--append"></a>/ BlobType: "blok" | "Strona" | "Dołączanie"

Określa, czy miejsce docelowe obiektów blob blob blok, blob strony lub dołączanie obiektów blob. Ta opcja ma zastosowanie tylko wtedy, gdy wysyłasz obiektów blob. W przeciwnym razie jest generowany błąd. Jeśli miejscem docelowym jest obiektów blob i ta opcja nie jest określony, domyślnie AzCopy tworzy blob blok.

**Dotyczą:** BLOB

### <a name="checkmd5"></a>-CheckMD5

Oblicza mieszania MD5 dla pobrane dane i weryfikuje, czy mieszania MD5 przechowywane w obiekcie blob lub właściwość MD5 zawartości pliku jest zgodna z obliczeniowe skrótu. Sprawdź MD5 jest domyślnie wyłączone, dlatego należy określić tę opcję, aby sprawdzić MD5 podczas pobierania danych.

Zauważ, że magazyn Azure nie gwarantuje, że mieszania MD5 przechowywane dla obiektów blob lub pliku jest aktualne. Odpowiada klienta aktualizowania MD5 jest zmodyfikowany obiektów blob lub pliku.

AzCopy zawsze ustawia właściwości MD5 zawartości dla obiektów blob platformy Azure lub pliku po przekazaniu go do usługi.  

**Dotyczą:** Blob, pliki

### <a name="snapshot"></a>-Migawki

Wskazuje, czy przesyłanie migawek. Ta opcja jest prawidłowy tylko w przypadku, gdy źródłem jest obiektów blob.

Migawki przeniesionych obiektów blob są zmieniane w następującym formacie: .extension nazw obiektów blob (migawkę czasu)

Domyślnie migawki nie są kopiowane.

**Dotyczą:** BLOB

### <a name="vverbose-log-file"></a>-V [plik pełne dziennika]

Komunikaty o stanie pełne wyjście do pliku dziennika.

Domyślnie pełnego pliku dziennika ma nazwę AzCopyVerbose.log w `%LocalAppData%\Microsoft\Azure\AzCopy`. Jeśli użytkownik określi istniejącej lokalizacji pliku dla tej opcji, pełny dziennik zostaną dołączone do tego pliku.  

**Dotyczą:** Tabele obiektów blob, pliki

### <a name="zjournal-file-folder"></a>/ Z: [ruch folder dziennik pliku]

Określa folder plików dziennika dla wznawianie operacji.

Zawsze AzCopy obsługuje wznawianie, jeśli została przerwana operacji.

Jeśli ta opcja nie jest określona lub jest określona bez ścieżkę folderu, następnie AzCopy utworzy plik dziennika w lokalizacji domyślnej, czyli % LocalAppData%\Microsoft\Azure\AzCopy.

Zawsze, gdy wydaj polecenie do AzCopy, to sprawdza, czy istnieje w pliku dziennika w domyślnym folderze lub czy nie istnieje w folderze określone za pomocą tej opcji. Jeśli plik dziennika nie istnieje w obu miejscu, AzCopy potraktuje operacji jako nowy i generuje nowy plik dziennika.

Jeśli istnieje plik dziennika, AzCopy sprawdza, czy wiersz polecenia, który wprowadzania odpowiada wiersza polecenia w pliku dziennika. Jeśli te dwie linie polecenia są zgodne, AzCopy życiorysy operację niekompletna. Jeśli nie odpowiadają, wyświetli się monit o albo zastąpić pliku dziennika, aby rozpocząć nową operację, lub, aby anulować bieżącej operacji.

Plik dziennika zostanie usunięty po pomyślnym zakończeniu operacji.

Należy zauważyć, że wznawianie operacji z pliku dziennika utworzonego przy użyciu poprzedniej wersji programu AzCopy nie jest obsługiwana.

**Dotyczą:** Tabele obiektów blob, pliki

### <a name="parameter-file"></a>/@:"parameter-file"

Określa plik, który zawiera parametry. AzCopy przetwarza parametry w pliku, tak jakby były został określony w wierszu polecenia.

W pliku odpowiedzi można określić wiele parametrów w jednym wierszu lub określać każdego parametru w osobnym wierszu. Należy zauważyć, że poszczególne parametru nie może obejmować wiele wierszy.

Pliki odpowiedzi mogą zawierać wiersze komentarzy, które zaczynają się od symbolu #.

Można określić wielu plików odpowiedzi. Jednak pamiętać, że AzCopy nie obsługuje zagnieżdżonych plików odpowiedzi.

**Dotyczą:** Tabele obiektów blob, pliki

### <a name="y"></a>/ Y

Pomija wszystkie AzCopy prośby o potwierdzenie.

**Dotyczą:** Tabele obiektów blob, pliki

### <a name="l"></a>L

Umożliwia określenie operacji pozycję na liście. dane nie są kopiowane.

AzCopy będzie interpretować przy użyciu tej opcji jako symulacja uruchamiania wiersza polecenia bez tych opcji l i zliczania, ile obiekty zostaną skopiowane, można określić opcję /V w tym samym czasie, aby sprawdzić, które obiekty zostaną skopiowane w dzienniku pełne.

Zachowanie tej opcji również jest określona przez lokalizację danych źródłowych i obecności cykliczne tryb opcja /S i plik deseniu opcji /Pattern.

AzCopy są wymagane uprawnienia listy i Odczyt tej lokalizacji źródła przy użyciu tej opcji.

**Dotyczą:** Blob, pliki

### <a name="mt"></a>/MT

Ustawia czasu ostatniej modyfikacji pobrany plik, aby taka sama jak blob źródła lub pliku.

**Dotyczą:** Blob, pliki

### <a name="xn"></a>/XN

Wyłącza nowszego zasobów źródła. Zasób nie zostaną skopiowane, jeśli godzina ostatniej modyfikacji źródła jest taki sam lub nowszy niż element docelowy.

**Dotyczą:** Blob, pliki

### <a name="xo"></a>/XO

Nie obejmuje starsze zasobów źródła. Zasób nie zostaną skopiowane, jeśli godzina ostatniej modyfikacji źródła jest taki sam lub starsze niż docelowy.

**Dotyczą:** Blob, pliki

### <a name="a"></a>/A

Przekazywanie tylko pliki, które mają ustawiony atrybut archiwum.

**Dotyczą:** Blob, pliki

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]

Przekazywanie tylko pliki, które w dowolnej z zestawu określonych atrybutów.

Dostępne atrybuty obejmują:

- R = pliki tylko do odczytu
- = Plik gotowy do archiwizacji
- S = pliki systemowe
- H = ukryte pliki
- C = skompresowane pliki
- N = zwykłych plików
- E = zaszyfrowana plików
- T = tymczasowych plików
- O = plików w trybie Offline
- I = indeksowane nie plików

**Dotyczą:** Blob, pliki

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Nie obejmuje pliki, które w dowolnej z zestawu określonych atrybutów.

Dostępne atrybuty obejmują:

- R = pliki tylko do odczytu
- = Plik gotowy do archiwizacji
- S = pliki systemowe
- H = ukryte pliki
- C = skompresowane pliki
- N = zwykłych plików
- E = zaszyfrowana plików
- T = tymczasowych plików
- O = plików w trybie Offline
- I = indeksowane nie plików

**Dotyczą:** Blob, pliki

### <a name="delimiterdelimiter"></a>-Ogranicznika: "separator"

Wskazuje znak ogranicznika używane do ograniczania katalogów wirtualnych w nazwach obiektów blob.

Domyślnie używa AzCopy / w charakterze ogranicznika. Jednak AzCopy obsługuje przy użyciu dowolnego typowych znaku (takie jak @, # lub %) jako ogranicznika. Jeśli chcesz dołączyć jedno z tych znaków specjalnych w wierszu polecenia, należy wpisać nazwę pliku w podwójne cudzysłowy proste.

Ta opcja dotyczy tylko pobieranie obiektów blob.

**Dotyczą:** BLOB

### <a name="ncnumber-of-concurrent-operations"></a>-NC: "numer z równoczesne operacje"

Określa liczbę operacji jednocześnie.

AzCopy domyślnie uruchamia określoną liczbę równoczesne operacje, aby zwiększyć przepustowość transfer danych. Należy zauważyć, że dużej liczby równoczesne wykonywanie operacji w środowisku niskiej przepustowości może zasypać połączenie sieciowe i zapobiec operacje w pełni ukończenie. Ograniczenia równoczesne operacje oparte na rzeczywiste dostępna przepustowość sieci.

Górny limit równoczesne wykonywanie operacji jest 512.

**Dotyczą:** Tabele obiektów blob, pliki

### <a name="sourcetypeblob--table"></a>-Źródłowa: "Obiektów Blob" | "Table"

Określa, że `source` zasób jest blob dostępne w środowisku lokalnym rozwoju, uruchomione w emulatorze przestrzeni dyskowej.

**Dotyczą:** Blob, tabel

### <a name="desttypeblob--table"></a>/ DestType: "Obiektów Blob" | "Table"

Określa, że `destination` zasób jest blob dostępne w środowisku lokalnym rozwoju, uruchomione w emulatorze przestrzeni dyskowej.

**Dotyczą:** Blob, tabel

### <a name="pkrskey1key2key3"></a>/ PKRS: "klucz1 #key2 klucz3 #..."

Dzieli zakres klucza partition umożliwiające eksportowania danych tabeli równolegle, co przyspiesza operacji eksportowania.

Jeśli ta opcja nie jest określony, następnie AzCopy używa jednym wątku do wyeksportowania tabeli jednostek. Na przykład, jeśli użytkownik wpisze /PKRS: "aa #bb", a następnie AzCopy zaczyna się trzy równoczesne wykonywanie operacji.

Kolejne operacje Eksportuje jeden z trzech zakresów klucza partition, tak jak pokazano poniżej:

  [Imię partition klucza, aa)

  [aa, bb)

  [bb, ostatni partition klucza]

**Dotyczą:** Tabele

### <a name="splitsizefile-size"></a>/ SplitSize: "rozmiar pliku"

Umożliwia określenie eksportowanego pliku dzielenie rozmiar w Megabajtach, minimalna dozwolona wartość wynosi 32.

Jeśli ta opcja nie jest określony, AzCopy będzie eksportowania danych tabeli do jednego pliku.

Jeśli dane w tabeli jest eksportowany do obiektów blob, a rozmiar eksportowanego pliku osiągnie 200 GB limit rozmiaru obiektów blob, następnie AzCopy zostaną podzielone eksportowanego pliku, nawet jeśli ta opcja nie jest określona.

**Dotyczą:** Tabele

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"

Umożliwia określenie procesu importowania danych tabeli.

- InsertOrSkip — umożliwia pomijanie istniejącego obiektu lub wstawia nowy obiekt, jeśli nie istnieje w tabeli.

- InsertOrMerge - scala istniejącego obiektu lub wstawia nowy obiekt, jeśli nie istnieje w tabeli.

- InsertOrReplace — powoduje zastąpienie istniejącego obiektu lub wstawia nowy obiekt, jeśli nie istnieje w tabeli.

**Dotyczą:** Tabele

### <a name="manifestmanifest-file"></a>-Manifestu: "manifest pliku"

Określa plik manifestu tabeli eksportowanie oraz importowanie operacji.

Ta opcja jest opcjonalne w trakcie operacji eksportowania, AzCopy wygeneruje plik manifestu z wstępnie zdefiniowanej nazwie, jeśli ta opcja nie jest określona.

Ta opcja jest wymagana podczas operacji importu dla znajdowanie plików danych.

**Dotyczą:** Tabele

### <a name="synccopy"></a>-SyncCopy

Wskazuje, czy do synchronicznie kopiowania obiektów blob lub plików między dwoma punktami końcowymi magazyn Azure.

AzCopy domyślnie używa kopii asynchroniczne po stronie serwera. Określ tę opcję, aby wykonać kopię synchroniczne, która pobiera obiektów blob lub pliki do lokalnej pamięci i przekazywania ich do magazynu Azure.

Za pomocą tej opcji podczas kopiowania plików w magazyn obiektów Blob, przechowywania plików, lub z magazynem obiektów Blob do przechowywania plików lub na odwrót.

**Dotyczą:** Blob, pliki

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "typ zawartości"

Określa typ zawartości MIME dla obiektów blob pola Lokalizacja docelowa lub pliki.

AzCopy ustawiono typ zawartości dla obiektów blob lub pliku aplikacji/octet strumienia domyślnie. Typ zawartości dla wszystkich obiektów blob lub pliki można ustawić jawnie określając wartości dla tej opcji.

Jeśli użytkownik określi ta opcja bez wartości, AzCopy spowoduje ustawienie poszczególnych obiektów blob lub typu zawartości pliku według rozszerzenie pliku.

**Dotyczą:** Blob, pliki

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"

Określa format pliku eksportowanych danych tabeli.

Jeśli ta opcja nie jest określony, domyślnie AzCopy eksportowany plik danych tabeli w formacie JSON.

**Dotyczą:** Tabele

## <a name="known-issues-and-best-practices"></a>Znane problemy i najważniejsze wskazówki

### <a name="limit-concurrent-writes-while-copying-data"></a>Równoczesne zapisy limit podczas kopiowania danych

Podczas kopiowania obiektów blob lub pliki z AzCopy, należy pamiętać o tym, że innej aplikacji może być modyfikowania danych podczas kopiowania go. Jeśli to możliwe upewnij się, że dane, które są kopiowane jest nie modyfikowany podczas kopiowania. Na przykład podczas kopiowania wirtualnego dysku twardego skojarzone z Azure maszyn wirtualnych, upewnij się, że żadne inne aplikacje są obecnie zapisywania do wirtualnego dysku twardego. Dobrym sposobem na tym jest przeniesienie zasobu do skopiowania. Można też najpierw utworzyć migawkę wirtualny dysk twardy, a następnie skopiuj migawki.

Jeśli nie zabezpiecza inne aplikacje napisanie obiektów blob lub pliki są kopiowane, następnie należy pamiętać o tym, że przez razem, gdy zakończy się zadanie, skopiowany zasobów już nie może mieć pełny spójności z zasobami źródła.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Uruchom jednego wystąpienia AzCopy na komputerze.
AzCopy jest przeznaczona do maksymalizowanie wykorzystania zasobu komputera przyspieszyć transfer danych, zalecamy, aby uruchomić tylko jedno wystąpienie AzCopy na jednym komputerze i zaznacz opcję `/NC` w razie potrzeby jednoczesną obsługę większej liczby operacji. Aby uzyskać więcej informacji, wpisz `AzCopy /?:NC` w wierszu polecenia.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>Włączanie algorytmów MD5 FIPS dla AzCopy podczas możesz "Użyj FIPS algorytmów do szyfrowania, mieszania i podpisywania".
AzCopy domyślnie używa implementacji .NET MD5 do obliczania wartości MD5 podczas kopiowania obiektów, ale istnieje kilka wymagań dotyczących zabezpieczeń, które wymagają AzCopy, aby włączyć ustawienie MD5 zgodny z FIPS.

Można utworzyć plik app.config `AzCopy.exe.config` właściwość `AzureStorageUseV1MD5` i umieszczanie go odłogowania z AzCopy.exe.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

• Właściwości "AzureStorageUseV1MD5" PRAWDA — wartość domyślna AzCopy powinny zawierać implementacji .NET MD5.
• FAŁSZ — AzCopy użyje zgodnego algorytmu MD5 FIPS.

Należy zauważyć, że algorytmów FIPS jest domyślnie wyłączone na komputerze systemu Windows, możesz wpisać secpol.msc w oknie Uruchom i zaznacz ten przełącznik na ustawienia zabezpieczeń -> lokalnych zasad -> Zabezpieczenia Opcje -> kryptograficzny systemu: Użyj zgodnych algorytmów FIPS do szyfrowania, mieszania i podpisywania.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o przechowywaniu Azure i AzCopy zapoznaj się z poniższych zasobów.

### <a name="azure-storage-documentation"></a>Dokumentacja Azure miejsca do magazynowania:

- [Wprowadzenie do przechowywania Azure](storage-introduction.md)
- [Jak używać magazyn obiektów Blob z .NET](storage-dotnet-how-to-use-blobs.md)
- [Jak używać magazyn plików z .NET](storage-dotnet-how-to-use-files.md)
- [Jak używać magazyn tabel z .NET](storage-dotnet-how-to-use-tables.md)
- [Sposoby tworzenia, zarządzania i usunąć konto miejsca do magazynowania](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure wpisów w blogu magazynowania:
- [Wprowadzenie do usługi Azure magazynowania danych przepływu biblioteki Preview](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Wprowadzenie do kopiowania synchroniczne i dostosowany typu zawartości](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Informowanie o ogólne dostępność 3.0 AzCopy plus wersji preview 4.0 AzCopy z tabeli i plik pomocy technicznej](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Zoptymalizowana pod kątem scenariuszy dużych Kopiuj](http://go.microsoft.com/fwlink/?LinkId=507682)
- [AzCopy: Obsługę magazynowania zbędne geo dostęp do odczytu](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Transferować dane z trybu będzie można ponownie uruchomić systemu i token skojarzenia zabezpieczeń](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Przy użyciu obiektów Blob kopiowania między konta](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Przekazywanie i pobieranie plików dla obiektów blob platformy Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
