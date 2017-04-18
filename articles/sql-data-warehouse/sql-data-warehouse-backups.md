<properties
   pageTitle="Kopie zapasowe magazynu danych SQL | Microsoft Azure"
   description="Informacje na temat kopie zapasowe wbudowanych bazy danych SQL Data Warehouse, umożliwiające przywracanie magazynu danych SQL Azure punkt przywracania lub innego regionu geograficznego."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lakshmi1812"
   manager="barbkess"
   editor="monicar"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="lakshmir;barbkess"/>

# <a name="sql-data-warehouse-backups"></a>Kopie zapasowe magazynu danych SQL

Program SQL Data Warehouse oferuje lokalnymi i geograficznych kopie zapasowe jako część możliwości kopii zapasowej magazynu danych. Należą do obiektów Blob miejsca do magazynowania Azure migawek i zbędne geo miejsca do magazynowania. Kopie zapasowe magazynu danych umożliwia przywracanie magazynu danych do punktu przywracania w regionie podstawowego lub przywrócić do innego regionu geograficznego. W tym artykule opisano szczegółowe informacje na temat wykonywania kopii zapasowych w magazynie danych SQL.

## <a name="what-is-a-data-warehouse-backup"></a>Co to jest kopia zapasowa magazynu danych?

Kopia zapasowa magazynu danych jest dane, które można przywrócić magazynu danych do określonego czasu.  Ponieważ program SQL Data Warehouse jest rozproszony system, kopia zapasowa magazynu danych składa się z wielu plików, które są przechowywane w Azure obiektów blob. 

Kopie zapasowe bazy danych są istotne część dowolnego strategii odzyskiwania firm, jak ciągłości i danych, ponieważ chronią dane z przypadkowe uszkodzenia lub usunięcia. Aby uzyskać więcej informacji zobacz [Omówienie ciągłości firm](../sql-database/sql-database-business-continuity.md).

## <a name="data-redundancy"></a>Nadmiarowości danych

Program SQL Data Warehouse chroni dane dane są przechowywane w magazynie Premium Azure, lokalnie zbędne (LRS). Ta funkcja magazyn Azure przechowuje wiele kopii synchroniczne danych w centrum danych lokalnych w celu zagwarantowania ochrony danych przezroczysty występują błędy zlokalizowanym. Nadmiarowości danych gwarantuje, że dane można po infrastruktury problemów, takie jak awarii dysku. Nadmiarowości danych zapewnia ciągłości z odporność na uszkodzenia i wysokiej dostępności infrastruktury.

Aby uzyskać więcej informacji:

