<properties
   pageTitle="Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (bcp) | Microsoft Azure"
   description="W przypadku małych danych, rozmiar używa bcp eksportowanie danych z programu SQL Server do plików prostych i zaimportować dane bezpośrednio do magazynu danych SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Ładowanie danych z programu SQL Server do magazynu danych SQL Azure (pliki płaskie)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Dla małych zestawów danych narzędzie wiersza polecenia bcp służy do eksportowania danych z programu SQL Server, a następnie załadować je bezpośrednio do magazynu danych SQL Azure.

W tym samouczku użyje bcp do:

- Eksportowanie tabeli z z programu SQL Server przy użyciu bcp polecenie (lub tworzenie prostego przykładowy plik)
- Importowanie tabeli z plikiem prostym do magazynu danych SQL.
- Tworzenie statystyki na pobrane dane.

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="before-you-begin"></a>Przed rozpoczęciem

### <a name="prerequisites"></a>Wymagania wstępne

Aby kroków ten samouczek, należy następująco:

- Bazy danych SQL magazynu danych
- Narzędzie wiersza polecenia bcp zainstalowany
- Narzędzie wiersza polecenia sqlcmd zainstalowany

Narzędzia bcp i sqlcmd można pobrać z [Centrum pobierania Microsoft][].

### <a name="data-in-ascii-or-utf-16-format"></a>Dane w formacie ASCII, lub UTF-16

Jeśli próbujesz ten samouczek w swoich danych, danych musi być ASCII lub UTF-16 kodowanie, ponieważ bcp nie obsługuje UTF-8. 

PolyBase obsługuje UTF-8, ale jeszcze nie obsługuje UTF-16. Należy zauważyć, że jeśli chcesz połączyć bcp z PolyBase będzie konieczne przekształcać dane UTF-8, gdy jest eksportowany z programu SQL Server. 


## <a name="1-create-a-destination-table"></a>1. Utwórz tabelę docelową

Definiowanie tabeli w magazynie danych SQL, który będzie tabeli docelowej dla Załaduj. Dane w każdym wierszu pliku danych musi odpowiadać kolumn w tabeli.

Aby utworzyć tabelę, otwórz wiersz polecenia i umożliwia sqlcmd.exe uruchom następujące polecenie:


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


## <a name="2-create-a-source-data-file"></a>2. Tworzenie pliku źródła danych

Otwórz program Notatnik i skopiuj następujące wiersze danych do nowego pliku tekstowego, a następnie zapisz ten plik w lokalnym katalogu tymczasowym, C:\Temp\DimDate2.txt. Te dane są w formacie ASCII.

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

(Opcjonalnie) Aby wyeksportować własnych danych z bazy danych programu SQL Server, otwórz wiersz polecenia, a następnie uruchom następujące polecenie. Zamień TableName, nazwa_serwera DatabaseName, nazwę użytkownika i hasło na własne informacje.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a>3. ładowanie danych
Aby załadować dane, otwórz wiersz polecenia i uruchom następujące polecenie, zastępując wartości nazwy serwera, nazwę bazy danych, nazwę użytkownika i hasło na własne informacje.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Użyj tego polecenia, aby sprawdzić, czy dane zostały poprawnie załadowane

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Wyniki powinna wyglądać następująco:

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

## <a name="4-create-statistics"></a>4. Tworzenie statystyki

Magazynu danych SQL czy jeszcze nie Autotworzenie pomocy technicznej lub automatycznej aktualizacji statystyki. Aby uzyskać najlepszą wydajność kwerendy, należy utworzyć statystyki na wszystkich kolumn wszystkie tabele, po pierwszym Załaduj lub po wprowadzeniu dowolnego znacznych zmian w danych. Aby uzyskać szczegółowy opis statystyki zobacz [Statystyka][]. 

Uruchom następujące polecenie, aby utworzyć statystyki na załadowaniu tabeli.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. Eksportowanie danych z magazynu danych SQL
Kolekcja możesz wyeksportować dane, które właśnie załadować ponownie poza magazynu danych SQL.  Polecenie Eksportuj dokładnie jest taka sama, jak eksportowanie z programu SQL Server.

Istnieje jednak różnica w wynikach. Ponieważ dane są przechowywane w lokalizacji zdalnej w obrębie magazynu danych SQL, podczas eksportowania danych każdego węzła obliczeń zapisuje danych w pliku docelowym. Kolejność danych w pliku wyjściowym prawdopodobnie może różnić się od kolejności danych w pliku wejściowego.

### <a name="export-a-table-and-compare-exported-results"></a>Eksportowanie tabeli i porównywanie wyników wyeksportowane

Aby wyświetlić wyeksportowane dane, otwórz wiersz polecenia i tego polecenia za pomocą własnego parametry. Nazwa_serwera to nazwa serwera SQL Azure logiczne.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Można sprawdzić, czy został prawidłowo wyeksportowane dane przez otwarcie nowego pliku. Dane w pliku powinny być zgodne tekst poniżej, ale prawdopodobnie są sortowane w innej kolejności:

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

### <a name="export-the-results-of-a-query"></a>Eksportowanie wyników kwerendy

Funkcja **queryout** bcp umożliwia eksportowanie wyników kwerendy zamiast eksportowania całej tabeli. 

## <a name="next-steps"></a>Następne kroki
Aby uzyskać omówienie ładowania zobacz [ładowania danych do magazynu danych SQL][].
Aby uzyskać więcej porad rozwój zobacz [Omówienie tworzenia magazynu danych SQL][].
Aby uzyskać więcej informacji o tworzeniu tabeli w magazynie danych SQL, zobacz [Omówienie tabeli][] lub [CREATE TABLE składni][] .

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
