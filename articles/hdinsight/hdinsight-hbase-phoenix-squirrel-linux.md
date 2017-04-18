<properties 
   pageTitle="Używanie Apache Phoenix i SQuirreL w HDInsight | Microsoft Azure" 
   description="Dowiedz się, jak używać Apache Phoenix w HDInsight i sposobu instalowania i konfigurowania SQuirreL w miejscu pracy, aby nawiązać połączenie z klastrem HBase w HDInsight." 
   services="hdinsight" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Używanie Apache Phoenix z systemem Linux HBase klastrów w HDInsight  

Dowiedz się, jak używać [Apache Phoenix](http://phoenix.apache.org/) w HDInsight i jak używać funkcji SQLLine. Aby uzyskać więcej informacji na temat Olsztyn zobacz [Phoenix w ciągu 15 minut lub mniej](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Aby uzyskać gramatyki Phoenix zobacz [Phoenix gramatyki](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Aby uzyskać informacje o wersji Phoenix w HDInsight, zobacz [Co nowego w wersji klaster Hadoop dostarczony przez HDInsight?] [hdinsight-versions].

##<a name="use-sqlline"></a>Za pomocą SQLLine
[SQLLine](http://sqlline.sourceforge.net/) to narzędzie wiersza polecenia do wykonywania SQL. 

###<a name="prerequisites"></a>Wymagania wstępne
Zanim będzie można używać SQLLine, musisz mieć następujące czynności:

- **Klaster HBase w HDInsight**. Aby uzyskać informacje na świadczenia HBase klaster, zobacz temat [wprowadzenie Apache HBase w HDInsight][hdinsight-hbase-get-started].
- **Nawiązywanie połączenia z klastrem HBase za pośrednictwem protokołu pulpitu zdalnego**. Aby uzyskać instrukcje, zobacz [Zarządzanie Hadoop klastrów w HDInsight za pomocą portalu klasyczny Azure][hdinsight-manage-portal].


Podczas łączenia się z klastrem HBase, musisz połączyć się z jedną opiekunowie. Każdy klaster HDInsight ma opiekunowie 3. 

**Aby dowiedzieć się, Zookeeper nazwa hosta**

1. Przeglądaj, aby otworzyć Ambari **https://<ClusterName>. azurehdinsight.net**.
2. Wprowadź nazwę użytkownika HTTP (klaster) i hasło logowania.
3. Kliknij pozycję **ZooKeeper** z menu po lewej stronie. Zapewniają 3 **ZooKeeper Server** na liście.
4. Kliknij jeden z **Serwera ZooKeeper** na liście. W okienku podsumowania Znajdź **Nazwa hosta**. Jest podobne do *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Aby użyć SQLLine**

1. Nawiązywanie połączenia z klastrem przy użyciu SSH. Aby uzyskać instrukcje zobacz [Użyj SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md) lub [Użyj SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md) w zależności od na komputerze klienckim systemu operacyjnego.

2. Z SSH uruchom następujące polecenia do uruchamiania SQLLine:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ClusterName>:2181:/hbase-unsecure

2. Uruchom następujące polecenia w celu utworzenia tabeli HBase i wstawianie danych:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
    
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;
        
        !quit

Aby uzyskać więcej informacji zobacz [Ręczne SQLLine](http://sqlline.sourceforge.net/#manual) i [Phoenix gramatyki](http://phoenix.apache.org/language/index.html).


 
##<a name="next-steps"></a>Następne kroki
W tym artykule zapoznaniu używać Apache Phoenix w HDInsight.  Aby uzyskać więcej informacji, zobacz

- [Omówienie HDInsight HBase][hdinsight-hbase-overview]: HBase jest Apache, Otwórz źródło NoSQL baza danych oparty na Hadoop zawierającego dostępie i spójność znaczący dla dużych ilości danych niestrukturalne i semistrukturalne.
- [Inicjowanie obsługi klastrów HBase w wirtualnej sieci Azure][hdinsight-hbase-provision-vnet]: Z integracji wirtualną sieć klastrów HBase może zostać rozmieszczony na tej samej sieci wirtualnej aplikacji tak, aby aplikacje mogą komunikować się z HBase bezpośrednio.
- [Konfigurowanie HBase replikacji w HDInsight](hdinsight-hbase-geo-replication.md): Dowiedz się, jak skonfigurować HBase replikacji między dwoma Azure centrach danych. 
- [Analizowanie upodobania Twitter z HBase w HDInsight][hbase-twitter-sentiment]: Dowiedz się, jak wykonywać w czasie rzeczywistym [analizy upodobania](http://en.wikipedia.org/wiki/Sentiment_analysis) duży danych za pomocą HBase w klastrze Hadoop HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
