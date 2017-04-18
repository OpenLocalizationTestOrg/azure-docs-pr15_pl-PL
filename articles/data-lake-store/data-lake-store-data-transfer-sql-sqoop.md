<properties 
   pageTitle="Kopiowanie danych między magazynu Lake danych i baza danych Azure SQL za pomocą Sqoop | Microsoft Azure"
   description="Kopiowanie danych między bazy danych SQL Azure i magazynu Lake danych przy użyciu Sqoop" 
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
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Kopiowanie danych między magazynu Lake danych i baza danych Azure SQL za pomocą Sqoop

Dowiedz się, jak za pomocą Apache Sqoop importowanie i eksportowanie danych między bazy danych SQL Azure i magazynu Lake danych.
 

## <a name="what-is-sqoop"></a>Co to jest Sqoop?

Duży dane aplikacji są naturalne wybór przetwarzania niestrukturalne danych strukturalnych i półstrukturalnych, takich jak dzienniki i pliki. Jednak może istnieć potrzeba przetwarzania danych strukturalnych, który jest przechowywany w relacyjnych baz danych.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) to narzędzie przeznaczone do przenoszenia danych między relacyjnych baz danych i repozytorium duży danych, takich jak magazyn Lake danych. Jego umożliwia importowanie danych z systemu zarządzania relacyjnej bazy danych (RDBMS), takich jak bazy danych SQL Azure do magazynu Lake danych. Można, a następnie Przekształcanie i analizowanie danych za pomocą obciążenia duży danych i wyeksportuj dane do RDBMS. W tym samouczku umożliwia bazy danych SQL Azure jako relacyjne bazy danych import/eksport z.
 

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).
- **Włączanie Azure subskrypcji** dla magazynu Lake danych publicznych Podgląd. Zobacz [instrukcje](data-lake-store-get-started-portal.md#signup). 
- **Usługa azure HDInsight klaster** z dostępu do konta magazynu Lake danych. Zobacz [Tworzenie klastrze HDInsight z magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md). W tym artykule założono, że masz klaster HDInsight Linux z magazynu Lake danych programu access.
- **Baza danych SQL azure**. Aby uzyskać instrukcje, jak go utworzyć zobacz [Tworzenie bazy danych programu Azure SQL](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Uzyskać szybkie z klipów wideo?

[Obejrzyj ten klip wideo](https://mix.office.com/watch/1butcdjxmu114) na temat przenoszenia danych między obiektami blob miejsca do magazynowania Azure i magazynu Lake danych przy użyciu DistCp.

## <a name="create-sample-tables-in-the-azure-sql-database"></a>Tworzenie przykładowych tabel w bazie danych SQL Azure

1. Zaczynać się utworzyć dwa przykładowe tabele w bazie danych SQL Azure. Aby nawiązać połączenie z bazą danych SQL Azure, a następnie uruchom następujące kwerendy za pomocą [Programu SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) lub Visual Studio.

    **Tworzenie Tabela1**

        CREATE TABLE [dbo].[Table1]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

    **Tworzenie tabela2**

        CREATE TABLE [dbo].[Table2]( 
        [ID] [int] NOT NULL, 
        [FName] [nvarchar](50) NOT NULL, 
        [LName] [nvarchar](50) NOT NULL, 
        CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED 
            ( 
                [ID] ASC 
            ) 
        ) ON [PRIMARY] 
        GO

2. W **tabeli Tabela1**Dodaj kilka przykładowych danych. **Tabela2** pozostaw puste. Firma Microsoft będą importowane dane z **tabeli Table1** do magazynu Lake danych. Następnie firma Microsoft będzie eksportować dane z magazynu Lake danych do **tabela2**. Uruchom następujące wstawek.

         
        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson'); 
  

## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-to-data-lake-store"></a>Za pomocą Sqoop w klastrze HDInsight z dostępem do magazynu Lake danych

Klaster HDInsight jest już dostępne pakiety Sqoop. Jeśli skonfigurowano klaster HDInsight w celu używania magazynu Lake danych jako dodatkowego miejsca do magazynowania, umożliwia Sqoop (bez zmiany konfiguracji) import/eksport danych między relacyjnej bazy danych (w tym przykładzie bazy danych SQL Azure) i kontem magazynu Lake danych. 

1. Ten samouczek możemy założono, że tworzone klastrze Linux, więc należy używać SSH, aby połączyć się z klastrem. Zobacz [Łączenie z klastrem HDInsight systemem Linux](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).

2. Sprawdź, czy można uzyskać dostęp do konta magazynu Lake danych z klaster. Uruchom następujące polecenie w wierszu SSH:

        
        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    To powinien zawierać listę plików i folderów w oknie konta magazynu Lake danych.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Importowanie danych z bazy danych SQL Azure do magazynu Lake danych

3. Przejdź do katalogu, w którym pakiety Sqoop są dostępne. Zazwyczaj są to w `/usr/hdp/<version>/sqoop/bin`. 

4. Importowanie danych z **tabeli Table1** do konta magazynu Lake danych. Użyj następującej składni:

        
        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Należy zauważyć, że ten symbol zastępczy **sql--nazwa serwera bazy danych —** oznacza nazwę serwera, na którym jest uruchomiona baza danych Azure SQL. **Nazwa bazy danych SQL** symbol zastępczy reprezentuje nazwę rzeczywistej bazy danych.

    Na przykład

        
        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

5. Upewnij się, że dane został przeniesiony do konta magazynu Lake danych. Uruchom następujące polecenie:

        
        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Powinien zostać wyświetlony następujący wynik.

        
        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Każdy **części-m -** * pliku odpowiada wiersza z tabeli źródłowej * *tabela1**. Można wyświetlać zawartość części-m -* plików, aby sprawdzić.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Wyeksportuj dane z magazynu Lake danych do bazy danych SQL Azure

6. Wyeksportuj dane z konta magazynu Lake danych do pustej tabeli, **tabela2**w bazie danych SQL Azure. Należy użyć następującej składni.

        
        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Na przykład

        
        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

6. Upewnij się, że dane przekazano do tabeli bazy danych SQL. Aby nawiązać połączenie z bazą danych SQL Azure, a następnie uruchom poniższe zapytanie za pomocą [Programu SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) lub Visual Studio.

        
        SELECT * FROM TABLE2

    To powinny mieć następujące wyniki.

        ID  FName   LName
        ------------------
        1   Neal    Kell
        2   Lila    Fulton
        3   Erna    Myers
        4   Annette Simpson

## <a name="see-also"></a>Zobacz też

- [Skopiuj dane z obiektami blob miejsca do magazynowania Azure do magazynu Lake danych](data-lake-store-copy-data-azure-storage-blob.md)
- [Zabezpieczanie danych w magazynie Lake danych](data-lake-store-secure-data.md)
- [Używanie analizy Lake Azure danych z magazynu Lake danych](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Usługa Azure HDInsight za pomocą magazynu Lake danych](data-lake-store-hdinsight-hadoop-use-portal.md)
