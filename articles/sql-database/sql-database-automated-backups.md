<properties
   pageTitle="Baza danych SQL wykonywania kopii zapasowych — automatyczne, zbędne geo | Microsoft Azure" 
   description="Baza danych SQL automatycznie utworzy kopię zapasową lokalnej bazy danych co pięć minut i używa Azure odczytu zbędne geo miejsca do magazynowania (Pomoc Zdalna GRS) w celu zapewnienia nadmiarowości geo. "
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/20/2016"
   ms.author="carlrab;barbkess"/>

<!------------------
This topic is annotated with TEMPLATE guidelines for FEATURE TOPICS.


Metadata guidelines

pageTitle
    60 characters or less. Includes name of the feature - primary benefit. Not the same as H1. Its 60 characters or fewer including all characters between the quotes and the Microsoft Azure site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "SQL Database automatically creates a local database backup every few minutes and uses Azure read-access geo-redundant storage for geo-redundancy."
------------------>

<!----------------

TEMPLATE GUIDELINES for feature topics

The Feature Topic is a one-pager (ok, sometimes longer) that explains a capability of the product or service. It explains what the capability is and characteristics of the capability.  

It is a "learning" topic, not an action topic.

DO explain this:
    • Definition of the feature terminology.  i.e., What is a database backup?
    • Characteristics and capabilities of the feature. (How the feature works)
    • Common uses with links to overview topics that recommend when to use the feature.
    • Reference specifications (Limitations and Restrictions, Permissions, General Remarks, etc.)
    • Next Steps with links to related overviews, features, and tasks.

DON'T explain this:
    • How to steps for using the feature (Tasks)
    • How to solve business problems that incorporate the feature (Overviews)
------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What is in this topic?" Write the H1 heading in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  
    
    To help people understand this is a learning topic and not an action topic, start the title with "Learn about ... "

    Heading must use an industry standard term. If your feature is a proprietary name like "Elastic database pools", use a synonym. For example:    "Learn about elastic database pools for multi-tenant databases". In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="learn-about-sql-database-backups"></a>Więcej informacji na temat wykonywania kopii zapasowych bazy danych SQL

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top key words that you are using throughout the article.The introduction should be brief and to the point of what the feature is, what it is used for, and what's in the article. 

    If the introduction is short enough, your article can pop to the top in Google Instant Answers.

    In this example:
    
 

Sentence #1 Explains what the article will cover, which is what the feature is or does. This is also the metadata description. 
    SQL Database automatically creates a local database backup every five minutes and uses Azure read-access geo-redundant storage (RA-GRS) to provide geo-redundancy. 

Sentence #2 Explains why I should care about this.  
    Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.

-------------------->

Baza danych SQL automatycznie utworzy kopię zapasową lokalnej bazy danych, co kilka minut i używa Azure odczytu zbędne geo miejsca do magazynowania dla geo nadmiarowości. Kopie zapasowe bazy danych są istotne część dowolnego strategii odzyskiwania firm, jak ciągłości i danych, ponieważ chronią dane z przypadkowe uszkodzenia lub usunięcia. 

<!-- This image needs work, so not putting it in right now.

This diagram shows SQL Database running in the US East region. It creates a database backup every five minutes, which it stores locally to Azure Read Access Geo-redundant Storage (RA-GRS). Azure uses geo-replication to copy the database backups to a paired data center in the US West region.

![geo-restore](./media/sql-database-geo-restore/geo-restore-1.png)

-->

<!---------------
GUIDELINES for the first ## H2.

    The first ## describes what the feature encompasses and how it is used. It points to related task articles.
    
    For consistency, being the heading with "What is ... "
----------------->

## <a name="what-is-a-sql-database-backup"></a>Co to jest kopii zapasowej bazy danych SQL?  

<!-- 
    Explains what a SQL Database backup is and answers an important question that people want to know.
-->

Kopię zapasową bazy danych SQL zawiera zarówno lokalnej bazy danych i zbędne geo kopii zapasowych. Te kopie zapasowe są tworzone automatycznie i bez dodatkowych opłat. Nie musisz niczego robić w celu ułatwienia ich stanie.

<!----------------- 
    Explains first component of the backup feature
