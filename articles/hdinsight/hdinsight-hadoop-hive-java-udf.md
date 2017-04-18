<properties
pageTitle="Java funkcji zdefiniowanych przez użytkownika (UDF) za pomocą gałąź w HDInsight | Microsoft Azure"
description="Dowiedz się, jak tworzyć i używać języka Java funkcji zdefiniowanych przez użytkownika (UDF) z gałąź w HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="09/27/2016"
ms.author="larryfr"/>

#<a name="use-a-java-udf-with-hive-in-hdinsight"></a>Za pomocą języka Java UDF z gałąź w HDInsight

Gałąź doskonale nadaje się do pracy z danymi w HDInsight, ale czasami potrzebujesz więcej ogólnych celu języka. Gałąź umożliwia tworzenia funkcji zdefiniowanych przez użytkownika (UDF), za pomocą różnych języków programowania. W tym dokumencie dowiesz się, jak korzystać z gałęzi Java UDF.

## <a name="requirements"></a>Wymagania dotyczące

* Subskrypcję usługi Azure

* Klaster HDInsight (Windows systemem Linux lub)

    > [AZURE.NOTE] Większość czynności opisane w tym dokumencie będzie działać na obu typów klaster; jednak kroków umożliwia przekazywanie skompilowane UDF z klastrem, a następnie uruchom go są specyficzne dla klastrów opartych na systemie Linux. Informacje, które mogą być używane z systemem Windows klastrów znajdują się łącza.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 lub nowszy (lub odpowiednik, takich jak OpenJDK)

* [Środowiska Maven Apache](http://maven.apache.org/)

* Edytor tekstów lub Java IDE

    > [AZURE.IMPORTANT] Jeśli korzystasz z serwera systemem Linux HDInsight, ale tworzenie plików Python na komputerze klienckim systemu Windows, należy użyć edytora używający wysuwu wiersza jako koniec wiersza. Jeśli nie masz pewności, czy edytor używa wysuwu wiersza i CRLF, zobacz sekcję [Rozwiązywanie problemów](#troubleshooting) instrukcje dotyczące usuwania znaków CR za pomocą narzędzi w klastrze HDInsight.

## <a name="create-an-example-udf"></a>Tworzenie przykładu UDF

1. W wierszu polecenia Utwórz nowy projekt środowiska Maven należy wykonać następujące kroki:

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] Jeśli korzystasz z programu PowerShell, należy umieścić w cudzysłów parametry. Na przykład `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Spowoduje to utworzenie nowego katalogu o nazwie __exampleudf__, który będzie zawierał projektu środowiska Maven.

2. Po utworzeniu projektu Usuwanie katalogu __exampleudf-src-test__ , który został utworzony w ramach projektu; go nie być używane w tym przykładzie.

3. Otwórz __exampleudf/pom.xml__, a następnie zamienić istniejący `<dependencies>` wpisu z następujących czynności:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Te wpisy określanie wersji programu Hadoop i gałęzi dołączonych HDInsight 3.3 i 3.4 klastrów. Można znaleźć informacje o wersji Hadoop i gałęzi do dyspozycji HDInsight z dokumentu [przechowywania wersji składnika HDInsight](hdinsight-component-versioning.md) .

    Dodawanie `<build>` sekcji przed `</project>` wiersza na końcu pliku. W tej sekcji powinien zawierać następujące czynności:

        <build>
            <plugins>
                <!-- build for Java 1.7, even if you're on a later version -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
                <!-- build an uber jar -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.3</version>
                    <configuration>
                        <!-- Keep us from getting a can't overwrite file error -->
                        <transformers>
                            <transformer
                                    implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                            </transformer>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                            </transformer>
                        </transformers>
                        <!-- Keep us from getting a bad signature error -->
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
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </build>
    
    Te wpisy zdefiniuj sposób tworzenia projektu. W szczególności wersji języka Java, która korzysta z projektu i sposobu tworzenia uberjar wdrożenia z klastrem.

    Zapisz plik, gdy zostaną wprowadzone zmiany.

4. Zmień nazwę __exampleudf/src/main/java/com/microsoft/examples/App.java__ na __ExampleUDF.java__, a następnie otwórz plik w edytorze.

5. Zamień zawartość pliku __ExampleUDF.java__ następujące polecenie, następnie zapisz plik.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    To wykonuje UDF, przyjmuje wartość ciągu, która zwraca wersję małe litery w ciągu.

## <a name="build-and-install-the-udf"></a>Tworzenie i instalowanie UDF

1. Gromadzenie i pakowanie UDF, użyj następującego polecenia:

        mvn compile package

    To będzie utworzyć, a następnie pakowanie UDF do __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__.

2. Używanie `scp` polecenie, aby skopiować go do klastrów HDInsight.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    Zamień __Myuser.pds__ na koncie użytkownika SSH klaster. Zamień __mycluster__ jego nazwy. Jeśli używasz hasła do zabezpieczenia konta SSH pojawi o wprowadzenie hasła. Jeśli używasz certyfikatu, może być konieczne używanie `-i` parametr, aby określić plik klucza prywatnego.

3. Nawiązywanie połączenia z klastrem przy użyciu SSH. 

        ssh myuser@mycluster-ssh.azurehdinsight.net

    Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz następujące dokumenty.

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

4. Od sesji SSH skopiuj plik słoik do magazynu HDInsight.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## <a name="use-the-udf-from-hive"></a>Użyj UDF z gałęzi

1. Uruchom klienta Beeline z sesji SSH należy wykonać następujące kroki.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    Z tego polecenia zakłada używane domyślne __Administrator__ konta logowania dla klaster.

2. Po wyświetleniu określonego `jdbc:hive2://localhost:10001/>` monit, wprowadź poniższe czynności, aby dodać UDF do gałęzi i udostępnić go jako funkcja.

        ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. Używanie UDF do konwertowania wartości pobrane z tabeli ciągów małe litery.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    To będzie wybierz platformę urządzenia (Android, systemu Windows, iOS itp.) z tabeli, konwertowanie ciągu na małe litery, a następnie wyświetl je. Wynik pojawi się podobny do następującego.

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Następne kroki

Aby uzyskać inne sposoby pracy z gałęzi zobacz [Gałęzi korzystanie z usługi HDInsight](hdinsight-use-hive.md).

Więcej informacji na temat funkcji Hive User-Defined, zobacz sekcję [gałęzi operatory i funkcje zdefiniowane przez użytkownika](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) Wiki gałęzi u apache.org.
