<properties
   pageTitle="Ładowanie przykładowych danych do magazynu danych SQL | Microsoft Azure"
   description="Ładowanie przykładowych danych do magazynu danych SQL"
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
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="load-sample-data-into-sql-data-warehouse"></a>Ładowanie przykładowych danych do magazynu danych SQL

Zrób tak proste na ładowanie i kwerendy Adventure Works przykładowej bazy danych. Te skrypty najpierw za pomocą sqlcmd uruchamianie SQL, w których utworzy tabele i widoki. Po utworzeniu tabel skrypty przy użyciu bcp załadować dane.  Jeśli nie masz jeszcze sqlcmd i bcp zainstalowany, skorzystaj z poniższych łączy do [zainstalowania bcp][] i [zainstalować sqlcmd][].

##<a name="load-sample-data"></a>Ładowanie przykładowych danych

1. Pobierz plik zip [Adventure Works przykładowe skrypty do magazynu danych SQL][] .

2. Wyodrębnianie plików z pobranego zip do katalogu na komputerze lokalnym.

3. Edytuj aw_create.bat wyodrębnionej pliku i ustaw następujące zmienne znajdujących się u góry pliku.  Pamiętaj pozostawić nie spacji między "=" oraz parametru.  Poniżej przedstawiono przykłady wygląd zmian.

    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```

4. W wierszu cmd systemu Windows uruchom edytowanego aw_create.bat.  Upewnij się, że jesteś w katalogu zapisywania wersji edytowanego aw_create.bat.
Ten skrypt będzie...
    * Upuść Adventure Works tabel lub widoków, które już istnieje w bazie danych
    * Tworzenie Adventure Works tabel i widoków
    * Ładowanie każdej tabeli Adventure Works przy użyciu bcp
    * Sprawdź poprawność liczba wierszy dla każdej tabeli Adventure Works
    * Zbieranie statystyki w każdej kolumnie dla każdej tabeli Adventure Works


##<a name="query-sample-data"></a>Dane przykładowe kwerendy

Po przykładowe dane zostały załadowane do magazynu danych SQL, możesz szybko uruchomić kilka kwerend.  Aby uruchomić kwerendę, Połącz do nowo utworzonej bazie danych Adventure Works w DW SQL Azure za pomocą programu Visual Studio i SSDT, zgodnie z opisem w dokumencie [kwerendy z programu Visual Studio][] .

Przykład prostej instrukcji select, aby uzyskać wszystkie informacje o pracowników:

```sql
SELECT * FROM DimEmployee;
```

Przykład bardziej złożonej kwerendy przy użyciu konstrukcji, takich jak GROUP BY aby przyjrzeć się łączną dla wszystkich sprzedaży dla każdego dnia:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Przykład SELECT z klauzuli WHERE w celu filtrowania zamówień z przed określoną datą:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Program SQL Data Warehouse obsługuje niemal wszystkich konstrukcji T-SQL, których obsługa programu SQL Server.  Opisano różnice w naszą dokumentację [migrację kodu][] .

## <a name="next-steps"></a>Następne kroki
Teraz, gdy użytkownik miał możliwość wypróbować niektóre kwerendy z przykładowymi danymi, zapoznaj się z jak [opracowywać][], [Ładowanie][]i [przeprowadzić migrację][] do magazynu danych SQL.

<!--Image references-->

<!--Article references-->
[Migrowanie]: sql-data-warehouse-overview-migrate.md
[można opracowywać]: sql-data-warehouse-overview-develop.md
[Ładowanie]: sql-data-warehouse-overview-load.md
[Kwerenda z programu Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Migrowanie kodu]: sql-data-warehouse-migrate-code.md
[Instalowanie bcp]: sql-data-warehouse-load-with-bcp.md
[Instalowanie sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works przykładowe skrypty dla magazynu danych SQL]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
