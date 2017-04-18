<properties
   pageTitle="Porady dotyczące korzystania z Hadoop na podstawie Linux HDInsight | Microsoft Azure"
   description="Implementacja porady dotyczące korzystania z systemem Linux HDInsight (Hadoop) klastrów w znanym środowisku Linux uruchomiony w chmurze Azure."
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
   ms.date="09/13/2016"
   ms.author="larryfr"/>

# <a name="information-about-using-hdinsight-on-linux"></a>Informacje o korzystaniu z usługi HDInsight na Linux

Systemem Linux klastrów Azure HDInsight umożliwiają Hadoop znanym środowisku Linux uruchomiony w chmurze Azure. Dla większości zadań powinna działać dokładnie jako inna instalacja Hadoop na Linux. Ten dokument wymaga określonych różnic, których należy pamiętać.

##<a name="prerequisites"></a>Wymagania wstępne

Wiele z tych kroków w tym dokumencie użyć następujących narzędzi, które mogą musi być zainstalowany na komputerze.

* [zawinięcie](https://curl.haxx.se/) — umożliwia komunikowanie się za pomocą usług sieci web
* [jq](https://stedolan.github.io/jq/) - używany do analizowania JSON dokumentów
* [Polecenie azure](../xplat-cli-install.md) — umożliwia zdalne zarządzanie usług Azure

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

## <a name="domain-names"></a>Nazwy domen

Jest w pełni kwalifikowaną nazwę domeny (FQDN) do użycia podczas łączenia się z klastrem z Internetu ** &lt;NazwaKlastra >. azurehdinsight.net** lub (w przypadku tylko SSH) ** &lt;NazwaKlastra-ssh >. azurehdinsight.net**.

Wewnętrznie każdy węzeł w klastrze ma nazwę przypisany podczas konfigurowania klaster. Aby znaleźć nazwy klaster, odwiedź stronę __Hosts__ w Interfejsie użytkownika witryny sieci Web Ambari lub zwraca listę hostów z interfejsu API usługi REST Ambari należy wykonać następujące kroki:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Zamień __hasło__ hasło konta administratora i __NAZWAKLASTRA__ o nazwie klaster. Spowoduje to przywrócenie JSON dokumentu, który zawiera listę hostów w klastrze, a następnie jq wysunięcie `host_name` wartości elementu dla każdego hosta w klastrze.

Jeśli potrzebujesz znaleźć nazwę węzła określonej usługi, możesz wyszukiwać Ambari tego składnika. Na przykład aby znaleźć hostów węzeł nazwę HDFS, należy użyć.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

To zwraca JSON dokument opisujący usługę, a następnie jq wysunięcie tylko `host_name` wartość hostów.

## <a name="remote-access-to-services"></a>Zdalny dostęp do usług

* **Ambari (sieć web)** - https://&lt;NazwaKlastra >. azurehdinsight.net

    Uwierzytelnianie przy użyciu klaster administratora użytkownika i hasło, a następnie zaloguj się do Ambari. Używa też klaster administratora użytkownika i hasło.

    Uwierzytelnianie jest zwykły tekst — zawsze używaj HTTPS, aby upewnić się, że połączenie jest bezpieczne.

    > [AZURE.IMPORTANT] Podczas Ambari dla klaster jest dostępna bezpośrednio w Internecie, niektóre funkcje oparte na uzyskiwanie dostępu do węzłów według nazwy domeny wewnętrznej używana przez klaster. Ponieważ jest to nazwa domeny wewnętrznej, a nie publicznego, otrzymasz błędy "nie można odnaleźć serwera" podczas próby dostępu do niektórych funkcji w Internecie.
    >
    > Aby korzystać ze wszystkich funkcji programu web Ambari interfejsu użytkownika, wykonaj tunelem SSH do serwera proxy ruchu w sieci web do węzła głowy. Zobacz [Używanie SSH Tunneling dostęp do sieci web Ambari interfejsu użytkownika, ResourceManager, JobHistory, NameNode, Oozie i inne osoby interfejs użytkownika w sieci web](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** - https://&lt;NazwaKlastra >.azurehdinsight.net/ambari

    > [AZURE.NOTE] Uwierzytelnianie za pomocą klaster administratora użytkownika i hasła.
    >
    > Uwierzytelnianie jest zwykły tekst — zawsze używaj HTTPS, aby upewnić się, że połączenie jest bezpieczne.

* **WebHCat (Templeton)** - https://&lt;NazwaKlastra >.azurehdinsight.net/templeton

    > [AZURE.NOTE] Uwierzytelnianie za pomocą klaster administratora użytkownika i hasła.
    >
    > Uwierzytelnianie jest zwykły tekst — zawsze używaj HTTPS, aby upewnić się, że połączenie jest bezpieczne.

* **SSH** - &lt;NazwaKlastra >-ssh.azurehdinsight.net na porcie 22 lub 23. Port 22 jest używany do łączenia z podstawowego headnode, podczas gdy 23 jest używane do łączenia się pomocniczej. Aby uzyskać więcej informacji w węzłach głowy zobacz [dostępności i niezawodności klastrów Hadoop w HDInsight](hdinsight-high-availability-linux.md).

    > [AZURE.NOTE] Dostępne są tylko głowy węzłach za pośrednictwem SSH na komputerze klienckim. Po połączeniu, następnie dostępne węzły pracownik przy użyciu SSH od headnode.

## <a name="file-locations"></a>Lokalizacje plików

Pliki związane z Hadoop można znaleźć w węzłach w `/usr/hdp`. Ten katalog zawiera podkatalogów następujące czynności:

* __2.2.4.9-1__: ten katalog ma nazwę dla wersji platformy danych Hortonworks używany przez HDInsight, więc liczba w klastrze mogą być inne niż wymienione tutaj.
* __bieżący__: ten katalog zawiera łącza do katalogów w katalogu __2.2.4.9-1__ i istnieje tak, aby nie trzeba wpisywać numer wersji (który może zostać zmieniony,) za każdym razem, gdy chce dostępu do pliku.

Przykładowe dane i pliki JAR można znaleźć pliku usługi Hadoop Distributed System (HDFS) lub obiektów Blob platformy Azure ilość miejsca do magazynowania w "-przykład" lub "wasbs: / / / przykład".

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>HDFS, magazyn obiektów Blob platformy Azure i miejsca do magazynowania najważniejsze wskazówki

W większości dystrybucji Hadoop HDFS jest przechowywana w magazynu lokalnego na komputerach w klastrze. Gdy jest to efektywne, może być kosztów dla rozwiązania opartego na chmurze miejsce, w którym są naliczane co godzinę lub minuty dla zasobów obliczeń.

Usługa HDInsight używa magazyn obiektów Blob platformy Azure jako domyślnego magazynu, który zapewnia następujące korzyści:

* Tani długotrwałego przechowywania

* Ułatwienia dostępu z usług zewnętrznych, takich jak witryny sieci Web, narzędzia przekazywania i pobierania plików, różne SDK języka i przeglądarki sieci web

Ponieważ jest domyślnego magazynu dla usługi HDInsight, zwykle nie trzeba coś robić, aby go używać. Na przykład następujące polecenie wyświetli listę plików w folderze **/example/data** , który jest przechowywany w magazynie obiektów Blob platformy Azure:

    hdfs dfs -ls /example/data

Niektóre polecenia może być konieczna określić, że korzystasz z magazynem obiektów Blob. Dla tych, można prefiks polecenie z **wasb: / /**, lub **wasbs: / /**.

Usługa HDInsight umożliwia także skojarzyć wiele kont magazyn obiektów Blob klastrze. Aby uzyskać dostęp do danych na koncie magazyn obiektów Blob inny niż domyślny, można użyć formatu **wasbs: / /&lt;container-name>@&lt;nazwa konta >.blob.core.windows.net/**. Na przykład poniższa zostanie wyświetlona zawartość katalogu **/example/data** określonym kontenerze i konta magazyn obiektów Blob:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>Jakie magazyn obiektów Blob jest klaster za pomocą?

Podczas tworzenia klaster zaznaczone korzystanie z istniejącego konta magazynu platformy Azure i kontener lub Utwórz nowy. Następnie prawdopodobnie pamiętasz związanych z nim. Za pomocą interfejsu API usługi REST Ambari można znaleźć domyślnego konta miejsca do magazynowania i kontener.

1. Użyj następującego polecenia do pobrania informacji o konfiguracji HDFS przy użyciu zwinięcie i filtrować przy użyciu [jq](https://stedolan.github.io/jq/):

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
    > [AZURE.NOTE] Spowoduje to przywrócenie pierwszej konfiguracji zastosowane na serwerze (`service_config_version=1`,) zawierający informacje. Jeśli są pobierane wartości, które zostały zmodyfikowane po utworzeniu klaster, może być konieczne listy wersji konfiguracji i pobieranie najnowszego.

    Zwróci podobny do następującego, gdzie __KONTENER__ jest domyślnego kontenera, a __Nazwa konta__ jest nazwę konta magazynu platformy Azure wartość:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

1. Uzyskać grupa zasobów dla konta miejsca do magazynowania, za pomocą [Interfejsu wiersza polecenia Azure](../xplat-cli-install.md). W następującym polecenia Zastąp __Nazwa konta__ nazwę konta magazynu pobrane z Ambari:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Spowoduje to przywrócenie Nazwa grupy zasobów dla konta.
    
    > [AZURE.NOTE] Jeśli nic się nie jest tego polecenia, może być konieczne zmienić polecenie Azure do trybu Azure Menedżera zasobów, a następnie ponownie uruchom polecenie. Aby przełączyć się do trybu Menedżera zasobów Azure, użyj następującego polecenia.
    >
    > `azure config mode arm`
    
2. Uzyskaj klucz konta miejsca do magazynowania. Zamień __grupy__ grupa zasobów w poprzednim kroku. __Nazwa konta__ należy zastąpić nazwę konta magazynu:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.[0].value'

    Spowoduje to przywrócenie klucza podstawowego dla konta.

Można również znaleźć informacje miejsca do magazynowania przy użyciu Azure Portal:

1. W [Azure Portal](https://portal.azure.com/)wybierz klaster HDInsight.

2. W sekcji __Essentials__ zaznacz __wszystkie ustawienia__.

3. W obszarze __Ustawienia__zaznacz __Azure miejsca do magazynowania klawiszy__.

4. Z __Klawiszy miejsca do magazynowania Azure__, wybierz jedną z kont miejsca do magazynowania na liście. Spowoduje to wyświetlenie informacji o koncie miejsca do magazynowania.

5. Wybierz ikonę klucza. Spowoduje to wyświetlenie klawiszy dla tego konta miejsca do magazynowania.

### <a name="how-do-i-access-blob-storage"></a>Jak uzyskać dostęp do magazyn obiektów Blob

Inne niż za pomocą polecenia Hadoop w grupie, są dostępne na różne sposoby, aby uzyskać dostęp do obiektów blob:

* [Polecenie Azure dla komputerów Mac, Linux i systemu Windows](../xplat-cli-install.md): interfejs wiersza polecenia polecenia dotyczące pracy z Azure. Po zainstalowaniu należy użyć `azure storage` polecenia, aby uzyskać pomoc na temat korzystania z miejscem do magazynowania, lub `azure blob` dla poleceń dotyczących obiektów blob.

* [blobxfer.PY](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): python skrypt do pracy z obiektami BLOB w magazynie Azure.

* Różne zestawy SDK:

    * [Java](https://github.com/Azure/azure-sdk-for-java)

    * [Node.js](https://github.com/Azure/azure-sdk-for-node)

    * [PHP](https://github.com/Azure/azure-sdk-for-php)

    * [Python](https://github.com/Azure/azure-sdk-for-python)

    * [Dopiskiem](https://github.com/Azure/azure-sdk-for-ruby)

    * [.NET](https://github.com/Azure/azure-sdk-for-net)

* [Interfejsu API usługi REST miejsca do magazynowania](https://msdn.microsoft.com/library/azure/dd135733.aspx)

##<a name="scaling"></a>Skalowanie klaster

Klaster skalowania funkcji umożliwia zmianę liczby węzłów dane używane przez klaster, na którym działa usługa Azure HDInsight bez konieczności usunąć i ponownie utworzyć klaster.

Operacje skalowania podczas inne zadania można wykonywać lub procesów uruchomionych w klastrze.

Typy różnych klastrów dotyczy skalowania w następujący sposób:

* __Hadoop__: gdy zmniejszania liczby węzłów w klastrze, niektóre z tych usług w klastrze ponownego uruchomienia. Może to powodować zadania uruchomione lub oczekujące kończy się niepowodzeniem po zakończeniu operacji skalowania. Po zakończeniu operacji można przesłać ponownie zadania.

* __HBase__: serwery regionalne są automatycznie zbilansowane za kilka minut po zakończeniu operacji skalowania. Aby ręcznie saldo serwery regionalne, wykonaj następujące czynności:

    1. Nawiązywanie połączenia z klastrem HDInsight przy użyciu SSH. Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz jeden z następujących dokumentów:

        * [Używanie SSH z usługi HDInsight z Linux, Unix lub systemu Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

        * [Używanie SSH z usługi HDInsight systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    1. Rozpoczynanie powłoki HBase należy wykonać następujące kroki:

            hbase shell

    2. Po załadowaniu powłoki HBase, ręcznie saldo serwery regionalne należy wykonać następujące kroki:

            balancer

* __Burza__: należy wyrównać wszystkie uruchomione topologii Burza po wykonaniu operacji skalowania. Dzięki temu topologii do Dopasuj ustawienia równoległości na podstawie nowych liczby węzłów w klastrze. Aby wyrównać uruchomionego topologii, użyj jednej z następujących opcji:

    * __SSH__: nawiązywanie połączenia z serwerem i użyj następującego polecenia, aby wyrównać topologii:

            storm rebalance TOPOLOGYNAME

        Można również określić parametry zastępowania wskazówki równoległości pierwotnie dostarczony przez topologii. Na przykład `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` będzie skonfigurowanie topologii 5 procesy, 3 testamentu dla składnika dziobek niebieski i 10 testamentu dla składnika błyskawicy żółty.

    * __Interfejs użytkownika Burza__: wykonaj następujące czynności, aby wyrównać topologii przy użyciu interfejsu użytkownika Burza.

        1. Otwórz __https://CLUSTERNAME.azurehdinsight.net/stormui__ w przeglądarce sieci web, w którym NAZWAKLASTRA to nazwa klaster Burza. Jeśli zostanie wyświetlony monit, wprowadź nazwę administratora (Administrator) klaster HDInsight i hasło, których określone podczas tworzenia klaster.

        3. Wybierz pozycję topologii, które chcesz wyrównać, a następnie wybierz przycisk __wyrównać__ . Wprowadź opóźnienie przed wykonaniem operacji ponownego równoważenia.

Aby uzyskać szczegółowe informacje na skalowanie klaster HDInsight zobacz:

* [Zarządzanie klastrów Hadoop w HDInsight za pomocą Azure Portal](hdinsight-administer-use-portal-linux.md#scaling)

* [Zarządzanie klastrów Hadoop w HDinsight przy użyciu programu PowerShell Azure](hdinsight-administer-use-command-line.md#scaling)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Jak zainstalować odcień (lub innego składnika Hadoop)?

Usługa HDInsight jest zarządzane usługi, co oznacza, że węzły w klastrze mogą być zniszczone i automatycznie reprovisioned przez Azure, jeśli zostanie wykryty problem. Z tego powodu nie zaleca się ręcznie zainstalować elementów bezpośrednio w węzłach. Należy użyć [Akcji skryptu HDInsight](hdinsight-hadoop-customize-cluster.md) , gdy trzeba zainstalować następujące elementy:

* Usługa lub witrynie sieci web, takiej jak Spark lub odcień.
* Składnik, który wymaga zmiany konfiguracji na wielu węzłach w klastrze. Na przykład zmiennej środowiska wymagane, tworzenie katalog rejestrowania lub utworzenie pliku konfiguracji.

Akcje skryptu są skrypty imprezie, które są uruchomione podczas inicjowania obsługi administracyjnej klaster i służy do instalowania i konfigurowania dodatkowe składniki w klastrze. Przykładowe skrypty są dostępne do zainstalowania następujących składników:

* [Odcień](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Aby uzyskać informacje dotyczące tworzenia własnych skryptów akcji zobacz [opracowywania skrypt akcji z usługi HDInsight](hdinsight-hadoop-script-actions-linux.md).

###<a name="jar-files"></a>Pliki JAR

Niektóre technologie Hadoop znajdują się w pliki samodzielne słoik, zawierające funkcje używane w ramach zadania MapReduce lub z wewnątrz świnka lub gałęzi. Gdy mogą być instalowane za pomocą skryptu akcje, ich często nie wymagają wszystkie ustawienia i można tylko przekazane do klaster po zainicjowaniu obsługi administracyjnej, a używane bezpośrednio. Jeśli chcesz makle się, że składnik przeżyje reimaging z klastrem słoik plik zostanie zapisany w WASB.

Na przykład jeśli chcesz używać najnowszej wersji [DataFu](http://datafu.incubator.apache.org/), można pobrać słoik zawierającego projekt i przekaż go do klastrów HDInsight. Następnie postępuj zgodnie z dokumentacją DataFu na jak z niego korzystać z świnka lub gałęzi.

> [AZURE.IMPORTANT] Niektóre składniki są pliki jar autonomicznego są dostarczane z usługi HDInsight, ale nie są w ścieżce. Jeśli szukasz określonego składnika umożliwiają obserwowanego za pomocą funkcji wyszukiwania w klastrze:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Zwraca ścieżkę pasujące pliki jar.

Jeśli klaster zawiera już wersję składnika jako autonomicznego pliku słoik, ale chcesz użyć innej wersji, możesz przekazać nową wersję składnika z klastrem i spróbuj użyć go w zadań.

> [AZURE.WARNING] Składniki dostarczony z klastrem HDInsight są w pełni obsługiwane i Microsoft Support pomogą izolowanie i rozwiązywanie problemów związanych z tych składników.
>
> Niestandardowe składniki otrzymają komercyjnego rozsądne pomocy technicznej, aby pomóc rozwiązać ten problem. Może to spowodować w rozwiązaniu problemu lub pytaniem nawiązanie dostępnych kanałów technologiami Otwórz źródło miejsce, w którym znajduje się głębokości specjalizacji tej technologii. Na przykład, istnieje wiele witryn społeczności, których można używać, takich jak: [forum w witrynie MSDN HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Projekty Apache mieć także witryn projektów na [http://apache.org](http://apache.org), na przykład: [Hadoop](http://hadoop.apache.org/), [iskrowy](http://spark.apache.org/).

## <a name="next-steps"></a>Następne kroki

* [Migrowanie z usługi HDInsight systemu Windows do systemem Linux](hdinsight-migrate-from-windows-to-linux.md)
* [Gałąź za pomocą usługi HDInsight](hdinsight-use-hive.md)
* [Świnka korzystanie z usługi HDInsight](hdinsight-use-pig.md)
* [MapReduce zadań za pomocą usługi HDInsight](hdinsight-use-mapreduce.md)
