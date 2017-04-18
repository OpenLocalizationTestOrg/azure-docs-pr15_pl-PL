<properties
pageTitle="Porty używane przez HDInsight | Azure"
description="Lista porty używane przez usługi Hadoop uruchomionych HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/03/2016"
ms.author="larryfr"/>

# <a name="ports-and-uris-used-by-hdinsight"></a>Porty i protokoły URI używane przez HDInsight

Niniejszy dokument zawiera listę portów używane przez usługi Hadoop uruchomionych dla klastrów HDInsight systemem Linux. Zawiera także informacje o porty używane do łączenia się z klastrem przy użyciu SSH.

## <a name="public-ports-vs-non-public-ports"></a>Porty publicznej a wartością publicznej porty

Systemem Linux klastrów HDInsight udostępnia tylko trzy porty publicznie w Internecie; 22, 23 i 443. Są one używane do uzyskiwania bezpiecznego dostępu do klaster przy użyciu SSH i usług uwidaczniany za pośrednictwem bezpiecznego protokołu HTTPS.

Wewnętrznie HDInsight jest zaimplementowana przez kilka Azure maszyn wirtualnych (węzłach w klastrze,) korzystania z sieci wirtualnych Azure. Z poziomu wirtualnej sieci można korzystać z portów nie przez internet. Na przykład w przypadku połączenia z jedną węzłów głowy przy użyciu SSH z węzła głównego można następnie bezpośrednio korzystać z usług uruchomionych w węzłach.

> [AZURE.IMPORTANT] Po utworzeniu klastrze HDInsight, jeśli nie określisz Azure wirtualną sieć jako opcja konfiguracji, zostanie on utworzony; jednak nie możesz dołączać do innych komputerów (na przykład pozostałych maszyn wirtualnych Azure lub komputerze rozwoju klienta) w tym automatyczne utworzenie wirtualną sieć. 

