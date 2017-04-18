<properties 
    pageTitle="Monitorowanie pamięci podręcznej Azure Redis | Microsoft Azure" 
    description="Dowiedz się, jak monitorowanie kondycji i wydajności usługi wystąpień Azure Redis w pamięci podręcznej" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="sdanie"/>

# <a name="how-to-monitor-azure-redis-cache"></a>Jak można monitorować Azure Redis w pamięci podręcznej

Azure Redis pamięci podręcznej oferuje kilka opcji monitorowania wystąpienia z pamięci podręcznej. Można wyświetlić metryki, przypiąć metryki wykresy Startboard, dostosowywanie zakres dat i godzin monitorowania wykresy, dodać i usunąć metryki z wykresów i Ustaw alerty, gdy są spełnione określone warunki. Te narzędzia umożliwiają monitorowanie kondycji usługi wystąpień Azure Redis w pamięci podręcznej i ułatwiają zarządzanie pamięci podręcznej aplikacji.

Po włączeniu diagnostyki pamięci podręcznej metryki dla wystąpienia Azure Redis w pamięci podręcznej są zbierane co około 30 sekund i przechowywane, mogą być wyświetlane na wykresach metryki i sprawdzane przy reguł alertów.

Metryki pamięci podręcznej są pobierane za pomocą Redis polecenie [informacje](http://redis.io/commands/info) . Aby uzyskać więcej informacji na temat różnych informacji o wartości używane dla każdego pamięci podręcznej metryczne, zobacz [Dostępne wskaźniki i raportowania interwały](#available-metrics-and-reporting-intervals).

Aby wyświetlić metryki pamięci podręcznej, [Przejdź](cache-configure.md#configure-redis-cache-settings) do wystąpienia pamięci podręcznej w [Azure portal](https://portal.azure.com). Metryki dla wystąpienia Azure Redis w pamięci podręcznej są dostępne na karta **Redis metryki** .

![Redis metryki][redis-cache-redis-metrics-blade]

>[AZURE.IMPORTANT] Jeśli w karta **Redis metryki** zostanie wyświetlony następujący komunikat, wykonaj czynności opisane w sekcji [Włączanie diagnostyki pamięci podręcznej](#enable-cache-diagnostics) , aby umożliwić diagnostyki pamięci podręcznej.
>
>`Monitoring may not be enabled. Click here to turn on Diagnostics.`

Karta **Redis metryki** zawiera wykresy **monitorowania** metryki pamięci podręcznej. Każdego wykresu można dostosować przez dodawanie lub usuwanie metryki i Zmienianie interwału raportowania. Do przeglądania i konfigurowanie operacji i alertów, karta **Podręcznej Redis** zawiera sekcję **operacji** , wyświetlający pamięci podręcznej **zdarzeń** i **reguł alertów**.

## <a name="enable-cache-diagnostics"></a>Włączanie diagnostyki pamięci podręcznej

Azure Redis pamięci podręcznej udostępnia możliwość diagnostyki danych przechowywanych w konto miejsca do magazynowania, dzięki czemu można użyć dowolnych narzędzi, które mają dostęp do przetwarzania danych bezpośrednio. Aby diagnostyki pamięci podręcznej mają być zbierane przechowywane i wyświetlane w portalu Azure konto miejsca do magazynowania musi być skonfigurowany. Pamięć podręczną w tym samym region i subskrypcji udostępnianie tego samego konta miejsca do magazynowania narzędzia diagnostyczne i po zmianie konfiguracji dotyczy wszystkich pamięci podręcznej w subskrypcji, które znajdują się w danym regionie.

Aby włączyć i skonfigurować diagnostyki pamięci podręcznej, przejdź do karta **Podręcznej Redis** wystąpienia pamięci podręcznej. Jeśli nie włączono jeszcze diagnostyki, zamiast wykresu diagnostyki zostanie wyświetlony komunikat.

![Włączanie diagnostyki pamięci podręcznej][redis-cache-enable-diagnostics]

Kliknij wiadomość, aby wyświetlić karta **jednostki metryczne** i kliknij pozycję **Ustawienia diagnostyczne** , aby włączyć i skonfigurować ustawień diagnostycznych dla wystąpienia usługi pamięci podręcznej.

![Ustawienia diagnostyki][redis-cache-diagnostic-settings]

![Konfigurowanie narzędzia diagnostyczne][redis-cache-configure-diagnostics]

Kliknij przycisk **na** włączyć diagnostyki pamięci podręcznej i wyświetlić konfigurację diagnostyki.

Kliknij strzałkę po prawej stronie **Miejsca do magazynowania konta** zaznacz konto miejsca do magazynowania do przechowywania danych diagnostycznych. Aby uzyskać optymalną wydajność wybierz konto miejsca do magazynowania w tym samym regionie jako pamięci podręcznej.

Po diagnostyczne ustawienia są skonfigurowane, kliknij przycisk **Zapisz** , aby zapisać konfigurację. Należy zauważyć, że może potrwać kilka minut, aby zmiany zostały wprowadzone.

>[AZURE.IMPORTANT] Pamięć podręczną w tym samym region i subskrypcji mają te same ustawienia miejsca do magazynowania diagnostyki, a opcja Konfiguracja jest zmienione (diagnostyki włączone/wyłączone lub zmienianie konta miejsca do magazynowania) jest stosowana do wszystkich pamięci podręcznej w subskrypcji, które znajdują się w danym regionie.

Aby wyświetlić przechowywane metryki, sprawdź tabele na Twoim koncie miejsca do magazynowania z nazwami, które zaczynają się `WADMetrics`. Aby uzyskać więcej informacji na temat uzyskiwania dostępu do przechowywanej metryki poza Azure portal Zobacz przykładowych [danych programu Access Redis monitorowania pamięci podręcznej](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) .

>[AZURE.NOTE] W portalu Azure są wyświetlane tylko metryk, które są przechowywane na koncie wybranego miejsca do magazynowania. Jeśli zmienisz konta miejsca do magazynowania danych na koncie wcześniej skonfigurowane miejsca do magazynowania pozostanie dostępna do pobrania, ale nie jest wyświetlana w portalu Azure.  

## <a name="available-metrics-and-reporting-intervals"></a>Dostępne wskaźniki i raportowania interwały

Metryki pamięci podręcznej są zgłaszane przy użyciu kilka okresów raportowania, w tym **ostatniej godziny**, **dzisiaj**, **ostatnim tygodniu**i **niestandardowe**. Karta **metryki** dla każdego wykresu metryki Wyświetla średnia, minimalną i maksymalną wartość dla każdego metryki na wykresie, a niektóre metryki wyświetlanie sumy w zakresie raportowania. 

Każda metryka zawiera dwie wersje. Metryka jeden środki wydajności dla całej pamięci podręcznej i pamięci podręcznej, korzystające z [klaster](cache-how-to-premium-clustering.md), druga wersja metryki, która zawiera `(Shard 0-9)` wydajności miary nazwę dla pojedynczego shard w pamięci podręcznej. Na przykład jeśli pamięci podręcznej ma 4 odłamki `Cache Hits` jest łączną liczbę trafień dla całej pamięci podręcznej i `Cache Hits (Shard 3)` jest po prostu trafień dla tego shard pamięci podręcznej.

>[AZURE.NOTE] Nawet jeśli pamięć podręczną jest bezczynny z żadnych aplikacji połączony klient aktywny, mogą być widoczne niektóre działania pamięci podręcznej, takich jak połączonych klientów, użycie pamięci i operacji. To działanie jest normalne podczas operacji wystąpienia Azure Redis w pamięci podręcznej.

| Metryka            | Opis                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Trafienia w pamięci podręcznej        | Liczba trafień klucza w określonym interwale raportowania. To map do `keyspace_hits` z Redis [informacje](http://redis.io/commands/info) polecenia.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Chybienia w pamięci podręcznej      | Liczba kluczowe wyszukiwania nie powiodło się w określonym interwale raportowania. To map do `keyspace_misses` polecenia Redis informacje. Chybienia w pamięci podręcznej nie musi oznaczać, że występuje problem z pamięci podręcznej. Na przykład podczas korzystania z pamięci podręcznej odłogowania programowania deseniu, aplikacja wygląda pierwszego w pamięci podręcznej dla elementu. Jeśli element nie ma (Brak trafienia pamięci podręcznej), element jest pobrane z bazy danych i dodane do pamięci podręcznej do użycia w przyszłości. Chybienia w pamięci podręcznej są normalne zachowanie deseń programowania odłogowania pamięci podręcznej. Jeżeli liczba trafień pamięci podręcznej jest większa niż oczekiwano, należy sprawdzić logiki aplikacji, która wypełnia i odczytuje z pamięci podręcznej. Jeśli elementy są zostanie usunięty z pamięci podręcznej z powodu pamięci, a następnie może istnieć kilka Chybienia w pamięci podręcznej, ale będzie lepiej metryki monitorowanie dla pamięci `Used Memory` lub `Evicted Keys`. |
| Połączonych klientów | Liczba połączeń klientów w pamięci podręcznej w określonym interwale raportowania. To map do `connected_clients` polecenia Redis informacje. Po osiągnięciu [limitu połączeń](cache-configure.md#default-redis-server-configuration) próby nawiązania połączenia z późniejszych z pamięci podręcznej nie powiedzie się. Należy zauważyć, że nawet w przypadku aplikacji nie aktywnego klienta, mogą się kilka wystąpień powiązanych ze sobą klientów ze względu na wewnętrzne procesy i połączenia.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| Usuniętego klawiszy      | Liczba elementów usunięty z pamięci podręcznej raportowania przedziale określoną datę ukończenia, aby `maxmemory` limit. To map do `evicted_keys` polecenia Redis informacje.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Wygasłe klawiszy      | Liczba elementów wygasła z pamięci podręcznej w określonym interwale raportowania. Ta wartość mapy do `expired_keys` polecenia Redis informacje.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Otrzymuje              | Liczba operacji get z pamięci podręcznej w określonym interwale raportowania. Ta wartość jest sumą następujące wartości z informacje Redis polecenie wszystkie: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, i `cmdstat_getrange`i jest odpowiednikiem sumę trafień w pamięci podręcznej i Chybienia w przedziale raportowania.                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Redis obciążenie serwera | Wartość procentowa cykli, w których jest zajęty, przetwarzania i nie trwa oczekiwanie bezczynny wiadomości na serwerze Redis. Jeśli ten licznik osiągnie wartość 100, który oznacza serwera Redis występują trafienie ceiling wydajności i nie może przetworzyć Procesora pracować dowolną szybciej. Jeśli widzisz wysokie obciążenie serwera Redis zostanie wyświetlona wyjątki przekroczenia limitu czasu w kliencie. W tym przypadku należy rozważyć skalowanie w górę lub dzielony danych na wiele pamięci podręcznej.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| Zestawy              | Liczba operacji zestawu w pamięci podręcznej w określonym interwale raportowania. Ta wartość jest sumą następujące wartości z informacje Redis wszystkie polecenia: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange`, i `cmdstat_setnx`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Operacje razem  | Całkowita liczba poleceń przetwarzane przez serwer pamięci podręcznej w określonym interwale raportowania. Ta wartość mapy do `total_commands_processed` polecenia Redis informacje. Należy zauważyć, że użycie pamięci podręcznej Azure Redis wyłącznie dla pub-sub będzie nie metryki dla `Cache Hits`, `Cache Misses`, `Gets`, lub `Sets`, ale będzie `Total Operations` metryki odzwierciedlająca użycie pamięci podręcznej do operacji pub podrzędnych.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Używane pamięci       | Liczba pamięci podręcznej na potrzeby par klucz wartość w pamięci podręcznej w Megabajtach w określonym interwale raportowania. Ta wartość mapy do `used_memory` polecenia Redis informacje. Nie dotyczy to metadane lub fragmentacji.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| Pamięci używanych danych RSS   | Liczba używanych w MB w określonym czasie raportowania, w tym fragmentacji i metadanych pamięci podręcznej. Ta wartość mapy do `used_memory_rss` polecenia Redis informacje.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| PROCESOR               | Wykorzystanie Procesora serwera Azure Redis w pamięci podręcznej jako wartość procentową w określonym interwale raportowania. Ta wartość mapy systemu operacyjnego `\Processor(_Total)\% Processor Time` licznika.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Odczyt pamięci podręcznej        | Przeczytaj ilości danych z pamięci podręcznej w megabajtach na sekundę (MB/s) w określonym interwale raportowania. Ta wartość pochodzi z kartami interfejsu sieciowego, obsługujące maszyny wirtualnej, która obsługuje pamięci podręcznej, a nie określonych Redis. **Ta wartość odpowiada przepustowość sieci pamięci podręcznej. Jeśli chcesz skonfigurować alerty dotyczące ograniczenia przepustowości sieci po stronie serwera, należy utworzyć za pomocą tego `Cache Read` licznik. Znajdują się [w tej tabeli](cache-faq.md#cache-performance) limitów obserwowanych przepustowości dla różnych pamięci podręcznej ceny warstwy i rozmiarów.**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Wpisywanie pamięci podręcznej       | Ilość zapisanych w pamięci podręcznej w megabajtach na sekundę (MB/s) podczas określonej danych raportowania interwał. Ta wartość pochodzi z karty sieciowe obsługujące maszyny wirtualnej, która obsługuje pamięci podręcznej, a nie określonych Redis. Ta wartość odpowiada przepustowość sieci danych przesyłanych w pamięci podręcznej w kliencie.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |


## <a name="how-to-view-metrics-and-customize-charts"></a>Jak wyświetlić metryki i dostosowywanie wykresów

Omówienie metryki można wyświetlać pamięć podręczną na karta **Redis metryki** . Aby uzyskać dostęp do **Redis metryki** karta wybierz pozycję **wszystkie ustawienia** > **Redis metryki**.

![Redis metryki][redis-cache-redis-metrics]


Karta **Redis metryki** zawiera następujące wykresy.

| Redis metryki wykresu | Wyświetlane metryki     |
|------------------|-------------------|
| Pisanie odczyt pamięci podręcznej i pamięci podręcznej | Odczyt pamięci podręcznej |
|                            | Wpisywanie pamięci podręcznej |
| Połączonych klientów      | Połączonych klientów |
| Odwołania i Chybienia  | Trafienia w pamięci podręcznej        |
|                  | Chybienia w pamięci podręcznej      |
| Całkowity poleceń   | Operacje razem  |
| Pobieranie i zestawy    | Otrzymuje              |
|                  | Zestawy              |
| Użycie Procesora         | PROCESOR           |
| Użycie pamięci      | Używane pamięci   |
|                   | Pamięci używanych danych RSS |
| Redis obciążenie serwera | Obciążenie serwera   |
| Liczba klucza | Wszystkie klucze |
|           | Usuniętego klawiszy |
|           | Wygasłe klawiszy |


Aby uzyskać bardziej szczegółowy widok miar na wykresie określone i aby dostosować wykres, kliknij odpowiedni wykres z karta **Redis metryki** , aby wyświetlić karta **metryki** dla tego wykresu.

![Karta metryczne][redis-cache-metric-blade]

Wszystkie alerty, ustawione dla metryki wyświetlanych na wykresie są wyświetlane u dołu karta **metryki** dla tego wykresu.

Aby dodać lub usunąć metryki lub zmienić interwał raportowania, wybierz pozycję **Edytuj wykresu**.

Aby dodać lub usunąć metryki z wykresu, kliknij pole wyboru obok nazwy metryki. Aby zmienić interwał raportowania, kliknij żądany przedział. Aby zmienić **Typ wykresu**, kliknij żądany styl. Po dokonaniu odpowiednich zmian, kliknij przycisk **Zapisz**. 

![Edytowanie wykresu][redis-cache-edit-chart]

Po kliknięciu przycisku **Zapisz** zmiany pozostanie wybrane do momentu opuszczenia karta **metryki** . Po powrocie później, pojawi się oryginalny jednostki metryczne i zakres czasu ponownie. Aby uzyskać więcej informacji o dostosowywaniu wykresów zobacz [Monitorowanie usługi metryki](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

Aby wyświetlić metryki dla określonego czasu na wykresie, umieść wskaźnik myszy na jednym z określonych pasków lub punktów na wykresie, który odpowiada odpowiedni czas, a zostaną wyświetlone metryki dla tego przedziału.

![Wyświetlanie szczegółów wykresu][redis-cache-view-chart-details]

## <a name="how-to-monitor-a-premium-cache-with-clustering"></a>Jak można monitorować premium pamięci podręcznej klaster

Pamięć podręczną Premium, mające włączonym, [klaster](cache-how-to-premium-clustering.md) może zawierać maksymalnie 10 odłamki. Każdy shard ma własny metryki, a tych metryki są agregowane zapewnienie metryki pamięci podręcznej jako całość. Każda metryka zawiera dwie wersje. Metryka jeden środki wydajność dla całej pamięci podręcznej i wersji drugiej metryki, która zawiera `(Shard 0-9)` wydajności miary nazwę dla pojedynczego shard w pamięci podręcznej. Na przykład jeśli pamięci podręcznej ma 3 odłamki `Cache Hits` jest łączną liczbę trafień dla całej pamięci podręcznej i `Cache Hits (Shard 2)` jest po prostu trafień dla tego shard pamięci podręcznej.

Każdy wykres monitorowania Wyświetla metryki najwyższego poziomu w pamięci podręcznej wraz z metryki dla każdego shard pamięci podręcznej.

![Monitorowanie][redis-cache-premium-monitor]

Kursor myszy nad punktów danych zawiera szczegóły dotyczące tego punktu w czasie. 

![Monitorowanie][redis-cache-premium-point-summary]

Większe wartości są zwykle wartości zagregowanych pamięci podręcznej, podczas gdy mniejszymi wartościami są poszczególne metryki dla shard. Zauważ, że w tym przykładzie są trzy odłamki i trafień w pamięci podręcznej są rozłożone równomiernie na odłamki.

![Monitorowanie][redis-cache-premium-point-shard]

Aby wyświetlić więcej szczegółów kliknij wykres, aby wyświetlić rozwinięty widok na karta **metryki** .

![Monitorowanie][redis-cache-premium-chart-detail]

Domyślnie każdy wykres zawiera licznik wydajności pamięci podręcznej najwyższego poziomu, a także liczników wydajności dla poszczególnych odłamki. Możesz dostosować w karta **Edytowanie wykresu** .

![Monitorowanie][redis-cache-premium-edit]

Aby uzyskać więcej informacji dotyczących liczników dostępne wydajności Zobacz [Dostępne wskaźniki i raportowania odstępach](#available-metrics-and-reporting-intervals).


## <a name="operations-and-alerts"></a>Operacje i alertów

W sekcji **operacje** karta **Redis pamięci podręcznej** występują sekcji **zdarzeń** i **reguł alertów** .

![Oeprations][redis-cache-operations-events]

Aby wyświetlić listę ostatnich operacji pamięci podręcznej, kliknij pozycję wykresu **zdarzenia** , aby wyświetlić karta **zdarzeń** . Przykładami operacji wyszukiwania i ponownego generowania klawiszy dostępu i aktywacji i rozdzielczości reguł alertów. Aby uzyskać więcej informacji na temat każde zdarzenie kliknij zdarzenie w karta **zdarzeń** .

Aby uzyskać więcej informacji o zdarzeniach zobacz [Wyświetlanie zdarzeń i dzienników inspekcji](../monitoring-and-diagnostics/insights-debugging-with-events.md).

W sekcji **reguły alertów** Wyświetla liczbę alertów dla wystąpienia pamięci podręcznej. Reguły alertu umożliwia monitorowanie wystąpienia pamięci podręcznej i otrzymasz wiadomość e-mail, gdy niektórych wartość metryki osiągnięciu progu zdefiniowane w regule. 

Reguły alertów są obliczane w przybliżeniu co pięć minut, a po aktywowaniu regułę alertu są wysyłane powiadomienia skonfigurowane. Aktywacji alertu i powiadomienia nie są przetwarzane natychmiast; może wystąpić opóźnienie kilka minut, aby aktywować regułę alertu i powiadomienia wysyłane.

Reguły alertów można wyświetlać i ustawić z karta **metryki** dla określonego wykresu monitorowania lub karta **reguł alertów** .

Reguły alertów mieć następujące właściwości.

| Właściwość alertu                 | Opis                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Zasób                            | Zasób obliczane przez regułę alertu. Podczas tworzenia reguły alertu z pamięci podręcznej Redis, pamięci podręcznej jest zasobu.                                                                                                                                                                                                                                                                                                                                                  |
| Nazwa                                | Nazwa musi jednoznacznie identyfikować alertu w bieżącym wystąpieniu pamięci podręcznej.                                                                                                                                                                                                                                                                                                                                                                                       |
| Opis                         | Opcjonalny opis reguły alertu.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Metryka                              | Metryka monitorowanie przez regułę alertu. Aby uzyskać listę metryki pamięci podręcznej Zobacz dostępne wskaźniki i raportowania odstępach.                                                                                                                                                                                                                                                                                                                                             |
| Warunek                           | Operator warunku dla reguły alertu. Dostępne są następujące opcje możliwe: większe niż, większe niż lub równe mniejsze niż mniejsze niż lub równe                                                                                                                                                                                                                                                                                                                             |
| Progowa.                           | Wartość umożliwia porównanie z metryki przy użyciu operatora określony przez właściwość warunku. W zależności od metryki, ta wartość może być w bajtów na sekundę, bajtów, % lub liczba.                                                                                                                                                                                                                                                                                     |
| Okres                              | Określa czas, w którym wartość średnia metryki jest używana do porównywania alertu. Na przykład jeśli okresu przekracza ostatniej godziny, średniej wartości metryki przedziale poprzedniej godziny służy do porównywania. Jeśli chcesz otrzymywać powiadomienia o progu zostały spełnione z powodu kolekcji w działaniu krócej jest odpowiedni. Aby otrzymywać powiadomienia o istnieje stałej działanie powyżej progu, użyj okres dłuższy. |
| Usługa poczty e-mail i administratorów współpracujących | W przypadku wartości true administrator usługi i Współtworzenie administrator wysyłane są po aktywowaniu alert.                                                                                                                                                                                                                                                                                                                                                                    |
| Dodatkowe administratora poczty e-mail      | Adres e-mail opcjonalne dodatkowe administratora, aby otrzymywać powiadomienia o alercie zostało aktywowane.                                                                                                                                                                                                                                                                                                                                                                    |

Tylko jeden powiadomienie jest wysyłane na aktywacji alertu. Po przekroczeniu progu dla reguły i wysłać powiadomienia, reguła nie jest ponownie obliczana, aż Metryka przypada poniżej progu. Jeśli później Metryka przekracza próg, ponownie uaktywnić alertu i są wysyłane powiadomienie o nowej.

Aby wyświetlić wszystkie reguły alertów dla wystąpienia pamięci podręcznej, kliknij ten element **reguł alertów** karta **Redis pamięci podręcznej** . Aby wyświetlić tylko reguły alertów, które korzystają z określonej metryki, przejdź do karta **metryki** dla wykresu, który zawiera ten metrykę.

![Reguły alertów][redis-cache-alert-rules]

Aby dodać regułę alertu, kliknij przycisk **Dodaj alert** z karta **jednostki metryczne** lub karta **reguł alertów** . 

Wprowadź odpowiednie kryteria w karta reguły **Dodaj alert** , a następnie kliknij **przycisk OK**. 

![Dodawanie reguły alertu][redis-cache-add-alert]

>[AZURE.NOTE] Po utworzeniu reguły alertu przez kliknięcie przycisku **Dodaj alert** na karta **metryki** metryki wyświetlane na wykresie w tym karta są wyświetlane w liście rozwijanej **metryki** . Po utworzeniu reguły alertu przez kliknięcie przycisku **Dodaj alert** na karta **reguł alertów** wszystkie metryki pamięci podręcznej są dostępne z listy rozwijanej **metryki** .

Po zapisaniu go regułę alertu zostanie wyświetlona karta **reguły alertów** , a także karta **metryki** dla dowolnego wykresy metryki używane w regule alertu. Aby edytować reguły alertu, kliknij nazwę alertu, aby wyświetlić karta **Edytowanie reguły** . Z karta **Edytuj regułę** można edytować właściwości reguły, usunąć lub wyłączyć regułę alertu lub ponownie włączyć regułę, jeśli wcześniej została wyłączona.

>[AZURE.NOTE] Wszelkie zmiany wprowadzone we właściwościach reguły może potrwać kilka minut, zanim zostaną wprowadzone na karta **reguł alertów** lub karta **metryki** .

Po aktywowaniu regułę alertu, w zależności od konfiguracji reguły alertu po wysłaniu wiadomości e-mail, a ikona alarmu jest wyświetlana w obszarze **reguły alertów** na karta **Redis pamięci podręcznej** .

Reguły alertu jest traktowana jako rozwiązany, gdy alert już warunek ma wartość PRAWDA. Gdy warunek alertu został rozwiązany, ikona alertu jest zastępowany znacznik wyboru. Aby uzyskać szczegółowe informacje o aktywacji alertu i rozwiązania kliknij część **wydarzeń** na karta **Redis pamięci podręcznej** , aby wyświetlić zdarzenia na karta **zdarzeń** .

Aby uzyskać więcej informacji na temat alertów w Azure zobacz [odbieranie alertów](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

## <a name="metrics-on-the-redis-cache-blade"></a>Metryki na karta Redis pamięci podręcznej

Karta **Redis pamięci podręcznej** są wyświetlane następujące kategorie metryki.

-   [Monitorowanie wykresów](#monitoring-charts)
-   [Wykresy zastosowania](#usage-charts)

### <a name="monitoring-charts"></a>Monitorowanie wykresów

Sekcja **Monitorowanie** ma **trafienia i zanika**, **pobiera i ustawia** **połączeń**i wykresy **Polecenia sumy** .

![Monitorowanie wykresów][redis-cache-monitoring-part]

Wykresy **monitorowania** zawierają następujące metryki.

| Monitorowanie wykresu | Metryki pamięci podręcznej     |
|------------------|-------------------|
| Odwołania i Chybienia  | Trafienia w pamięci podręcznej        |
|                  | Chybienia w pamięci podręcznej      |
| Pobieranie i zestawy    | Otrzymuje              |
|                  | Zestawy              |
| Połączenia      | Połączonych klientów |
| Całkowity poleceń   | Operacje razem  |

Informacji na temat wyświetlania metryki i dostosowywanie pojedynczych wykresów w tej sekcji Zobacz następną sekcję [informacje o wyświetlaniu metryki i dostosowywanie wykresów metryki](#how-to-view-metrics-and-customize-charts) .

### <a name="usage-charts"></a>Wykresy zastosowania

Sekcja **zastosowania** **Redis obciążenie serwera**, **Użycie pamięci** **Sieci przepustowość sieci**i wykresy **Użycie Procesora** , a także wyświetla **poziomu ceny** dla danego wystąpienia pamięci podręcznej.

![Wykresy zastosowania][redis-cache-usage-part]

Wyświetla **poziom ceny** ceny pamięci podręcznej poziomu i może być używane do [skali](cache-how-to-scale.md) pamięci podręcznej do innej warstwy cennik.

Wykresy **zastosowania** zawierają następujące metryki.

| Wykres użycia       | Metryki pamięci podręcznej |
|-------------------|---------------|
| Redis obciążenie serwera | Obciążenie serwera   |
| Użycie pamięci      | Używane pamięci   |
| Przepustowość sieci | Wpisywanie pamięci podręcznej   |
| Użycie Procesora         | PROCESOR           |

Informacji na temat wyświetlania metryki i dostosowywanie pojedynczych wykresów w tej sekcji Zobacz następną sekcję [informacje o wyświetlaniu metryki i dostosowywanie wykresów metryki](#how-to-view-metrics-and-customize-charts) .
  
<!-- IMAGES -->

[redis-cache-enable-diagnostics]: ./media/cache-how-to-monitor/redis-cache-enable-diagnostics.png

[redis-cache-diagnostic-settings]: ./media/cache-how-to-monitor/redis-cache-diagnostic-settings.png

[redis-cache-configure-diagnostics]: ./media/cache-how-to-monitor/redis-cache-configure-diagnostics.png

[redis-cache-monitoring-part]: ./media/cache-how-to-monitor/redis-cache-monitoring-part.png

[redis-cache-usage-part]: ./media/cache-how-to-monitor/redis-cache-usage-part.png

[redis-cache-metric-blade]: ./media/cache-how-to-monitor/redis-cache-metric-blade.png

[redis-cache-edit-chart]: ./media/cache-how-to-monitor/redis-cache-edit-chart.png

[redis-cache-view-chart-details]: ./media/cache-how-to-monitor/redis-cache-view-chart-details.png

[redis-cache-operations-events]: ./media/cache-how-to-monitor/redis-cache-operations-events.png

[redis-cache-alert-rules]: ./media/cache-how-to-monitor/redis-cache-alert-rules.png

[redis-cache-add-alert]: ./media/cache-how-to-monitor/redis-cache-add-alert.png

[redis-cache-premium-monitor]: ./media/cache-how-to-monitor/redis-cache-premium-monitor.png

[redis-cache-premium-edit]: ./media/cache-how-to-monitor/redis-cache-premium-edit.png

[redis-cache-premium-chart-detail]: ./media/cache-how-to-monitor/redis-cache-premium-chart-detail.png

[redis-cache-premium-point-summary]: ./media/cache-how-to-monitor/redis-cache-premium-point-summary.png

[redis-cache-premium-point-shard]: ./media/cache-how-to-monitor/redis-cache-premium-point-shard.png

[redis-cache-redis-metrics]: ./media/cache-how-to-monitor/redis-cache-redis-metrics.png

[redis-cache-redis-metrics-blade]: ./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png



