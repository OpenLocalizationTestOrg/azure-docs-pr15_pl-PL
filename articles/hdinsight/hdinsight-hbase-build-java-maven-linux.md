<properties
    pageTitle="Tworzenie aplikacji HBase przy użyciu środowiska Maven i Java, a następnie wdrożyć na podstawie Linux HDInsight | Microsoft Azure"
    description="Dowiedz się, jak używać środowiska Maven Apache do tworzenia aplikacji opartego na języku Java HBase Apache, a następnie wdrożyć go na podstawie Linux HDInsight w chmurze Azure."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-maven-to-build-java-applications-that-use-hbase-with-linux-based-hdinsight-hadoop"></a>Za pomocą środowiska Maven tworzyć aplikacje Java, które używają HBase z systemem Linux HDInsight (Hadoop)

Dowiedz się, jak tworzyć i tworzenie aplikacji [Apache HBase](http://hbase.apache.org/) w Java przy użyciu środowiska Maven Apache. Następnie użyj aplikacji z klastrem HDInsight systemem Linux.

[Środowiska maven](http://maven.apache.org/) to oprogramowanie zarządzania projektami i zrozumienia narzędzia, które umożliwia tworzenie raportów dla projektów języka Java, dokumentacji i oprogramowania. W tym artykule pokażemy, jak używać do tworzenia podstawowych aplikacji Java, że jest tworzony, kwerendy i usuwa tabelę HBase w klastrze HDInsight systemem Linux.

> [AZURE.NOTE] Kroki opisane w tym dokumencie przyjęto założenie, że używasz klastrze systemem Linux HDInsight. Aby uzyskać informacje na temat korzystania z klastrem HDInsight systemu Windows zobacz [Używanie środowiska Maven do tworzenia aplikacji Java, które używają HBase z usługi HDInsight systemu Windows](hdinsight-hbase-build-java-maven.md)

##<a name="requirements"></a>Wymagania dotyczące

* [Języka Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 lub nowszy

* [Środowiska maven](http://maven.apache.org/)

* [Klaster systemem Linux Azure HDInsight z HBase](../hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

    > [AZURE.NOTE] Kroki opisane w tym dokumencie przetestowano z wersjami klaster HDInsight 3,2, 3.3 i 3.4. Wartości domyślnych w przykładach są dla klastrów HDInsight 3.4.

* **Znajomość SSH i połączenia**. Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH i połączenia Zobacz:

    * **Klienci Linux, Unix lub OS X**: zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, OS X lub Unix](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Klienci systemu Windows**: zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

##<a name="create-the-project"></a>Tworzenie projektu

1. W wierszu polecenia w środowisku rozwoju, zmień katalog do lokalizacji, w której chcesz utworzyć projekt, na przykład `cd code/hdinsight`.

2. Za pomocą polecenia __mvn__ , które jest zainstalowany wraz z środowiska Maven, aby wygenerować rusztowania projektu.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Spowoduje to utworzenie nowego katalogu w bieżącym katalogu o nazwie określonej przez parametr __artifactID__ (**hbaseapp** w tym przykładzie.) Katalog ten będzie zawierał następujące elementy:

    * __pom.XML__: modelu obiektowego projektu ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) zawiera informacje i konfiguracja są używane do tworzenia projektu.

    * __src__: katalog zawierający katalogu __głównego java-com/microsoft/przykłady__ miejsce, w którym będzie Autor aplikacji.

3. Usuń plik __src/test/java/com/microsoft/examples/apptest.java__ , ponieważ nie będą używane w tym przykładzie.

##<a name="update-the-project-object-model"></a>Aktualizacja modelu obiektowego projektu

1. Edytowanie pliku __pom.xml__ i Dodaj następujący kod wewnątrz `<dependencies>` sekcji:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Informuje środowiska Maven, że projekt wymaga wersji __klienta hbase__ __1.1.2__. W czasie kompilacji to będą pobierane z repozytorium środowiska Maven domyślne. Dowiedz się więcej o tym współzależności umożliwia [Środowiska Maven centralnym repozytorium wyszukiwania](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) .

    > [AZURE.IMPORTANT] Numer wersji musi odpowiadać wersji HBase dołączonej klaster HDInsight. Aby znaleźć numer wersji poprawne, skorzystaj z poniższej tabeli.

  	| Usługa HDInsight klaster wersji | Wersję HBase ma być używana |
  	| ----- | ----- |
  	| 3,2 | 0.98.4-hadoop2 |
  	| 3.3 i 3.4 | 1.1.2 |

    Uzyskać więcej informacji o wersji usługi HDInsight i składników zobacz [Co to są dostępne z usługi HDInsight różne składniki Hadoop](hdinsight-component-versioning.md).

2. Jeśli korzystasz z usługi HDInsight 3.3 lub 3.4 klaster, można też dodać poniższe czynności, aby `<dependencies>` sekcji:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>
    
    Spowoduje to załadowanie phoenix podstawowe składniki, które są potrzebne wersją Hbase 1.1.x.

2. Dodaj następujący kod do pliku __pom.xml__ . To musi znajdować się wewnątrz `<project>...</project>` znaczników w pliku, na przykład między `</dependencies>` i `</project>`.

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

    Konfiguruje tego zasobu (__telefoniczna hbase-site.xml__,), która zawiera informacje dotyczące HBase konfiguracji.

    > [AZURE.NOTE] Można także ustawić wartości konfiguracji za pomocą kodu. Zobacz komentarzy w następującym przykładzie __CreateTable__ do wykonywania tego zadania.

    Konfiguruje również [Wtyczki kompilatora środowiska Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) oraz [Dodatek cień środowiska Maven](http://maven.apache.org/plugins/maven-shade-plugin/). Wtyczki kompilatora jest używana do kompilowania topologii. Wtyczki cień jest używany do zapobiegania duplikowanie licencji w pakiecie SŁOIK jest tworzona środowiska Maven. Przyczyny, dla której ta opcja jest używana to, że pliki licencji zduplikowanych powodują wystąpienie błędu w czasie wykonywania w klastrze HDInsight. Za pomocą środowiska maven cień dodatek z `ApacheLicenseResourceTransformer` implementacji zapobiega ten błąd.

    Dodatek cień środowiska maven również tworzy słoik uber (lub tłuszczu słoik) zawierający wszystkie zależności wymagane przez tę aplikację.

3. Zapisz plik __pom.xml__ .

4. Tworzenie nowego katalogu o nazwie __telefoniczna__ w katalogu __hbaseapp__ . To posłuży do przechowywania informacji o konfiguracji do łączenia się z HBase.

5. Użyj następującego polecenia, aby skopiować konfigurację HBase z serwera usługi HDInsight do katalogu __telefonicznej__ . Zamień **nazwę użytkownika** nazwę usługi logowania SSH. Zastąp **NAZWAKLASTRA** nazwy klaster HDInsight:

        scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml

    > [AZURE.NOTE] Jeśli używasz hasła dla konta SSH pojawi o wprowadzenie hasła. Jeśli użyto klawisza SSH z kontem, może być konieczne używanie `-i` parametr, aby określić ścieżkę do pliku klucza. Poniższy przykład pobierze klucz prywatny z `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml`

##<a name="create-the-application"></a>Tworzenie aplikacji

1. Przejdź do katalogu __hbaseapp-src główne java-com/microsoft/przykłady__ i Zmień nazwę pliku app.java na __CreateTable.java__.

2. Otwórz plik __CreateTable.java__ i zastąpić istniejącą zawartość następujących czynności:

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
            //
            //NOTE: Actual zookeeper host names can be found using Ambari:
            //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"
            
            //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
            config.set("zookeeper.znode.parent","/hbase-unsecure");

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

    Jest to klasa __CreateTable__ , Utwórz tabelę o nazwie __osoby__ i niektórych użytkowników wstępnie wypełnione.

3. Zapisz plik __CreateTable.java__ .

4. W katalogu __hbaseapp-src główne java-com/microsoft/przykładów__ należy utworzyć nowy plik o nazwie __SearchByEmail.java__. Zawartość tego pliku, należy użyć następującej składni:

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

    Klasa __SearchByEmail__ może być używana do kwerendy dla wierszy za pomocą adresu e-mail. Ponieważ korzysta ona z filtrem wyrażeń regularnych, można zapewnić ciągu lub wyrażenie podczas korzystania z klasą.

5. Zapisz plik __SearchByEmail.java__ .

6. W katalogu __hbaseapp-src główne hava-com/microsoft/przykładów__ należy utworzyć nowy plik o nazwie __DeleteTable.java__. Zawartość tego pliku, należy użyć następującej składni:

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

2. Z katalogu __hbaseapp__ Użyj następującego polecenia, aby utworzyć plik SŁOIK, zawierającego aplikację:

        mvn clean package

    To czyści wszelkie poprzedniego artefakty kompilacji, pobiera wszystkie zależności, które jeszcze nie zostały zainstalowane, a następnie tworzy i pakietów aplikacji.

3. Po zakończeniu polecenie katalog __hbaseapp docelowy__ będzie zawierać plik o nazwie __hbaseapp-1.0-SNAPSHOT.jar__.

    > [AZURE.NOTE] Plik __hbaseapp-1.0-SNAPSHOT.jar__ jest uber słoik (nazywane tłuszczu słoik), zawierające wszystkie zależności wymagane do uruchamiania aplikacji.

##<a name="upload-the-jar-file-and-run-jobs"></a>Przekaż plik SŁOIK i zadań

1. Przekazywanie słoju do klastrów HDInsight należy wykonać następujące kroki. Zamień **nazwę użytkownika** nazwę usługi logowania SSH. Zastąp **NAZWAKLASTRA** nazwy klaster HDInsight:

        scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Spowoduje to Przekaż plik do katalogu macierzystego dla swojego konta użytkownika SSH.

    > [AZURE.NOTE] Jeśli używasz hasła dla konta SSH pojawi o wprowadzenie hasła. Jeśli użyto klawisza SSH z kontem, może być konieczne używanie `-i` parametr, aby określić ścieżkę do pliku klucza. Poniższy przykład pobierze klucz prywatny z `~/.ssh/id_rsa`:
    >
    > `scp -i ~/.ssh/id_rsa ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

2. Nawiązywanie połączenia z klastrem HDInsight za pomocą SSH. Zamień **nazwę użytkownika** nazwę usługi logowania SSH. Zastąp **NAZWAKLASTRA** nazwy klaster HDInsight:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [AZURE.NOTE] Jeśli używasz hasła dla konta SSH pojawi o wprowadzenie hasła. Jeśli użyto klawisza SSH z kontem, może być konieczne używanie `-i` parametr, aby określić ścieżkę do pliku klucza. Poniższy przykład pobierze klucz prywatny z `~/.ssh/id_rsa`:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Po połączeniu, tworzenie nowej tabeli HBase za pomocą aplikacji Java należy wykonać następujące kroki:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable

    To będzie utworzyć nową tabelę HBase o nazwie __osoby__, a następnie wypełnij go z danymi.

4. Następnie wyszukaj adresów e-mail przechowywanych w tabeli należy wykonać następujące kroki:

        hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com

    Powinien zostać wyświetlony następujące wyniki:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

##<a name="delete-the-table"></a>Usuwanie tabeli

Po wykonaniu tych czynności z przykładem, użyj następującego polecenia z sesji programu PowerShell Azure, aby usunąć tabelę __osób__ , w tym przykładzie użyto:

    hadoop jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable

