<properties
    pageTitle="Monitorowanie VMware rozwiązanie do analizy dziennika | Microsoft Azure"
    description="Dowiedz się, jak monitorowania VMware rozwiązanie może pomóc w ESXi hosts monitorowania i zarządzania nimi dzienników."
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
    ms.date="10/28/2016"
    ms.author="banders"/>

# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>Rozwiązanie do analizy dziennika VMware monitorowania (wersja Preview)

Monitorowanie VMware rozwiązanie do analizy dziennika jest rozwiązanie, które ułatwia tworzenie scentralizowane rejestrowania i monitorowania podejście do dużych dzienników VMware. W tym artykule opisano, jak można Rozwiązywanie problemów, przechwytywanie i zarządzać hosts ESXi w jednym miejscu za pomocą rozwiązanie. Z rozwiązanie można wyświetlić dane szczegółowe dla wszystkich hosty ESXi w jednym miejscu. Widać zlicza górny zdarzenia, status i trendów maszyn wirtualnych i ESXi hosts przez dzienniki hosta ESXi. Można rozwiązać, wyświetlając i wyszukiwanie scentralizowane ESXi hosta dzienników. I można utworzyć alerty oparte na dziennika kwerend wyszukiwania.

Rozwiązanie używa funkcji natywnych syslog hosta ESXi do przekazania danych do elementu docelowego Głosowa, która ma agenta usługi OMS. Jednak rozwiązanie nie zapisywać pliki w syslog w docelowym maszyn wirtualnych. Agent usługi OMS otwiera port 1514 i wykrywa to. Po odebraniu danych agenta usługi OMS umieszcza dane do usługi OMS.

## <a name="installing-and-configuring-the-solution"></a>Instalowanie i konfigurowanie rozwiązanie

Aby zainstalować i skonfigurować rozwiązanie, korzystając z następujących informacji.

- Dodaj rozwiązanie monitorowania VMware do obszaru roboczego usługi OMS przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>Obsługiwane hosts VMware ESXi
vSphere ESXi hosta 5.5 i 6.0

