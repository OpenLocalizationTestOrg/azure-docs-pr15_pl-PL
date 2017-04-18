<properties
   pageTitle="Analizowanie danych czujnik z Burza Apache i HBase | Microsoft Azure"
   description="Dowiedz się, jak połączyć się burza Apache w wirtualnej sieci. Za pomocą Burza HBase przetworzyć czujnik danych z Centrum zdarzeń i wizualizowanie go z D3.js."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Analizowanie danych czujnik z Burza Apache, Centrum zdarzeń i HBase w HDInsight (Hadoop) 

Dowiedz się, jak przy użyciu Burza Apache HDInsight proces czujnik danych z Centrum zdarzeń Azure, zapisz go do Apache HBase na HDInsight i wizualizowanie go przy użyciu D3.js działającego jako aplikacji sieci Web programu Azure.

Szablon Azure Menedżera zasobów używanych w tym dokumencie przedstawia sposób tworzenia wielu Azure zasoby w grupie zasobów. W szczególności tworzy wirtualną sieć Azure, dwóch klastrów HDInsight (Burza i HBase) i Azure Web App. Node.js stosowania pulpit nawigacyjny sieci web w czasie rzeczywistym jest automatycznie używany aplikacji sieci web.

> [AZURE.NOTE] Przetestowano informacji w tym dokumencie i przykład pod warunkiem korzystania z systemem Linux HDInsight 3.3 i 3.4 wersji klaster.

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcję usługi Azure. Zobacz [Azure pobrać bezpłatną wersję próbną](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

    > [AZURE.IMPORTANT] Nie ma potrzeby istniejącym klastrem HDInsight; kroki opisane w tym dokumencie utworzy następujące zasoby:
    >
    > * Wirtualna sieć Azure
    > * Burza w klastrze HDInsight (systemem Linux węzły pracownik 2)
    > * HBase w klastrze HDInsight (systemem Linux węzły pracownik 2)
    > * Aplikacji sieci Web programu Azure znajduje się na pulpicie nawigacyjnym sieci web

* [Node.js](http://nodejs.org/): umożliwia wyświetlanie podglądu pulpitu nawigacyjnego sieci web lokalnie na środowiska programowania.

* [Java i JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): używany do projektowania topologii Burza.

* [Środowiska maven](http://maven.apache.org/what-is-maven.html): umożliwia tworzenie i kompilowania projektu.

* [Cyfra](http://git-scm.com/): używana do pobierania projektu z GitHub.

* Klient programu __SSH__ : używane do łączenia się klastrów HDInsight systemem Linux. Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz następujące dokumenty.

    * [Za pomocą SSH HDInsight z klientem systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Za pomocą SSH HDInsight kliencie Linux, Unix lub komputerów Mac](hdinsight-hadoop-linux-use-ssh-unix.md)

    > [AZURE.NOTE] Możesz również musi mieć dostęp do `scp` polecenia, które służy do kopiowania plików między środowiskiem lokalnym rozwoju i klaster HDInsight przy użyciu SSH.

## <a name="architecture"></a>Architektura

![diagram architektury](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

W tym przykładzie składa się z następujących składników:

* **Koncentratory zdarzenia Azure**: zawiera dane, które są zbierane z czujników. W tym przykładzie aplikacja jest pod warunkiem, że generuje dane.

* **Burza na HDInsight**: miejsce w czasie rzeczywistym przetwarzania danych z Centrum zdarzenia.

* **HBase na HDInsight**: zapewnia trwałych magazynu danych NoSQL danych po przetworzeniu przez Burza.

* **Usługa Azure wirtualną sieć**: umożliwia bezpieczne komunikacji między Burza na HDInsight i HBase na HDInsight klastrów.

    > [AZURE.NOTE] Wirtualnej sieci jest wymagana, aby korzystać z interfejsu API języka Java HBase klienta, jako nie jest dostępna na publicznej bramy dla klastrów HBase. Instalowania klastrów HBase i Burza do tej samej sieci wirtualnej umożliwia klaster Burza (lub innego systemu w wirtualnej sieci) w celu bezpośredniego dostępu HBase za pomocą interfejsu API klienta.

* **Witryna sieci Web pulpitu nawigacyjnego**: przykładowy pulpit nawigacyjny wykresy danych w czasie rzeczywistym.

    * Witryny sieci Web jest zaimplementowana w Node.js, więc można uruchamiać we wszystkich systemach operacyjnych klienta do testowania lub może zostać rozmieszczony na Azure witryn sieci Web.

    * [Socket.IO](http://socket.io/) jest używany w czasie rzeczywistym komunikacji między topologii Burza i witryny sieci Web.

        > [AZURE.NOTE] Jest to szczegółowa implementacja. Możesz użyć dowolnego architektura komunikacji, takich jak nieprzetworzonych WebSockets lub SignalR.

    * [D3.js](http://d3js.org/) służy do graficznego przedstawiania danych, które są wysyłane do witryny sieci Web.

> [AZURE.IMPORTANT] Dwa klastrów są wymagane, jak istnieje obsługiwana metoda utworzyć jeden klaster HDInsight zarówno burza, jak i HBase.

Topologia odczytuje dane z Centrum zdarzeń za pomocą klasy [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) i zapisuje dane w HBase za pomocą klasy [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html) . Komunikowanie się z witryny sieci Web jest osiągnąć przy użyciu [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).

Poniżej przedstawiono diagram topologii.

![diagram topologii](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [AZURE.NOTE] To jest widok bardzo uproszczony topologii. W czasie wykonywania wystąpienie każdego składnika jest tworzony dla każdej partition koncentratora zdarzenia, które są odczytywane. Te wystąpienia są rozmieszczane węzłów w klastrze, a dane są przesyłane między nimi w następujący sposób:
>
> * Dane z dziobek Analizator jest równoważenia obciążenia.
> * Dane z Analizator do pulpitu nawigacyjnego i HBase są pogrupowane według identyfikator urządzenia tak, aby wiadomości z tym samym urządzeniu przepływ zawsze do tego samego składnika.

### <a name="topology-components"></a>Składniki topologii

* **Dziobek EventHub**: dziobek jest podany Burza Apache wersję 0.10.0 lub nowszy.

    > [AZURE.NOTE] Dziobek koncentratory zdarzenia, w tym przykładzie użyto wymaga Burza HDInsight klaster wersji 3.3 lub 3.4. Aby uzyskać informacje dotyczące sposobu używania koncentratory zdarzeń za pomocą starszej wersji Burza na HDInsight zobacz [zdarzenia procesu z Azure koncentratory wydarzenia z Burza na HDInsight](hdinsight-storm-develop-java-event-hub-topology.md).

* **ParserBolt.java**: dane, które są wysyłane przez dziobek jest nieprzetworzonych JSON, a czasami więcej niż jedno zdarzenie są wysyłane jednocześnie. Ten błyskawicy przedstawiono sposób odczytywania danych dostarczanych przez dziobek i dodaj go do nowego strumienia jako krotki, która zawiera wiele pól.

* **DashboardBolt.java**: to przedstawiono sposób użycia Biblioteka klienta Socket.io dla języka Java wysyłanie danych czasie rzeczywistym do pulpitu nawigacyjnego w sieci web.

W tym przykładzie użyto framework [strumień](https://storm.apache.org/releases/0.10.0/flux.html) , więc definicja topologii znajduje się w plikach YAML. Istnieją dwa:

* __nie hbase.yaml__ - Użyj tego pliku podczas testowania topologii w środowisku rozwoju. Nie używa składniki HBase, ponieważ nie masz dostępu do interfejsu API języka Java HBase z poza wirtualnej sieci, które znajdują się w grupie.

* __z hbase.yaml__ - Użyj tego pliku, wdrażając topologii do klastrów Burza. Używa składniki HBase, ponieważ jest on w tej samej sieci wirtualnej klaster HBase.

## <a name="prepare-your-environment"></a>Przygotowania środowiska

Przed użyciem w tym przykładzie, musisz utworzyć koncentratora zdarzenia Azure topologii Burza odczytuje z.

### <a name="configure-event-hub"></a>Konfigurowanie Centrum zdarzenia

Centrum zdarzenie jest źródłem danych, w tym przykładzie. Wykonaj następujące czynności, aby utworzyć nowe Centrum zdarzeń.

1. [Azure portal](https://portal.azure.com), wybierz pozycję **+ Nowy** -> __Internet rzeczy__ -> __Koncentratory zdarzenia__.

2. Karta __Tworzenie Namespace__ wykonywanie następujących zadań:

    1. Wprowadź __nazwę__ obszaru nazw.
    2. Wybierz poziom cennik. __Podstawowe__ wystarcza w tym przykładzie.
    3. Wybierz Azure __subskrypcji__ , aby użyć.
    4. Wybierz istniejącą grupą zasobów lub Utwórz nowy.
    5. Wybierz __lokalizację__ dla Centrum wydarzenie.
    6. Wybierz pozycję __Przypnij do pulpitu nawigacyjnego__, a następnie kliknij przycisk __Utwórz__.

3. Po zakończeniu procesu tworzenia, zostanie wyświetlona karta koncentratory zdarzeń dla obszaru nazw. W tym miejscu zaznacz __+ Dodaj Centrum zdarzenia__. Na karta __Tworzenie Centrum zdarzenia__ wprowadź nazwę __sensordata__ , a następnie wybierz pozycję __Utwórz__. Pozostaw pozostałe pola na wartości domyślne.

4. Karta koncentratory zdarzeń dla obszaru nazw zaznacz __Koncentratory wydarzenie__. Wybierz pozycję __sensordata__ .

5. Karta dla sensordata Centrum zdarzenia zaznacz __zasady dostępu udostępnione__. __+ Dodaj__ łącze umożliwia dodawanie następujących zasad:


  	| Nazwa zasady | Roszczeń |
  	| ----- | ----- |
  	| urządzenia | Wyślij |
  	| Burza | Odsłuchać |

5. Zaznacz obie zasady i zanotuj wartość __Klucza podstawowego__ . Konieczne będzie wartość dla obu zasad w przyszłości kroków.

## <a name="download-and-configure-the-project"></a>Pobieranie i konfigurowanie projektu

Pobierz projektu z GitHub należy wykonać następujące kroki.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Po zakończeniu polecenia masz następującą strukturę katalogów:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal
                no-hbase.yaml - topology definition for local testing
                with-hbase.yaml - topology definition that uses HBase in a virutal network
            src/ - the Java bolts
            dev.properties - contains configuration values for your environment
        dashboard/nodejs/ - this is the node.js web dashboard
        SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] Ten dokument nie pierwotnemu pełne szczegóły kod zawarte w tym przykładzie; kod w pełni stanowiący komentarz.

Otwórz plik **hdinsight-eventhub-example/TemperatureMonitor/dev.properties** i Dodaj informacje o Centrum zdarzeń do następujących wierszy:

    eventhub.read.policy.name: storm
    eventhub.read.policy.key: KeyForTheStormPolicy
    eventhub.namespace: YourNamespace
    eventhub.name: sensordata

> [AZURE.NOTE] W tym przykładzie przyjęto założenie, używana __Burza__ jako nazwa zasady, która ma roszczenia __odsłuchać__ , a które Twoim Centrum zdarzenia o nazwie __sensordata__.

 Zapisz plik po dodaniu tych informacji.

## <a name="compile-and-test-locally"></a>Skompilować i przetestować lokalnie

Przed sprawdzeniem, możesz rozpocząć pulpitu nawigacyjnego, aby wyświetlić wynik topologii i wygenerować dane mają być przechowywane w Centrum zdarzenia.

> [AZURE.IMPORTANT] Podczas testowania lokalnie, co w interfejsie API języka Java dla klastrów HBase nie są dostępne z poza wirtualną sieć Azure, która zawiera klastrów HBase część tej topologii nie jest aktywna.

### <a name="start-the-web-application"></a>Uruchom aplikację sieci web

1. Otwórz nowy wiersz polecenia lub terminal i zmiana katalogu **hdinsight-eventhub przykładowy/pulpit nawigacyjny**, a następnie użyj następującego polecenia, aby zainstalować zależności wymagane przez aplikację sieci web:

        npm install

2. Użyj następującego polecenia do uruchamiania aplikacji sieci web:

        node server.js

    Powinien zostać wyświetlony komunikat podobny do następującego:

        Server listening at port 3000

2. Otwórz przeglądarkę sieci web i wprowadź **http://localhost:3000-** jako adres. Powinien zostać wyświetlony strony podobny do następującego:

    ![pulpit nawigacyjny sieci Web](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

    Pozostaw otwarte tego wiersza polecenia lub terminal. Po przetestowaniu za pomocą Ctrl + C, aby zatrzymać serwer sieci web.

### <a name="start-generating-data"></a>Rozpoczynanie generowania danych

> [AZURE.NOTE] Czynności opisane w tej sekcji za pomocą Node.js tak, aby można było na wszystkich platformach. Aby uzyskać inne przykłady języka Zobacz katalogu **SendEvents** .

1. Otwórz nowy wiersz polecenia, powłoki lub terminal i zmiana katalogu **hdinsight — eventhub — przykład SendEvents-nodejs**, a następnie użyj następującego polecenia, aby zainstalować zależności potrzebne aplikacji:

        npm install

2. Otwórz plik **app.js** w edytorze tekstów, a następnie dodaj informacje Centrum zdarzeń, które uzyskany wcześniej:

        // ServiceBus Namespace
        var namespace = 'YourNamespace';
        // Event Hub Name
        var hubname ='sensordata';
        // Shared access Policy name and key (from Event Hub configuration)
        var my_key_name = 'devices';
        var my_key = 'YourKey';
    
    > [AZURE.NOTE] W tym przykładzie założono, użycie __sensordata__ jako nazwę Centrum zdarzeń i __urządzeń__ jako nazwę zasady, którą ma __wysłać__ roszczenia.

2. Wstawianie nowych wpisów w Centrum wydarzenia przy użyciu następującego polecenia:

        node app.js

    Powinien zostać wyświetlony kilku wierszy dane wyjściowe, które zawierają dane wysyłane do Centrum zdarzenia. Powoduje wyświetlenie tych podobny do następującego:

        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="start-the-topology"></a>Rozpoczynanie topologii

2. Otwórz nowy wiersz polecenia, powłoki lub terminal i zmień katalogów __Hdinsight — eventhub — przykład TemperatureMonitor__, a następnie użyj następującego polecenia, aby uruchomić topologii:

        mvn compile exec:java -Dexec.args="--local -R /no-hbase.yaml --filter dev.properties"
    
    Jeśli korzystasz z programu PowerShell, użyj zamiast tego następujące czynności:

        mvn compile exec:java "-Dexec.args=--local -R /no-hbase.yaml --filter dev.properties"

    > [AZURE.NOTE] Jeśli jesteś w systemie Linux i Unix-OS X i jest [zainstalowanie Burza w środowisku rozwoju](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), możesz w zamian użyć następujących poleceń:
    >
    > `mvn compile package`
    > `storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /no-hbase.yaml`

    Zostanie uruchomiony topologii zdefiniowane w pliku __hbase.yaml nie__ w trybie lokalnym. Wartości znajdujące się w pliku __dev.properties__ umożliwiają informacje o połączeniu dla koncentratorów zdarzenia. Po rozpoczęciu topologii odczytuje wpisy z Centrum zdarzeń i wysyła je do pulpitu nawigacyjnego na komputerze lokalnym. Powinien zostać wyświetlony wiersze są wyświetlane na pulpicie nawigacyjnym sieci web, podobny do następującego:

    ![pulpit nawigacyjny z danymi](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. Po uruchomieniu pulpitu nawigacyjnego za pomocą `node app.js` polecenia z poprzednie kroki, aby wysłać nowe dane do koncentratorów zdarzenia. Ponieważ wartości temperatury są losowo wygenerowany, wykresu należy zaktualizować pokazanie dużych zmian temperatury.

    > [AZURE.NOTE] Użytkownik musi być w katalogu __Hdinsight — eventhub — przykład/SendEvents/Nodejs__ podczas korzystania z `node app.js` polecenia.

3. Po sprawdzeniu, czy to działa, Zatrzymaj topologii za pomocą klawiszy Ctrl + C. Za pomocą klawisze Ctrl + C, aby zatrzymać serwer sieci web lokalne również.

## <a name="create-a-storm-and-hbase-cluster"></a>Utworzyć klaster Burza i HBase

Aby uruchomić topologii HDInsight i umożliwić śruby HBase, możesz utworzyć nowy klaster Burza i klaster HBase. Czynności opisane w tej sekcji utworzyć nową, wirtualną sieć Azure i Burza i HBase klaster w wirtualnej sieci przy użyciu [szablonu Azure Menedżera zasobów](../resource-group-template-deploy.md) . Szablon również tworzy aplikacji sieci Web programu Azure i wdrożeniu jej kopię pulpitu nawigacyjnego do niego.

> [AZURE.NOTE] Wirtualna sieć jest używana, dzięki czemu topologii uruchamiania w klastrze Burza może bezpośrednio komunikować się z klastrem HBase za pomocą interfejsu API języka Java HBase.

Szablon Menedżera zasobów używanych w tym dokumencie znajduje się w kontenerze obiektów blob publicznej w __https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json__.

1. Kliknij przycisk, aby zalogować się do Azure i otwórz szablon Menedżera zasobów w portalu Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Z karta **Parametry** wprowadź następujące informacje:

    ![Parametry HDInsight](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
    
    * **BASECLUSTERNAME**: Ta wartość będzie używana jako podstawa nazwę Burza i klastrów HBase. Na przykład wprowadzanie __hdi__ utworzy klastrze Burza o nazwie __hdi Burza__ i klaster HBase o nazwie __hbase hdi__.
    * __CLUSTERLOGINUSERNAME__: nazwa użytkownika administratora dla klastrów Burza i HBase.
    * __CLUSTERLOGINPASSWORD__: hasło użytkownika administratora dla klastrów Burza i HBase.
    * __SSHUSERNAME__: SSH użytkownikowi tworzenie dla klastrów Burza i HBase.
    * __SSHPASSWORD__: hasło użytkownika SSH dla klastrów Burza i HBase.
    * __Lokalizacja__: region, który klastrów zostanie utworzony w.
    
    Kliknij __przycisk OK__ , aby zapisać parametry.
    
3. Aby utworzyć nową grupę zasobów lub wybierz istniejący za pomocą sekcji __Grupa zasobów__ .

4. W menu rozwijanym __lokalizacji grupa zasobów__ jako zaznaczony w parametrze __Lokalizacja__ wybierz tej samej lokalizacji.

5. Zaznacz __warunki prawne__, a następnie wybierz pozycję __Utwórz__.

6. Na końcu __numeru Pin do pulpitu nawigacyjnego__ i wybierz pozycję __Utwórz__. Aby utworzyć klastrów zajmie około 20 minut.

Po utworzeniu zasobów nastąpi przekierowanie do kart grupy zasobów, zawierającą klastrów i sieci web pulpitu nawigacyjnego.

![Karta Grupa zasobów dla vnet i klastrów](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [AZURE.IMPORTANT] Zauważyć, że nazwy klastrów HDInsight są __BASENAME Burza__ i __hbase BASENAME__, gdzie BASENAME jest nazwą opisane w szablonie. Te nazwy użyje w krokach później podczas łączenia się klastrów. Należy również zauważyć, że nazwa witryny pulpitu nawigacyjnego jest __pulpit nawigacyjny basename__. Podanie hasła później podczas wyświetlania pulpitu nawigacyjnego.

## <a name="configure-the-dashboard-bolt"></a>Konfigurowanie śruby pulpitu nawigacyjnego

Aby można było wysyłać dane na pulpicie nawigacyjnym używany jako aplikacji sieci web, należy zmodyfikować następujący wiersz w pliku __dev.properties__ :

    dashboard.uri: http://localhost:3000

Zmienianie `http://localhost:3000` do `http://BASENAME-dashboard.azurewebsites.net` i Zapisz plik. Zamień __BASENAME__ nazwę podstawową podanych w poprzednim kroku. Umożliwia także Grupa zasobów utworzone wcześniej wybierz pulpit nawigacyjny i wyświetlanie adresu URL.

## <a name="create-the-hbase-table"></a>Tworzenie tabeli HBase

Aby zapisać dane w HBase, możemy utworzyć tabelę. Chcesz zwykle wstępnie utworzyć zasoby, które należy zapisać burza, jako próby utworzenia zasoby z wewnątrz topologii Burza może spowodować, rozłożone kopie kod próby utworzenia tego zasobu. Tworzenie zasobów poza topologii i po prostu użyć Burza do czytania i pisania i analizy.

1. Nawiązywanie połączenia z klastrem HBase przy użyciu SSH użytkownika i hasło do szablonu podczas tworzenia klaster za pomocą SSH. Na przykład nawiązywanie połączenia za pomocą `ssh` polecenia, można użyć następującej składni:

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
    
    W tym poleceniu __nazwa_użytkownika__ należy zastąpić nazwę użytkownika SSH, podana podczas tworzenia klaster i __BASENAME__ z podanymi nazwę podstawową. Gdy zostanie wyświetlony monit, wprowadź hasło dla użytkownika SSH.

2. Z sesji SSH Uruchom powłoki HBase.

        hbase shell
    
    Po załadowaniu powłokę, zostanie wyświetlona `hbase(main):001:0>` wiersza.

3. Z poziomu powłoki HBase wpisz następujące polecenie, aby utworzyć tabelę do przechowywania danych czujnika:

        create 'SensorData', 'cf'

4. Sprawdź, czy tabela został utworzony przy użyciu następującego polecenia:

        scan 'SensorData'
        
    To zwraca informacje podobne do następującego przykładu wskazuje, że są 0 wierszy w tabeli.
    
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Wprowadź poniższe czynności, aby zakończyć powłoki HBase:

        exit

## <a name="configure-the-hbase-bolt"></a>Konfigurowanie śruby HBase

Zapisywanie HBase w grupie burza, musisz podać śruby HBase z szczegóły konfiguracji klaster HBase. Najprostszym sposobem na tym jest pobieranie __hbase site.xml__ z klastrem i dołączyć go do projektu. Należy również Usuń komentarze kilka zależności w pliku __pom.xml__ , które załadować składnik hbase Burza i wymaganych zależności.

> [AZURE.IMPORTANT] Należy również pobrać plik hbase.jar Burza opisane na swojej Burza na HDInsight 3.3 lub 3.4 klastrze; Ta wersja jest skompilowany do współdziałania z HBase 1.1.x, która jest używana do HBase na HDInsight 3.3 i 3.4 klastrów. Użycie składnika hbase Burza z innego miejsca, może być można skompilować przed ze starszej wersji programu HBase.

### <a name="download-the-hbase-sitexml"></a>Pobierz hbase site.xml

W wierszu polecenia za pomocą połączenia o pobranie pliku __hbase site.xml__ z klastrem. W poniższym przykładzie Zamień __nazwa_użytkownika__ użytkownika SSH podana podczas tworzenia klaster i __BASENAME__ o nazwie podstawowej podanych powyżej. Gdy zostanie wyświetlony monit, wprowadź hasło dla użytkownika SSH. Zamienianie `/path/to/TemperatureMonitor/resources/hbase-site.xml` ścieżką do tego pliku w programie project TemperatureMonitor.

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

Pobierze __hbase site.xml__ do określonej lokalizacji.

### <a name="download-and-install-the-storm-hbase-component"></a>Pobierz i zainstaluj składnik hbase Burza

1. W wierszu polecenia za pomocą połączenia o pobranie pliku __hbase.jar Burza__ z klaster Burza. W poniższym przykładzie Zamień __nazwa_użytkownika__ użytkownika SSH podana podczas tworzenia klaster i __BASENAME__ o nazwie podstawowej podanych powyżej. Gdy zostanie wyświetlony monit, wprowadź hasło dla użytkownika SSH.

        scp USERNAME@storm-BASENAME-ssh.azurehdinsight.net:/usr/hdp/current/storm-client/contrib/storm-hbase/storm-hbase*.jar .

    Spowoduje to pobieranie pliku o nazwie `storm-hbase-####.jar`, gdzie ### jest numer wersji Burza dany klaster. Zanotuj tego numeru, ponieważ jest używana później.

2. Użyj następującego polecenia, aby zainstalować ten składnik do lokalnego środowiska Maven repozytorium na środowiska programowania. Dzięki temu środowiska Maven znaleźć pakiet podczas kompilowania projektu. Zamienianie __####__ z numerem wersji, zawarte w polu Nazwa pliku.

        mvn install:install-file -Dfile=storm-hbase-####.jar -DgroupId=org.apache.storm -DartifactId=storm-hbase -Dversion=#### -Dpackaging=jar
    
    Jeśli korzystasz z programu PowerShell, użyj następującego polecenia:

        mvn install:install-file "-Dfile=storm-hbase-####.jar" "-DgroupId=org.apache.storm" "-DartifactId=storm-hbase" "-Dversion=####" "-Dpackaging=jar"

### <a name="enable-the-storm-hbase-component-in-the-project"></a>Włącz składnik hbase Burza w projekcie

1. Otwórz plik __TemperatureMonitor/pom.xml__ i Usuń następujące wiersze:

        <!-- uncomment this section to enable the hbase-bolt
        end comment for hbase-bolt section -->
    
    > [AZURE.IMPORTANT] Usunąć tylko te dwie linie; Nie usuwaj żadnych wierszy między nimi.
    
    Dzięki temu kilka składników, które są wymagane komunikowania się z HBase przy użyciu śruby hbase.

2. Znajdź następujące wiersze, a następnie Zamień __####__ z numerem wersji wcześniejszej pobrany plik hbase Burza.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-hbase</artifactId>
            <version>####</version>
        </dependency>

    > [AZURE.IMPORTANT] Numer wersji musi odpowiadać wersji, który został użyty podczas instalowania składnika do lokalnego repozytorium środowiska Maven jako środowiska Maven korzysta z tych informacji do załadować składnika podczas tworzenia projektu.

2. Zapisz plik __pom.xml__ .

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Tworzenie pakietu i wdrażanie rozwiązania HDInsight

W środowisku rozwoju wykonaj następujące czynności, aby wdrożyć topologii Burza klaster Burza.

1. Z katalogu __TemperatureMonitor__ Użyj następującego polecenia do wykonywania nową kompilację i utworzyć pakiet SŁOIK z Twojego projektu:

        mvn clean compile package

    Spowoduje to utworzenie pliku o nazwie **TemperatureMonitor-1.0-SNAPSHOT.jar** w katalogu **docelowym** projektu.

2. Aby przekazać plik __TemperatureMonitor-1.0-SNAPSHOT.jar__ ma klaster Burza za pomocą połączenia. W poniższym przykładzie Zamień __nazwa_użytkownika__ użytkownika SSH podana podczas tworzenia klaster i __BASENAME__ o nazwie podstawowej podanych powyżej. Gdy zostanie wyświetlony monit, wprowadź hasło dla użytkownika SSH.

        scp target\TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar
    
    > [AZURE.NOTE] Może potrwać kilka minut, aby przekazać plik, jak to będzie miało kilka MB.

    Użyj połączenia przekazać plik __dev.properties__ zawiera dane używane do łączenia się koncentratory zdarzeń i pulpitu nawigacyjnego.

        scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties

3. Po przesłaniu plików nawiązywanie połączenia z klastrem przy użyciu SSH.

        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. Od sesji SSH Użyj następującego polecenia, aby uruchomić topologii.

        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /with-hbase.yaml --filter dev.properties
    
    Zostanie uruchomiony topologii przy użyciu topologii definicji w pliku __z hbase.yaml__ i wartości konfiguracji w pliku __dev.properties__ .

3. Po rozpoczęciu topologii, otwórz przeglądarkę sieci aby witryny sieci Web opublikowany Azure, a następnie użyj `node app.js` polecenie wysyłanie danych do Centrum zdarzenia. Powinny być widoczne na pulpicie nawigacyjnym sieci web aktualizacji do wyświetlania informacji.

    ![pulpit nawigacyjny](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>Wyświetlanie danych HBase

Po zostały przesłane dane przy użyciu topologii `node app.js`, wykonaj następujące czynności, nawiązać połączenie z HBase i sprawdź, czy dane zostały zapisane w tabeli został utworzony wcześniej.

1. Nawiązywanie połączenia z klastrem HBase za pomocą SSH.

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. Z sesji SSH Uruchom powłoki HBase.

        hbase shell
    
    Po załadowaniu powłokę, zostanie wyświetlona `hbase(main):001:0>` wiersza.

2. Wyświetlanie wierszy z tabeli:

        scan 'SensorData'
        
    To powinna zwrócić informacje podobne do poniższych wskazuje, że są 0 wierszy w tabeli.
    
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds

    > [AZURE.NOTE] Operacja skanowania tylko może zwrócić maksymalnie 10 wierszy z tabeli.

## <a name="delete-your-clusters"></a>Usuwanie klastrów

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Aby usunąć klastrów, miejsca do magazynowania i web app w tym samym czasie, Usuń grupa zasobów, zawierającego je.

## <a name="next-steps"></a>Następne kroki

Teraz zapoznaniu za pomocą Burza Odczyt danych koncentratory zdarzenia, zapisz go do HBase i wizualizowanie informacje znajdujące się na zewnętrznym pulpitu nawigacyjnego za pomocą Socket.io i D3.js.

* Aby uzyskać więcej przykładów topologii Burza z usługi HDInsight zobacz:

    * [Przykład topologii dla Burza na HDInsight](hdinsight-storm-example-topology.md)

* Więcej informacji na temat burzy Apache znajduje się w witrynie [Burza Apache](https://storm.incubator.apache.org/) .

* Aby uzyskać więcej informacji o HBase na HDInsight zobacz [HBase HDInsight Przegląd](hdinsight-hbase-overview.md).

* Aby uzyskać więcej informacji o Socket.io odwiedź witrynę [socket.io](http://socket.io/) .

* Aby uzyskać więcej informacji o D3.js zobacz [D3.js - dokumentów zmiennych danych](http://d3js.org/).

* Aby uzyskać informacje o tworzeniu topologii w Java zobacz [topologii opracowywanie Java dla Burza Apache na HDInsight](hdinsight-storm-develop-java-topology.md).

* Aby uzyskać informacje o tworzeniu topologii w .NET zobacz [topologii opracowywanie C# dla Burza Apache na HDInsight przy użyciu programu Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com
