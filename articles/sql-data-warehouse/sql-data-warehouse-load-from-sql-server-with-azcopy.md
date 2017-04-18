<properties
   pageTitle="Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (PolyBase) | Microsoft Azure"
   description="Używa bcp, aby wyeksportować dane z programu SQL Server do prostym plików, AZCopy, aby zaimportować dane z magazynem obiektów blob platformy Azure i PolyBase, aby mogły zjeść tej ostatniej danych do magazynu danych SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a>Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (AZCopy)

Aby załadować dane z programu SQL Server z magazynem obiektów blob platformy Azure za pomocą bcp i narzędzi wiersza polecenia AZCopy. Następnie użyj PolyBase lub Factory danych Azure, aby załadować dane do magazynu danych SQL Azure. 


## <a name="prerequisites"></a>Wymagania wstępne

Aby kroków ten samouczek, należy następująco:

- Bazy danych SQL magazynu danych
- Narzędzia wiersza polecenia bcp zainstalowany
- Narzędzia wiersza polecenia SQLCMD zainstalowany

>[AZURE.NOTE] Narzędzia bcp i sqlcmd można pobrać z [Centrum pobierania Microsoft][].

## <a name="import-data-into-sql-data-warehouse"></a>Importowanie danych do magazynu danych SQL

W tym samouczku możesz utworzyć tabelę w magazynie danych SQL Azure i zaimportować dane do tabeli.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Krok 1: Tworzenie tabeli w magazynie danych SQL Azure

W wierszu polecenia Uruchom następujące kwerendę, aby utworzyć tabelę w programie za pomocą sqlcmd:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```

>[AZURE.NOTE] Aby uzyskać więcej informacji o tworzeniu tabeli na magazynu danych SQL i opcji dostępnych w klauzuli WITH, zobacz [Omówienie tabeli][] lub [CREATE TABLE składni][] .

### <a name="step-2-create-a-source-data-file"></a>Krok 2: Tworzenie pliku źródła danych

Otwórz program Notatnik i skopiuj następujące wiersze danych do nowego pliku tekstowego, a następnie zapisz ten plik w lokalnym katalogu tymczasowym, C:\Temp\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [AZURE.NOTE] Należy należy pamiętać, że tego bcp.exe nie obsługuje kodowania UTF-8 pliku. Użyj plików ASCII lub UTF-16 zakodowany podczas korzystania z bcp.exe.

### <a name="step-3-connect-and-import-the-data"></a>Krok 3: Łączenia i importowania danych
Przy użyciu bcp, można łączyć i importowanie danych przy użyciu następującego polecenia zamienianiu wartości zależnie od potrzeb:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Można sprawdzić, czy dane załadowania, uruchamiając poniższe zapytanie przy użyciu sqlcmd:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Czy to zwracają następujące wyniki:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Krok 4: Tworzenie statystyk na załadowaniu danych

Magazyn danych SQL Azure czy jeszcze nie pomocy technicznej automatyczne tworzenie lub automatycznej aktualizacji statystyki. W celu uzyskania najlepszej wydajności należy przejść z zapytań, należy pamiętać, że statystyki można tworzyć na wszystkich kolumn wszystkie tabele po załadowaniu pierwszego lub wystąpienia jakichkolwiek znacznych zmian w danych. Aby uzyskać szczegółowy opis statystyki zobacz temat [Statystyki][] w grupie opracowanie tematów. Oto szybki przykład umożliwiające utworzenie tabeli statystyki załadowanego w tym przykładzie

W wierszu sqlcmd, należy wykonać następujące instrukcje Tworzenie statystyki:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Eksportowanie danych z magazynu danych SQL
W tym samouczku utworzy plik danych z tabeli w magazynie danych SQL. Firma Microsoft będzie Wyeksportuj dane utworzone powyżej do nowego pliku danych o nazwie DimDate2_export.txt.

### <a name="step-1-export-the-data"></a>Krok 1: Wyeksportuj dane

Za pomocą narzędzia bcp, można łączyć i eksportowanie danych przy użyciu następującego polecenia zamienianiu wartości zależnie od potrzeb:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Można sprawdzić, czy został prawidłowo wyeksportowane dane przez otwarcie nowego pliku. Dane w pliku powinny być zgodne tekst poniżej:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

>[AZURE.NOTE] Ze względu na charakter systemów dystrybucji porządkowania danych może nie być taka sama całej bazy danych SQL magazynu danych. Innym rozwiązaniem jest za pomocą funkcji **queryout** bcp pisanie Wyodrębnij kwerendy, a nie wyeksportować całą tabelę.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać omówienie ładowania zobacz [ładowania danych do magazynu danych SQL][].
Aby uzyskać więcej porad rozwoju zobacz [Omówienie rozwoju magazynu danych SQL][].

<!--Image references-->

<!--Article references-->

[Ładowanie danych do magazynu danych SQL]: ./sql-data-warehouse-overview-load.md
[Omówienie tworzenia magazynu danych SQL]: ./sql-data-warehouse-overview-develop.md
[Omówienie tabeli]: ./sql-data-warehouse-tables-overview.md
[Statystyki]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[Tworzenie tabeli składni]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Centrum pobierania Microsoft]: https://www.microsoft.com/download/details.aspx?id=36433