Do połączenia dodatkowych maszyn wirtualnych sieci, musisz najpierw utworzyć wirtualnej sieci, a następnie określ, tworząc klaster HDInsight. Aby uzyskać więcej informacji zobacz [możliwości rozszerzenie HDInsight przy użyciu wirtualnej sieci Azure](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Porty publicznej

Wszystkie węzły w klastrze HDInsight znajdują się w wirtualnej sieci Azure, a nie są bezpośrednio dostępne z Internetu. Brama publicznej udostępnia internet następujące porty, które są wspólne dla całej wszystkie typy klaster HDInsight.

| Usługa | Port | Protocol (protokół) | Opis |
| ---- | ---------- | -------- | ----------- | ----------- |
| sshd | 22 | SSH | Łączy klientów sshd na headnode podstawowego. Zobacz [Używanie SSH z systemem Linux HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| sshd | 22 | SSH | Łączy klientów sshd w węźle krawędzi (tylko usługa HDInsight Premium). Zobacz [Rozpoczynanie pracy z serwera R w HDInsight](hdinsight-hadoop-r-server-get-started.md) |
| sshd | 23 | SSH | Łączy klientów sshd na pomocniczym headnode. Zobacz [Używanie SSH z systemem Linux HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) |
| Ambari | 443 | PROTOKÓŁ HTTPS | Ambari interfejs użytkownika sieci web. Zobacz [Zarządzanie HDInsight za pomocą Interfejsu sieci Web Ambari](hdinsight-hadoop-manage-ambari.md) |
| Ambari | 443 | PROTOKÓŁ HTTPS | Ambari interfejsu API usługi REST. Zobacz [Zarządzanie HDInsight za pomocą interfejsu API usługi REST Ambari](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat | 443 | PROTOKÓŁ HTTPS | HCatalog interfejsu API usługi REST. Zobacz [Używanie gałęzi z zwinięcie](hdinsight-hadoop-use-pig-curl.md), [za pomocą świnka z zwinięcie](hdinsight-hadoop-use-pig-curl.md), [za pomocą MapReduce z zwinięcie](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 | 443 | ODBC | Łączy do gałęzi przy użyciu interfejsu ODBC. Zobacz [Połączyć program Excel z usługą hdinsight za pomocą sterownika ODBC firmy Microsoft](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 | 443 | JDBC | Łączy do gałęzi przy użyciu JDBC. Zobacz [Łączenie z gałęzi na HDInsight za pomocą sterownika gałęzi JDBC](hdinsight-connect-hive-jdbc-driver.md) |

Poniżej przedstawiono dostępne dla klastrów określonych typów:

| Usługa | Port | Protocol (protokół) |Typ klaster | Opis |
| ------------ | ---- |  ----------- | --- | ----------- |
| Stargate | 443 | PROTOKÓŁ HTTPS | HBase | HBase interfejsu API usługi REST. Zobacz [Rozpoczynanie pracy z HBase](hdinsight-hbase-tutorial-get-started-linux.md) |
| Livy | 443 | PROTOKÓŁ HTTPS |  Spark | Interfejsu API usługi REST Spark. Zobacz [zadania przesyłanie Spark zdalnie przy użyciu Livy](hdinsight-apache-spark-livy-rest-interface.md) |
| Burza | 443 | PROTOKÓŁ HTTPS | Burza | Burza interfejs użytkownika sieci web. Zobacz [rozmieszczanie i zarządzanie nimi topologii Burza na HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="authentication"></a>Uwierzytelnianie

Muszą zostać uwierzytelnione wszystkich usług publicznie dostępne w Internecie:

| Port | Poświadczenia |
| ---- | ----------- |
| 22 lub 23 | Poświadczenia użytkownika SSH określone podczas tworzenia klaster |
| 443 | Nazwa logowania (domyślny: Administrator) i hasło, które zostały ustawione podczas tworzenia klaster |

## <a name="non-public-ports"></a>Porty niezwiązaną publicznej

> [AZURE.NOTE] Niektóre usługi są dostępne tylko na klaster określonych typów. Na przykład HBase jest dostępna tylko na typy klastrów HBase.

### <a name="hdfs-ports"></a>Porty HDFS

| Usługa | Węzłów | Port | Protocol (protokół) | Opis |
| ------- | ------- | ---- | -------- | ----------- | 
| NameNode interfejs użytkownika sieci web | Węzły głowy | 30070 | PROTOKÓŁ HTTPS | Interfejs użytkownika, aby wyświetlić bieżący stan sieci Web |
| Usługa metadanych NameNode | węzły głowy | 8020 | IPC | Metadane systemu plików 
| DataNode | Wszystkie węzły pracownika | 30075 | PROTOKÓŁ HTTPS | Interfejs użytkownika sieci Web do Wyświetl stan, dzienniki itp. |
| DataNode | Wszystkie węzły pracownika | 30010 | &nbsp; | Transfer danych |
| DataNode | Wszystkie węzły pracownika | 30020 | IPC | Operacje metadanych |
| NameNode pomocniczej | Węzły głowy | 50090 | HTTP | Punkt kontrolny NameNode metadanych |

### <a name="yarn-ports"></a>Porty PRZĘDZY

| Usługa | Węzłów | Port | Protocol (protokół) | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| Menedżer zasobów interfejs użytkownika sieci web | Węzły głowy | 8088 | HTTP | Interfejs użytkownika dla Menedżera zasobów sieci Web |
| Menedżer zasobów interfejs użytkownika sieci web | Węzły głowy | 8090 | PROTOKÓŁ HTTPS | Interfejs użytkownika dla Menedżera zasobów sieci Web |
| Interfejsu administracyjnego Menedżera zasobów | węzły głowy | 8141 | IPC | Dla aplikacji przesyłania (gałęzi, serwer gałęzi, świnka, itp.) |
| Harmonogram Menedżera zasobów | węzły głowy | 8030 | HTTP | Interfejsu administracyjnego |
| Interfejs Menedżera zasobów | węzły głowy | 8050 | HTTP |Adres interfejsu Menedżera aplikacji |
| NodeManager | Wszystkie węzły pracownika | 30050 | &nbsp; | Adres menedżera kontenera |
| NodeManager interfejs użytkownika sieci web | Wszystkie węzły pracownika | 30060 | HTTP | Interfejs Menedżera zasobów |
| Adres osi czasu | Węzły głowy | 10200 | RPC | RPC usługi osi czasu. |
| Interfejs użytkownika sieci web osi czasu | Węzły głowy | 8181 | HTTP | Interfejs użytkownika sieci web usługi osi czasu |

### <a name="hive-ports"></a>Porty gałęzi

| Usługa | Węzłów | Port | Protocol (protokół) | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | Węzły głowy | 10001 | Thrift | Usługa programowy nawiązywania połączenia gałęzi (Thrift-JDBC) |
| HiveServer | Węzły głowy | 10000 | Thrift | Usługa programowy nawiązywania połączenia gałęzi (Thrift-JDBC) |
| Gałąź Metastore | Węzły głowy | 9083 | Thrift | Usługa programowy nawiązywania połączenia gałęzi metadanych (Thrift-JDBC) |

### <a name="webhcat-ports"></a>Porty WebHCat

| Usługa | Węzłów | Port | Protocol (protokół) | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| Serwer WebHCat | Węzły głowy | 30111 | HTTP | Interfejsu API sieci Web u góry HCatalog i inne usługi Hadoop |

### <a name="mapreduce-ports"></a>Porty MapReduce

| Usługa | Węzłów | Port | Protocol (protokół) | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | Węzły głowy | 19888 | HTTP | MapReduce JobHistory interfejs użytkownika sieci web |
| JobHistory | Węzły głowy | 10020 | &nbsp; | Serwer MapReduce JobHistory |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | Mapa pośrednie przeniesienia Wyświetla z żądaniem reduktory |

### <a name="oozie"></a>Oozie

| Usługa | Węzłów | Port | Protocol (protokół) | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| Serwer Oozie | Węzły głowy | 11000 | HTTP | Adres URL usługi Oozie |
| Serwer Oozie | Węzły głowy | 11001 | HTTP | Port dla administratorów Oozie |

### <a name="ambari-metrics"></a>Metryki Ambari

| Usługa | Węzłów | Port | Protocol (protokół) | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| Oś czasu (Historia aplikacji) | Węzły głowy | 6188 | HTTP | Interfejs użytkownika sieci web usługi osi czasu |
| Oś czasu (Historia aplikacji) | Węzły głowy | 30200 | RPC | Interfejs użytkownika sieci web usługi osi czasu |

### <a name="hbase-ports"></a>Porty HBase

| Usługa | Węzłów | Port | Protocol (protokół) | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | Węzły głowy | 16000 | &nbsp; | &nbsp; |
| Informacje o HMaster interfejs sieci Web | Węzły głowy | 16010 | HTTP | Port dla sieci web wzorzec HBase interfejsu użytkownika |
| Serwer regionu | Wszystkie węzły pracownika | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | Portu, który umożliwia nawiązywanie połączenia z ZooKeeper klientów |

### <a name="kafka-ports"></a>Porty Kafka

| Usługa | Węzłów | Port | Protocol (protokół) | Opis |
| ------- | ------- | ---- | -------- | ----------- |
| Broker  | Węzły pracownika | 9092 | [Protokół Kafka](http://kafka.apache.org/protocol.html) | Na potrzeby komunikacji z klientami |
| &nbsp; | Węzły zookeeper | 2181 | &nbsp; | Portu, który umożliwia nawiązywanie połączenia z Zookeeper klientów |