#### <a name="prepare-a-linux-server"></a>Przygotowywanie serwer Linux
Tworzenie systemu operacyjnego Linux maszyn wirtualnych do odbierania wszystkich danych syslog od hostów ESXi. [Linux Agent usługi OMS](log-analytics-linux-agents.md) jest punktem zbioru dla wszystkich ESXi hosta syslog danych. Wiele hostów ESXi umożliwia przekazywanie dzienników na jednym serwerze Linux, tak jak w poniższym przykładzie.  

   ![Przepływ SYSLOG](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Konfigurowanie zbierania syslog

1. Konfigurowanie przesyłania syslog dla VSphere. Aby uzyskać szczegółowe informacje ułatwiające konfigurowanie przesyłania dalej syslog, zobacz [Konfigurowanie syslog na ESXi 5.x oraz 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Przejdź do pozycji **Konfiguracja hosta ESXi** > **oprogramowania** > **Ustawienia zaawansowane** > **Syslog**.
  ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  

2. W polu *Syslog.global.logHost* Dodaj serwer Linux i numer portu *1514*. Na przykład `tcp://hostname:1514` lub`tcp://123.456.789.101:1514`

3. Otwórz zaporę hosta ESXi dla syslog. **Konfiguracja hosta ESXi** > **oprogramowania** > **Profil zabezpieczeń** > **zaporę** i Otwórz okno **Właściwości**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  

4. Sprawdzanie vSphere konsoli w celu zweryfikowania, że tego syslog jest prawidłowo skonfigurowana. Potwierdź na hoście ESXI portu **1514** jest skonfigurowany.

5. Testowanie łączności między serwer Linux i hosta ESXi przy użyciu `nc` polecenia na hoście ESXi. Na przykład:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

6. Pobierz i zainstaluj agenta usługi OMS Linux na serwerze Linux. Aby uzyskać więcej informacji zobacz w [dokumentacji usługi OMS agenta Linux](https://github.com/Microsoft/OMS-Agent-for-Linux).

7. Po zainstalowaniu agenta usługi OMS Linux, przejdź do katalogu /etc/opt/microsoft/omsagent/sysconf/omsagent.d, a następnie skopiuj plik vmware_esxi.conf do katalogu /etc/opt/microsoft/omsagent/conf/omsagent.d i zmiana właściciela lub grupy i uprawnienia do tego pliku. Na przykład:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```

8.  Ponowne uruchomienie agenta usługi OMS Linux, uruchamiając `sudo /opt/microsoft/omsagent/bin/service_control restart`.

9. W portalu usługi OMS Wyszukaj dziennika `Type=VMware_CL`. Usługi OMS zbiera dane syslog, zachowuje formatowanie syslog. W portalu niektórych określonych pól są przechwytywane, takie jak *Nazwa hosta* i *parametr*.  

    ![Typ](./media/log-analytics-vmware/type.png)  

    Wyświetlanie wyników wyszukiwania dziennika są podobne do obraz powyżej, jest równa za pomocą pulpitu nawigacyjnego rozwiązanie monitorowania VMware usługi OMS.  

## <a name="vmware-data-collection-details"></a>Szczegóły zbioru danych VMware

Rozwiązanie monitorowania VMware zbiera różnych wskaźników i dziennika dane dotyczące wydajności z hosts ESXi używania agentów usługi OMS Linux, które są włączone.

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące sposobu dane są zbierane.

| platformy | Agent usługi OMS Linux | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|
|Linux|![Tak](./media/log-analytics-vmware/oms-bullet-green.png)|![Brak](./media/log-analytics-vmware/oms-bullet-red.png)|![Brak](./media/log-analytics-vmware/oms-bullet-red.png)|            ![Brak](./media/log-analytics-containers/oms-bullet-red.png)|![Brak](./media/log-analytics-vmware/oms-bullet-red.png)| co 3 minut|


Poniższa tabela Pokaż Przykłady pól danych zebranych przez monitorowanie VMware rozwiązanie:

| Nazwa pola | Opis |
| --- | --- |
| Device_s| Urządzenia magazynowania VMware |
| ESXIFailure_s | typy błąd |
| EventTime_t | Godzina wystąpienia zdarzenia |
| HostName_s | Nazwa hosta ESXi |
| Operation_s | Tworzenie maszyn wirtualnych lub usuwanie maszyn wirtualnych |
| ProcessName_s | Nazwa zdarzenia |
| ResourceId_s | Nazwa hosta VMware |
| ResourceLocation_s | VMware |
| ResourceName_s | VMware |
| ResourceType_s | Funkcji Hyper-V |
| SCSIStatus_s | Stan VMware SCSI |
| SyslogMessage_s | SYSLOG danych |
| UserName_s | użytkownika, który utworzył lub usunięte maszyn wirtualnych |
| VMName_s | Nazwa maszyn wirtualnych |
| Komputer | komputer |
| TimeGenerated | Godzina wygenerowania danych |
| DataCenter_s | VMware centrum danych |
| StorageLatency_s | Opóźnienie miejsca do magazynowania (ms) |

## <a name="vmware-monitoring-solution-overview"></a>Przegląd rozwiązania VMware monitorowania

Fragment VMware pojawi się w portalu usługi OMS. Umożliwia szczegółowego widoku wszystkie błędy. Po kliknięciu kafelka, przejdź do widoku pulpitu nawigacyjnego.

![kafelków](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Nawigowanie w widoku pulpitu nawigacyjnego

W widoku pulpitu nawigacyjnego **VMware** karty są zorganizowane według:

- Liczba stanu błędu
- Zlicza górny hosta przez zdarzenie
- Zlicza górny zdarzenia
- Działania maszyn wirtualnych
- Zdarzenia dysku ESXi hosta


![solution1](./media/log-analytics-vmware/solutionview1-1.png)

![solution2](./media/log-analytics-vmware/solutionview1-2.png)

Kliknij dowolny karta, aby otworzyć okienko Wyszukiwanie analizy dziennika, który zawiera szczegółowe informacje specyficzne dla karta.

W tym miejscu możesz edytować kwerendy wyszukiwania, aby zmodyfikować określonego elementu. Samouczek dotyczący podstawy wyszukiwania usługi OMS, zapoznaj się z [Samouczek wyszukiwania dziennika usługi OMS.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>Znajdowanie ESXi zdarzenia hosta

Jeden host ESXi generuje wiele dzienników, opartych na ich procesów. Rozwiązanie monitorowania VMware centralizuje je i zawiera podsumowanie liczby zdarzeń. W tym widoku scentralizowane ułatwia opis, który host ESXi ma dużej liczby wydarzeń i jakie zdarzenia najczęściej występują w środowisku.

![zdarzenia](./media/log-analytics-vmware/events.png)

Można przechodzić do dalszych, klikając pozycję hosta ESXi lub typu zdarzenia.

Po kliknięciu nazwy hosta ESXi można wyświetlić informacje z tego hosta ESXi. Jeśli chcesz zawęzić wyniki z typem zdarzenia, Dodaj `“ProcessName_s=EVENT TYPE”` w kwerendzie wyszukiwania. Możesz wybrać **parametr** w filtrze wyszukiwania. Który zawęża danych.

![Przechodzenie do](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Znajdowanie wysoka działania maszyn wirtualnych

Można tworzyć i usunięte dowolnego hosta ESXi maszyny wirtualnej. Dobrze jest na identyfikowanie ile maszyny wirtualne tworzy hosta ESXi przez administratora. Tego w przewracania, ułatwia zrozumienie Planowanie wydajności i pojemności. Rejestrowanie informacji o zdarzeń aktywności maszyn wirtualnych jest ważnych podczas zarządzania środowiska.

![Przechodzenie do](./media/log-analytics-vmware/vmactivities1.png)

Jeśli chcesz wyświetlić dodatkowe ESXi hosta maszyn wirtualnych tworzenia danych, kliknij nazwę hosta ESXi.

![Przechodzenie do](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Typowe kwerend wyszukiwania

Rozwiązanie zawiera inne przydatne zapytania, które mogą ułatwić zarządzanie hosty ESXi, takie jak wysoka ilości miejsca do magazynowania, opóźnienie miejsca do magazynowania i ścieżkę błąd.

![kwerendy](./media/log-analytics-vmware/queries.png)

#### <a name="save-queries"></a>Zapisywanie kwerendy

Zapisywanie kwerend wyszukiwania jest funkcją standardową w usługi OMS i mogą ułatwić organizowanie niektóre kwerendy znalezieniu przydatne. Po utworzeniu kwerendę, która okaże się przydatne, zapisz go, klikając pozycję **Ulubione**. Zapisane zapytanie pozwala na łatwe późniejszego go na stronie [Moje pulpit nawigacyjny](log-analytics-dashboards.md) miejsce, w którym można tworzyć własne niestandardowe pulpity nawigacyjne.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Tworzenie alertów z zapytań

Po utworzeniu zapytań można użyć kwerendy wyświetlane w przypadku wystąpienia określonych zdarzeń. Aby uzyskać informacje o tworzeniu alertów, zobacz [alerty w dzienniku analizy](log-analytics-alerts.md) . Aby zapoznać się z przykładami alarmowe kwerendy i inne przykłady kwerendy zawiera wpis w blogu [Monitor VMware za pomocą usługi OMS dziennika analizy](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) .

## <a name="frequently-asked-questions"></a>Często zadawane pytania

### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Co muszę na ESXi hosta ustawienie? Jaki wpływ będzie miał na Moje bieżące środowisko?
Rozwiązanie używa natywnych Syslog hosta ESXi przekazywanie mechanizmu. Nie musisz dodatkowego oprogramowania firmy Microsoft na hoście ESXi do przechwytywania dzienniki. Powinien mieć niskim wpływ na istniejące środowiska. Potrzebny skonfigurować przekazywanie syslog, czyli ESXI funkcji.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Należy ponownie uruchomić host mojej ESXi?
Wartość nie. Ten proces nie wymaga ponownego uruchomienia. Czasami prawidłowo vSphere nie aktualizuje syslog. W takim przypadku logowania się do hosta ESXi i ponownie załaduj syslog. Ponownie nie musisz ponownie uruchomić hosta, aby ten proces nie ma przerw w pracy środowiska.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Czy zwiększanie lub zmniejszanie głośności danych dziennika wysyłane do usługi OMS?
Tak, możesz to zrobić. Możesz korzystać z ustawień poziomu dziennika hosta ESXi w vSphere. Zbieranie dzienników jest oparty na poziomie *informacje* . Tak Jeśli chcesz poddać inspekcji maszyn wirtualnych tworzenia lub usuwania, należy zachować poziom *informacje* na Hostd. Aby uzyskać więcej informacji zobacz [VMware bazy wiedzy Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Dlaczego Hostd nie dostarcza dane do usługi OMS? Moje dziennika jest ustawienie informacje.
Wystąpił błąd hosta ESXi dla syslog sygnatura czasowa. Aby uzyskać więcej informacji zobacz [VMware bazy wiedzy Knowledge Base](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Po wprowadzeniu rozwiązania Hostd powinna działać normalnie.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Czy można mieć wiele hostów ESXi przekazywanie syslog danych w jednym maszyn wirtualnych z omsagent?
Wartość Tak. Można mieć wiele hostów ESXi przekazywanie w jednym maszyn wirtualnych z omsagent.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Dlaczego nie widzę danych wejściowa do usługi OMS?

Może istnieć kilka przyczyn:

- ESXi host nie jest poprawnie przekazywanie danych do maszyn wirtualnych, uruchomiony omsagent. Aby przetestować, należy wykonać następujące czynności:
    1. Aby potwierdzić, zaloguj się z hostem ESXi przy użyciu ssh i uruchom następujące polecenie:`nc -z ipaddressofVM 1514`

        Jeśli to nie powiedzie się, prawdopodobnie ustawienia vSphere w Konfiguracja zaawansowana nie poprawiane. Zobacz [Konfigurowanie syslog zbioru](#configure-syslog-collection) informacje na temat konfigurowania hosta ESXi syslog przekazywanie.

    2. Jeśli syslog port łączności się pomyślnie, ale nadal nie widać jakiekolwiek dane, Załaduj ponownie syslog na hoście ESXi, za pomocą ssh uruchom następujące polecenie:` esxcli system syslog reload`

- Maszyn wirtualnych z agentem usługi OMS nie jest ustawiona prawidłowo. Aby to sprawdzić, wykonaj następujące czynności:
    1. Usługi OMS wykrywa do portu 1514 i umieszcza dane do usługi OMS. Aby sprawdzić, czy jest otwarty, uruchom następujące polecenie:`netstat -a | grep 1514`
    2. Powinien zostać wyświetlony port `1514/tcp` Otwórz. W przeciwnym razie, upewnij się, że omsagent został zainstalowany poprawnie. Jeśli nie widzisz informacji portu, a następnie syslog port nie jest otwarty na maszyn wirtualnych.
        1. Sprawdź, czy Agent usługi OMS jest uruchomiony przy użyciu `ps -ef | grep oms`. Jeśli nie jest uruchomiony, należy uruchomić proces, uruchamiając polecenie` sudo /opt/microsoft/omsagent/bin/service_control start`
        2. Otwórz `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` pliku.

            Sprawdź, czy użytkownik pisane z wielkiej litery i ustawienie dla grupy jest prawidłowy, podobnie jak:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

            Jeśli plik nie istnieje lub użytkownik i ustawienie dla grupy jest nieprawidłowy, podejmowania działań naprawczych przez [Przygotowywanie serwer Linux](#prepare-a-linux-server).

## <a name="next-steps"></a>Następne kroki

- Wyświetlanie szczegółowych danych hosta VMware przy użyciu [Wyszukiwania dziennika](log-analytics-log-searches.md) do analizy dziennika.
- [Tworzenie własnych pulpitów nawigacyjnych](log-analytics-dashboards.md) przedstawiający VMware przechowywania danych.
- [Tworzenie alertów](log-analytics-alerts.md) po wystąpieniu zdarzenia hosta VMware określone.
