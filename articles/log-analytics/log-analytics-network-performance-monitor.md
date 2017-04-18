<properties
    pageTitle="Sieci rozwiązania monitorowanie wydajności w programie usługi OMS | Microsoft Azure"
    description="Monitorowanie wydajności usługi sieci w pobliżu real-jednorazowej do pomaga wykrywać Monitor wydajności sieci i Znajdź gardła wydajności sieci."
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
    ms.date="07/28/2016"
    ms.author="banders"/>

# <a name="network-performance-monitor-preview-solution-in-oms"></a>Sieci rozwiązania monitora wydajności (wersja Preview) w programie usługi OMS

>[AZURE.NOTE] Jest to [rozwiązanie Podgląd](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

W tym dokumencie opisano, jak do konfiguracji i przy użyciu rozwiązania Monitor wydajności sieci w programie usługi OMS, która ułatwia monitorowanie wydajności usługi sieci w pobliżu real-jednorazowej do wykrywanie i Znajdź sieciowych gardła wydajności. Z rozwiązanie Monitor wydajności sieci można monitorować utratę i opóźnienie między dwiema sieciami, podsieci lub serwerów. Monitorowanie wydajności sieci wykrywa problemów sieciowych, takich jak blackholing ruch routingu błędów i problemów metody monitorowania konwencjonalny sieci nie może wykryć. Monitorowanie wydajności sieci generuje alerty i powiadamia, gdy progu naruszenia łącza do sieci. Progi te mogą materiału automatycznie przez system lub można skonfigurować ich używać niestandardowych reguł alertów. Monitorowanie wydajności sieci zapewnia szybkie wykrycie problemów z wydajnością sieci i lokalizuje źródłem problemu do części określonej sieci lub urządzenia.

Można wykryć sieć problemów z pulpitu nawigacyjnego rozwiązanie, które są wyświetlane sumaryczne informacje o sieci w tym ostatnie zdarzenia kondycji sieci, łącza nieprawidłowe sieci i podsieć łącza, które są przeciwległych utraty pakietów wysoki i opóźnienie. Możesz można przechodzić do szczegółów łącze sieci, aby wyświetlić bieżący stan kondycji podsieć łącza, jak również łącza węzła do węzła. Możesz także wyświetlić historycznych trendu utraty oraz opóźnienie w sieci, podsieć i poziomu węzła do węzła. Można wykryć problemy z sieci, wyświetlając wykresy historycznych trendu utraty pakietów i opóźnienie i Znajdź gardła sieci na mapie topologii. Na wykresie interakcyjnych topologii umożliwia wizualizowanie przekierowuje przeskoku skok sieci i określić źródło problemu. Jak innych rozwiązań dziennika wyszukiwanie różne wymagania analizy służy do tworzenia niestandardowych raportów na podstawie danych zebranych przez Monitor wydajności sieci.

Rozwiązanie używa transakcji syntetycznych w charakterze mechanizmu podstawowego wykrywanie błędów sieci. Tak można go używać bez względu na urządzeniu sieciowym określonego dostawcy lub modelu. Działania w lokalnym, chmury (IaaS) i środowiska hybrydowe. Rozwiązanie automatycznie wykrywa topologia sieci i różnych sposobów w Twojej sieci.

Produkty monitorowania typowej sieci skoncentrować się na monitorowanie kondycji sieci urządzenia (routery, przełączniki itp.), ale nie zawierają spostrzeżeń rzeczywistej jakości łączność sieciowa między dwoma punktami, które monitorze wydajności sieci.

### <a name="using-the-solution-standalone"></a>Za pomocą samodzielnego rozwiązanie

Jeśli chcesz monitorować jakość połączenia między ich krytyczne obciążenia sieci, centrach danych lub witryny pakietu office, a następnie możesz umożliwia rozwiązanie Monitor wydajności sieci przez siebie monitorowanie kondycji łączność między:

- wiele witryn centrach danych lub pakietu office, połączonych za pomocą sieci publicznych lub prywatnych
- krytyczne obciążenia, które są uruchomione aplikacje biznesowe
- chmury publicznych usług, takich jak Microsoft Azure lub usługi sieci Web Amazon (AWS) i sieci lokalnej, jeśli masz IaaS (maszyn wirtualnych) dostępne i masz skonfigurowany do zezwalania na komunikacji bram między sieci lokalnej i sieciami chmury
- Azure i lokalnych sieci, korzystając z rozsyłania Express

### <a name="using-the-solution-with-other-networking-tools"></a>Rozwiązanie za pomocą innych narzędzi do sieci

Jeśli chcesz monitorować linią aplikacji biznesowych, umożliwiają rozwiązanie Monitor wydajności sieci jako rozwiązanie companion inne narzędzia sieci. Wolno działającej sieci może prowadzić do aplikacji działa wolno i monitorowanie wydajności sieci ułatwiają badanie problemów z wydajnością aplikacji powodowanych przez źródłowych problemów sieciowych. Ponieważ rozwiązanie nie wymaga dostępu do urządzenia sieciowe, administrator aplikacji nie trzeba oparte na sieci zespołu o podanie informacji na temat sposobu sieci ma wpływ na aplikacje.

Ponadto jeśli już inwestować w innych sieciach narzędzi monitorowania, następnie rozwiązanie może uzupełnić te narzędzia ponieważ najbardziej tradycyjny rozwiązań monitorowania sieci nie oferuje spostrzeżeń sieci zakończenia do końca wskaźniki, takich jak utratę i opóźnienie.  Rozwiązanie Monitor wydajności sieci może ułatwić wypełnianie tego odstępu.


## <a name="installing-and-configuring-agents-for-the-solution"></a>Instalowanie i konfigurowanie wykluczonych agentów rozwiązania

Podstawowy proces należy zainstalować agentów na [komputerach z programem Windows łączenie do analizy dziennika](log-analytics-windows-agents.md) i [Łączenie analizy dziennika programu Operations Manager](log-analytics-om-agents.md).

>[AZURE.NOTE]
Musisz zainstalować agentów co najmniej 2 w celu za mało danych, aby odnaleźć i monitorować do zasobów sieci. W przeciwnym razie rozwiązanie pozostanie w Państwie konfigurowania, aż zainstalować i skonfigurować dodatkowe agentów.

### <a name="where-to-install-the-agents"></a>Gdzie zainstalować agentów

Przed rozpoczęciem instalacji czynników, warto rozważyć topologii sieci i które części sieci, który chcesz monitorować. Firma Microsoft zaleca się zainstalowanie więcej niż jednego agenta dla każdej podsieci, które chcesz monitorować. Innymi słowy dla każdej podsieci, której chcesz monitorować, wybierz co najmniej dwa serwery lub maszyny wirtualne i zainstalować agenta nad nimi.

Jeśli masz pewności topologii sieci, należy zainstalować agentów na serwerach z krytycznej obciążenia miejsce, w którym chcesz monitorować wydajność sieci. Na przykład może być umożliwia śledzenie połączenie sieciowe między serwerem sieci Web i na serwerze z programem SQL Server. W tym przykładzie możesz zainstalować agenta na obu serwerach.

Czynniki monitorować łączność sieciowa (łączy) między hosts — nie samych hostów. Tak aby monitorować łącza sieci, należy zainstalować agentów na oba punkty końcowe tego łącza.

### <a name="configure-agents"></a>Konfigurowanie wykluczonych agentów

Po zainstalowaniu czynnikami, musisz otworzyć portów zapory dla komputerów upewnić się, że agenci mogą się komunikować. Należy pobrać, a następnie uruchom [skrypt programu PowerShell EnableRules.ps1](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) bez parametrów w oknie programu PowerShell z uprawnieniami administratora

Skrypt tworzy klucze rejestru wymagane przez Monitor wydajności sieci i tworzy reguły zapory systemu Windows zezwalające agentów do tworzenia połączeń ze sobą. Również kluczach rejestru utworzone przez skrypt Określ, czy do logowania dzienniki i ścieżkę do pliku dzienników. Definiuje port TCP agenta na potrzeby komunikacji. Wartości tych kluczy są automatycznie ustawiane przez skrypt, więc nie należy ręcznie zmienić klucze.

Port domyślnie otwierany jest 8084. Za pomocą niestandardowych portu, dostarczając parametr `portNumber` do skryptu. Jednak ten sam port powinien być używany na wszystkich komputerach miejsce, w którym skrypt jest uruchamiany.

>[AZURE.NOTE] Skrypt EnableRules.ps1 konfiguruje reguły zapory systemu Windows tylko na komputerze, na którym skrypt jest uruchamiany. Mając sieciowa zapora, należy upewnić się, że umożliwia ruchu przeznaczonego dla port TCP używany przez Monitor wydajności sieci.


## <a name="configuring-the-solution"></a>Konfigurowanie rozwiązanie

Aby zainstalować i skonfigurować rozwiązanie, korzystając z następujących informacji.

1. Rozwiązanie Monitor wydajności sieci uzyskuje dane z komputerów z systemem Windows Server 2008 z dodatkiem SP 1 lub nowszym lub Windows 7 z dodatkiem SP1 lub nowszym, które są takie same wymagania jako Microsoft monitorowania agenta (MMA).
2. Dodaj rozwiązanie Monitor wydajności sieci do usługi OMS obszaru roboczego przy użyciu procesu opisanego w [rozwiązań Dodawanie analizy dziennika z galerii rozwiązań](log-analytics-add-solutions.md).  
  ![Symbol monitora wydajności sieci](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. W portalu usługi OMS pojawi się fragment zatytułowany **Monitor wydajności sieci** z tą wiadomością *rozwiązanie wymaga dodatkowej konfiguracji*. Musisz skonfigurować rozwiązanie, aby dodać sieci na podstawie podsieci i węzły, które zostały wykryte przez agentów. Kliknij pozycję **Monitor wydajności sieci** , aby rozpocząć konfigurowanie sieci domyślne.  
  ![rozwiązanie wymaga dodatkowej konfiguracji](./media/log-analytics-network-performance-monitor/npm-config.png)


### <a name="configure-the-solution-with-a-default-network"></a>Konfigurowanie rozwiązania przy użyciu domyślnej sieci

Na stronie Konfiguracja pojawi się jednej sieci o nazwie **domyślne**. Gdy nie zostało zdefiniowane sieci, wszystkie automatycznie odnalezione podsieci są umieszczane w domyślnej sieci.

Gdy utworzysz sieci podsieć możesz dodać do niej, a podsieci zostanie usunięte z jako domyślnej sieci. Jeśli usuniesz sieci, wszystkie jej podsieci automatycznie są zwracane do domyślnej sieci.

Innymi słowy jako domyślnej sieci jest kontenerem dla wszystkich podsieci, które nie są zawarte w dowolna sieć, zdefiniowane przez użytkownika. Nie można edytować lub usunąć jako domyślnej sieci. Zawsze pozostaje w systemie. Jednak można utworzyć możliwie jak najwięcej sieci, jak to potrzebne.

W większości przypadków podsieci w organizacji będą rozmieszczone w więcej niż jednej sieci, a następnie należy utworzyć jednej lub kilku sieci do logicznego grupowania usługi podsieci.

### <a name="create-new-networks"></a>Tworzenie nowych sieci

Sieć w Monitorze wydajności sieci to kontener podsieci. Można utworzyć sieć z dowolną nazwę i Dodaj podsieci z siecią. Na przykład możesz utworzyć sieć o nazwie *Building1* , a następnie dodać podsieci lub można utworzyć sieć o nazwie *strefy Zdemilitaryzowanej* , a następnie dodaj wszystkich podsieci należące do strefy zdemilitaryzowaną z tą siecią.

#### <a name="to-create-a-new-network"></a>Aby utworzyć nową sieć

1. Kliknij pozycję **Dodaj sieć** , a następnie wpisz nazwę sieci i opis.
2.  Zaznacz jeden lub więcej podsieci, a następnie kliknij przycisk **Dodaj**.
3. Kliknij przycisk **Zapisz** , aby zapisać konfigurację.  
  ![Dodawanie sieci](./media/log-analytics-network-performance-monitor/npm-add-network.png)



### <a name="wait-for-data-aggregation"></a>Poczekaj, aż agregacji danych

Po zapisaniu konfiguracji po raz pierwszy, zostanie uruchomiony rozwiązanie zbieranie pakietów utratę i opóźnienie informacji o sieci węzłów zainstalowanego agentów. Ten proces może zająć trochę czasu, czasami 30 minut. W tym stanie kafelków Monitor wydajności sieci na stronie Omówienie wyświetla komunikat *agregacji danych w procesie*.

![agregacja danych w toku](./media/log-analytics-network-performance-monitor/npm-aggregation.png)


Po przekazaniu danych, pojawi się Monitor wydajności sieci kafelków aktualizacja z danymi.

![Kafelek Monitor wydajności sieci](./media/log-analytics-network-performance-monitor/npm-tile.png)

Kliknij Kafelek, aby wyświetlić pulpit nawigacyjny Monitor wydajności sieci.

![Pulpit nawigacyjny monitorowanie wydajności sieci](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Edytowanie ustawień monitorowania dla podsieci

Wszystkie podsieci, w którym zainstalowano co najmniej jednego agenta znajdują się na karcie **podsieci** na stronie Konfiguracja.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>Aby włączyć lub wyłączyć monitorowania dla określonego podsieci

1. Zaznacz lub wyczyść pola obok **identyfikator podsieć** , a następnie sprawdź, czy **ma zastosowanie w przypadku monitorowania** jest wybrane lub wyczyszczone, zależnie od potrzeb. Można zaznaczyć lub wyczyścić wiele podsieci. Wyłączenie, podsieci nie są monitorowane, jak agentów zostaną zaktualizowane, aby zatrzymać polecenie ping innych czynników.
2. Wybierz węzły, które chcesz monitorować dla określonego podsieć, zaznaczając podsieci z listy i nawigowanie wymagane węzły list zawierających węzły niemonitorowanych i monitorowane.
W razie potrzeby możesz dodać niestandardowy **Opis** do podsieć.
3. Kliknij przycisk **Zapisz** , aby zapisać konfigurację.  
  ![Edytowanie podsieci](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Wybierz węzły do monitorowania

Wszystkie węzły, które agent jest zainstalowany na nich znajdują się na karcie **węzły** .

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Aby włączyć lub wyłączyć monitorowania węzły

1. Zaznacz lub wyczyść węzły, które chcesz monitorować lub zatrzymywania monitorowania.
2. Kliknij pozycję **Użyj dla monitorowania**, lub wyczyść to pole, zależnie od potrzeb.
3. Kliknij przycisk **Zapisz**.  
  ![Włącz monitorowanie węzeł](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)


### <a name="set-monitoring-rules"></a>Konfigurowanie reguły monitorowania

Monitorowanie wydajności sieci generuje zdarzenia kondycji o łączność między dwoma węzły lub podsieć lub sieci łączy po naruszenia progu. Progi te mogą materiału automatycznie przez system lub można je skonfigurować reguły alertu niestandardowe.

*Domyślna reguła* jest tworzona przez system i tworzy zdarzenie kondycji przy każdym utraty lub opóźnienia między każdą parą sieci lub podsieć łączy naruszenia progu materiału systemu. Użytkownik może wyłączyć regułę domyślne i tworzenie niestandardowej reguły monitorowania

#### <a name="to-create-custom-monitoring-rules"></a>Tworzenie niestandardowej reguły monitorowania

1. Na karcie **Monitor** kliknij przycisk **Dodaj regułę** i wprowadź nazwę reguły i opis.
2. Wybierz pozycję parą łączy sieci lub podsieć monitorowanie z list.
3. Najpierw wybierz sieć, w której znajduje się pierwszy podsieć/s zainteresowania z menu rozwijanego sieci, a następnie wybierz podsieć/s z menu rozwijanego odpowiednich podsieć.
Zaznacz **wszystkie podsieci** , jeśli chcesz monitorować wszystkich podsieci w łączu sieci. Podobnie wybierz inne podsieć/s zainteresowania. I kliknięcie **Przycisku Dodaj wyjątek** wykluczyć monitorowania dla określonego podsieć łączy z zaznaczenia, która została wprowadzona.
4. Jeśli nie chcesz utworzyć kondycji zdarzeń dla elementów, które jest zaznaczona, wyczyść **Włączanie monitorowania łącza objęte przez tę regułę kondycji**.
5. Wybierz pozycję monitorowania warunków.
Progi niestandardowych do generowania zdarzeń kondycji można ustawić, wpisując wartości progowe. Zawsze, gdy wartość warunek odbywa się powyżej progu zaznaczonego pary wybranej sieci/podsieci, jest generowany zdarzenie kondycji.
6. Kliknij przycisk **Zapisz** , aby zapisać konfigurację.  
  ![Tworzenie reguły niestandardowej monitorowania](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)


## <a name="data-collection-details"></a>Szczegóły zbioru danych

Monitor wydajności sieci używa pakietów uzgadniania TCP SYN-SYNACK-ACK do zbierania utratę i opóźnienie informacji i traceroute służy także aby uzyskać informacje o topologii.

W poniższej tabeli przedstawiono metody zbioru danych i inne szczegóły dotyczące sposobu dane są zbierane dla monitora wydajności sieci.

| platformy | Bezpośrednie agenta | SCOM agent | Azure miejsca do magazynowania | SCOM wymagane? | Dane agenta SCOM wysyłane za pośrednictwem grupy zarządzania | częstotliwość pobierania |
|---|---|---|---|---|---|---|
| Systemu Windows |![Tak](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Tak](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Brak](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|            ![Brak](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|![Brak](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)| Port TCP uzgodnień 5 sekund, dane wysyłane co 3 minut |

Rozwiązanie korzysta z transakcji syntetycznych do oceny kondycji sieci. Czynniki usługi OMS zainstalowany w momencie różnych w pakiety TCP exchange sieci ze sobą, a w procesie informacje wysyłającego utraty czasu i pakietów ewentualne. Okresowo każdego agenta również wykonuje śledzenie trasy do innych czynników, aby znaleźć wszystkie różnych sposobów w sieci, które muszą zostać przetestowane. Za pomocą tych danych, czynników mogą ustalenie czasu oczekiwania w sieci i strat pakietów. Badanie jest powtarzane co pięć sekund i danych jest agregowane w okresie trzech minut czynnikami przed przesłaniem go do usługi OMS.

>[AZURE.NOTE] Chociaż agentów komunikowania się między sobą często, ich generuje wiele ruch sieciowy podczas przeprowadzania badań. Czynniki tylko zależeć pakietów uzgadniania TCP SYN-SYNACK-ACK do określenia utratę i opóźnienie — żadnych danych, które pakietów. W trakcie tego procesu agentów komunikowania się między sobą tylko w razie potrzeby i topologii komunikacji agenta jest zoptymalizowany w celu zmniejszenia ruchu sieciowego.

## <a name="using-the-solution"></a>Za pomocą rozwiązanie

W tej sekcji omówiono wszystkie pulpitu nawigacyjnego funkcje i jak z nich korzystać.

### <a name="solution-overview-tile"></a>Rozwiązanie omówienie kafelków

Po włączeniu rozwiązanie Monitor wydajności sieci, kafelków rozwiązanie na stronie Omówienie usługi OMS zawiera krótkie omówienie kondycji sieci. Zostanie wyświetlona wykresu pierścieniowy pokazujący liczbę łączy podsieć prawidłowy i nieprawidłowe. Po kliknięciu kafelka zostanie wyświetlona na pulpicie nawigacyjnym rozwiązanie.

![Kafelek Monitor wydajności sieci](./media/log-analytics-network-performance-monitor/npm-tile.png)


### <a name="network-performance-monitor-solution-dashboard"></a>Pulpit nawigacyjny rozwiązanie monitorowanie wydajności sieci

Karta **Podsumowanie sieci** zawiera podsumowanie sieci wraz z ich względną wielkość. To następuje kafelków pokazujący całkowitą liczbę łączy sieci, łącza podsieci i ścieżki w systemie (ścieżka składa się z dwóch hostów czynnikami i wszystkie przeskoków między ich adresy IP).

Karta **Zdarzeń kondycji sieci góry** zawiera listę ostatnich zdarzeń zdrowia i alerty w systemie i czas, ponieważ zdarzenie była aktywna. Kondycja zdarzenie lub alertu jest generowany przy każdym utraty pakietów lub opóźnienie łącza sieci lub podsieć przekracza progu.

Karta **Góry nieprawidłowe łącza sieci** zawiera listę łączy sieciowych nieprawidłowe. Są to łącza sieci, zawierających jeden lub więcej zdarzeń niepożądanych kondycji dla nich w momencie.

Karty **Góry podsieć łącza ze stratą najbardziej** oraz **Łącza podsieć z najbardziej opóźnienie** Pokaż odpowiednio łączy górny podsieć według utraty pakietów i łącza górny podsieć za opóźnienie. Długim czasem oczekiwania lub niektóre ilość utraty pakietów można się spodziewać na niektórych łączy sieci. Takie łącza są wyświetlane na liście dziesięć górnego, ale nie są oznaczane nieprawidłowe.

Karta **Typowych kwerend** zawiera zestaw kwerend wyszukiwania, które uzyskiwanie zdalnego dostępu do sieci nieprzetworzonych bezpośrednio monitorowanie danych. Za pomocą te kwerendy jako punktu wyjścia do tworzenia zapytań dla raportów niestandardowych.

![Pulpit nawigacyjny monitorowanie wydajności sieci](./media/log-analytics-network-performance-monitor/npm-dash01.png)


### <a name="drill-down-for-depth"></a>Przechodzenie do szczegółów długości

Kliknięcie różnych łączy na rozwiązanie pulpitu nawigacyjnego do rozwijania szczegółowego do dowolnego obszar zainteresowania. Na przykład gdy zostanie wyświetlony alert lub łącze sieci nieprawidłowe są wyświetlane na pulpicie nawigacyjnym, klikając go, aby zbadać. Zostaniesz przekierowany do strony, która zawiera listę wszystkich łączy podsieć łącza określonej sieci. Można wyświetlić utratę, opóźnienie oraz stan każdego łącza podsieć i szybko Dowiedz się, jakie łącza podsieć powodują problem. Kliknięcie **łącza węzeł widoku** , aby zobaczyć wszystkie łącza węzeł łącza podsieci nieprawidłowe. Następnie można wyświetlać poszczególne łącza węzła do węzła i łącza węzeł nieprawidłowe.

Kliknięcie **topologii widoku** do wyświetlania topologii przeskoku skok trasy węzłów źródłowej i docelowej. Nieprawidłowe trasy lub przeskoków są wyświetlane w kolorze czerwonym, dzięki czemu można szybko ustalić problemu z określoną część sieci.

![Przechodzenie do szczegółów danych](./media/log-analytics-network-performance-monitor/npm-drill.png)


#### <a name="trend-charts"></a>Wykresy trendu

W każdym poziom, który rozwijania, może wyświetlać trendu utraty oraz opóźnienie łącza do sieci. Wykresy trendu są również dostępne dla podsieci i węzła łącza. Możesz zmienić przedział czasu dla wykresu kreślenia przy użyciu formantu czasu w górnej części wykresu.

Wykresy trendu Pokaż perspektywy historycznych wydajność łącza sieci. Niektóre problemy z siecią są przejściowych charakter i byłyby trudne do efektywnej tylko sprawdzając bieżący stan sieci. Jest to spowodowane problemów można szybko powierzchni i znikają przed powiadomienia każdy tylko do się ponownie w dowolnym momencie w czasie. Kwestie te przejściowych również może być trudne dla administratorów aplikacji, ponieważ te problemy często powierzchni wraz ze wzrostem Niewyjaśnione czas reakcji aplikacji, nawet wtedy, gdy wszystkie składniki aplikacji są wyświetlane sprawniej przeprowadzić.

Analizując wykresu trendu, gdy problem pojawi się jako szybkiego kolekcji w opóźnień sieci lub utraty pakietów, można łatwo wykrywać tych rodzajów problemów.

![Wykres trendu](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Mapa topologii przeskoku przez przeskoków

Monitorowanie wydajności sieci zawiera topologii przeskoku skok trasy między dwóch węzłów na mapie interakcyjnych topologii. Możesz wyświetlić mapę topologii, wybierając węzeł łącze, a następnie klikając polecenie **Widok topologii**. Ponadto można wyświetlić mapę topologii, klikając Kafelek **ścieżki** na pulpicie nawigacyjnym. Po kliknięciu **ścieżek** na pulpicie nawigacyjnym, konieczne będzie zaznacz węzły źródłowej i docelowej przy użyciu panelu po lewej, a następnie kliknij pozycję **Kreśl** kreślenia trasy między dwa węzły.

Mapa topologii jest wyświetlana, ile trasy są między dwa węzły i co ścieżek wykonać pakiety danych. Gardła wydajności sieci są oznaczone w kolorze czerwonym na mapie topologii. Możesz odnaleźć połączenie sieciowe uszkodzone lub uszkodzenia urządzenia sieciowego, sprawdzając czerwony kolorowe elementy na mapie topologii.

Po kliknięciu węzła lub umieść wskaźnik myszy nad nim na mapie topologii, zobaczysz właściwości węzła, takich jak adres IP i nazwy FQDN. Kliknij przycisk przeskoku, aby sprawdzić, czy jest adres IP. Można wyróżnić określony przekierowuje wyczyszczenie i wybierając tylko trasy, które mają być wyróżniane na mapie. Przy użyciu kółka myszy można powiększanie i pomniejszanie mapy topologii.

Należy zauważyć, że topologii wyświetlane w planie topologii warstwy 3 i nie zawiera urządzenia warstwy 2 i połączenia.

![mapy topologii przeskoku przez przeskoków](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Lokalizacja błędów

Monitorowanie wydajności sieci to może znaleźć gardła sieci bez łączenia urządzeń sieciowych. Na podstawie danych zebranych z sieci i przez zastosowanie zaawansowane algorytmy na wykresie sieci, Monitor wydajności sieci sprawia, że prawdopodobieństwa oszacowanie części sieci, które są najczęściej źródła problemu.

Ta metoda jest przydatne do określenia gardła sieci, gdy dostęp do przeskoków nie jest dostępna, ponieważ nie wymaga jakiekolwiek dane zbierane z urządzeń sieciowych np przełączniki. Jest to również przydatne podczas przeskoków między dwa węzły nie znajdują się w swojej kontroli administracyjnej. Na przykład przeskoków może być routery usługodawcy internetowego.

### <a name="log-analytics-search"></a>Przeszukiwanie analizy dziennika

Wszystkie dane, które jest strony graficznie dostępne za pośrednictwem pulpitu nawigacyjnego Monitor wydajności sieci i przechodzenie do szczegółów jest także dostępny oryginalnie w wyszukiwaniu analizy dziennika. Możesz kwerendy danych przy użyciu języka kwerend wyszukiwania i tworzyć raporty niestandardowe za pomocą eksportowania danych do programu Excel lub PowerBI. Karta **Typowych kwerend** na pulpicie nawigacyjnym zawiera niektóre przydatne kwerendy, które są dostępne jako punktu wyjścia do tworzenia własnych kwerend i raportów.

![kwerendy wyszukiwania](./media/log-analytics-network-performance-monitor/npm-queries.png)


## <a name="investigate-the-root-cause-of-a-health-alert"></a>Zbadać głównego alertu kondycji

Teraz, gdy przeczytane o Monitorze wydajności sieci, Przyjrzyjmy się proste śledztwa przyczyny dla zdarzenia kondycji.

1. Na stronie Przegląd otrzymasz migawki podręczna kondycji sieci obserwując **Monitor wydajności sieci** kafelka. Zwróć uwagę, że poza łączy 80 podsieci monitorowane, 43 są nieprawidłowe. To gwarantuje dochodzenia. Kliknij Kafelek, aby wyświetlić pulpit nawigacyjny rozwiązanie.  
  ![Kafelek Monitor wydajności sieci](./media/log-analytics-network-performance-monitor/npm-investigation01.png)

2. Poniższa ilustracja przykład można zauważyć, że istnieje obecnie 4 wydarzeń zdrowia i 4 łączy sieciowych, które są nieprawidłowe. Użytkownik chce zbadać problem i kliknij łącze sieci **Sieci Web programu Sharepoint** , aby dowiedzieć się, na poziomie głównym problemu.  
  ![przykład łącza nieprawidłowe sieci](./media/log-analytics-network-performance-monitor/npm-investigation02.png)

3. Strona przechodzenia zawiera wszystkie łącza podsieć w łączu sieci **Web programu Sharepoint** . Można zauważyć dla obu podsieć łączy, opóźnienie występują skrzyżowane progu wprowadzania łącze sieciowe są nieprawidłowe. Można też wyświetlić trendy opóźnienie łączy podsieć. Można użyć wyboru strefy czasowej sterowania na wykresie skoncentrować się na zakres czasu wymagane. Pora dnia po osiągnięciu opóźnienie Szczyt jest widoczny. Później możesz wyszukać dzienniki dla tego okresu zbadać ten problem. Kliknij pozycję **Wyświetl węzeł łącza** do przechodzenia dalej.  
  ![przykład łącza nieprawidłowe podsieci](./media/log-analytics-network-performance-monitor/npm-investigation03.png)

4.  Podobnie do poprzedniej strony, na stronie rozwijania łącza określonego podsieć lista łącza składowe węzeł w dół. Podobne działania tutaj tak samo jak w poprzednim kroku. Kliknij pozycję **topologii widoku** do wyświetlania topologii węzły 2.  
  ![przykład łączy węzeł nieprawidłowe](./media/log-analytics-network-performance-monitor/npm-investigation04.png)

5. Wszystkie ścieżki między 2 wybranych węzłów są wykreślane na mapie topologii. Można wyświetlać wizualizację topologii przeskoku skok trasy między dwoma węzłów na mapie topologii. Umożliwia Wyczyść obraz przekierowuje ile istnieje między dwa węzły i co ścieżek trwa pakiety danych. Gardła wydajności sieci są oznaczane kolorem czerwonym. Możesz odnaleźć połączenie sieciowe uszkodzone lub uszkodzenia urządzenia sieciowego, sprawdzając czerwony kolorowe elementy na mapie topologii.  
  ![przykład widoku nieprawidłowe topologii](./media/log-analytics-network-performance-monitor/npm-investigation05.png)

6. W okienku **Szczegółów ścieżki** można sprawdzić utratę, opóźnienia i liczby przeskoków w każdej ścieżki. W tym przykładzie widać, że istnieje 3 ścieżek nieprawidłowe wymienione w okienku. Aby wyświetlić szczegółowe informacje o tych nieprawidłowe ścieżek za pomocą paska przewijania.  Wybierz jedną ze ścieżek tak, aby dane są wykreślane topologii tylko jeden ścieżki za pomocą pól wyboru. Kółko myszy umożliwia powiększanie i pomniejszanie mapę topologii.

  W pod obrazem wyraźnie widać przyczyny problemów określonej sekcji sieci sprawdzając ścieżek i przeskoków w kolorze czerwonym. Kliknięcie w węźle mapy topologii powoduje wyświetlenie właściwości węzła, w tym nazwy FQDN oraz adres IP. Kliknięcie przeskoku zawiera adres IP przeskoku.  
  ![Topologia nieprawidłowe — przykład szczegóły ścieżki](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="next-steps"></a>Następne kroki

- [Dzienniki wyszukiwania](log-analytics-log-searches.md) , aby wyświetlić rekordy danych wydajności szczegółowego sieci.
