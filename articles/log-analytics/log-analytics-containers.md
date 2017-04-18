<properties
    pageTitle="Rozwiązanie kontenerów w dzienniku analizy | Microsoft Azure"
    description="Rozwiązanie kontenerów w dzienniku analizy ułatwia wyświetlanie i zarządzanie nimi hosty kontener Docker w jednym miejscu."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>



# <a name="containers-preview-solution-log-analytics"></a>Rozwiązanie kontenerów (wersja Preview) analizy dziennika

W tym artykule opisano konfigurowanie i używanie rozwiązanie kontenerów w analizy dziennika, który umożliwia wyświetlanie i zarządzanie nimi hosty kontener Docker w jednym miejscu. Docker jest systemem wirtualizacji oprogramowanie użyte do utworzenia kontenerów automatyzujących wdrażania oprogramowania do infrastruktury informatycznej.

Dzięki rozwiązanie można wyświetlić kontenery, które są uruchomione na hosty kontenera i obrazy działają w kontenerach. Możesz wyświetlić szczegółowe informacje inspekcji przedstawiająca polecenia używane z kontenerami. I kontenerów można rozwiązać, wyświetlając i wyszukiwanie scentralizowane dzienniki bez konieczności zdalnie wyświetlić Docker hosts. Można znaleźć kontenery, które mogą być zakłócenia i dużo nadmiarowego zasoby na hoście. I scentralizowane Procesora, pamięci, miejsca do magazynowania i informacje do użycia i wydajności sieci dla kontenerów można przeglądać.

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązanie

Aby zainstalować i skonfigurować rozwiązanie, korzystając z następujących informacji.

Dodawanie kontenerów rozwiązanie do obszaru roboczego usługi OMS przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).

Istnieją dwa sposoby instalowania i Docker za pomocą usługi OMS:

- Na obsługiwane systemy operacyjne Linux Instalowanie i uruchamianie Docker zainstalować i skonfigurować agenta usługi OMS Linux
- Na CoreOS Instalowanie i uruchamianie Docker, a następnie skonfiguruj OMSAgent, aby uruchomić znajdujących się w kontenerze

