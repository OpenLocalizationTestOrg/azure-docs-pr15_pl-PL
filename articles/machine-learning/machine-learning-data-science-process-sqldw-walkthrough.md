<properties
    pageTitle="Proces zespołu danych naukowych w działaniu: przy użyciu magazynu danych SQL | Microsoft Azure"
    description="Proces zaawansowanej analizy i technologii w działaniu"  
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
    ms.date="06/24/2016"
    ms.author="bradsev;hangzh;weig"/>


# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>Proces zespołu danych naukowych w działaniu: przy użyciu magazynu danych SQL

W tym samouczku wykonaniem przez tworzenie i wdrażanie maszynowego uczenia modelu przy użyciu magazynu danych SQL (SQL DW) publicznie zestawu danych — [Warszawa taksówki podróży](http://www.andresmh.com/nyctaxitrips/) zestawu danych. Model klasyfikacji binarne wykonane przewiduje czy etykietkę zapłacono w podróży, a modeli klasyfikacji multiclass i regresji także omówiono które przewidywanie rozkład kwot Porada zapłacone.

Procedura wykonuje przepływu pracy [Zespołu danych nauki proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) . Pokażemy, jak skonfigurować środowisku nauki danych, jak załadować dane do SQL DW i jak używać SQL DW lub notes IPython Eksplorowanie danych i odtwarzania funkcje modelu. Następnie zostanie wyświetlona sposobu tworzenia i wdrażania modelu z Azure maszynowego uczenia.


## <a name="dataset"></a>Warszawa taksówki podróży zestawu danych

Dane podróży taksówki Warszawa składa się z około 20GB skompresowane pliki CSV (~ 48GB nieskompresowane), rejestrowanie więcej niż miliona 173 pojedynczych podróży i opłaty opłacona każdej podróży. Każdy podróży rekord zawiera lokalizacje pobrania i przyjmowania i godzin, zastąpione anonimowymi próba złamania (sterownik) liczbę licencji i liczba Medalionu (taksówki i unikatowy identyfikator). Dane obejmuje wszystkie podróży w roku 2013 i znajduje się w następujących dwa zestawy danych dla każdego miesiąca:

1. Plik **trip_data.csv** zawiera szczegóły podróży, takie jak liczba osób, pobrania i dropoff punktów, czas trwania podróży i długość podróży. Poniżej przedstawiono kilka przykładowych rekordów:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Plik **trip_fare.csv** zawiera szczegóły dotyczące taryfy za każdym razem, takie jak typ płatności, kwota taryfy, opłaty dodatkowej i podatki, porady i drogowe i całkowitą kwotę zapłaconą. Poniżej przedstawiono kilka przykładowych rekordów:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

**Unikatowy klucz** do łączenia podróży\_danych i podróży\_taryfy składa się z trzech poniższych pól:

- Medalionu,
- Funkcja\_licencji i
- pobranie\_daty i godziny.

## <a name="mltasks"></a>Trzy typy zadań przewidywania adresu

Firma Microsoft sformułowania trzy problemy przewidywania na podstawie *Porada\_kwota* aby przedstawić trzy rodzaje modelowania zadania:

1. **Binarny klasyfikacji**: przewidywanie czy etykietkę zapłacono w podróży, to znaczy *Porada\_kwota* większą niż 0 zł jest dodatnia przykład, podczas gdy *Porada\_kwota* $0 jest ujemna przykład.

2. **Klasyfikacja multiclass**: przewidywanie zakres Porada opłacona podróży. Dzielimy *Porada\_kwota* do pięciu przedziałów lub klas:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Zadanie regresji**: przewidywanie ilość Porada spłacona w podróży.  


## <a name="setup"></a>Konfigurowanie środowiska nauki Azure danych dla zaawansowanej analizy

Aby skonfigurować środowisko usługi Azure nauki danych, wykonaj następujące kroki.

**Tworzenie własnego konta magazyn obiektów blob platformy Azure**

- Po postanowień magazyn obiektów blob platformy Azure, wybierz lokalizację geo magazyn obiektów blob platformy Azure w lub jak najbliżej **Południowej centralnej w Stanach Zjednoczonych**, czyli miejsce, w którym taksówki Warszawa przechowywania danych. Dane zostaną skopiowane za pomocą AzCopy z kontenera magazyn obiektów blob publicznej do kontenera na koncie miejsca do magazynowania. Dokładniejsze magazyn obiektów blob platformy Azure Południowej centralnej nam, tym szybciej będzie można ukończyć tego zadania (krok 4).
- Aby utworzyć konto Azure miejsca do magazynowania, postępuj zgodnie z instrukcjami podanymi w [o Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md). Pamiętaj notatki na wartości dla następujących magazynu poświadczeń konta będą potrzebne w dalszej części tego instruktażu.

  - **Nazwę konta magazynu**
  - **Klucz konta miejsca do magazynowania**
  - **Nazwa kontenera** (który mają dane, które mają być przechowywane w magazynie obiektów blob platformy Azure)

**Inicjowanie obsługi wystąpienia Azure SQL DW.**
Postępuj zgodnie z dokumentacją pod adresem [Tworzenie magazynu danych SQL](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) do zapewniania obsługi w przypadku wystąpienia magazynu danych SQL. Upewnij się, wprowadź notacji przy użyciu następujących poświadczeń magazynu danych SQL, które będą używane w dalszych krokach.

  - **Nazwa serwera**: <server Name>. database.windows.net
  - **Nazwa SQLDW (baza danych)**
  - **Nazwa użytkownika**
  - **Hasło**

**Zainstaluj program Visual Studio 2015 i SQL Server Data Tools.** Aby uzyskać instrukcje zobacz [Instalowanie programu Visual Studio 2015 i/lub SSDT (program SQL Server Data Tools) dla magazynu danych SQL](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Połącz ze swojego DW Azure SQL z programem Visual Studio.** Aby uzyskać instrukcje zobacz kroki 1 i 2 w [Nawiązywanie połączenia z magazynem danych SQL Azure z programem Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

>[AZURE.NOTE] Uruchom poniższe zapytanie SQL do bazy danych utworzonej w magazynie danych SQL (zamiast kwerendy opisane w kroku 3 w temacie łączenie) do **tworzenia klucza głównego**.

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Tworzenie obszaru roboczego Azure maszynowego uczenia w subskrypcji usługi Azure.** Aby uzyskać instrukcje zobacz [Tworzenie obszaru roboczego Azure maszynowego uczenia](machine-learning-create-workspace.md).

## <a name="getdata"></a>Ładowanie danych do magazynu danych SQL

Otwieranie konsoli poleceń programu Windows PowerShell. Uruchom następujące programu PowerShell polecenia w celu pobrania przykład SQL skryptów plików, które udostępniamy Ci na Github do katalogu lokalnego przez użytkownika przy użyciu parametru *- DestDir*. Możesz zmienić wartość parametru *-DestDir* do dowolnego katalogu lokalnego. Jeśli *- DestDir* nie istnieje, zostanie on utworzony przez skrypt programu PowerShell.

>[AZURE.NOTE] Może być konieczne **polecenie Uruchom jako Administrator** podczas wykonywania tego skryptu programu PowerShell, jeśli katalog *DestDir* wymaga uprawnienia administratora, aby utworzyć lub do zapisu.

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Po pomyślnym wykonaniu bieżącego katalogu pracy zmienia *- DestDir*. Powinny być widoczne na ekranie, takich jak poniżej:

![][19]

W *Dolnym DestDir*wykonaj następujący skrypt programu PowerShell w trybie administratora:

    ./SQLDW_Data_Import.ps1

Po uruchomieniu skrypt programu PowerShell po raz pierwszy, zostanie wyświetlony monit o podanie informacji z usługi DW SQL Azure i konta magazyn obiektów blob platformy Azure. Po ukończeniu tego skryptu programu PowerShell uruchomiony po raz pierwszy, poświadczenia wprowadzania można będzie zapisano do pliku konfiguracji SQLDW.conf w katalogu bieżącą pracę. Przyszłych Uruchom ten plik skryptu programu PowerShell zawiera odpowiednią opcję, aby przeczytać wszystkie potrzebne parametry z tego pliku konfiguracji. Jeśli chcesz zmienić niektóre parametry, możesz wybrać wprowadzania parametrów na ekranie po wierszu, usuwając ten plik konfiguracyjny i wprowadzanie wartości parametrów w komunikatach lub zmień wartości parametrów edytując plik SQLDW.conf w katalogu *- DestDir* .

>[AZURE.NOTE] Aby uniknąć konfliktów nazw schematu z tych, które już istnieje w Twojej DW SQL Azure, podczas czytania parametrów bezpośrednio z pliku SQLDW.conf, liczbę losową z zakresu 3-cyfrowy jest dodawany do nazwy schematu z pliku SQLDW.conf jako domyślnej nazwy schematu dla każdej serii. Skrypt programu PowerShell może zostać wyświetlony komunikat o podanie nazwy schematu: Nazwa może być określona według uznania użytkownika.

Ten plik **skryptu programu PowerShell** wykonuje następujące zadania:

- **Pobiera i instaluje AzCopy**, jeśli AzCopy nie jest już zainstalowany.

        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
            Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }

- **Kopie danych do swojego konta magazyn obiektów blob prywatne** z publicznej obiektów blob z AzCopy

        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"


- **Obciążenia danych przy użyciu Polybase (wykonując LoadDataToSQLDW.sql) do swojego DW SQL Azure** , z Twojego konta magazynu obiektów blob prywatne z następujących poleceń.

    - Tworzenie schematu

            EXEC (''CREATE SCHEMA {schemaname};'');

    - Tworzenie poświadczenie występujące bazy danych

            CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
            WITH IDENTITY = ''asbkey'' ,
            Secret = ''{StorageAccountKey}''

    - Tworzenie zewnętrznego źródła danych dla obiektów blob platformy Azure miejsca do magazynowania

            CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

            CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
            WITH
            (
                TYPE = HADOOP,
                LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
                CREDENTIAL = {KeyAlias}
            )
            ;

    - Tworzenie zewnętrznym formacie pliku dla pliku csv. Dane są nieskompresowane i pola są oddzielone znakiem potoku.

            CREATE EXTERNAL FILE FORMAT {csv_file_format}
            WITH
            (   
                FORMAT_TYPE = DELIMITEDTEXT,
                FORMAT_OPTIONS  
                (
                    FIELD_TERMINATOR ='','',
                    USE_TYPE_DEFAULT = TRUE
                )
            )
            ;

    - Tworzenie zewnętrznych taryfy i tabel podróży dla zestawu danych taksówki Warszawa w magazynie obiektów blob platformy Azure.

            CREATE EXTERNAL TABLE {external_nyctaxi_fare}
            (
                medallion varchar(50) not null,
                hack_license varchar(50) not null,
                vendor_id char(3),
                pickup_datetime datetime not null,
                payment_type char(3),
                fare_amount float,
                surcharge float,
                mta_tax float,
                tip_amount float,
                tolls_amount float,
                total_amount float
            )
            with (
                LOCATION    = ''/nyctaxifare/'',
                DATA_SOURCE = {nyctaxi_fare_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12     
            )  


            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                medallion varchar(50) not null,
                hack_license varchar(50)  not null,
                vendor_id char(3),
                rate_code char(3),
                store_and_fwd_flag char(3),
                pickup_datetime datetime  not null,
                dropoff_datetime datetime,
                passenger_count int,
                trip_time_in_secs bigint,
                trip_distance float,
                pickup_longitude varchar(30),
                pickup_latitude varchar(30),
                dropoff_longitude varchar(30),
                dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Ładowanie danych z zewnętrznych tabel w magazynie obiektów blob platformy Azure do magazynu danych SQL

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Tworzenie tabeli danych przykładowych (NYCTaxi_Sample) i wstawić dane do niego z Wybieranie zapytania SQL w tabelach podróży i taryfy. (Pewne czynności w tym instruktażu należy korzystać z tej tabeli przykładowych.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

Położenie geograficzne konta miejsca do magazynowania ma wpływ na czas ładowania.

>[AZURE.NOTE] W zależności od lokalizacji geograficznej konta magazyn obiektów blob prywatne, proces kopiowania danych z publicznej obiektów blob do swojego konta prywatnego magazynowania może zająć około 15 minut, a nawet dłużej i proces ładowania danych z Twojego konta miejsca do magazynowania do swojego DW SQL Azure może potrwać 20 minut lub więcej.  

Konieczne będzie podjęcie czego, jeśli masz zduplikowanych plików źródłowej i docelowej.

>[AZURE.NOTE] Jeśli pliki CSV do skopiowania z magazynu publicznej obiektów blob do swojego konta magazyn obiektów blob prywatne już istnieje na koncie magazyn obiektów blob prywatne, AzCopy poprosi Cię, czy chcesz je zastąpić. Jeśli nie chcesz zastąpić je, wprowadzania **n** po wyświetleniu monitu. Jeśli chcesz zastąpić **Wszystkie** je, wprowadzania **a** po wyświetleniu monitu. Można również wprowadzania **y** , aby zastąpić pliki CSV pojedynczo.

![Kreśl #21][21]

Możesz użyć własnych danych. Jeśli dane znajdują się na tym komputerze lokalnego w aplikacji rzeczywistych, aby przekazać danych w wersji lokalnej z magazynem obiektów blob platformy Azure prywatne można nadal używać AzCopy. Należy zmienić lokalizację **źródłową** `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, w tym poleceniu AzCopy plik skryptu programu PowerShell do katalogu lokalnego, który zawiera dane.


>[AZURE.TIP] Jeśli dane są w magazynie obiektów blob platformy Azure prywatne w aplikacji rzeczywistych, można przejść do kroku AzCopy skrypt programu PowerShell i bezpośrednio przekazywanie danych do Azure SQL DW. Wymaga to dodatkowe modyfikacje skrypt, aby dopasować go do formatu danych.


Tego skryptu programu Powershell również podłączyć informacje Azure SQL DW do plików danych badań przykład SQLDW_Explorations.sql, SQLDW_Explorations.ipynb i SQLDW_Explorations_Scripts.py tak, aby te trzy pliki są gotowe do być wypróbowane natychmiast po zakończeniu skrypt programu PowerShell.

Po pomyślnym wykonaniu zostanie wyświetlony ekran, takich jak poniżej:

![][20]

## <a name="dbexplore"></a>Poszukiwanie danych i funkcji techniczny w magazynie danych SQL Azure

W tej sekcji możemy wykonywanie badań danych oraz generowanie funkcji przez wykonywanie zapytań SQL Azure SQL DW bezpośrednio przy użyciu **Programu Visual Studio Tools danych**. Wszystkie zapytania SQL używane w tej sekcji znajdują się w przykładowy skrypt o nazwie *SQLDW_Explorations.sql*. Ten plik został już pobrany do katalogu lokalnego przez skrypt programu PowerShell. Można także pobrać z [Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). Ale plik Github nie ma informacji Azure SQL DW podłączone.

Połącz się z DW SQL Azure za pomocą programu Visual Studio za pomocą SQL DW nazwa logowania i hasła i otwierania w **Eksploratorze obiektów SQL** , upewnij się, że zostały zaimportowane bazę danych i tabele. Pobierz plik *SQLDW_Explorations.sql* .

>[AZURE.NOTE] Aby otworzyć Edytor zapytań równoległe magazynu danych (PDW), użyj polecenia **Nowe zapytanie** , podczas Twojej PDW jest zaznaczone w **Eksploratorze obiektów SQL**. Standardowy Edytor zapytań SQL nie jest obsługiwane przez PDW.

Poniżej przedstawiono typ danych badań i funkcji Generowanie zadania wykonywane w tej sekcji:

- Eksplorowanie danych podziału kilku pól w różnych okien czasu.
- Badanie jakości danych pól szerokości i długości geograficznej.
- Generowanie etykiet binarne i multiclass klasyfikacji na podstawie **Porada\_kwota**.
- Generowanie funkcje i obliczeń i porównaj odległości podróży.
- Łączenie dwóch tabel i ekstrahować losowo używany do tworzenia modeli.

### <a name="data-import-verification"></a>Weryfikacja importu danych

Te kwerendy udostępnia szybkie weryfikacji liczbę wierszy i kolumn w tabelach wypełnione wcześniej przy użyciu programu zbiorcze równoległe Polybase i zaimportować,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Wynik:** Należy uzyskać 173,179,759 wierszy i kolumn 14.

### <a name="exploration-trip-distribution-by-medallion"></a>Więcej informacji: Podróży rozpowszechniania przez Medalionu

Ta przykładowa kwerenda służy do identyfikowania medallions (taksówki liczby), które wykonane więcej niż 100 podróży w danym okresie. Kwerendy będzie korzystać z podzielonej na partycje tabeli programu access, ponieważ należy przygotować w schemacie partition **pobrania\_datetime**. Kwerenda pełny zestaw danych spowoduje również, że wykorzystanie tabeli podzielonej na partycje i/lub indeksowania skanowania.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Wynik:** Kwerenda powinna zwrócić tabeli z wierszami, określając 13,369 medallions (taksówki) i Liczba podróży wykonywanych przez nich w 2013. Ostatnia kolumna zawiera Liczba podróży wykonane.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Więcej informacji: Podróży rozkładu Medalionu i hack_license

W tym przykładzie identyfikuje medallions (taksówki liczby) i hack_license liczby (sterowniki) zakończenia więcej niż 100 podróży w danym okresie.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Wynik:** Kwerenda powinna zwrócić tabeli z wierszami 13,369 określającą 13,369 samochodu sterownik identyfikatorów, które zostały wykonane więcej tej operacji 100 w 2013. Ostatnia kolumna zawiera Liczba podróży wykonane.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Oceny jakości danych: Sprawdź rekordów zawierających nieprawidłowe długości geograficznej i/lub szerokości

W tym przykładzie bada Jeśli polach szerokości i długości geograficznej albo zawiera nieprawidłowe wartości (stopni w radianach powinna być między -90 a 90), lub mają (0; 0) współrzędne.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Wynik:** Kwerenda zwróci 837,467 podróży, zawierających nieprawidłowe pola długości geograficznej i/lub szerokości.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Więcej informacji: Przechylony a rozkładu nie Przechylony podróży

W tym przykładzie znajduje Liczba podróży, które zostały Przechylony a numer, które nie zostały Przechylony w danym okresie (lub w pełny zestaw danych, jeśli obejmujące pełny rok, jak została skonfigurowana poniżej). Rozkład ten odzwierciedla rozkład binarne etykiety później używanego do modelowania klasyfikacji binarne.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Wynik:** Kwerenda powinna zwrócić Przechylony następujących częstotliwości Porada na rok 2013: 90,447,622 i Przechylony nie 82,264,709.

### <a name="exploration-tip-classrange-distribution"></a>Więcej informacji: Rozkład zajęć/zakres Porada

W tym przykładzie oblicza rozkład Porada zakresy w danym okresie (lub w pełny zestaw danych, jeśli obejmujące pełny rok). Jest to rozkład klas etykiety, które będą używane później umożliwia modelowanie multiclass klasyfikacji.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Wynik:**

|tip_class  | tip_freq |
| --------- | -------|
|1  | 82230915 |
|2  | 6198803 |
|3  | 1932223 |
|0  | 82264625 |
|4  | 85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Więcej informacji: Obliczanie i porównywanie odległość podróży

W tym przykładzie konwertuje długości geograficznej odbiór i przyjmowania i szerokości do Geografia SQL punktów, oblicza odległość podróży służbowej przy użyciu różnicę punktów Geografia SQL, a zwraca losowo wyniki porównania. Przykład ogranicza wyniki na prawidłową współrzędne tylko przy użyciu kwerendy oceny jakości danych omówione wcześniej.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Funkcja techniczny przy użyciu funkcji SQL

Czasami funkcje w języku SQL może być wydajność opcję techniczny funkcji. W tym instruktażu możemy zdefiniować funkcji języka SQL, aby obliczyć bezpośredni odległość między lokalizacjami odbiór i dropoff. Możesz uruchomić następujące skrypty SQL w **Visual Studio Tools danych**.

Oto skrypt SQL, który definiuje funkcję odległość.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
        DECLARE @distance decimal(28, 10)
        -- Convert to radians
        SET @Lat1 = @Lat1 / 57.2958
        SET @Long1 = @Long1 / 57.2958
        SET @Lat2 = @Lat2 / 57.2958
        SET @Long2 = @Long2 / 57.2958
        -- Calculate distance
        SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
        --Convert to miles
        IF @distance <> 0
        BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
        END
        RETURN @distance
    END
    GO

Oto przykład połączenie tej funkcji, aby wygenerować funkcje w zapytaniu SQL:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Wynik:** Ta kwerenda generuje tabelę (z 2,803,538 wierszy) z pobrania i dropoff długości i szerokości geograficzne i odpowiadające im bezpośrednie odległości mila. Poniżej przedstawiono wyniki pierwsze 3 wiersze:

||pickup_latitude | pickup_longitude    | dropoff_latitude |dropoff_longitude | DirectDistance |
|---| --------- | -------|-------| --------- | -------|
|1  | 40.731804 | -74.001083 | 40.736622 | -73.988953 | .7169601222 |
|2  | 40.715794 | -74,010635 | 40.725338 | -74.00399 | .7448343721 |
|3  | 40.761456 | -73.999886 | 40.766544 | -73.988228 | 0.7037227967 |



### <a name="prepare-data-for-model-building"></a>Przygotowywanie danych do tworzenia modelu

Poniższa kwerenda sprzężenia **nyctaxi\_podróży** i **nyctaxi\_taryfy** tabele, generuje binarne klasyfikacji etykiety **Przechylony**, etykiety klasyfikacji wielu klasy **Porada\_klasy**i wyciągi z pełnego zestawu danych sprzężonych próbki. Próbki jest wykonywane przez pobieranie podzbiór podróży na podstawie czasu pobrania.  Ta kwerenda można skopiować następnie wklejony bezpośrednio w [Azure maszynowego uczenia Studio](https://studio.azureml.net) [Importowanie danych] [ import-data] moduł dla spożyciu dane bezpośrednio z wystąpienia bazy danych SQL platformy Azure. Kwerenda nie obejmuje rekordów zawierających nieprawidłowe (0; 0) współrzędne.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Po zakończeniu przejdź do nauki maszynowego Azure, użytkownik może:  

1. Zapisywanie ostateczne kwerendy SQL do wyodrębniania i przykładowe dane i kopiowanie i wklejanie kwerendy bezpośrednio do [Importowania danych] [ import-data] moduł w Azure maszynowego uczenia, lub
2. Zachować dane próbki i odtwarzania planujesz używać dla modelu tworzenia nowej tabeli SQL DW oraz nowej tabeli w oknie [Importowanie danych] [ import-data] moduł w Azure maszynowego uczenia. Skrypt programu PowerShell w poprzednim kroku wystąpiło to za Ciebie. Można czytać bezpośrednio z tej tabeli w module importowanie danych.


## <a name="ipnb"></a>Poszukiwanie danych i funkcji techniczny w notesie IPython

W tej sekcji możemy wykona badań danych i generowanie funkcji za pomocą obu Python i kwerend SQL względem SQL DW utworzony wcześniej. Przykładowy notes IPython, o nazwie **SQLDW_Explorations.ipynb** i Python plik skryptu **SQLDW_Explorations_Scripts.py** zostały pobrane do katalogu lokalnego. Są one również dostępne na [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Tych plików są identyczne w Python skryptów. Plik skryptu Python są dostępne w przypadku, gdy nie masz na serwerze IPython notesu. Te dwa przykładowe pliki Python są zaprojektowane w obszarze **Python 2.7**.

Wymagane informacje Azure SQL DW w próbce Notes IPython i plik skryptu Python zostało pobrane na komputer lokalny został podłączony przez skrypt programu PowerShell wcześniej. Są one wykonywalny bez żadnych modyfikacji.

Jeśli masz już skonfigurowane z obszarem roboczym AzureML, można bezpośrednio Przekaż przykładowy IPython notesu do usługi AzureML IPython notesu i uruchomione go. Poniżej przedstawiono kroki, aby przekazać do usługi AzureML IPython notesu:

1. Zaloguj się do obszaru roboczego AzureML, kliknij polecenie "Studio" u góry, a następnie kliknij "NOTESY" po lewej stronie strony sieci web.

    ![Kreśl #22][22]

2. Kliknij przycisk "Nowy" w lewym dolnym rogu strony sieci web, a następnie wybierz pozycję "Python 2". Następnie należy podać nazwę do notesu i kliknij znacznik wyboru, aby utworzyć nowy pusty notes IPython.

    ![Kreśl #23][23]

3. Kliknij symbol "Jupyter" w lewym górnym rogu nowego notesu IPython.

    ![Kreśl #24][24]

4. Przeciągnij i upuść próbki IPython notesu na stronie **drzewa** usługi AzureML IPython notesu i kliknij przycisk **Przekaż**. Następnie w próbce notesu IPython zostanie przekazany z usługą AzureML IPython notesu.

    ![Kreśl #25][25]

Aby uruchomić przykład Notes IPython lub Python skrypt pliku, następujące Python, pakiety są wymagane. Jeśli korzystasz z usługi AzureML IPython notesu, te pakiety zostały zainstalowany.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Zalecana kolejność podczas konstruowania zaawansowane rozwiązania analitycznych na AzureML z dużych danych jest następująca:

- Przeczytaj w małych przykładowych danych do ramki danych w pamięci.
- Wykonywanie niektórych wizualizacji i explorations przy użyciu próbki danych.
- Wypróbuj techniczny funkcji włączonych danych.
- Dla większych badań danych, manipulowania danych i funkcji za pomocą Python można wygenerować zapytania SQL bezpośrednio przed SQL DW.
- Określ wielkością próbki, aby nadawało do tworzenia modelu Azure maszynowego uczenia się.

Następujących typów to kilka badań danych, wizualizacji danych i funkcji inżynierskich przykłady. Więcej explorations danych można znaleźć w próbce IPython notesu i przykładowy plik skryptu Python.

### <a name="initialize-database-credentials"></a>Inicjowanie poświadczeń bazy danych

Inicjowanie ustawień połączenia bazy danych w następujących czynników:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Tworzenie połączenia z bazą danych

Poniżej przedstawiono parametry połączenia, który tworzy połączenie z bazą danych.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Raport liczbę wierszy i kolumn w tabeli < nyctaxi_trip >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Całkowita liczba wierszy = 173179759  
- Całkowita liczba kolumn = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Raport liczbę wierszy i kolumn w tabeli < nyctaxi_fare >

    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

- Całkowita liczba wierszy = 173179759  
- Całkowita liczba kolumn = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Odczytu w próbce małych danych z magazynu bazy danych SQL

    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Czas odczytu przykładowej tabeli jest 14.096495 sekund.  
Liczba wierszy i kolumn pobranych = (1000, 21).

### <a name="descriptive-statistics"></a>Statystyki opisowe

Teraz możesz przystąpić do Eksplorowanie danych próbki. Firma Microsoft zaczynają się na niektórych Statystyki opisowe dla **podróży\_odległość** (lub innych pól wybrania).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Wizualizacji: Przykład wykresu pola

Następny przyjrzymy się skrzynkowy odległość podróży wizualizacji quantiles.

    df1.boxplot(column='trip_distance',return_type='dict')

![Kreśl #1][1]

### <a name="visualization-distribution-plot-example"></a>Wizualizacji: Przykład wykresu rozkładu

Wykresy, które wizualizowanie przydzielania i histogram odległości próbki podróży.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Kreśl #2][2]

### <a name="visualization-bar-and-line-plots"></a>Wizualizacja: Słupkowe i liniowe wykresy

W tym przykładzie możemy przedziałów odległość podróży w przedziałach pięć i wizualizowanie binning wyników.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Firma Microsoft przedstawione powyżej rozkład pojemnika na pasku lub linii kreślenia z:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Kreśl #3][3]

oraz

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Kreśl #4][4]

### <a name="visualization-scatterplot-examples"></a>Wizualizacji: Przykłady Scatterplot

Możemy wyświetlić wykres punktowy między **podróży\_czasu\_w\_s** i **podróży\_odległość** czy jest dowolny korelacji

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Kreśl #6][6]

Podobnie można sprawdzić relacje między **Stawka\_kod** i **podróży\_odległość**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Kreśl #8][8]


### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Poszukiwanie danych na próbki danych za pomocą zapytania SQL w notesie IPython

W tej sekcji możemy Eksplorowanie rozkład danych przy użyciu próbki danych, w której jest umieszczany w nowej tabeli utworzonego powyżej. Należy zauważyć, że podobne explorations mogą być wykonywane przy użyciu oryginalnego tabel.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Więcej informacji: Raport liczbę wierszy i kolumn w tabeli próbki

    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Więcej informacji: Przechylony nie samoczynnie rozkładu

    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Więcej informacji: Rozkład zajęć Porada

    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Więcej informacji: Kreśl rozkład Porada według klasy

    tip_class_dist['tip_freq'].plot(kind='bar')

![Kreśl #26][26]


#### <a name="exploration-daily-distribution-of-trips"></a>Więcej informacji: Dzienny rozkład podróży

    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Więcej informacji: Podróży rozkładu na Medalionu

    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Więcej informacji: Podróży rozpowszechniania przez Medalionu i próba złamania licencji

    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Badań: Rozkład czasów podróży

    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Badań: Rozkład odległość podróży

    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Więcej informacji: Dystrybucja typ płatności

    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Sprawdź ostatecznej formy tabeli featurized

    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Tworzenie modeli w nauki maszynowego Azure

Pracujemy obecnie przejść do tworzenia modelu i wdrożenia modelu w [Azure maszynowego uczenia](https://studio.azureml.net). Dane są gotowe do użytku w innych problemów przewidywania wymienionych wcześniej, takich jak:

1. **Binarny klasyfikacji**: przewidywanie czy etykietkę zapłacono w podróży.

2. **Klasyfikacja multiclass**: przewidywanie zakres Porada zapłacone zgodnie z wcześniej zdefiniowanych klas.

3. **Zadanie regresji**: przewidywanie ilość Porada spłacona w podróży.  



Aby rozpocząć wykonywanie modelowania, zaloguj się do obszaru roboczego **Azure maszynowego uczenia** . Jeśli nie został jeszcze utworzony maszynowego uczenia obszaru roboczego, zobacz [Tworzenie z obszarem roboczym Azure ML](machine-learning-create-workspace.md).

1. Aby rozpocząć pracę z Azure maszynowego uczenia, zobacz [Co to jest Azure maszynowego uczenia Studio?](machine-learning-what-is-ml-studio.md)

2. Zaloguj się do [komputera Azure nauki Studio](https://studio.azureml.net).

3. Strona główna Studio zapewnia wiele informacji, klipów wideo, samouczki, łącza do odwołania moduły i inne zasoby. Więcej informacji na temat Azure maszynowego uczenia można znaleźć w [Centrum dokumentacji szkoleniowe maszynowego Azure](https://azure.microsoft.com/documentation/services/machine-learning/).

Eksperyment typowe szkolenie składa się z następujących czynności:

1. Tworzenie eksperyment **+ Nowe** .
2. Uzyskiwanie danych do Azure ML.
3. Wstępne przetwarzanie, przekształcanie i manipulować danych zgodnie z potrzebami.
4. Generowanie funkcje zgodnie z potrzebami.
5. Dane można podzielić zestawy szkolenia i i sprawdzanie poprawności danych (lub osobne zestawy danych dla każdej z nich).
6. Wybierz jeden lub więcej algorytmów nauki komputera w zależności od problemu nauki do rozwiązania. Przykład klasyfikacji binarne, multiclass klasyfikacji, regresji.
7. Przeszkolenie jednego lub kilku modeli przy użyciu zestawu danych szkolenie.
8. Wynik sprawdzania poprawności zestawu danych przy użyciu przeszkolony modeli.
9. Szacowanie modele do obliczenia odpowiednich metryki problemu szkoleniowe.
10. Precyzyjnie dostosować modele i wybierz najlepsze modelu do wdrożenia.

W tym wykonywania mamy już zbadane i odtwarzane danych w magazynie danych SQL i danego na wielkością próbki mogły zjeść tej ostatniej w Azure ML. Poniżej przedstawiono procedurę tworzenia jednej lub większej liczby modeli przewidywania:

1. Uzyskiwanie danych do używania [Importowanie danych] ML Azure[ import-data] modułu dostępnych w sekcji **dane wejściowe i wyjściowe** . Aby uzyskać więcej informacji, zobacz [Importowanie danych] [ import-data] odwołania modułu do stron.

    ![Azure ML importowanie danych][17]

2. Jako **źródło danych** w panelu **Właściwości** wybierz **Bazy danych SQL Azure** .

3. Wpisz nazwę serwera DNS bazy danych w polu **Nazwa serwera bazy danych** . Format:`tcp:<your_virtual_machine_DNS_name>,1433`

4. Wprowadź **nazwę bazy danych** w odpowiednich pól.

5. Wprowadź *nazwę użytkownika programu SQL* **Server nazwy konta użytkownika**i *hasło* w polu **hasło do konta użytkownika serwera**.

6. Zaznacz opcję **Zaakceptuj wszystkie certyfikat** .

7. W obszarze **kwerendy w bazie danych** edycji tekstu Wklej kwerendę, która wyodrębnia odpowiednie pola (w tym wszelkie obliczoną pól, takich jak etykiety) bazy danych i w dół próbki danych do odpowiedniej próbka.

Przykład doświadczenia dwójkową klasyfikacji odczytywanie danych bezpośrednio z bazy danych SQL Data Warehouse jest na poniższej ilustracji (należy pamiętać o nyctaxi_trip nazwy tabeli i zastąpić nyctaxi_fare nazwę schematu i nazw tabel używane w Twojej instruktażu). Podobne doświadczeń można utworzyć dla klasyfikacji multiclass i problemów regresji.

![Azure pociągu ML][10]

> [AZURE.IMPORTANT] Modelowanie danych wyodrębniania i pobierania próbek przykłady kwerendy opisane w poprzedniej sekcji, a **wszystkie etykiety ćwiczeń modelowania trzy są uwzględnione w kwerendzie**. Ważny krok (wymagane) w każdym ćwiczeń modelowania jest **wykluczyć** niepotrzebne etykiet dla innych problemów i wszelkie inne **przeciek docelowej**. Na przykład przy użyciu binarne klasyfikacji, użyj etykiety **Przechylony** i wykluczyć pola **Porada\_klasy**, **Porada\_kwota**, i **sumy\_kwota**. Te ostatnie są przecieki docelowej od nich służą do przedstawiania poradę zapłacone.
>
> Aby wykluczyć niepotrzebne kolumny lub docelowego przecieków, można użyć [Wybieranie kolumn w zestawie danych] [ select-columns] modułu lub [Edytować metadane][edit-metadata]. Aby uzyskać więcej informacji, zobacz [Wybieranie kolumn w zestawie danych] [ select-columns] i [Edytować metadane] [ edit-metadata] odwołanie stron.

## <a name="mldeploy"></a>Wdrażanie modeli w nauki maszynowego Azure

Gdy model będzie gotowa, możesz łatwo wdrożyć go jako usługi sieci web bezpośrednio z doświadczenia. Aby uzyskać więcej informacji na temat wdrażania usługi sieci web Azure ML zobacz temat [Deploy usługi sieci web Azure maszynowego uczenia](machine-learning-publish-a-machine-learning-web-service.md).

Aby wdrożyć Nowa usługa sieci web, należy:

1. Tworzenie eksperyment wyników.
2. Wdrażanie usługi sieci web.

Aby utworzyć eksperyment wyników z eksperyment szkolenie **zakończone** , kliknij pozycję **Tworzenie wyników POEKSPERYMENTOWAĆ** na dolnym pasku akcji.

![Azure wyników][18]

Azure maszynowego uczenia spróbuje utworzyć eksperyment wyników według składników doświadczenia szkolenie. W szczególności będzie:

1. Zapisz model przeszkolony i Usuń modułów szkoleniowych modelu.
2. Identyfikowanie logiczny **port wejściowy** reprezentować schematem oczekiwany danych wejściowych.
3. Identyfikowanie logiczny **port wyjściowy** oznaczający schemat danych wyjściowych usługi sieci web oczekiwany.

Po utworzeniu wyników doświadczenia z nią zapoznać i upewnij się, ustaw stosownie do potrzeb. Typowe korekty ma zastąpić zestawu danych wejściowych i/lub kwerendy które wyłącza pola etykiet, jak te nie będą dostępne, gdy usługa jest wywoływana. Należy również zaleca się zmniejszyć rozmiar zestawu danych wejściowych i/lub kwerendy do kilku rekordów, po prostu za mało oznaczającą schemacie wprowadzania danych. Port wyjścia w przypadku typowych wykluczyć wszystkie wejściowych i zawierać tylko **Uzyskał etykiety** i **Prawdopodobieństwa uzyskał** w wyniku kwerendy przy użyciu [Wybierz kolumny w zestawie danych] [ select-columns] modułu.

Przykładowy wyników doświadczenia jest dostarczany na poniższej ilustracji. Gdy gotowe do wdrożenia, kliknij przycisk **PUBLIKUJ usługi sieci WEB** na pasku akcji niższy.

![Publikowanie Azure ML][11]


## <a name="summary"></a>Podsumowanie
Do recap, co możemy została utworzona w tym samouczku Instruktaż, utworzono środowisku nauki Azure danych pracy z dużego zestawu danych publicznych, zabierz przez proces zespołu nauki danych, od nabycia danych do modelu szkolenia, a następnie do wdrożenia usługi sieci web Azure maszynowego uczenia.

### <a name="license-information"></a>Informacje o licencji

W tym instruktażu próbki i jego towarzyszących skrypty i IPython notebook(s) udostępnianych przez firmę Microsoft w obszarze licencja. Sprawdź, czy plik LICENSE.txt w katalogu przykładowy kod na GitHub, aby uzyskać więcej informacji.

## <a name="references"></a>Odwołania

• [Podróży taksówki Warszawa Andrés Monroy pobieranie strony](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing osoby Warszawa taksówki podróży danych według Whong Tomasza](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Taksówki Warszawa oraz Limousine prowizji badań i statystyki](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
