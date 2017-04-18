<properties
pageTitle="Migrowanie z usługi HDInsight systemu Windows do systemem Linux HDInsight | Azure"
description="Dowiedz się, jak przeprowadzić migrację z klastrem HDInsight systemu Windows do klastrów HDInsight systemem Linux."
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
ms.date="10/28/2016"
ms.author="larryfr"/>

#<a name="migrate-from-a-windows-based-hdinsight-cluster-to-a-linux-based-cluster"></a>Migrację z klastrem HDInsight systemu Windows do klastrów systemem Linux

HDInsight systemu Windows zapewnia łatwy sposób za pomocą Hadoop w chmurze, jednocześnie może się okazać, że potrzebujesz klastrze systemem Linux, aby można było korzystać z narzędzi i technologii, które są wymagane do rozwiązania. Wiele elementów w ekosystemie Hadoop są tworzone w systemach Linux, a niektóre mogą być dostępne do użytku z usługi HDInsight systemu Windows. Ponadto wiele książek, klipy wideo i inne materiały szkoleniowe przyjęto założenie, że korzystasz z systemem Linux podczas pracy z Hadoop.

Ten dokument zawiera szczegółowe informacje dotyczące różnic między HDInsight w systemach Windows i Linux oraz wytyczne dotyczące jak przeprowadzić migrację istniejącej obciążenia do klastrów opartych na systemie Linux.

> [AZURE.NOTE] Usługa HDInsight klastrów użyć funkcji długoterminową Ubuntu (KÓW) jako system operacyjny węzłów w klastrze. Uzyskać informacji na temat wersji Ubuntu, które są dostępne z usługi HDInsight, oraz inne informacje o składniku przechowywania wersji zobacz [wersje składników HDInsight](hdinsight-component-versioning.md).

## <a name="migration-tasks"></a>Zadania związane z migracją

Ogólne przepływu pracy dla migracji wygląda następująco:

![Diagram przepływu pracy migracji](./media/hdinsight-migrate-from-windows-to-linux/workflow.png)

1.  Przeczytaj sekcję ten dokument, aby zrozumieć zmian, które mogą być wymagane podczas migracji z istniejącego przepływu pracy, zadania itd do klastrów opartych na systemie Linux.

