<properties
    pageTitle="Samouczek Factory danych: pierwszego planowana danych | Microsoft Azure"
    description="Ten samouczek Factory danych Azure pokazano, jak tworzyć i planowanie factory danych, który przetwarza dane za pomocą skryptu gałęzi w klastrze Hadoop."
    services="data-factory"
    keywords="Azure danych factory samouczek, hadoop klaster, gałąź hadoop"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-pipeline-to-process-data-using-hadoop-cluster"></a>Samouczek: Tworzenie pierwszej planowaną przetworzyć danych przy użyciu klaster Hadoop 
> [AZURE.SELECTOR]
- [Omówienie i wymagania wstępne](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Programu Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Programu PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Szablon Menedżera zasobów](data-factory-build-your-first-pipeline-using-arm.md)
- [INTERFEJSU API USŁUGI REST](data-factory-build-your-first-pipeline-using-rest-api.md)

W tym samouczku konstruujesz firmie pierwszego Azure danych z potoku danych, który przetwarza dane, uruchamiając skrypt gałęzi w klastrze Azure HDInsight (Hadoop). 

> [AZURE.NOTE] Ten artykuł nie zawiera omówienie fabryki danych Azure. Aby uzyskać omówienie usługi zobacz [Wprowadzenie do Azure danych Factory](data-factory-introduction.md). Zobacz [ścieżka nauki Factory danych](https://azure.microsoft.com/documentation/learning-paths/data-factory/) dotyczących zalecany sposób przechodzić między Nasza zawartość Aby uzyskać informacje o Factory danych.

## <a name="whats-covered-in-this-tutorial"></a>Co to jest objęta w tym samouczku? 
**Factory danych Azure** Umożliwia redagowanie danych **ruchu** i zadania **przetwarzania** danych jako sterowanych danymi przepływy pracy (zwanych również procesy danych). Jak utworzyć pierwszą aktywności procesu przetwarzania danych (lub Przekształcanie) danych. To działanie używa klastrze HDInsight Hadoop Przekształcanie i analizować próbki dzienniki sieci web.  

W tym samouczku należy wykonać następujące czynności:

1.  Tworzenie **factory danych**. Factory danych może zawierać jedną lub więcej danych rurociągi tego przenoszenie i przetwarzanie danych. 
2.  Tworzenie **połączonych usług**. Można tworzyć połączone usługi, aby połączyć magazynu danych lub usługą obliczeń do fabryki danych. Magazyn danych, takich jak magazyn Azure przechowuje dane wejścia i wyjścia działań w potoku. Do uruchamiania usługi takiej jak klaster HDInsight Hadoop procesów i przekształceń poprawiona danych.    
3.  Tworzenie dane wejściowe i wyjściowe **zestawy danych**. Zestaw danych wejściowych reprezentuje dane wejściowe do działania w potoku a zestaw danych wyjściowych wynik działania.
3.  Tworzenie **procesu**. Potok może zawierać jedną lub więcej czynności (przykłady: aktywność kopiowania, działania gałęzi HDInsight). W tym przykładzie użyto działanie gałąź HDInsight, do którego jest uruchamiany skrypt gałęzi w klastrze HDInsight Hadoop. Skrypt najpierw tworzy tabelę, która odwołuje się do danych dziennika nieprzetworzonych sieci web, które są przechowywane w magazynie obiektów blob platformy Azure, a następnie partycje dane według roku i miesiąca.

    Istnieją dwa typy działań obsługiwanych przez Azure danych Factory. Są one: [działania przepływu danych](data-factory-data-movement-activities.md) i [działania przekształcania danych](data-factory-data-transformation-activities.md). Istnieje tylko jeden danych działania przepływu to działanie Kopiuj. W tym samouczku nie należy używać aktywności Kopiuj. Samouczek dotyczący używania działania Kopiuj, zobacz [Samouczek: kopiowanie danych z platformy Azure blob SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Działanie gałąź HDInsight, używanych w ramach tego samouczka jest działalność przekształcania danych obsługiwanych przez Factory danych.  
 
Oto **Widok diagramu** fabryki dane przykładowe, tworzonych w tym samouczku. 

![Widok diagramu w samouczku Factory danych](./media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)

W tym samouczku folder **inputdata** kontenera obiektów blob platformy Azure **adfgetstarted** zawiera jeden plik o nazwie input.log. Ten plik dziennika ma wpisy z trzech miesięcy: styczniu, lutym i marcu 2016. Poniżej wymieniono wierszy próbki dla każdego miesiąca w pliku wejściowego. 

    2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Gdy plik jest przetwarzana przez potok aktywnością HDInsight gałęzi, działania uruchomi skrypt gałęzi w klastrze HDInsight że partycje wprowadzania danych według roku i miesiąca. Skrypt tworzy trzy foldery dane wyjściowe, które zawierają plik z wpisy z każdego miesiąca.  

    adfgetstarted/partitioneddata/year=2016/month=1/000000_0
    adfgetstarted/partitioneddata/year=2016/month=2/000000_0
    adfgetstarted/partitioneddata/year=2016/month=3/000000_0

Z linii próbki, jak pokazano powyżej, pierwszego (z 2016-01-01) jest zapisywany do pliku 000000_0 w miesiącu = 1 folder. Podobnie, drugi jest zapisywany do pliku w miesiącu = 2 folderu, a trzecia jedną są zapisywane w pliku w miesiącu = 3 folder.  


## <a name="pre-requisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka, musi mieć następujące wymagania:

1.  **Subskrypcja azure** — Jeśli nie masz subskrypcji usługi Azure, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Zobacz artykuł [Bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/) , w jaki sposób mogą uzyskać bezpłatne konto wersji próbnej.

2.  **Magazyn azure** — do przechowywania danych w tym samouczku za pomocą konta usługi Azure miejsca do magazynowania. Jeśli nie masz konta usługi Azure miejsca do magazynowania, zobacz artykuł [Tworzenie konta miejsca do magazynowania](../storage/storage-create-storage-account.md#create-a-storage-account) . Po utworzeniu konta miejsca do magazynowania, zanotuj **nazwę konta** i **klawisz dostępu**. Zobacz [Wyświetlanie, Kopiuj i klawisze dostępu wyniku miejsca do magazynowania](../storage/storage-create-storage-account.md#view-and-copy-storage-access-keys). 

### <a name="upload-files-to-azure-storage-for-the-tutorial"></a>Przekazywanie plików do magazynu Azure samouczka
Przed rozpoczęciem przerabiania samouczka należy przygotować konta magazynu platformy Azure z przykładowe pliki samouczka.

1. Przekaż plik kwerendy gałęzi (HQL) do folderu **skrypt** kontenera blob **adfgetstarted** .
2. Przekaż plik wprowadzania danych do folderu **inputdata** kontenera obiektów blob **adfgetstarted** . 

#### <a name="create-hql-script-file"></a>Utwórz plik skryptu HQL 

1. Uruchom **Notatnik** i wklej następujący skrypt HQL. Ten skrypt gałęzi tworzy dwóch tabel: **WebLogsRaw** i **WebLogsPartitioned**. W menu kliknij pozycję **plik** , a następnie wybierz polecenie **Zapisz jako**. Przejdź do folderu **C:\adfgetstarted** na dysku twardym. Wybierz pozycję * *wszystkie pliki (*.*) **dla** Zapisz jako wpisz** pola. Wprowadź **partitionweblogs.hql** dla **nazwy pliku**. Upewnij się, że **kodowanie w** polu u dołu okna dialogowego jest ustawiona wartość **ANSI**. Jeśli nie, ustaw **ANSI **.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

W czasie wykonywania gałęzi aktywności w potoku Factory danych przekazuje wartości **inputtable** i parametrów **partitionedtable** , jak pokazano w poniższej wstawki kodu:  

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

**Storageaccountname** to nazwa Twojego konta Azure miejsca do magazynowania.
 
#### <a name="create-a-sample-input-file"></a>Tworzenie przykładowego pliku wprowadzania danych
Za pomocą Notatnika, Utwórz plik o nazwie **input.log** w **c:\adfgetstarted** o następującej treści: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

#### <a name="upload-input-file-and-hql-file-to-your-azure-blob-storage"></a>Przekaż plik wprowadzania i plik HQL z magazynem obiektów Blob platformy Azure

Ta sekcja zawiera instrukcje dotyczące narzędzia **AzCopy** , aby skopiować pliki input.log i partitionweblogs.hql magazynem obiektów Blob platformy Azure. Można użyć dowolnego narzędzia wyboru (na przykład: [Eksplorator magazynu usługi Microsoft Azure](http://storageexplorer.com/), [CloudXPlorer przez oprogramowanie ClumsyLeaf](http://clumsyleaf.com/products/cloudxplorer) , aby wykonać to zadanie.   
     
1. Pobierz [najnowszą wersję programu **AzCopy**](http://aka.ms/downloadazcopy)lub [najnowszej wersji preview](http://aka.ms/downloadazcopypr). Zobacz [jak za pomocą AzCopy](../storage/storage-use-azcopy.md) artykuł, aby uzyskać instrukcje dotyczące korzystania z narzędzia.
2. Przejdź do folderu c:\adfgetstarted, a następnie uruchom następujące polecenie: 

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy" /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storageaccesskey>  /Pattern:input.log

    To polecenie wysyła plik **input.log** na koncie miejsca do magazynowania (kontener**adfgetstarted** i **inputdata** folder). Zamień **storageaccountname** z nazwą swojego konta miejsca do magazynowania i **storageaccesskey** klawisz dostępu miejsca do magazynowania.

    > [AZURE.NOTE] To polecenie tworzy kontenera o nazwie **adfgetstarted** w magazynie obiektów Blob platformy Azure i kopiuje plik **input.log** z dysku lokalnego w folderze **inputdata** w kontenerze. 
    
3. Po pomyślnym przekazaniu pliku pojawi się dane wyjściowe podobne do następujących jeden z AzCopy.
    
        Finished 1 of total 1 file(s).
        [2015/12/16 23:07:33] Transfer summary:
        -----------------
        Total files transferred: 1
        Transfer successfully:   1
        Transfer skipped:        0
        Transfer failed:         0
        Elapsed time:            00.00:00:01
5. Uruchom następujące polecenie, aby przekazać plik **partitionweblogs.hql** do folderu **skryptów** kontenera **adfgetstarted** . Oto polecenia: 
    
        AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storageaccesskey>  /Pattern:partitionweblogs.hql

Ukończono wymagania wstępne. Możesz utworzyć fabryki danych przy użyciu jednej z następujących metod. Kliknij jedną z kart u góry lub poniższe łącza do wykonywania samouczka. 

- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Programu Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Programu PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Szablon Menedżera zasobów](data-factory-build-your-first-pipeline-using-arm.md)
- [INTERFEJSU API USŁUGI REST](data-factory-build-your-first-pipeline-using-rest-api.md)