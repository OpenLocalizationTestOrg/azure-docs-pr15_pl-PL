<properties
    pageTitle="Generowanie zalecenia dotyczące korzystania z usługi HDInsight systemu WIndows i Mahout | Microsoft Azure"
    description="Dowiedz się, jak za pomocą komputera Apache Mahout nauki biblioteki Generuj recommendations filmu z usługi HDInsight systemu Windows (Hadoop)."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-hadoop-in-hdinsight"></a>Generuj recommendations filmu przy użyciu Apache Mahout Hadoop w HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Dowiedz się, jak za pomocą komputera [Apache Mahout](http://mahout.apache.org) nauki bibliotece przy użyciu Azure HDInsight Generuj recommendations filmu.

> [AZURE.NOTE] Kroki opisane w tym dokumencie wymagają klienta systemu Windows i klaster HDInsight systemu Windows. Uzyskać informacji na temat korzystania z Mahout z Linux, OS X lub Unix client z klastrem systemem Linux HDInsight zobacz [Generuj recommendations filmu przy użyciu Mahout Apache z systemem Linux Hadoop w HDInsight](hdinsight-hadoop-mahout-linux-mac.md)


##<a name="learn"></a>Będzie więcej

Mahout jest [maszynowego uczenia] [ ml] biblioteki Apache Hadoop. Mahout zawiera algorytmów przetwarzania danych, takich jak filtrowanie klasyfikacji, a klaster. W tym artykule zostanie użyta aparat rekomendacji do wygenerowania zalecenia dotyczące filmu, które są oparte na filmy, które miały znajomych. Również dowiesz się, jak wykonywać klasyfikacje z las decyzji. To udzieli Ci następujące czynności:

* Jak uruchomić Mahout zadań przy użyciu programu Windows PowerShell

* Jak uruchomić Mahout zadań z poziomu wiersza polecenia Hadoop