2.  Utwórz klaster systemem Linux jako środowisku assurance test jakości. Aby uzyskać więcej informacji na temat tworzenia klastrze systemem Linux zobacz [systemem Linux oraz tworzenie klastrów w HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

3.  Kopiowanie istniejącego zadania, źródeł danych i pochłaniacze do nowego środowiska. Zobacz kopiowanie danych do sekcji środowiska test, aby uzyskać więcej informacji.

4.  Wykonywanie, aby upewnić się, że zadań działają zgodnie z oczekiwaniami na nowym klastrze sprawdzanie poprawności.

Po upewnieniu się, że wszystko działa zgodnie z oczekiwaniami, planowanie przestojów migracji. W tym przestoje wykonaj następujące czynności.

1.  Aby utworzyć kopię zapasową jakiekolwiek dane przejściowych zgromadzone na przez klaster. Na przykład, jeśli masz dane przechowywane bezpośrednio na węzła głównego.

2.  Usuń klaster systemu Windows.

3.  Utwórz klaster systemem Linux za pomocą samego magazynu danych domyślnych, używany przez klaster systemu Windows. Dzięki temu będzie nowy klaster kontynuować pracę z istniejących danych produkcji.

4.  Importowanie danych przejściowych, które kopii zapasowej.

5.  Rozpoczęcie zadania i kontynuować przetwarzanie przy użyciu nowy klaster.

### <a name="copy-data-to-the-test-environment"></a>Kopiowanie danych do środowiska testowego

Istnieje wiele metod, aby skopiować dane i zadania, jednak opisane w tej sekcji są najłatwiejszym metody bezpośrednio przenosić pliki do klastrów test.

#### <a name="hdfs-dfs-copy"></a>Kopiowanie HDFS DFS

Polecenie usługi Hadoop HDFS bezpośrednio Kopiuj danych z magazynu w istniejącym klastrem produkcji, do magazynowania w ramach nowy klaster test, wykonując następujące czynności.

1. Znajdź miejsca do magazynowania konta i domyślny kontener informacje dotyczące istniejącego klaster. Możesz to zrobić przy użyciu tego skryptu programu PowerShell Azure.

        $clusterName="Your existing HDInsight cluster name"
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        write-host "Storage account name: $clusterInfo.DefaultStorageAccount.split('.')[0]"
        write-host "Default container: $clusterInfo.DefaultStorageContainer"

2. Postępuj zgodnie z instrukcjami systemem Linux oraz tworzenie klastrów w HDInsight dokumentu do utworzenia nowego środowiska testowego. Zatrzymywanie przed utworzeniem klaster, a zamiast tego wybrać **Opcjonalnym**.

3. Karta opcjonalnym zaznacz **Połączone konta miejsca do magazynowania**.

4. Wybierz pozycję **Dodaj klucz magazynowania**, a po wyświetleniu monitu wybierz konto miejsca do magazynowania, który został zwrócony przez skrypt programu PowerShell w kroku 1. Kliknij przycisk **Zaznacz** na każdym karta, aby je zamknąć. Na koniec utworzyć klaster.

5. Po utworzeniu klaster nawiązania połączenia przy użyciu **SSH.** Jeśli nie znasz z SSH za pomocą usługi HDInsight, zobacz następujące artykuły.

    * [Używanie SSH z systemem Linux HDInsight przez klientów systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Używanie SSH z systemem Linux HDInsight z klientami Linux, Unix i komputerów Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

6. Od sesji SSH Użyj następującego polecenia, aby skopiować pliki z konta połączone miejsca do magazynowania do nowego domyślnego konta miejsca do magazynowania. Zamień kontenera i konto kontener oraz informacje o koncie zwróconą przez skrypt programu PowerShell w kroku 1. Zastąp ścieżkę dostępu do danych ścieżką do pliku danych.

        hdfs dfs -cp wasbs://CONTAINER@ACCOUNT.blob.core.windows.net/path/to/old/data /path/to/new/location

    [AZURE.NOTE] Jeśli nie istnieje w środowisku testowym strukturę katalogu, która zawiera dane, możesz utworzyć go przy użyciu następującego polecenia.

        hdfs dfs -mkdir -p /new/path/to/create

    `-p` Przełącz umożliwia tworzenie katalogów w ścieżce.

#### <a name="direct-copy-between-azure-storage-blobs"></a>Bezpośrednie kopiowania między obiektami blob magazyn Azure

Możesz też warto używać `Start-AzureStorageBlobCopy` polecenia cmdlet programu PowerShell Azure, aby skopiować obiektów blob między kontami miejsca do magazynowania poza HDInsight. Aby uzyskać więcej informacji, zobacz jak zarządzać sekcji obiektów blob platformy Azure za pomocą programu PowerShell Azure z nośnikami Azure.

##<a name="client-side-technologies"></a>Technologie po stronie klienta

Na ogół technologii po stronie klienta, takich jak [polecenia cmdlet programu PowerShell Azure](../powershell-install-configure.md), [Polecenie Azure](../xplat-cli-install.md) lub [Zestaw SDK programu .NET dla Hadoop](https://hadoopsdk.codeplex.com/) będzie nadal działa tak samo, z systemem Linux klastrów, jak polegają na pozostałych interfejsów API, które są takie same w obu typów klaster systemu operacyjnego.

##<a name="server-side-technologies"></a>Technologie po stronie serwera

Poniższa tabela zawiera wskazówki dotyczące migrowania składników po stronie serwera, które są określonego systemu Windows.

| Jeśli korzystasz z tej technologii... | Wykonanie tej czynności... |
| ----- | ----- |
| **Programu PowerShell** (skrypty po stronie serwera, łącznie z działaniami skrypt używany podczas tworzenia klaster) | Ponownie zapisać jako imprezie skryptów. Do wykonywania akcji skryptów zobacz [systemem Linux Dostosowywanie HDInsight z akcjami skryptu](hdinsight-hadoop-customize-cluster-linux.md) i [opracowywanie akcji skryptów dla HDInsight systemem Linux](hdinsight-hadoop-script-actions-linux.md). |
| **Polecenie Azure** (skrypty po stronie serwera) | Polecenie Azure są dostępne w systemie Linux, go nie są zainstalowane na głowy węzłach HDInsight. Jeśli potrzebujesz dla skryptów po stronie serwera, zobacz [Instalowanie polecenie Azure](../xplat-cli-install.md) informacje na temat instalowania na platformach opartych na systemie Linux. |
| **Składniki .NET** | .NET nie jest w pełni obsługiwany w klastrów HDInsight systemem Linux. Systemem Linux Burza na HDInsight klastrów utworzone po pomocy technicznej 2017-10-28 topologii C# Burza przy użyciu struktury SCP.NET. Dodatkowe wsparcie dla środowiska .NET zostanie dodany w przyszłych aktualizacji. |
| **Składniki Win32 lub inne technologie tylko do systemu Windows** | Wskazówki dotyczące zależy od składnika lub technologii; można znaleźć w wersji, który jest zgodny z systemem Linux lub może być konieczne znalezienia rozwiązania alternatywne lub ponownego tego składnika. |

##<a name="cluster-creation"></a>Tworzenie klaster

Ta sekcja zawiera informacje dotyczące różnic między podczas tworzenia klaster.

### <a name="ssh-user"></a>SSH użytkownika

Systemem Linux klastrów HDInsight protokół **Secure Shell (SSH)** zapewnia dostęp zdalny do przez klaster. W przeciwieństwie do systemu Windows dla pulpitu zdalnego klastrów większość klientów SSH nie udostępnia środowisko graficznego, ale zamiast tego zapewnia wiersza polecenia, która umożliwia uruchomienie poleceń w klastrze. Niektórzy klienci (na przykład [MobaXterm](http://mobaxterm.mobatek.net/),) podaj przeglądarki system plików graficznych, oprócz zdalnego wiersza polecenia.

Podczas tworzenia klaster należy podać SSH użytkownika i **hasła** lub **certyfikatu klucza publicznego** uwierzytelniania.

Zalecamy używanie certyfikatu klucza publicznego, ponieważ jest bezpieczniejsze niż za pomocą hasła. Uwierzytelnianie na podstawie certyfikatu polega na generowanie podpisanego pary kluczy publicznych i prywatnych, a następnie dostarczając klucz publiczny, tworząc klaster. Podczas łączenia się z serwerem przy użyciu SSH, klucz prywatny na komputerze klienckim zawiera uwierzytelniania dla połączenia.

Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz:

- [Za pomocą SSH HDInsight przez klientów systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

- [Za pomocą SSH HDInsight z klientami Linux, Unix i OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

### <a name="cluster-customization"></a>Dostosowywanie klaster

**Akcje skryptu** używane z systemem Linux klastrów musi być napisana w imprezie skrypt. Podczas akcje skryptu mogą być używane podczas tworzenia klaster, systemem Linux klastrów można je również używanych do wykonania dostosowywania po klaster działa i uruchomiony. Aby uzyskać więcej informacji zobacz [systemem Linux Dostosowywanie HDInsight z akcjami skryptu](hdinsight-hadoop-customize-cluster-linux.md) i [opracowywanie akcji skryptów dla HDInsight systemem Linux](hdinsight-hadoop-script-actions-linux.md).

Inną funkcją dostosowywania jest **uruchamiania**. W przypadku klastrów systemu Windows to umożliwia Określ lokalizację dodatkowe biblioteki do użytku z gałęzi. Po utworzeniu klaster biblioteki te są automatycznie dostępne do użytku z kwerendami gałęzi bez konieczności używania `ADD JAR`.

Tę funkcję, nie udostępnia początkowego dla klastrów opartych na systemie Linux. Użyj zamiast tego skryptu akcji w [gałęzi Dodawanie bibliotek podczas tworzenia klaster](hdinsight-hadoop-add-hive-libraries.md).

### <a name="virtual-networks"></a>Wirtualnych sieci

Usługa HDInsight systemu Windows klastrów działa tylko z klasycznej wirtualnych sieci, a systemem Linux HDInsight klastrów wymagają Menedżera zasobów wirtualne sieci. Mając zasobów w sieci wirtualnej klasycznym należy połączyć klaster Linux HDInsight, zobacz [Łączenie klasyczny wirtualnej sieci w sieci wirtualnych Menedżera zasobów](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

Uzyskać więcej informacji o konfiguracji wymagania dotyczące wirtualnych sieci Azure za pomocą usługi HDInsight zobacz [możliwości rozszerzenie HDInsight przy użyciu wirtualnej sieci](hdinsight-extend-hadoop-virtual-network.md).

##<a name="management-and-monitoring"></a>Zarządzanie i monitorowanie

Wiele użyto z HDInsight systemu Windows, takich jak historię zatrudnienia lub przędzy interfejsu użytkownika pakietu sieci Web jest dostępne za pośrednictwem Ambari. Ponadto widoku gałęzi Ambari umożliwia uruchamianie zapytania gałęzi za pomocą przeglądarki sieci web. Ambari interfejs sieci Web jest dostępna na podstawie Linux klastrów na https://CLUSTERNAME.azurehdinsight.net.

Aby uzyskać więcej informacji na temat pracy z Ambari zobacz następujące dokumenty:

- [Ambari sieci Web](hdinsight-hadoop-manage-ambari.md)

- [Ambari interfejsu API usługi REST](hdinsight-hadoop-manage-ambari-rest-api.md)

### <a name="ambari-alerts"></a>Alerty Ambari

Ambari zawiera system alertu, który można informujący o potencjalnych problemach z klastrem. Alerty są wyświetlane jako czerwone lub żółte wpisy w Ambari interfejs sieci Web, jednak można również pobierać za pośrednictwem interfejsu API usługi REST.

> [AZURE.IMPORTANT] Alerty Ambari wskazują tej *może* wystąpić problem, że *jest* problem. Na przykład może zostać wyświetlony alert, że HiveServer2 nie jest dostępny, mimo że można do niego dostęp normalnie.
>
> Wiele alertów są wykonywane jako oparte na Interwał kwerend dla usługi i oczekiwać odpowiedzi w określonym przedziale czasu. Aby alert nie musi oznaczać, że usługa nie działa, tylko że nie zwraca wyników w oczekiwany przedziale czasu.

Ogólnie należy sprawdzić, czy alert został występujące przez dłuższy czas lub odzwierciedla problemów użytkownika, które wcześniej zostały zgłoszone z klastrem przed podjęciem działań na nim.

##<a name="file-system-locations"></a>Lokalizacje systemu plików

System plików klaster Linux jest rozmieszczona inaczej niż klastrów HDInsight systemu Windows. Znajdowanie często używanych plików, skorzystaj z poniższej tabeli.

| Muszę także znaleźć... | Znajduje się... |
| ----- | ----- |
| Konfiguracja | `/etc`. Na przykład`/etc/hadoop/conf/core-site.xml` |
| Pliki dziennika | `/var/logs` |
| Platformy danych Hortonworks (HDP) | `/usr/hdp`. Istnieją dwa poziomy znajduje się w tym miejscu jest bieżącą wersję HDP (na przykład `2.2.9.1-1`,) i `current`. `current` Katalogu zawiera łącza symbolicznego do pliki i katalogi w katalogu numer wersji i jest pod warunkiem, że to wygodny sposób uzyskiwania dostępu do plików HDP od wersji zostanie zmieniony numer wersji HDP jest aktualizowana. |
| hadoop streaming.jar | `/usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar` |

Ogólnie Jeśli znasz nazwę pliku, umożliwia uruchom następujące polecenie z sesji SSH znaleźć ścieżki pliku:

    find / -name FILENAME 2>/dev/null

Za pomocą symboli wieloznacznych do nazwy pliku. Na przykład `find / -name *streaming*.jar 2>/dev/null` zwróci ścieżkę dostępu do plików jar zawierających wyraz streaming, jako część nazwy pliku.

##<a name="hive-pig-and-mapreduce"></a>Gałąź, świnka i MapReduce

Świnka i MapReduce obciążenia są bardzo podobne na podstawie Linux klastrów — podstawowa różnica polega na Jeśli nawiązywanie połączenia z klastrem bazującym na systemie Windows za pomocą pulpitu zdalnego i uruchomić zadania, którego użyjesz SSH z systemem Linux klastrów.

- [Świnka za pomocą SSH](hdinsight-hadoop-use-pig-ssh.md)

- [MapReduce za pomocą SSH](hdinsight-hadoop-use-mapreduce-ssh.md)

### <a name="hive"></a>Gałąź

Na poniższym diagramie przedstawiono wskazówki na temat przeprowadzania migracji z obciążeń pracą gałęzi.

| Na opartych na systemie Windows I Użyj... | Na podstawie Linux... |
| ----- | ----- |
| **Edytor gałęzi** | [Widok gałęzi w Ambari](hdinsight-hadoop-use-hive-ambari-view.md) |
| `set hive.execution.engine=tez;`Aby włączyć Tez | Tez jest domyślny aparat wykonania systemem Linux klastrów, więc instrukcji set nie są już potrzebne. |
| CMD, pliki lub skryptów na serwerze wywoływane w ramach zadania gałęzi | za pomocą skryptów imprezie |
| `hive`polecenia z pulpitu zdalnego | Za pomocą [Beeline](hdinsight-hadoop-use-hive-beeline.md) lub [gałęzi z sesji SSH](hdinsight-hadoop-use-hive-ssh.md) |

##<a name="storm"></a>Burza

| Na opartych na systemie Windows, można użyć... | Na podstawie Linux... |
| ----- | ----- |
| Pulpit nawigacyjny Burza | Pulpit nawigacyjny Burza nie jest dostępna. Zobacz [rozmieszczanie i zarządzanie Burza topologii na podstawie Linux HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md) sposoby przesyłania topologii |
| Burza interfejsu użytkownika | Interfejs użytkownika Burza jest dostępna w https://CLUSTERNAME.azurehdinsight.net/stormui |
| Program Visual Studio, aby utworzyć, wdrażania i zarządzania topologii C# lub hybrydowym | Program Visual Studio umożliwia tworzenie i wdrażanie oraz zarządzanie C# (SCP.NET) lub topologii hybrydowych na podstawie Linux Burza dotyczących klastrów HDInsight utworzone po 2017-10-28. |

##<a name="hbase"></a>HBase

Na podstawie Linux klastrów jest określony znode HBase `/hbase-unsecure`. Należy ustawić ten w konfiguracji dla każdego klienta Java aplikacje, które używają natywnych interfejsu API języka Java HBase.

Przykład klienta, który ustawia wartość, zobacz [Tworzenie aplikacji opartego na języku Java HBase](hdinsight-hbase-build-java-maven.md) .

##<a name="spark"></a>Spark

Klastrów Spark były dostępne w klastrów systemu Windows w wersji preview; Jednak w wersji Spark jest dostępna tylko dla klastrów systemem Linux. Istnieje nie ścieżkę migracji z klastrze Podgląd Spark systemu Windows do klastrów systemem Linux Spark wersji.

##<a name="known-issues"></a>Znane problemy

### <a name="azure-data-factory-custom-net-activities"></a>Azure Factory danych, niestandardowe działania .NET

Azure Factory danych, niestandardowe działania .NET nie są obecnie obsługiwane na klastrów HDInsight systemem Linux. Zamiast tego należy przy użyciu jednej z następujących metod niestandardowe działania w ramach planowaną ADF.

-   Wykonanie działania .NET w puli partii Azure. W sekcji partii Azure Użyj połączone usługi [Użyj niestandardowe działania w potoku Factory danych Azure](../data-factory/data-factory-use-custom-activities.md#AzureBatch)

-   Wdrożenie działania jako działania MapReduce. Aby uzyskać więcej informacji, zobacz [Wywołania programy MapReduce z fabryki danych](../data-factory/data-factory-map-reduce.md) .

### <a name="line-endings"></a>Zakończenia wierszy

Na ogół końców w systemach Windows za pomocą CRLF, podczas gdy systemy Linux wysuwu wiersza. Jeśli warzywa lub oczekiwać danych za pomocą CRLF końców, może być konieczne modyfikowanie producentów i konsumentów do pracy z końcowa wiersza wysuwu wiersza.

Na przykład przy użyciu programu PowerShell Azure HDInsight zapytania w klastrze opartych na systemie Windows zwróci danych za pomocą CRLF. Tę samą kwerendę z systemem Linux klastrze zwróci wysuwu wiersza. W większości przypadków to nie ma znaczenia danych dla klientów indywidualnych, jednak należy zbadać przed migrowaniem do klastrów systemem Linux.

Jeśli masz skrypty wykonanych bezpośrednio na Linux węzłach (na przykład skrypt Python używać z gałęzi lub zadanie MapReduce), należy zawsze używać wysuwu wiersza jako koniec wiersza. Jeśli używasz CRLF, można zobaczyć błędy podczas uruchamiania skryptów w klastrze z systemem Linux.

Jeśli wiesz, że skrypty nie zawierają ciągów znaków powrotu Karetki osadzony, można zbiorczo zmienić końców przy użyciu jednej z następujących metod:

-   **Jeśli masz skrypty, które planujesz przekazywania z klastrem**, użyj poniższych stwierdzeń programu PowerShell, aby zmienić zakończeń linii z CRLF wysuwu wiersza przed przekazaniem skrypt z klastrem.

        $original_file ='c:\path\to\script.py'
        $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
        [IO.File]::WriteAllText($original_file, $text)

-   **Jeśli masz skryptów, które znajdują się już w magazynie używana przez klaster**, umożliwia uruchom następujące polecenie z sesji SSH z systemem Linux klastrem zmodyfikuj skrypt.

        hdfs dfs -get wasbs:///path/to/script.py oldscript.py
        tr -d '\r' < oldscript.py > script.py
        hdfs dfs -put -f script.py wasbs:///path/to/script.py

##<a name="next-steps"></a>Następne kroki

-   [Dowiedz się, jak utworzyć klastrów systemem Linux HDInsight](hdinsight-hadoop-provision-linux-clusters.md)

-   [Nawiązywanie połączenia z systemem Linux klaster przy użyciu SSH z klientem systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

-   [Nawiązywanie połączenia z systemem Linux klaster przy użyciu SSH kliencie Linux, Unix lub komputerów Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

-   [Zarządzanie systemem Linux klaster przy użyciu Ambari](hdinsight-hadoop-manage-ambari.md)
