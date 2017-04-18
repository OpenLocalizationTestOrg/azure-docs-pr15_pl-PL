<properties
   pageTitle="Dynamiczne SQL w magazynie danych SQL | Microsoft Azure"
   description="Porady dotyczące korzystania z dynamicznego kodu SQL w magazynie danych SQL Azure dla opracowania rozwiązań."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamiczne SQL w magazynie danych SQL
Podczas tworzenia kodu aplikacji dla magazynu danych SQL może być konieczne za pomocą dynamicznego kodu sql elastyczne, ogólne i moduły rozwiązań. Program SQL Data Warehouse nie obsługuje typów danych obiektów blob w tej chwili. Mogą ograniczyć rozmiar z ciągów, ponieważ typy obiektów blob zawiera zarówno varchar(max) i nvarchar(max) typów. Jeśli te typy zostało użyte w kodzie aplikacji, podczas tworzenia ciągów bardzo duże, należy podzielić fragmenty kodu i użyć instrukcji WYKONYWALNA.

Prosty przykład:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Jeśli ciąg jest krótkim używając [sp_executesql][] w zwykły sposób.

> [AZURE.NOTE] Wszystkie reguły poprawności TSQL nadal podlega instrukcje wykonywane jako dynamicznego kodu SQL.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad rozwoju zobacz [Omówienie tworzenia][].

<!--Image references-->

<!--Article references-->
[Omówienie tworzenia]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
