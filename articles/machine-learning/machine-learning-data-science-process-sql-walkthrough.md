<properties
    pageTitle="Proces nauki danych zespołu w akcji: przy użyciu programu SQL Server | Microsoft Azure"
    description="Zaawansowane narzędzia analityczne procesu pracy i technologii w akcji"  
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
    ms.author="fashah;bradsev"/>


# <a name="the-team-data-science-process-in-action-using-sql-server"></a>Proces nauki danych zespołu w akcji: przy użyciu programu SQL Server

W tym samouczek, możesz instruktażu budowania i wdrażania modelu uczenie maszynowe przy użyciu programu SQL Server i publicznie dostępnych dataset-- [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) zestawu danych. Procedura następuje przepływ pracy naukowej standardowych danych: odbieranie i eksplorować dane funkcje inżynierskie do ułatwienia nauki, a następnie tworzenia i wdrażania modelu.


## <a name="dataset"></a>NYC Taxi wycieczki opis zestawu danych

Dane podróży taksówką NYC wynosi około 20GB skompresowanych plików CSV (~ 48GB bez kompresji), składającą się z więcej niż 173 mln pojedynczych podróży i taryfami zapłacił za każdym rejsie. Każdy rekord podróży zawiera lokalizację odbioru i zwrotu i czas, hack anonimowych (sterownik) numer licencji i numer medalion (taksówki Unikatowy identyfikator). Dane obejmuje wszystkich podróży w roku 2013 i znajduje się w następujących dwóch zestawów danych dla każdego miesiąca:

1. "Trip_data" CSV zawiera szczegóły podróży, takie jak liczba pasażerów, odbioru i punkty porzucania, czas trwania podróży i długość podróży. Oto kilka przykładowych rekordów:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. "trip_fare" CSV zawiera szczegółowe informacje o opłacie uiszczonej dla każdego rejsu, takie jak typ płatności, kwota opłaty za przejazd, opłatą i podatki, porady i opłaty, a całkowitą kwotę zapłaconą. Oto kilka przykładowych rekordów:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Unikatowy klucz do przyłączenia podróży\_danych i podróży\_opłata składa się z pól: medalion hack\_licencji i odbiór\_datetime.

## <a name="mltasks"></a>Przykłady zadań przewidywania

Firma Microsoft będzie formułować trzy problemy przewidywania na podstawie *końcówki\_kwoty*, a mianowicie:

1. Klasyfikacja binarna: przewidzieć, czy etykietka poniesiony na wycieczkę, tj *końcówki\_kwoty* większą niż 0 zł jest pozytywny przykład, podczas gdy *Wskazówka\_kwoty* $ 0 jest negatywny przykład.

2. Klasyfikacja wspólnotowy: do przewidzenia zakres wskazówka wypłacane na czas podróży. Można podzielić *końcówki\_kwota* na pięć pojemników lub klas:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. Zadanie regresji: aby przewidzieć ilość wskazówka wypłacane w podróży.  


## <a name="setup"></a>Ustawienie Up the Azure środowiska nauki danych do przeprowadzania zaawansowanych analiz

Jak widać z przewodnika [Planu swojego środowiska](machine-learning-data-science-plan-your-environment.md) , istnieje kilka opcji do pracy z zestawu danych NYC Taxi wycieczki w Azure:

- Praca z danymi w Azure plamy następnie model analityków
- Ładowanie danych do bazy danych SQL Server, a następnie model analityków

W tym samouczku pokażemy import zbiorczy równolegle danych do programu SQL Server, eksploracji danych, funkcja inżynierii i próbkowania w dół za pomocą programu SQL Server Management Studio, a także za pomocą Optymalizowanie. [Przykładowe skrypty](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) i [IPython notebooki](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) są współużytkowane w GitHub. Optymalizowanie próbki do pracy z danymi w obiektach Azure blob jest również dostępna w tej samej lokalizacji.

Do konfigurowania środowiska nauki danych Azure:

1. [Utwórz konto magazynu](../storage/storage-create-storage-account.md)

2. [Tworzenie obszaru roboczego z usługą sieci Web](machine-learning-create-workspace.md)

