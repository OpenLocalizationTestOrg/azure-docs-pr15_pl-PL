<properties
   pageTitle="Migrowanie rozwiązania do magazynu danych SQL | Microsoft Azure"
   description="Wytyczne migracji za rozwiązania do magazynu danych SQL Azure platformy."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="barbkess;jrj;sonyama"/>

# <a name="migrate-your-solution-to-sql-data-warehouse"></a>Migrowanie rozwiązania do magazynu danych SQL

Magazyn danych SQL jest rozproszony system bazy danych, który elastically skale stosownie do potrzeb. Aby zachować wydajności i skali, nie wszystkie funkcje programu SQL Server są wykonywane wewnątrz magazynu danych SQL. W poniższych tematach migracji dotknij w niektórych kluczowymi wskaźnikami migrowania rozwiązania do magazynu danych SQL. Projektowanie magazynów danych dla skali wprowadza inny projekt, że wzorców i dlatego tradycyjnych metod nie są zawsze najlepsze. Znajdziesz w związku z tym, że dostosowania istniejących rozwiązanie gwarantuje, że w pełni korzystać dostarczony przez magazynu danych SQL platformy rozłożone.

Należy również pamiętasz magazynu danych SQL platformy na Microsoft Azure. Część migracji może również dotyczą przenoszenia danych w chmurze. Transfer danych jest temat osobnym i należy dokładnie rozważyć; zwłaszcza wzrostem wielkości. Przesyłanie danych i ładowanie danych są osobne tematy.

## <a name="migration-guidance"></a>Wskazówki dotyczące migracji

Upewnij się, że zostały przeczytane przez te artykuły, aby upewnić się, że opis niektórych różnic produktu i podstawowe pojęcia przed wchodzących na migracji.

- [Migrowanie schematu][]
- [Migrowanie danych][]
- [Migrowanie kodu][]

## <a name="next-steps"></a>Następne kroki

KOT (zespół doradcze klienta) jest również poradnika magazynu danych SQL doskonałe, które ich publikowania za pośrednictwem blogów.  Zapoznaj się z ich artykuł [Migrowanie danych do magazynu danych SQL Azure w praktyce][] dodatkowe wskazówki dotyczące migracji.

<!--Image references-->

<!--Article references-->
[Migrowanie schematu]: sql-data-warehouse-migrate-schema.md
[Migrowanie danych]: sql-data-warehouse-migrate-data.md
[Migrowanie kodu]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[Migrowanie danych do magazynu danych SQL Azure w praktyce]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
