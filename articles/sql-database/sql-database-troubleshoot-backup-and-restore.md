<properties
    pageTitle="Rozwiązywanie problemów z kopii zapasowych i przywracanie z bazy danych SQL Azure"
    description="Dowiedz się, jak odzyskać bazy danych w chmurze z błędów i awarii przy użyciu kopii zapasowych i replik w bazie danych SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>

# <a name="restore-a-database-to-a-previous-point-in-time-restore-a-deleted-database-or-recover-from-a-data-center-outage"></a>Przywracanie bazy danych do poprzedniego punktu w czasie, przywracanie usuniętych bazy danych lub odzyskiwanie z awaria centrum danych

Baza danych SQL przechowywana replik bazy danych, więc można odzyskać z dostawie i błąd użytkownika. Dostępne opcje zależą od poziomu usług bazy danych i wybrać jedną z opcji. Zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md) szczegóły i zagadnienia projektowe.

## <a name="to-restore-a-database-to-a-previous-point-in-time"></a>Aby przywrócić bazy danych do poprzedniego punktu w czasie
1.  [Azure Portal](https://azure.microsoft.com/)kliknij **baz danych programu SQL**.
2.  Z listy wybierz bazę danych, a następnie kliknij pozycję **Przywróć**.
3.  Wpisz nową nazwę bazy danych, wybierz datę i czas, aby przywrócić z, a następnie kliknij pozycję **Tworzenie.**
4.  Dostosować aplikacji stosownie do potrzeb odwołać się nowej bazy danych. Zobacz [odzyskiwanie bazy danych do punktu w czasie](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="to-restore-an-accidentally-deleted-database"></a>Aby przywrócić przypadkowo usuniętych bazy danych
1.  W [Azure Portal](https://azure.microsoft.com/)kliknij **serwerów SQL**.
2.  Wybierz serwer, który obsługiwane bazy danych z listy.
3.  Na karta serwera przewiń w dół i kliknij **usunięte baz danych**.
4.  Wybierz bazę danych, aby przywrócić, a następnie kliknij przycisk **Utwórz**.
5.  Dostosować aplikacji stosownie do potrzeb odwołać się nowej bazy danych. Zobacz [Odzyskiwanie usuniętych bazy danych](sql-database-recovery-using-backups.md#deleted-database-restore).

## <a name="to-recover-from-a-regional-datacenter-outage"></a>Aby odzyskać awaria regionalnego centrum danych
Z bazami danych Standard i Premium Jeśli skonfigurowano replikowane geo pomocnicze można odzyskać przy użyciu tych pomocnicze. Zapewnia możliwości przywracania bazy danych za pomocą mniej możliwość utraty danych. Aby uzyskać szczegółowe informacje, zobacz [odzyskiwanie bazy danych programu Azure SQL za pomocą kopie zapasowe automatycznego bazy danych](sql-database-disaster-recovery.md) .

Azure udostępnia kopie zapasowe każdej bazy danych w innym regionie (geo zbędne kopia zapasowa). Można utworzyć nową bazę danych z tych kopii zapasowych, które jest nazywane Geo przywracania, ale prawdopodobnie jest będzie występować utratą danych w przypadku korzystania z tej metody można użyć tylko.

**Aby odzyskać przy użyciu geo przywracania bazy danych:**

- W [Azure Portal](https://azure.microsoft.com/)kliknij pozycję **Nowy**, kliknij pozycję **dane i miejsca do magazynowania**, kliknij **Bazy danych SQL**, a następnie wybierz pozycję **Kopia zapasowa** jako źródło bazy danych. Aby uzyskać szczegółowe informacje, zobacz [odzyskiwanie bazy danych programu Azure SQL z awarii](sql-database-disaster-recovery.md) .