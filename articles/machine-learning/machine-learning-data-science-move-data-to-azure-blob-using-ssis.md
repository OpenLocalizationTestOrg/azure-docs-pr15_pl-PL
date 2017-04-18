<properties
    pageTitle="Przenoszenie danych do lub z magazynem obiektów Blob platformy Azure za pomocą łączników SSIS | Microsoft Azure"
    description="Przenoszenie danych do lub z magazynem obiektów Blob platformy Azure za pomocą łączników SSIS."
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
    ms.author="bradsev" />

# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Przenoszenie danych do lub z magazynem obiektów Blob platformy Azure za pomocą łączników SSIS

[SQL Server Integration Services Feature Pack dla Azure](https://msdn.microsoft.com/library/mt146770.aspx) znajdują się składniki nawiązać Azure, transfer danych między Azure i lokalnych źródeł danych i proces dane przechowywane w Azure.

Wytyczne dotyczące technologii umożliwia przenoszenie danych do i/lub z magazynem obiektów Blob platformy Azure są połączone poniżej:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


Gdy klienci przeniesione danych lokalnych w chmurze, ich do niego dostęp z dowolnego Azure usługi, aby korzystać ze wszystkich możliwości pakietem technologii Azure. Może on używany, na przykład Azure maszynowego uczenia lub w klastrze HDInsight.

Jest to zazwyczaj, należy najpierw dla instruktaży [SQL](machine-learning-data-science-process-sql-walkthrough.md) i [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) .

Omówienie kanonicznych scenariusze SSIS za pomocą można wykonywać typowe w scenariuszy integracji hybrydowego danych potrzeb biznesowych zobacz blog [robić więcej z SQL Server Integration Services Feature Pack dla Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) .

> [AZURE.NOTE] Pełne wprowadzenie z magazynem obiektów blob platformy Azure odwoływać się do [Podstawy obiektów Blob platformy Azure](../storage/storage-dotnet-how-to-use-blobs.md) i usługą [obiektów Blob platformy Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, musisz dysponować subskrypcji usługi Azure i skonfigurowane konto Azure miejsca do magazynowania. Musisz znać Azure magazynowania nazwy i konta klucz konta przekazywanie lub pobieranie danych.

- Aby skonfigurować **Azure subskrypcji**, zobacz [bezpłatną wersję próbną jednego miesiąca](https://azure.microsoft.com/pricing/free-trial/).

- Aby uzyskać instrukcje dotyczące tworzenia **konta miejsca do magazynowania** i Uzyskiwanie konta i informacje o kluczu zobacz [temat Azure miejsca do magazynowania konta](../storage/storage-create-storage-account.md).


Aby użyć **łączników SSIS**, musisz pobrać:

- **SQL Server 2014 lub 2016 Standard (lub nowszy)**: instalacja zawiera SQL Server Integration Services.
- **Microsoft SQL Server 2014 lub 2016 Integration Services Feature Pack dla Azure**: te można pobrać, odpowiednio ze strony [Usług SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) i [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) .

> [AZURE.NOTE] SSIS jest zainstalowany wraz z programu SQL Server, ale jest niedostępna w wersji Express. Aby uzyskać informacje na jakie aplikacje są zawarte w różnych wersjach programu SQL Server zobacz [Wersjach programu SQL Server](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)

Materiały szkoleniowe na SSIS zobacz [Wskazówki na szkolenia dotyczącego SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Aby uzyskać informacje dotyczące uzyskiwania Strzałka w górę i bieżącą przy użyciu SISS do tworzenia prostych wyodrębniania, przekształcania i ładowania (ETL) pakiety, zobacz [SSIS samouczek: Tworzenie prostego pakietu ETL](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Pobierz zestaw danych taksówki Warszawa  
Przykład opisanej w tym miejscu za pomocą publicznie zestawu danych — [Warszawa taksówki podróży](http://www.andresmh.com/nyctaxitrips/) zestawu danych. Zestaw danych składa się z około 173 milionów było taksówki w Warszawa na rok 2013. Istnieją dwa typy danych: podróży szczegółów danych i taryfy danych. Ponieważ istnieje pliku dla każdego miesiąca, mamy 24 pliki we wszystkich, z których każdy jest nieskompresowane około 2GB.


## <a name="upload-data-to-azure-blob-storage"></a>Przekazywanie danych z magazynem obiektów blob platformy Azure
Aby przenieść, że danych przy użyciu SSIS feature pack ze źródeł lokalnych z magazynem obiektów blob platformy Azure, firma Microsoft korzysta z wystąpienia [**Zadania przekazywanie obiektów Blob platformy Azure**](https://msdn.microsoft.com/library/mt146776.aspx), pokazano poniżej:

![Konfigurowanie danych — nauki-maszyn wirtualnych](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)


Parametry używane zadania opisane poniżej:


Pole|Opis|
----------------------|----------------|
**AzureStorageConnection**|Umożliwia określenie istniejącej Azure miejsca do magazynowania Connection Manager lub utworzenie nowej odwołujące się do konta usługi Azure miejsca do magazynowania, który wskazuje miejsce, w którym znajdują się pliki obiektów blob.|
**BlobContainer**|Nazwa kontenera obiektów blob, który przytrzymaj przekazane pliki jako obiektów blob.|
**BlobDirectory**|Określa miejsce, w którym jest przechowywany plik przekazane jako blob blok katalog obiektów blob. Katalog obiektów blob jest wirtualna hierarchicznej struktury. Jeśli to już istnieje, zastąpić ia it.|
**LocalDirectory**|Umożliwia określenie katalogu lokalnego, który zawiera pliki do przekazania.|
**Nazwa pliku**|Określa filtr nazw, aby wybrać pliki z wzorcem o określonej nazwie. Na przykład MySheet\*xls\* zawiera pliki, takie jak MySheet001.xls i MySheetABC.xlsx|
**TimeRangeFrom-TimeRangeTo**|Określa filtr przedziału czasu. Pliki zmodyfikowane po *TimeRangeFrom* i przed *TimeRangeTo* są uwzględniane.|


> [AZURE.NOTE] Poświadczenia **AzureStorageConnection** , muszą być prawidłowe i **BlobContainer** , musi istnieć przed przesłaniem oprogramowania jest.

## <a name="download-data-from-azure-blob-storage"></a>Pobieranie danych z magazynem obiektów blob Azure

Aby pobrać dane z magazynem obiektów blob Azure ze środowiskiem lokalnym miejsca do magazynowania z SSIS, użyj wystąpienie [Zadania przekazywanie obiektów Blob platformy Azure](https://msdn.microsoft.com/library/mt146779.aspx).

##<a name="more-advanced-ssis-azure-scenarios"></a>Bardziej zaawansowanych scenariuszy SSIS Azure
Uwaga Firma Microsoft tutaj, że pakietu SSIS feature pack umożliwia bardziej złożone przepływy obsługiwanych przez opakowania zadania ze sobą. Na przykład obiektów blob może strumieniowego źródła danych bezpośrednio w klastrze HDInsight, w których produkcja mogły zostać pobrane powrót do obiektów blob, a następnie do magazynu lokalnego. SSIS można uruchamiać zadania gałąź i świnka w klastrze HDInsight za pomocą łączników SSIS dodatkowe:

- Aby uruchomić skrypt gałęzi w klastrze Azure HDInsight z SSIS, użyj [Azure HDInsight gałęzi zadania](https://msdn.microsoft.com/library/mt146771.aspx).
- Aby uruchomić skrypt świnka w klastrze Azure HDInsight z SSIS, użyj [Azure HDInsight świnka zadania](https://msdn.microsoft.com/library/mt146781.aspx).
