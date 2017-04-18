<properties
    pageTitle="Przenoszenie danych do i z magazynem obiektów Blob Azure | Microsoft Azure"
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
    ms.date="09/14/2016"
    ms.author="bradsev;sachouks" />

# <a name="move-data-to-and-from-azure-blob-storage"></a>Przenoszenie danych do i z magazynem obiektów Blob Azure

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Wytyczne dotyczące technologii umożliwia przenoszenie danych do i/lub z magazynem obiektów Blob platformy Azure są połączone poniżej:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]
 
Które metoda jest najbardziej przydatna dla Ciebie w zależności od rozwiązania. Artykuł [scenariusze zaawansowanej analizy w Azure maszynowego uczenia](machine-learning-data-science-plan-sample-scenarios.md) pomaga określić potrzebne dla różnych przepływów pracy nauki dane używane w procesie zaawansowanej analizy zasoby.

> [AZURE.NOTE] Pełne wprowadzenie z magazynem obiektów blob platformy Azure odwoływać się do [Podstawy obiektów Blob platformy Azure](../storage/storage-dotnet-how-to-use-blobs.md) i usługą [obiektów Blob platformy Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).

Alternatywnie można wykonać [Azure Factory danych](https://azure.microsoft.com/services/data-factory/) do: 

- Tworzenie i Planowanie procesu, który pobiera dane z magazynu obiektów blob platformy Azure 
- przekazać je do opublikowanych Azure maszynowego uczenia usługi sieci web, 
- wyniki analizy przewidywanych i 
- przekazywanie wyników do miejsca do magazynowania. 

Aby uzyskać więcej informacji zobacz [Tworzenie procesy przewidywanych przy użyciu Factory danych Azure i nauka komputera Azure](../data-factory/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Wymagania wstępne

Ten dokument przyjęto założenie, że masz subskrypcję usługi Azure, konto miejsca do magazynowania i odpowiedni klucz miejsca do magazynowania dla tego konta. Przed przekazywania i pobierania danych, musisz znać Azure magazynowania nazwy i konta klucz konta.

- Aby skonfigurować Azure subskrypcji, zobacz [bezpłatną wersję próbną jednego miesiąca](https://azure.microsoft.com/pricing/free-trial/).
- Aby uzyskać instrukcje dotyczące tworzenia konta miejsca do magazynowania i Uzyskiwanie konta i informacje o kluczu zobacz [temat Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md).
