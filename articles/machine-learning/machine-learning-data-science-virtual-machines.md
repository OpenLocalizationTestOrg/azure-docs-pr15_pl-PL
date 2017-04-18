<properties
    pageTitle="Maszyn wirtualnych nauki danych platformy Azure | Microsoft Azure"
    description="Ustawianie w górę maszyny wirtualnej nauki danych"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="xibingao;bradsev" />

# <a name="data-science-virtual-machines-in-azure"></a>Maszyn wirtualnych nauki danych platformy Azure

Instrukcje są tutaj pod warunkiem, że opisują sposobu konfigurowania maszyn wirtualnych Azure i maszyn wirtualnych Azure za pomocą usługi SQL jako serwery IPython notesu. Maszyny wirtualnej Windows skonfigurowano obsługi narzędzi, takich jak IPython notesu, Eksplorator magazynu Azure i AzCopy, jak również innych narzędzi, które są przydatne w przypadku projektów nauki danych. Eksplorator magazynu Azure i AzCopy, na przykład udostępniają wygodnie przekazać danych do magazynowania Azure z komputera lokalnego lub go pobrać na komputer lokalny z magazynu. 

Ten menu łącza do tematów opisujących, jak skonfigurować w różnych środowiskach nauki dane używane przez [Zespół danych nauki proces (TDSP)](data-science-process-overview.md).

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Kilka typów Azure maszyn wirtualnych można obsługi administracyjnej i skonfigurowana do używania w ramach środowiska nauki opartej na chmurze danych. Decyzja o które podtyp maszyn wirtualnych umożliwia zależy od typu i ilość danych, aby być w modelu z komputera szkoleniowe i docelowej dla danych w chmurze. 

* Wskazówki dotyczące pytania brać pod uwagę podczas podejmowania decyzji tego zobacz [Planowanie i Azure maszynowego nauki danych nauki środowisko](machine-learning-data-science-plan-your-environment.md). 
* Aby wykaz niektórych scenariuszy, które mogą wystąpić podczas zaawansowanej analizy zobacz [scenariusze proces zaawansowane analizy i technologii w Azure maszynowego uczenia](machine-learning-data-science-plan-sample-scenarios.md)

Dostępne są dwa zestawy instrukcje:

* [Konfigurowanie Azure maszyn wirtualnych jako serwer notesu IPython zaawansowanej analizy](machine-learning-data-science-setup-virtual-machine.md) przedstawiono sposoby obsługi administracyjnej Azure maszyn wirtualnych z notesu IPython i inne narzędzia używane do nauki danych na wypadek, w których Azure magazyn niż SQL może służyć do przechowywania danych.

* [Konfigurowanie maszyn wirtualnych Azure SQL Server jako serwer notesu IPython zaawansowanej analizy](machine-learning-data-science-setup-sql-server-virtual-machine.md) pokazano, jak inicjować obsługę maszyn wirtualnych Azure SQL Server z notesu IPython i inne narzędzia używane do nauki danych na wypadek, w których baza danych SQL może służyć do przechowywania danych.

Po ustanawianie i skonfigurowany tych maszyn wirtualnych są gotowe do użycia jako serwery notesu IPython badań i przetwarzania danych oraz inne zadania wymagane w połączeniu z Azure maszynowego uczenia się i proces nauki danych zespołu (TDSP). Następne kroki w procesie nauki danych są mapowane w [ścieżka nauki TDSP](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) i może obejmować czynności, które przenoszenie danych do programu SQL Server lub HDInsight, procesu i przykładowe tak w przygotowanie do nauki z danych z nauki maszynowego Azure.


> [AZURE.NOTE] Azure maszyn wirtualnych kosztują jako **zapłacić tylko w przypadku możesz użyć**. Aby upewnić się, że nie jest konta podczas nie przy użyciu komputera wirtualnych, musi ona być w stanie **zatrzymania (Deallocated)** w [Klasycznym Portal Azure](http://manage.windowsazure.com/). W przypadku lub jak cofnąć przydział maszyn wirtualnych instrukcje krok po kroku, zobacz [zamknięcia i cofnąć maszyn wirtualnych nieużywane](machine-learning-data-science-setup-virtual-machine.md#shutdown)
 
