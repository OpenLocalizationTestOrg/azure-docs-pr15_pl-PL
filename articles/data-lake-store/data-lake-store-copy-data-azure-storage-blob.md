<properties
   pageTitle="Skopiuj dane z obiektami blob miejsca do magazynowania Azure do magazynu Lake danych | Microsoft Azure"
   description="Skopiuj dane z obiektami blob miejsca do magazynowania Azure do magazynu Lake danych za pomocą narzędzia AdlCopy"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Skopiuj dane z obiektami blob miejsca do magazynowania Azure do magazynu Lake danych

> [AZURE.SELECTOR]
- [Przy użyciu DistCp](data-lake-store-copy-data-wasb-distcp.md)
- [Przy użyciu AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)

Magazyn Lake danych Azure udostępnia narzędzia wiersza polecenia [AdlCopy](http://aka.ms/downloadadlcopy), aby skopiować dane z następujących źródeł:

* Z Azure magazyn obiektów blob do magazynu Lake danych. Nie można używać AdlCopy, aby skopiować dane z magazynu Lake danych do magazynowania Azure blob.

* Między dwoma kontami magazynu Lake danych Azure. 

Ponadto służy narzędzie AdlCopy w dwóch różnych trybach:

* **Autonomiczna**, gdzie narzędzie używa magazynu Lake danych zasobów do wykonania tego zadania.

* **Przy użyciu konta analizy Lake danych**, której jednostki przypisanej do swojego konta analizy Lake danych służą do wykonywania operacji kopiowania. Można użyć tej opcji, jeśli szukasz wykonywania kopii w sposób przewidywalne.

##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).

- Kontener **Obiektów blob miejsca do magazynowania Azure** zawierającego dane.

- **Konto azure danych Lake analizy (opcjonalnie)** — zobacz [Wprowadzenie do analizy Lake danych Azure](../data-lake-analytics/data-lake-analytics-get-started-portal.md) Aby uzyskać instrukcje dotyczące tworzenia konta magazynu Lake danych.

- **Narzędzie AdlCopy**. Zainstaluj narzędzie AdlCopy z [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>Składnia narzędzia AdlCopy

Praca z narzędzia AdlCopy przy użyciu następującej składni

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

Poniżej opisano parametrów w składni:

| Opcja    | Opis                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Źródła    | Określa lokalizację danych źródłowych w Azure magazyn obiektów blob. Źródła może być kontenera obiektów blob, obiektów blob lub innego konta magazynu Lake danych.                                                                                                                                                                                                                                                                                                 |
| Docelowy      | Określa miejsce docelowe magazynu Lake danych kopiowania.                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Określa klucz dostępu miejsca do magazynowania dla źródła obiektów blob platformy Azure miejsca do magazynowania. Jest to wymagane tylko wtedy, gdy źródłem jest kontenera obiektów blob lub obiektów blob.                                                                                                                                                                                                                                                                                                                                                 |
| Konta   | **Opcjonalnie**. Użyj, aby uruchomić zadanie kopiowania za pomocą konta Azure danych Lake analizy. Jeśli opcja /Account w składni, ale nie określono konta analizy Lake danych, AdlCopy używa domyślnego konta do wykonywania zadania. Ponadto użycie tej opcji, należy dodać źródłowej (Blob miejsca do magazynowania Azure) i docelowej (Azure magazynu Lake danych) jako źródła danych dla Twojego konta analizy Lake danych.  |
| Jednostki     |  Określa liczbę jednostek analizy Lake danych, które będą używane do zadanie kopiowania. Ta opcja jest wymagane, jeśli określanie konta analizy Lake danych za pomocą opcji **/Account** .
| Wzór   | Określa wzorzec regex wskazującą, które obiektów blob lub pliki do skopiowania. AdlCopy używa dopasowania uwzględniana wielkość liter. Domyślny deseń używany, gdy określono nie wzorzec jest kopiowanie wszystkich elementów. Określanie wielu wzorców pliku nie jest obsługiwane.                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>Aby skopiować dane z obiektów blob magazyn Azure za pomocą AdlCopy (jako autonomicznego)

1. Otwórz wiersz polecenia i przejdź do katalogu, w którym AdlCopy jest zainstalowany, zwykle `%HOMEPATH%\Documents\adlcopy`.

2. Uruchom następujące polecenie, aby skopiować określonych obiektów blob z kontenera źródła do magazynu Lake danych:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Na przykład:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] Składnia powyżej Określa plik, który można skopiować do folderu na koncie magazynu Lake danych. Narzędzie AdlCopy tworzy folder, jeśli podana nazwa folderu nie istnieje.

    Wyświetli monit o podanie poświadczeń dla subskrypcji Azure, w którym masz konto magazynu Lake danych. Zostaną wyświetlone informacje podobne do następujących:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Możesz również skopiować z jednego kontenera wszystkich obiektów blob kontem magazynu Lake danych przy użyciu następującego polecenia:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Na przykład:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>Aby skopiować dane z innego konta magazynu Lake danych za pomocą AdlCopy (jako autonomicznego)

Za pomocą AdlCopy do skopiowania danych między dwa konta magazynu Lake danych.

1. Otwórz wiersz polecenia i przejdź do katalogu, w którym AdlCopy jest zainstalowany, zwykle `%HOMEPATH%\Documents\adlcopy`.

2. Uruchom następujące polecenie, aby skopiować określonego pliku z jednego konta magazynu Lake danych do innego.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Na przykład:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] Składnia powyżej Określa plik, który można skopiować do folderu znajdującego się w lokalizacji docelowej konta magazynu Lake danych. Narzędzie AdlCopy tworzy folder, jeśli podana nazwa folderu nie istnieje.

    Wyświetli monit o podanie poświadczeń dla subskrypcji Azure, w którym masz konto magazynu Lake danych. Zostaną wyświetlone informacje podobne do następujących:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. Następujące polecenie kopiuje wszystkie pliki z określonego folderu w źródle konta magazynu Lake danych do folderu znajdującego się w lokalizacji docelowej konta magazynu Lake danych.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Aby skopiować dane za pomocą AdlCopy (za pomocą konta analizy Lake danych)

