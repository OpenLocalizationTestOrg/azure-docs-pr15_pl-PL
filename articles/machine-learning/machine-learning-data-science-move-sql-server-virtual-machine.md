<properties 
    pageTitle="Przenoszenie danych do programu SQL Server na Azure maszyn wirtualnych | Azure" 
    description="Przenoszenie danych z plików z prostym lub z lokalnego programu SQL Server do programu SQL Server na maszyn wirtualnych Azure." 
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
    ms.date="09/14/2016" 
    ms.author="bradsev" /> 

# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Przenoszenie danych do programu SQL Server na Azure maszyn wirtualnych

W tym temacie opisano opcje przenoszenia danych z plików prostych (format CSV lub TSV) lub z lokalnego programu SQL Server do programu SQL Server na Azure maszyn wirtualnych. W ramach procesu nauki danych zespołu są te zadania przenoszenia danych do chmury.

Temat wokół opcji przenoszenia danych do bazy danych SQL Azure szkoleniowe na komputerze zobacz [Przenoszenie danych do bazy danych SQL Azure Azure szkoleniowe na komputerze](machine-learning-data-science-move-sql-azure.md).

**Menu** poniżej łącza do tematów z opisem mogły zjeść tej ostatniej danych do innych środowisk docelowej miejsce, w którym dane można przechowywane i przetwarzane podczas procesu nauki danych zespołu (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


W poniższej tabeli przedstawiono opcje przenoszenia danych do programu SQL Server na Azure maszyn wirtualnych.

<b>ŹRÓDŁA</b> |<b>Miejsce docelowe: Programu SQL Server na Azure maszyn wirtualnych</b> |
------------------ |-------------------- |
<b>Prosty plik</b> |1. <a href="#insert-tables-bcp">zbiorcze wiersza polecenia Kopiuj narzędzie (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">zbiorczo Wstaw SQL kwerendy</a><br> 3. <a href="#sql-builtin-utilities">wbudowanych narzędzi graficznych w programie SQL Server</a>
<b>Lokalnego programu SQL Server</b> | 1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Rozmieszczanie bazy danych programu SQL Server w Kreatorze maszyn wirtualnych programu Microsoft Azure</a><br> 2. <a href="#export-flat-file">Eksportowanie z plikiem prostym</a><br> 3. <a href="#sql-migration">Kreator migracji bazy danych SQL</a> <br> 4. <a href="#sql-backup">bazy danych kopii zapasowych i przywracanie</a><br>

Zauważ, że ten dokument przyjęto założenie, że polecenia SQL są wykonywane z programu SQL Server Management Studio lub Visual Studio Eksplorator bazy danych.

> [AZURE.TIP] Alternatywnie [Azure fabrycznych danych](https://azure.microsoft.com/services/data-factory/) służy do tworzenia i planowania procesu, który powoduje przeniesienie danych do maszyny serwera SQL Azure. Aby uzyskać więcej informacji zobacz [Kopiowanie danych za pomocą Azure danych Factory (czynność Kopiuj)](../data-factory/data-factory-data-movement-activities.md).


## <a name="prereqs"></a>Wymagania wstępne
Tego samouczka przyjęto założenie, że masz:

* **Azure subskrypcji**. Jeśli nie masz subskrypcji, można Załóż [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
* **Konto Azure miejsca do magazynowania**. Konto Azure magazynowania użyje do przechowywania danych w tym samouczku. Jeśli nie masz konta usługi Azure miejsca do magazynowania, zobacz artykuł [Tworzenie konta miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account) . Po utworzeniu konta miejsca do magazynowania, musisz uzyskać klucz konta używanego do uzyskiwania dostępu do przechowywania. Zobacz [Wyświetlanie, Kopiuj i klawisze dostępu wyniku miejsca do magazynowania](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Ustanawianie **programu SQL Server na Azure maszyn wirtualnych**. Aby uzyskać instrukcje zobacz [Konfigurowanie maszyn wirtualnych Azure SQL Server jako serwer notesu IPython zaawansowanej analizy](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Zainstalowane i skonfigurowane **Azure programu PowerShell** lokalnie. Aby uzyskać instrukcje zobacz [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).


## <a name="filesource_to_sqlonazurevm"></a>Przenoszenie danych ze źródła prosty plik do programu SQL Server na maszyn wirtualnych Azure

Jeśli dane znajdują się w pliku prostego (rozmieszczone w formacie wierszy i kolumn), można ją przenosić do maszyn wirtualnych programu SQL Server Azure za pomocą następujących metod:

1. [Zbiorcze wiersza polecenia Kopiuj narzędzie (BCP)](#insert-tables-bcp) 
2. [Kwerenda SQL Wstawianie zbiorcze](#insert-tables-bulkquery)
3. [Graficzne narzędzia wbudowanych w program SQL Server (Importuj/Eksportuj, SSIS)](#sql-builtin-utilities)


### <a name="insert-tables-bcp"></a>Zbiorcze wiersza polecenia Kopiuj narzędzie (BCP)

BCP jest narzędziem wiersza polecenia zainstalowano z programem SQL Server i jest jednym ze sposobów najszybszą przenoszenie danych. Działa na wszystkie trzy warianty programu SQL Server (lokalnego programu SQL Server, platformy SQL Azure i maszyn wirtualnych serwera SQL Azure). 

> [AZURE.NOTE]**Miejsce, w którym dane należy dla BCP?**  
> Gdy nie jest wymagane, o zbiorach danych źródłowych znajduje się na tym samym komputerze co program SQL server docelowej umożliwia szybsze przeniesienia (sieć szybkości w porównaniu z lokalnym dysku szybkości ei). Możesz przenieść pliki płaskie, zawierający dane do komputera w przypadku, gdy program SQL Server jest instalowany za pomocą kopiowania narzędzi, takich jak [AZCopy](../storage/storage-use-azcopy.md)różnych plików, [Eksplorator magazynu Azure](http://storageexplorer.com/) lub windows skopiować/wkleić za pośrednictwem protokołu RDP (Remote Desktop).

1. Upewnij się, że baza danych i tabele są tworzone w docelowej bazy danych programu SQL Server. Oto przykładowy sposobów wykonywania tego za pomocą `Create Database` i `Create Table` polecenia:

        CREATE DATABASE <database_name>
    
        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        ) 
    
2. Generowanie pliku formatu, opisującą schemat tabeli wysyłając poniższe polecenie w wierszu polecenia na komputerze zainstalowanego bcp.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n` 

3. Wstawianie danych do bazy danych za pomocą polecenia bcp w następujący sposób. Czy to praca z wiersza polecenia przy założeniu, że na tym samym komputerze jest zainstalowany program SQL Server:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Optymalizacja BCP wstawia** Przeczytaj następujący artykuł ["Wskazówki dotyczące optymalizowania importu zbiorczego"](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) zoptymalizować takie zostanie wstawiony.


### <a name="insert-tables-bulkquery-parallel"></a>Parallelizing wstawia dla szybsze przepływu danych

Jeśli dane, które chcesz przenieść jest duży, możesz może przyspieszyć przez jednoczesne wykonywanie wielu poleceń BCP równolegle w skrypt programu PowerShell.

> [AZURE.NOTE]**Duży danych spożyciu** Aby zoptymalizować dane ładowanie dla dużych i bardzo dużych zbiorów danych, podzielić tabel logicznych i fizycznych bazy danych przy użyciu wielu tabel grup i partycją. Aby uzyskać więcej informacji na temat tworzenia i ładowanie danych do tabel partition zobacz [Równoległe tabele Partition SQL ładowania](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).


Poniższy przykładowy skrypt programu PowerShell wykazać równoległe zostanie wstawiony za pomocą bcp:
    
    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    

    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    

    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>Kwerenda SQL Wstawianie zbiorcze

[Zbiorcze Wstawianie zapytania SQL](https://msdn.microsoft.com/library/ms188365) może służyć do importowania danych do bazy danych na podstawie wiersz/kolumnę plików (obsługiwane typy są objęte[Przygotowywanie danych dla zbiorcze eksportowanie lub importowanie (program SQL Server)](https://msdn.microsoft.com/library/ms188609)) tematu. 

Poniżej przedstawiono niektóre polecenia próbki dla Wstawianie zbiorcze są, jak pokazano poniżej:  

1. Analizowanie danych i niestandardowych opcji przed zaimportowaniem, aby upewnić się, że baza danych programu SQL Server przyjęto założenie, takim samym formatem dla wszystkich pól specjalne, takie jak daty. Oto przykładowy sposób ustawić format daty jako rok, miesiąc, dzień (Jeśli dane zawierają datę w formacie rok, miesiąc, dzień):

        SET DATEFORMAT ymd; 
    
2. Importowanie danych za pomocą instrukcje importu zbiorczego:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )
      

### <a name="sql-builtin-utilities"></a>Wbudowane narzędzia w programie SQL Server

Program SQL Server integracji Services (SSIS) umożliwia importowanie danych do maszyn wirtualnych serwera SQL Azure z plikiem prostym. SSIS jest dostępne w dwóch środowiskach studio. Aby uzyskać szczegółowe informacje zobacz [Integration Services (SSIS) i w środowisku Studio](https://technet.microsoft.com/library/ms140028.aspx):

- Aby uzyskać szczegółowe informacje na program SQL Server Data Tools zobacz [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
- Aby uzyskać szczegółowe informacje na Kreator importu i eksportu zobacz [Kreatora programu SQL Server importu i eksportu](https://msdn.microsoft.com/library/ms141209.aspx)


## <a name="sqlonprem_to_sqlonazurevm"></a>Przenoszenie danych z lokalnego programu SQL Server do programu SQL Server na Azure maszyn wirtualnych

Można również użyć następujących strategii migracji:

1. [Wdrażanie bazy danych programu SQL Server Kreator maszyn wirtualnych programu Microsoft Azure](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Eksportowanie do pliku prostego](#export-flat-file) 
3. [Kreator migracji bazy danych SQL](#sql-migration)
4. [Bazy danych kopii zapasowych i przywracanie](#sql-backup)

Opisano każdy z tych poniżej:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Wdrażanie bazy danych programu SQL Server do kreatora maszyn wirtualnych programu Microsoft Azure

**Rozmieszczanie bazy danych programu SQL Server w Kreatorze maszyn wirtualnych programu Microsoft Azure** jest proste i zalecanych umożliwia przeniesienie danych z lokalnego wystąpienia programu SQL Server do programu SQL Server na maszyn wirtualnych Azure. Aby uzyskać szczegółowe instrukcje, jak również zapoznać się z omówieniem alternatywnych zobacz [Migrowanie bazy danych programu SQL Server na maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Eksportowanie do pliku prostego

Różne metody może służyć do zbiorczego Eksportuj dane z programu SQL Server w środowisku lokalnym, jak opisano w temacie [importu zbiorczego i eksportowanie danych o (program SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) . Ten dokument będzie obejmować Program Kopiuj zbiorczo (BCP) jako przykład. Gdy dane są eksportowane do pliku prostego, można zaimportować do innego programu SQL server przy użyciu importu zbiorczego. 

1. Eksportowanie danych z lokalnego programu SQL Server do pliku przy użyciu narzędzia bcp w następujący sposób

    `bcp dbname..tablename out datafile.tsv -S  servername\sqlinstancename -T -t \t -t \n -c`

2. Tworzenie bazy danych i tabeli na maszyn wirtualnych programu SQL Server przy użyciu Azure `create database` i `create table` dla schematu tabeli wyeksportowane w kroku 1.

3. Utwórz plik w formacie opisu schematu tabeli dane są eksportowane zaimportowane. Szczegóły plik w formacie są opisane w sekcji [Tworzenie pliku formatu (program SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx).

    Formatowanie generowania plików podczas uruchamiania BCP z komputera programu SQL Server 

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    Formatowanie generowania plików podczas uruchamiania BCP zdalnie przed programu SQL Server 

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
        
    
4. Aby przenieść dane w plikach płaskie z programem SQL Server za pomocą dowolnej z metod opisanych w sekcji [Przenoszenie danych z pliku źródła](#filesource_to_sqlonazurevm) .

### <a name="sql-migration"></a>Kreator migracji bazy danych SQL

[Kreator migracji bazy danych programu SQL Server](http://sqlazuremw.codeplex.com/) zawiera zrozumiałe dla użytkownika umożliwia przenoszenie danych między dwa wystąpienia serwera SQL. Umożliwia użytkownikowi do zamapować schemat danych między źródeł i tabele docelowe, wybierz typy kolumn i różne inne funkcje. Używa kopiowania zbiorczego (BCP) w obszarze okładki. Poniżej przedstawiono zrzut ekranu przedstawiający ekran powitalny Kreatora migracji bazy danych SQL.  

![Kreator migracji programu SQL Server][2]

### <a name="sql-backup"></a>Bazy danych kopii zapasowych i przywracanie

Obsługa programu SQL Server: 

1. [Bazy danych kopii zapasowych i przywracanie funkcjonalności](https://msdn.microsoft.com/library/ms187048.aspx) (zarówno do pliku lub lokalnego Plecak eksportowanie do obiektów blob) i [Aplikacje warstwy danych](https://msdn.microsoft.com/library/ee210546.aspx) (za pomocą Plecak). 
2. Możliwość bezpośrednio tworzenia maszyny wirtualne serwera SQL Azure za pomocą skopiowany bazy danych lub Kopiuj do istniejącej bazy danych platformy SQL Azure. Aby uzyskać więcej informacji zobacz [Używanie kreatora kopiowania baz danych](https://msdn.microsoft.com/library/ms188664.aspx). 

Zrzut ekranu przedstawiający kopii bazy danych w górę/Przywracanie opcji z programu SQL Server Management Studio przedstawiono poniżej.

![Narzędzie importowania programu SQL Server][1]

## <a name="resources"></a>Zasoby

[Migrowanie bazy danych do programu SQL Server na Azure maszyn wirtualnych](../virtual-machines/virtual-machines-windows-migrate-sql.md)

[Program SQL Server w środowisku maszyn wirtualnych systemu Azure — omówienie](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
