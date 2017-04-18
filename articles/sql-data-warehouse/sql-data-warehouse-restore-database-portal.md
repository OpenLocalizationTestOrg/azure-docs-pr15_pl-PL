<properties
   pageTitle="Przywracanie magazynu danych Azure SQL (Portal) | Microsoft Azure"
   description="Azure portalu zadania przywracania magazynu danych SQL Azure."
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

# <a name="restore-an-azure-sql-data-warehouse-portal"></a>Przywracanie magazynu danych Azure SQL (Portal)

> [AZURE.SELECTOR]
- [Omówienie][]
- [Portal][]
- [Programu PowerShell][]
- [POZOSTAŁE][]

W tym artykule dowiesz się, jak przywrócić magazynu danych SQL Azure za pomocą portalu Azure.

## <a name="before-you-begin"></a>Przed rozpoczęciem

**Sprawdź możliwości DTU.** Każdego magazynu danych SQL jest obsługiwana przez serwer SQL (na przykład myserver.database.windows.net), który ma przydział DTU domyślny.  Przed można przywrócić magazynu danych SQL, sprawdź, czy program SQL server jest za mało pozostała przydziałów DTU Trwa przywracanie bazy danych. Aby dowiedzieć się, jak obliczyć DTU potrzebne albo zażądaj DTU więcej, zobacz [żądanie zmiany przydziału DTU][].


## <a name="restore-an-active-or-paused-database"></a>Przywracanie aktywnego lub wstrzymana bazy danych

Aby przywrócić bazy danych:

1. Zaloguj się do [portalu Azure][]
2. Po lewej stronie ekranu wybierz pozycję **Przeglądaj** , a następnie wybierz pozycję **serwerów SQL**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Przejdź do swojego serwera i zaznacz go
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. Znajdowanie magazynu danych SQL, który chcesz przywrócić z, a następnie wybierz ją
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. W górnej części karta magazynu danych kliknij przycisk **Przywróć**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Podaj nową **nazwę bazy danych**
7. Wybierz najnowszą **Punkt przywracania**
    1. Upewnij się, że możesz wybrać najnowszy punkt przywracania.  Ponieważ punkty przywracania są wyświetlane w czasie UTC, czasami domyślna opcja pokazano nie jest najnowszą punkt przywracania.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Kliknij **przycisk OK**
9. Proces przywracania bazy danych rozpocznie się i można monitorować za pomocą **POWIADOMIEŃ**

>[AZURE.NOTE] Po zakończeniu przywracania można skonfigurować odzyskanego bazy danych przez następujące [konfiguracji bazy danych po odzyskiwania][].


## <a name="restore-a-deleted-database"></a>Przywracanie usuniętego bazy danych

Aby przywrócić usunięty bazy danych:

1. Zaloguj się do [portalu Azure][]
2. Po lewej stronie ekranu wybierz pozycję **Przeglądaj** , a następnie wybierz pozycję **serwerów SQL**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Przejdź do swojego serwera i zaznacz go
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Przewiń w dół do sekcji operacji na karta swojego serwera
5. Kliknij Kafelek **Usunięte baz danych**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Wybierz usunięte bazę danych, do której chcesz przywrócić
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Podaj nową **nazwę bazy danych**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Kliknij **przycisk OK**
9. Proces przywracania bazy danych rozpocznie się i można monitorować za pomocą **POWIADOMIEŃ**

>[AZURE.NOTE] Aby skonfigurować bazy danych po zakończeniu przywracania, zobacz [Konfigurowanie bazy danych po odzyskiwania][]. 

## <a name="next-steps"></a>Następne kroki
Aby uzyskać informacje o funkcji ciągłości biznesowych wersji bazy danych SQL Azure, przeczytaj [Przegląd ciągłości działalności bazy danych SQL Azure][].

<!--Image references-->

<!--Article references-->
[Omówienie ciągłości firm w usłudze Azure baza danych SQL]: ./sql-database-business-continuity.md
[Omówienie]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[Programu PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[POZOSTAŁE]: ./sql-data-warehouse-restore-database-rest-api.md
[Konfigurowanie bazy danych po odzyskiwania]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Żądanie zmiany przydziału DTU]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