Za pomocą konta analizy Lake danych do wykonywania zadania AdlCopy w celu skopiowania danych z Azure magazyn obiektów blob do magazynu Lake danych. Czy zwykle Użyj tej opcji, gdy dane, które mają zostać przeniesione znajduje się w zakresie gigabajtów i terabajtów i chcesz przepustowość przewidywalne i poprawić wydajność.

Aby użyć swojego konta analizy Lake danych z AdlCopy do skopiowania obiektów Blob platformy Azure miejsca do magazynowania, źródła (Azure magazyn obiektów Blob) musi zostać dodana jako źródła danych dla Twojego konta analizy Lake danych. Aby uzyskać instrukcje dotyczące dodawania kolejnych źródeł danych do konta analizy Lake danych zobacz [Zarządzanie analizy Lake dane konto źródeł danych](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

>[AZURE.NOTE] Jeśli kopiujesz z konta magazynu Lake Azure danych jako źródła przy użyciu konta analizy Lake danych, nie musisz skojarzyć konto magazynu Lake danych za pomocą konta analizy Lake danych. Wymóg chcesz skojarzyć magazynu źródłowego konta analizy Lake danych jest tylko wtedy, gdy źródłem jest konto Azure miejsca do magazynowania.

Uruchom następujące polecenie, aby skopiować z Azure magazyn obiektów blob do konta magazynu Lake danych przy użyciu konta analizy Lake danych:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Na przykład:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


Podobnie uruchom następujące polecenie, aby skopiować z Azure magazyn obiektów blob do konta magazynu Lake danych przy użyciu konta analizy Lake danych:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Kopiowanie danych przy użyciu wzorcem przy użyciu AdlCopy

W tej sekcji można dowiedzieć się, jak używać AdlCopy w celu skopiowania danych ze źródła (w naszym przykładzie poniżej firma Microsoft korzysta z obiektów Blob miejsca do magazynowania Azure) do miejsca docelowego konta magazynu Lake danych przy użyciu wzorcem. Za pomocą poniższych kroków można na przykład, skopiuj wszystkie pliki z rozszerzeniem CSV z obiektów blob źródła do miejsca docelowego.

1. Otwórz wiersz polecenia i przejdź do katalogu, w którym AdlCopy jest zainstalowany, zwykle `%HOMEPATH%\Documents\adlcopy`.

2. Uruchom następujące polecenie, aby skopiować wszystkie pliki z rozszerzeniem *.csv z określonych obiektów blob z kontenera źródła do magazynu Lake danych:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Na przykład:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>Rozliczenia

* Jeśli korzystasz z narzędzia AdlCopy jako autonomicznego będą naliczane koszty wyjściowego przenoszenia danych, jeśli źródło magazyn Azure konta nie znajduje się w tym samym regionie do przechowywania Lake danych.

* Jeśli korzystasz z narzędzia AdlCopy za pomocą konta analizy Lake danych zostaną zastosowane standardowe [stawki rozliczeń analizy Lake danych](https://azure.microsoft.com/pricing/details/data-lake-analytics/) .

## <a name="considerations-for-using-adlcopy"></a>Zagadnienia dotyczące korzystania z AdlCopy

* AdlCopy (w przypadku wersji 1.0.5) obsługuje kopiowania danych ze źródeł, które wspólnie mieć więcej niż tysiące plików i folderów. Jednak jeśli występują problemy, kopiując dużego zestawu danych, można rozmieścić pliki i foldery w różnych podfolderach i użyć ścieżkę do tych podfoldery jako źródła.

## <a name="next-steps"></a>Następne kroki

- [Zabezpieczanie danych w magazynie Lake danych](data-lake-store-secure-data.md)
- [Używanie analizy Lake Azure danych z magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usługa Azure HDInsight za pomocą magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)
