<properties
   pageTitle="Przywracanie magazynu danych Azure SQL (interfejsu API usługi REST) | Microsoft Azure"
   description="Zadania interfejsu API usługi REST przywracania magazynu danych SQL Azure."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Przywracanie magazynu danych Azure SQL (interfejsu API usługi REST)

> [AZURE.SELECTOR]
- [Omówienie][]
- [Portal][]
- [Programu PowerShell][]
- [POZOSTAŁE][]

W tym artykule dowiesz się, jak przywrócić magazynu danych SQL Azure za pomocą interfejsu API usługi REST.

## <a name="before-you-begin"></a>Przed rozpoczęciem

**Sprawdź możliwości DTU.** Każdego magazynu danych SQL jest obsługiwana przez serwer SQL (na przykład myserver.database.windows.net), który ma przydział DTU domyślny.  Przed można przywrócić magazynu danych SQL, sprawdź, czy program SQL server jest za mało pozostała przydziałów DTU Trwa przywracanie bazy danych. Aby dowiedzieć się, jak obliczyć DTU potrzebne albo zażądaj DTU więcej, zobacz [żądanie zmiany przydziału DTU][].

## <a name="restore-an-active-or-paused-database"></a>Przywracanie aktywnego lub wstrzymana bazy danych

Aby przywrócić bazy danych:

1. Uzyskiwanie listy punktów przywracania bazy danych przy użyciu operacji Get punkty przywracania bazy danych.
2. Rozpocznij przywracania przy użyciu operacji [przywracania bazy danych utwórz żądanie][] .
3. Śledzenie stanu przywracania przy użyciu operacji [Stan operacji bazy danych][] .

>[AZURE.NOTE] Po zakończeniu przywracania można skonfigurować odzyskanego bazy danych przez następujące [konfiguracji bazy danych po odzyskiwania][].

## <a name="restore-a-deleted-database"></a>Przywracanie usuniętego bazy danych

Aby przywrócić usunięty bazy danych:

1.  Lista wszystkich umożliwiająca przywrócenie usuniętych baz danych przy użyciu operacji [lista umożliwiająca przywrócenie elementów usuniętych baz danych][] .
2.  Szczegółowe informacje dla usuniętych bazy danych, którą chcesz przywrócić przy użyciu operacji [Uzyskiwanie umożliwiająca przywrócenie elementów usuniętych bazy danych][] .
3.  Rozpocznij przywracania przy użyciu operacji [przywracania bazy danych utwórz żądanie][] .
4.  Śledzenie stanu przywracania przy użyciu operacji [Stan operacji bazy danych][] .

>[AZURE.NOTE] Aby skonfigurować bazy danych po zakończeniu przywracania, zobacz [Konfigurowanie bazy danych po odzyskiwania][]. 


## <a name="next-steps"></a>Następne kroki
Aby uzyskać informacje o funkcji ciągłości biznesowych wersji bazy danych SQL Azure, przeczytaj [Przegląd ciągłości działalności bazy danych SQL Azure][].

<!--Image references-->

<!--Article references-->
[Omówienie ciągłości firm w usłudze Azure baza danych SQL]: ./sql-database-business-continuity.md
[Żądanie zmiany przydziału DTU]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Konfigurowanie bazy danych po odzyskiwania]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[Omówienie]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[Programu PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[POZOSTAŁE]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Tworzenie żądania przywracania bazy danych]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Stan operacji bazy danych]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Uzyskiwanie umożliwiająca przywrócenie elementów usuniętych bazy danych]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Lista umożliwiająca przywrócenie inicjały baz danych]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