- Azure miejscem do magazynowania Premium, zobacz [Wprowadzenie do przechowywania Premium Azure](../storage/storage-premium-storage.md).
- Lokalnie zbędne miejscem do magazynowania, zobacz [Replikacja magazyn Azure](../storage/storage-redundancy.md#locally-redundant-storage).


## <a name="azure-storage-blob-snapshots"></a>Migawki magazyn obiektów Blob platformy Azure

Zaletą korzystania z magazynu Premium Azure SQL Data Warehouse stosuje migawek obiektów Blob miejsca do magazynowania Azure wykonywania kopii zapasowej magazynu danych lokalnie. Magazyn danych można przywrócić do punktu przywracania migawkę. Migawki uruchomić co najmniej co osiem godzin i są dostępne przez siedem dni.  

Aby uzyskać więcej informacji:

- Migawki obiektów blob platformy Azure, zobacz [Tworzenie migawki obiektów blob](../storage/storage-blob-snapshots.md).


## <a name="geo-redundant-backups"></a>Zbędne Geo wykonywania kopii zapasowych

Co 24 godziny, magazynu danych SQL przechowuje magazynu danych w standardowej miejsca do magazynowania. Magazyn danych zostanie utworzona zgodnie z czas ostatniej migawkę. Standardowego magazynu należy konto zbędne geo miejsca do magazynowania z dostępem do odczytu (Pomoc Zdalna GRS). Funkcja miejsca do magazynowania Azure pomoc Zdalna-GRS replikuje plików kopii zapasowej do [Centrum danych iloczynów](../best-practices-availability-paired-regions.md). Replikacja geo gwarantuje, że w przypadku, gdy nie masz dostępu do migawki w danym regionie podstawowego można przywrócić magazynu danych. 

Ta funkcja jest domyślnie włączone. Jeśli nie chcesz używać zbędne geo kopie zapasowe, możesz wybrać opcję. 

>[AZURE.NOTE] W magazynie Azure terminów *replikacji* odwołuje się do kopiowania plików z jednej lokalizacji do drugiej. SQL do *Replikacja bazy danych* odwołuje się do pozostawiania wiele baz danych pomocniczej synchronizowane z podstawową bazą danych. 

Aby uzyskać więcej informacji:
- Zbędne Geo miejscem do magazynowania, zobacz [Replikacja magazyn Azure](../storage/storage-redundancy.md).
- Pomoc Zdalna GRS miejscem do magazynowania, zobacz [Magazyn zbędne geo dostęp do odczytu](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Planowanie kopii zapasowej magazynu danych i okres przechowywania

Magazynu danych SQL tworzy migawki w magazynie online danych co godziny czterech do ośmiu i zachowuje każdego migawkę przez siedem dni. Online bazy danych na jednym z punktów przywracania można przywrócić w ciągu ostatnich siedmiu dni. 

Aby wyświetlić rozpoczęcie ostatniej migawki, uruchom tę kwerendę w magazynie online danych SQL. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Jeśli chcesz zachować migawkę dłużej niż siedem dni, można przywrócić punkt przywracania do nowego magazynu danych. Po zakończeniu przywracania magazynu danych SQL uruchamia tworzenia migawek na nowego magazynu danych. Jeśli nie wprowadzisz zmiany do nowego magazynu danych migawki pozostać puste i w związku z tym koszt migawki jest minimalny. Można również wstrzymać bazy danych, aby uniemożliwić tworzenia migawek magazynu danych SQL.


### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Co się stanie z moim przechowywania kopii zapasowej podczas została wstrzymana Moje magazynu danych?

SQL Data Warehouse nie tworzenia migawek i nie wygasa migawek podczas magazynu danych została wstrzymana. Wiek migawki nie zmienia się podczas magazynu danych została wstrzymana. Migawka przechowywania jest oparty na liczbę dni, które magazynu danych jest w trybie online, nie dni kalendarza.

Na przykład jeśli migawki zaczyna się 1 października godzinie 4 i magazynu danych została wstrzymana 3 października godzinie 4, migawki jest dwa dni. Po powrocie do trybu online zawiera magazynu danych migawki jest dwa dni. Jeśli magazynu danych przechodzi do trybu online 5 października godzinie 4, migawki jest dwa dni i pozostaje przez pięć dni więcej.

Gdy magazynu danych przechodzi do trybu online, magazynu danych SQL życiorysy nowych migawek i wygaśnięciu migawek, gdy zawierają więcej niż siedem dni danych.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Jak długo trwa okres przechowywania magazynu porzucone danych?
Po upuszczeniu magazynu danych, magazynu danych i migawki są zapisywane przez siedem dni i następnie usunięta. Na dowolnej punktów przywracania zapisanego można przywrócić magazynu danych.

> [AZURE.IMPORTANT] Jeśli usuniesz logiczne instalacja programu SQL server, wszystkich baz danych, które należą do wystąpienia usuwane są również i nie można odzyskać. Nie można przywrócić usunięty serwera.

## <a name="data-warehouse-backup-costs"></a>Koszty kopii zapasowej magazynu danych

Całkowitego kosztu magazynu danych podstawowych i siedem dni migawek obiektów Blob platformy Azure jest zaokrąglana do najbliższej TB. Na przykład jeśli magazynu danych jest 1,5 TB i migawki za pomocą 100 GB, konta do 2 TB danych szybkością magazyn Premium Azure. 

>[AZURE.NOTE] Tworzenie każdej migawki jest pusta początkowo i rozwoju podczas wprowadzania zmian w magazynie danych podstawowych. Wszystkich migawek zwiększyć rozmiar wraz ze zmianą magazynu danych. Dlatego kosztów miejsca do magazynowania dla migawki Powiększ według szybkość zmian.

Jeśli używasz zbędne geo miejsca do magazynowania, otrzymujesz opłatę oddzielnie. Zbędne geo magazynowania jest wystawiona stawki standardowej odczytu geograficznie zbędne miejsca do magazynowania (Pomoc Zdalna GRS).

Aby uzyskać więcej informacji o cenach magazynu danych SQL zobacz [Ceny magazynu danych SQL](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="using-database-backups"></a>Za pomocą kopie zapasowe bazy danych

Podstawowe użycie kopii zapasowych magazynu danych SQL jest przywrócić magazynu danych do jednego z punktów przywracania w okresie zachowywania.  

- Aby przywrócić magazynu danych SQL, zobacz [Przywracanie magazynu danych SQL](sql-data-warehouse-restore-database-overview.md).


## <a name="related-topics"></a>Tematy pokrewne

### <a name="scenarios"></a>Scenariusze

- Aby uzyskać omówienie ciągłości firm zobacz [Omówienie ciągłości firm](../sql-database/sql-database-business-continuity.md)


<!-- ### Tasks -->

- Aby przywrócić magazynu danych, zobacz [Przywracanie magazynu danych SQL](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

