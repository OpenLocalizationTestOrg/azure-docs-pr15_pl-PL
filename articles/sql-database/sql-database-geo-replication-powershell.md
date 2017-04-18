<properties 
    pageTitle="Konfigurowanie aktywnej replikacji Geo dla bazy danych SQL Azure za pomocą programu PowerShell | Microsoft Azure" 
    description="Konfigurowanie aktywnej replikacji Geo dla bazy danych SQL Azure za pomocą programu PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
   ms.workload="NA"
    ms.date="07/14/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-powershell"></a>Konfigurowanie replikacji Geo dla bazy danych Azure SQL za pomocą programu PowerShell

> [AZURE.SELECTOR]
- [Omówienie](sql-database-geo-replication-overview.md)
- [Azure Portal](sql-database-geo-replication-portal.md)
- [Programu PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

W tym artykule pokazano, jak skonfigurować aktywnej replikacji Geo bazy danych SQL przy użyciu programu PowerShell.

Aby zainicjować pracy awaryjnej przy użyciu programu PowerShell, zobacz [zainicjować planowane lub niezaplanowane awaryjnego bazy danych SQL Azure za pomocą programu PowerShell](sql-database-geo-replication-failover-powershell.md).

>[AZURE.NOTE] Aktywne replikacji Geo (czytelne pomocnicze) jest teraz dostępny dla wszystkich baz danych ze wszystkich warstw usługi. W kwietnia 2017-czytelne typu pomocniczego zostanie wycofana i istniejące bazy danych nie do odczytu zostanie automatycznie uaktualniony pomocnicze czytelne.



Przed skonfigurowaniem aktywnej replikacji Geo przy użyciu programu PowerShell, należy następujące czynności:

- Subskrypcję usługi Azure. 
- Baza danych Azure SQL - podstawowej bazy danych, który chcesz odtworzyć.
- Azure PowerShell 1.0 lub nowsza. Można pobrać i zainstalować moduł programu PowerShell Azure przez następujące [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).


## <a name="configure-your-credentials-and-select-your-subscription"></a>Skonfiguruj poświadczenia i wybierz subskrypcję

Najpierw należy ustanowić dostępu do konta Azure tak Uruchom programu PowerShell, a następnie uruchom następujące polecenie cmdlet. Na ekranie logowania wprowadź tę samą poczty e-mail i hasło, którego używasz, aby zalogować się do portalu Azure.


    Login-AzureRmAccount

Po pomyślnym zalogowaniu się pewne informacje będą wyświetlane na ekranie, która zawiera identyfikator zalogowani przy użyciu i Azure subskrypcje, do których masz dostęp do.


### <a name="select-your-azure-subscription"></a>Wybierz subskrypcję Azure

Aby Wybierz subskrypcję, potrzebujesz subskrypcji identyfikatora. Identyfikator subskrypcji można kopiować z informacji wyświetlanych w poprzednim kroku, lub jeśli masz wiele subskrypcji i chcesz uzyskać więcej informacji można uruchomić polecenia cmdlet **Get-AzureRmSubscription** i skopiuj informacje o odpowiedniej subskrypcji w zestawie wyników. Następujące polecenie cmdlet używa identyfikator subskrypcji, aby ustawić bieżącej subskrypcji:

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

Po pomyślnym uruchomieniu **AzureRmSubscription wybierz** wyświetlasz programu PowerShell monitu.


## <a name="add-secondary-database"></a>Dodawanie pomocniczej bazy danych


Poniższe kroki Utwórz nową bazę pomocniczej partnerskie Geo replikacji.  
  
Aby włączyć pomocniczym musi być właścicielem subskrypcji lub współwłaściciela. 

Polecenia cmdlet **New-AzureRmSqlDatabaseSecondary** umożliwia dodawanie że pomocniczej bazy danych na serwerze partnera z lokalną bazą danych na serwerze, do którego są podłączone (podstawowy bazy danych). 

To polecenie cmdlet zastępuje **Start AzureSqlDatabaseCopy** z parametrem **– IsContinuous** .  Będzie on wyjściowy obiektu **AzureRmSqlDatabaseSecondary** , które mogą być używane przez innych poleceń cmdlet do wyraźnie wskazują łącza replikacji określonych. To polecenie cmdlet zwróci podczas pomocniczego bazy danych jest w pełni obsługiwany. W zależności od rozmiaru bazy danych może potrwać od minut do godziny.

Zreplikowanej bazy danych na serwerze pomocniczym będą mieć taką samą nazwę jak bazy danych na serwerze podstawowym i będzie domyślnie mają taki sam poziom usługi. Pomocniczej bazy danych może być czytelny lub nie do odczytu i może być jednej bazie danych lub elastycznych bazy danych. Aby uzyskać więcej informacji zobacz [Nowy AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) i [Warstwy usługi](sql-database-service-tiers.md).
Po pomocniczej jest tworzony i obsługiwany, danych rozpocznie się replikacji od podstawowej bazy danych do nowej bazy danych pomocniczą. Poniżej opisano sposób wykonania tego zadania, aby utworzyć pomocnicze nie do odczytu i poprawić jego czytelność, przy jednej bazie danych lub elastycznych bazy danych przy użyciu programu PowerShell.

Jeśli bazy danych partnera już istnieje (na przykład - zakończenia poprzedniego relacji Geo replikacji) to polecenie nie powiedzie się.



### <a name="add-a-non-readable-secondary-single-database"></a>Dodawanie pomocniczym nie do odczytu (jednej bazie danych)

Następujące polecenie tworzy-czytelne pomocniczym bazy danych "mydb" z serwera "srv2" w grupie zasobów "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "No"



### <a name="add-readable-secondary-single-database"></a>Dodawanie czytelne pomocniczego (jednej bazie danych)

Następujące polecenie tworzy czytelne pomocniczym bazy danych "mydb" z serwera "srv2" w grupie zasobów "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"




### <a name="add-a-non-readable-secondary-elastic-database"></a>Dodawanie pomocniczym nie do odczytu (elastyczne bazy danych)

Następujące polecenie tworzy-czytelne pomocniczym bazy danych "mydb" w puli elastyczne bazy danych o nazwie "ElasticPool1" z serwera "srv2" w grupie zasobów "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "No"


### <a name="add-a-readable-secondary-elastic-database"></a>Dodawanie czytelne pomocniczym (elastyczne bazy danych)

Następujące polecenie tworzy czytelne pomocniczym bazy danych "mydb" w puli elastyczne bazy danych o nazwie "ElasticPool1" z serwera "srv2" w grupie zasobów "rg2":

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "All"





## <a name="remove-secondary-database"></a>Usuwanie pomocniczego bazy danych

Polecenie cmdlet **AzureRmSqlDatabaseSecondary Usuń** trwale zakończyć partnerstwa replikacji między pomocniczej bazy danych i jego podstawowym. Po zakończeniu relacji pomocniczego bazy danych staje się odczytu i zapisu bazy danych. Jeśli łączności z pomocniczego bazy danych zostało przerwane polecenia zakończyło się powodzeniem, ale pomocniczej będzie odczytu i zapisu, po przywróceniu łączności. Aby uzyskać więcej informacji zobacz [Usuwanie AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) i [Warstwy usługi](sql-database-service-tiers.md).

To polecenie cmdlet zastępuje AzureSqlDatabaseCopy tabulatora replikacji. 

Usunięcie odpowiada wymuszonego zakończenia, która powoduje usunięcie łącza replikacji i pozostawienie byłego pomocniczej jako autonomicznego bazy danych, które nie są w pełni replikowane przed zakończeniem. Wszystkie dane łącze będą czyszczone byłego podstawowego i była pomocniczym, lub gdy są one dostępne. To polecenie cmdlet zwróci usunięcie łącza replikacji. 


Aby usunąć pomocniczym, użytkownicy powinni mieć uprawnienia do zapisu w podstawowych i pomocniczych baz danych według RBAC. Zobacz kontrola dostępu oparta na rolach, aby uzyskać szczegółowe informacje.

Poniższy Usuwa łącza replikacji bazy danych o nazwie "mydb" do serwera "srv2" Grupa zasobów "rg2". 

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –SecondaryResourceGroup "rg2" –PartnerServerName "srv2"
    $secondaryLink | Remove-AzureRmSqlDatabaseSecondary 


## <a name="monitor-geo-replication-configuration-and-health"></a>Monitorowanie konfiguracji replikacji geo i kondycji

Monitorowania zadania obejmują monitorowanie konfiguracji replikacji geo i monitorowanie kondycji replikacji danych.  

[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) można pobrać informacji o widoczny w widoku wykazu sys.geo_replication_links łącza replikacji do przodu.

Poniższe polecenie pobiera stan łącza replikacji między podstawowej bazy danych "mydb" i pomocniczego na serwerze "srv2" Grupa zasobów "rg2".

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" -ResourceGroupName "rg1" -ServerName "srv1"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –PartnerResourceGroup "rg2” –PartnerServerName "srv2”


## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej o aktywnej Geo replikacji, zobacz - [Aktywnej replikacji Geo](sql-database-geo-replication-overview.md)
- Przegląd ciągłości działalności i scenariuszy zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md)

