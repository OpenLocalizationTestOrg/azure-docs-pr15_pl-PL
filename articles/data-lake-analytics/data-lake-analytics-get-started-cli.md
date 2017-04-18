<properties 
   pageTitle="Wprowadzenie do analiz Lake danych Azure za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure" 
   description="Dowiedz się, jak utworzyć konto magazynu Lake danych, utworzyć zadanie analizy Lake danych za pomocą U-SQL za pomocą interfejsu wiersza polecenia Azure i przesłać zadanie. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Samouczek: wprowadzenie do analiz Lake danych Azure za pomocą interfejsu wiersza polecenia Azure (polecenie)

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Dowiedz się, jak utworzyć konta Azure danych Lake analizy, definiowanie zadań analizy Lake danych w [Języku SQL U](data-lake-analytics-u-sql-get-started.md)i przesyłać zadania z kontami analizy Lake danych za pomocą polecenie Azure. Aby uzyskać więcej informacji na temat analizy Lake danych zobacz [Omówienie analizy Lake danych Azure](data-lake-analytics-overview.md).

W tym samouczku będzie rozwijanie zadanie, który odczytuje karty pliku wartości (TSV) i konwertuje ją do pliku (CSV wartości) przecinkami. Do wykonywania kroków samej samouczka przy użyciu innych obsługiwanych narzędzi, kliknij karty u góry tej sekcji.

##<a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/).
- **Polecenie azure**. Zobacz [zainstalować i skonfigurować polecenie Azure](../xplat-cli-install.md).
    - Pobieranie i instalowanie **wersji wstępnej** [Azure interfejsu wiersza polecenia narzędzia](https://github.com/MicrosoftBigData/AzureDataLake/releases) w celu wykonania tego pokazu.
- **Uwierzytelnianie**przy użyciu następującego polecenia:

        azure login
    Aby uzyskać więcej informacji o uwierzytelnianiu za pomocą konta służbowego zobacz [Łączenie się Azure subskrypcję polecenie Azure](../xplat-cli-connect.md).
- **Przełącz do trybu Menedżer zasobów Azure**, przy użyciu następującego polecenia:

        azure config mode arm
        
## <a name="create-data-lake-analytics-account"></a>Tworzenie konta analizy Lake danych

Przed uruchomieniem zadania, musisz mieć konto analizy Lake danych. Aby utworzyć konto analizy Lake danych, określ następujące czynności:

- **Grupa zasobów Azure**: musi zostać utworzone konto analizy Lake dane w grupie Azure zasobów. [Menedżer zasobów Azure](../azure-resource-manager/resource-group-overview.md) umożliwia pracę z zasobami w aplikacji jako grupy. Możesz wdrożyć, aktualizowanie lub usuwanie wszystkich zasobów dla aplikacji w jednym, skoordynowanego operacji.  

    Wyliczyć grup zasobów w ramach subskrypcji:
    
        azure group list 
    
    Aby utworzyć nową grupę zasobów:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Nazwa konta analizy Lake danych**
- **Lokalizacja**: jeden z centrami danych Azure, obsługiwanych analizy Lake danych.
- **Konto domyślne dane Lake**: każde konto analizy Lake danych ma domyślne konto Lake danych.

    Aby wyświetlić listę istniejącego konta Lake danych:
    
        azure datalake store account list

    Aby utworzyć nowe konto Lake danych:

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] Nazwa konta Lake danych musi zawierać tylko małe litery i cyfry.



**Aby utworzyć konto analizy Lake danych**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Wyświetlanie analizy Lake danych konta](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] Nazwę konta analizy Lake danych musi zawierać tylko małe litery i cyfry.


## <a name="upload-data-to-data-lake-store"></a>Przekazywanie danych do magazynu Lake danych

W tym samouczku będzie przetwarzał niektóre dzienniki wyszukiwania.  Dziennik wyszukiwania mogą być przechowywane w magazynie Lake danych lub magazyn obiektów Blob platformy Azure. 

Azure Portal przewiduje kopiowania kilka przykładowych plików danych do domyślnego konta Lake danych, które dołączyć plik dziennika wyszukiwania w interfejsie użytkownika. Zobacz [Przygotowywanie źródła danych](data-lake-analytics-get-started-portal.md#prepare-source-data) do przekazania danych do domyślnego konta magazynu Lake danych.

Aby przekazać pliki przy użyciu interfejsu wiersza polecenia, użyj następującego polecenia:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Analizy Lake danych może też uzyskiwać dostęp Magazyn obiektów Blob platformy Azure.  Dla przekazywania danych z magazynem obiektów Blob platformy Azure, zobacz [za pomocą interfejsu wiersza polecenia Azure z nośnikami Azure](../storage/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Przesyłanie danych Lake analizy zadań

Zadania analizy Lake danych są zapisywane w języku U SQL. Aby dowiedzieć się więcej na temat U SQL, zobacz [Wprowadzenie do języka U SQL](data-lake-analytics-u-sql-get-started.md) i [Dokumentacja języka U SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Aby utworzyć skrypt zadanie analizy Lake danych**

- Tworzenie pliku tekstowego z poniższy skrypt U SQL i Zapisz plik tekstowy na miejscu pracy:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Ten skrypt U SQL odczytuje plik źródła danych przy użyciu **Extractors.Tsv()**, a następnie utworzy plik csv przy użyciu **Outputters.Csv()**. 
    
    Nie Modyfikuj dwie ścieżki, chyba że skopiuj plik źródłowy do innej lokalizacji.  Jeśli nie istnieje, analizy Lake danych utworzy folder wyjściowy.
    
    Jest prostsze Użyj ścieżki względne do plików przechowywanych w domyślnych danych Lake konta. Można także używać ścieżek bezwzględnych.  Na przykład 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Aby uzyskać dostęp do plików w połączonych kont miejsca do magazynowania, należy użyć ścieżki bezwzględne.  Składnia plików przechowywanych w połączony klient magazyn Azure jest następująca:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure kontenera obiektów Blob z uprawnieniami dostępu publicznej kontenerów lub publicznej blob nie są obecnie obsługiwane.      

    
**Aby przesłać zadanie**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
Następujące polecenia może służyć do listę zadań, wyświetlanie szczegółów zadania i anulować zadania:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

Po zakończeniu zadania można go za pomocą następujące polecenia cmdlet i pobrać plik:
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## <a name="see-also"></a>Zobacz też

- Aby wyświetlić samej samouczka przy użyciu innych narzędzi, kliknij selektory kartę w górnej części strony.
- Aby uzyskać bardziej złożonych kwerend, zobacz [Dzienniki analiza witryny sieci Web przy użyciu analizy Lake danych Azure](data-lake-analytics-analyze-weblogs.md).
- Aby rozpocząć tworzenie aplikacji U SQL, zobacz [skryptów można opracowywać U-SQL przy użyciu narzędzia Lake danych dla programu Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Aby dowiedzieć się U SQL, zobacz [Wprowadzenie do języka Azure danych Lake analizy U-SQL](data-lake-analytics-u-sql-get-started.md).
- Do zadań zarządzania zobacz [Zarządzanie Azure danych Lake analiz za pomocą Azure Portal](data-lake-analytics-manage-use-portal.md).
- Aby uzyskać omówienie analizy Lake danych, zobacz [Omówienie analizy Lake danych Azure](data-lake-analytics-overview.md).

