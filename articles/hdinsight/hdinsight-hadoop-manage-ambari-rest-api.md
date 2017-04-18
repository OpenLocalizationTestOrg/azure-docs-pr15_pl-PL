<properties
   pageTitle="Monitorowanie i zarządzanie nimi za pomocą interfejsu API usługi REST Ambari Apache klastrów HDInsight | Microsoft Azure"
   description="Dowiedz się, jak za pomocą Ambari monitorowania i zarządzania systemem Linux HDInsight klastrów. W tym dokumencie dowiesz się, jak za pomocą interfejsu API usługi REST Ambari, które zostały dołączone do klastrów HDInsight."
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
   ms.date="09/20/2016"
   ms.author="larryfr"/>

#<a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Zarządzanie klastrów HDInsight za pomocą interfejsu API usługi REST Ambari

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari upraszcza zarządzanie i monitorowanie klastrze Hadoop, dostarczając ułatwia korzystanie z sieci web interfejsu użytkownika i interfejsu API usługi REST. Ambari znajduje się na klastrów systemem Linux HDInsight i służy do monitorowania klaster i wprowadzania zmian w konfiguracji. W tym dokumencie Dowiedz się podstawy pracy z interfejsu API usługi REST Ambari przez wykonywanie typowych zadań przy użyciu zwinięcie.

##<a name="prerequisites"></a>Wymagania wstępne