Przejrzyj obsługiwanych wersji systemu operacyjnego Docker i Linux oraz dla hosta kontener na [GitHub](https://github.com/Microsoft/OMS-docker).

>[AZURE.IMPORTANT] Docker musi być uruchomiony **przed** zainstalowaniem [Agenta usługi OMS Linux](log-analytics-linux-agents.md) na hosty kontener. Jeśli został już zainstalowany agent przed zainstalowaniem Docker, musisz ponownie zainstalować agenta usługi OMS Linux. Aby uzyskać więcej informacji o Docker zobacz [Docker witryny sieci Web](https://www.docker.com).

Potrzebujesz następujące ustawienia skonfigurowane na hosty kontenera, aby można było monitorować kontenerów.

## <a name="configure-settings-for-the-linux-container-host"></a>Konfigurowanie ustawień dla hosta kontenera Linux

Po zainstalowaniu Docker umożliwia skonfigurowanie agenta do użytku z Docker następujące ustawienia dla hosta kontener. CoreOS nie obsługuje tej metody konfiguracji.

### <a name="to-configure-settings-for-the-container-host---systemd-suse-opensuse-centos-7x-rhel-7x-and-ubuntu-15x-and-higher"></a>Aby skonfigurować ustawienia dla hosta kontenera - systemd (SUSE, openSUSE, CentOS 7.x, RHEL 7.x i Ubuntu 15.x lub nowszy)

1. Edytowanie docker.service, aby dodać następujące czynności:

    ```
    [Service]
    ...
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ...
    ```

2. Dodawanie $DOCKER\_ZDECYDUJE się w &quot;demon = / usr/pojemnika docker ExecStart&quot; w pliku docker.service. Używanie w poniższym przykładzie.

    ```
    [Service]
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS
    ```

3. Uruchom ponownie usługę Docker. Na przykład:

    ```
    sudo systemctl restart docker.service
    ```

### <a name="to-configure-settings-for-the-container-host---upstart-ubuntu-14x"></a>Aby skonfigurować ustawienia dla hosta kontenera - Upstart (Ubuntu 14.x)

1. Edytowanie /etc/default/docker, a następnie dodaj następujące:

    ```
    DOCKER_OPTS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Zapisz plik, a następnie ponownie uruchom usługi Docker i usługi OMS.

    ```
    sudo service docker restart
    ```

### <a name="to-configure-settings-for-the-container-host---amazon-linux"></a>Aby skonfigurować ustawienia dla hosta kontenera - Amazon Linux

1. Edytowanie /etc/sysconfig/docker, a następnie dodaj następujące:

    ```
    OPTIONS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Zapisz plik, a następnie ponownie uruchom usługę Docker.

    ```
    sudo service docker restart
    ```

## <a name="configure-settings-for-coreos-containers"></a>Konfigurowanie ustawień dla kontenerów CoreOS

Po zainstalowaniu Docker, użyj poniższych ustawień dla CoreOS, aby uruchomić Docker i Tworzenie kontenera. Można użyć dowolnej obsługiwanej wersji programu Linux — w tym CoreOS, przy użyciu tej metody konfiguracji. Musisz swój [Identyfikator obszaru roboczego usługi OMS i klucz](log-analytics-linux-agents.md).

### <a name="to-use-oms-for-all-containers-with-coreos"></a>Aby użyć usługi OMS dla wszystkich kontenerów z CoreOS

- Rozpocznij kontener usługi OMS, który chcesz monitorować. Modyfikowanie i użyć następującego przykładu.

  ```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25224:25224/udp -p 127.0.0.1:25225:25225 --name="omsagent" --log-driver=none --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-agent-to-one-in-a-container"></a>Przenoszenie się z za pomocą zainstalowanych agenta do jednego w kontenerze

Jeśli wcześniej używane agenta bezpośrednio zainstalowany i chcesz użyć agenta uruchomiony w kontenerze, musisz najpierw usunąć OMSAgent. Zobacz [kroki, aby zainstalować agenta usługi OMS Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md).

## <a name="containers-data-collection-details"></a>Szczegóły zbioru danych kontenerów

Rozwiązanie kontenerów zbiera różnych wskaźników i dziennika dane dotyczące wydajności z kontenera hosts i kontenerów używania agentów usługi OMS Linux, które są włączone i OMSAgent uruchomiony w kontenerach.

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące sposobu dane są zbierane dla kontenerów.

| platformy | Agent usługi OMS Linux | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Linux|![Tak](./media/log-analytics-containers/oms-bullet-green.png)|![Brak](./media/log-analytics-containers/oms-bullet-red.png)|![Brak](./media/log-analytics-containers/oms-bullet-red.png)|            ![Brak](./media/log-analytics-containers/oms-bullet-red.png)|![Brak](./media/log-analytics-containers/oms-bullet-red.png)| co 3 minut|


Poniższa tabela Pokaż przykłady typów danych zebranych przez rozwiązanie kontenery:

| Typ danych | Pola |
| --- | --- |
| Wydajność hosts i kontenerów | Komputer, nazwa_obiektu, CounterName & #40; % czasu procesora, dysku odczytuje MB, dysk zapisuje MB, MB użycia pamięci, bajtów otrzymanych sieci, sieci wysyłanie bajty, procesor s zastosowania sieci & #41; równowartości, TimeGenerated, Ścieżka_licznika, SourceSystem |
| Kontener zapasów | TimeGenerated, komputer, nazwę kontenera ContainerHostname, obraz, ImageTag, ContinerState, ExitCode, EnvironmentVar, polecenie, CreatedTime, StartedTime, FinishedTime, SourceSystem, Identyfikator_kontenera, ImageID |
| Kontener obraz zapasów | TimeGenerated, komputer, obraz, ImageTag, ImageSize, VirtualSize, uruchamianie, wstrzymywane, zatrzymana, nie powiodło się, SourceSystem, ImageID, TotalContainer |
| Kontener dziennika | TimeGenerated, komputer, identyfikator obrazu, nazwę kontenera LogEntrySource, LogEntry, SourceSystem, Identyfikator_kontenera |
| Dziennik usługi kontenera | TimeGenerated, komputer, TimeOfCommand, obraz, polecenie, SourceSystem, Identyfikator_kontenera |

## <a name="monitor-containers"></a>Monitorowanie kontenerów

Po umieszczeniu rozwiązanie włączone w portalu usługi OMS, zobaczysz kafelków **kontenerów** przedstawiający informacje podsumowujące dotyczące hosty kontenerów i kontenerów działa hosts.

![Kontenerach kafelków](./media/log-analytics-containers/containers-title.png)

Fragmentu zawiera omówienie kontenerów ile masz w środowisku, czy masz niepowodzeniem uruchomiony lub zatrzymany.

### <a name="using-the-containers-dashboard"></a>Przy użyciu kontenerów pulpitu nawigacyjnego

Kliknij Kafelek **kontenerów** . Stamtąd zobaczysz widoki zorganizowane według:

- Kontener zdarzenia
- Błędy
- Stan kontenerów
- Kontener obraz zapasów
- Wydajność Procesora i pamięci

Każdy okienko na pulpicie nawigacyjnym jest będący wizualną reprezentacją uruchamianego na zebranych danych wyszukiwania.

![Pulpit nawigacyjny kontenerów](./media/log-analytics-containers/containers-dash01.png)

![Pulpit nawigacyjny kontenerów](./media/log-analytics-containers/containers-dash02.png)

W karta **Kontener stanu** kliknij górnym obszarze, tak jak pokazano poniżej.

![Stan kontenerów](./media/log-analytics-containers/containers-status.png)

Zostanie otwarty przeszukiwanie dziennika wyświetlanie informacji o hosts i kontenerów uruchomiony w nich.

![Przeszukiwanie dziennika dla kontenerów](./media/log-analytics-containers/containers-log-search.png)

W tym miejscu możesz edytować kwerendy wyszukiwania modyfikowania go do znajdowania określonych informacji, że Cię interesuje. Aby uzyskać więcej informacji na temat wyszukiwania dziennika zobacz [wyszukiwania dziennika analizy dziennika](log-analytics-log-searches.md).

Na przykład możesz zmodyfikować kwerendę wyszukiwania, tak aby wszystkie kontenery przestał zamiast uruchomionego kontenerów jest wyświetlany, zmieniając typ **uruchamiania** na **zatrzymania** w kwerendzie wyszukiwania.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Rozwiązywanie problemów z znajdując kontenera nie powiodło się

Usługi OMS oznacza kontenera jako **nie powiodło się** , jeśli został zakończony z kodem zakończenia zera. Można wyświetlić podsumowanie błędów i awarii w środowisku w karta **Failed kontenerów** .

### <a name="to-find-failed-containers"></a>Aby znaleźć nie powiodło się kontenerów

1. Kliknij pozycję Karta **Zdarzeń kontener** .  
  ![zdarzenia kontenerów](./media/log-analytics-containers/containers-events.png)
2. Zostanie wyświetlona przeszukiwanie dziennika wyświetlania stanu kontenery, podobny do następującego.  
  ![Stan kontenerów](./media/log-analytics-containers/containers-container-state.png)
3. Następnie kliknij wartość nie powiodło się, aby wyświetlić dodatkowe informacje, takie jak rozmiar obrazu i liczby obrazów zatrzymane, a nie powiodło się. Rozwiń pozycję **Pokaż więcej** , aby wyświetlić identyfikator obrazu.  
  ![kontenery nie powiodło się](./media/log-analytics-containers/containers-state-failed.png)
4. Następnie znajdź kontener, w którym jest uruchomiony ten obraz. Wpisz następujące do kwerendy wyszukiwania.
  `Type=ContainerInventory <ImageID>`Spowoduje to wyświetlenie dzienniki. Można przewijać, aby wyświetlić kontenera nie powiodło się.  
  ![kontenery nie powiodło się](./media/log-analytics-containers/containers-failed04.png)


## <a name="search-logs-for-container-data"></a>Dzienniki wyszukiwania danych kontenera

Przy rozwiązywaniu konkretnego problemu, może pomóc w celu wyświetlenia miejsce, w którym występuje w środowisku. Następujących typów dzienników ułatwi tworzenie kwerend zwraca informacje potrzebne.

- **ContainerInventory** — Użyj tego typu, gdy chcesz, aby informacje o lokalizacji kontenera, jakie są ich nazwy i co obrazów korzystasz z.
- **ContainerImageInventory** — Użyj tego typu, gdy próbujesz informacje zorganizowane przez obraz i wyświetlania obrazu informacje, takie jak obraz identyfikatory i rozmiarów.
- **ContainerLog** — Użyj tego typu umożliwia znajdowanie informacji dziennika konkretnego problemu i wpisów.
- **ContainerServiceLog** — Użyj tego typu, gdy próbujesz informacje dziennik inspekcji dla demona Docker, takich jak rozpoczęcie, Zatrzymaj, usuń lub polecenia pobieraj.

### <a name="to-search-logs-for-container-data"></a>Aby wyszukać dzienniki kontenera danych

- Wybierz obraz, który nie powiodło się ostatnio i znaleźć dzienników błędów. Zacznij od znajdowaniu nazwy kontenera uruchomionym obrazu przy użyciu wyszukiwania **ContainerInventory** . Na przykład wyszukiwanie`Type=ContainerInventory ubuntu Failed`  
    ![Wyszukiwanie Ubuntu kontenerów](./media/log-analytics-containers/search-ubuntu.png)

  Zanotuj nazwę kontenera obok **nazwy**i wyszukaj tych dzienników. W tym przykładzie jest `Type=ContainerLog adoring_meitner`.

**Wyświetlanie informacji o wydajności**

Gdy zaczynasz tworzyć zapytania, może pomóc zobacz Co to jest możliwe najpierw. Na przykład aby wyświetlić wszystkie dane dotyczące wydajności, spróbuj ogólne kwerendy, wpisując następujące zapytanie wyszukiwania.

```
Type=Perf
```

![wydajność kontenerów](./media/log-analytics-containers/containers-perf01.png)



Można to zobaczyć w postaci bardziej po kliknięciu wyraz **metryki** w wynikach.

![wydajność kontenerów](./media/log-analytics-containers/containers-perf02.png)



Można ograniczyć dane dotyczące wydajności, który jest wyświetlany do określonego kontenera, wpisując nazwę go po prawej stronie zapytania.

```
Type=Perf <containerName>
```

Przedstawiająca listę miar wydajności, które są zbierane dla poszczególnych kontenera.

![wydajność kontenerów](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Przykład kwerend wyszukiwania dziennika

Często jest przydatne do tworzenia zapytań, zaczynając od przykład lub dwa i następnie modyfikowanie ich tak, aby dopasować środowiska. Jako punktu startowego możesz poeksperymentować z karta **Godne uwagi kwerend** ułatwia tworzenie bardziej zaawansowanych zapytań.

![Kwerendy kontenerów](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Zapisywanie dzienników kwerend wyszukiwania

Zapisywanie kwerend jest funkcją standardową w dzienniku analizy. Zapisując je, że te, które znalezieniu przydatne przydatnego do użycia w przyszłości.

Po utworzeniu kwerendę, która okaże się przydatne, zapisz go, klikając pozycję **Ulubione** u góry strony wyszukiwania dziennika. Następnie możesz łatwo do niego dostęp później na stronie **Moje pulpit nawigacyjny** .

## <a name="next-steps"></a>Następne kroki

- [Dzienniki wyszukiwania](log-analytics-log-searches.md) , aby wyświetlić rekordy danych szczegółowych kontener.
