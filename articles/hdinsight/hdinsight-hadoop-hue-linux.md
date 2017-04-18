<properties
    pageTitle="Używanie odcień z Hadoop na klastrów HDInsight Linux | Microsoft Azure"
    description="Dowiedz się, jak zainstalować i odcień za pomocą klastrów Hadoop na HDInsight Linux."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="nitinme"/>

# <a name="install-and-use-hue-on-hdinsight-hadoop-clusters"></a>Instalowanie i używanie odcień na klastrów HDInsight Hadoop

Dowiedz się, jak zainstalować odcień dotyczących klastrów HDInsight Linux i kierowanie żądań do odcień przy użyciu tunelowania.

## <a name="what-is-hue"></a>Co to jest odcień?

Odcień to zestaw aplikacji sieci Web umożliwiające interakcję z klastrem Hadoop. Odcień umożliwia przeglądanie magazynowania skojarzone z klastrem Hadoop (w przypadku klastrów HDInsight WASB), uruchomić zadania gałęzi i skryptów świnka itp. Następujące składniki są dostępne w instalacjach odcień w klastrze HDInsight Hadoop.

* Edytor gałęzi wosk pszczeli
* Świnka
* Menedżer Metastore
* Oozie
* FileBrowser (który zawiera do kontenera domyślnego WASB)
* Przeglądarki zadania

