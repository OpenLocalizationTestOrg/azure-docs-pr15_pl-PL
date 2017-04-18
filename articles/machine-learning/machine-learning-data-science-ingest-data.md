<properties 
    pageTitle="Ładowanie danych do środowiska miejsca do magazynowania dla analizy | Microsoft Azure" 
    description="Przenoszenie danych do i z magazynem obiektów Blob Azure" 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="bradsev" />

# <a name="load-data-into-storage-environments-for-analytics"></a>Ładowanie danych do środowiska miejsca do magazynowania dla analizy

Proces nauki danych zespołu wymaga danych można wchłonięte lub załadowana do różnych środowiskach magazynu innego przetwarzania lub analizowane w najlepszy sposób na każdym etapie procesu. Miejsca docelowe danych często używanych do przetwarzania obejmuje magazyn obiektów Blob platformy Azure, bazy danych platformy SQL Azure, SQL Server Azure maszyn wirtualnych, HDInsight (Hadoop) i Azure maszynowego uczenia. 

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Ten **menu** linki do tematy opisujące mogły zjeść tej ostatniej danych w tych środowiskach docelowej miejsce, w którym dane są przechowywane i przetwarzane.

Technicznych i potrzeb biznesowych, a także początkowej lokalizację, format i rozmiar danych określi środowiskach docelowej, do których dane mają być wchłonięte osiągnięciu podczas analizy. Nie jest nietypowych scenariusza wymagać dane mają zostać przeniesione między środowiskami kilka uzyskanie różnych zadań wymaganych do tworzenia modelu przewidywanych. Ta sekwencja zadań można dołączyć, na przykład badań danych, wstępne przetwarzanie czyszczenia, przy próbkowaniu w dół i szkolenia modelu.
