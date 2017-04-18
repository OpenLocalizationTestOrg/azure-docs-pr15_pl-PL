<properties
 pageTitle="Można opracowywać sparzanie MapReduce zadania środowiska Maven | Microsoft Azure"
 description="Dowiedz się, jak utworzyć zadanie sparzanie MapReduce wdrażanie, a następnie uruchom zadanie Hadoop w klastrze HDInsight za pomocą środowiska Maven."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
    tags="azure-portal"/>
<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="10/18/2016"
 ms.author="larryfr"/>

# <a name="develop-scalding-mapreduce-jobs-with-apache-hadoop-on-hdinsight"></a>Można opracowywać sparzanie MapReduce zadania Apache Hadoop na HDInsight

Sparzanie jest biblioteką Scala, który ułatwia tworzenie Hadoop MapReduce zadania. Zapewnia on składni zwięzły, jak również ścisłą integrację z Scala.

W tym dokumencie Dowiedz się, jak utworzyć zadanie MapReduce liczba podstawowe word napisana Scalding za pomocą środowiska Maven. Następnie dowiesz jak wdrażać i uruchomić to zadanie w klastrze HDInsight.

## <a name="prerequisites"></a>Wymagania wstępne

- **Azure subskrypcji**. Zobacz [Azure pobrać bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Systemu Windows i Linux oraz podstawie Hadoop w klastrze HDInsight**. Aby uzyskać więcej informacji, zobacz [systemem Linux należy Hadoop na HDInsight](hdinsight-hadoop-provision-linux-clusters.md) lub [Hadoop na HDInsight systemu Windows Obsługa administracyjna](hdinsight-provision-clusters.md) .

* **[Środowiska maven](http://maven.apache.org/)**

* **[Języka Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 lub nowszy**

## <a name="create-and-build-the-project"></a>Tworzenie i tworzenie projektu

1. Aby utworzyć nowy projekt środowiska Maven, użyj następującego polecenia:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    To polecenie Utwórz nowy katalog o nazwie **scaldingwordcount**i Utwórz rusztowania aplikacji Scala.

2. W katalogu **scaldingwordcount** Otwórz plik **pom.xml** i zastąpić zawartość z następujących czynności:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
                    <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                        <exclude>META-INF/*.SF</exclude>
                        <exclude>META-INF/*.DSA</exclude>
                        <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Ten plik zawiera opis projektu, zależności i dodatków plug-in. Poniżej przedstawiono ważne wpisy:

    * **maven.Compiler.Source** i **maven.compiler.target**: ustawia wersji języka Java dla tego projektu

    * **repozytoria**: repozytoria, zawierających pliki zależności używane przez tego projektu

    * **sparzanie core_2.11** i **hadoop podstawowe**: tego projektu zależy od zarówno Scalding, jak i Hadoop pakietów core

    * **środowiska maven-scala — dodatek**: dodatek skompilować scala aplikacji

    * **środowiska maven cień — dodatek**: dodatek, aby utworzyć cieniowane słoików (fat). Ten dodatek dotyczy filtrów i przekształceń; specificially:

        * **Filtry**: filtry zastosowane modyfikowanie meta informacje zawarte w pliku słoik. Aby zapobiec podpisywaniu wyjątki w czasie wykonywania, z wyłączeniem różnych plików podpisu, które mogą być dołączone zależności.

        * **wykonania**: Konfiguracja wykonanie faza pakietu Określa klasę **com.twitter.scalding.Tool** jako główna klasa pakietu. Bez niej należy określić com.twitter.scalding.Tool, a także klasy, który zawiera logikę aplikacji podczas uruchamiania zadania przy użyciu polecenia hadoop.

3. Usuwanie katalogu **src-test** , jak zostanie nie utworzony testy z w tym przykładzie.

4. Otwórz plik **src/main/scala/com/microsoft/example/App.scala** i zastąpić zawartość z następujących czynności:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    To wykonuje zadanie Statystyka podstawowe programu word.

5. Zapisz i zamknij pliki.

6. Użyj następującego polecenia z katalogu **scaldingwordcount** do tworzenia i pakiet aplikacji:

        mvn package

    Po wykonaniu tego zadania pakiet zawierający aplikację WordCount można znaleźć w **target/scaldingwordcount-1.0-SNAPSHOT.jar**.

## <a name="run-the-job-on-a-linux-based-cluster"></a>Uruchamianie zadania w klastrze z systemem Linux

> [AZURE.NOTE] W poniższej procedurze użyto SSH i polecenie Hadoop. Dla innych metod wykonywania zadań MapReduce zobacz [Używanie MapReduce w Hadoop na HDInsight](hdinsight-use-mapreduce.md).

1. Aby przekazać pakiet do klaster HDInsight, użyj następującego polecenia:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    Spowoduje to skopiowanie plików z systemu lokalnego do węzła głównego.

    > [AZURE.NOTE] Jeśli hasło jest używany do zabezpieczenia konta SSH, zostanie wyświetlony monit o podanie hasła. Jeśli został użyty klucz SSH, może być konieczne używanie `-i` parametru i ścieżkę do klucz prywatny. Na przykład`scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. Użyj następującego polecenia, aby nawiązać połączenie głowy węzła:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Jeśli hasło jest używany do zabezpieczenia konta SSH, zostanie wyświetlony monit o podanie hasła. Jeśli został użyty klucz SSH, może być konieczne używanie `-i` parametru i ścieżkę do klucz prywatny. Na przykład`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. Po połączeniu węzła głównego, użyj następującego polecenia, aby uruchomić zadanie Statystyka programu word

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    Zostanie zwrócony klasy WordCount, którą zaimplementowana w starszym. `--hdfs`powoduje, że zadanie za pomocą HDFS. `--input`Określa plik wprowadzania tekstu podczas `--output` Określa lokalizację danych wyjściowych.

4. Po zakończeniu zadania umożliwia wyświetlanie danych wyjściowych poniższe czynności.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Spowoduje to wyświetlenie informacji podobny do następującego:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="run-the-job-on-a-windows-based-cluster"></a>Uruchamianie zadania w klastrze systemu Windows

W poniższej procedurze użyto programu Windows PowerShell. Dla innych metod wykonywania zadań MapReduce zobacz [Używanie MapReduce w Hadoop na HDInsight](hdinsight-use-mapreduce.md).

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Uruchom Azure programu PowerShell i zalogowany do konta Azure. Po zapewnieniu poświadczenia, polecenie zwraca informacje o Twoim koncie.

        Add-AzureRMAccount

        Id                             Type       ...
        --                             ----
        someone@example.com            User       ...

3. Jeśli masz wiele subskrypcji, podaj identyfikator subskrypcji, którego chcesz użyć do wdrożenia.

        Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] Możesz użyć `Get-AzureRMSubscription` Aby uzyskać listę wszystkich subskrypcji skojarzonych z Twoim kontem, która zawiera identyfikator subskrypcji dla każdego z nich.

4. Poniższy skrypt umożliwia przekazywanie i wykonywania zadania WordCount. Zamienianie `CLUSTERNAME` o nazwie usługi HDInsight klaster i upewnij się, że `$fileToUpload` jest poprawny ścieżkę do pliku __SNAPSHOT.jar-scaldingwordcount-1.0__ .

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = Read-Host -Prompt "Enter the HDInsight cluster name"
        $fileToUpload = Read-Host -Prompt "Enter the path to the scaldingwordcount-1.0-SNAPSHOT.jar file"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential -Message "Enter the login credentials for the cluster"
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
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                        "--input", `
                        "wasbs:///example/data/gutenberg/davinci.txt", `
                        "--output", `
                        "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     Po uruchomieniu skrypt, wyświetli się monit o wprowadź nazwę użytkownika administratora i hasło dla klaster HDInsight. Błędy występujące podczas uruchamiania zadania będą rejestrowane do konsoli.
     
6. Po zakończeniu zadania, wyniki będą pobierane na plik __output.txt__ w bieżącym katalogu. Użyj następującego polecenia, aby wyświetlić wyniki.

        cat output.txt

    Plik powinien zawierać wartości podobny do następującego:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz sposobu umożliwia utworzenie zadania MapReduce dla HDInsight Scalding, użyj następujących łączy, aby poznać inne sposoby pracy z usługą hdinsight Azure za.

* [Gałąź za pomocą usługi HDInsight](hdinsight-use-hive.md)

* [Świnka korzystanie z usługi HDInsight](hdinsight-use-pig.md)

* [MapReduce zadań za pomocą usługi HDInsight](hdinsight-use-mapreduce.md)
