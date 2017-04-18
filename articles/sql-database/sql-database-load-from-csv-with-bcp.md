<properties
   pageTitle="Ładowanie danych z pliku CSV do Databaase SQL Azure (bcp) | Microsoft Azure"
   description="W przypadku małych danych, rozmiar używa bcp do importowania danych do bazy danych SQL Azure."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>Ładowanie danych z pliku CSV do magazynu danych SQL Azure (pliki płaskie)

Narzędzie wiersza polecenia bcp umożliwia importowanie danych z pliku CSV do bazy danych SQL Azure.

## <a name="before-you-begin"></a>Przed rozpoczęciem

### <a name="prerequisites"></a>Wymagania wstępne

Aby kroków ten samouczek, należy następująco:

- Bazy danych SQL Azure logiczne serwer i bazę danych
- Narzędzie wiersza polecenia bcp zainstalowany
- Narzędzie wiersza polecenia sqlcmd zainstalowany

Narzędzia bcp i sqlcmd można pobrać z [Centrum pobierania Microsoft][].

### <a name="data-in-ascii-or-utf-16-format"></a>Dane w formacie ASCII, lub UTF-16

Jeśli próbujesz ten samouczek w swoich danych, danych musi być ASCII lub UTF-16 kodowanie, ponieważ bcp nie obsługuje UTF-8. 

## <a name="1-create-a-destination-table"></a>1. Utwórz tabelę docelową

Definiowanie tabeli w bazie danych SQL, jak tabela docelowa. Dane w każdym wierszu pliku danych musi odpowiadać kolumn w tabeli.

Aby utworzyć tabelę, otwórz wiersz polecenia i umożliwia sqlcmd.exe uruchom następujące polecenie:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
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


## <a name="next-steps"></a>Następne kroki

Aby przeprowadzić migrację bazy danych programu SQL Server, zobacz [Migracja bazy danych programu SQL Server](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Centrum pobierania Microsoft]: https://www.microsoft.com/download/details.aspx?id=36433
