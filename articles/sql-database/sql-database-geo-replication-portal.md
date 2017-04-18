<properties 
    pageTitle="Konfigurowanie Geo Replikacja bazy danych SQL Azure Portal Azure | Microsoft Azure" 
    description="Konfigurowanie Geo Replikacja bazy danych SQL Azure za pomocą portalu Azure" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/18/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-the-azure-portal"></a>Konfigurowanie Geo Replikacja bazy danych SQL Azure Portal Azure


> [AZURE.SELECTOR]
- [Omówienie](sql-database-geo-replication-overview.md)
- [Azure portal](sql-database-geo-replication-portal.md)
- [Programu PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

W tym artykule pokazano, jak skonfigurować aktywnej replikacji Geo bazy danych SQL [Azure portalu](http://portal.azure.com).

Aby zainicjować pracy awaryjnej Portal Azure, zobacz [zainicjować planowane lub niezaplanowane awaryjnego bazy danych SQL Azure Portal Azure](sql-database-geo-replication-failover-portal.md).

>[AZURE.NOTE] Aktywne replikacji Geo (czytelne pomocnicze) jest teraz dostępny dla wszystkich baz danych ze wszystkich warstw usługi. W 2017 kwietnia-czytelne typu pomocniczego zostanie wycofana i istniejące bazy danych nie do odczytu zostanie automatycznie uaktualniony pomocnicze czytelne.

Aby skonfigurować przy użyciu portalu Azure replikacji Geo, są potrzebne następujące zasoby:

- Baza danych Azure SQL - podstawowego bazę danych, którą chcesz replikować do innego regionu geograficznego.

## <a name="add-secondary-database"></a>Dodawanie pomocniczej bazy danych

Poniższe kroki Utwórz nową bazę pomocniczej partnerskie Geo replikacji.  

Aby dodać pomocniczym, musi być właścicielem subskrypcji lub współwłaściciela. 

Pomocniczego bazy danych będą mieć taką samą nazwę jak podstawowej bazy danych i będzie domyślnie mają taki sam poziom usługi. Pomocniczego bazy danych może być jednej bazie danych lub elastycznych bazy danych. Aby uzyskać więcej informacji zobacz [Warstwy usługi](sql-database-service-tiers.md).
Po pomocniczej jest tworzony i obsługiwany, danych rozpocznie się replikacji od podstawowej bazy danych do nowej bazy danych pomocniczą. 

> [AZURE.NOTE] Jeśli baza danych partnera już istnieje (na przykład - na skutek zakończenia poprzedniego relacji replikacji Geo) polecenie zakończy się niepowodzeniem.

### <a name="add-secondary"></a>Dodawanie pomocniczej

1. W [portalu Azure](http://portal.azure.com)przejdź do bazy danych, którą chcesz skonfigurować Geo replikacji.
2. Na stronie bazy danych SQL zaznacz **Geo replikacji**, a następnie wybierz region tworzenie pomocniczego bazy danych. Można wybrać dowolny region niż region hostingu podstawowej bazy danych, ale zalecane jest [iloczynów region](../best-practices-availability-paired-regions.md) .

    ![Konfigurowanie Geo replikacji](./media/sql-database-geo-replication-portal/configure-geo-replication.png)


4. Zaznaczanie lub skonfigurować serwer i cennik warstwa pomocniczej bazy danych.

    ![Konfigurowanie pomocniczego](./media/sql-database-geo-replication-portal/create-secondary.png)

5. Opcjonalnie można dodać pomocniczej bazy danych do puli elastyczne bazy danych:

 Aby utworzyć pomocniczego bazy danych w puli, kliknij **puli elastyczne bazy danych** i wybierz puli na serwerze docelowym. Jak ten przepływ pracy nie tworzenie puli puli musi już istnieć na serwerze docelowym.

6. Kliknij przycisk **Utwórz** , aby dodać pomocniczej.
 
6. Pomocniczej baza danych zostanie utworzona i rozpocznie proces obsługi. 
 
    ![Konfigurowanie pomocniczego](./media/sql-database-geo-replication-portal/seeding0.png)

7. Po zakończeniu procesu obsługi pomocniczej bazy danych zostanie wyświetlony jego stan.

    ![obsługiwanie wykonania](./media/sql-database-geo-replication-portal/seeding-complete.png)


## <a name="remove-secondary-database"></a>Usuwanie pomocniczej bazy danych

Operacja trwale kończy replikacji do pomocniczej bazy danych i przybierze rolę pomocniczej zwykłej odczytu i zapisu bazy danych. W przypadku niepoprawnych łączności z pomocniczego bazy danych, polecenie zakończyło się powodzeniem, ale będzie pomocniczej, nie są zamieniane na odczytu i zapisu, aż po łączności zostanie przywrócony.  

1. W [Azure portal](http://portal.azure.com)przejdź do głównej bazy danych w partnerstwa replikacji Geo.
2. Na stronie bazy danych SQL zaznacz **Geo replikacji**.
3. Na liście **pomocnicze** wybierz bazę danych, którą chcesz usunąć z partnerstwa replikacji Geo.
4. Kliknij przycisk **Zatrzymaj replikacji**.

    ![Usuwanie pomocniczego](./media/sql-database-geo-replication-portal/remove-secondary.png)

5. Klikając przycisk **Zatrzymaj replikacji** otwiera okno potwierdzenia kliknij **Tak,** Aby usunąć bazę danych z partnerstwa replikacji Geo (ustawiono do odczytu i zapisu bazy danych nie jest częścią replikacji).


## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej o aktywnej Geo replikacji, zobacz - [Aktywnej replikacji Geo](sql-database-geo-replication-overview.md)
- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)