> [AZURE.WARNING] Składniki dostarczony z klastrem HDInsight są w pełni obsługiwane i Microsoft Support pomogą izolowanie i rozwiązywanie problemów związanych z tych składników.
>
> Niestandardowe składniki otrzymają komercyjnego rozsądne pomocy technicznej, aby pomóc rozwiązać ten problem. Może to spowodować w rozwiązaniu problemu lub pytaniem nawiązanie dostępnych kanałów technologiami Otwórz źródło miejsce, w którym znajduje się głębokości specjalizacji tej technologii. Na przykład, istnieje wiele witryn społeczności, których można używać, takich jak: [forum w witrynie MSDN HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Projekty Apache mieć także witryn projektów na [http://apache.org](http://apache.org), na przykład: [Hadoop](http://hadoop.apache.org/).

## <a name="install-hue-using-script-actions"></a>Instalowanie odcień przy użyciu akcji skryptu

Następującą akcję skryptu może służyć do zainstalowania odcień w klastrze HDInsight systemem Linux.
https://hdiconfigactions.blob.Core.Windows.NET/linuxhueconfigactionv02/Install-Hue-Uber-v02.sh
    
Ta sekcja zawiera instrukcje dotyczące podczas inicjowania obsługi administracyjnej klaster przy użyciu Azure Portal za pomocą skryptu. 

> [AZURE.NOTE] Azure programu PowerShell, polecenie Azure, HDInsight .NET SDK lub Menedżer zasobów Azure szablony można również stosowanie akcji skryptów. Akcje skryptu można też zastosowane do już uruchomiony klastrów. Aby uzyskać więcej informacji zobacz [Dostosowywanie HDInsight klastrów z akcjami skrypt](hdinsight-hadoop-customize-cluster-linux.md).

1. Rozpoczynanie inicjowania obsługi administracyjnej klastrze, wykonując kroki opisane w [klastrów świadczenia usługi HDInsight na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), ale nie zostanie inicjowania obsługi administracyjnej.

    > [AZURE.NOTE] Aby zainstalować odcień na HDInsight klastrów, rozmiar headnode zalecane jest co najmniej A4 (8 rdzeniom, 14 GB pamięci RAM).

2. Na karta **Opcjonalnym** wybierz **Akcje skryptu**i wprowadź informacje, jak pokazano poniżej:

    ![Podaj skrypt akcji parametrów odcień] (./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Podaj skrypt akcji parametrów odcień")

    * __Nazwa__: Wprowadź przyjazną nazwę akcji skryptów.
    * __Identyfikator URI skrypt__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    * __Szef__: Zaznaczenie tego pola wyboru
    * __Pracownik__: pozostaw to pole puste.
    * __ZOOKEEPER__: pozostaw to pole puste.
    * __Parametry__: pozostaw to pole puste.

3. U dołu **Akcje skrypt**Użyj przycisk **Wybierz** , aby zapisać konfigurację. Na koniec używania przycisku **Zaznaczenie** u dołu karta **Opcjonalnym** do zapisywania informacji opcjonalnym.

4. Kontynuuj inicjowania obsługi administracyjnej klaster, zgodnie z opisem w [klastrów świadczenia usługi HDInsight na Linux](hdinsight-hadoop-provision-linux-clusters.md#portal).

## <a name="use-hue-with-hdinsight-clusters"></a>Odcień za pomocą klastrów HDInsight

SSH Tunneling jest jedynym sposobem na dostęp do odcień w klastrze po uruchomieniu. Tunelowanie za pośrednictwem SSH umożliwia ruch przejść bezpośrednio do headnode klastrze, w którym działa odcień. Po zakończeniu klaster inicjowania obsługi administracyjnej, wykonaj następujące czynności, umożliwia odcień w klastrze HDInsight Linux.

1. Skorzystaj z informacji w [Za pomocą SSH Tunneling dostęp do Ambari interfejs użytkownika sieci web, ResourceManager, JobHistory, NameNode, Oozie i innych sieci web Interfejsem](hdinsight-linux-ambari-ssh-tunnel.md) przekształcania tunelem SSH systemu klienta do klastrów HDInsight, a następnie skonfiguruj przeglądarki sieci Web, aby użyć tunelem SSH jako serwer proxy.

2. Po utworzeniu tunelem SSH i skonfigurowany serwer proxy ruchu przez go przy użyciu przeglądarki, możesz znaleźć nazwę hosta podstawowego węzła głównego. Możesz to zrobić za pomocą połączenia z klastrem przy użyciu SSH na porcie 22. Na przykład `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net` gdzie __nazwa_użytkownika__ to nazwa użytkownika SSH i __NAZWAKLASTRA__ to nazwa klaster.

    Aby uzyskać więcej informacji na temat korzystania z SSH zobacz następujące dokumenty:

    * [Używanie SSH z systemem Linux HDInsight kliencie Linux, Unix lub systemu Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Używanie SSH z systemem Linux HDInsight z klientem systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Po połączeniu, użyj następującego polecenia, aby uzyskać w pełni kwalifikowana nazwa domeny podstawowej headnode:

        hostname -f

    Spowoduje to przywrócenie nazwę podobną do następującej:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    Jest to nazwa hosta podstawowego headnode miejsce, w którym znajduje się w witrynie sieci Web odcień.

2. Otwórz portal odcień pod adresem http://HOSTNAME:8888 za pomocą przeglądarki. Zamień HOSTNAME nazwę, której uzyskaną w poprzednim kroku.

    > [AZURE.NOTE] Po zalogowaniu się po raz pierwszy, zostanie wyświetlony monit o utworzenie konta, aby zalogować się do portalu odcień. Poświadczenia określone w tym miejscu jest ograniczona do portalu i nie są związane z administratorem lub SSH poświadczenia określone podczas należy klaster.

    ![Zaloguj się do portalu odcień] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Określ poświadczenia dla portalu odcień")

### <a name="run-a-hive-query"></a>Uruchamianie kwerendy gałęzi

1. Z portalu odcień kliknij **Edytory kwerendy**, a następnie kliknij **gałąź** , aby otworzyć Edytor gałęzi.

    ![Używanie gałęzi] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Używanie gałęzi")

2. Na karcie **Pomoc** w obszarze **bazy danych**zobacz **hivesampletable**. To jest przykładowej tabeli, która jest dołączona do programu wszystkich klastrów Hadoop na HDInsight. W okienku po prawej stronie wprowadź zapytanie próbki i wyświetlane na karcie **wyników** w okienku poniżej, jak pokazano w przechwycony ekran.

    ![Uruchamianie gałęzi kwerendy] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Uruchamianie gałęzi kwerendy")

    Aby wyświetlić będący wizualną reprezentacją wynik umożliwia także na karcie **Wykres** .

### <a name="browse-the-cluster-storage"></a>Przejdź na przechowywanie klaster

1. Z portalu odcień w prawym górnym rogu na pasku menu kliknij pozycję **Przeglądarka plików** .

2. Domyślnie przeglądarka plików otwiera się w katalogu **/user/myuser** . Kliknij pozycję kreski ułamkowej bezpośrednio przed katalogu użytkowników w ścieżce, aby przejść do katalogu głównego kontenera magazynu Azure, skojarzone z klastrem.

    ![Używanie pliku przeglądarki] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Używanie pliku przeglądarki")

3. Kliknij prawym przyciskiem myszy plik lub folder, aby wyświetlić dostępne operacje. Przycisk **Przekaż** w prawym rogu przekazywać pliki do bieżącego katalogu. Przycisk **Nowy** umożliwia tworzenie nowych plików lub katalogów.

> [AZURE.NOTE] Przeglądarka plików odcień może tylko wyświetlać zawartość domyślnego kontenera skojarzone z klastrem HDInsight. Wszelkie dodatkowe miejsce w magazynie kont i kontenerach, które mogą być skojarzone z klastrem nie będzie dostępny za pomocą przeglądarki pliku. Jednak dodatkowe kontenery skojarzone z klastrem zawsze będą dostępne dla zadań gałęzi. Na przykład po wprowadzeniu polecenia `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` w edytorze gałęzi mogą wyświetlać zawartość również dodatkowe kontenerów. W tym poleceniu **newcontainer** nie jest skojarzone z klastrem domyślny kontener.

## <a name="important-considerations"></a>Ważne zagadnienia

1. Skrypt użyte do zainstalowania odcień instaluje go tylko na podstawowy headnode klastrze.

2. Podczas instalacji aktualizacji konfiguracji ponownego uruchomienia wielu usługi Hadoop (HDFS, PRZĘDZY, MR2, Oozie). Po zakończeniu instalacji odcień skrypt może upłynąć trochę czasu na inne usługi Hadoop do uruchamiania. Początkowo może wpłynąć na odcień wydajności. Po uruchomieniu wszystkich usług, odcień będzie w pełni funkcjonalny.

3.  Odcień nie rozpoznaje Tez zadania, która jest bieżącym domyślnego gałęzi. Jeśli chcesz używać MapReduce jako aparat wykonania gałęzi, należy zaktualizować skrypt umożliwiający Użyj następującego polecenia za pomocą skryptu:

        set hive.execution.engine=mr;

4.  Dzięki klastrów Linux mogą mieć scenariusz miejsce, w którym usługi są uruchomione na headnode podstawowego może być uruchomiona Menedżera zasobów na pomocniczej. Takiej sytuacji może spowodować błędy (jak pokazano poniżej) w przypadku wyświetlanie szczegółów zadań uruchomiony w klastrze za pomocą odcień. Jednak można wyświetlać szczegóły zadania po ukończeniu zadania.

    ![Błąd portalu odcień] (./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Błąd portalu odcień")

    To jest ze względu na to znany problem. Obejść ten problem należy zmodyfikować Ambari, tak aby aktywne Menedżera zasobów również działa na headnode podstawowego.

5.  Odcień rozumie WebHDFS, podczas gdy klastrów HDInsight magazyn Azure za pomocą `wasbs://`. Tak skryptu niestandardowego używane z akcją skrypt instaluje WebWasb, który jest zgodny z WebHDFS usługę do rozmowy WASB. Tak mimo że portal odcień mówi HDFS w miejscach (na przykład po umieszczeniu wskaźnika myszy na **Przeglądarka plików**) powinny być interpretowane jako WASB.


## <a name="next-steps"></a>Następne kroki

- [Instalowanie Giraph na HDInsight klastrów](hdinsight-hadoop-giraph-install-linux.md). Dostosowywanie klaster należy zainstalować Giraph na HDInsight Hadoop klastrów. Giraph umożliwia wykonywanie przetwarzanie wykresu za pomocą Hadoop i można łączyć z usługi HDInsight Azure.

- [Instalowanie Solr na HDInsight klastrów](hdinsight-hadoop-solr-install-linux.md). Dostosowywanie klaster należy zainstalować Solr na HDInsight Hadoop klastrów. Solr umożliwia wykonywanie operacji wyszukiwania zaawansowane na przechowywanych danych.

- [Instalowanie R dotyczących klastrów HDInsight](hdinsight-hadoop-r-scripts-linux.md). Dostosowywanie klaster należy zainstalować R dotyczących klastrów HDInsight Hadoop. R jest język źródłowy Otwórz i środowisko przetwarzania danych statystycznych. Udostępnia setki wbudowanych funkcji statystycznych i własny język programowania łączy aspekty funkcjonalne i obiektowych programowania. Umożliwia także rozbudowane funkcje graficzne.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
