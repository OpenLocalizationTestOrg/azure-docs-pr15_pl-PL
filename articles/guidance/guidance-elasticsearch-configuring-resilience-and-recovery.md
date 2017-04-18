<properties
   pageTitle="Konfigurowanie odporność i odzyskiwania na Elasticsearch Azure"
   description="Zagadnienia związane z elastyczność i odzyskiwania dla Elasticsearch."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>
   
# <a name="configuring-resilience-and-recovery-on-elasticsearch-on-azure"></a>Konfigurowanie odporność i odzyskiwania na Elasticsearch Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ten artykuł jest [częścią serii](guidance-elasticsearch.md). 

Funkcja klucza Elasticsearch jest obsługi, które zapewnia elastyczność wypadku awarii węzłów i/lub zdarzeń partition sieci. Replikacja jest najbardziej oczywiste sposób, w którym można poprawić elastyczność systemu wszelkie klaster Włączanie Elasticsearch upewnić się, czy więcej niż jednej kopii dowolnego elementu danych jest dostępny w różnych węzłach w przypadku, gdy jeden węzeł powinny stać się niedostępne. Jeśli węzeł jest tymczasowo niedostępny, węzłach zawierające repliki danych z brakujących węzła może służyć brakujących danych, aż problem zostanie rozwiązany. W przypadku dłuższych problem terminów Brak węzeł można zastąpić nową, a Elasticsearch można przywrócić dane do nowego węzła z repliki.

W tym miejscu możemy podsumowywanie elastyczność i odzyskiwanie opcjach Elasticsearch po obsługiwany w Azure i opisano kilka ważnych aspektów klaster Elasticsearch, które należy rozważyć, aby zminimalizować prawdopodobieństwo utraty danych i godzin odzyskiwania danych rozszerzonych.

W tym artykule przedstawiono również niektóre testach wykonanych umożliwia wyświetlanie skutków tej czynności dla różnych typów błędów klaster Elasticsearch i jak system odpowiada realizując.

Klaster Elasticsearch używa repliki, aby zachować dostępność i poprawić wydajność odczytu. Repliki powinny być przechowywane w różnych maszyny wirtualne z podstawowego odłamki, które mają być replikowane. Zamiar to, że jeśli maszyn wirtualnych, hostingu węzeł danych kończy się niepowodzeniem lub staje się niedostępna, system można nadal działa przy użyciu maszyny wirtualne, przytrzymując repliki.

## <a name="using-dedicated-master-nodes"></a>Za pomocą dedykowane węzły wzorca

Jeden węzeł w klastrze Elasticsearch jest wybierany jako węzeł wzorca. Celem tego węzła jest do wykonywania operacji zarządzania klaster, takich jak:

- Wykrywanie uszkodzone węzły i finalizowanie migracji do repliki.

- Przenoszenie odłamki do równoważenia obciążenia węzeł.

- Odzyskiwanie odłamki, gdy węzeł zostanie przeniesiony z powrotem do trybu online.

Należy rozważyć użycie dedykowanej węzły wzorca w krytycznych klastrów i upewnij się, że są 3 węzły dedykowane, którego rolę tylko ma być wzorca. Tej konfiguracji zmniejszenie ilości pracy intensywnie zasobów, zawierające te węzły do wykonywania (nie przechowywania danych lub obsługi kwerend) i pomaga zwiększyć stabilność klaster. Tylko jeden z tych węzłów wybranych, ale inne będzie zawierać kopię stanu systemu i może przejąć ulegnie awarii zaznaczony wzorzec.

## <a name="controlling-high-availability-with-azure--update-domains-and-fault-domains"></a>Sterowanie wysokiej dostępności z Azure — a domenami aktualizacji błędów 

Różne maszyny wirtualne można udostępniać tego samego sprzętu fizycznym. W Azure centrum danych stojaków pojedynczej może zawierać wiele maszyny wirtualne i wszystkie te maszyny wirtualne Udostępnianie wspólnych źródeł i sieci przełącznik. Awarii na poziomie stojaków pojedynczej w związku z tym może mieć wpływ na liczbę maszyny wirtualne. Azure używa koncepcji domen błędów i spróbuj rozdzielanie ten czynnik ryzyka. Domeny błędów odpowiada około grupy maszyny wirtualne, które współużytkują tej samej szafy. Aby upewnić się, że awarii na poziomie stojaków nie ulec awarii węzeł i węzły przytrzymując wszystkie jego repliki jednocześnie, należy upewnić się maszyny wirtualne są rozmieszczane domen błędów.

