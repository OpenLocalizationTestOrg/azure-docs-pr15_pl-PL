<properties
    pageTitle="Kopiowanie wydajność działania i dostosowywania przewodnik | Microsoft Azure"
    description="Informacje na temat kluczy czynniki, które wpływ na wydajność przepływu danych w Factory danych Azure, korzystając z kopii aktywności."
    services="data-factory"
    documentationCenter=""
    authors="linda33wj"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="jingwang"/>


# <a name="copy-activity-performance-and-tuning-guide"></a>Kopiowanie wydajność działania i dostosowywania przewodnika
Azure danych Factory kopii w trybie offline udostępnia najlepszych bezpieczne, niezawodne i wysokiej wydajności ładowania danych rozwiązanie. On umożliwia dziesiątki kopię terabajtów danych codziennie różnych sformatowanego chmury i magazynów lokalnego. Ładowanie wydajności danych ogromną szybkie jest klucz, aby upewnić się, możesz skoncentrować się na problemu "big data" core: tworzenie rozwiązań zaawansowanej analizy i uzyskiwanie wniosków głębokości z wszystkie te dane.

Azure udostępnia zestaw klasy korporacyjnej rozwiązań magazynu danych i magazynowania danych i kopiowanie działanie oferuje wysoce zoptymalizowane ładowania środowisko, które są łatwe do skonfigurowania i danych. Aktywnością tylko jedną kopią można uzyskać:

- Ładowanie danych do **Magazynu danych SQL Azure** w **1,2 GB**
- Ładowanie danych do **Magazyn obiektów Blob platformy Azure** na **GB 1.0**
- Ładowanie danych do **Magazynu Lake danych Azure** na **GB 1.0**


Ten artykuł zawiera opis:

