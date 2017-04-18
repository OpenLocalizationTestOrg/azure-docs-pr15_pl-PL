<properties
pageTitle="Tworzenie aplikacji HBase przy użyciu środowiska Maven i wdrażanie usługi HDInsight systemu Windows | Microsoft Azure"
description="Dowiedz się, jak używać środowiska Maven Apache do tworzenia aplikacji opartego na języku Java HBase Apache, a następnie wdrożyć go do klastrów HDInsight systemu Windows Azure."
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
ms.date="10/03/2016"
ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Używanie środowiska Maven tworzenie aplikacji Java HBase za pomocą usługi HDInsight systemu Windows (Hadoop)

Dowiedz się, jak tworzyć i tworzenie aplikacji [Apache HBase](http://hbase.apache.org/) w Java przy użyciu środowiska Maven Apache. Następnie za pomocą aplikacji Azure HDInsight (Hadoop).

[Środowiska maven](http://maven.apache.org/) to oprogramowanie zarządzania projektami i zrozumienia narzędzia, które umożliwia tworzenie raportów dla projektów języka Java, dokumentacji i oprogramowania. W tym artykule można Dowiedz się, jak za jej pomocą tworzyć podstawowe aplikacji Java, który tworzy kwerendy, i usuwa tabelę HBase w klastrze Azure HDInsight.

> [AZURE.NOTE] Kroki opisane w tym dokumencie przyjęto założenie, że używasz klastrze HDInsight systemu Windows. Aby uzyskać informacje na temat korzystania z klastrem systemem Linux HDInsight zobacz [Używanie środowiska Maven do tworzenia aplikacji Java, które używają HBase z systemem Linux HDInsight](hdinsight-hbase-build-java-maven-linux.md)

##<a name="requirements"></a>Wymagania dotyczące

* [Języka Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 lub nowszy

* [Środowiska maven](http://maven.apache.org/)


* [Klaster HDInsight systemu Windows z HBase](hdinsight-hbase-tutorial-get-started.md#create-hbase-cluster)


    > [AZURE.NOTE] Kroki opisane w tym dokumencie przetestowano z wersjami klaster HDInsight 3,2 i 3.3. Wartości domyślnych w przykładach są dla klastrów HDInsight 3.3.

##<a name="create-the-project"></a>Tworzenie projektu

1. Z poziomu wiersza polecenia w środowisku rozwoju Zmień katalog do lokalizacji, w której chcesz utworzyć projekt, na przykład `cd code\hdinsight`.

2. Za pomocą polecenia __mvn__ , które jest zainstalowany wraz z środowiska Maven, aby wygenerować rusztowania projektu.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    To polecenie tworzy katalog w bieżącej lokalizacji o nazwie określonej przez parametr __artifactID__ (**hbaseapp** w tym przykładzie.) Ten katalog zawiera następujące elementy:

    * __pom.XML__: modelu obiektowego projektu ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) zawiera informacje i konfiguracja są używane do tworzenia projektu.

    * __src__: katalog zawierający katalogu __main\java\com\microsoft\examples__ , gdzie będzie Autor aplikacji.

3. Usuń plik __src\test\java\com\microsoft\examples\apptest.java__ , ponieważ nie jest używany w tym przykładzie.

##<a name="update-the-project-object-model"></a>Aktualizacja modelu obiektowego projektu

1. Edytowanie pliku __pom.xml__ i Dodaj następujący kod wewnątrz `<dependencies>` sekcji:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    W tej sekcji opisano środowiska Maven, że projekt wymaga wersji __klienta hbase__ __1.1.2__. W czasie kompilacji tę współzależność są pobierane z repozytorium środowiska Maven domyślne. Dowiedz się więcej o tym współzależności umożliwia [Środowiska Maven centralnym repozytorium wyszukiwania](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) .

    > [AZURE.IMPORTANT] Numer wersji musi odpowiadać wersji HBase dołączonej klaster HDInsight. Aby znaleźć numer wersji poprawne, skorzystaj z poniższej tabeli.

  	| Usługa HDInsight klaster wersji | Wersję HBase ma być używana |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3.3 | 1.1.2 |

    Uzyskać więcej informacji o wersji usługi HDInsight i składników zobacz [Co to są dostępne z usługi HDInsight różne składniki Hadoop](hdinsight-component-versioning.md).

2. Jeśli korzystasz z klastrem HDInsight 3.3, można też dodać poniższe czynności, aby `<dependencies>` sekcji:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Ta zależność pobierze phoenix podstawowe składniki, które są używane przez wersji Hbase 1.1.x.

2. Dodaj następujący kod do pliku __pom.xml__ . W tej sekcji musi znajdować się wewnątrz `<project>...</project>` znaczników w pliku, na przykład między `</dependencies>` i `</project>`.

        <build>
          <sourceDirectory>src</sourceDirectory>
          <resources>
              <resource>
                <directory>${basedir}/conf</directory>
                <filtering>false</filtering>
                <includes>
                  <include>hbase-site.xml</include>
                </includes>
              </resource>
            </resources>
          <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                </configuration>
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
              </configuration>
              <executions>
                <execution>
                  <phase>package</phase>
                  <goals>
                    <goal>shade</goal>
                  </goals>
                </execution>
              </executions>
            </plugin>
          </plugins>
        </build>

    `<resources>` Sekcji konfiguruje zasobu (__conf\hbase site.xml__), która zawiera informacje dotyczące HBase konfiguracji.

    > [AZURE.NOTE] Można także ustawić wartości konfiguracyjnych za pomocą kodu. Zobacz komentarzy w następującym przykładzie __CreateTable__ do wykonywania tego zadania.

    To `<plugins>` sekcji konfiguruje [Wtyczki kompilatora środowiska Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) oraz [Dodatek cień środowiska Maven](http://maven.apache.org/plugins/maven-shade-plugin/). Wtyczki kompilujący jest używana do kompilowania topologii. Wtyczki cień jest używany do zapobiegania duplikowanie licencji w pakiecie SŁOIK jest tworzona środowiska Maven. Przyczyny, dla której ta opcja jest używana to, że pliki licencji zduplikowanych powodują wystąpienie błędu w czasie wykonywania w klastrze HDInsight. Za pomocą środowiska maven cień dodatek z `ApacheLicenseResourceTransformer` implementacji zapobiega ten błąd.

    Dodatek cień środowiska maven również tworzy słoik uber (lub tłuszczu słoik) zawierający wszystkie zależności wymagane przez tę aplikację.

3. Zapisz plik __pom.xml__ .

4. Tworzenie nowego katalogu o nazwie __telefoniczna__ w katalogu __hbaseapp__ . W katalogu __telefonicznej__ Utwórz plik o nazwie __hbase site.xml__. Zawartość pliku, należy użyć następującej składni:

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     http://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    Ten plik będzie używany załadować konfiguracji HBase klaster HDInsight.

    > [AZURE.NOTE] Jest to plik minimalnego site.xml hbase i zawiera od zera minimalnych ustawień klaster HDInsight.

3. Zapisz plik __hbase site.xml__ .

##<a name="create-the-application"></a>Tworzenie aplikacji

1. Przejdź do katalogu __hbaseapp\src\main\java\com\microsoft\examples__ i Zmień nazwę pliku app.java na __CreateTable.java__.

2. Otwórz plik __CreateTable.java__ i zastąpić istniejącą zawartość następujący kod:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;
        import org.apache.hadoop.hbase.HTableDescriptor;
        import org.apache.hadoop.hbase.TableName;
        import org.apache.hadoop.hbase.HColumnDescriptor;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Put;
        import org.apache.hadoop.hbase.util.Bytes;

        public class CreateTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Example of setting zookeeper values for HDInsight
            // in code instead of an hbase-site.xml file
            //
            // config.set("hbase.zookeeper.quorum",
            //            "zookeepernode0,zookeepernode1,zookeepernode2");
            //config.set("hbase.zookeeper.property.clientPort", "2181");
            //config.set("hbase.cluster.distributed", "true");
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

            // create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // create the table...
            HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
            // ... with two column families
            tableDescriptor.addFamily(new HColumnDescriptor("name"));
            tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
            admin.createTable(tableDescriptor);

            // define some people
            String[][] people = {
                { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
                { "2", "Franklin", "Holtz", "franklin@contoso.com" },
                { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
                { "4", "Rae", "Schroeder", "rae@contoso.com" },
                { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
                { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

            HTable table = new HTable(config, "people");

            // Add each person to the table
            //   Use the `name` column family for the name
            //   Use the `contactinfo` column family for the email
            for (int i = 0; i< people.length; i++) {
              Put person = new Put(Bytes.toBytes(people[i][0]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
              person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
              person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
              table.put(person);
            }
            // flush commits and close the table
            table.flushCommits();
            table.close();
          }
        }

    Jest to klasa __CreateTable__ , Utwórz tabelę o nazwie __osoby__ i niektórzy użytkownicy wstępnie wypełnione.

3. Zapisz plik __CreateTable.java__ .

4. W katalogu __hbaseapp\src\main\java\com\microsoft\examples__ Utwórz nowy plik o nazwie __SearchByEmail.java__. Zawartość tego pliku, należy użyć następującego kodu:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HTable;
        import org.apache.hadoop.hbase.client.Scan;
        import org.apache.hadoop.hbase.client.ResultScanner;
        import org.apache.hadoop.hbase.client.Result;
        import org.apache.hadoop.hbase.filter.RegexStringComparator;
        import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
        import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
        import org.apache.hadoop.hbase.util.Bytes;
        import org.apache.hadoop.util.GenericOptionsParser;

        public class SearchByEmail {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Use GenericOptionsParser to get only the parameters to the class
            // and not all the parameters passed (when using WebHCat for example)
            String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
            if (otherArgs.length != 1) {
              System.out.println("usage: [regular expression]");
              System.exit(-1);
            }

            // Open the table
            HTable table = new HTable(config, "people");

            // Define the family and qualifiers to be used
            byte[] contactFamily = Bytes.toBytes("contactinfo");
            byte[] emailQualifier = Bytes.toBytes("email");
            byte[] nameFamily = Bytes.toBytes("name");
            byte[] firstNameQualifier = Bytes.toBytes("first");
            byte[] lastNameQualifier = Bytes.toBytes("last");

            // Create a new regex filter
            RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
            // Attach the regex filter to a filter
            //   for the email column
            SingleColumnValueFilter filter = new SingleColumnValueFilter(
              contactFamily,
              emailQualifier,
              CompareOp.EQUAL,
              emailFilter
            );

            // Create a scan and set the filter
            Scan scan = new Scan();
            scan.setFilter(filter);

            // Get the results
            ResultScanner results = table.getScanner(scan);
            // Iterate over results and print  values
            for (Result result : results ) {
              String id = new String(result.getRow());
              byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
              String firstName = new String(firstNameObj);
              byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
              String lastName = new String(lastNameObj);
              System.out.println(firstName + " " + lastName + " - ID: " + id);
              byte[] emailObj = result.getValue(contactFamily, emailQualifier);
              String email = new String(emailObj);
              System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
            }
            results.close();
            table.close();
          }
        }

    Klasa __SearchByEmail__ może być używana do kwerendy dla wierszy za pomocą adresu e-mail. Ponieważ korzysta ona z filtrem wyrażeń regularnych, można udostępnić ciągu lub wyrażenie podczas korzystania z klasą.

5. Zapisz plik __SearchByEmail.java__ .

6. W katalogu __hbaseapp\src\main\hava\com\microsoft\examples__ Utwórz nowy plik o nazwie __DeleteTable.java__. Zawartość tego pliku, należy użyć następującego kodu:

        package com.microsoft.examples;
        import java.io.IOException;

        import org.apache.hadoop.conf.Configuration;
        import org.apache.hadoop.hbase.HBaseConfiguration;
        import org.apache.hadoop.hbase.client.HBaseAdmin;

        public class DeleteTable {
          public static void main(String[] args) throws IOException {
            Configuration config = HBaseConfiguration.create();

            // Create an admin object using the config
            HBaseAdmin admin = new HBaseAdmin(config);

            // Disable, and then delete the table
            admin.disableTable("people");
            admin.deleteTable("people");
          }
        }

    Ta klasa jest oczyszczania w tym przykładzie, wyłączając i upuszczanie tabeli utworzone przez klasę __CreateTable__ .

7. Zapisz plik __DeleteTable.java__ .

##<a name="build-and-package-the-application"></a>Tworzenie i pakiet aplikacji

1. Otwórz wiersz polecenia i przejdź do katalogu __hbaseapp__ katalogów.

2. Aby utworzyć plik SŁOIK, zawierającego aplikację, użyj następującego polecenia:

        mvn clean package

    To czyści wszelkie poprzedniego artefakty kompilacji, pobiera wszystkie zależności, które jeszcze nie zostały zainstalowane, a następnie tworzy i pakietów aplikacji.

3. Po zakończeniu polecenie katalog __hbaseapp\target__ zawiera plik o nazwie __hbaseapp-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] Plik __hbaseapp-1.0-SNAPSHOT.jar__ jest uber słoik (nazywane tłuszczu słoik), zawierające wszystkie zależności wymagane do uruchamiania aplikacji.

##<a name="upload-the-jar-file-and-start-a-job"></a>Przekaż plik SŁOIK i rozpocząć zadanie

Istnieje wiele sposobów przekazywanie pliku do klaster HDInsight, zgodnie z opisem w [przekazywanie danych dla zadań Hadoop w HDInsight](hdinsight-upload-data.md). W poniższej procedurze użyto Azure programu PowerShell.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


1. Po zainstalowaniu i skonfigurowaniu programu PowerShell Azure, Utwórz nowy plik o nazwie __hbase runner.psm1__. Zawartość tego pliku, należy użyć następującej składni:

        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.CreateTable"
        -clusterName "MyHDInsightCluster"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "contoso.com"
        
        .EXAMPLE
        Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
        -clusterName "MyHDInsightCluster"
        -emailRegex "^r" -showErr
        #>
        
        function Start-HBaseExample {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
        #The class to run
        [Parameter(Mandatory = $true)]
        [String]$className,
        
        #The name of the HDInsight cluster
        [Parameter(Mandatory = $true)]
        [String]$clusterName,
        
        #Only used when using SearchByEmail
        [Parameter(Mandatory = $false)]
        [String]$emailRegex,
        
        #Use if you want to see stderr output
        [Parameter(Mandatory = $false)]
        [Switch]$showErr
        )
        
        Set-StrictMode -Version 3
        
        # Is the Azure module installed?
        FindAzure
        
        # Get the login for the HDInsight cluster
        $creds = Get-Credential
        
        # Get storage information
        $storage = GetStorage -clusterName $clusterName
        
        # The JAR
        $jarFile = "wasbs:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"
        
        # The job definition
        $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile $jarFile `
            -ClassName $className `
            -Arguments $emailRegex
        
        # Get the job output
        $job = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $jobDefinition `
            -HttpCredential $creds
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        if($showErr)
        {
        Write-Host "STDERR"
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds `
                    -DisplayOutputType StandardError
        }
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
                    -Clustername $clusterName `
                    -JobId $job.JobId `
                    -DefaultContainer $storage.container `
                    -DefaultStorageAccountName $storage.storageAccount `
                    -DefaultStorageAccountKey $storage.storageAccountKey `
                    -HttpCredential $creds
        }
        
        <#
        .SYNOPSIS
        Copies a file to the primary storage of an HDInsight cluster.
        .DESCRIPTION
        Copies a file from a local directory to the blob container for
        the HDInsight cluster.
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        .EXAMPLE
        Add-HDInsightFile -localPath "C:\temp\data.txt"
        -destinationPath "example/data/data.txt"
        -ClusterName "MyHDInsightCluster"
        -Container "MyContainer"
        #>
        
        function Add-HDInsightFile {
            [CmdletBinding(SupportsShouldProcess = $true)]
            param(
                #The path to the local file.
                [Parameter(Mandatory = $true)]
                [String]$localPath,
                
                #The destination path and file name, relative to the root of the container.
                [Parameter(Mandatory = $true)]
                [String]$destinationPath,
                
                #The name of the HDInsight cluster
                [Parameter(Mandatory = $true)]
                [String]$clusterName,
                
                #If specified, overwrites existing files without prompting
                [Parameter(Mandatory = $false)]
                [Switch]$force
            )
            
            Set-StrictMode -Version 3
            
            # Is the Azure module installed?
            FindAzure
            
            # Get authentication for the cluster
            $creds=Get-Credential
            
            # Does the local path exist?
            if (-not (Test-Path $localPath))
            {
                throw "Source path '$localPath' does not exist."
            }
            
            # Get the primary storage container
            $storage = GetStorage -clusterName $clusterName
            
            # Upload file to storage, overwriting existing files if -force was used.
            Set-AzureStorageBlobContent -File $localPath `
                -Blob $destinationPath `
                -force:$force `
                -Container $storage.container `
                -Context $storage.context
        }
        
        function FindAzure {
            # Is there an active Azure subscription?
            $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
            if(-not($sub))
            {
                throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
            }
        }
        
        function GetStorage {
            param(
                [Parameter(Mandatory = $true)]
                [String]$clusterName
            )
            $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
            # Does the cluster exist?
            if (!$hdi)
            {
                throw "HDInsight cluster '$clusterName' does not exist."
            }
            # Create a return object for context & container
            $return = @{}
            $storageAccounts = @{}
            
            # Get storage information
            $resourceGroup = $hdi.ResourceGroup
            $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
            $container=$hdi.DefaultStorageContainer
            $storageAccountKey=(Get-AzureRmStorageAccountKey `
                -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
            # Get the resource group, in case we need that
            $return.resourceGroup = $resourceGroup
            # Get the storage context, as we can't depend
            # on using the default storage context
            $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
            # Get the container, so we know where to
            # find/store blobs
            $return.container = $container
            # Return storage accounts to support finding all accounts for
            # a cluster
            $return.storageAccount = $storageAccountName
            $return.storageAccountKey = $storageAccountKey
            
            return $return
        }
        # Only export the verb-phrase things
        export-modulemember *-*

    Ten plik zawiera dwa moduły:

    * __Dodaj HDInsightFile__ — umożliwia przekazywanie plików do usługi HDInsight

    * __Start HBaseExample__ - używanych do uruchamiania klas utworzony wcześniej

2. Zapisz plik __hbase runner.psm1__ .

3. Otwieranie nowego okna programu PowerShell Azure, przejdź do katalogu __hbaseapp__ katalogów, a następnie uruchom następujące polecenie.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Zmienić ścieżkę do lokalizacji pliku __hbase runner.psm1__ utworzony wcześniej. Moduł to rejestruje dla tej sesji programu PowerShell Azure.

2. Użyj następującego polecenia, aby przekazać __hbaseapp-1.0-SNAPSHOT.jar__ do klaster HDInsight.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Zamień nazwę klaster HDInsight __hdinsightclustername__ . Polecenie wysyła __hbaseapp-1.0-SNAPSHOT.jar__ do lokalizacji __przykład/słoików__ w podstawową pamięć dla klaster HDInsight.

3. Po przekazaniu pliku są umożliwiają tworzenie tabeli przy użyciu __hbaseapp__następujący kod:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Zamień nazwę klaster HDInsight __hdinsightclustername__ .

    To polecenie tworzy nową tabelę o nazwie __osoby__ w klastrze HDInsight. To polecenie nie jest wyświetlana dowolne dane wyjściowe w oknie konsoli.

2. Aby wyszukać wpisy w tabeli, użyj następującego polecenia:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Zamień nazwę klaster HDInsight __hdinsightclustername__ .

    Tego polecenia używa klasy **SearchByEmail** , aby wyszukać wszystkie wiersze, gdzie rodzinie kolumny __contactinformation__ i kolumny __e-mail__ zawiera ciąg __contoso.com__. Powinien zostać wyświetlony następujące wyniki:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Za pomocą __fabrikam.com__ dla `-emailRegex` wartość zwraca użytkowników, którzy mają __fabrikam.com__ w polu adres e-mail. Ponieważ to wyszukiwanie jest zaimplementowana przy użyciu filtru wyrażeń regularnych, można także wprowadzić wyrażeń regularnych, takich jak __^ r__, które wpisy zwraca miejsce, w którym wiadomości e-mail zaczyna się od litery "r".

##<a name="delete-the-table"></a>Usuwanie tabeli

Po wykonaniu tych czynności z przykładem, użyj następującego polecenia z sesji programu PowerShell Azure, aby usunąć tabelę __osób__ , w tym przykładzie użyto:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Zamień nazwę klaster HDInsight __hdinsightclustername__ .

##<a name="troubleshooting"></a>Rozwiązywanie problemów

###<a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Nie wyników lub nieoczekiwane wyniki podczas korzystania z Start HBaseExample

Używanie `-showErr` parametr, aby wyświetlić błąd standardowy (STDERR) powstaje podczas wykonywania zadania.
