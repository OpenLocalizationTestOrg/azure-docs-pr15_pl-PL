<properties
    pageTitle="Przywracanie bazy danych programu Azure SQL z automatycznej kopii zapasowej (Azure portal) | Microsoft Azure"
    description="Przywracanie bazy danych programu Azure SQL z automatycznej kopii zapasowej (Azure portal)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/18/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Przywracanie bazy danych programu Azure SQL z automatycznej kopii zapasowej za pomocą portalu Azure


> [AZURE.SELECTOR]
- [Omówienie](sql-database-recovery-using-backups.md#geo-restore)
- [Przywracanie Geo: programu PowerShell](sql-database-geo-restore-powershell.md)

W tym artykule pokazano, jak przywrócić bazę danych z [automatycznej kopii zapasowej](sql-database-automated-backups.md) do nowego serwera z [Przywróć Geo](sql-database-recovery-using-backups/.md#geo-restore) za pomocą portalu Azure.

## <a name="select-a-database-to-restore"></a>Wybierz bazę danych, aby przywrócić

Aby przywrócić bazę danych w portalu Azure, wykonaj następujące czynności:

1.  Przejdź do [portalu Azure](https://portal.azure.com).
2.  Po lewej stronie ekranu wybierz pozycję **+ Nowy** > **baz danych** > **Bazy danych SQL**:

    ![Przywracanie bazy danych programu Azure SQL](./media/sql-database-geo-restore-portal/new-sql-database.png)

3.  Wybierz **kopii zapasowej** jako źródło, a następnie wybierz kopię zapasową, którą chcesz przywrócić. Podaj nazwę bazy danych serwera, aby przywrócić bazę danych, a następnie kliknij pozycję **Utwórz**:
  
    ![Przywracanie bazy danych programu Azure SQL](./media/sql-database-geo-restore-portal/geo-restore.png)

Monitorowanie stanu operacji przywracania, klikając ikonę powiadomień w prawym górnym rogu strony. 


## <a name="next-steps"></a>Następne kroki

- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md)
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego odzyskiwania, zobacz [Przywracanie bazy danych z kopii zapasowych zainicjowane usługi](sql-database-recovery-using-backups.md)
- Aby poznać opcje odzyskiwania szybciej, zobacz [Aktywne Geo replikacji](sql-database-geo-replication-overview.md)  
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego archiwizowania, zobacz [Kopiowanie bazy danych](sql-database-copy.md)
