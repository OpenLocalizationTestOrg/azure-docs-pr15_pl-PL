<properties 
    pageTitle="Zmienianie poziomu warstwa i wydajności usługi bazy danych programu Azure SQL za pomocą programu PowerShell | Microsoft Azure" 
    description="Zmiana poziomu usług i poziom wydajności bazy danych programu Azure SQL pokazano, jak przeskalować bazy danych SQL w górę lub w dół przy użyciu programu PowerShell. Zmienianie warstwie cennik bazy danych Azure SQL za pomocą programu PowerShell." 
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
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>Zmienianie usługi warstwy i wydajności poziomu (poziomu ceny) bazy danych SQL za pomocą programu PowerShell


> [AZURE.SELECTOR]
- [Azure portal](sql-database-scale-up.md)
- [**Programu PowerShell**](sql-database-scale-up-powershell.md)


Warstwy usługi i poziomy wydajności opis funkcji i zasobów dostępnych dla bazy danych SQL i może być aktualizowane jako potrzeb zmiany aplikacji. Aby uzyskać szczegółowe informacje zobacz [Warstwy usługi](sql-database-service-tiers.md).

Należy zauważyć, że zmiana poziomu usług i/lub poziom wydajności bazy danych tworzy replice oryginalnej bazy danych na nowy poziom wydajności, a następnie przełącza połączenia replice. W trakcie tego procesu są tracone żadne dane, ale podczas krótki moment, kiedy możemy przełączyć się do replice, połączenia z bazą danych są wyłączone, aby niektóre transakcje w lotów może zostać przywrócona. Tego okna zmienia się, ale trwa średnio poniżej 4 sekund, a w ponad 99% przypadków jest mniejsza niż 30 sekund. Bardzo rzadko zwłaszcza w przypadku dużej liczby transakcji w lotów w momencie połączenia są wyłączone, to okno może być dłuższy.  

Czas trwania całego procesu Skala up zależy od rozmiar i usług warstwie bazę danych przed i po zmianie. Na przykład 250 GB bazy danych, która jest zmiana do z lub w jednej warstwie standardowa usługa należy wykonać w ciągu 6 godzin. W bazie danych o tym samym rozmiarze, który jest zmieniając poziomy wydajności w warstwie usługi Premium należy wykonać w ciągu 3 godzin.


- Na starszą wersję bazy danych, bazy danych powinien być mniejszy niż maksymalny rozmiar dozwolonych poziomu usługi docelowej. 
- Podczas uaktualniania bazy danych z [Replikacji Geo](sql-database-geo-replication-portal.md) włączone, należy najpierw uaktualnić pomocniczej bazy danych w warstwie odpowiedniej wydajności przed uaktualnieniem podstawowej bazy danych.
- Gdy przechodzeniu z poziomu usługi Premium, należy najpierw zakończyć wszystkie relacje Geo replikacji. Można wykonać kroki opisane w temacie [Odzyskiwanie z awarii](sql-database-disaster-recovery.md) , aby zatrzymać proces replikacji między podstawowych i aktywne pomocniczej bazy danych.
- Oferty usług przywracania są różne dla różnych poziomów usług. Jeśli możesz kontaktować mogą utracić możliwość Przywracanie do punktu w czasie lub dolnym okres przechowywania kopii zapasowej. Aby uzyskać więcej informacji zobacz [i przywracania kopii zapasowej bazy danych SQL Azure](sql-database-business-continuity.md).
- Nowe właściwości bazy danych obowiązują dopiero po zakończeniu wprowadzania zmian.



**Aby zakończyć w tym artykule są potrzebne następujące elementy:**

- Subskrypcję usługi Azure. W razie potrzeby subskrypcji usługi Azure po prostu kliknij **Bezpłatne konto** u góry strony, a następnie wróć do końca w tym artykule.
- Baza danych Azure SQL. Jeśli nie masz bazy danych SQL, należy utworzyć jeden wykonując czynności opisane w tym artykule: [Tworzenie pierwszej bazy danych SQL Azure](sql-database-get-started.md).
- Azure programu PowerShell.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>Zmienianie poziomu warstwa i wydajności usługi bazy danych SQL

Uruchamianie [AzureRmSqlDatabase zestaw] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) polecenia cmdlet i set **-RequestedServiceObjectiveName** do poziomu wydajności odpowiedniej warstwie cennik; na przykład *S0* *S1*, *S2*, *S3*, *P1*, *P2*,...

```
$ResourceGroupName = "resourceGroupName"
    
$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

  

   


## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>Przykładowy skrypt programu PowerShell, aby zmienić poziom warstwa i wydajności usługi bazy danych SQL

Zamienianie ```{variables}``` z wartości (nie dołączaj nawiasy klamrowe).

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"
    
$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"
    
$ServerName = "{server}"
$DatabaseName = "{database}"
    
$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"
    
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId
    
Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```
        


## <a name="next-steps"></a>Następne kroki

- [Skalowanie i](sql-database-elastic-scale-get-started.md)
- [Nawiązywanie połączenia i kwerendy bazy danych SQL za pomocą SSMS](sql-database-connect-query-ssms.md)
- [Eksportowanie bazy danych programu Azure SQL](sql-database-export-powershell.md)

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Przegląd ciągłości działalności](sql-database-business-continuity.md)
- [Dokumentacja bazy danych SQL](http://azure.microsoft.com/documentation/services/sql-database/)
- [Cmdlet bazy danych azure SQL] (http://msdn.microsoft.com/library/azure/mt574084https :/ / msdn.microsoft.com/biblioteki/azure-mt619433(v=azure.300\).aspx.aspx)