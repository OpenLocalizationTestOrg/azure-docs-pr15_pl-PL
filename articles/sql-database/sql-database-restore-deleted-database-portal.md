<properties
    pageTitle="Przywracanie usuniętego bazy danych Azure SQL (Azure portal) | Microsoft Azure"
    description="Przywracanie usuniętego bazy danych Azure SQL (Azure portal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-using-the-azure-portal"></a>Przywracanie usuniętego bazy danych Azure SQL za pomocą portalu Azure

> [AZURE.SELECTOR]
- [Omówienie](sql-database-recovery-using-backups.md)
- [**Przywracanie usuniętych DB: Portal**](sql-database-restore-deleted-database-portal.md)
- [Przywracanie usuniętych DB: programu PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="select-the-database-to-restore"></a>Wybierz bazę danych, aby przywrócić 

Aby przywrócić usunięty bazy danych w portalu Azure:

1.  W [portalu Azure](https://portal.azure.com), kliknij pozycję **więcej usług** > **serwerów SQL**.
3.  Wybierz serwer zawierający bazę danych, którą chcesz przywrócić.
4.  Przewiń w dół do sekcji **operacji** karta swojego serwera i wybierz pozycję **usunięte baz danych**: ![Przywracanie bazy danych programu Azure SQL](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
5.  Wybierz bazę danych, którą chcesz przywrócić.
6.  Podaj nazwę bazy danych, a następnie kliknij **przycisk OK**:

    ![Przywracanie bazy danych programu Azure SQL](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)


## <a name="next-steps"></a>Następne kroki

- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md)
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego odzyskiwania, zobacz [Przywracanie bazy danych z kopii zapasowych zainicjowane usługi](sql-database-recovery-using-backups.md)
- Aby poznać opcje odzyskiwania szybciej, zobacz [Aktywne Geo replikacji](sql-database-geo-replication-overview.md)  
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego archiwizowania, zobacz [Kopiowanie bazy danych](sql-database-copy.md)