Podobnie maszyny wirtualne może zostać pobrany w dół [Azure tkaninie kontrolera](https://azure.microsoft.com/documentation/videos/fabric-controller-internals-building-and-updating-high-availability-apps/) do wykonywania planowanej konserwacji i system operacyjny uaktualniania. Azure przydziela maszyny wirtualne w celu zaktualizowania domeny. W przypadku planowanych zdarzeń konserwacji tylko maszyny wirtualne w domenie pojedynczej aktualizacji odbywa się w dowolnym momencie. Maszyny wirtualne w innych domenach aktualizacji pozostają uruchomione aż maszyny wirtualne w domenie aktualizacja zostanie zaktualizowany przełączane z powrotem do trybu online. W związku z tym należy upewnić się, że maszyny wirtualne hostingu węzły i ich repliki należą do domeny inną aktualizację, gdy jest to możliwe.

> [AZURE.NOTE] Aby uzyskać więcej informacji o domenach błędów i Aktualizuj domen zobacz [Zarządzanie dostępność maszyn wirtualnych][].

Nie można jawnie przydzielić maszyn wirtualnych do domeny określonej aktualizacji i domeny błędów. Przydział ten steruje Azure podczas tworzenia maszyny wirtualne. Jednak można określić, że maszyny wirtualne ma zostać utworzona w ramach zestawu dostępności. Maszyny wirtualne w ten sam zestaw dostępność będzie rozmieszczony a domenami aktualizacji błędów. Ręczne tworzenie maszyny wirtualne Azure tworzy każdego dostępność zestaw z dwóch błędów a domenami pięć aktualizacji. Maszyny wirtualne zostało przydzielonych do tych domen błędów i aktualizowanie domen pracującym zaokrąglanie podczas dalszej maszyny wirtualne zainicjowano obsługę administracyjną, w następujący sposób: 

| MASZYN WIRTUALNYCH | Błąd domeny | Aktualizowanie domeny |
|----|--------------|---------------|
|  1 |            0 |             0 |
|  2 |            1 |             1 |
|  3 |            0 |             2 |
|  4 |            1 |             3 |
|  5 |            0 |             4 |
|  6 |            1 |             0 |
|  7 |            0 |             1 |

> [AZURE.IMPORTANT] Jeśli tworzysz maszyny wirtualne za pomocą Menedżera zasobów Azure, każdy zestaw dostępność można przypisać maksymalnie 3 błędów a domenami 20 aktualizacji. Jest to ciekawe powód za pomocą Menedżera zasobów.

Ogólnie Umieść wszystkie maszyny wirtualne, które obsługują tę samą funkcję w ten sam zestaw dostępność, ale tworzenie zestawów różnych dostępność dla maszyny wirtualne, które wykonują różne funkcje. Z Elasticsearch, oznacza to, że należy rozważyć tworzenie dostępność co najmniej osobne zestawy dla:

- Maszyny wirtualne hostingu węzłów danych.
- Maszyny wirtualne hostingu węzłów klienckich (Jeśli korzystasz z ich).
- Maszyny wirtualne hostingu węzły wzorca.

Ponadto należy upewnić się, że każdy węzeł w klastrze jest aktualizacji domeny i domeny błędów, które należy pamiętać. Te informacje mogą pomóc w celu zapewnienia Elasticsearch nie utworzyć odłamki i ich repliki, w tym samym błędów i aktualizowanie domen, minimalizując możliwość shard i jego repliki podejmowaniu w dół w tym samym czasie. Możesz skonfigurować węzła Elasticsearch lustrzane rozkład sprzętu klaster przez skonfigurowanie [Sygnalizacja dostępności alokacji shard](https://www.elastic.co/guide/en/elasticsearch/reference/current/allocation-awareness.html#allocation-awareness). Na przykład można zdefiniować pary atrybutów niestandardowych węzeł o nazwie *faultDomain* i *updateDomain* w pliku elasticsearch.yml w następujący sposób:

```yaml
node.faultDomain: \${FAULTDOMAIN}
node.updateDomain: \${UPDATEDOMAIN}
```

W tym przypadku atrybuty są ustawione, używając wartości przechowywanych w * \${FAULTDOMAIN}* i * \${UPDATEDOMAIN}* zmienne środowiska po uruchomieniu Elasticsearch. Musisz również Dodaj następujące wpisy do pliku Elasticsearch.yml, aby wskazać, że *faultDomain* i *updateDomain* są alokacji atrybuty Sygnalizacja dostępności i określić zestawy dopuszczalne wartości tych atrybutów:

```yaml
cluster.routing.allocation.awareness.force.updateDomain.values: 0,1,2,3,4
cluster.routing.allocation.awareness.force.faultDomain.values: 0,1
cluster.routing.allocation.awareness.attributes: updateDomain, faultDomain
```

Sygnalizacja dostępności alokacji shard w połączeniu z [filtrowaniem alokacji shard](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/shard-allocation-filtering.html#shard-allocation-filtering) umożliwia Określa węzły, które mogą obsługiwać odłamki dla dowolnego danego indeksu.

Jeśli chcesz skalować poza liczbę błędów a domenami aktualizacji w zestawie dostępność, możesz utworzyć maszyny wirtualne w zestawach dodatkowe dostępności. Potrzebny dowiedzieć się, że węzłów na dostępność różnych zestawów odbywa się w obsługi jednocześnie. Spróbuj upewnić się, że każdy shard i co najmniej jeden z jego repliki są zawarte w ten sam zestaw dostępności.

> [AZURE.NOTE] Jest obecnie limit 100 maszyny wirtualne według dostępności. Aby uzyskać więcej informacji zobacz [Azure subskrypcji i limity dotyczące usługi, przydziały oraz ograniczenia](../azure-subscription-service-limits.md).

### <a name="backup-and-restore"></a>Kopia zapasowa i przywracanie

Użycie repliki nie zapewnia pełną ochronę losowych awarii (na przykład przypadkowego usunięcia cały klaster). Należy upewnić się regularnie kopie zapasowe danych w klastrze i że masz wypróbowanie i przetestowane strategię Przywracanie z tych kopii zapasowych.

Korzystanie z Elasticsearch [migawki i ich przywracanie interfejsy API](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) : elastyczne nie Caps tych. >> kopii zapasowych i przywracanie indeksy. Migawki można zapisywać plików udostępnionych. Możesz też wtyczki są dostępne, które można zapisują migawek distributed system plików usługi Hadoop (HDFS) ( [dodatek HDFS](https://github.com/elasticsearch/elasticsearch-hadoop/tree/master/repository-hdfs)) lub Azure miejsca do magazynowania ( [dodatek Azure](https://github.com/elasticsearch/elasticsearch-cloud-azure#azure-repository)).

Należy rozważyć następujące punkty, wybierając mechanizm przechowywania migawki:

- [Magazyn plików Azure](https://azure.microsoft.com/services/storage/files/) umożliwia wdrożenie udostępnionego systemu plików, który jest dostępny ze wszystkich węzłów.

- Dodatek HDFS należy używać tylko, jeśli są uruchomione Elasticsearch w połączeniu z Hadoop.

- Dodatek HDFS wymaga wyłączenia Menedżera zabezpieczeń Java uruchomiony wystąpienie Elasticsearch środowiska Java (maszyny wirtualnej Java).

- Dodatek HDFS obsługuje żadnego systemu plików HDFS zgodnego, pod warunkiem, że poprawnej konfiguracji Hadoop jest używana z Elasticsearch.

  
## <a name="handling-intermittent-connectivity-between-nodes"></a>Obsługa przerw w łączności węzłów

Zakłócenia sieci przejściowymi maszyn wirtualnych ponownie po konserwacja w centrum danych i innych podobnych zdarzeń mogą powodować węzły stać się tymczasowo niedostępne. W takiej sytuacji miejsce, w którym zdarzenie jest prawdopodobnie krótko, ogólnych o ponowne równoważenie odłamki występuje dwa razy w szybki kolejno (raz, kiedy zostanie wykryty błąd i ponownie po węzeł stają się widoczne we wzorcu) może stać się istotne obciążenie, które wpływa na wydajność. Możesz zapobiec inaccessibility tymczasowe węzeł powodujące wzorcu, aby wyrównać klaster, ustawiając *opóźnione\_przekroczenia limitu czasu* indeksu lub wszystkie indeksy. W poniższym przykładzie ustawiono 5 minut opóźnienia:

```http
PUT /_all/settings
{
    "settings": {
    "index.unassigned.node_left.delayed_timeout": "5m"
    }
}
```

Aby uzyskać więcej informacji zobacz [alokacji Delaying, gdy odchodzi węzeł](https://www.elastic.co/guide/en/elasticsearch/reference/current/delayed-allocation.html).

W przypadku sieci jest podatne na przerw w pracy można także modyfikować parametry, które skonfigurować wzorzec wykrywanie innego węzła nie jest już dostępny. Parametry te są częścią moduł [odnajdowania zen](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery-zen.html#modules-discovery-zen) do dyspozycji Elasticsearch i można ustawić je w pliku Elasticsearch.yml. Na przykład parametr *discovery.zen.fd.ping.retries* Określa, ile razy węźle głównym spróbuje ping inne węzły w klastrze przed podjęciem decyzji, które nie powiodło się. Ten parametr jest równa 3, ale można ją zmienić w następujący sposób:

```yaml
discovery.zen.fd.ping_retries: 6
```

## <a name="controlling-recovery"></a>Sterowanie odzyskiwania

Po przywróceniu połączenia do węzła po awarii wszelkie odłamki w tym węźle będzie konieczne można odzyskać w celu dostosowania ich aktualność. Domyślnie Elasticsearch odzyskuje odłamki w następującej kolejności:

- Według daty utworzenia odwrotnej indeksu. Indeksy nowszego zostaną odzyskane przed starsze indeksy.

- Według nazwy odwrotnej indeksu. Najpierw zostanie przywrócony indeksy, które mają nazwy, które są zgrupowane większe niż inne.

Jeśli niektóre indeksy są bardziej krytyczne od innych osób, ale nie odpowiadają te kryteria priorytet indeksów można zastąpić przez ustawienie właściwości *index.priority* . Indeksy z wartością nowszy dla tej właściwości zostaną przywrócone przed indeksy, które mają mniejsze wartości:

```http
PUT low_priority_index
{
    "settings": {
        "index.priority": 1
    }
}

PUT high_priority_index
{
    "settings": {
        "index.priority": 10
    }
}
```

Aby uzyskać więcej informacji zobacz [Priorytetów odzyskiwania indeksu](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/recovery-prioritization.html#recovery-prioritization).

Można monitorować proces odzyskiwania dla jednego lub kilku indeksów przy użyciu * \_odzyskiwania* interfejsu API:

```http
GET /high_priority_index/_recovery?pretty=true
```

Aby uzyskać więcej informacji zobacz [Odzyskiwanie indeksy](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-recovery.html#indices-recovery).

> [AZURE.NOTE] Klaster z odłamki, które wymagają odzyskiwania będzie miał stan *żółty* oznaczającą, że nie wszystkie odłamki są obecnie dostępne. Gdy dostępne są wszystkie odłamki, stan klaster należy przywrócić *zielony*. Klaster ze stanem *czerwony* wskazuje, że jeden lub więcej odłamki fizycznie Brak może być konieczne do przywrócenia danych z kopii zapasowej.

## <a name="preventing-split-brain"></a>Zapobieganie badania nad umysłem podziału 

Badania nad umysłem podziału może wystąpić, jeśli połączenia między węzły kończą się niepowodzeniem. Jeśli wzorcowej węzeł stanie się niedostępny części klaster, wybór ma być wykonywana w części sieci, która pozostaje stycznych, a inny węzeł będzie wzorcu. W klastrze skonfigurowane źle jest możliwe dla każdej części klaster mają różne wzorce niespójności danych lub uszkodzenia. Tego zjawiska nosi nazwę *badania nad umysłem podziału*.

Szanse badania nad umysłem podziału można zmniejszyć przez skonfigurowanie *minimalne\_wzorca\_węzły* właściwość modułu odnajdowanie, w pliku elasticsearch.yml. Ta właściwość określa liczbę węzłów musi być umożliwia wybór wzorca. Poniższy przykład ustawia wartość tej właściwości do 2:

```yaml
discovery.zen.minimum_master_nodes: 2
```

Ta wartość powinna być równa większość najmniejszej liczby węzłów, które są w stanie spełnić roli wzorca. Jeśli klaster ma 3 węzły wzorca, na przykład *minimalne\_wzorca\_węzły* powinna być równa 2. Jeśli masz 5 węzłów wzorca, *minimalne\_wzorca\_węzły* powinna być równa 3. Najlepiej, jeśli trzeba nieparzystej liczby węzłów wzorca.

> [AZURE.NOTE] Istnieje możliwość podziału badania nad umysłem występuje, jeśli wiele węzłów wzorca w tym samym klastrze są uruchamiane jednocześnie. Mimo to wystąpienie rzadkich, można wyłączyć jego uruchamiając kolejno węzły z krótkim opóźnieniu (5 w sekundach) między każdą z nich. 

## <a name="handling-rolling-updates"></a>Obsługa stopniowych aktualizacji

Jeśli przeprowadzasz uaktualnienie oprogramowania do węzłów siebie (na przykład migracji do nowszych wersji lub wykonywanie poprawkę), może być konieczne wykonywanie pracy na poszczególnych węzłach, które wymaga ich w trybie offline podjęcia przy zachowaniu pozostałą część dostępny klaster. W tej sytuacji należy rozważyć wykonania w następujący sposób. 

1. Upewnij się, że ponowne przydzielenie tego shard jest opóźnione wystarczająco aby uniemożliwić ponowne równoważenie odłamki z brakuje węzła przez pozostałą część klaster zaznaczony wzorzec. Domyślnie ponowne przydzielenie shard jest opóźnione 1 minuty, ale można wydłużyć czas trwania, jeśli jest mogą być niedostępne przez dłuższy czas. Poniższy przykład zwiększa opóźnienie 5 minut:

    ```http
    PUT /_all/_settings
    {
        "settings": {
            "index.unassigned.node_left.delayed_timeout": "5m"
        }
    }
    ```

    > [AZURE.IMPORTANT] Ponowne przydzielenie shard można także wyłączyć całkowicie, ustawiając *cluster.routing.allocation.enable* klastrze *Brak*. Należy jednak przy użyciu tej metody, jeśli nowe indeksy mogą zostać utworzone podczas węzeł jest niedostępny, jak to spowodować przydział kończy się niepowodzeniem, uzyskując klaster ze stanem czerwony.

2. Zatrzymywanie Elasticsearch w węźle utrzymania. Jeśli Elasticsearch działa jako usługa, można zatrzymać proces w sposób sterowane przy użyciu polecenia systemu operacyjnego. W poniższym przykładzie pokazano sposób zatrzymanie usługi Elasticsearch w jednym węźle uruchomionych Ubuntu:

    ```bash
    service elasticsearch stop
    ```

    Można też kliknąć można wykonać interfejsu API zamknięcia bezpośrednio w węźle:

    ```http
    POST /_cluster/nodes/_local/_shutdown
    ```

3. Przeprowadź niezbędne konserwacji na węźle

4. Ponownie uruchom węzeł i poczekaj, aż go, aby dołączyć klaster.

5. Ponowne włączanie alokacji shard:

    ```http
    PUT /_cluster/settings
    {
        "transient": {
            "cluster.routing.allocation.enable": "all"
        }
    }
    ```

> [AZURE.NOTE] Jeśli jest potrzebna obsługa więcej niż jeden węzeł, powtórz kroki od 2&ndash;4 w każdym węźle przed ponownym włączeniem alokacji shard.

Jeśli to możliwe, Zatrzymaj indeksowanie nowe dane w trakcie tego procesu. Dzięki temu można zminimalizować czas odtwarzania, gdy węzły są przywrócony do trybu online i ponownie dołączyć klaster.

Uwaga Aktualizacje automatyczne do elementów takich jak maszyny wirtualnej Java (najlepiej, wyłącz automatyczne aktualizacje dla tych elementów), szczególnie podczas korzystania z Elasticsearch w systemie Windows. Java update agent można pobrać najnowszą wersję dodatku Java automatycznie, ale może wymagać Elasticsearch do aktualizacji zostały zastosowane, należy ponownie uruchomić. To może spowodować utratę tymczasowe nieskoordynowane węzłów, w zależności od sposobu skonfigurowania Java Update agent. To również może spowodować innego wystąpienia Elasticsearch w tym samym klastrze z różnymi wersjami maszyny wirtualnej Java, co może spowodować problemy ze zgodnością.

## <a name="testing-and-analyzing-elasticsearch-resilience-and-recovery"></a>Badania i analizowania odporność Elasticsearch i odzyskiwanie

W tej sekcji opisano serię testów, które były wykonywane ma być obliczona odporność i odzyskiwania klaster Elasticsearch zawierające trzy węzły danych i trzy węzły wzorca.

Zostały przetestowane następujących scenariuszy: 

- Węzła i uruchom ponownie bez utraty danych. Węzeł danych zatrzymując i ponownie po 5 minut. Nie chcesz, aby ponownie przydzielić brakuje odłamki w tym okresie, nie dodatkowych we/wy jest poniesione w poruszanie odłamki został skonfigurowany Elasticsearch. Po ponownym uruchomieniu węzła procesu odzyskiwania powoduje odłamki w tym węźle wstecz na bieżąco.

- Węzła z utratą danych losowych. Węzeł danych jest zablokowany i dane, które przechowuje jest staną się zasymulowania awarii dysku losowych. Węzeł następnie uruchomieniu (po 5 minut) skutecznie działające jako zamiennik oryginalnego węzła. Proces odzyskiwania wymaga odbudowanie brakujących danych dla tego węzła i może obejmować przenoszenie odłamki przechowywanych na innych węzłach.

- Węzła i uruchom ponownie bez utraty danych, ale ponowne przydzielenie shard. Węzeł danych jest zablokowany i odłamki, które przechowuje są przesunąć do innych węzłów. Następnie uruchomieniu węzeł i ponowne przydzielenie więcej dotyczy, aby wyrównać klaster.

- Stopniowe aktualizacji. Każdy węzeł w klastrze zatrzymując i ponownie po krótki odstęp w celu zasymulowania maszyny jest uruchomienia po aktualizacji oprogramowania. Tylko jeden węzeł jest zablokowany w dowolnym momencie. Odłamki nie zostaną ponownie przydzielona podczas węzeł nie działa.

Każdy scenariusz podlegała samej obciążenie pracą, łącznie z różnych zadań spożyciu danych, agregacji i kwerend filtru podczas węzły zostały pobrane odzyskane i w trybie offline. Zbiorcze Wstawianie operacje w obciążenie pracą poszczególnych przechowywane dokumenty 1000 i były wykonywane przed jeden indeks podczas agregacji i kwerend filtru używane oddzielny indeks zawierające kilka miliony dokumentów. To jest umożliwiające wydajność kwerend ocenę niezależnie od wstawia zbiorczo. Każdy indeks zawiera pięć odłamki i jednej replice.

Poniższe sekcje podsumować wyniki tych testów, uwzględniając wszystkie obniżenie wydajności podczas węzeł jest w trybie offline lub ich odzyskania, a wszystkie błędy, które zostały zgłoszone. Wyniki są przedstawione w formie graficznej, wyróżnianie punkty, w których brakuje i Szacowanie czas potrzebny do pełnego odzyskiwanie i osiągnięcia podobnych poziom wydajności znajdowała się przed węzły przechodzenia do trybu offline systemu jeden lub więcej węzłów.

> [AZURE.NOTE] Wiązka test używane do wykonywania testy są dostępne w trybie online. Można dostosować i sprawdź odporność i odzyskiwania konfiguracji klaster przy użyciu tych uprzęże. Aby uzyskać więcej informacji zobacz [Uruchamianie testów elastyczność Elasticsearch][].

## <a name="node-failure-and-restart-with-no-data-loss-results"></a>Węzła i uruchom ponownie bez utraty danych: wyników

<!-- TODO; reformat this pdf for display inline --> 

Wyniki tego badania są wyświetlane w pliku [ElasticsearchRecoveryScenario1.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario1.pdf). Na wykresach wyświetlane profilu wydajności obciążenie pracą i fizycznie zasobów dla każdego węzła w klastrze. Początkowa część wykresy Pokaż systemu zwykle przez około 20 minut, po czym węzeł 0 jest zamknięty dla 5 minut przed uruchamiany ponownie. Statystyki przez 20 minut przedstawiono; system trwa około 10 minut odzyskiwanie i stabilizację. Jest to zilustrowane według stawki transakcji i czasy odpowiedzi na różne obciążenia.

Zwróć uwagę następujące punkty:

- Podczas badania zanotowano żadne błędy. Utracono żadnych danych, a wszystkie operacje ukończona pomyślnie.

- Stopy transakcji dla wszystkich trzech typów operacji (Wstawianie zbiorcze, kwerendy agregującej i kwerendy filtru) Inicjały i czasy odpowiedzi średnia zwiększone, gdy węzeł 0 jest w trybie offline.

- Podczas okresu zwrotu, transakcji stawki i czasu odpowiedzi kwerendy agregującej i kwerendy filtru operacje stopniowo zostały przywrócone. Wydajność do wstawiania zbiorcze odzyskać na chwilę podczas przed zmniejszenia. Jednak to prawdopodobnie ze względu na ilości danych powoduje indeks używane przez wstawianie zbiorcze w celu usprawnienia i stawki transakcji dla tej operacji są widoczne, aby zwolnić nawet przed węzeł 0 do trybu offline.

- Wykres wykorzystania Procesora dla węzła 0 ograniczona aktywności jest wyświetlana podczas fazy odzyskiwania to ze względu na lepszą dysku i aktywności sieci powodowanych przez mechanizm odzyskiwania węzeł ma zapoznaj się z danych, które ma odebrane, w trybie offline i aktualizowanie odłamki, które zawiera.

- Odłamki dla indeksy nie są rozpowszechniane dokładnie jednakowo we wszystkich węzłach. Istnieją dwa indeksy zawierające odłamki 5 i 1 replice, co łącznie odłamki 20. Dwa węzły w związku z tym będzie zawierać odłamki 6, a pozostałe dwa przytrzymaj 7 każdego. Jest to widoczne na wykresach wykorzystania Procesora podczas początkowego okresu 20 minut, 0 jest mniej obciążony niż pozostałe dwa. Po ukończeniu odzyskiwania niektórych przełączanie wydaje się przez węzeł 2 może stać się bardziej lekko załadowanego węzeł.

    
## <a name="node-failure-with-catastrophic-data-loss-results"></a>Węzła z utratą danych losowych: wyników

<!-- TODO; reformat this pdf for display inline -->

Wyniki tego badania są przedstawione w pliku [ElasticsearchRecoveryScenario2.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario2.pdf). Jak z pierwszego warunku początkowa część wykresy pokazano systemu zwykle przez około 20 minut, na który węzeł punkt 0 zostanie zamknięty przez 5 minut. W tym czasie danych Elasticsearch w tym węźle zostanie usunięty, symulowanie utracie danych losowych, przed uruchamiany ponownie. Pełnego odzyskiwania pojawi się podjęcie 12-15 minut przed poziomy wydajności pojawiały test zostaną przywrócone.

Zwróć uwagę następujące punkty:

- Podczas badania zanotowano żadne błędy. Utracono żadnych danych, a wszystkie operacje ukończona pomyślnie.

- Stopy transakcji dla wszystkich trzech typów operacji (Wstawianie zbiorcze, kwerendy agregującej i kwerendy filtru) Inicjały i czasy odpowiedzi średnia zwiększone, gdy węzeł 0 jest w trybie offline. W tym momencie profilu wydajności badania jest podobna do pierwszego scenariusza. Nie jest zaskakująco jako do tego punktu, scenariusze są takie same.

- W okresie odzyskiwania transakcji stawki i czasu odpowiedzi zostały przywrócone, mimo że w tym czasie w danych liczbowych zostało znacznie więcej zmienności. Jest to prawdopodobnie spowodowane dodatkowej pracy czy węzły w klastrze są wykonywane, dostarczając dane, których chcesz przywrócić brakujące odłamki. Dodatkowej pracy jest widoczne w wykorzystania Procesora, operacji na dysku i wykresy aktywności sieci.

- Na wykresie wykorzystania Procesora dla węzłów 0 i 1 są wyświetlane aktywności ograniczona podczas fazy odzyskiwania to ze względu na lepszą dysku i aktywności sieci spowodowane przez proces odzyskiwania. W pierwszym scenariuszu tylko węzeł ich odzyskania uwidocznione to zachowanie, ale w tym scenariuszu wydaje się prawdopodobnie że większość brakujących danych dla węzła 0 Trwa przywracanie kopii zapasowej z węzła 1.

- Aktywność We/Wy węzła 0 jest faktycznie zmniejszona, porównując do pierwszego scenariusza. Może to być spowodowane efektywności we/wy po prostu skopiować dane dla całego shard zamiast serii mniejszych żądania We/Wy potrzebne do istniejącego shard na bieżąco.

- Aktywność sieci dla wszystkich trzech węzłów wskazują seria działania danych jest wysłanych i odebranych węzłów. Scenariusz 1 tylko węzeł 0 wystawiony jako intensywności sieci, ale to działanie sprawiał być poniesione przez dłuższy czas. Ponownie ta różnica może być efektywność przekazywania całego danych dla shard jako pojedyncze żądanie zamiast serii mniejszych żądania odebrano odzyskiwanie shard.

## <a name="node-failure-and-restart-with-shard-reallocation-results"></a>Węzła i ponownym uruchomieniu z ponowne przydzielenie shard: wyników

<!-- TODO; reformat this pdf for display inline -->

Plik [ElasticsearchRecoveryScenario3.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario3.pdf) ilustruje wyników test. Zgodnie z pierwszego warunku początkowa część wykresy wyświetlić systemu zwykle przez około 20 minut, na który węzeł punkt 0 jest zamknięty przez 5 minut. W tym momencie klaster Elasticsearch próbuje przywrócić brakujące odłamki i wyrównać odłamki w pozostałych węzłach. Po 5 minut węźle 0 jest tryb online, a następnie ponownie klaster ma wyrównać odłamki. Wydajność zostanie przywrócony po 12-15 minut.

Zwróć uwagę następujące punkty:

- Podczas badania zanotowano żadne błędy. Utracono żadnych danych, a wszystkie operacje ukończona pomyślnie.

- Porzucone stawki transakcji dla wszystkich trzech typów operacji (Wstawianie zbiorcze, kwerendy agregującej i kwerendy filtru) i czas odpowiedzi średnia znacznie zwiększone podczas węzeł 0 jest w trybie offline w porównaniu z poprzedniego dwóch testów. To jest ze względu na to działanie lepszą klaster ponownego utworzenia brakuje odłamki i ponowne równoważenie klaster co wypukłe ilustracji wykonania dysku i sieci węzły 1 i 2 w tym okresie.

- W okresie po węzeł 0 jest tryb online, transakcji stawki i czasu odpowiedzi pozostają nietrwałe.

- Procesor wykorzystania dysku aktywności wykresów i węzeł 0 pokazuje bardzo ograniczona początkowej działania na etapie odzyskiwania. Jest to, ponieważ w tym momencie węzeł 0 nie służy jakichkolwiek danych. Po upływie około 5 minut węzeł ulega zapaleniu akcji < pochodzące: tak mi się przynajmniej snort głos. I nie mam pochodzących z lepszym sposobem mimo to powiedzieć.  >> wskazywanych przez zwiększenie liczby sieci, dysku i Procesora aktywności. Jest to prawdopodobnie spowodowane przez klaster dystrybucji odłamki w węzłach. Węzeł 0 wyświetlone normalnego działania.
  
## <a name="rolling-updates-results"></a>Stopniowe aktualizacji: wyników

<!-- TODO; reformat this pdf for display inline -->

Wyniki tego badania w pliku [ElasticsearchRecoveryScenario4.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario4.pdf)wskazują, jak każdy węzeł jest tryb offline i przełączane do tyłu w górę ponownie kolejno. Każdy węzeł jest zamknięty dla 5 minut przed uruchamiany ponownie, po czym zatrzymaniu następnego węzła w sekwencji.

Zwróć uwagę następujące punkty:

- Podczas ponownym włączeniu każdego węzła, rozsądnie nawet pozostaje wydajność w godzinach przepustowość i odpowiedzi.

- Aktywność dysku zwiększa dla każdego węzła przez krótki czas zostanie przeniesiony Wstecz w trybie online. Jest to prawdopodobnie spowodowane procesu odzyskiwania do przodu stopniowych wszelkie zmiany, które wystąpiły podczas węzeł nie działa.

- Gdy węzeł do trybu offline, tych najwyższych wartościach w działaniu sieci występuje w pozostałych węzłach. Tych najwyższych wartościach również wystąpić po uruchomieniu węzła.

- Po usunięciu ostatni węzeł system wprowadza okres znacznej zmienności. Jest to prawdopodobnie spowodowane przez proces odzyskiwania konieczności synchronizowanie zmian w każdym węźle i upewnij się, że wszystkie repliki i ich odpowiednich odłamki są zgodne. W jednym punkcie zbiorcze kolejnych przyczyny nakładu Wstawianie operacje, aby przekroczenia limitu czasu i kończą się niepowodzeniem. Błędy zgłoszone każdorazowo zostały:

```
Failure -- BulkDataInsertTest17(org.apache.jmeter.protocol.java.sampler.JUnitSampler$AnnotatedTestCase): java.lang.AssertionError: failure in bulk execution:
[1]: index [systwo], type [logs], id [AVEg0JwjRKxX_sVoNrte], message [UnavailableShardsException[[systwo][2] Primary shard is not active or isn't assigned to a known node. Timeout: [1m], request: org.elasticsearch.action.bulk.BulkShardRequest@787cc3cd]]

```

Kolejne eksperyment pokazano, że wprowadzenie opóźnienie kilka minut między cykliczne każdy węzeł usunięte ten błąd, aby znajdował się najprawdopodobniej spowodowany niezgodności między procesu odzyskiwania próby przywrócenia jednocześnie kilka węzłów i zbiorcze wstawić operacje próby przechowywać tysiące nowe dokumenty.


## <a name="summary"></a>Podsumowanie

Przeprowadzonych badań wskazuje, że:

- Elasticsearch jest wysoce mechanizm do najczęściej używanych rodzajów błąd może wystąpić w klastrze.

- Elasticsearch można odzyskać szybko atrakcyjnego klaster podlega utratą danych losowych w węźle. To może się zdarzyć, jeśli Konfigurowanie Elasticsearch, aby zapisać dane do magazynu tymczasowych i węzeł jest następnie reprovisioned po ponownym uruchomieniu. Te wyniki pokazują, że nawet w tym przypadku ryzyka związanego z magazynu tymczasowych najprawdopodobniej większe niż korzyści wydajności, które zawiera ten rodzaj miejsca do magazynowania.

- W pierwsze trzy scenariusze, nie podczas wystąpiły błędy w Wstawianie równoczesne zbiorcze, agregacji i Filtr kwerendy obciążenia węzeł wykonano odzyskane i w trybie offline.

- Tylko ostatni scenariusz wskazywana utracie danych, a to dotyczy tylko nowych danych dodana. Najlepiej w aplikacjach wykonywania spożyciu danych zmniejszeniu to prawdopodobieństwo próbując operacje wstawiania, które nie powiodło się, jak typ błędu zgłoszone jest wysoce prawdopodobnie przejściowych.

- Wyniki ostatniego testu również wskazują, że jeśli przeprowadzasz planowanej konserwacji węzłów w klastrze, wydajności skorzystają Jeśli zezwolisz na kilka minut między cykliczne jeden węzeł i dalej. W sytuacjach niezaplanowane (na przykład centrum danych odtwarzanie węzły po wykonaniu aktualizacji systemu operacyjnego) ma mniej kontrolę nad sposobu i czasu węzły zarejestrowane w dół i ponownie uruchomić. Konfliktu, który pojawia się po Elasticsearch spróbuje przywrócić stan klaster po dostawie sekwencyjne węzeł może spowodować błędy i limitów czasu. 

[Zarządzanie dostępność maszyn wirtualnych]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[Uruchamianie testów elastyczność automatyczną Elasticsearch]: guidance-elasticsearch-running-automated-resilience-tests.md