* [zawinięcie](http://curl.haxx.se/): zwinięcie to narzędzie i platform, który może być używany do pracy z pozostałych interfejsy API z wiersza polecenia. W tym dokumencie umożliwia komunikowanie się za pomocą interfejsu API usługi REST Ambari.
* [jq](https://stedolan.github.io/jq/): jq jest narzędziem wiersza polecenia i platform do pracy z dokumentami JSON. W tym dokumencie służy do analizowania JSON zwrócone z interfejsu API usługi REST Ambari.
* [Polecenie Azure](../xplat-cli-install.md): narzędzie wiersza polecenia i platform do pracy z usług Azure.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

##<a id="whatis"></a>Co to jest Ambari?

[Apache Ambari](http://ambari.apache.org) ułatwiają zarządzanie Hadoop prostsze, dostarczając web łatwe w obsłudze interfejs użytkownika, który może być używany do obsługi administracyjnej, zarządzanie i monitorowanie klastrów Hadoop. Deweloperów można zintegrować te funkcje aplikacji przy użyciu [Interfejsów API pozostałych Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari znajduje się domyślnie z systemem Linux HDInsight klastrów.

##<a name="rest-api"></a>INTERFEJSU API USŁUGI REST

Podstawowy identyfikator URI dla interfejsu API usługi REST Ambari na HDInsight jest https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, gdzie __NAZWAKLASTRA__ nazwę klaster.

> [AZURE.IMPORTANT] Gdy nazwa klaster w obszarze nazwy FQDN FQDN identyfikator URI (CLUSTERNAME.azurehdinsight.net) jest bez uwzględniania wielkości liter, pozostałe wystąpienia w identyfikator URI jest uwzględniana wielkość liter. Na przykład jeśli klaster ma nazwę MyCluster, Oto prawidłowe identyfikatory URI:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Następujące identyfikatory URI zwrócenie błędu, ponieważ druga wystąpienie nazwy nie jest o odpowiedniej wielkości liter.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Nawiązywanie połączenia z Ambari na HDInsight wymaga HTTPS. Po uwierzytelnieniu połączenia, należy użyć tę nazwę konta administratora (wartość domyślna to __Administrator__) i hasło, podana podczas tworzenia klaster.

W poniższym przykładzie użyto zwinięcie żądania GET, przed interfejsu API usługi REST. Zamień __hasło__ hasło administratora dla klaster. Zamień __NAZWAKLASTRA__ nazwę grupie:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Odpowiedź jest dokument JSON, który zaczyna się od informacji podobny do następującego:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Ponieważ jest to JSON, łatwiej jest pracować z danymi za pomocą parsera JSON. Na przykład, w poniższym przykładzie użyto jq mają być wyświetlane tylko `health_report` elementu.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

##<a name="example-get-the-fqdn-of-cluster-nodes"></a>Przykład: Pobieranie Kwalifikowaną węzłach

Podczas pracy z usługi HDInsight, może być konieczne ustalić w pełni kwalifikowaną nazwę domeny (FQDN) węzła klaster. Można łatwo podjąć FQDN dla różnych węzłów w klastrze za pomocą następujących czynności:

* __Węzły głowy__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Węzły pracownika__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Węzły zookeeper__:`curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Uwaga Każdy z tych przykładów postępuj zgodnie ze wzorcem same:

1. Kwerendy składnik, który jest znamy jest uruchamiany w tych węzłów.
2. Pobieranie `host_name` elementów, które zawierają nazwy FQDN dla tych węzłów.

`host_components` Element dokumentu zwrotu zawiera wiele elementów. Za pomocą `.host_components[]`, a następnie określ ścieżkę w elemencie zostanie pętli każdego elementu i wysunąć wartość z określonej ścieżki. Jeśli chcesz tylko jedną wartość, taka jak pierwszego wpisu FQDN można przywrócić elementów jako zbiór, a następnie wybierz określony wpis:

    jq '[.host_components[].HostRoles.host_name][0]'

Pierwszy FQDN zwraca wartość z kolekcji.

##<a name="example-get-the-default-storage-account-and-container"></a>Przykład: Uzyskiwanie domyślne konto miejsca do magazynowania i kontener

Po utworzeniu klaster HDInsight należy użyć konta magazynu platformy Azure i kontenera obiektów blob jako magazyn domyślny dla klaster. Za pomocą Ambari pobiera te informacje po utworzeniu klaster. Na przykład jeśli chcesz programowy Napisz danych bezpośrednio w kontenerze.

Identyfikator URI WASB magazynowania domyślne klastrów pobiera:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Zwraca to pierwszy konfiguracji zastosowane na serwerze (`service_config_version=1`,) zawierający informacje. Po pobraniu wartości, które zostały zmodyfikowane po utworzeniu klaster może być konieczne listy wersji konfiguracji i pobieranie najnowszego.

To zwraca wartość podobny do następującego przykładu, gdzie __KONTENER__ jest domyślnego kontenera, a __Nazwa konta__ jest nazwę konta magazynu platformy Azure:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Następnie można te informacje z [Polecenie Azure](../xplat-cli-install.md) przekazywanie lub pobieranie danych z kontenera.

1. Uzyskaj grupa zasobów dla konta miejsca do magazynowania. __Nazwa konta__ należy zastąpić nazwę konta magazynu pobrane z Ambari:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Zwraca to nazwa grupy zasobów dla konta.
    
    > [AZURE.NOTE] Jeśli nic się nie jest tego polecenia, może być konieczne zmienić polecenie Azure do trybu Azure Menedżera zasobów, a następnie ponownie uruchom polecenie. Aby przełączyć się do trybu Menedżer zasobów Azure, użyj następującego polecenia:
    >
    > `azure config mode arm`
    
2. Uzyskaj klucz konta miejsca do magazynowania. Zamień __grupy__ grupa zasobów w poprzednim kroku. __Nazwa konta__ należy zastąpić nazwę konta magazynu:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    W tym przykładzie zwraca klucza podstawowego dla konta.
    
3. Użyj polecenia przekazywania do zapisania pliku w kontenerze:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    Zamień nazwę konta magazynu __Nazwa konta__ . Zamień __ACCOUNTKEY__ klucz pobrane wcześniej. __Ścieżka pliku__ jest ścieżką do pliku, który chcesz przekazać, gdy __BLOBPATH__ jest ścieżkę w kontenerze.

    Na przykład, jeśli chcesz, aby plik znajdował się w HDInsight na wasbs://example/data/filename.txt, następnie __BLOBPATH__ będzie `example/data/filename.txt`.

##<a name="example-update-ambari-configuration"></a>Przykład: Ambari aktualizacji konfiguracji

1. Uzyskiwanie bieżącej konfiguracji Ambari są przechowywane jako "Konfiguracja docelowa":

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    W tym przykładzie zwraca JSON dokumentu zawierającego bieżącej konfiguracji (określone przez wartość _tagu_ ) składniki zainstalowane w klastrze. Poniższy przykład to fragment danych zwróconych przez typu klaster Spark.
    
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }

    Z tej listy, należy skopiować nazwa składnika (na przykład __spark\_thrift\_sparkconf__ i __Etykieta__ wartość.
    
2. Pobieranie konfiguracji składnika i znacznika przy użyciu następującego polecenia. Zamień __spark-thrift-sparkconf__ i __początkowej__ składnik i znacznika, który chcesz pobrać konfiguracji.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    Zwinięcie pobiera dokumentu JSON, a następnie jq służy do modyfikowania danych w celu utworzenia szablonu. Szablon następnie umożliwia dodawanie/modyfikowanie wartości konfiguracji. Specjalnie wykonuje następujące czynności:
    
    * Tworzy wartość unikatowa zawierającą ciąg "wersji" i daty, który jest przechowywany w __newtag__.
    * Tworzy dokument główny nowej konfiguracji odpowiednie.
    * Otrzymuje zawartość `.items[]` tablicy i dodaje ją w elemencie __desired_config__ .
    * Usuwa __Odwołanie__, __wersję__i elementów __konfiguracji__ , te elementy nie są potrzebne do przesyłanie nowej konfiguracji.
    * Powoduje dodanie nowego elementu __znacznika__ i ustawia wartość na __wersję ###__. Część numeryczna jest oparty na bieżącą datę. Każdy konfiguracja musi mieć unikatowy tag.
    
    Na koniec dane są zapisywane w dokumencie __newconfig.json__ . W strukturze dokumentu powinna wyglądać podobnie do następującego przykładu:
    
        {
            "Clusters": {
                "desired_config": {
                "tag": "version1459260185774265400",
                "type": "spark-thrift-sparkconf",
                "properties": {
                    ....
                 },
                 "properties_attributes": {
                     ....
                 }
            }
        }

3. Otwórz __newconfig.json__ dokumentu i modyfikowanie dodawanie wartości w obiekcie __Właściwości__ . Poniższy przykład zmienia wartość __"spark.yarn.am.memory"__ z __"1 g"__ na __"3 g"__ i i dodanie nowego elementu do __"spark.kryoserializer.buffer.max"__ z wartością __"256 m"__.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Zapisz plik po zakończeniu wprowadzania zmian.

4. Użyj następującego polecenia, aby przesłać zaktualizowaną konfigurację do Ambari.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    To polecenie przewody zawartość pliku __newconfig.json__ na żądanie zwinięcie i przesyła go do klastrów jako nowy Konfiguracja docelowa. Żądanie zwinięcie zwraca dokumentu JSON. Element __versionTag__ w tym dokumencie powinny być zgodne wersji Ciebie, a obiekt __podawać__ będzie zawierać żądanej zmiany w konfiguracji.

###<a name="example-restart-a-service-component"></a>Przykład: Ponowne uruchomienie składnika usługi

W tym momencie po wyświetleniu web Ambari interfejsu użytkownika usługi Spark będzie oznaczała, konieczne jest ponowne uruchomienie nowej konfiguracji zostały wprowadzone. Wykonaj następujące czynności, aby ponownie uruchomić usługę.

1. Włączanie trybu konserwacji usługi Spark należy wykonać następujące kroki:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    To polecenie wysyła JSON dokument na serwerze (zawarte w `echo` instrukcji) którego jest włączany tryb konserwacji.
    Możesz sprawdzić, czy usługa jest teraz w trybie konserwacji za pomocą następujące żądanie:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    Ten zwróci wartość `"ON"`.

3. Następnie należy wyłączyć usługę należy wykonać następujące kroki:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    To polecenie zwraca odpowiedź podobny do następującego.
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    `href` Wartość zwrócona przez ten identyfikator URI korzysta z wewnętrzny adres IP węzła. Aby użyć go z poza klastrem, Zamień część "10.0.0.18:8080" Kwalifikowaną klaster. Na przykład następujące polecenie pobiera stanu żądania.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Jeśli to zwraca wartość `"COMPLETED"` , a następnie żądanie zostało zakończone.

4. Po wykonaniu poprzedniego żądania uruchomienia usługi należy wykonać następujące kroki.

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Po ponownym uruchomieniu usługi, jest nowe ustawienia konfiguracji.

5. Na koniec Wyłącz tryb konserwacji należy wykonać następujące kroki.

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##<a name="next-steps"></a>Następne kroki

Aby uzyskać pełne informacje interfejsu API usługi REST zobacz [Ambari interfejsu API odwołanie w wersji 1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Niektóre funkcje Ambari jest wyłączona, jak zarządza usługa w chmurze HDInsight; na przykład dodawanie lub usuwanie hostów z klastrem i dodawanie nowych usług.