* Jak zainstalować Mahout na klastrów HDInsight 3.0 i HDInsight 2.0

    > [AZURE.NOTE] Mahout jest dostarczany z wersji HDInsight 3.1 klastrów. Jeśli używasz wcześniejszej wersji programu HDInsight, zobacz [Instalowanie Mahout](#install) przed Kontynuuj.

##<a name="prerequisites"></a>wymagania wstępne

- **Klaster systemu Windows Hadoop w HDInsight**. Aby dowiedzieć się, jak utworzyć jedną zobacz [wprowadzenie] do przy użyciu Hadoop w HDInsight[getstarted]
- **Pracy z programem PowerShell Azure**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a name="recommendations"></a>Generuj recommendations przy użyciu programu Windows PowerShell

> [AZURE.NOTE] Mimo że zadaniu używane w tej sekcji działa przy użyciu programu Windows PowerShell, wiele klas do dyspozycji Mahout obecnie nie współpracuje z programu Windows PowerShell, a musi być uruchamiane przy użyciu wiersza polecenia Hadoop. Aby uzyskać listę klas, które nie są obsługiwane przy użyciu programu Windows PowerShell zobacz sekcję [Rozwiązywanie problemów](#troubleshooting) .
>
> Na przykład Mahout zadań przy użyciu wiersza polecenia Hadoop zobacz [Klasyfikacja danych przy użyciu wiersza polecenia Hadoop](#classify).

Jedna z funkcji, które są udostępniane przez Mahout jest aparatem rekomendacji. Ten aparat wprowadzania danych w formacie `userID`, `itemId`, i `prefValue` (preferencji użytkowników dla elementu). Mahout można wykonywać analizy Współtworzenie wystąpienia w celu oznaczenia: _użytkowników, którzy mają preferencji dla elementu mieć także preferencji elementy skoroszytu_. Mahout określa użytkownikom preferencje podobnych elementów, które mogą być używane zaleceń.

Oto przykład bardzo proste, z zastosowaniem filmy:

* __Współtworzenie wystąpienia__: Joe, Alicja i Robert polubiły _słów_, _Ponownie ataki Empire_i _powrotu Jedi_. Mahout Określa, że użytkownicy, którzy również, takich jak dowolną spośród następujących filmy, takich jak pozostałe dwa.

* __Współtworzenie wystąpienia__: Roman i Alicja również łączone _Zagrożenie fantom_, _atakami klonów_i _zemsty Sith_. Mahout Określa, że użytkownicy, którzy również łączone poprzedniego filmy trzy, takich jak te trzy.

* __Podobieństwa zalecenie__: ponieważ Joe polubiły pierwsze trzy filmy, Mahout analizuje filmy tego innym użytkownikom podobne preferencje łączone, ale Joe nie ma obserwowane (polubiły klasyfikowane). W tym przypadku Mahout zaleca _Zagrożenie fantom_, _atakami klonów_i _zemsty Sith_.

###<a name="understanding-the-data"></a>Opis danych

Wygodne, [Poszukaj GroupLens] [ movielens] zawiera dane klasyfikacji w przypadku filmów w formacie, który jest zgodny z Mahout. Te dane są dostępne na w klastrze domyślne miejsce do magazynowania przy `/HdiSamples/MahoutMovieData`.

Istnieją dwa pliki `moviedb.txt` (informacje na temat filmów,) i `user-ratings.txt`. Podczas analizy, zostanie użyty plik ratings.txt użytkownika, gdy moviedb.txt służą do zapewnienia informacji o tekst zrozumiałe dla użytkownika, wyświetlając wyniki analizy.

Danych znajdujących się w ratings.txt użytkownik ma strukturę z `userID`, `movieID`, `userRating`, i `timestamp`, który wskazuje nam jak wysoce każdego użytkownika ocenianie filmu. Oto przykład danych:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

###<a name="run-the-job"></a>Uruchamianie zadania

Użyj tego skryptu programu Windows PowerShell, aby uruchomić zadanie, które używa aparatu rekomendacji Mahout danymi film:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get HTTPS/Admin credentials for submitting the job later
    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
            
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
    # NOTE: The version number in the file path
    # may change in future versions of HDInsight.
    $jarFile =  "file:///C:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar"
    #
    # If you are using an earlier version of HDInsight,
    # set $jarFile to the jar file you
    # uploaded.
    # For example,
    # $jarFile = "wasbs:///example/jars/mahout-core-0.9-job.jar"

    # The arguments for this job
    # * input - the path to the data uploaded to HDInsight
    # * output - the path to store output data
    # * tempDir - the directory for temp files
    $jobArguments = "--similarityClassname", "recommenditembased", `
                    "-s", "SIMILARITY_COOCCURRENCE", `
                    "--input", "wasbs:///HdiSamples/MahoutMovieData/user-ratings.txt",
                    "--output", "wasbs:///example/out",
                    "--tempDir", "wasbs:///example/temp"

    # Create the job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
      -JarFile $jarFile `
      -ClassName "org.apache.mahout.cf.taste.hadoop.item.RecommenderJob" `
      -Arguments $jobArguments

    # Start the job
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds

    # Wait on the job to complete
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
            -Blob example/out/part-r-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
            
    # Write out any error information
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

> [AZURE.NOTE] Zadania mahout zrobić Usuń dane utworzony podczas przetwarzania zadania. `--tempDir` Parametr jest określony w zadaniu przykład w celu wyodrębnienia tymczasowych plików w określonym katalogu.

Zadanie Mahout nie zwraca wynik stdout. Zamiast tego go jest on przechowywany w katalogu wyjściowego określonej jako __część r-00000__. Skrypt pobiera tego pliku __output.txt__ w bieżącym katalogu w miejscu pracy.

Oto przykład zawartość tego pliku:

    1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
    2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
    3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
    4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

Pierwsza kolumna zawiera `userID`. Wartości zawarte w "[" i "]" są `movieId`:`recommendationScore`.

###<a name="view-the-output"></a>Wyświetlanie danych wyjściowych

Chociaż wygenerowane dane wyjściowe mogą być OK do użycia w aplikacji, nie jest bardzo czytelne. `moviedb.txt` Na serwerze można rozwiązać `movieId` do filmu nazwę, ale musisz najpierw pobrać go i plik ocen za pomocą skryptu następujące usługi:

    # The HDInsight cluster name.
    $clusterName = "the cluster name"
    
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
    #Download the files
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/moviedb.txt" `
    -Container $container `
    -Destination moviedb.txt `
    -Context $context
    Get-AzureStorageBlobContent -blob "HdiSamples/MahoutMovieData/user-ratings.txt" `
    -Container $container `
    -Destination user-ratings.txt `
    -Context $context

Po pobraniu plików umożliwia wyświetlanie zalecenia nazwami filmu tego skryptu programu PowerShell:

    <#
    .SYNOPSIS
        Displays recommendations for movies.
    .DESCRIPTION
        Displays recommendations generated by Mahout
        with HDInsight example in a human readable format.
    .EXAMPLE
        .\Show-Recommendation -userId 4
            -userDataFile "user-ratings.txt"
            -movieFile "moviedb.txt"
            -recommendationFile "output.txt"
    #>

    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
        #The user ID
        [Parameter(Mandatory = $true)]
        [String]$userId,

        [Parameter(Mandatory = $true)]
        [String]$userDataFile,

        [Parameter(Mandatory = $true)]
        [String]$movieFile,

        [Parameter(Mandatory = $true)]
        [String]$recommendationFile
    )
    # Read movie ID & description into hash table
    Write-Host "Reading movies descriptions" -ForegroundColor Green
    $movieById = @{}
    foreach($line in Get-Content $movieFile)
    {
        $tokens = $line.Split("|")
        $movieById[$tokens[0]] = $tokens[1]
    }
    # Load movies user has already seen (rated)
    # into a hash table
    Write-Host "Reading rated movies" -ForegroundColor Green
    $ratedMovieIds = @{}
    foreach($line in Get-Content $userDataFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            # Resolve the ID to the movie name
            $ratedMovieIds[$movieById[$tokens[1]]] = $tokens[2]
        }
    }
    # Read recommendations generated by Mahout
    Write-Host "Reading recommendations" -ForegroundColor Green
    $recommendations = @{}
    foreach($line in get-content $recommendationFile)
    {
        $tokens = $line.Split("`t")
        if($tokens[0] -eq $userId)
        {
            #Trim leading/treailing [] and split at ,
            $movieIdAndScores = $tokens[1].TrimStart("[").TrimEnd("]").Split(",")
            foreach($movieIdAndScore in $movieIdAndScores)
            {
                #Split at : and store title and score in a hash table
                $idAndScore = $movieIdAndScore.Split(":")
                $recommendations[$movieById[$idAndScore[0]]] = $idAndScore[1]
            }
            break
        }
    }

    Write-Host "Rated movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $ratedFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                   @{Expression={$_.Value};Label="Rating"}
    $ratedMovieIds | format-table $ratedFormat
    Write-Host "---------------------------" -ForegroundColor Green

    write-host "Recommended movies" -ForegroundColor Green
    Write-Host "---------------------------" -ForegroundColor Green
    $recommendationFormat = @{Expression={$_.Name};Label="Movie";Width=40}, `
                            @{Expression={$_.Value};Label="Score"}
    $recommendations | format-table $recommendationFormat

Oto przykład uruchamianie skryptu:

    PS C:\> show-recommendation.ps1 -userId 4 -userDataFile .\user-ratings.txt -movieFile .\moviedb.txt -recommendationFile .\output.txt

Wynik powinna wyglądać podobnie do następującej:

    Reading movies descriptions
    Reading rated movies
    Reading recommendations
    Rated movies
    ---------------------------
    Movie                                    Rating
    -----                                    ------
    Devil's Own, The (1997)                  1
    Alien: Resurrection (1997)               3
    187 (1997)                               2
    (lines ommitted)

    ---------------------------
    Recommended movies
    ---------------------------

    Movie                                    Score
    -----                                    -----
    Good Will Hunting (1997)                 4.6504064
    Swingers (1996)                          4.6862745
    Wings of the Dove, The (1997)            4.6666665
    People vs. Larry Flynt, The (1996)       4.834559
    Everyone Says I Love You (1996)          4.707071
    Secrets & Lies (1996)                    4.818182
    That Thing You Do! (1996)                4.75
    Grosse Pointe Blank (1997)               4.8235292
    Donnie Brasco (1997)                     4.6792455
    Lone Star (1996)                         4.7099237  

##<a name="classify"></a>Klasyfikowanie danych przy użyciu wiersza polecenia Hadoop

Jedną z metod klasyfikacji dostępne w przypadku Mahout jest utworzenie [losowe las][forest]. To jest wiele proces krok, który obejmuje przy użyciu danych szkoleniowych, aby wygenerować drzewa decyzji, które następnie są używane do klasyfikowania danych. Ta opcja stosuje klasy __org.apache.mahout.classifier.df.tools.Describe__ dostarczony przez Mahout. Obecnie należy uruchomić przy użyciu wiersza polecenia Hadoop.

###<a name="load-the-data"></a>Ładowanie danych

1. Pobierz następujące pliki ze [zbioru danych i NSL KDD](http://nsl.cs.unb.ca/NSL-KDD/).

  * [KDDTrain +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTrain+.arff): plik szkolenia

  * [KDDTest +. ARFF](http://nsl.cs.unb.ca/NSL-KDD/KDDTest+.arff): testowymi danymi

2. Otwórz każdy z nich i usunąć wiersze u góry, które zaczynają się od '@', , a następnie zapisz pliki. Jeśli te nie zostaną usunięte, otrzymasz komunikat o błędzie podczas korzystania z tych danych z Mahout.

2. Przekaż pliki do __przykład/danych__. Możesz to zrobić przy użyciu tego skryptu. Zamień __NAZWAKLASTRA__ nazwę klaster HDInsight. Zastąp FILENAME źródeł plik do przekazania.

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName="CLUSTERNAME"
        $fileToUpload="FILENAME"
        $blobPath="example/data/FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context

###<a name="run-the-job"></a>Uruchamianie zadania

1. To zadanie wymaga wiersza polecenia Hadoop. Włączanie pulpitu zdalnego dla klastrów HDInsight, a następnie postępując zgodnie z instrukcjami na [Nawiązywanie połączenia przy użyciu RDP klastrów HDInsight](hdinsight-administer-use-management-portal.md#rdp)się z nim połączyć.

3. Po nawiązaniu połączenia użyj ikony __wiersza polecenia Hadoop__ , aby otworzyć z poziomu wiersza polecenia Hadoop:

    ![polecenie hadoop][hadoopcli]

3. Użyj następującego polecenia, aby wygenerować deskryptora plików (__KDDTrain + .info__), który używa Mahout.

        hadoop jar "c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar" org.apache.mahout.classifier.df.tools.Describe -p "wasbs:///example/data/KDDTrain+.arff" -f "wasbs:///example/data/KDDTrain+.info" -d N 3 C 2 N C 4 N C 8 N 2 C 19 N L

    `N 3 C 2 N C 4 N C 8 N 2 C 19 N L` Opisuje atrybuty danych w pliku. Na przykład L wskazuje etykietę.

4. Tworzenie las algorytmów przy użyciu następującego polecenia:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.BuildForest -Dmapred.max.split.size=1874231 -d wasbs:///example/data/KDDTrain+.arff -ds wasbs:///example/data/KDDTrain+.info -sl 5 -p -t 100 -o nsl-forest

    Wynik tej operacji są przechowywane w katalogu __las nsl__ , który znajduje się w magazynie dla klaster HDInsight u __ wasbs://user/&lt;nazwa_użytkownika > / nsl las nsl-forest.seq. &lt;Nazwa_użytkownika > jest nazwą użytkownika, którego użyto do sesji pulpitu zdalnego. Ten plik nie jest możliwe do odczytania przez człowieka.

5. Przetestuj las klasyfikacji __KDDTest + .arff__ zestawu danych. Użyj następującego polecenia:

        hadoop jar c:/apps/dist/mahout-0.9.0.2.2.9.1-8/examples/target/mahout-examples-0.9.0.2.2.9.1-8-job.jar org.apache.mahout.classifier.df.mapreduce.TestForest -i wasbs:///example/data/KDDTest+.arff -ds wasbs:///example/data/KDDTrain+.info -m nsl-forest -a -mr -o wasbs:///example/data/predictions

    To polecenie zwraca informacje podsumowujące dotyczące procesu klasyfikacji podobny do następującego:

        14/07/02 14:29:28 INFO mapreduce.TestForest:

        =======================================================
        Summary
        -------------------------------------------------------
        Correctly Classified Instances          :      17560       77.8921%
        Incorrectly Classified Instances        :       4984       22.1079%
        Total Classified Instances              :      22544

        =======================================================
        Confusion Matrix
        -------------------------------------------------------
        a       b       <--Classified as
        9437    274      |  9711        a     = normal
        4710    8123     |  12833       b     = anomaly

        =======================================================
        Statistics
        -------------------------------------------------------
        Kappa                                       0.5728
        Accuracy                                   77.8921%
        Reliability                                53.4921%
        Reliability (standard deviation)            0.4933

  To zadanie powoduje również pliku znajdującego się na __wasbs:///example/data/predictions/KDDTest+.arff.out__. Ten plik jest czytelny przez człowieka.

> [AZURE.NOTE] Zadania mahout nie zastępować plików. Jeśli chcesz ponownie uruchomić te zadania, należy usunąć pliki, które zostały utworzone przez poprzedniego zadania.

##<a name="troubleshooting"></a>Rozwiązywanie problemów

###<a name="install"></a>Instalowanie Mahout

Mahout jest zainstalowana na klastrów HDInsight 3.1 i zainstalowaniem ręcznie na klastrów HDInsight 3.0 lub HDInsight 2.1, wykonując następujące czynności:

1. Wersja Mahout używać zależy od wersji usługi HDInsight klaster. Wersja klaster można znaleźć, wyświetlając właściwości klaster w portalu Azure.

  * __Dla 2.1 HDInsight__, możesz pobrać plik archiwum Java (SŁOIK), który zawiera [Mahout 0,9](http://repo2.maven.org/maven2/org/apache/mahout/mahout-core/0.9/mahout-core-0.9-job.jar).

  * __Dla 3.0 HDInsight__, należy [utworzyć Mahout ze źródła] [ build] i określ Hadoop dostarczony przez HDInsight. Zainstaluj wstępnie wymienione na stronie Tworzenie, pobrać źródło, a następnie użyj następującego polecenia do tworzenia plików jar Mahout:

            mvn -Dhadoop2.version=2.2.0 -DskipTests clean package

        After the build completes, you can find the JAR file at __mahout\mrlegacy\target\mahout-mrlegacy-1.0-SNAPSHOT-job.jar__.

        > [AZURE.NOTE] Po zwolnieniu Mahout 1.0, należy skorzystać z usługi HDInsight 3.0 gotowych pakietów.

2. Przekaż plik słoik, na __przykład słoików__ w magazynie domyślne dla klaster. Zamień NAZWAKLASTRA w następujący skrypt nazwę klaster HDInsight i zastąpić FILENAME ścieżkę do pliku __mahout-coure 0,9 job.jar__ .

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterName = "CLUSTERNAME"
        $fileToUpload = "FILENAME"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob "example/jars/mahout-core-0.9-job.jar" `
            -Container $container `
            -Context $context

###<a name="cannot-overwrite-files"></a>Nie można zastąpić plików

Mahout zadania nie Oczyszczanie tymczasowych plików, które zostały utworzone podczas przetwarzania. Ponadto zadania nie powoduje zastąpienie istniejącego pliku danych wyjściowych.

Aby uniknąć błędów podczas wykonywania zadań Mahout, usuwanie plików tymczasowych i wyjściowe między zostanie uruchomiona lub użyć nazwy unikatowy katalog tymczasowy i wyjściowe.

###<a name="cannot-find-the-jar-file"></a>Nie można odnaleźć pliku SŁOIK

Usługa HDInsight 3.1 klastrów obejmuje Mahout. Ścieżkę i nazwę pliku zawiera numer wersji Mahout zainstalowanej w klastrze. Skrypt programu Windows PowerShell w tym samouczku używa ścieżkę, która jest ważna od 2015 listopada, ale numer wersji zmieni się w przyszłości aktualizacje z usługą HDInsight. Aby określić bieżącą ścieżkę do pliku Mahout JAR klaster, użyj następującego polecenia środowiska Windows PowerShell, a następnie zmodyfikuj skrypt umożliwiający odwołać ścieżka do pliku, który został zwrócony:

    Use-AzureRmHDInsightCluster -ClusterName $clusterName
    #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "wasbs:///example/statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query '!${env:COMSPEC} /c dir /b /s ${env:MAHOUT_HOME}\examples\target\*-job.jar'

###<a name="nopowershell"></a>Klasy, które nie są obsługiwane przy użyciu programu Windows PowerShell

Zadania mahout, które korzystają z następujących klas zwracają różne komunikaty o błędach, jeśli są one używane z programu Windows PowerShell:

* org.apache.mahout.utils.clustering.ClusterDumper
* org.apache.mahout.utils.SequenceFileDumper
* org.apache.mahout.utils.vectors.lucene.Driver
* org.apache.mahout.utils.vectors.arff.Driver
* org.apache.mahout.text.WikipediaToSequenceFile
* org.apache.mahout.clustering.streaming.tools.ResplitSequenceFiles
* org.apache.mahout.clustering.streaming.tools.ClusterQualitySummarizer
* org.apache.mahout.classifier.sgd.TrainLogistic
* org.apache.mahout.classifier.sgd.RunLogistic
* org.apache.mahout.classifier.sgd.TrainAdaptiveLogistic
* org.apache.mahout.classifier.sgd.ValidateAdaptiveLogistic
* org.apache.mahout.classifier.sgd.RunAdaptiveLogistic
* org.apache.mahout.classifier.sequencelearning.hmm.BaumWelchTrainer
* org.apache.mahout.classifier.sequencelearning.hmm.ViterbiEvaluator
* org.apache.mahout.classifier.sequencelearning.hmm.RandomSequenceGenerator
* org.apache.mahout.classifier.df.tools.Describe

Aby uruchomić zadania, które używają tych klas, połącz się z klastrem HDInsight i uruchamiania zadań przy użyciu wiersza polecenia Hadoop. Zobacz [Klasyfikacja danych za pomocą wiersza polecenia Hadoop](#classify) , na przykład.

##<a name="next-steps"></a>Następne kroki

Teraz, gdy znasz sposobu używania Mahout, poznawanie innych sposobów pracy z danymi w HDInsight:

* [Gałąź z usługi HDInsight](hdinsight-use-hive.md)
* [Świnka z usługi HDInsight](hdinsight-use-pig.md)
* [MapReduce z usługi HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[aps]: ../powershell-install-configure.md
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
