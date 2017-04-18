<properties
    pageTitle="Monitorowanie klastrów Hadoop w HDInsight za pomocą interfejsu API Ambari | Microsoft Azure"
    description="Używanie interfejsów API Ambari Apache do tworzenia, zarządzania i monitorowania klastrów Hadoop. Operator intuicyjny narzędzia oraz interfejsy API ukrycie złożoności Hadoop."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Monitorowanie klastrów Hadoop w HDInsight za pomocą interfejsu API Ambari

Dowiedz się, jak klastrów HDInsight można monitorować za pomocą interfejsów API Ambari.

> [AZURE.NOTE] Informacje w tym artykule są przede wszystkim dla klastrów HDInsight systemu Windows, które zapewniają kopię interfejsu API usługi REST Ambari tylko do odczytu. W przypadku klastrów z systemem Linux zobacz [klastrów zarządzanie Hadoop przy użyciu Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Co to jest Ambari?

[Apache Ambari] [ ambari-home] jest używana do obsługi administracyjnej, zarządzania i monitorowania klastrów Apache Hadoop. Zawiera intuicyjny zbiór operator narzędzia i zestaw interfejsów API ukrycie złożoności Hadoop upraszczanie operacji klastrów. Aby uzyskać więcej informacji na temat interfejsów API, zobacz [Informacje dotyczące interfejsu API Ambari][ambari-api-reference]. 

Usługa HDInsight obecnie obsługuje tylko funkcja monitorowania Ambari. Ambari 1.0 interfejsu API jest obsługiwany przez klastrów w wersji 3.0 i 2.1 HDInsight. W tym artykule opisano, jak uzyskiwanie dostępu do API Ambari na HDInsight wersji 3.1 i 2.1 klastrów. Kluczowe różnicę między nimi jest niektóre składniki zostały zmienione z wprowadzeniem nowe funkcje (na przykład serwer historii zadań). 

**Wymagania wstępne**

Przed rozpoczęciem tego samouczka, musisz mieć następujące czynności:

- **Pracy z programem PowerShell Azure**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- (Opcjonalnie) [zawinięcie] [curl]. Aby go zainstalować, zobacz [zawinięcie wersjach i pliki do pobrania][curl-download].

    >[AZURE.NOTE] Kiedy należy używać polecenia zwinięcie w systemie Windows, użyj podwójny cudzysłów zamiast pojedyncze cudzysłowy dla wartości opcji.

- **Klaster HDInsight Azure**. Aby uzyskać instrukcje dotyczące klaster inicjowania obsługi administracyjnej, zobacz [Wprowadzenie do korzystania z usługi HDInsight] [ hdinsight-get-started] lub [klastrów świadczenia usługi HDInsight][hdinsight-provision]. Potrzebne są następujące dane przez samouczka:

    Właściwość klaster|Nazwa zmiennej Azure programu PowerShell|Wartość|Opis
    ---|---|---|---
    Nazwa klaster HDInsight|$clusterName||Nazwa klaster HDInsight.
    Klaster nazwy użytkownika|$clusterUsername||Nazwa użytkownika klaster określonego utworzenia klaster.
    Klaster hasła|$clusterPassword||Klaster hasła użytkownika.

    >[AZURE.NOTE] Wypełnianie wartości w tabeli. Jest to pomocne przy przechodzące przez tego samouczka.

## <a name="jump-start"></a>Przechodzenie do początku

Istnieje kilka sposobów monitorowania klastrów HDInsight za pomocą Ambari.

**Za pomocą programu PowerShell Azure**

Oto skrypt programu Azure PowerShell uzyskanie informacji o śledzeniu zadania MapReduce *w klastrze HDInsight 3.1.*  Kluczowe różnica polega na tym, że firma Microsoft pobierają te szczegółów usługi PRZĘDZY (zamiast MapReduce).

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Oto skrypt programu PowerShell Azure uzyskiwania MapReduce zadania śledzenie informacji *w klastrze HDInsight 2.1*:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Wynik to:

![Wynik Jobtracker][img-jobtracker-output]

**Zwinięcie za pomocą**

Oto przykład uzyskiwanie informacji na temat klaster przy użyciu zwinięcie:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Wynik to:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**10-8-2014 r Zwolnij**:

Podczas korzystania z punkt końcowy Ambari "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", polu *host_name* zwraca w pełni kwalifikowaną nazwę domeny (FQDN) węzła zamiast nazwa hosta. Przed wydaniem 2014-10-8 w tym przykładzie zwracany po prostu "**headnode0**". Po zwolnieniu 2014-10-8 zostanie wyświetlony Kwalifikowaną "**headnode0. {} .Azurehdinsight ClusterDNS} .net**", jak pokazano w poprzednim przykładzie. Ta zmiana została wymagane do ułatwienia scenariusze, w którym wielu typów klaster (na przykład HBase i Hadoop) mogą być rozmieszczone w jednej sieci wirtualnych (VNET). Dzieje się tak, na przykład, używając HBase jako platformy wewnętrznej dla Hadoop.

## <a name="ambari-monitoring-apis"></a>Monitorowanie interfejsy API Ambari

W poniższej tabeli wymieniono niektóre z najczęściej używanych Ambari, monitorowanie interfejsu API. Aby uzyskać więcej informacji na temat interfejsu API, zobacz [Informacje dotyczące interfejsu API Ambari][ambari-api-reference].

Interfejs API monitora połączenia|IDENTYFIKATOR URI|Opis
---|---|---
Uzyskiwanie klastrów|`/api/v1/clusters`|
Uzyskaj informacje klaster.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|klastrów, usługami, hosts
Dostęp do usług|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|Usługi obejmują: hdfs, mapreduce
Uzyskiwanie informacji o usługach.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
Uzyskaj składniki usługi|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|HDFS: namenode, datanode<br/>MapReduce: jobtracker; tasktracker
Uzyskaj informacje składnik.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo hosta składników, metryki
Uzyskiwanie hosts|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0, workernode0
Uzyskaj informacje hosta.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
Uzyskaj składniki hosta|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|namenode, resourcemanager
Uzyskaj informacje składnik hosta.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles składnik, hosta, metryki
Uzyskiwanie konfiguracji|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|Typy konfiguracji: core witryny, witryny hdfs, mapred witryny, gałęzi witryny
Uzyskaj informacje o konfiguracji.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|Typy konfiguracji: core witryny, witryny hdfs, mapred witryny, gałęzi witryny


##<a name="next-steps"></a>Następne kroki

Teraz zapoznaniu umożliwia Ambari monitorowania interfejsu API. Aby uzyskać więcej informacji, zobacz:

- [Zarządzanie klastrów HDInsight za pomocą portalu Azure][hdinsight-admin-portal]
- [Zarządzanie klastrów HDInsight przy użyciu programu PowerShell Azure][hdinsight-admin-powershell]
- [Zarządzanie klastrów HDInsight przy użyciu interfejsu wiersza polecenia][hdinsight-admin-cli]
- [Dokumentacja HDInsight][hdinsight-documentation]
- [Wprowadzenie do usługi HDInsight][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
