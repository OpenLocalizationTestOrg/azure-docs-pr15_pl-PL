<properties
   pageTitle="Projektowanie decyzji i technik kodowania rozwoju magazynu danych SQL | Microsoft Azure"
   description="Rozwoju pojęcia, decyzje projektowe, zalecenia i technik kodowania dla magazynu danych SQL."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Decyzje projektowe i technik kodowania dla magazynu danych SQL

Zapoznaj się z za pośrednictwem te artykuły rozwoju, aby lepiej zrozumieć kluczowych decyzji projektowych, zalecenia i technik kodowania dla magazynu danych SQL.

## <a name="key-design-decisions"></a>Kluczowych decyzji projektowych
Następujące artykuły wyróżnić część podstawowych pojęć oraz decyzje projektu, które należy zrozumieć rozwoju magazynu rozłożone danych przy użyciu magazynu danych SQL:

- [połączenia][]
- [współbieżności][]
- [transakcje][]
- [schematy zdefiniowane przez użytkownika][]
- [Dystrybucja tabeli][]
- [indeksy tabeli][]
- [partycje tabeli][]
- [CTAS][]
- [statystyki][]

## <a name="development-recommendations-and-coding-techniques"></a>Zalecenia dotyczące opracowywania i technik kodowania
Te artykuły wyróżnić poszczególne techniki kodowania, porady i zalecenia dotyczące opracowywania magazynu danych SQL:

- [procedury składowane][]
- [etykiety][]
- [Widoki][]
- [tabele tymczasowe][]
- [dynamiczne SQL][]
- [odtwarzanie w pętli][]
- [Opcje grupowania według][]
- [Przypisanie zmiennej][]

## <a name="next-steps"></a>Następne kroki
Po zostały za pośrednictwem artykuły rozwoju zapoznaj się z za pośrednictwem strony [języka Transact - SQL][] , aby uzyskać więcej informacji o składni obsługiwane dla magazynu danych SQL.

<!--Image references-->

<!--Article references-->
[współbieżności]: ./sql-data-warehouse-develop-concurrency.md
[połączenia]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamiczne SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[Opcje grupowania według]: ./sql-data-warehouse-develop-group-by-options.md
[etykiety]: ./sql-data-warehouse-develop-label.md
[odtwarzanie w pętli]: ./sql-data-warehouse-develop-loops.md
[statystyki]: ./sql-data-warehouse-tables-statistics.md
[procedury składowane]: ./sql-data-warehouse-develop-stored-procedures.md
[Dystrybucja tabeli]: ./sql-data-warehouse-tables-distribute.md
[indeksy tabeli]: ./sql-data-warehouse-tables-index.md
[partycje tabeli]: ./sql-data-warehouse-tables-partition.md
[tabele tymczasowe]: ./sql-data-warehouse-tables-temporary.md
[transakcje]: ./sql-data-warehouse-develop-transactions.md
[schematy zdefiniowane przez użytkownika]: ./sql-data-warehouse-develop-user-defined-schemas.md
[Przypisanie zmiennej]: ./sql-data-warehouse-develop-variable-assignment.md
[Widoki]: ./sql-data-warehouse-develop-views.md
[Odwołanie Transact-SQL]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
