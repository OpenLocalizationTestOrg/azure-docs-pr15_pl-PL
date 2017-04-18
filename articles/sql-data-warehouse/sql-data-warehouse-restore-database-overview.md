<properties
   pageTitle="Przywracanie magazynu danych SQL | Microsoft Azure"
   description="Omówienie opcji przywracania bazy danych do bazy danych w magazynie danych SQL Azure odzyskiwania."
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
   ms.date="09/29/2016"
   ms.author="lakshmir;barbkess;sonyama"/>


# <a name="sql-data-warehouse-restore"></a>Przywracanie magazynu danych SQL

> [AZURE.SELECTOR]
- [Omówienie][]
- [Portal][]
- [Programu PowerShell][]
- [POZOSTAŁE][]

Magazynu danych SQL oferuje przywraca lokalnymi i geograficzne jako część jego uszkodzenia magazynu danych, funkcje odzyskiwania. Umożliwia przywracanie magazynu danych do punktu przywracania w regionie podstawowego kopie zapasowe magazynu danych lub użyj zbędne geo kopie zapasowe przywrócić innego regionu geograficznego. W tym artykule opisano szczegółowe informacje na temat przywracania magazynu danych.

## <a name="what-is-a-data-warehouse-restore"></a>Co to jest Przywracanie magazynu danych?

Przywracanie magazynu danych jest nowego magazynu danych utworzonego na podstawie kopii zapasowej istniejącego lub usunięte dane magazynu. Magazyn przywracane dane odtwarza magazynu danych kopii zapasowej w określonym czasie. Ponieważ program SQL Data Warehouse jest rozproszony system, Przywracanie magazynu danych zostanie utworzona z wielu plików kopii zapasowej, które są przechowywane w Azure obiektów blob. 

Przywracanie bazy danych jest istotną część dowolnego strategii odzyskiwania firm, jak ciągłości i danych, ponieważ ponownie tworzy danych po przypadkowe uszkodzenia lub usunięcia.

Aby uzyskać więcej informacji zobacz:

-  [Kopie zapasowe magazynu danych SQL](sql-data-warehouse-backups.md)
-  [Przegląd ciągłości działalności](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Punkty przywracania magazynu danych

Zaletą korzystania z magazynu Premium Azure magazynu danych SQL stosuje migawek obiektów Blob miejsca do magazynowania Azure wykonywania kopii zapasowej magazynu danych podstawowego. Każdy migawki ma punkt przywracania reprezentujący uruchomienia migawki. Aby przywrócić magazynu danych, możesz wybrać punkt przywracania i wydaj polecenie Przywróć.  

Program SQL Data Warehouse zawsze przywraca kopii zapasowej nowego magazynu danych. Możesz zachować magazynu przywracane dane i bieżąca lub usunąć jeden z nich. Jeśli chcesz zastąpić bieżącego magazynu danych magazynu przywracane dane, można ją zmienić.

Jeśli chcesz przywrócić magazynu danych usuniętych lub wstrzymania, możesz [utworzyć bilet pomocy technicznej](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>Przywracanie zbędne Geo

Jeśli korzystasz z zbędne geo przestrzeni dyskowej, można przywrócić magazynu danych do Twojego [Centrum danych iloczynów](../best-practices-availability-paired-regions.md) na innego regionu geograficznego. Po przywróceniu magazynu danych z ostatniego codziennego wykonywania kopii zapasowej. 

## <a name="restore-timeline"></a>Przywracanie osi czasu

Można przywrócić bazę danych z dowolnego punktu przywracania dostępne w ciągu ostatnich siedmiu dni. Migawki Uruchom co godziny czterech do ośmiu i są dostępne przez siedem dni. W przypadku starszych niż siedem dni migawkę jej wygaśnięciu i punktu przywracania nie jest już dostępna.

## <a name="restore-costs"></a>Przywracanie kosztów

Dodatkowego miejsca do magazynowania dla magazynu przywracane dane jest wystawiona stawki magazyn Premium Azure. 

Jeśli zatrzymasz magazynu przywracane dane są naliczane do przechowywania stawki magazyn Premium Azure. Zaletą wstrzymywanie jest, że użytkownik nie jest naliczany dla zasobów komputerowych DWU.

Aby uzyskać więcej informacji o cenach magazynu danych SQL zobacz [Ceny magazynu danych SQL](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Zastosowanie do przywrócenia

Używanie podstawowego do przywracania magazynu danych jest odzyskiwanie danych po utraty lub uszkodzenia danych przypadkowe.

Za pomocą przywracania magazynu danych do przechowywania kopii zapasowej przez dłużej niż siedem dni. Po przywróceniu kopii zapasowej, masz magazynu danych online i rozwiązaniem czas nieokreślony, aby zapisać koszty obliczeń. Wstrzymanie bazy danych z opłatami miejsca do magazynowania stawki magazyn Premium Azure. 

## <a name="related-topics"></a>Tematy pokrewne

### <a name="scenarios"></a>Scenariusze

- Aby uzyskać omówienie ciągłości firm zobacz [Omówienie ciągłości firm](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

Aby wykonać przywracanie magazynu danych, przywracanie przy użyciu:

- Azure portal, zobacz [Przywracanie magazynu danych za pomocą portalu Azure](sql-data-warehouse-restore-database-portal.md)
- Polecenia cmdlet programu PowerShell, zobacz [Przywracanie magazynu danych przy użyciu poleceń cmdlet programu PowerShell](sql-data-warehouse-restore-database-powershell.md)
- Umieść interfejsów API, zobacz [Przywracanie magazynu danych za pomocą interfejsów API REST](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Omówienie]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[Programu PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[POZOSTAŁE]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
