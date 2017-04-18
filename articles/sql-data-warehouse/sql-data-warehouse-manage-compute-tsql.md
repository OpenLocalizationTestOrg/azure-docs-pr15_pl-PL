<properties
   pageTitle="Zarządzanie power obliczeń w magazynie danych SQL Azure (RESZTA) | Microsoft Azure"
   description="Zadania Transact-SQL (T-SQL) do poza skalowanie wydajności za pomocą dostosowania DWUs. Aby zmniejszyć koszty, skalowania Wstecz w czasie nie Szczyt."
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
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Zarządzanie power obliczeń w magazynie danych SQL Azure (T-SQL)

> [AZURE.SELECTOR]
- [Omówienie](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [Programu PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [POZOSTAŁE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Wydajność skali Skalowanie zewnętrzne obliczyć zasoby i pamięć do spełnienia wymagań zmiana z pracą. Zapisz kosztów według skalowania zasobów wstecz podczas godzin niezwiązanych Szczyt lub wstrzymywanie obliczeń całkowicie. 

Ten zbiór zadań używa T-SQL, aby:

- Ustawienia DWU bieżącego widoku
- Zmiana obliczeń zasobów za pomocą dostosowania DWUs

Aby wstrzymać lub wznowić bazy danych, wybierz jedną z innych opcji platformy w górnej części tego artykułu.

Aby uzyskać informacje na ten temat, zobacz [Zarządzanie obliczyć power — omówienie][].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Ustawienia DWU bieżącego widoku

Aby wyświetlić bieżące ustawienia DWU baz danych:

1. Otwórz program SQL Server Eksploratora obiektów w programie Visual Studio 2015 r.
2. Połączenia z bazą danych wzorca, skojarzone z serwerem bazy danych SQL logiczne.
2. Wybierz z view dynamiczne zarządzanie sys.database_service_objectives. Oto przykład: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Skala obliczeń

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Aby zmienić DWUs:


1. Połączenia z bazą danych wzorca, skojarzone z serwerem bazy danych SQL logiczne.
2. Instrukcja [ALTER DATABASE][] TSQL. Poniższy przykład ustawia wskaźniku poziomu usług DW1000 MySQLDW bazy danych. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Następne kroki

W przypadku innych zadań zarządzania zobacz [Omówienie zarządzania][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Omówienie zarządzania]: ./sql-data-warehouse-overview-manage.md
[Zarządzanie obliczeń power — omówienie]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ZMIANY BAZY DANYCH]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
