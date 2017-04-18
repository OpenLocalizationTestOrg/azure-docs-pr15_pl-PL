<properties
    pageTitle="Używanie węzły pustych krawędzi w HDInsight | Microsoft Azure"
    description="Jak dodać węzła krawędź ampty do klastrów HDInsight, który może być używany jako klient, a test-hosta aplikacji HDInsight."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>Używanie węzły pustych krawędzi w HDInsight

Dowiedz się, jak dodać węzła pustego krawędź do klastrów HDInsight systemem Linux. Węzeł pustego krawędź jest maszyny wirtualnej Linux samego narzędzia klienckie zainstalowaniu i skonfigurowaniu jak headnodes, ale nie uruchomione usługi hadoop. Węzeł krawędzi służy do uzyskiwania dostępu klaster, testowanie aplikacji klienckich i hostingu aplikacji klienta. 

Węzeł pustych krawędzi można dodać do istniejącego klastrów HDInsight do nowego klastrów podczas tworzenia klaster. Dodawanie węzła pustego krawędź jest wykonywane przy użyciu szablonu Azure Menedżera zasobów.  Poniższy przykład pokazuje, jak to zrobić przy użyciu szablonu:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Jak pokazano w próbce, możesz opcjonalnie połączenie [Akcja skrypt](hdinsight-hadoop-customize-cluster-linux.md) do wykonywać dodatkowych kroków konfiguracji, takie jak instalowanie [Odcień Apache](hdinsight-hadoop-hue-linux.md) w węźle krawędzi.

Po utworzeniu węzeł krawędzi można nawiązać połączenie przy użyciu SSH węzeł krawędzi i uruchomić narzędziami klienta, aby uzyskać dostęp do klaster Hadoop w HDInsight.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Dodawanie węzła krawędź z istniejącym klastrem

W tej sekcji można Użyj szablonu Menedżera zasobów, aby dodać węzeł krawędź z istniejącym klastrem HDInsight.  Szablon Menedżera zasobów można znaleźć w [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode). Szablon Menedżera zasobów połączeń skrypt akcji skryptów znajdującego się w https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Skrypt nie wykonywać żadnych akcji.  To wykazać wywołującego akcję skryptu przy użyciu szablonu Menedżera zasobów.

**Aby dodać węzeł pustego krawędź z istniejącym klastrem**

