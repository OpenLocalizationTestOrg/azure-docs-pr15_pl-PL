<properties
   pageTitle="Zarządzanie power obliczeń w magazynie danych SQL Azure (RESZTA) | Microsoft Azure"
   description="Zadania programu PowerShell do zarządzania obliczyć power. Skala obliczyć zasobów za pomocą dostosowania DWUs. Lub wstrzymywanie i wznawianie obliczenia kosztów zasobów."
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

# <a name="manage-compute-power-in-azure-sql-data-warehouse-rest"></a>Zarządzanie power obliczeń w magazynie danych SQL Azure (RESZTA)

> [AZURE.SELECTOR]
- [Omówienie](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [Programu PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [POZOSTAŁE](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Wydajność skali Skalowanie zewnętrzne obliczyć zasoby i pamięć do spełnienia wymagań zmiana z pracą. Zapisz kosztów według skalowania zasobów wstecz podczas godzin niezwiązanych Szczyt lub wstrzymywanie obliczeń całkowicie. 

Ten zbiór zadań używa portal Azure, aby:

- Skala obliczeń
- Wstrzymaj obliczeń
- Życiorys obliczeń

Aby uzyskać informacje na ten temat, zobacz [Zarządzanie obliczyć Przegląd][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Skala power obliczeń

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Aby zmienić DWUs, za pomocą interfejsu API usługi REST [Tworzenie lub aktualizowanie bazy danych][] . Poniższy przykład ustawia wskaźniku poziomu usług DW1000 MySQLDW, który znajduje się na serwerze nazwa_serwera bazy danych. Serwer jest w grupie Azure zasobów o nazwie ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Wstrzymaj obliczeń

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Aby wstrzymać bazy danych, za pomocą interfejsu API usługi REST [Wstrzymać bazy danych][] . W następującym przykładzie wstrzymano bazy danych o nazwie Database02 znajdującej się na serwerze o nazwie Serwer01. Serwer jest w grupie Azure zasobów o nazwie ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Życiorys obliczeń

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Aby utworzyć bazę danych, należy użyć interfejsu API usługi REST [Życiorys bazy danych][] . W następującym przykładzie uruchomiono bazy danych o nazwie Database02 znajdującej się na serwerze o nazwie Serwer01. Serwer jest w grupie Azure zasobów o nazwie ResourceGroup1. 

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Następne kroki

W przypadku innych zadań zarządzania zobacz [Omówienie zarządzania][].

<!--Image references-->

<!--Article references-->
[Omówienie zarządzania]: ./sql-data-warehouse-overview-manage.md
[Zarządzanie omówienie obliczeń]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Wstrzymaj bazy danych]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Życiorys bazy danych]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Tworzenie lub aktualizowanie bazy danych]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
