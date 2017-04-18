<properties
    pageTitle="Zarządzanie bazą danych SQL Azure za pomocą portalu Azure | Microsoft Azure"
    description="Dowiedz się, jak zarządzać relacyjnej bazy danych w chmurze za pomocą portalu Azure za pomocą Azure Portal."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.date="09/19/2016"
    ms.author="sstein"/>


# <a name="managing-azure-sql-databases-using-the-azure-portal"></a>Zarządzanie bazami danych programu SQL Azure za pomocą portalu Azure


> [AZURE.SELECTOR]
- [Azure portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [Programu PowerShell](sql-database-manage-powershell.md)

[Azure portal](https://portal.azure.com/) umożliwia tworzenie, monitorowanie i zarządzanie baz danych programu SQL Azure i serwerów. Ten artykuł zawiera opis szybki i łącza do szczegółowych informacji najbardziej typowych zadań.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Wyświetlanie bazy danych Azure SQL, serwerów i pule

Aby wyświetlić dostępne usługi bazy danych SQL, kliknij **więcej usług**i w polu wyszukiwania wpisz **SQL** :

![Baza danych SQL](./media/sql-database-manage-portal/sql-services.png)


## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Jak utworzyć lub wyświetlić bazy danych programu SQL Azure?

Aby otworzyć karta **baz danych programu SQL** , kliknij **baz danych programu SQL**, a następnie kliknij odpowiednią bazę danych, którą chcesz pracować z lub kliknij przycisk **+ Dodaj** do utworzenia bazy danych SQL. Aby uzyskać szczegółowe informacje zobacz [Tworzenie bazy danych SQL w minutach przy użyciu Azure portal](sql-database-get-started.md).


![Bazy danych SQL](./media/sql-database-manage-portal/sql-databases.png)


## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Jak utworzyć lub wyświetlić serwery Azure SQL?

Aby otworzyć karta **serwerów SQL** , kliknij **serwerów SQL**, a następnie kliknij serwer, z którym chcesz pracować z lub kliknij przycisk **+ Dodaj** do tworzenia programu SQL server. Aby uzyskać szczegółowe informacje zobacz [Tworzenie bazy danych SQL w minutach przy użyciu Azure portal](sql-database-get-started.md).

![Serwery SQL](./media/sql-database-manage-portal/sql-servers.png)


## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Jak utworzyć lub wyświetlanie pul elastyczne SQL?

Aby otworzyć karta **pul elastyczne SQL** , kliknij **pul elastyczne SQL**i następnie kliknij pulę, którą chcesz pracować z lub kliknij przycisk **+ Dodaj** tworzenie puli. Aby uzyskać szczegółowe informacje zobacz [Tworzenie puli elastyczne bazy danych za pomocą portalu Azure](sql-database-elastic-pool-create-portal.md).

![Elastyczne pul SQL](./media/sql-database-manage-portal/elastic-pools.png)



## <a name="how-do-i-update-or-view-sql-database-settings"></a>Jak zaktualizować lub wyświetlanie ustawień bazy danych SQL

Aby wyświetlić lub zaktualizować ustawienia bazy danych, kliknij odpowiednie ustawienie na karta bazy danych SQL:


![Ustawienia bazy danych SQL](./media/sql-database-manage-portal/settings.png)


## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Jak znaleźć nazwy serwera w pełni kwalifikowana bazy danych SQL

Aby wyświetlić nazwy serwera baz danych, kliknij pozycję **Przegląd** na karta **bazy danych SQL** i zanotuj nazwę serwera:


![Ustawienia bazy danych SQL](./media/sql-database-manage-portal/server-name.png)


## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Jak zarządzać reguły zapory do kontrolowania dostępu programu SQL server i bazy danych?

Aby wyświetlić, tworzenie lub aktualizowanie reguł zapory, kliknij przycisk **Ustaw zaporę serwera** na karta **bazy danych SQL** . Aby uzyskać szczegółowe informacje zobacz [Konfigurowanie reguły zapory na poziomie serwera bazy danych SQL Azure za pomocą portalu Azure](sql-database-configure-firewall-settings.md).


![reguły zapory](./media/sql-database-manage-portal/sql-database-firewall.png)


## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Jak zmienić moje SQL bazy danych usługi warstwa poziomu?


Aby zaktualizować poziom warstwa lub wydajności usługi z bazą danych SQL, polecenie **Warstwa ceny (skala DTUs)** karta **bazy danych SQL** . Aby uzyskać szczegółowe informacje zobacz [Zmienianie usługi warstwa i wydajności poziomu (poziomu cen) z bazą danych SQL](sql-database-scale-up.md).


![Cennik warstwy](./media/sql-database-manage-portal/pricing-tier.png)


## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Jak skonfigurować inspekcję i zagrożenie wykrywania dla bazy danych SQL

Aby skonfigurować inspekcji i zagrożenie wykrywania dla bazy danych SQL, kliknij **wykrywania Inspekcja i zagrożenia** na karta **bazy danych SQL** . Aby uzyskać szczegółowe informacje zobacz [wprowadzenie inspekcja bazy danych SQL](sql-database-auditing-get-started.md)i [rozpocząć pracę z wykrywania zagrożenie bazy danych SQL](sql-database-threat-detection-get-started.md).


## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Jak skonfigurować dynamiczne dane maskowanie dla bazy danych SQL?

Aby skonfigurować dynamiczne dane maskowanie dla bazy danych SQL, polecenie **dynamiczne dane maskowanie** karta **bazy danych SQL** . Aby uzyskać szczegółowe informacje zobacz [Wprowadzenie do programu SQL bazy danych dynamicznych danych maskowanie](sql-database-dynamic-data-masking-get-started.md).


## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Jak skonfigurować szyfrowanie danych przezroczysty (TDE) dla bazy danych SQL?

Aby skonfigurować szyfrowanie przezroczysty danych w bazie danych SQL, kliknij **szyfrowania przezroczysty danych** na karta **bazy danych SQL** . Aby uzyskać szczegółowe informacje zobacz [Włączanie TDE bazy danych za pomocą portalu](https://msdn.microsoft.com/library/dn948096#Anchor_1).

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Jak wyświetlić lub zmienić maksymalny rozmiar bazy danych SQL

Aby wyświetlić lub zmienić jego rozmiar bazy danych SQL, kliknij pozycję **rozmiar bazy danych** na karta **bazy danych SQL** . Aktualizowanie maksymalny rozmiar bazy danych, zmieniając poziomy warstwa lub wydajności usługi. Aby uzyskać szczegółowe informacje zobacz [Zmienianie usługi warstwa i wydajności poziomu (poziomu cen) z bazą danych SQL](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Jak można monitorować i zwiększyć wydajność bazy danych SQL

Aby monitorować i poprawić wydajność właściwości bazy danych SQL, kliknij przycisk **Podgląd wydajności** na karta **bazy danych SQL** . Aby uzyskać szczegółowe informacje zobacz [Wglądu wydajności bazy danych SQL](sql-database-performance.md).


## <a name="how-do-i-configure-geo-replication"></a>Jak skonfigurować replikacji Geo?

Aby skonfigurować Geo Replikacja bazy danych SQL, polecenie **Replikacji Geo** karta **bazy danych SQL** . Aby uzyskać szczegółowe informacje zobacz [Konfigurowanie Geo Replikacja bazy danych SQL Azure Portal Azure](sql-database-geo-replication-portal.md).


## <a name="how-do-i-failover-to-a-geo-replicated-sql-database"></a>Jak przełączanie awaryjne do bazy danych SQL replikowane geo?

Przełączanie awaryjne do replikowane geo pomocniczym **Geo replikacji** na karta **bazy danych SQL** , klikając **pracy awaryjnej**. Aby uzyskać szczegółowe informacje zobacz [zainicjować planowane lub niezaplanowane awaryjnego bazy danych SQL Azure Portal Azure](sql-database-geo-replication-failover-portal.md).


## <a name="how-do-i-copy-a-sql-database"></a>Jak skopiować z bazą danych SQL?

Aby skopiować z bazą danych SQL, kliknij przycisk **Kopiuj** na karta **bazy danych SQL** . Aby uzyskać szczegółowe informacje zobacz [Kopiowanie bazy danych programu Azure SQL za pomocą portalu Azure](sql-database-copy-portal.md).


![Ustawienia bazy danych SQL](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Jak archiwizować bazy danych programu Azure SQL w pliku PLECAK?

Aby utworzyć PLECAK bazy danych SQL, kliknij polecenie **Eksportuj** w karta **bazy danych SQL** . Aby uzyskać szczegółowe informacje zobacz [archiwum bazy danych programu Azure SQL plik PLECAK za pomocą portalu Azure](sql-database-export.md).


![Eksportowanie do bazy danych SQL](./media/sql-database-manage-portal/sql-database-export.png)



## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Jak przywrócić bazy danych SQL do poprzedniego punktu w czasie?

Aby przywrócić bazy danych SQL, kliknij pozycję **Przywróć** na karta **bazy danych SQL** . Aby uzyskać szczegółowe informacje zobacz [Przywracanie bazy danych SQL Azure do poprzedniego punktu w czasie Portal Azure](sql-database-point-in-time-restore-portal.md).


![Ustawienia bazy danych SQL](./media/sql-database-manage-portal/sql-database-restore.png)


## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Jak utworzyć bazy danych programu Azure SQL z pliku PLECAK?

Aby utworzyć bazy danych SQL z pliku PLECAK kliknij pozycję **Importuj bazę danych** na karta **programu SQL server** . Aby uzyskać szczegółowe informacje zobacz [Importowanie pliku PLECAK do utworzenia bazy danych programu Azure SQL](sql-database-import.md).


![Program SQL server](./media/sql-database-manage-portal/server-commands.png)


## <a name="how-do-i-restore-a-deleted-sql-database"></a>Jak przywrócić usunięty bazy danych SQL?

Aby przywrócić usunięty bazy danych SQL, kliknij opcję **usunięte baz danych** na karta **programu SQL server** (serwer SQL, która zawierała bazy danych, który został usunięty). Aby uzyskać szczegółowe informacje zobacz [Przywracanie usuniętych bazą danych Azure SQL za pomocą portalu Azure](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Jak usunąć bazy danych SQL

Aby usunąć z bazą danych SQL, kliknij polecenie **Usuń** w karta **bazy danych SQL** . 

![Ustawienia bazy danych SQL](./media/sql-database-manage-portal/sql-database-delete.png)



## <a name="additional-resources"></a>Dodatkowe zasoby

- [Baza danych SQL](sql-database-technical-overview.md)
- [Monitorowanie i zarządzanie nimi puli elastyczne bazy danych za pomocą portalu Azure](sql-database-elastic-pool-manage-portal.md)
