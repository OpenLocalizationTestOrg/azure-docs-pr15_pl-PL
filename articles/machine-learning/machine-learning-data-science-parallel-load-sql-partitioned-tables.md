<properties 
    pageTitle="Równoległe zbiorczego importowania danych za pomocą tabel partycji SQL | Microsoft Azure" 
    description="Równoległe masowego importu danych za pomocą tabel partycji SQL" 
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
    ms.author="bradsev" /> 

# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Równoległe masowego importu danych za pomocą tabel partycji SQL

Niniejszy dokument zawiera opis sposobu tworzenia tabel partycjonowanych fast równoległe zbiorczego importowania danych do bazy danych programu SQL Server. Dla dużych ilości danych załadunku/przekazywania do bazy danych SQL importowanie danych do bazy danych SQL i kolejne kwerendy można poprawić za pomocą _podzielonym na partycje tabel i widoków_. 


## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Utwórz nową bazę danych i zestaw grup plików

- [Utwórz nową bazę danych](https://technet.microsoft.com/library/ms176061.aspx) (Jeśli nie istnieje)
- Dodawanie aplikacjami bazy danych do bazy danych, które będzie sprawować podzielonym na partycje plików fizycznych

  Uwaga: Można to zrobić z [Tworzenie bazy danych](https://technet.microsoft.com/library/ms176061.aspx) , jeśli nowe lub [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) Jeśli już istnieje w bazie danych

- Dodaj jeden lub więcej plików (w razie potrzeby) do każdej grupy plików bazy danych

 > [AZURE.NOTE] Określ miejsce docelowe grupy plików, który zawiera dane dla tej partycji i nazwy plików fizycznej bazy danych, gdzie będą przechowywane dane dotyczące grupy plików.
 
Poniższy przykład tworzy nową bazę danych z trzech grup plików innych niż podstawowy i grupy dziennika, zawierające jednym fizycznym pliku w każdym. Pliki bazy danych są tworzone w domyślnym folderze danych programu SQL Server, zgodnie z konfiguracją w wystąpieniu programu SQL Server. Aby uzyskać więcej informacji na temat domyślne lokalizacje plików Zobacz [Lokalizacje domyślne i nazwanych wystąpień programu SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);
    
    EXECUTE ('
        CREATE DATABASE <database_name>
        ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
        LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')
    
## <a name="create-a-partitioned-table"></a>Tworzenie tabeli partycjonowanej

Tworzenie tabel partycjonowanych zgodnie ze schematem danych, mapowane do grup bazy danych utworzony w poprzednim kroku. Gdy dane są importowane do tabel partycjonowanych luzem, rekordów zostanie rozdzielony między aplikacjami według schematu partycji, jak opisano poniżej.

**Aby utworzyć tabelę partycji, należy:**

- [Utwórz funkcję partycji](https://msdn.microsoft.com/library/ms187802.aspx) , który definiuje zakres wartości/granice mają zostać uwzględnione w każdej tabeli poszczególnych partycji, np., aby ograniczyć partycje według miesiąca (niektóre\_datetime\_pola) w roku 2013:

        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

- [Utwórz schemat partycji](https://msdn.microsoft.com/library/ms179854.aspx) , który mapuje każdy zakres partycji w funkcji partycji fizycznej grupa plików, np.:

        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )

  Wskazówka: Aby sprawdzić zakresy obowiązujących w każdej partycji według funkcji/schemat, uruchom następującą kwerendę:

        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

- [Tworzenie tabeli partycjonowanej](https://msdn.microsoft.com/library/ms174979.aspx) (s) zgodne z schemat danych i określ pole schemat i ograniczenie partycji używana do partycji tabeli, np.:

        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Aby uzyskać więcej informacji zobacz [Tworzenie tabel podzielonym na partycje i indeksy](https://msdn.microsoft.com/library/ms188730.aspx).


## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Importu zbiorczego danych dla każdej tabeli poszczególnych partycji

- Można użyć narzędzia BCP, BULK INSERT lub innych metod, takich jak [Kreator migracji programu SQL Server](http://sqlazuremw.codeplex.com/). Podany przykład używa metody BCP.

- [Zmiany bazy danych,](https://msdn.microsoft.com/library/bb522682.aspx) Aby zmienić schemat rejestrowania transakcji na BULK_LOGGED, aby zminimalizować obciążenie związane z rejestrowania, np.:

        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED

- Aby przyspieszyć ładowanie danych, uruchamianie operacji importu zbiorczego jednocześnie. Aby uzyskać porady dotyczące usprawnienia luzem importowanie dużych ilości danych w bazach danych programu SQL Server zobacz [Ładowanie 1 TB w mniej niż 1 godzinę](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

Poniższy skrypt programu PowerShell jest przykładem danych równoległego załadunku, przy użyciu narzędzia BCP.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0
    
    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>
       
    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"
   
    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir
      
    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }
    
    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }
    
    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }
    
    Get-Job
    
    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Tworzenie indeksów w celu zoptymalizowania sprzężeń i wydajność kwerendy

- Jeśli będzie wyodrębnić dane do modelowania z wielu tabel, należy utworzyć indeksy na sprzężenie kluczy do poprawy wydajności sprzężenia.

- [Tworzenie indeksów](https://technet.microsoft.com/library/ms188783.aspx) (klastrowany lub nieklastrowany) kierowanie tej samej grupie plików dla każdej partycji, np.:

        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
lub,

        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)

 > [AZURE.NOTE] Użytkownik może utworzyć indeksy przed zbiorczego importowania danych. Tworzenie indeksu przed rozpoczęciem importu zbiorczego spowolni ładowania danych.


## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Zaawansowane narzędzia analityczne procesu pracy i technologii w przykładzie akcji

Na przykład wskazówki end-to-end z publicznego zestawu danych przy użyciu procesu Cortana Analytics, zobacz [Cortana Analytics procesu w akcji: przy użyciu programu SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
 
