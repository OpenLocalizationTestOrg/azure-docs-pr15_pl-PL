<properties
    pageTitle="Przywracanie bazy danych programu Azure SQL do poprzedniego punktu w czasie (Azure portal) | Microsoft Azure"
    description="Przywracanie bazy danych programu Azure SQL do poprzedniego punktu w czasie."
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


# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Przywracanie bazy danych programu Azure SQL do poprzedniego punktu w czasie Portal Azure


> [AZURE.SELECTOR]
- [Omówienie](sql-database-recovery-using-backups.md)
- [Przywracanie w chwili: programu PowerShell](sql-database-point-in-time-restore-powershell.md)

W tym artykule pokazano, jak przywrócić bazę danych do wcześniejszego punktu w czasie z [bazą danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md) za pomocą portalu Azure.

## <a name="restore-a-sql-database-to-a-previous-point-in-time"></a>Przywracanie bazy danych SQL do poprzedniego punktu w czasie

Wybierz bazę danych, aby przywrócić w portalu Azure:

1.  Otwórz [Azure portal](https://portal.azure.com).
2.  Po lewej stronie ekranu, wybierz pozycję **więcej usług** > **bazy danych programu SQL**.
3.  Kliknij bazę danych, którą chcesz przywrócić.
4.  W górnej części strony bazy danych wybierz opcję **Przywróć**:

    ![Przywracanie bazy danych programu Azure SQL](./media/sql-database-point-in-time-restore-portal/restore.png)

5.  Na stronie **Przywracanie** wybierz datę i godzinę (w czasie UTC) do przywracania bazy danych, a następnie kliknij **przycisk OK**:

    ![Przywracanie bazy danych programu Azure SQL](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## <a name="monitor-the-restore-operation"></a>Monitorowanie operacji przywracania

1. Po kliknięciu przycisku **OK** w poprzednim kroku, kliknij ikonę powiadomień w prawym górnym rogu strony, a następnie kliknij powiadomienie **bazy danych SQL Przywracanie** , aby uzyskać szczegółowe informacje.

    ![Przywracanie bazy danych programu Azure SQL](./media/sql-database-point-in-time-restore-portal/notification-icon.png)

2. Informacje o stanie przywracania zostanie wyświetlona strona bazy danych SQL przywracanie. Możesz też kliknąć pozycję, aby uzyskać więcej informacji:

    ![Przywracanie bazy danych programu Azure SQL](./media/sql-database-point-in-time-restore-portal/inprogress.png)

 

## <a name="next-steps"></a>Następne kroki

- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)
- Aby uzyskać informacje o bazy danych SQL Azure automatycznego wykonywania kopii zapasowych, zobacz [Baza danych SQL automatycznego wykonywania kopii zapasowych](sql-database-automated-backups.md)
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego odzyskiwania, zobacz [Przywracanie bazy danych z kopii zapasowych zainicjowane usługi](sql-database-recovery-using-backups.md)
- Aby poznać opcje odzyskiwania szybciej, zobacz [Aktywne Geo replikacji](sql-database-geo-replication-overview.md)  
- Aby dowiedzieć się o używaniu kopie zapasowe automatycznego archiwizowania, zobacz [Kopiowanie bazy danych](sql-database-copy.md)