- [Numery odwołań wydajności](#performance-reference) dla obsługiwanych źródeł i sink danych są przechowywane do planowania projektu.
- Funkcje, które może zwiększyć przepustowość kopii w różnych scenariuszach, takich jak [osobną kopię](#parallel-copy), [jednostki przepływu danych w chmurze](#cloud-data-movement-units)i [umieszczane kopiowania](#staged-copy);
- [Wskazówki dotyczące dostosowywania wydajności](#performance-tuning-steps) na temat Dostosowywanie wydajności i kluczowych czynników, które mogą mieć wpływ na wydajność kopii.

> [AZURE.NOTE] Jeśli znasz nie Kopiuj aktywności ogólnie, zobacz [Przenoszenie danych za pomocą kopiowania aktywności](data-factory-data-movement-activities.md) przed przeczytaniem tego artykułu.

## <a name="performance-reference"></a>Informacje dotyczące wydajności

![Macierz wydajności](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

> [AZURE.NOTE] Można uzyskać wyższej przepustowości dzięki zastosowaniu jednostki przepływu więcej danych (DMUs) niż domyślny maksymalny DMUs, które jest 8 wykonania kopii w chmurze w chmurze, uruchom. Na przykład 100 DMUs umożliwia kopiowanie danych z obiektów Blob platformy Azure magazynu Lake danych Azure szybkością 1 gigabajt na sekundę. Zobacz sekcję [jednostki przepływu danych w chmurze](#cloud-data-movement-units) , aby uzyskać szczegółowe informacje o tej funkcji. Skontaktuj się z [Azure pomocy technicznej](https://azure.microsoft.com/support/) , aby zażądać więcej DMUs.

Informacje, które należy pamiętać:

- Przepustowość jest obliczana przy użyciu następującej formuły: [odczytu rozmiar danych ze źródła] / [czas trwania zadania czynność kopii].
- Numery wydajności w tabeli zostały mierzone przy użyciu zestawu danych [TPC-H](http://www.tpc.org/tpch/) w działaniu jedną kopią Uruchom.
- Aby skopiować między magazynów danych w chmurze, ustaw **cloudDataMovementUnits** 1 i 4 (lub 8) do porównania. nie określono **parallelCopies** . Zobacz sekcję [osobną kopię](#parallel-copy) , aby uzyskać szczegółowe informacje o tych funkcjach.
- W sklepach Azure danych źródła i sink są w tym samym regionie Azure.
- Dla hybrydowych (lokalnego do chmury lub w chmurze do lokalnego) przenoszenia danych w jedno wystąpienie bramy działała na komputerze, który był osobnym z lokalnego magazynu danych. Konfiguracja znajduje się w następnej tabeli. Jeśli pojedynczego działania został uruchomiony dla bramy, operacja kopiowania zużyte tylko niewielką część Procesora komputera test, pamięci lub przepustowość sieci.
    <table>
    <tr>
        <td>PROCESOR</td>
        <td>32 rdzenie 2,20 GHz firmy Intel Xeon E5-2660 wersji 2</td>
    </tr>
    <tr>
        <td>Pamięci</td>
        <td>128 GB</td>
    </tr>
    <tr>
        <td>Sieci</td>
        <td>Interfejs internetowy: 10 GB Interfejs sieci intranet: 40 GB</td>
    </tr>
    </table>

## <a name="parallel-copy"></a>Osobną kopię
Można odczytywać dane ze źródła lub zapisać danych do miejsca docelowego **równolegle w działaniem kopii uruchomić**. Ta funkcja zwiększa przepustowość operacji kopiowania i skraca czas potrzebny do przenoszenia danych.

To ustawienie jest inne niż właściwości **współbieżności** w definicji aktywności. Właściwość **współbieżności** określa liczbę **uruchamia równoczesne aktywności kopii** przetwarzania danych z różnych aktywności systemu windows (1: 00 do 2 AM, AM 2 do 3 AM, 3 Południa do 4 AM i tak dalej). Ta funkcja jest przydatne podczas wykonywania historycznych ładowania. Funkcja osobną kopię dotyczy **pojedynczego działania uruchomić**.

Przyjrzyjmy się przykładowy scenariusz. W poniższym przykładzie wiele wycinków w przeszłości muszą być przetworzone. Factory danych działa wystąpienie aktywności Kopiuj (Uruchom aktywności) dla każdego wycinka:

- Wycinek z okna pierwszej aktywności (1: 00 AM do 2) == > aktywności uruchomić 1
- Wycinek z drugiego okna aktywności (2 AM do 3 AM) == > aktywności Uruchom 2
- Wycinek z drugiego okna aktywności (3 AM do 4: 00) == > aktywności uruchomić 3

I tak dalej.

W tym przykładzie wartość **współbieżności** jest ustawiona na wartość 2, **aktywności Uruchom 1** i **2 uruchomić działania** skopiować dane z dwóch aktywności windows **jednocześnie** w celu zwiększenia wydajności przepływu danych. Jednak jeśli pliki są skojarzone z działaniem uruchomić 1, usługi przepływu danych kopiuje pliki ze źródła do jednego pliku docelowego naraz.

### <a name="parallelcopies"></a>parallelCopies
Właściwość **parallelCopies** oznaczającą podobieństwa odpowiednią aktywności kopii korzystać. Tej właściwości można traktować jako maksymalna liczba wątków w kopii działanie, które można przeczytać ze źródła lub zapisać sklepach danych sink równolegle.

Dla każdej czynności kopii uruchamianie Factory danych określa liczbę kopii równoległe, aby skopiować dane ze źródła danych, przechowywanie i danych miejsca docelowego przechowywanie za pomocą. Domyślne liczbę kopii równoległe używa zależy od typu źródła i zlew, którego używasz.  

Źródłowa i sink |   Liczba osobną kopię domyślne określone przez usługę
------------- | -------------------------------------------------
Kopiowanie danych między opartych na plikach sklepy (magazyn obiektów Blob; Magazyn Lake danych; Amazon S3; system plików lokalnego; HDFS lokalnego) | Od 1 do 32. Zależy od rozmiaru plików i liczba jednostek przepływu danych chmury (DMUs) używane do przenoszenia danych między dwoma zbiorów danych chmury lub fizycznie konfiguracji komputera bramy na potrzeby kopii hybrydowych (Aby skopiować dane, do lub z lokalnego magazynu danych).
Skopiuj dane z **przechowywać dowolne dane źródłowe z magazynem tabel platformy Azure** | 4
Wszystkie inne źródła i sink pary | 1

Zazwyczaj domyślne zachowanie powinien podać Ci najlepszą wydajność. Do sterowania obciążenia na komputerach, które obsługują dane są przechowywane, lub, aby dostosować wydajności kopiowania, możesz zastąpić wartość domyślna i określić wartości dla właściwości **parallelCopies** . Wartość musi być między 1 a 32 (obie włącznie). W czasie wykonywania celu uzyskania najlepszej wydajności, Kopiuj aktywności używa wartości, która jest mniejsza niż lub równa wartości ustawiany.

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "parallelCopies": 8
            }
        }
    ]

Informacje, które należy pamiętać:

- Podczas kopiowania danych między opartych na plikach sklepy równoległości odbywa się na poziomie pliku. Nie ma żadnych segmentu w jednym pliku. Rzeczywista liczba kopii równoległe używa usługi przepływu danych dla operacji kopiowania w czasie wykonywania nie jest większa niż liczba plików. Jeśli zachowanie Kopiuj jest **mergeFile**, kopiowanie działanie nie można korzystać z równoległości.
- W przypadku określenia wartości dla właściwości **parallelCopies** , należy rozważyć, czy Zwiększ ładowania magazynów danych źródła i sink i do bramy przypadku kopii hybrydowych. Dzieje się tak szczególnie gdy masz kilka działań lub równoczesne zostanie uruchomiona działalności uruchamiane przed tym samym magazynu danych. Jeśli okaże się, że magazynu danych albo brama jest przerasta obciążenia, zmniejsz wartość **parallelCopies** w celu zmniejszenia obciążenia.
- Po skopiowaniu danych z magazynów, które nie są na podstawie pliku do sklepów, które są oparte na plik usługi przepływu danych ignoruje właściwości **parallelCopies** . Nawet jeśli określono równoległości, nie zostanie zastosowane w tym przypadku.

> [AZURE.NOTE] Korzystanie z funkcji **parallelCopies** po wykonaniu kopii hybrydowe, należy użyć bramy zarządzania danymi w wersji 1.11 lub nowszej.

### <a name="cloud-data-movement-units"></a>Jednostki przepływu danych w chmurze
**Jednostka przepływu danych w chmurze (DMU)** jest miarą, która reprezentuje możliwości jednostką w Factory danych dodatku (kombinacja Procesora, pamięci i alokacji zasobów sieci). DMU może być używana w operacji kopii w chmurze w chmurze, ale nie w kopii hybrydowych.

Domyślnie Factory danych korzysta z jednego chmury DMU do wykonania jednego działania kopii uruchamianie. Aby zmienić to ustawienie domyślne, określ następujące wartości dla właściwości **cloudDataMovementUnits** . Aby uzyskać informacje o poziomie wydajność może zostać wyświetlony po skonfigurowaniu większej liczby jednostek dla określonej kopii źródła i zlew zobacz [informacje dotyczące wydajności](#performance-reference).

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "cloudDataMovementUnits": 4
            }
        }
    ]

**Dozwolone wartości** dla właściwości **cloudDataMovementUnits** są 1 (ustawienie domyślne), 2, 4 i 8. **Rzeczywista liczba chmury DMUs** używaną podczas wykonywania operacji kopiowania jest równa lub mniejsza niż wartość skonfigurowane, w zależności od deseniu danych. 

> [AZURE.NOTE] Jeśli potrzebujesz więcej w chmurze DMUs dla wyższej przepustowości, skontaktuj się z [Azure obsługi](https://azure.microsoft.com/support/). Ustawianie 8 oraz powyżej obecnie działa tylko wtedy, gdy skopiować kilka plików z magazynem obiektów Blob magazynem obiektów Blob, magazynu Lake danych lub bazy danych SQL Azure i rozmiar pliku jest większe niż lub równe 16 MB pojedynczo.

Aby lepiej użyć tych dwóch właściwości i ulepszanie usługi przepustowość przepływu danych, zobacz [przykładowe spraw za pomocą](#case-study-use-parallel-copy). Nie musisz skonfigurować **parallelCopies** skorzystać z zachowaniem domyślnym. Jeśli zostanie skonfigurowane i **parallelCopies** jest za mały, wielu chmury DMUs może nie w pełni w.  

Jego **Ważne** należy pamiętać, że są pobierane na podstawie całkowity czas kopiowania. Jeśli zadanie kopiowania używana do zbierania godzinę jednostki jedna chmura i teraz trwa 15 minut z czterech jednostek w chmurze, ogólnego rachunek pozostaje prawie tak samo. Na przykład możesz użyć cztery jednostki chmury. Jednostka pierwszej cloud spędza 10 minut, drugi, 10 minut, to trzecia 5 minut i jedna czwarta, 5 minut, uruchom wszystko w jednym działaniu Kopiuj. Naliczanego za usługę Czas całkowity Kopiuj (przepływ danych), który wynosi 10 + 10 + 5 + 5 = 30 minut. Przy użyciu **parallelCopies** nie wpływa na rozliczenia.

## <a name="staged-copy"></a>Kopiowanie etapowej
Podczas kopiowania danych z magazynu danych źródłowych do magazynu danych zlew, można użyć magazyn obiektów Blob jako tymczasowy magazyn tymczasowy. Organizowanie jest szczególnie przydatne w następujących przypadkach:

1.  **Aby mogły zjeść tej ostatniej danych z różnych baz danych do magazynu danych SQL za pośrednictwem PolyBase**. Magazynu danych SQL używa PolyBase w charakterze mechanizmu wysokiej przepustowości do ładowania dużych ilości danych do magazynu danych SQL. Jednak dane źródłowe muszą być w magazynie obiektów Blob i musi spełniać dodatkowe kryteria. Podczas ładowania danych z magazynu danych niż magazyn obiektów Blob, można aktywować dane kopiowanie za pośrednictwem pośrednie tymczasowy magazyn obiektów Blob. W takim przypadku Factory danych wykonuje przekształcenia wymagane dane, aby upewnić się, że spełnia wymagania PolyBase. Następnie używa PolyBase, aby załadować dane do magazynu danych SQL. Aby uzyskać więcej informacji zobacz [Używanie PolyBase, aby załadować dane do magazynu danych SQL Azure](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
2.  **Czasami zajmie trochę czasu do wykonywania przenoszenia danych hybrydowych (oznacza to, aby skopiować między danymi lokalnego przechowywanie i dane chmur przechowywać) przez połączenie sieciowe działa wolno**. Aby zwiększyć wydajność, możesz skompresować danych lokalnych, aby przyspieszyć mniej czasu, aby przenieść dane do przemieszczenia danych przechowywanych w chmurze. Następnie można wyodrębnić dane w magazynie tymczasowy przed rozpoczęciem ładowania w magazynie danych miejsca docelowego.
3.  **Nie chcesz, aby otworzyć porty inne niż port 80 i 443 w zaporze ze względu na zasad firmowych IT**. Na przykład podczas kopiowania danych z lokalnego magazynu danych do bazy danych SQL Azure zlew lub tonąć magazynu danych SQL Azure, musisz aktywować komunikacji wychodzącej TCP na porcie 1433 zarówno Zapora systemu Windows i zapory firmowej. W tym scenariuszu korzystać z bramę do pierwszej kopiowanie danych do wystąpienia tymczasowy magazyn obiektów Blob przez HTTP lub HTTPS na porcie 443. Następnie załadować dane do bazy danych SQL lub magazynu danych SQL z tymczasowego magazyn obiektów Blob. W tym przepływie nie musisz włączyć port 1433.

### <a name="how-staged-copy-works"></a>Jak etapowej kopia działa poprawnie.
Po uaktywnieniu funkcji tymczasowej najpierw dane są kopiowane z magazynu danych źródłowych w magazynie przemieszczenia danych (wyświetlić własne). Następnie dane są kopiowane z tymczasowy magazynu danych do magazynu danych sink. Dane Factory automatycznie zarządza dwuetapowy przepływ dla Ciebie. Factory danych czyści dane z magazynu tymczasowy również po zakończeniu przenoszenia danych.

Bramy nie jest używane w scenariuszu Kopiuj do chmury (źródłowego i sink dane sklepy znajdują się w chmurze). Usługi danych Factory wykonuje operacje kopii.

![Kopiowanie umieszczane: Scenariusz chmury](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

Scenariusz kopii hybrydowych (źródłem jest lokalnego i sink jest w chmurze), bramy przenosi dane z magazynu źródła danych do tymczasowej magazynu danych. Usługa Factory danych przenosi dane z tymczasowej magazynu danych do magazynu danych sink. Kopiowanie danych z magazynu danych w chmurze do lokalnego magazynu danych za pośrednictwem tymczasowego jest obsługiwane również odwróconej przepływu.

![Kopiowanie umieszczane: scenariuszy hybrydowego](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Po aktywowaniu przenoszenia danych za pomocą tymczasowy magazyn można określić, czy dane mają być kompresowane przed przeniesieniem danych z magazynu danych źródłowych do magazynu danych tymczasowych lub tymczasowy, a następnie dekompresowany przed przenoszenie danych z tymczasową lub przemieszczania magazynu danych do magazynu danych sink.

Obecnie nie można skopiować danych między dwóch zbiorów danych lokalnego przy użyciu tymczasowy magazyn. Oczekujemy tę opcję, aby wkrótce niedostępna.

### <a name="configuration"></a>Konfiguracja
Ustawienie **enableStaging** w działaniu Kopiuj, aby określić, czy dane, które mają być umieszczone w magazynie obiektów Blob przed rozpoczęciem ładowania do magazynu danych miejsca docelowego. Gdy **enableStaging** jest ustawiona na wartość PRAWDA, określ dodatkowe właściwości wymienionych w następnej tabeli. Jeśli nie masz, musisz też utworzyć magazyn Azure lub usługi połączone podpisu dostęp do przemieszczania udostępnione miejsca do magazynowania.

Właściwość | Opis | Wartość domyślna | Wymagane
--------- | ----------- | ------------ | --------
**enableStaging** | Określ, czy chcesz skopiować dane przez tymczasową przemieszczenia ze sklepu. | FAŁSZ | Brak
**linkedServiceName** | Określ nazwę [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) lub [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) połączonych usługa, która odwołuje się do wystąpienie magazynu, że użyć jako tymczasowy magazyn tymczasowy. <br/><br/> Za pomocą miejsca do magazynowania z podpisem udostępniania nie można załadować dane do magazynu danych SQL za pośrednictwem PolyBase. Można go używać we wszystkich innych sytuacjach. | N/D! | Tak, gdy **enableStaging** jest ustawiona na wartość PRAWDA.
**Ścieżka** | Określ ścieżkę magazyn obiektów Blob, która ma zawierać etapowej danych. Jeśli nie podasz ścieżkę, usługę tworzy kontenera do przechowywania danych tymczasowych. <br/><br/> Określ ścieżkę tylko wtedy, gdy używasz miejsca do magazynowania z podpisem udostępniania lub wymaga dane w określonej lokalizacji. | N/D! | Brak
**enableCompression** | Określa, czy dane powinny być kompresowany, przed skopiowaniem do miejsca docelowego. To ustawienie, co pozwala zmniejszyć objętość przesyłane dane. | FAŁSZ | Brak

Oto przykładowy definicji aktywności Kopiuj z właściwościami, które zostały opisane w powyższej tabeli:

    "activities":[  
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [{ "name": "OnpremisesSQLServerInput" }],
        "outputs": [{ "name": "AzureSQLDBOutput" }],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": "MyStagingBlob",
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
    ]


### <a name="billing-impact"></a>Wpływ rozliczeń
Są naliczane na podstawie dwie czynności: kopiowanie czas trwania i skopiować typu. 

- Gdy używasz tymczasowej podczas kopiowania chmury (kopiowanie danych z magazynu danych w chmurze do innego magazynu danych w chmurze) naliczanego za [łączny czas trwania Kopiuj kroki 1 i 2] x [Cena jednostkowa kopii chmury].
- Gdy używasz tymczasowej podczas kopiowania hybrydowych (kopiowanie danych z lokalnego magazynu danych do magazynu danych w chmurze) naliczanego za usługę [hybrydowych kopii czas trwania] x [Cena jednostkowa kopii hybrydowych] + [czas trwania kopii w chmurze] x [Cena jednostkowa kopii chmury].


## <a name="performance-tuning-steps"></a>Procedura dostosowywania wydajności
Zaleca się, że wykonać poniższe czynności, aby Dostosowywanie wydajności usługi Factory danych z działaniem kopii:

1.  **Ustanawianie planu bazowego**. Podczas fazy opracowywania przetestować planowaną za pomocą kopiowania aktywności przed próbki przedstawiciela danych. Za pomocą danych Factory [krojenie modelu](data-factory-scheduling-and-execution.md#time-series-datasets-and-data-slices) ograniczyć ilość danych, z którymi pracujesz.

    Zbieranie czas wykonywania i parametrów przy użyciu **monitorowania i zarządzania aplikacji**. Wybierz **Monitor i Zarządzaj** na stronie głównej Factory danych. W widoku drzewa wybierz **zestaw danych dane wyjściowe**. Na liście **Aktywności Windows** wybierz pozycję aktywności kopii Uruchom. **Windows aktywności** Wyświetla czas trwania działania kopii i rozmiar danych, które są kopiowane. Przepustowość znajduje się w **Eksploratorze okna aktywności**. Aby dowiedzieć się więcej o tej aplikacji, zobacz [monitorze i zarządzanie procesy Factory danych Azure za pomocą monitorowania i zarządzania aplikacji](data-factory-monitor-manage-app.md).

    ![Uruchamianie szczegóły aktywności](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

    W dalszej części tego artykułu możesz porównać wydajność i konfiguracji rozwiązania do kopiowania działania [dotyczące wydajności](#performance-reference) z testów.
2. **Diagnozowanie i optymalizowanie**. Jeśli działanie, które można obserwować nie spełnia Twoich oczekiwań, należy zidentyfikować gardła wydajności. Następnie optymalizacji wydajności, aby usunąć lub zmniejszyć efekt gardła. Pełny opis Diagnostyka wydajności wykracza poza zakres tego artykułu, ale Oto niektóre typowe zagadnienia:
    - Funkcje wydajności:
        - [Osobną kopię](#parallel-copy)
        - [Jednostki przepływu danych w chmurze](#cloud-data-movement-units)
        - [Kopiowanie etapowej](#staged-copy)   
    - [Źródła](#considerations-for-the-source)
    - [Zlew](#considerations-for-the-sink)
    - [Szeregowania i deserializacji](#considerations-for-serialization-and-deserialization)
    - [Stopień kompresji](#considerations-for-compression)
    - [Mapowania kolumn](#considerations-for-column-mapping)
    - [Brama zarządzania danymi](#considerations-for-data-management-gateway)
    - [Inne zagadnienia](#other-considerations)

3. **Rozwiń konfiguracji do całego zestawu danych**. Po zakończeniu wyniki wykonania i wydajności, można rozwinąć definicji i planowana aktywnego okresu w celu przykrycia całego zestawu danych.

## <a name="considerations-for-the-source"></a>Zagadnienia dotyczące źródła
### <a name="general"></a>Ogólne
Upewnij się, że magazyn danych nie jest przerasta inne obciążenia uruchomione przed nim. 

Dla zbiorów danych firmy Microsoft zobacz [Monitorowanie i Dostosowywanie tematy](#performance-reference) są specyficzne dla zbioru danych i pozwalają w łatwy sposób opis danych przechowywanie cech wydajności, zminimalizować czas reakcji i maksymalne zwiększenie produktywności.

Po skopiowaniu danych z magazynem obiektów Blob do magazynu danych SQL, warto rozważyć użycie **PolyBase** zwiększania wydajności. Aby uzyskać szczegółowe informacje, zobacz [Używanie PolyBase, aby załadować dane do magazynu danych SQL Azure](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) .


### <a name="file-based-data-stores"></a>Na podstawie pliku danych
*(Zawiera obiektów Blob miejsca do magazynowania, magazynu Lake danych, Amazon S3, lokalne systemy plików i HDFS lokalnego)*

- **Średni rozmiar pliku i liczba plików**: kopiowanie działanie przenosi jednego pliku danych w czasie. Z tym samym ilości danych, które mają zostać przeniesione ogólną wydajność jest niższe, jeśli dane składają się wiele małych plików zamiast kilka dużych plików z powodu Faza uruchamiania dla każdego pliku. W związku z tym jeśli to możliwe, łącząc małych plików większych plików w celu uzyskania wyższej przepustowości.
- **Format pliku i kompresji**: Aby uzyskać więcej sposobów na zwiększenie wydajności, zobacz sekcje [Zagadnienia związane z szeregowania i deserializacji](#considerations-for-serialization-and-deserialization) i [Zagadnienia związane z kompresji](#considerations-for-compression) .
- Scenariusz **lokalnego systemu plików** , w którym **Bramy zarządzania danymi** jest wymagane, znajduje się w sekcji [Zagadnienia związane z bramą zarządzania danymi](#considerations-for-data-management-gateway) .

### <a name="relational-data-stores"></a>Sklepy danych relacyjnych
*(Obejmuje bazy danych SQL. Program SQL Data Warehouse; Amazon Redshift; Bazy danych programu SQL Server; i Oracle bazy danych MySQL, DB2 Teradata, Sybase i PostgreSQL itp.)*

- **Wzorzec danych**: schemat tabeli ma wpływ na przepustowość kopii. Rozmiar dużych wiersza umożliwia lepszą wydajność niż rozmiar small wiersza, aby skopiować samej ilości danych. Przyczyną tego jest to, że bazy danych można zwiększyć wydajność pobrać mniej partiach zawierających dane, które zawierają mniej wierszy.
- **Kwerendy lub procedura składowana**: Optymalizowanie logiki kwerendy lub procedura składowana określić w źródle działania Kopiuj do pobierania danych zwiększyć wydajność.
- Dla **lokalnej relacyjnych baz danych**, takich jak program SQL Server i Oracle, który wymaga użycia **Bramy zarządzania danymi**, zobacz sekcję [Zagadnienia związane z bramą zarządzania danymi](#considerations-on-data-management-gateway) .

## <a name="considerations-for-the-sink"></a>Zagadnienia dotyczące obiekt sink

### <a name="general"></a>Ogólne
Upewnij się, że magazyn danych nie jest przerasta inne obciążenia uruchomione przed nim. 

Dla zbiorów danych firmy Microsoft zapoznaj się z [monitorowania i Dostosowywanie tematy](#performance-reference) są specyficzne dla danych. Następujące tematy mogą ułatwić Ci zrozumienie właściwości wydajności magazynu danych oraz jak zminimalizować czas reakcji i maksymalne zwiększenie produktywności.

Jeśli dane są kopiowane z **magazynem obiektów Blob** do **Magazynu danych SQL**, warto rozważyć użycie **PolyBase** zwiększania wydajności. Aby uzyskać szczegółowe informacje, zobacz [Używanie PolyBase, aby załadować dane do magazynu danych SQL Azure](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) .


### <a name="file-based-data-stores"></a>Na podstawie pliku danych
*(Obejmuje Blob miejsca do magazynowania, magazynu Lake danych Amazon S3, lokalne systemy plików i HDFS lokalnego)*

- **Zachowanie kopii**: po skopiowaniu danych z magazynu innych danych opartych na plikach kopii aktywności oferuje trzy opcje za pomocą właściwości **copyBehavior** . Zachowuje hierarchię, spłaszcza hierarchii lub Scala pliki. Zachowywanie albo spłaszczanie hierarchii zawiera niewielki lub obciążenie, ale scalania plików powoduje obciążenie zwiększyć.
- **Format pliku i kompresji**: zobacz sekcje [Zagadnienia związane z szeregowania i deserializacji](#considerations-for-serialization-and-deserialization) i [Zagadnienia związane z kompresji](#considerations-for-compression) innych metod w celu zwiększenia wydajności.
- **Magazyn obiektów blob**: obecnie obsługuje magazyn obiektów Blob blokować tylko blob przesyłania danych zoptymalizowanej i przepustowość.
- Dla scenariuszy **lokalne systemy plików** , które wymagają używania **Bramy zarządzania danymi**zobacz sekcję [Zagadnienia związane z bramą zarządzania danymi](#considerations-for-data-management-gateway) .

### <a name="relational-data-stores"></a>Sklepy danych relacyjnych
*(Obejmuje bazy danych SQL, program SQL Data Warehouse bazy danych programu SQL Server i bazy danych programu Oracle)*

- **Kopiowanie zachowania**: w zależności od właściwości ustawionych dla **sqlSink**aktywności kopii powoduje zapisanie danych do docelowej bazy danych na różne sposoby.
    - Domyślnie dane przepływu używane przez usługę interfejsu API kopii zbiorcze Wstawianie danych w dołączyć trybie, co zapewnia najlepszą wydajność.
    - Po skonfigurowaniu procedura składowana w obiekt sink bazy danych dotyczy jeden wiersz danych w czasie zamiast jako ładowanie zbiorcze. Wydajność pomija znacznie. Jeśli zestaw danych jest duży, gdy jest to właściwe, należy rozważyć przechodzenia do przy użyciu właściwości **sqlWriterCleanupScript** .
    - Po skonfigurowaniu właściwości **sqlWriterCleanupScript** dla każdego działania kopii uruchomić usługę wyzwalaczy skrypt, a następnie użyj interfejsu API kopii zbiorcze Aby wstawić dane. Na przykład aby zastąpić całą tabelę przy użyciu najnowszych danych, można określić skrypt, aby usunąć wszystkie rekordy przed zbiorczego ładowania nowych danych ze źródła.
- **Rozmiar deseniu i partii danych**:
    - Schemat tabeli ma wpływ na przepustowość kopii. Aby skopiować samej ilości danych, rozmiar dużych wiersza zapewnia lepszą wydajność niż rozmiar small wiersza ponieważ bazy danych można zwiększyć wydajność zatwierdzenie partie mniej danych.
    - Kopiowanie działanie wstawia dane w serii partii. Liczba wierszy w partii można ustawić przy użyciu właściwości **writeBatchSize** . Jeśli dane mają małe wiersze, można ustawić właściwość **writeBatchSize** z wartością nowszy do korzystania z niższe koszty partii i wyższej przepustowości. Jeśli rozmiar wiersza danych jest duży, należy zachować ostrożność Zwiększ **writeBatchSize**. Wysoka wartość może prowadzić do awarii kopii spowodowane przeciążeniem bazy danych.
- Dla **lokalnej relacyjnych baz danych** programu SQL Server i Oracle, które wymagają używania bramy **zarządzania danymi**, zobacz sekcję [Zagadnienia związane z bramą zarządzania danymi](#considerations-for-data-management-gateway) .


### <a name="nosql-stores"></a>Sklepy NoSQL
*(Obejmuje magazyn tabel i Azure DocumentDB)*

- **Magazyn tabel**:
    - **Partycją**: zapisywanie danych do przeplotem partycje znacznie obniża wydajność. Sortowanie danych źródłowych przez klucz partition tak, aby dane jest wstawiany efektywne partition jeden po drugim lub dostosować logika podczas zapisywania danych z jedną partycją.
- Dla **DocumentDB**:
    - **Rozmiar partii**: właściwość **writeBatchSize** określa liczbę równoległych żądań usługi DocumentDB do tworzenia dokumentów. Zwiększanie **writeBatchSize** , ponieważ więcej równoległe żądania są wysyłane do DocumentDB można oczekiwać lepszą wydajność. Jednak czekanie ograniczania podczas zapisywania DocumentDB (komunikat o błędzie jest "Żądanie stopa jest duży"). Różne czynniki mogą powodować ograniczania rozmiar dokumentu, w tym liczbę warunków określonych w dokumentach i docelowym zbiorze zasad indeksowania. Aby osiągnąć wyższej przepustowości Kopiuj, warto rozważyć użycie lepiej kolekcji, na przykład S3.

## <a name="considerations-for-serialization-and-deserialization"></a>Zagadnienia dotyczące szeregowania i deserializacji
Szeregowania i deserializacji mogą wystąpić w przypadku usługi wprowadzania zestawu danych lub dane wyjściowe zestawu danych jest plikiem. Kopiowanie działanie obsługuje obecnie Avro i tekst (na przykład CSV i TSV) formatów danych.

**Kopiowanie działanie**:

-   Kopiowanie plików między magazynów opartych na plikach danych:
    - Gdy wejściowe i wyjściowe zestawów danych zarówno mają takie same lub nie ustawienia formatu pliku, usługi przepływu danych wykonuje kopię binarne bez szeregowania lub deserializacji. Zobacz wyższej przepustowości w porównaniu z tego scenariusza, w którym ustawienia formatu pliku źródła i sink różnią się od siebie.
    - Podczas wprowadzania i zestawy danych dane wyjściowe obu znajdują się w formacie tekstowym i tylko kodowanie typ jest inny, usługi przepływu danych tylko oznacza Konwersja kodowania. Nie wykonaj dowolną szeregowania i deserializacji, co powoduje, że niektóre szybsze obciążenie binarne kopii.
    - Gdy wejściowe i wyjściowe zestawów danych zarówno mają różnych formatów plików lub różnych konfiguracji, takie jak ograniczniki, usługi przepływu danych deserializes danych źródłowych strumienia, przekształcanie i szeregować go do formatu wyjściowego, które należy wskazać. Operacja wynikiem wykonania bardziej znaczące obciążenie w porównaniu z innych sytuacjach.
- Po skopiowaniu plików z magazynu danych, która nie jest oparte na pliku (na przykład w sklepie opartych na plikach do sklepu relacyjnych) krok szeregowania lub deserializacji jest wymagany. Ten krok powoduje znaczące obciążenie.

**Format pliku**: Możesz wybrać format pliku mogą mieć wpływ na wydajność kopii. Na przykład Avro to niewielkie format binarny przechowuje metadane z danymi. Zadanie ma szeroki zakres obsługi w ekosystemu Hadoop przetwarzania i wykonywanie zapytań za. Jednak Avro jest bardziej drogich szeregowania i deserializacji, powstają dolnym przepustowość kopii porównaniu ich format tekstu. Wprowadź żądaną wartość formatu pliku w procesie przetwarzania całościowo. Zaczyna się od czego formularza danych jest przechowywany w sklepach danych źródłowych lub można wyodrębnić z systemów zewnętrznych; najlepszym formatem miejsca do magazynowania, przetwarzania analitycznego i wykonywanie zapytań za; i w jakim formacie dane powinny być eksportowane do marts danych dla dodatku Narzędzia do raportowania i wizualizacji. Czasami format pliku, który jest utratę jakości dla czytanie i wydajność zapisu może być dobrym rozwiązaniem, gdy można uznać ogólnej analitycznych proces.

## <a name="considerations-for-compression"></a>Zagadnienia związane z kompresji
Gdy zestaw danych wejściowe i wyjściowe znajduje się plik, można ustawić aktywności Kopiuj do wykonania kompresji lub dekompresji zapisuje dane do miejsca docelowego. Po wybraniu kompresji wprowadzeniu zależnościami między wejścia i wyjścia i Procesora. Kompresowanie dodatkowych zasobów obliczeń kosztów danych. Ale w zamian zmniejsza sieciowe We/Wy i miejsca do magazynowania. W zależności od danych może zostać wyświetlony zwiększenie wydajności w ogólnej wydajności kopii.

**Koder-dekoder**: kopiowanie działanie obsługuje gzip, bzip2 i typy kompresji Deflate. Usługa Azure HDInsight mogą używać wszystkich trzech typów przetwarzania. Każdy koder-dekoder kompresji ma pewne zalety. Na przykład bzip2 zawiera najniższe przepustowość Kopiuj, ale został wyświetlony najlepszą wydajność kwerendy gałęzi z bzip2, ponieważ możesz podzielić je do przetwarzania. Gzip jest opcja najbardziej strategii i są one najczęściej używane. Wybierz pozycję kodera-dekodera, który najlepiej pasuje do scenariusza końcu do końca.

**Poziom**: dostępne są dwie opcje dla każdego koder-dekoder kompresji: najszybciej skompresowany i optymalnie skompresowany. Najszybciej skompresowany opcja tak szybko, jak to możliwe, kompresuje dane, nawet jeśli plik wynikowy nie jest skompresowany optymalnie. Opcja optymalnie skompresowany spędza więcej czasu na kompresji i zwraca niewielkiej ilości danych. Istnieje możliwość przetestowania obu opcji, aby zobaczyć, który zapewnia lepsze ogólną wydajność w Twoim przypadku.

**Uwagę A**: Aby skopiować dużych ilości danych między magazynu w lokalnym a chmurą, warto rozważyć użycie magazyn obiektów blob pośredni kompresji. Korzystanie z magazynu pośredniego jest przydatne, gdy przepustowość sieci firmowej i usługi Azure jest współczynnik ograniczanie i chcesz zestawu danych wejściowych i zestaw danych dane wyjściowe zarówno w formularzu nieskompresowane. Dokładniej można podzielić działaniem pojedynczej kopii działania dwóch kopii. Z pierwszym działaniem Kopiuj Kopiuje ze źródła do tymczasowych lub tymczasowy obiektów blob w formularzu skompresowany. Druga aktywności Kopiuj Kopiuje skompresowane dane z tymczasowego, a następnie dekompresuje podczas zapisuje obiekt sink.

## <a name="considerations-for-column-mapping"></a>Zagadnienia związane z mapowania kolumn
Właściwość **columnMappings** można ustawić w działaniu kopii mapy wszystkie lub część wprowadzania kolumn do kolumn wyjściowych. Po usługi przepływu danych odczytuje dane ze źródła, należy podczas mapowania kolumn danych przed zapisuje dane obiekt sink. To dodatkowe przetwarzanie zmniejsza przepustowość kopii.

Jeśli przechowywanie danych źródłowych jest opcję uwzględniana w kwerendach, na przykład, jeśli jest magazynem relacyjnych, takich jak bazy danych SQL lub SQL Server lub jeśli jest w sklepie NoSQL, takich jak magazyn tabel lub DocumentDB, należy rozważyć, czy naciśnięcie filtrowanie kolumn i zmieniania kolejności logiki do właściwości **kwerendy** zamiast mapowania kolumn. W ten sposób rzut występuje, gdy usługa przepływu danych odczytuje dane z magazynu danych źródła, gdzie jest wydajniejsza.

## <a name="considerations-for-data-management-gateway"></a>Zagadnienia związane z bramą zarządzania danymi
Aby uzyskać zalecenia dotyczące konfiguracji bramy zobacz [Zagadnienia dotyczące korzystania z bramy zarządzania danymi](data-factory-move-data-between-onprem-and-cloud.md#Considerations-for-using-Data-Management-Gateway).

**Środowisko komputera bramy**: Firma Microsoft zaleca używanie dedykowanej komputera hosta bramy zarządzania danymi. Korzystanie z narzędzi, takich jak monitora sprawdzenie Procesora, pamięci i przepustowości podczas kopiowania na komputerze bramy. Przejdź do bardziej zaawansowanych komputera, jeśli Procesora, pamięci i przepustowość sieci staje się gardło.

**Równoczesne aktywności Kopiuj jest uruchomiony**: jedno wystąpienie bramy zarządzania danymi może służyć wiele kopii aktywności zostanie uruchomiona w tym samym czasie lub jednocześnie. Maksymalna liczba zadań jednocześnie jest obliczana na podstawie konfiguracji sprzętowej komputera bramy. Kopiowanie dodatkowe zadania znajduje się w kolejce do momentu ich pobrania przez bramę lub przekroczenie limitu czasu innego zadania. Aby uniknąć konfliktu zasobów na komputerze bramy, etapu harmonogramu kopii aktywności zmniejszyć liczbę kopii zadania w kolejce naraz lub należy rozważyć, czy podział obciążenia na wielu komputerach bramy.


## <a name="other-considerations"></a>Inne zagadnienia
Jeśli rozmiar danych, które chcesz skopiować jest duży, możesz dostosować logiki biznesowej na kolejne partycje danych przy użyciu mechanizmu skalowania Factory danych. Następnie Zaplanuj aktywności kopii uruchomić częściej, aby zmniejszyć rozmiar danych dla każdej czynności kopii Uruchom.

Należy zachować ostrożność liczbę zestawów danych i działania kopii wymaganie Factory danych do łącznika do samego magazynu danych, w tym samym czasie. Wielu zadań jednocześnie kopiowania mogą ograniczyć magazynu danych i czasu wyprzedzenia ograniczone wydajność, Kopiuj zadania wewnętrznych ponowne próby, a w niektórych przypadkach błędy wykonania.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Przykładowy scenariusz: kopiowanie z programu SQL Server w środowisku lokalnym magazynem obiektów Blob
**Scenariusz**: potok jest wbudowany w celu skopiowania danych z lokalnego programu SQL Server do magazyn obiektów Blob w formacie CSV. Aby przyspieszyć zadanie kopiowania, plików CSV powinien być kompresowany do formatu bzip2.

**Badania i analizy**: przepustowość działalności Kopiuj jest mniejsza niż 2 MB/s, znacznie mniejsza niż testu wydajności.

**Analiza wydajności i dostosowywanie**: Aby rozwiązać problem z wydajnością, Oto jak dane są przetwarzane i przenoszone.

1.  **Odczyt danych**: Brama otwiera połączenie z programem SQL Server i wysyła kwerendę. Program SQL Server odpowiada, wysyłając strumienia danych do bramy za pośrednictwem sieci intranet.
2.  **Dane seryjne i Kompresuj**: Brama serializes strumienia danych w formacie CSV i kompresuje dane ze strumieniem bzip2.
3.  **Zapisywanie danych**: Brama wydajnie przekazuje strumienia bzip2 magazynem obiektów Blob przez Internet.

Jak widać, dane są przetwarzane i przenoszone w sposób sekwencyjne przesyłanie strumieniowe: SQL Server > LAN > bramy > WAN > Blob miejsca do magazynowania. **Otrzymały ogólnej wydajności przez minimalną przepustowość przez proces**.

![Przepływ danych](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Co najmniej jednym z następujących czynników może spowodować gardła wydajności:

-   **Źródła**: SQL Server, sam ma niskiej przepustowości ze względu na dużym obciążeniem.
-   **Brama zarządzania danymi**:
    -   **Sieci LAN**: Brama znajduje się daleko od komputera programu SQL Server i ma połączenie niskiej przepustowości.
    -   **Brama**: bramy osiągnął swoje ograniczenia Załaduj do wykonywania następujących czynności:
        -   **Szeregowania**: szeregowania strumienia danych w formacie CSV jest powolne przepustowość.
        -   **Kompresji**: wybierzesz powolne koder-dekoder kompresji (na przykład bzip2, czyli 2,8 MB/s z Core i7).
    -   **WAN**: przepustowość połączenia między sieci firmowej i usługi Azure jest niska (na przykład T1 = 1,544 KB/s; T2 = 6,312 KB/s).
-   **Zlew**: magazyn obiektów Blob występują niskiej przepustowości. (W tym scenariuszu jest prawdopodobne, ponieważ jego SLA gwarantuje co najmniej 60 MB/s).

W tym przypadku bzip2 kompresji danych może być zmniejszają całego procesu. Przechodzenia do koder-dekoder kompresji gzip może ułatwić ten gardło.


## <a name="sample-scenarios-use-parallel-copy"></a>Przykładowe scenariusze: używanie osobną kopię  

**Scenariusz I:** Skopiuj 1000 pliki 1 MB z lokalnym systemem plików z magazynem obiektów Blob.

**Analiza i dostosowywanie wydajności**: na przykład jeśli zainstalowano bramy na komputerze cztery podstawowe Factory danych korzysta z 16 kopii równoległe Aby przenieść pliki z systemu plików do magazynu obiektów Blob jednocześnie. Ten równoległego powinno spowodować wysokiej wydajności. Można również jawnie określić liczba kopii równoległe. Po skopiowaniu wiele małych plików kopii równoległe znacznie w celu przepustowość bardziej efektywne używanie zasobów.

![Scenariusz 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Scenariusz II**: kopiowanie 20 blob 500 MB z magazynem obiektów Blob do analizy sklepu Lake danych, a następnie Dostosowywanie wydajności.

**Analiza i dostosowywanie wydajności**: W tym scenariuszu Factory danych kopiuje dane z magazynem obiektów Blob do magazynu Lake danych za pomocą jedną kopię (**parallelCopies** równa 1) i jednostki przepływu danych pojedyncze chmury. Przepustowość, które można obserwować będą zbliżony opisane w [sekcji odwołania wydajności](#performance-reference).   

![Scenariusz 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Scenariusz III**: rozmiar pojedynczego pliku jest większy niż dziesiątki MB i łącznej wielkości jest duży.

**Analiza i wydajności zmianę**: zwiększenie **parallelCopies** nie powodują lepszą wydajność kopii z powodu ograniczeń zasobów DMU pojedyncze chmury. Zamiast tego należy określić więcej chmury DMUs, aby uzyskać więcej zasobów do wykonania w przepływie danych. Nie należy określać wartości dla właściwości **parallelCopies** . Factory danych obsługuje podobieństwa dla Ciebie. W tym przypadku po ustawieniu **cloudDataMovementUnits** 4 przepustowości około cztery razy występuje.

![Scenariusz 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Odwołania
Poniżej przedstawiono monitorowania wydajności i dostosowywanie odwołania dla niektórych obsługiwanych magazynów:

- Azure magazynowania (łącznie z magazynem obiektów Blob i Magazyn tabel): [cele skalowalność magazyn Azure](../storage/storage-scalability-targets.md) i [Magazyn Azure wydajność i skalowalność — Lista kontrolna](../storage//storage-performance-checklist.md)
- Baza danych SQL Azure: Możesz [Monitorowanie wydajności](../sql-database/sql-database-service-tiers.md#monitoring-performance) i zaznacz pole wyboru procent jednostek (DTU) transakcji bazy danych
- Magazyn danych SQL Azure: Możliwości jest mierzona w jednostkach magazynu danych (DWUs); zobacz [Zarządzanie obliczyć power w magazynie danych SQL Azure (omówienie)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
- Azure DocumentDB: [Poziomy wydajności w DocumentDB](../documentdb/documentdb-performance-levels.md)
- W lokalnym program SQL Server: [Monitor i dostosować w celu uzyskania wydajności](https://msdn.microsoft.com/library/ms189081.aspx)
- Serwer plików lokalnego: [Dostosowywanie wydajności dla serwerów plików](https://msdn.microsoft.com/library/dn567661.aspx)