3. [Przepis maszyny wirtualnej nauki danych](machine-learning-data-science-setup-sql-server-virtual-machine.md), która będzie służyć jako serwer SQL, a także Optymalizowanie serwera.

    > [AZURE.NOTE] Przykładowe skrypty i notebooki IPython zostaną pobrane z maszyną wirtualną nauki danych podczas procesu instalacji. Po zakończeniu działania skryptu po instalacji maszyny Wirtualnej, próbki będą w bibliotece dokumentów z maszyny Wirtualnej:  
    > - Przykładowe skrypty:`C:\Users\<user_name>\Documents\Data Science Scripts`  
    > - Notebooki IPython próbki:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
    > gdy `<user_name>` jest nazwą logowania systemu Windows sieci maszyny Wirtualnej. Odnoszą się do folderów próbki jako **Przykładowe skrypty** i **Notebooki IPython próbki**.


Na podstawie wielkości zbioru danych, lokalizacja źródła danych i środowiska docelowego wybranego Azure, w tym scenariuszu jest podobny do [scenariusza \#5: dużego zestawu danych w plikach lokalnych, docelowe w Azure VM SQL Server](../machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Pobierz dane z publicznych źródła

Aby uzyskać dataset [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) z jego lokalizacji publicznej, może użyć jedną z metod opisanych w [Przenoszenie danych do i z magazynu obiektów Blob Azure](machine-learning-data-science-move-azure-blob.md) Aby skopiować dane do sieci nowej maszyny wirtualnej.

Aby skopiować dane przy użyciu AzCopy:

1. Zaloguj się na maszynie wirtualnej (VM)

2. Utwórz nowy katalog dysku danych maszyny Wirtualnej (Uwaga: nie należy używać dysku tymczasowym, który pochodzi z maszyną Wirtualną jako dysk danych).

3. W oknie wiersza polecenia Uruchom następujący wiersz polecenia Azcopy, zastępując < path_to_data_folder > folderu dane utworzone w (2):

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

    Po zakończeniu AzCopy łącznie 24 spakowane pliki CSV (12 dla podróży\_danych i 12 dla podróży\_taryfy) powinny znajdować się w folderze dane.

4. Rozpakuj pobrane pliki. Uwaga folder, w którym znajdują się pliki nieskompresowane. Ten folder będzie nazywać < ścieżka\_do\_danych\_plików\>.

## <a name="dbload"></a>Zbiorcze importowanie danych do bazy danych programu SQL Server

Za pomocą _podzielonym na partycje tabel i widoków_można poprawić wydajność ładowania/przesyłania dużych ilości danych do bazy danych SQL i następne kwerendy. W tej sekcji pójdziemy instrukcje opisane w [Równoległych zbiorczego importu za pomocą SQL partycji tabele danych](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) , Utwórz nową bazę danych i ładowanie danych do tabel podzielonych na partycje równolegle.

1. Logując się do maszyny Wirtualnej, należy uruchomić **Program SQL Server Management Studio**.

2. Połącz przy użyciu uwierzytelniania systemu Windows.

    ![Podłącz SSMS][12]

3. Jeśli jeszcze nie masz zmienić tryb uwierzytelniania programu SQL Server i utworzyć nowego użytkownika logowania SQL, otwórz plik skryptu o nazwie **zmienić\_auth.sql** w folderze **Przykładowe skrypty** . Zmienić domyślną nazwę użytkownika i hasło. Kliknij **! Wykonanie** na pasku narzędziowym, aby uruchomić skrypt.

    ![Wykonaj skrypt][13]

4. Sprawdź i/lub zmiany programu SQL Server domyślny bazy danych i dziennika folderów do zapewnienia nowo utworzone baz danych będą przechowywane w danych na dysku. Obraz maszyny Wirtualnej programu SQL Server, który jest zoptymalizowany pod kątem obciążeń hurtownie danych jest wstępnie skonfigurowany z dyskami danych i dziennika. Jeśli maszynę Wirtualną nie zawiera danych na dysku i podczas procesu instalacji maszyny Wirtualnej dodano nowe wirtualne dyski twarde, zmień domyślne foldery w następujący sposób:

    - Kliknij prawym przyciskiem myszy nazwę serwera SQL w panelu po lewej stronie, a następnie kliknij polecenie **Właściwości**.

        ![Właściwości: SQL Server][14]

    - Wybierz **Ustawienia bazy danych** z listy **Wybierz stronę** w lewo.

    - Sprawdź i/lub zmienić **Lokalizacje domyślne bazy danych** do lokalizacji **Dysku danych** wybranych przez użytkownika. Jest to, gdzie nowe bazy danych znajdują się, jeśli utworzony z domyślnymi ustawieniami lokalizacji.

        ![Ustawienia domyślne bazy danych SQL][15]  

5. Aby utworzyć nową bazę danych i zestaw grup plików do przechowywania tabel podzielonych na partycje, Otwórz przykładowy skrypt **utworzyć\_db\_default.sql**. Skrypt utworzy nową bazę danych o nazwie **TaxiNYC** i 12 aplikacjami w domyślnej lokalizacji danych. Każda grupa plików będzie zawierać jednego miesiąca wyjazdu\_danych i podróży\_taryfy danych. W razie potrzeby zmodyfikuj nazwę bazy danych. Kliknij **! Wykonanie** do uruchomienia skryptu.

6. Następnie należy utworzyć dwie tabele partycji, jeden dla podróży\_danych i drugi dla podróży\_taryfy. Otwórz przykładowy skrypt **utworzyć\_podzielonym na partycje\_table.sql**, która będzie:

    - Utworzyć funkcję partycji, aby podzielić dane według miesiąca.
    - Tworzenie partycji schematu mapowania danych każdego miesiąca do innej grupy plików.
    - Tworzenie dwóch tabel partycjonowanych mapowane do schematu partycji: **nyctaxi\_podróży** odbędzie podróży\_danych i **nyctaxi\_taryfy** odbędzie podróży\_taryfy danych.

    Kliknij **! Wykonanie** do uruchomienia skryptu i tworzenia tabel podzielonych na partycje.

7. W folderze **Przykładowe skrypty** są dwa przykładowe skrypty środowiska PowerShell dostarczane do wykazania, że przywóz równoległy masowych danych do tabel programu SQL Server.

    - **bcp\_równoległe\_generic.ps1** jest skrypt rodzajowy równoległego importu danych do tabeli. Zmodyfikuj ten skrypt, aby ustawić zmienne wejściowe i miejsce docelowe, jak wskazano w wiersze komentarza w skrypcie.
    - **bcp\_równoległe\_nyctaxi.ps1** jest wstępnie skonfigurowana wersja skryptu rodzajowego i może służyć do załadować obie tabele danych NYC Taxi Trips.  

8. Kliknij prawym przyciskiem myszy **bcp\_równoległe\_nyctaxi.ps1** nazwę skryptu i kliknij przycisk **Edytuj** , aby otworzyć go w programie PowerShell. Przeglądanie predefiniowanych zmiennych i modyfikować w zależności od wybranej nazwie bazy danych, folder dane wejściowe, cel folder dziennika i ścieżek do plików formatu próbki **nyctaxi_trip.xml** i **nyctaxi\_fare.xml** (dostarczone w folderze **Przykładowe skrypty** ).

    ![Zbiorcze importowanie danych][16]

    Można także wybrać tryb uwierzytelniania, domyślną jest uwierzytelnianie systemu Windows. Kliknij zieloną strzałkę w pasku narzędzi, aby uruchomić. Skrypt zostanie uruchomiony 24 operacji importu zbiorczego w 12 równoległego, dla każdej tabeli podzielonej na partycje. Otwierając domyślnego folderu danych programu SQL Server jako powyżej może monitorować postęp importu danych.

9. Skrypt programu PowerShell raporty godziny rozpoczęcia i zakończenia. Jeśli wszystkie masowe przywozu pełną, godzina zakończenia jest zgłaszane. Wyboru folderu docelowego dziennika, aby zweryfikować, że większość przywozu były udane, czyli bez błędów zgłaszane w folderze docelowym dziennika.

10. Bazy danych jest gotowy na badanie, funkcja inżynierii lądowej i wodnej i innych czynności. Ponieważ tabele są podzielone na partycje zgodnie z **pickup\_datetime** pola kwerendy, które obejmują **pickup\_datetime** warunki określone w klauzuli **gdzie** będą korzystać z schemat partycji.

11. W **Programie SQL Server Management Studio**, należy zbadać skrypt dostarczona próbka **próbki\_queries.sql**. Aby móc uruchomić każdy z kwerendy przykładowe, Podświetl wiersze zapytań, następnie kliknij **! Wykonanie** na pasku narzędziowym.

12. Załadowaniu danych NYC Taxi Trips w dwóch osobnych tabelach. Aby poprawić operacji sprzężenia, zalecane jest do indeksowania w tabelach. Przykładowy skrypt **utworzyć\_podzielonym na partycje\_index.sql** tworzy indeksy podzielone na partycje na kluczu sprzężenia kompozytowe **medalion hack\_licencji i odbiór\_datetime**.

## <a name="dbexplore"></a>Eksploracji danych i inżynierii funkcji w programie SQL Server

W tej sekcji będziemy wykonywać eksploracji danych i generowania funkcji wykonywania kwerend SQL bezpośrednio w **Programie SQL Server Management Studio** za pomocą bazy danych programu SQL Server, utworzony wcześniej. Przykładowy skrypt o nazwie **próbki\_queries.sql** znajduje się w folderze **Przykładowe skrypty** . Zmodyfikuj skrypt, aby zmienić nazwę bazy danych, jeśli jest inny niż domyślny: **TaxiNYC**.

W tym ćwiczeniu zostaną wykonane:

- Podłącz do **Programu SQL Server Management Studio** za pomocą albo uwierzytelniania systemu Windows lub uwierzytelniania programu SQL i nazwa logowania SQL i hasło.
- Poznaj dystrybucji danych kilka pól w różnym czasie systemu windows.
- Zbadać jakość danych pól szerokości i długości geograficznej.
- Generowanie etykiet binarne i wspólnotowy klasyfikacji na podstawie **końcówki\_kwoty**.
- Generowanie funkcje i obliczeń/porównanie odległości podróży.
- Sprzężenia dwóch tabel i ekstrahować losowo, która posłuży do budowania modeli.

Gdy zechcesz przejść do usługą sieci Web, użytkownik może:  

1. Zapisz kwerendę SQL końcowego wyodrębnić i przykładowe dane i Kopiuj wklej kwerendy bezpośrednio do [Importowania danych] [ import-data] moduł analityków, lub
2. Utrwalić planujesz używać dla modelu budynku w nowej tabeli bazy danych oraz nowej tabeli [Danych importu] danych włączonych do próby statystycznej i sztucznymi[ import-data] moduł analityków.

W tej sekcji są zapisywane końcowe kwerendę, aby wyodrębnić i przykładowe dane. Druga metoda jest wykazane w sekcji [eksploracji danych i funkcji inżynierskich Optymalizowanie](#ipnb) .

Do szybkiego weryfikacji liczby wierszy i kolumn w tabelach wypełnione wcześniej za pomocą importu zbiorczego równoległe,

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Badanie: Podróż dystrybucji przez medalion

W tym przykładzie identyfikuje medalion (taksówki numery) z ponad 100 trips w danym okresie. Kwerenda będzie korzystać z dostępu do tabeli podzielonej na partycje, ponieważ jest uwarunkowane schemat partycji **pickup\_datetime**. Podczas badania pełen zestaw danych pozwoli również użyć tabeli podzielonej na partycje i/lub skanowania indeksu.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Badanie: Podróż dystrybucji przez medalion i hack_license

    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Ocena jakości danych: Weryfikowanie rekordów z niepoprawna długość geograficzna i/lub szerokości geograficznej

W tym przykładzie sprawdza, czy któreś z pól Długość geograficzna i/lub szerokości geograficznej albo zawiera nieprawidłową wartość (stopni w radianach powinny być między -90 a 90), lub mieć (0, 0) współrzędne.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Badanie: Rozkład Tipped a nie Przechylony Trips

W tym przykładzie znajduje się liczba podróży, które były wyrzucenia a nie przechylić w danym momencie okresu (lub w pełen zestaw danych, jeśli obejmujące cały rok). Rozkład ten odzwierciedla dystrybucji binarne etykieta ma być później używany do modelowania Klasyfikacja binarna.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Badanie: Klasa/zakres dystrybucji Porada

Ten przykład oblicza rozkład wskazówka zakresów w danym okresie czasu (lub pełnego zestawu danych, jeśli obejmujące cały rok). Jest to rozkład klas etykiet, które będą później używane do modelowania wspólnotowy klasyfikacji.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Badanie: Obliczyć i porównać dystans

W tym przykładzie konwertuje długości geograficznej odbioru i zwrotu i szerokość Geografia SQL punkty, oblicza odległość podróży za pomocą różnicę punktów Geografia SQL i zwraca losowo wyników porównania. W przykładzie ogranicza wyniki na prawidłowe współrzędne tylko za pomocą wcześniej oceny jakości danych kwerendy.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Funkcja inżynierii lądowej i wodnej w kwerendach SQL

Etykieta generacji i geografii konwersji eksploracji kwerendy można również wygenerować etykiety/funkcje poprzez usunięcie części inwentaryzacji. Dodatkową cechą engineering SQL przykłady są podane w sekcji [eksploracji danych i funkcji inżynierskich Optymalizowanie](#ipnb) . Jest bardziej efektywne, aby uruchomić funkcję generowania kwerend na pełen zestaw danych lub duży podzbiór go za pomocą kwerendy SQL, które prowadzą bezpośrednio na wystąpienie bazy danych SQL Server. Kwerendy mogą być wykonywane w **Programie SQL Server Management Studio**, optymalizowanie lub dowolnego narzędzia/środowisko programistyczne, który lokalnie lub zdalnie mają dostęp do bazy.

#### <a name="preparing-data-for-model-building"></a>Przygotowywanie danych do modelu budynku

Poniższa kwerenda sprzężeń **nyctaxi\_podróży** i **nyctaxi\_taryfy** tabele, generuje Klasyfikacja binarna etykieta **wyrzucenia**, Etykieta klasyfikacji wielu klas **końcówki\_klasy**i wyodrębnia losowo 1% pełnego zestawu danych sprzężonych. Ta kwerenda może skopiowane następnie wklejane bezpośrednio w [Omówienie](https://studio.azureml.net) [Importowanie danych] [ import-data] moduł do przyjmowania danych bezpośrednio z wystąpienia bazy danych SQL Server w systemie Azure. Kwerenda nie obejmuje rekordów o błędnej (0, 0) współrzędne.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Eksploracji danych i funkcji inżynierii Optymalizowanie

W tej sekcji będziemy wykonywać eksploracji danych i przy użyciu Python oraz SQL kwerendy do bazy danych programu SQL Server, utworzony wcześniej. Optymalizowanie próbki o nazwie **machine-Learning-data-science-process-sql-story.ipynb** znajduje się w folderze **Notesy IPython próbki** . Ten notes jest również dostępna w [witrynie GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

Zalecana kolejność podczas pracy z dużych ilości danych jest następująca:

- Przeczytaj w małych próbek danych do ramki danych w pamięci.
- Wykonać niektóre wizualizacje i poszukiwań przy użyciu próbki danych.
- Eksperymentuj z funkcją inżynierii lądowej i wodnej, przy użyciu próbki danych.
- Dla większych eksploracji danych, manipulowania danymi i inżynierii lądowej i wodnej funkcji za pomocą Python można wygenerować kwerendy SQL bezpośrednio w bazie danych programu SQL Server w Azure VM.
- Zdecydować, rozmiar próbki dla analityków modelu budynku.

Gdy jest gotowy do usługą sieci Web, użytkownik może:  

1. Zapisz kwerendę SQL końcowego wyodrębnić i przykładowe dane i Kopiuj wklej kwerendy bezpośrednio do [Importowania danych] [ import-data] moduł analityków. Ta metoda jest pokazano w sekcji [Modele budynku analityków](#mlmodel) .    
2. Utrzymują się dane włączonych do próby statystycznej i sztucznymi, ma być używany dla modelu budynku w nowej tabeli bazy danych, a następnie użyj nowej tabeli [Danych importu] [ import-data] modułu.

Poniżej przedstawiono kilka eksploracji danych, wizualizacji danych i funkcji inżynierii przykłady. Aby uzyskać więcej przykładów Zobacz próbki SQL Optymalizowanie w folderze **Notesy IPython próbki** .

#### <a name="initialize-database-credentials"></a>Zainicjowanie bazy danych poświadczeń

Zainicjuj ustawienia połączenia bazy danych w następujących zmiennych:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Tworzenie połączenia z bazą danych

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Raport liczbę wierszy i kolumn w tabeli nyctaxi_trip

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Całkowita liczba wierszy = 173179759  
- Całkowita liczba kolumn = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Odczyt w przykładowych małych danych z bazy danych programu SQL Server

    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Czas do czytania tabeli przykładowej jest 6.492000 sekund  
Liczba wierszy i kolumn źródło = (84952, 21)

#### <a name="descriptive-statistics"></a>Statystyki opisowe

Teraz przystąpić do zbadania próbki danych. Zaczynamy od patrząc na Statystyki opisowe dla **podróży\_odległość** (lub inny) pola:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Wizualizacji: Przykład wydruku pola

Następna przyjrzymy skrzynkowy dla długości podróży do wizualizacji kwantyli

    df1.boxplot(column='trip_distance',return_type='dict')

![Wykreślić #1][1]

#### <a name="visualization-distribution-plot-example"></a>Wizualizacji: Przykład wydruku dystrybucji

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Wykreślić #2][2]

#### <a name="visualization-bar-and-line-plots"></a>Wizualizacji: Pasek i linia działek

W tym przykładzie będziemy bin dystans do pięciu pojemników i wizualizacji binning wyniki.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Możemy wykreślić powyżej dystrybucji pojemnika na pasku lub linii działek, jak poniżej

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Wykreślić #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Wykreślić #4][4]

#### <a name="visualization-scatterplot-example"></a>Wizualizacji: Przykład rozrzutu

Możemy Pokaż wykres punktowy między **podróży\_czas\_w\_s** i **podróży\_odległość** Aby sprawdzić czy ma żadnych powiązań

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Wykreślić #6][6]

Podobnie można sprawdzić relację między **Stawka\_kod** i **podróży\_odległość**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Wykreślić #8][8]

### <a name="sub-sampling-the-data-in-sql"></a>Pobieranie próbki z próbki danych w języku SQL

Podczas przygotowywania danych dla modelu budynku w [Omówienie](https://studio.azureml.net), można zdecydować się na **kwerendy SQL bezpośrednio w module danych importu** lub utrwalenia odtworzone i włączonych do próby statystycznej danych w nowej tabeli można użyć [Danych importu] [ import-data] moduł z prostym * *Wybierz OPCJĘ* FROM < swój\_nowy\_tabeli\_nazwa >**.

W tej sekcji tworzymy nową tabelę do przechowywania danych włączonych do próby statystycznej i odtworzone. Przykład bezpośrednie kwerendy SQL do tworzenia modelu można znaleźć w sekcji [eksploracji danych i funkcji odtwarzania w programie SQL Server](#dbexplore) .

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Tworzenie przykładu tabeli i wypełnić 1% tabel sprzężonych. Upuść pierwszej tabeli, jeśli taka istnieje.

W tej sekcji, możemy sprzężenia tabel **nyctaxi\_podróży** i **nyctaxi\_taryfy**, dodatek 1% losowo i utrzymują się próbki danych w nowej nazwy tabeli **nyctaxi\_jeden\_%**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Za pomocą kwerendy SQL Optymalizowanie eksploracji danych

W tej sekcji możemy zbadać dystrybucji danych przy użyciu danych pobrane próbki 1%, co jest zachowywane w nowej tabeli, stworzyliśmy wyżej. Należy zauważyć, że podobne badania mogą być wykonywane przy użyciu oryginalnego tabel, opcjonalnie przy użyciu **TABLESAMPLE** Aby ograniczyć badań próbki lub poprzez ograniczanie wyników do okresu danym czasie za pomocą **pickup\_datetime** partycje, jak pokazano w sekcji [eksploracji danych i funkcji odtwarzania w programie SQL Server](#dbexplore) .

#### <a name="exploration-daily-distribution-of-trips"></a>Badanie: Codzienną dystrybucję trips

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Badanie: Podróż dystrybucji na medalion

    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Optymalizowanie algorytmów przy użyciu zapytań SQL

W tej sekcji możemy wygenerować nowe etykiety i funkcje bezpośrednio przy użyciu kwerendy SQL, działających w tabeli 1% stworzyliśmy w poprzedniej sekcji.

#### <a name="label-generation-generate-class-labels"></a>Generowanie etykiet: Generowanie etykiet klasy

W poniższym przykładzie możemy wygenerować dwa zestawy etykiet na potrzeby modelowania:

1. Binarne etykiety klasy **Przechylony** (przewidywania, jeśli będzie miał wskazówka)
2. Etykiety wspólnotowy **końcówki\_klasy** (przewidywania wskazówka pojemnika lub zakres)

        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''

        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()

        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''

        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Funkcja inżynierii lądowej i wodnej: Count funkcje dla kolumny kategorii

W tym przykładzie przekształca kategorii pola do pola liczbowego przez zastąpienie każdej kategorii liczba jego wystąpień w danych.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Funkcja inżynierii lądowej i wodnej: Funkcje pojemnika na kolumnach liczbowych

W tym przykładzie przekształca pole liczbowe ciągłe zakresy predefiniowanych kategorii, czyli transformacji pola liczbowego do pola kategorii.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Funkcja inżynierii lądowej i wodnej: Importowanie danych lokalizacji dziesiętnych szerokości i długości geograficznej

W tym przykładzie rozkłada dziesiętnej pole szerokość geograficzna i długość geograficzna na kilka pól regionu różnych rozdrobnienia, takich, jak kraj, Miasto, Miasto, blok itd. Należy zauważyć, że nowe pola geo nie są mapowane do rzeczywistych lokalizacji. Dla informacji o mapowaniu Oblicz lokalizacje zobacz [Usługi REST mapy Bing](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Sprawdź ostatecznej formie w tabeli modeli

    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Jesteśmy teraz przystąpić do tworzenia modelu i modelu z [Usługą sieci](https://studio.azureml.net). Dane są gotowe do żadnego z problemów przewidywania określone wcześniej, a mianowicie:

1. Klasyfikacja binarna: do przewidywania, czy etykietka została wpłacona w podróży.

2. Klasyfikacja wspólnotowy: do przewidzenia zakres wskazówka wypłacone zgodnie z wcześniej zdefiniowanych klas.

3. Zadanie regresji: aby przewidzieć ilość wskazówka wypłacane w podróży.  


## <a name="mlmodel"></a>Modele budynku w uczenia maszynowego Azure

Aby rozpocząć ćwiczenie modelowania, zaloguj się do swojego obszaru roboczego z usługą sieci Web. Jeśli maszynę uczenia się obszaru roboczego nie zostały jeszcze utworzone, zobacz [Tworzenie obszaru roboczego uczenia](machine-learning-create-workspace.md).

1. Aby rozpocząć pracę z usługą sieci Web, zobacz [Co to jest omówienie?](machine-learning-what-is-ml-studio.md)

2. Zaloguj się do [maszyny Azure nauki Studio](https://studio.azureml.net).

3. Strona główna Studio dostarcza wiele informacji, filmy, samouczki, łącza do odwołania modułów i innych zasobów. Uzyskać więcej informacji na temat usługą sieci Web można znaleźć w [Dokumentacji Poznajemy stanow Azure](https://azure.microsoft.com/documentation/services/machine-learning/).

Eksperyment typowe szkolenie składa się z następujących czynności:

1. Utwórz eksperyment **+ Nowy** .
2. Pobieranie danych do analityków.
3. Wstępnie przetworzyć, przekształcania i manipulować danymi, stosownie do potrzeb.
4. Generowanie funkcje, stosownie do potrzeb.
5. Podzielić dane na szkolenie / / sprawdzanie poprawności zestawów danych (lub mieć osobne zestawy danych dla każdego).
6. Zaznacz jeden lub więcej algorytmów uczenia maszynowego w zależności od rozwiązać problem nauki. Np. klasyfikacja binarna, wspólnotowy klasyfikacji, regresji.
7. Pociąg jednego lub kilku modeli przy użyciu zestawu danych szkolenia.
8. Ocena dataset sprawdzania poprawności za pomocą wyszkolonych modele.
9. Ocena modele do obliczenia odpowiednich wskaźników dla problemu nauki.
10. Precyzyjną modele oraz wybierz model najlepiej wdrożyć.

W tym ćwiczeniu mamy już badane i odtwarzane danych w programie SQL Server i zdecydował się na wielkość próbki do połknięcia analityków. Do tworzenia jednego lub więcej modeli przewidywania zdecydowaliśmy się:

1. Pobieranie danych do usługą sieci Web przy użyciu [Danych importu] [ import-data] moduł, dostępny w sekcji **danych wejściowych i wyjściowych** . Aby uzyskać więcej informacji, zobacz temat [Importowanie danych] [ import-data] moduł odwołanie do strony.

    ![Dane dotyczące przywozu uczenia maszynowego Azure][17]

2. Wybierz **Bazy danych SQL Azure** jako **źródło danych** w panelu **Właściwości** .

3. Wprowadź nazwę bazy danych DNS w polu **Nazwa serwera bazy danych** . Format:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Wprowadź **nazwę bazy danych** w odpowiednim polu.

5. Wprowadź **nazwę użytkownika SQL** w **Server Nazwa aqccount użytkownika i hasło w **Server użytkownika konta hasło **.

6. Zaznacz opcję **Zaakceptuj dowolnego certyfikatu serwera** .

7. W obszarze tekstowym Edytuj **kwerendę bazy danych** Wklej kwerendy, który wyodrębnia niezbędne pola (w tym żadnych pól obliczanych, takich jak etykiety) bazy danych i w dół próbkuje dane do rozmiaru żądanego próbki.

Na rysunku poniżej jest przykładem eksperyment Klasyfikacja binarna odczytywania danych bezpośrednio z bazy danych programu SQL Server. Podobne eksperymenty mogą być skonstruowane do klasyfikacji wspólnotowy i problemy regresji.

![Maszyny Azure Learning pociągu][10]

> [AZURE.IMPORTANT] W danych modelowania ekstrakcji i przykłady kwerendy do pobierania próbek określonych w poprzednich sekcjach, **wszystkie etykiety do ćwiczeń trzy modelowania są uwzględnione w kwerendzie**. Ważnym krokiem (wymagane) w każdym z ćwiczeń modelowania jest **Wykluczanie** niepotrzebnych etykiety dla dwóch pozostałych problemów i wszelkich innych **przecieków docelowej**. Np., używając klasyfikacji binarnym, użyj etykiety **Przechylony** i wykluczyć pola **końcówki\_klasy**, **Wskazówka\_kwoty**, i **całkowita\_kwoty**. Te ostatnie są przecieki docelowej ponieważ oznaczają one końcówki zapłacone.
>
> Aby wykluczyć zbędne kolumny i/lub docelowych przecieków, mogą używać [Wybierz kolumny w zestawie danych] [ select-columns] modułu lub [Edytuj metadane][edit-metadata]. Aby uzyskać więcej informacji, zobacz [Wybieranie kolumn w zestawie danych] [ select-columns] i [Edycja metadanych] [ edit-metadata] odwołania do stron.

## <a name="mldeploy"></a>Wdrażanie modeli w uczenia maszynowego Azure

Gdy model jest gotowy, można łatwo rozmieścić go jako usługi sieci web bezpośrednio z doświadczenia. Aby uzyskać więcej informacji na temat wdrażania usług sieci web z usługą sieci Web zobacz [Wdrażanie usługi sieci web z usługą sieci Web](machine-learning-publish-a-machine-learning-web-service.md).

Aby wdrożyć nowe usługi sieci web, należy:

1. Tworzenie punktów końcowych.
2. Wdrażanie usługi sieci web.

Do tworzenia punktów końcowych z doświadczenia kształcenie **zakończone** , kliknij przycisk **Utwórz PUNKTACJI EKSPERYMENTOWAĆ** na dolnym pasku akcji.

![Punktacja Azure][18]

Uczenie maszynowe Azure będzie podejmować próby utworzenia punktów końcowych, oparte na składnikach doświadczenia szkolenia. W szczególności będzie:

1. Zapisz model wyszkolonych i Usuń modułów szkoleniowych w modelu.
2. Identyfikuje logiczne **port wejściowy** do reprezentowania schematu oczekiwane dane wejściowe.
3. Identyfikuje logiczne **port wyjściowy** do reprezentowania schemat danych wyjściowych planowanej sieci web service.

Podczas tworzenia punktów końcowych go przejrzeć i dostosować stosownie do potrzeb. Typowe korekty jest zastąpienie zestawu danych wejściowych i/lub kwerendy z jednym, co wyklucza pól etykiet, jak te nie będą dostępne, gdy usługa jest wywoływana. To także dobrych praktyk w celu zmniejszenia rozmiaru zestawu danych wejściowych i/lub kwerendy do kilku rekordów, wystarczy, aby wskazać schemacie wejściowym. Dla portu wyjściowego jest wspólne wykluczyć input wszystkie pola i zawierać tylko **Zdobył etykiety** i **Prawdopodobieństwa zdobytych** w danych wyjściowych przy użyciu [Wybierz kolumny w zestawie danych] [ select-columns] modułu.

Próbka punktacji eksperymentu jest na rysunku poniżej. Gdy jest gotowy do wdrożenia, kliknij przycisk **Publikowania usługi sieci WEB** na dolnym pasku akcji.

![Publikowanie uczenia maszynowego Azure][11]

Podsumowując, w tym samouczku wskazówki utworzono środowisku nauki danych Azure pracował z dużego zestawu danych publicznych od nabycia danych do modelu szkolenia i wdrażanie usługi sieci web z usługą sieci Web.

### <a name="license-information"></a>Informacje o licencji

W tym instruktażu próbki i towarzyszące jej notatniki() IPython i skrypty są udostępniane przez firmę Microsoft licencji MIT. Sprawdź plik LICENSE.txt w katalogu przykładowy kod na GitHub uzyskać więcej szczegółów.

### <a name="references"></a>Odwołania

• [Strona pobierania Trips NYC Taxi Andrés Monroy](http://www.andresmh.com/nyctaxitrips/)  
• [Foliowanie nowego JORKU Taxi dane podróży przez Chrisa Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi, badania Komisji kombi i statystyki](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
