<properties
   pageTitle="Migrowanie schematu do magazynu danych SQL | Microsoft Azure"
   description="Porady dotyczące migracja schematu do magazynu danych SQL Azure dla opracowania rozwiązań."
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
   ms.date="08/25/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="migrate-your-schema-to-sql-data-warehouse"></a>Migrowanie schematu do magazynu danych SQL#

Następujące podsumowania ułatwić zrozumienie różnic między programu SQL Server i magazynu danych SQL, aby pomóc migrację bazy danych.

## <a name="table-migration"></a>Tabela migracji

Podczas migracji tabel, warto zapoznać się z funkcjami tabeli tabel magazynu danych SQL.  [Omówienie tabeli][] jest doskonałe miejsce do rozpoczęcia.  W tym artykule przedstawiono najważniejszych zagadnień podczas tworzenia tabeli na przykład tabeli statystyki, rozkład, partycje i indeksowania.  Obejmuje także niektóre [nieobsługiwane funkcje tabel][] i rozwiązania problemu.

Program SQL Data Warehouse obsługuje typowych typów danych biznesowych.  Zobacz artykuł [typów danych][] , aby uzyskać listę obsługiwanych i [nieobsługiwanych typów danych][].  Artykuł [typów danych][] zawiera również kwerendy do identyfikowania [nieobsługiwane typy danych][].  Podczas konwertowania do typów danych, należy przeglądać [wskazówkach typu danych][].

## <a name="next-steps"></a>Następne kroki
Gdy schemat bazy danych zostały pomyślnie zmigrowane do magazynu danych SQL, przejdź do jednej z następujących artykułów:

- [Migrowanie danych][]
- [Migrowanie kodu][]

Aby uzyskać więcej informacji o najważniejszych wskazówkach magazynu danych SQL zobacz artykuł [najlepsze rozwiązania][] .

<!--Image references-->

<!--Article references-->
[Migrowanie kodu]: ./sql-data-warehouse-migrate-code.md
[Migrowanie danych]: ./sql-data-warehouse-migrate-data.md
[Najważniejsze wskazówki]: ./sql-data-warehouse-best-practices.md
[Omówienie tabeli]: ./sql-data-warehouse-tables-overview.md
[nieobsługiwane funkcje tabel]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[typy danych]: ./sql-data-warehouse-tables-data-types.md
[nieobsługiwane typy danych]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Najważniejsze wskazówki dotyczące typu danych]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->
