<properties
   pageTitle="Typy danych dla tabel w magazynie danych SQL | Microsoft Azure"
   description="Wprowadzenie do typów danych dla tabel magazynu danych SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="data-types-for-tables-in-sql-data-warehouse"></a>Typy danych dla tabel w magazynie danych SQL

> [AZURE.SELECTOR]
- [Omówienie][]
- [Typy danych][]
- [Rozpowszechnianie][]
- [Indeks][]
- [Partition][]
- [Statystyki][]
- [Tymczasowe][]

Obsługa magazynu danych SQL najczęściej używanych typów danych.  Poniżej przedstawiono listę typów danych obsługiwanych przez program SQL Data Warehouse.  Aby uzyskać dodatkowe informacje na typ danych obsługi, zobacz [Tworzenie tabeli][].

|**Obsługiwane typy danych**|||
|---|---|---|
[bigint][]|[dziesiętna][]|[smallint][]|
[format binarny][]|[Przestaw][]|[smallmoney][]|
[bit][]|[int][]|[Sysname][]|
[znak][]|[pieniądze][]|[czas][]|
[Data][]|[nchar][]|[tinyint][]|
[daty i godziny][]|[nvarchar][]|[uniqueidentifier][]|
[datetime2][]|[liczba rzeczywista][]|[zmiennej liczby dwójkowej][]|
[datetimeoffset][]|[smalldatetime][]|[varchar][]|


## <a name="data-type-best-practices"></a>Najważniejsze wskazówki dotyczące typu danych

 Podczas definiowania usługi typy kolumn, za pomocą najmniejszą typ danych, który będzie obsługiwać dane zwiększa wydajność kwerendy. Jest to szczególnie ważne CHAR i VARCHAR kolumn. Jeśli najdłuższej wartości w kolumnie jest 25 znaków, Zdefiniuj kolumny jako VARCHAR(25). Unikaj, definiując wszystkie kolumny znaków do dużego domyślną długość. Ponadto zdefiniować kolumny jako VARCHAR, gdy to wszystko, która jest potrzebna, a nie za pomocą [NVARCHAR][].  Użyj zamiast NVARCHAR(MAX) lub VARCHAR(MAX) NVARCHAR(4000) lub VARCHAR(8000), jeśli to możliwe.

## <a name="polybase-limitation"></a>Ograniczenie Polybase

Jeśli korzystasz z Polybase ładowanie tabel, należy zdefiniować tabel, tak aby maksymalnego możliwe rozmiaru wiersza, w tym długie kolumn zmiennej długości nie przekracza 32 767 bajtów.  Gdy można zdefiniować wiersza z danymi zmiennej długości, które można przekracza ten szerokości i ładowanie wierszy z BCP, nie można załadować danych przy użyciu Polybase.  Wkrótce zostanie dodany Polybase obsługę wielu wierszy.

## <a name="unsupported-data-types"></a>Nieobsługiwane typy danych

Jeśli bazy danych są migrowane z innej platformy SQL, takich jak bazy danych SQL Azure, jak możesz przeprowadzić migrację, mogą wystąpić niektórych typów danych, które nie są obsługiwane w magazynie danych SQL.  Poniżej przedstawiono nieobsługiwanych typów danych, a także niektóre rozwiązania alternatywne, których można używać zamiast nieobsługiwane typy danych.

|Typ danych|Obejście|
|---|---|
|[geometrii][]|[zmiennej liczby dwójkowej][]|
|[geograficznych][]|[zmiennej liczby dwójkowej][]|
|[Identyfikator hierarchii][]|[nvarchar][] (4000)|
|[Obraz][ntext,text,image]|[zmiennej liczby dwójkowej][]|
|[tekst][ntext,text,image]|[varchar][]|
|[ntext][ntext,text,image]|[nvarchar][]|
|[sql_variant][]|Podziel kolumny do kilku jednoznacznie określone typy kolumn.|
|[Tabela][]|Konwertowanie na tymczasowe tabele.|
|[Sygnatura czasowa][]|Zmiana zasad kodu w celu użycia [datetime2][] i `CURRENT_TIMESTAMP` funkcji.  Obsługiwane są tylko stałych jako domyślne, dlatego current_timestamp nie można zdefiniować jako domyślne ograniczenie. Jeśli musisz przeprowadzić migrację wiersza wersję wartości z kolumny wpisany sygnatury czasowej należy użyć [BINARNE][](8) lub [zmiennej liczby DWÓJKOWEJ][BINARNE](8) dla nie NULL lub wartość NULL wiersz wartości wersję.|
|[XML][]|[varchar][]|
|[typy zdefiniowane przez użytkownika][]|przekonwertować z powrotem na typy natywnych miarę|
|domyślne wartości|domyślne wartości obsługuje literałów i stałych tylko.  Osoby, które nie deterministyczne wyrażenia lub funkcji, takich jak `GETDATE()` lub `CURRENT_TIMESTAMP`, nie są obsługiwane.|

Poniżej SQL może działać w bieżącym bazy danych SQL do identyfikowania kolumn, które nie są obsługiwane przez magazynu danych SQL Azure:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz artykuły w [Omówienie tabeli][Omówienie], [rozpowszechniania tabeli][Rozłóż], [indeksowania tabeli][indeks], [podziału tabeli][Partition], [Zachowywanie statystyki tabeli][Statystyki] i [Tabele tymczasowe][tymczasowe].  Aby uzyskać więcej informacji o najważniejszych wskazówkach dotyczących zobacz [Najważniejsze wskazówki magazynu danych SQL][].

<!--Image references-->

<!--Article references-->
[Omówienie]: ./sql-data-warehouse-tables-overview.md
[Typy danych]: ./sql-data-warehouse-tables-data-types.md
[Rozpowszechnianie]: ./sql-data-warehouse-tables-distribute.md
[Indeks]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statystyki]: ./sql-data-warehouse-tables-statistics.md
[Tymczasowe]: ./sql-data-warehouse-tables-temporary.md
[Najważniejsze wskazówki magazynu danych SQL]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[Tworzenie tabeli]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[format binarny]: https://msdn.microsoft.com/library/ms188362.aspx
[bit]: https://msdn.microsoft.com/library/ms177603.aspx
[znak]: https://msdn.microsoft.com/library/ms176089.aspx
[Data]: https://msdn.microsoft.com/library/bb630352.aspx
[daty i godziny]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[dziesiętna]: https://msdn.microsoft.com/library/ms187746.aspx
[Przestaw]: https://msdn.microsoft.com/library/ms173773.aspx
[geometrii]: https://msdn.microsoft.com/library/cc280487.aspx
[geograficznych]: https://msdn.microsoft.com/library/cc280766.aspx
[Identyfikator hierarchii]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[pieniądze]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[liczba rzeczywista]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[Sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[Tabela]: https://msdn.microsoft.com/library/ms175010.aspx
[czas]: https://msdn.microsoft.com/library/bb677243.aspx
[Sygnatura czasowa]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[uniqueidentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[zmiennej liczby dwójkowej]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[XML]: https://msdn.microsoft.com/library/ms187339.aspx
[typy zdefiniowane przez użytkownika]: https://msdn.microsoft.com/library/ms131694.aspx
