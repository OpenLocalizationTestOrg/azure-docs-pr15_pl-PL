<properties
    pageTitle="Skalowalna nauki danych w Lake danych Azure: Instruktaż zakończenia do końca | Microsoft Azure"
    description="Jak używać Azure Lake danych do wykonywania zadań danych badań i binarny klasyfikacji w zestawie danych."  
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;weig"/>


# <a name="scalable-data-science-in-azure-data-lake-an-end-to-end-walkthrough"></a>Skalowalna nauki danych w Lake danych Azure: Instruktaż zakończenia do końca

W tym instruktażu przedstawiono sposób użycia Lake danych Azure badań danych i zadań binarne klasyfikacji próbki wyjazdu taksówki Warszawa i taryfy zestawu danych do przewidywania, czy etykietkę zostanie zwrócona przez taryfy. Przeprowadzi Cię przez kroki [Procesu nauki danych zespołu](http://aka.ms/datascienceprocess)końca do końca, od nabycia danych do modelu szkolenia, a następnie do wdrożenia usługi sieci web, która publikuje modelu.


### <a name="azure-data-lake-analytics"></a>Analizy Lake Azure danych

[Microsoft Azure danych Lake](https://azure.microsoft.com/solutions/data-lake/) zawiera wszystkie funkcje wymagane w celu łatwego naukowców danych do przechowywania danych, rozmiar, kształt i szybkości i przeprowadzanie przetwarzania danych, zaawansowanej analizy i maszynowego uczenia modelowania z wysoką skalowalność obciążenia.   Płatność na podstawie dla zadania, i tylko wtedy, gdy dane są faktyczną przetwarzane. Azure analizy Lake danych zawiera U SQL, w języku, który łączy deklaracyjnych rodzaj SQL z wyrazisty możliwości C# o podanie skalowalna rozmieszczona możliwości kwerendy. Umożliwia przetwarzanie danych niestrukturalne przez zastosowanie schematu odczytu, wstawianie logiki niestandardowej i funkcje zdefiniowane przez użytkownika (UDF), a zawiera rozszerzeń, aby włączyć lub dostosowywanie kolorów kontrolę nad sposobu wykonania w skali. Aby dowiedzieć się więcej na temat zasady projektu za U SQL, zobacz [wpis w blogu programu Visual Studio](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Analizy Lake danych jest również klucza część pakietu analizy Cortana i współpraca z magazynu danych SQL Azure, Power BI i Factory danych. Umożliwia pełną chmury duży danych oraz platformę zaawansowanej analizy.

W tym instruktażu zaczyna się od opisu wymagania wstępne i zasoby, które są wymagane do ukończenia zadań za pomocą analizy Lake danych, który formularza proces nauki danych i jak je zainstalować. A następnie go omówiono kroki przetwarzania danych za pomocą U SQL i nie zawiera przez przedstawiająca sposób używania Python i gałęzi z Azure maszynowego uczenia Studio do tworzenia i wdrażania przewidywanych modeli. 


### <a name="u-sql-and-visual-studio"></a>U-SQL i programu Visual Studio

W tym instruktażu zaleca edytowania skryptów U SQL przetwarzania zestawu danych przy użyciu programu Visual Studio. Opisane w tym miejscu są skrypty U SQL, opisane w osobnym pliku. Proces obejmuje ingesting, przeglądanie i próbki danych. Pokazuje także sposobu uruchamiania zadania U-SQL tworzone z portalem Azure. Gałąź tabele są tworzone dla danych w klastrze skojarzone HDInsight w celu ułatwienia konstrukcyjnych i wdrażania modelu klasyfikacji binarne Azure maszynowego uczenia Studio.  


### <a name="python"></a>Python

Ten instruktaż zawiera również sekcję, którą przedstawiono sposób tworzenia i wdrażania modelu przewidywanych Python za pomocą Azure maszynowego uczenia Studio.  Firma Microsoft udostępnia notesu Jupyter skryptów Python dla te kroki tego procesu. Notes zawiera kod kilka dodatkowych funkcji konstrukcyjną czynności i modele budowy przykład klasyfikacji multiclass i regresji modelowania oprócz model klasyfikacji binarne przedstawionych poniżej. Zadanie regresji jest przewidywanie ilość Porada na podstawie innych funkcji porada. 


### <a name="azure-machine-learning"></a>Nauka Azure komputera
Azure maszynowego uczenia Studio umożliwia tworzenie i wdrażanie przewidywanych modeli. Można to zrobić za pomocą dwóch metod: najpierw Python skryptów, a następnie z tabelami gałęzi w klastrze HDInsight (Hadoop).


### <a name="scripts"></a>Skryptów

W tym instruktażu opisano podstawowe kroki. Pełna **skrypt języka SQL U** i **Jupyter notesu** można pobrać z [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).


## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem następujące tematy musi mieć następujące czynności:

- Subskrypcję usługi Azure. Jeśli nie masz już jeden, zobacz [Uzyskiwanie Azure bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- [Zalecane] Visual Studio 2013 lub 2015 r. Jeśli nie masz już jedną z następujących wersji zainstalowany, możesz pobrać bezpłatna wersja społeczności [tutaj](https://www.visualstudio.com/visual-studio-homepage-vs.aspx). Kliknij przycisk **Pobierz 2015 społeczności** w sekcji programu Visual Studio. 

>[AZURE.NOTE] Zamiast Visual Studio umożliwia także Azure Portal przesłania kwerendy Lake danych Azure. Firma Microsoft przekazuje instrukcje o tym, jak to zrobić zarówno z programem Visual Studio i portalu w części **danych proces za pomocą U-SQL**. 

- Tworzenie konta dla Podgląd Lake Azure danych

>[AZURE.NOTE] Jest potrzebne do uzyskania zatwierdzenia korzystanie Azure danych Lake magazynu (ADLS) i Azure danych Lake analizy (ADLA), ponieważ te usługi są w podglądzie. Pojawi do utworzenia konta podczas tworzenia do pierwszej ADLS lub ADLA. Aby sigh w górę, kliknij polecenie **Utwórz konto wyświetlić podgląd**, przeczytaj umowę, a następnie kliknij **przycisk OK**. W tym miejscu na przykład jest znak ADLS stronę w górę:

 ![2](./media/machine-learning-data-science-process-data-lake-walkthrough/2-ADLA-preview-signup.PNG)


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Przygotowywanie danych środowiska nauki dla Lake danych Azure
Aby przygotować środowisko nauki danych dla tego instruktażu, utwórz następujące zasoby:

- Azure magazynu Lake danych (ADLS) 
- Analizy Lake Azure danych (ADLA)
- Konto Azure magazyn obiektów Blob
- Konto komputera nauki Studio Azure
- Narzędzia Lake Azure danych dla programu Visual Studio (zalecane)

Ta sekcja zawiera instrukcje dotyczące tworzenia każdy z tych zasobów. Jeśli zdecydujesz się na użycie gałąź tabele z Azure maszynowego uczenia, zamiast Python, tworzenia modelu, również należy obsługi administracyjnej klaster HDInsight (Hadoop). Tę procedurę alternatywnych w opisane w odpowiedniej sekcji poniżej.
<br/>
>AZURE. Uwaga **Magazynu Lake Azure danych** można tworzyć zarówno oddzielnie lub po utworzeniu **Azure danych Lake analizy** jako magazyn domyślny. Odwołuje się instrukcje dotyczące tworzenia każdy z tych zasobów oddzielnie poniżej, ale konto miejsca do magazynowania danych Lake potrzebujesz nie można utworzyć oddzielnie.
<br/>
### <a name="create-an-azure-data-lake-store"></a>Tworzenie magazynu Lake danych Azure

Tworzenie ADLS [Azure Portal](http://portal.azure.com). Aby uzyskać szczegółowe informacje zobacz [Tworzenie klastrze HDInsight z magazynu Lake danych przy użyciu Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Pamiętaj skonfigurować tożsamość AAD klaster w karta **źródła danych** : karta **Opcjonalnym** opisane tam. 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)


### <a name="create-an-azure-data-lake-analytics-account"></a>Utwórz konto Azure danych Lake analizy
Tworzenie konta ADLA [Azure Portal](http://portal.azure.com). Aby uzyskać szczegółowe informacje, zobacz [Samouczek: wprowadzenie do analiz Lake danych Azure za pomocą Azure Portal](../data-lake-analytics/data-lake-analytics-get-started-portal.md). 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)


### <a name="create-an-azure-blob-storage-account"></a>Tworzenie konta magazyn obiektów Blob platformy Azure
Tworzenie konta magazyn obiektów Blob platformy Azure [Azure Portal](http://portal.azure.com). Aby uzyskać szczegółowe informacje Zobacz Tworzenie sekcji konta miejsca do magazynowania w [o Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md).
    
 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)


### <a name="set-up-an-azure-machine-learning-studio-account"></a>Konfigurowanie konta usługi Azure maszynowego uczenia Studio
Zaloguj się w górę/w Azure maszynowego uczenia Studio ze strony [Azure maszynowego uczenia](https://azure.microsoft.com/services/machine-learning/) . Kliknij przycisk **Rozpocznij teraz** , a następnie wybierz pozycję "Wolnego obszaru roboczego" lub "Standardowy obszar roboczy". Dzięki temu będzie można tworzyć doświadczenia w Azure ML Studio.  

### <a name="install-azure-data-lake-tools-recommended"></a>Instalowanie narzędzia Lake Azure danych [zalecane]
Zainstaluj narzędzia Lake danych Azure dla używanej wersji programu Visual Studio z [Narzędzia Lake Azure danych dla programu Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Po pomyślnym zakończeniu instalacji otwarcia programu Visual Studio. Powinien zostać wyświetlony Lake danych, na karcie menu u góry. Po zalogowaniu się do konta usługi Azure Azure zasobów powinien być wyświetlany w panelu po lewej stronie.

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)


## <a name="the-nyc-taxi-trips-dataset"></a>Warszawa taksówki podróży zestawu danych
Zestaw danych, który użyliśmy w tym miejscu jest publicznie zestawu danych — [Warszawa taksówki podróży zestawu danych](http://www.andresmh.com/nyctaxitrips/). Dane podróży taksówki Warszawa składa się z około 20GB skompresowane pliki CSV (~ 48GB nieskompresowane), rejestrowanie więcej niż miliona 173 pojedynczych podróży i opłaty opłacona każdej podróży. Każdy podróży rekord zawiera lokalizacje pobrania i przyjmowania i godzin, zastąpione anonimowymi próba złamania (sterownik) liczbę licencji i liczba Medalionu (taksówki i unikatowy identyfikator). Dane obejmuje wszystkie podróży w roku 2013 i znajduje się w następujących dwa zestawy danych dla każdego miesiąca:

 - "Trip_data" CSV zawiera szczegóły podróży, takie jak liczba osób, pobrania i dropoff punktów, czas trwania podróży i długość podróży. Poniżej przedstawiono kilka przykładowych rekordów:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

 - Trip_fare CSV zawiera szczegóły dotyczące taryfy za każdym razem, takie jak typ płatności, kwota taryfy, opłaty dodatkowej i podatki, porady i drogowe i całkowitą kwotę zapłaconą. Poniżej przedstawiono kilka przykładowych rekordów:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Unikatowy klucz, aby dołączyć do podróży\_danych i podróży\_taryfy składa się z trzech poniższych pól: Medalionu, próba złamania\_licencji oraz odbiór\_daty i godziny. Nieprzetworzonych plików CSV są dostępne z blob publicznej Azure przestrzeni dyskowej. Skrypt języka SQL U dla tego sprzężenia znajduje się w sekcji [Dołącz tabele podróży i taryfy](#join) .

## <a name="process-data-with-u-sql"></a>Przetwarzanie danych za pomocą U SQL

Zadania przetwarzania danych przedstawione w tej sekcji obejmują ingesting, sprawdzanie jakości Eksplorowanie i próbki danych. Zostanie wyświetlona również sposób łączenia tabel podróży i taryfy. Ostateczne sekcji przedstawiono uruchamianie skryptów zadanie U SQL z portalu usługi Azure. Poniżej przedstawiono linki do każdego podsekcji:

- [Spożyciu danych: odczytywanie danych z publicznej obiektów blob](#ingest)
- [Sprawdzanie jakości danych](#quality)
- [Poszukiwanie danych](#explore)
- [Łączenia tabel podróży i taryfy](#join)
- [Pobierania danych](#sample)
- [Wykonywanie zadań U SQL](#run)

Opisane w tym miejscu są skrypty U SQL, opisane w osobnym pliku. Pełna **skrypty U SQL** można pobrać z [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

Aby wykonaj instrukcję SQL U, otwórz Visual Studio, kliknij pozycję **pliku--> Nowy--> projektu**, wybierz pozycję **U SQL projektu**, nazwę i zapisz je do folderu.

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

>[AZURE.NOTE] Istnieje możliwość wykonaj instrukcję SQL U zamiast programu Visual Studio za pomocą Azure Portal. Można przejść do zasobu Azure danych Lake analizy w portalu i przesłać kwerendy bezpośrednio jako ilustrowany na poniższym rysunku.

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Spożyciu danych: Odczytywanie danych z publicznej obiektów blob

Lokalizacja danych w obiektów blob platformy Azure przez odwołanie pełni funkcję **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** i wyodrębnionych za pomocą **Extractors.Csv()**. Podstaw własną nazwę kontenera i nazwę konta magazynu w następujących skryptów dla container_name@blob_storage_account_name w adresie wasb. Ponieważ nazwy plików w formacie, firma Microsoft korzysta **podróży\_data_ {\*\}CSV** w wszystkie pliki 12 podróży. 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Ponieważ w pierwszym wierszu znajdują się nagłówki, trzeba usunąć nagłówki i zmień typy kolumn do odpowiedniego z nich. Możemy albo zapisz przetworzone dane do przechowywania Lake danych Azure za pomocą **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ lub magazyn obiektów Blob platformy Azure konta przy użyciu **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**. 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

Podobnie firma Microsoft może czytać w klasie zbiorach danych. Kliknij prawym przyciskiem myszy magazynu Lake danych Azure, można przeglądać dane w **Azure Portal--> Eksplorator danych** lub **Eksploratora plików** w programie Visual Studio. 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)


### <a name="quality"></a>Sprawdzanie jakości danych

Po podróży i taryfy tabele zostały przeczytane w, kontrola jakości danych można zrobić w następujący sposób. Otrzymane pliki CSV mogą być dane wyjściowe magazyn obiektów Blob platformy Azure lub magazynu Lake danych Azure. 

Znajdowanie liczby medallions i unikatowy numer medallions:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;
    
    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

Znajdź tych medallions, które miały więcej niż 100 podróży:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

Znajdź rekordy nieprawidłowe pod względem pickup_longitude:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

Znajdź brakujące wartości dla niektórych zmiennych:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;
    
    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Poszukiwanie danych

Możemy niektóre badań danych w celu uzyskania lepszego zrozumienia danych.

Znajdź rozkład Przechylony i Przechylony podróży:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;
    
    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

Znajdowanie rozkład kwoty Porada z wartościami granicy: 0,5,10 i 20 zł.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

Znajdowanie statystyki podstawowe odległość podróży:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Znajdź percentylu odległość podróży:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <a name="join"></a>Łączenia tabel podróży i taryfy

Tabele podróży i taryfy można łączyć Medalionu, hack_license i pickup_time.

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


Dla każdego poziomu liczba osób obliczyć liczbę rekordów, kwota Porada średnia, odchylenie Porada kwoty, procent Przechylony podróży.

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Pobierania danych

Firma Microsoft losowo zaznacz najpierw 0,1% dane z tabeli sprzężonych:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;
    
    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;
    
    OUTPUT @model_data_random_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

Firma Microsoft wykonaj stratyfikowana przy próbkowaniu przez dwójkową tip_class zmiennych:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;
    
    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <a name="run"></a>Wykonywanie zadań U SQL

Po zakończeniu edycji skryptów U SQL, można przesłać je na serwerze przy użyciu konta usługi Azure danych Lake analizy. Kliknij pozycję Wybierz **Lake danych**, **Przesyłanie zadania** **Konta analizy**, wybierz **równoległości**, a następnie kliknij przycisk **Prześlij** .  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

Gdy zadanie jest spełnione pomyślnie, stan zadania pojawi się w programie Visual Studio do monitorowania. Po zakończeniu zadania można nawet powtarzania procesu wykonanie zadania i Dowiedz się, czynności gardło, aby zwiększyć wydajność pracy. Możesz również przejść do Portal Azure, aby sprawdzić stan zadań U-SQL.

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)


 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)


Teraz możesz sprawdzić pliki wyjściowe magazyn obiektów Blob platformy Azure lub Azure Portal. Użyjemy stratyfikowana przykładowych danych dla naszych modelowanie w następnym kroku.

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)


## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Tworzenie i wdrażanie modeli w nauki maszynowego Azure

Firma Microsoft wykazać dwie opcje dostępne na wciąganie danych do nauki maszynowego Azure do utworzenia i 

- W opcji pierwszy za pomocą próbki dane, które zostały zapisane w obiektów Blob platformy Azure (w poprzednim kroku **pobierania danych** ) i Python umożliwia tworzenie i wdrażanie modeli z Azure maszynowego uczenia. 
- W drugiej opcji, możesz kwerendy danych w Lake danych Azure bezpośrednio przy użyciu kwerendy gałęzi. Ta opcja wymaga Utwórz nowy klaster HDInsight, lub użyj istniejącego klaster HDInsight miejsce, w którym tabele gałęzi wskaż pozycję dane taksówki NY w magazynie Lake danych Azure.  Omówimy obu tych opcji poniżej. 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Opcja 1: Użyj Python do tworzenia i wdrażania komputera modeli szkoleniowe

Do tworzenia i wdrażania modeli nauki komputera przy użyciu Python, Utwórz notes Jupyter na komputerze lokalnym lub Azure maszynowego uczenia Studio. Notes Jupyter opisane na [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) zawiera pełny kod Eksplorowanie, wizualizowanie danych, funkcja techniczny, modelowania i wdrożenia. W tym artykule zostanie wyświetlona tylko modelowanie i wdrożenia. 

### <a name="import-python-libraries"></a>Importowanie Python bibliotek

Aby uruchomić przykład Notes Jupyter lub Python skrypt pliku, następujące Python, pakiety są wymagane. Jeśli korzystasz z usługi AzureML notesu, te pakiety zostały zainstalowany.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>Odczytywanie danych z obiektów blob

- Parametry połączenia   

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    
- Przeczytaj w postaci tekstu

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

 ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
 
- Dodawanie nazw kolumn i oddzielić kolumn

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
    


- Zmiana niektórych kolumn na liczbowe

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Tworzenie modeli nauki komputera

W tym miejscu budowy model klasyfikacji binarne do przewidywania, czy wycieczki jest Przechylony lub nie. W notesie Jupyter znajdują się inne dwóch modeli: multiclass klasyfikacji i modeli regresji.

- Najpierw należy utworzyć fikcyjny czynników, które mogą być używane w scikit — informacje modeli

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')

- Tworzenie ramki danych modelowania

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
        
        X = data.iloc[:,1:]
        Y = data.tipped

- Szkoleń i testowania 60 40 podziału

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)

- Regresją w zestawie szkolenia

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)

- Wynik badania zestawu danych

        Y_test_pred = logit_fit.predict(X_test)

- Obliczanie metryki oceny

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
        
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
        
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
        
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)


 
### <a name="build-web-service-api-and-consume-it-in-python"></a>Tworzyć interfejs API usług sieci Web i wykorzystywać go w Python

Chcemy operationalize maszynowego uczenia modelu po został utworzony. W tym miejscu używamy modelu logistyczne binarnego jako przykład. Upewnij się, scikit-informacje 0.15.1 jest wersja na komputerze lokalnym. Nie musisz martwić się o to, jeśli jest używana usługa studio Azure ML.

- Znajdź swoje poświadczenia obszaru roboczego z poziomu strony ustawień studio Azure ML. W Azure maszynowego uczenia Studio, kliknij pozycję **Ustawienia** --> **Nazwa** --> **Tokeny autoryzacji**. 

    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)


        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

- Tworzenie usługi sieci Web

        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)

- Uzyskaj poświadczenia usługi sieci web

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
        
        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass

- Zadzwoń interfejs API usług sieci Web. Trzeba czekać 5-10 sekund po poprzednim kroku.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)


## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Opcja 2: Tworzenie i wdrażanie modeli bezpośrednio w nauki maszynowego Azure

Azure Studio nauki Machine może odczytać dane bezpośrednio z magazynu Lake Azure danych, a następnie można użyć do tworzenia i wdrażanie modeli. Tej metody używa tabeli gałęzi, która wskazuje w sklepie Lake Azure danych. Wymaga to, że osobnych klaster Azure HDInsight być obsługi administracyjnej, na jest tworzona tabela gałęzi. W poniższych sekcjach przedstawiono jak to zrobić. 

### <a name="create-an-hdinsight-linux-cluster"></a>Utwórz klaster Linux HDInsight

Tworzenie [Azure Portal](http://portal.azure.com)HDInsight klaster (Linux). Aby uzyskać szczegółowe informacje Zobacz sekcję **Utwórz klaster HDInsight z dostępem do magazynu Lake danych Azure** się [Utwórz klaster HDInsight z magazynu Lake danych przy użyciu Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Tworzenie tabeli gałęzi w HDInsight

Teraz możemy utworzyć gałąź Tabele może być używany w Azure maszynowego uczenia Studio w klastrze HDInsight przy użyciu danych przechowywanych w magazynie Lake danych Azure w poprzednim kroku. Przejdź do klastrów HDInsight właśnie utworzony. Kliknij pozycję **Ustawienia** --> **Właściwości** --> **Tożsamości AAD klaster** --> **ADLS dostępu**, upewnij się, konto Azure magazynu Lake danych zostanie dodane listy z odczytu, zapisu i wykonywania praw. 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)


Następnie kliknij pozycję **pulpit nawigacyjny** obok przycisku **Ustawienia** i oknie wyskakującym zostanie. Kliknij kartę **Widok gałęzi** w prawym górnym rogu strony, a zostanie wyświetlony **Edytor zapytań**.

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)


Wklej następujące skrypty gałęzi, aby utworzyć tabelę. Źródło danych znajduje się w odwołaniu magazynu Lake danych Azure w ten sposób: **adl://data_lake_store_name.azuredatalakestore.net:443/nazwa_folderu/nazwa_pliku**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Po zakończeniu kwerendy pojawią się wyniki w następujący sposób:

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)



### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Tworzenie i wdrażanie modeli w Azure maszynowego uczenia Studio

Teraz możemy przystąpić do tworzenia i wdrażania modelu, który wskazuje, czy etykietkę zapłacono za pomocą Azure maszynowego uczenia. Stratyfikowana przykładowych danych jest gotowa do użycia w klasyfikacji binarne (Porada lub nie) problem. Modele przewidywanych przy użyciu multiclass klasyfikacji (tip_class) i regresji (tip_amount) można także wbudowane i wdrażane Azure maszynowego uczenia Studio, ale tutaj możemy wyświetlić tylko sposobu obsługi wielkości liter przy użyciu modelu klasyfikacji binarne.

1. Pobieranie danych do Azure ML przy użyciu modułu **Importowanie danych** dostępnych w sekcji **dane wejściowe i wyjściowe** . Aby uzyskać więcej informacji zobacz stronę odwołania [modułu importowanie danych](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) .
2. Umożliwia wybranie **Gałęzi kwerendy** jako **źródła danych** w panelu **Właściwości** .
3. Wklej następujący skrypt gałąź w edytorze **gałęzi zapytania bazy danych**

        select * from nyc_stratified_sample;

4. Wprowadź identyfikator URI HDInsight klaster (ten znajduje się w portalu Azure), Hadoop poświadczeń, lokalizację danych wyjściowych i nazwę kontenera kluczy i nazwę konta magazynu Azure.

 ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

Przykład doświadczenia binarne klasyfikacji odczytywania danych z gałęzi tabeli przedstawiono na poniższej ilustracji.

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

Po utworzeniu doświadczenia kliknij **Konto usługi sieci Web** --> **Przewidywanych usługi sieci Web**

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

Uruchamianie tworzonych automatycznie wyników doświadczenia, po zakończeniu kliknij przycisk **Wdrażanie usługi sieci Web**

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Wkrótce będą wyświetlane na pulpicie nawigacyjnym usługi sieci web:

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)


## <a name="summary"></a>Podsumowanie

Kończenie pracy w tym instruktażu utworzono środowisku nauki dane dotyczące tworzenia skalowalna rozwiązań zakończenia do końca w Lake danych Azure. To środowisko został użyty do analizowania dużych publicznej zestawu danych, zabierz kanonicznych kroki procesu nauki danych z uzyskiwanie danych poprzez szkolenie model, a następnie do wdrożenia modelu jako usługi sieci web. U SQL został użyty do procesu, eksplorowanie i przykładowe dane. Azure maszynowego uczenia Studio Python i gałęzi były używane do tworzenia i wdrażania przewidywanych modeli.

## <a name="whats-next"></a>Co to jest dalej?

Ścieżka nauki dla [Zespołu danych nauki proces (TDSP)](http://aka.ms/datascienceprocess) zawiera łącza do tematów opisujących każdego kroku procesu zaawansowanej analizy. Istnieje szereg instruktaży wyszczególnione na stronie [instruktaży proces nauki danych zespołu](data-science-process-walkthroughs.md) , który zaprezentowania sposobu użycia zasobów i usług w różnych scenariuszach przewidywanych analizy:

- [Proces zespołu danych naukowych w działaniu: przy użyciu magazynu danych SQL](machine-learning-data-science-process-sqldw-walkthrough.md)
- [Proces zespołu danych naukowych w działaniu: używania klastrów HDInsight Hadoop](machine-learning-data-science-process-hive-walkthrough.md)
- [Proces zespołu danych nauki: przy użyciu programu SQL Server](machine-learning-data-science-process-sql-walkthrough.md)
- [Omówienie procesu nauki danych za pomocą Spark na Azure HDInsight](machine-learning-data-science-spark-overview.md)