1. Utwórz klaster HDInsight, jeśli nie masz jeszcze.  Zobacz [Hadoop samouczek: wprowadzenie do korzystania z systemem Linux Hadoop w HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Kliknij, aby zalogować się do Azure i otwórz szablon Menedżera zasobów Azure w portalu Azure następujące obraz. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Skonfiguruj następujące właściwości:

    - NAZWAKLASTRA: Wprowadź nazwę istniejącego klaster HDInsight.
    - EDGENODESIZE: Wybierz jeden z rozmiarów maszyn wirtualnych.
    - **EDGENODEPREFIX: Wartość domyślna nowego.**  Zostanie użyta wartość domyślna, nazwa węzła krawędź jest **Nowe edgenode**.  Możesz dostosować prefiks z portalu. Można także dostosować imię i nazwisko z szablonu.


4. Kliknij **przycisk OK** , aby zapisać zmiany.
5. **Grupa zasobów**wybierz grupę zasobów.
6. Kliknij pozycję **Recenzja warunki prawne**, a następnie kliknij **zakupu**.
7. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij przycisk **Utwórz**.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Dodawanie węzła krawędzi podczas tworzenia klastrze

W tej sekcji umożliwia szablonu Menedżera zasobów utworzyć klaster HDInsight węzeł krawędzi.  Szablon Menedżera zasobów można znaleźć w publicznej [kontenera magazynu obiektów Blob platformy Azure](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json). Szablon Menedżera zasobów połączeń skrypt akcji skryptów znajdującego się w https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh. Skrypt nie wykonywać żadnych akcji.  To wykazać wywołującego akcję skryptu przy użyciu szablonu Menedżera zasobów.

**Aby dodać węzeł pustego krawędź z istniejącym klastrem**

1. Utwórz klaster HDInsight, jeśli nie masz jeszcze.  Zobacz [Hadoop samouczek: wprowadzenie do korzystania z systemem Linux Hadoop w HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Kliknij, aby zalogować się do Azure i otwórz szablon Menedżera zasobów Azure w portalu Azure następujące obraz. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Skonfiguruj następujące właściwości:
        
    - NAZWAKLASTRA: Wprowadź nazwę dla nowego klastrze w celu utworzenia.
    - CLUSTERLOGINUSERNAME: Wprowadź nazwę użytkownika Hadoop HTTP.  Domyślna nazwa to **administratora**.
    - CLUSTERLOGINPASSWORD: Wprowadź hasło użytkownika Hadoop HTTP.
    - SSHUSERNAME: Wprowadź nazwę użytkownika, SSH. Domyślna nazwa to **sshuser**.
    - SSHPASSWORD: Wprowadź hasło użytkownika SSH.
    - Lokalizacja: Wprowadź lokalizację nazwy grupy zasobów, klaster i domyślne konto miejsca do magazynowania.
    - CLUSTERTYPE: Wartość domyślna to **hadoop**.
    - CLUSTERWORKERNODECOUNT: Wartość domyślna to 2.
    - EDGENODESIZE: Wybierz jeden z rozmiarów maszyn wirtualnych.
    - **EDGENODEPREFIX: Wartość domyślna nowego.**  Zostanie użyta wartość domyślna, nazwa węzła krawędź jest **Nowe edgenode**.  Możesz dostosować prefiks z portalu. Można także dostosować imię i nazwisko z szablonu.

4. Kliknij **przycisk OK** , aby zapisać zmiany.
5. **Grupa zasobów**wprowadź nazwę nowej grupy zasobów.
6. Kliknij pozycję **Recenzja warunki prawne**, a następnie kliknij **zakupu**.
7. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij przycisk **Utwórz**. 


## <a name="access-an-edge-node"></a>Dostęp do węzła krawędzi

Węzeł krawędzi ssh punkt końcowy jest <EdgeNodeName>. <ClusterName>-ssh.azurehdinsight.net:22.  Na przykład nowy edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Węzeł krawędź jest wyświetlany jako aplikacja portalu Azure.  Portalu zawiera informacje, aby uzyskać dostęp do węzła krawędź przy użyciu SSH.

**Aby sprawdzić punkt końcowy SSH węzeł krawędzi**

1. Logowanie się do [portalu Azure](https://portal.azure.com).
2. Otwórz klastrze HDInsight z węzła krawędzi.
3. Kliknij pozycję **aplikacje** z karta klaster. Zapewniają węzeł krawędzi.  Domyślna nazwa to **Nowy edgenode**.
4. Kliknij węzeł krawędzi. Zapewniają punkt końcowy SSH.

**Aby użyć gałęzi w węźle krawędzi**

5. Nawiązywanie połączenia z węzeł krawędzi za pomocą SSH.  Zobacz [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md) lub [za pomocą SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Po podłączeniu do węzła krawędź przy użyciu SSH umożliwia otwieranie konsoli gałęzi następujące polecenie:

        hive
7. Uruchom następujące polecenie, aby wyświetlić tabele gałęzi w grupie:

        show tables;

## <a name="delete-an-edge-node"></a>Usuń węzeł krawędzi

Możesz usunąć węzeł krawędź z portalu Azure.

**Aby uzyskać dostęp do węzła krawędzi**

1. Logowanie się do [portalu Azure](https://portal.azure.com).
2. Otwórz klastrze HDInsight z węzła krawędzi.
3. Kliknij pozycję **aplikacje** z karta klaster. Są zostanie wyświetlona lista węzłów krawędzi.  
4. Kliknij prawym przyciskiem myszy węzeł krawędź, którą chcesz usunąć, a następnie kliknij polecenie **Usuń**.
5. Kliknij przycisk **Tak,** aby potwierdzić.

## <a name="next-steps"></a>Następne kroki

W tym artykule zapewne wiesz, jak dodać węzła krawędzi oraz jak uzyskać dostęp do węzła krawędzi. Aby uzyskać więcej informacji, zobacz następujące artykuły:

- [Instalowanie usługi HDInsight aplikacji](hdinsight-apps-install-applications.md): Dowiedz się, jak zainstalować aplikację HDInsight do klastrów.
- [Instalowanie niestandardowej aplikacji HDInsight](hdinsight-apps-install-custom-applications.md): Dowiedz się, jak wdrożyć aplikację wycofanych HDInsight w HDInsight.
- [Publikowanie HDInsight aplikacji](hdinsight-apps-publish-applications.md): Dowiedz się, jak opublikować niestandardowe aplikacje HDInsight Azure Marketplace.
- [MSDN: zainstalować aplikację HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Dowiedz się, jak zdefiniować HDInsight aplikacji.
- [Przy użyciu akcji skryptu klastrów systemem Linux Dostosowywanie HDInsight](hdinsight-hadoop-customize-cluster-linux.md): Dowiedz się, jak zainstalować dodatkowe aplikacje za pomocą skryptu akcji.
- [Oparte na tworzenie Linux Hadoop klastrów w HDInsight przy użyciu szablonów Menedżera zasobów](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Dowiedz się, jak połączeń szablony Menedżera zasobów do tworzenia HDInsight klastrów.

