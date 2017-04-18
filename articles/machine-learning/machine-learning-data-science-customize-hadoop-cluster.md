<properties 
    pageTitle="Dostosowywanie klastrów Hadoop podczas nauki danych zespołu | Microsoft Azure" 
    description="Popularne moduły Python dostępne w niestandardowych klastrów Azure HDInsight Hadoop."
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"  />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="hangzh;bradsev" />

# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a>Dostosowywanie klastrów Azure HDInsight Hadoop w procesie nauki danych zespołu 

W tym artykule opisano dostosowywanie klaster HDInsight Hadoop zainstalować 64-bitowej Anaconda (Python 2.7) w każdym węźle po klaster jest obsługi administracyjnej jako usługa HDInsight. Pokazuje także jak uzyskać dostęp do headnode przesyłać niestandardowe zadania z klastrem. To dostosowanie sprawia, że wielu popularnych modułów Python zawartymi w wygodny sposób dostępny do użytku w funkcje zdefiniowane przez użytkownika (UDF), przeznaczonych do procesu rekordy gałęzi w klastrze Anaconda. Aby uzyskać instrukcje dotyczące procedur, w tym scenariuszu zobacz [sposób przesyłania kwerend gałęzi](machine-learning-data-science-move-hive-tables.md#submit).

Menu poniższych łączy do tematów, w których opisano sposób konfigurowania w różnych środowiskach nauki dane używane przez [Zespół danych nauki proces (TDSP)](data-science-process-overview.md).

[AZURE.INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]


## <a name="customize"></a>Dostosowywanie klaster Hadoop usługa Azure HDInsight

Aby utworzyć niestandardowe klaster HDInsight Hadoop, użytkownicy muszą się zalogować w [**Klasycznym Portal Azure**](https://manage.windowsazure.com/), w lewym dolnym rogu kliknij pozycję **Nowy** , a następnie wybierz pozycję usług danych -> HDINSIGHT -> **Utwórz niestandardowe** , aby wyświetlić okno **Szczegóły klaster** . 

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Wprowadzanie nazwy klaster ma zostać utworzony na stronie Konfiguracja 1 i zaakceptuj wartości domyślnych dla innych pól. Kliknij strzałkę, aby przejść do następnej strony konfiguracji. 

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

Na stronie Konfiguracja 2 wprowadzania liczby **Węzłów danych**, wybierz **REGION i WIRTUALNEJ sieci**, a następnie wybierz **Węzeł głowy** i **Węzeł danych**. Kliknij strzałkę, aby przejść do następnej strony konfiguracji.

>[AZURE.NOTE] **REGION i WIRTUALNEJ sieci** musi być taka sama jak region konta miejsca do magazynowania, który ma być używana w przypadku klaster HDInsight Hadoop. W przeciwnym razie czwartym na stronie Konfiguracja konta miejsca do magazynowania, którego chcesz używać użytkowników nie będą wyświetlane na liście rozwijanej **Nazwę konta**.

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

Na stronie Konfiguracja 3 Podaj nazwę użytkownika i hasło dla klastrów HDInsight Hadoop. **Nie należy** zaznaczać _Enter Metastore gałęzi-Oozie_. Następnie kliknij strzałkę, aby przejść do następnej strony konfiguracji. 

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

Na stronie Konfiguracja 4 Określ nazwę konta magazynu domyślnego kontenera klaster HDInsight Hadoop. Po zaznaczeniu _utworzyć kontener domyślny_ na liście rozwijanej **Domyślnego kontenera** , zostanie utworzony kontenera o takiej samej nazwie co klaster. Kliknij strzałkę, aby przejść do ostatniej strony konfiguracji.

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

Na ostatniej stronie konfiguracji **Akcje skrypt** kliknij przycisk **Dodaj akcję skryptu** , a wypełnienie pola tekstowe z następujących wartości.
 
* **Nazwa** - dowolny ciąg z nazwą tej akcji skryptów. 
* **Typ węzła** — wybierz pozycję **wszystkie węzły**. 
* **Identyfikator URI skryptu** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1* 
    * *publicscripts* jest kontenerem publicznej na koncie miejsca do magazynowania 
    * *getgoing* używanych do udostępniania plików skrypt programu PowerShell w celu ułatwienia pracy użytkowników w Azure. 
* **Parametry** — (pozostaw to pole puste)

Na koniec kliknij znacznik wyboru, aby rozpocząć tworzenie niestandardowych klaster HDInsight Hadoop. 

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <a name="headnode"></a>Dostęp do węzła głowy klaster Hadoop

Użytkownicy, należy włączyć dostęp zdalny do klastrów Hadoop platformy Azure, przed uzyskaniem dostępu węzła głównego klastrze Hadoop za pośrednictwem RDP. 

1. Zaloguj się na [**Klasyczny Portal Azure**](https://manage.windowsazure.com/), po lewej stronie wybierz pozycję **HDInsight** , wybierz klaster Hadoop z listy klastrów, kliknij kartę **Konfiguracja** , a następnie kliknij ikonę **Włączanie zdalnego** u dołu strony.
    
    ![Tworzenie obszaru roboczego](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)

2. W oknie **Konfigurowanie pulpitu zdalnego** Wprowadź pola Nazwa użytkownika i hasło, a następnie wybierz datę wygaśnięcia dla dostępu zdalnego. Następnie kliknij znacznik wyboru, aby włączyć dostęp zdalny do węzła głównego klastrze Hadoop.

    ![Tworzenie obszaru roboczego](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)
    
>[AZURE.NOTE] Nazwa użytkownika i hasło dla dostępu zdalnego nie są nazwę użytkownika i hasło używane podczas tworzenia klaster Hadoop. Są to oddzielny zestaw poświadczeń. Ponadto datę wygaśnięcia dostępu zdalnego musi być w ciągu 7 dni od daty bieżącej.

Po włączeniu dostępu zdalnego, kliknij pozycję **POŁĄCZ** u dołu strony, aby zdalnego do węzła głównego. Możesz zalogować się do węzła głównego klastrze Hadoop przez wprowadzenie poświadczeń dla użytkownika dostępu zdalnego określony wcześniej.

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

Następne kroki w procesie zaawansowanej analizy są mapowane w [Zespołu danych nauki proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) i może obejmować czynności, które przenoszenie danych do usługi HDInsight, procesu i przykładowe tak w przygotowanie do nauki z danych z nauki komputera Azure.

Zobacz, [jak przesyłać kwerendy gałęzi](machine-learning-data-science-move-hive-tables.md#submit) instrukcje na temat uzyskać dostęp do modułów Python, w których znajdują się w Anaconda z węzła głównego klastrze w funkcje zdefiniowane przez użytkownika (UDF), używane do procesu rekordy gałęzi przechowywane w klastrze.

 