------------------>

Dla lokalnej kopii bazy danych SQL korzysta z technologii programu SQL Server do tworzenia kopii zapasowych [Pełny](https://msdn.microsoft.com/library/ms186289.aspx), [różnicy](https://msdn.microsoft.com/library/ms175526.aspx )i [Dziennik transakcji](https://msdn.microsoft.com/library/ms191429.aspx) . Kopie zapasowe dziennika transakcji wystąpić co pięć minut, które można wykonać w chwili przywracania na tym samym serwerze obsługującym bazę danych. Po przywróceniu bazy danych usługi danych liczbowych się, jaki dziennik pełnej, różnicy i transakcji kopie zapasowe trzeba je przywrócić.

<!--------------- 
    Explicit list of what to do with a local backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Za pomocą kopii zapasowej lokalnej bazy danych:

- Przywracanie bazy danych do punktu w czasie w okresie zachowywania. Z kopii zapasowej bazy danych można przywrócić bazę danych do punktu w czasie, przywracanie usuniętych bazy danych do czasu, który został usunięty lub przywracanie bazy danych do innego regionu geograficznego. Aby wykonać przywracania, zobacz [Przywracanie bazy danych z kopii zapasowej bazy danych](sql-database-recovery-using-backups.md).

- Kopiowanie bazy danych do programu SQL server w regionie takich samych lub różnych. Kopiuj jest transakcyjnie zgodne z bieżącej bazy danych SQL. Aby wykonać kopię, zobacz [Kopiowanie bazy danych](sql-database-copy.md).

- Archiwizowanie kopii zapasowej bazy danych poza okres przechowywania kopii zapasowej. Wykonywanie archiwum, [z bazą danych SQL do PLECAK Eksportuj](sql-database-export.md) plik. Można archiwizować PLECAK do długotrwałego przechowywania i zapisać go poza usługi okres przechowywania. Możesz także użyć PLECAK Transfer kopii bazy danych programu SQL Server obsługiwanych lokalnie lub w Azure maszyn wirtualnych (maszyn wirtualnych).

<!----------------- 
    Explains first component of the backup feature
------------------>

Kopii zapasowych zbędne geo bazy danych SQL używa [replikacji magazyn Azure](../storage/storage-redundancy.md). Baza danych SQL są przechowywane pliki kopii zapasowej lokalnej bazy danych na koncie [Odczytu zbędne Geo miejsca do magazynowania (Pomoc Zdalna GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) . Azure replikuje plików kopii zapasowej do [Centrum danych iloczynów](../best-practices-availability-paired-regions.md). 

<!--------------- 
    Explicit list of what to do with a geo-redundant backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Za pomocą kopii zapasowej zbędne geo:

- Przywracanie bazy danych do innego regionu geograficznego, w przypadku, gdy nie masz dostępu do kopii zapasowej bazy danych z Twoim regionie podstawowej bazy danych. 

>[AZURE.NOTE] W magazynie Azure terminów *replikacji* odwołuje się do kopiowania plików z jednej lokalizacji do drugiej. SQL do *Replikacja bazy danych* odwołuje się do pozostawiania wiele baz danych pomocniczej synchronizowane z podstawową bazą danych. 

<!----------------
    The next ## H2's discuss key characteristics of how the feature works. The title is in conversational language and asks the question that will be answered.
------------------->
## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Ile miejsca do magazynowania kopii zapasowej jest dołączany bezpłatnie?

Baza danych SQL zawiera maksymalnie 200% magazynie maksymalna ustanawianie bazy danych jako magazynu kopii zapasowej bez dodatkowych opłat. Na przykład jeśli masz wystąpienie standardowej bazy danych o rozmiarze DB ustanawianie 250 GB, masz 500 GB miejsca w kopii zapasowej bez dodatkowych opłat. Jeśli bazy danych przekracza dostarczonych magazynu kopii zapasowej, możesz zmniejszyć okres przechowywania, kontaktując się z Azure pomocy technicznej. Innym rozwiązaniem jest zapłacić za magazynu dodatkowych kopii zapasowej, który zostanie wystawiona stawki standardowej odczytu geograficznie zbędne miejsca do magazynowania (Pomoc Zdalna GRS). 

## <a name="how-often-do-backups-happen"></a>Jak często stanie kopie zapasowe?

Kopii zapasowych lokalnej bazy danych kopii zapasowych pełnej bazy danych się zdarzyć, co tydzień, wystąpić kopie zapasowe różnicy bazy danych, co godzina i dziennik transakcji, których kopie zapasowe wystąpić co pięć minut. Pierwszy pełnej kopii zapasowej zostaje zaplanowane zaraz po utworzeniu bazy danych. Zazwyczaj zakończeniu przekracza 30 minut, ale może potrwać dłużej gdy baza danych jest znaczącej wielkości. Na przykład wstępnej kopii zapasowej może trwać dłużej przywrócenie bazy danych lub kopii bazy danych. Po pierwszym pełną kopię zapasową wszystkich dodatkowych kopii zapasowych są zaplanowane automatycznie i zarządzanych w trybie cichym w tle. Dokładnie chronometrażu pełny i [różnicy](https://msdn.microsoft.com/library/ms175526.aspx) kopie zapasowe bazy danych jest określona jako jej ogólną obciążenia systemu. 

Kopii zapasowych zbędne geo pełnego i różnicy kopie zapasowe są kopiowane zgodnie z harmonogramem replikacji magazyn Azure.

## <a name="how-long-do-you-keep-my-backups"></a>Jak długo trwa zachować Moje kopie zapasowe?

Każda kopia zapasowa bazy danych SQL występują okres przechowywania, oparty na [warstwa usług](sql-database-service-tiers.md) bazy danych. Okres przechowywania bazy danych w:

<!------------------

    Using a list so the information is easy to find when scanning.
------------------->

- Warstwy podstawowej usługi jest siedem dni.
- Standardowa usługa warstwa jest 35 dni.
- Warstwa usług Premium to 35 dni.


Konwertowanie bazy danych z warstwy usługi Standard lub Premium podstawowy, kopie zapasowe są zapisywane przez siedem dni. Wszystkie istniejące kopie zapasowe starsze niż siedem dni nie są już dostępne. 

Po uaktualnieniu bazy danych z warstwy podstawowej usługi na standardowy lub Premium bazy danych SQL zachowuje istniejące kopie zapasowe, dopóki nie są one 35 dni. Nowe kopie zapasowe jej przechowywana występujące 35 dni.
 
Po usunięciu bazy danych SQL Database przechowywana kopii zapasowych w taki sam sposób, który w ten sposób bazy danych w trybie online. Załóżmy na przykład, możesz usunąć podstawowe bazy danych, która ma okres przechowywania siedem dni. Kopii zapasowej cztery dni jest zapisywany trzy dni.

>[AZURE.IMPORTANT]
    Po usunięciu serwera Azure SQL obsługującego bazy danych programu SQL usuwane są również wszystkie bazy danych, które należą do serwera, a nie można odzyskać. Nie można przywrócić usunięty serwera.

<!-------------------
OPTIONAL section
## Best practices 
--------------------->

<!-------------------
OPTIONAL section
## General remarks
--------------------->

<!-------------------
OPTIONAL section
## Limitations and restrictions
--------------------->

<!-------------------
OPTIONAL section
## Metadata
--------------------->

<!-------------------
OPTIONAL section
## Performance
--------------------->

<!-------------------
OPTIONAL section
## Permissions
--------------------->

<!-------------------
OPTIONAL section
## Security
--------------------->

<!-------------------
GUIDELINES for Next Steps

    The last section is Next Steps. Give a next step that would be relevant to the customer after they have learned about the feature and the tasks associated with it.  Perhaps point them to one or two key scenarios that use this feature.

    You don't need to repeat links you have already given them.
--------------------->

## <a name="next-steps"></a>Następne kroki

Kopie zapasowe bazy danych są istotne część dowolnego strategii odzyskiwania firm, jak ciągłości i danych, ponieważ chronią dane z przypadkowe uszkodzenia lub usunięcia. Aby zobaczyć jak tworzenie kopii zapasowych w szerszym strategii, zobacz [Omówienie ciągłości firm](sql-database-business-continuity.md).


