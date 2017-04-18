<properties 
    pageTitle="Jak rozwiązywać problemy z pamięci podręcznej Azure Redis | Microsoft Azure" 
    description="Dowiedz się, jak rozwiązać typowe problemy z Azure Redis w pamięci podręcznej." 
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
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-troubleshoot-azure-redis-cache"></a>Jak rozwiązywać problemy z pamięci podręcznej Azure Redis

Ten artykuł zawiera wskazówki dotyczące rozwiązywania problemów z następujących kategorii problemy Azure Redis w pamięci podręcznej.

-   [Rozwiązywanie problemów po stronie klienta](#client-side-troubleshooting) — ta sekcja zawiera wskazówki dotyczące identyfikowanie i rozwiązywanie problemów powodowanych przez aplikację nawiązywanie Azure Redis w pamięci podręcznej.
-   [Rozwiązywanie problemów po stronie serwera](#server-side-troubleshooting) — ta sekcja zawiera wskazówki dotyczące identyfikowanie i rozwiązywanie problemów powodowanych po stronie serwera Azure Redis w pamięci podręcznej.
-   [Wyjątki przekroczenia limitu czasu StackExchange.Redis](#stackexchangeredis-timeout-exceptions) — ta sekcja zawiera informacje dotyczące rozwiązywania problemów, gdy przy użyciu klienta StackExchange.Redis.


>[AZURE.NOTE] Kilka kroków w tym przewodniku zawiera instrukcje, aby uruchomić polecenia Redis i monitorować różne wskaźniki. Aby uzyskać więcej informacji i instrukcje zobacz artykuły w sekcji [dodatkowe informacje](#additional-information) .

## <a name="client-side-troubleshooting"></a>Rozwiązywanie problemów po stronie klienta


W tej sekcji omówiono problemów występujących ze względu na warunku w aplikacji klienckiej.

-   [Ciśnienia pamięci na komputerze klienckim](#memory-pressure-on-the-client)
-   [Serii ruchu](#burst-of-traffic)
-   [Duży klienta użycie Procesora](#high-client-cpu-usage)
-   [Przekroczona przepustowość po stronie klienta](#client-side-bandwidth-exceeded)
-   [Rozmiar dużych żądanie i odpowiedź](#large-requestresponse-size)
-   [Co się stało z moimi danymi w Redis?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-the-client"></a>Ciśnienia pamięci na komputerze klienckim

#### <a name="problem"></a>Problem

Ciśnienia pamięci na komputerze klienckim, prowadzi do wszystkich rodzajów problemów z wydajnością, które może opóźnić przetwarzania danych, która została wysłana przez wystąpienie Redis bez zwłoki. Gdy trafienia pamięci, system zwykle musi być danych strony z pamięci fizycznej w pamięci wirtualnej, który znajduje się na dysku. Ten *błąd strony* powoduje, że system w celu znacznie spowolnić pracę.

#### <a name="measurement"></a>Miary 

1.  Monitorowanie użycia pamięci na komputerze, aby upewnić się, że nie przekracza dostępną pamięć. 
2.  Monitorowanie `Page Faults/Sec` licznika. Większość systemów będą miały niektórych błędów stron nawet podczas normalnego działania, tak obserwowania maksymalnej w tym licznika błędów strony, które odpowiadają limity czasu.

#### <a name="resolution"></a>Rozdzielczość

Uaktualnianie klienta na kliencie większy rozmiar pamięci Wirtualnej z większą ilością pamięci lub Drąż do usługi upodobania pamięci, aby zmniejszyć consuption pamięci.


### <a name="burst-of-traffic"></a>Serii ruchu

#### <a name="problem"></a>Problem

Seria ruchu w połączeniu z słabej `ThreadPool` ustawień może powodować opóźnienia podczas przetwarzania danych już wysłane przez serwer Redis, ale jeszcze nie zostały zużyte po stronie klienta.

#### <a name="measurement"></a>Miary 

Monitorowanie jak usługi `ThreadPool` statystyki zmian w czasie przy użyciu kodu, [Tak jak poniżej](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Możesz również sprawdzić `TimeoutException` wiadomości od StackExchange.Redis. Oto przykład:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

W komunikacie powyżej istnieje kilka problemów, które mogą być interesujące:

 1. Należy zauważyć, że w `IOCP` sekcji i `WORKER` sekcji masz `Busy` wartość, która jest większa niż `Min` wartość. Oznacza to, że usługi `ThreadPool` ustawienia wymagać dostosowania.
 2. Można też wyświetlić `in: 64221`. To oznacza, że 64211 bajtów otrzymano w warstwie socket jądra, ale jeszcze nie zostały przeczytane przez aplikację (np. StackExchange.Redis). Zazwyczaj oznacza to, że aplikacja nie jest odczycie z sieci tak szybko, jak serwer jest go do Ciebie wysyłać.

#### <a name="resolution"></a>Rozdzielczość

Skonfiguruj [Ustawienia puli](https://gist.github.com/JonCole/e65411214030f0d823cb) , aby upewnić się, że do puli wątków będzie rozbudowy szybko w serii scenariuszach.


### <a name="high-client-cpu-usage"></a>Duży klienta użycie Procesora

#### <a name="problem"></a>Problem

Użycie procesora CPU wysoka na komputerze klienckim jest wskazujące, że system nie może przechowywać w pracy, który ma zostać monit o wykonanie. Oznacza to, że klient może zakończyć się niepowodzeniem w przetwarzania odpowiedzi Redis w odpowiednim czasie, mimo że Redis bardzo szybko wysłana odpowiedź.

#### <a name="measurement"></a>Miary

Monitorowanie użycie Procesora szerokości systemu za pośrednictwem Azure Portal lub skojarzone licznika. Uważaj, aby nie monitorowanie Procesora *proces* , ponieważ jeden proces może mieć niskie użycie Procesora w tym samym czasie, które Procesora systemu ogólnego może być duży. Obserwowania maksymalnej w użycie Procesora, które odpowiadają limity czasu. Wyniku Procesora duży, może także zostać wyświetlony wysoki `in: XXX` wartości w `TimeoutException` komunikaty o błędach zgodnie z opisem w sekcji [serii danych](#burst-of-traffic) .

>[AZURE.NOTE] StackExchange.Redis 1.1.603 i nowszy zawiera `local-cpu` metryki w `TimeoutException` komunikaty o błędach. Upewnij się, możesz korzystać z najnowszych wersji [pakietu StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Występują błędy stale usunięte w kodzie, aby była bardziej rozbudowany do przekroczenia limitu czasu, ważne jest najnowszą wersję.

#### <a name="resolution"></a>Rozdzielczość

Uaktualnianie do większy rozmiar pamięci Wirtualnej z możliwości Procesora lub badanie, co powoduje Procesora tych najwyższych wartościach. 



### <a name="client-side-bandwidth-exceeded"></a>Przekroczona przepustowość po stronie klienta

#### <a name="problem"></a>Problem

Różne klienta wymiarach mają ograniczenia na jak przepustowości sieci są dostępne. Jeśli dostępna przepustowość jest większa niż klienta, następnie danych nie będą przetwarzane po stronie klienta tak szybko, jak serwer wysyła go. To może prowadzić do przekroczenia limitu czasu.

#### <a name="measurement"></a>Miary

Monitorowanie, jak zmienić wykorzystanie przepustowości sieci upływu czasu przy użyciu kodu, [Tak jak poniżej](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Zauważ, że ten kod może nie działać pomyślnie w niektórych środowiskach z ograniczonymi uprawnieniami (na przykład Azure witryn sieci web).

#### <a name="resolution"></a>Rozdzielczość 

Zwiększ rozmiar pamięci Wirtualnej klienta lub zmniejszenie wykorzystania przepustowości sieci.


### <a name="large-requestresponse-size"></a>Rozmiar dużych żądanie i odpowiedź

#### <a name="problem"></a>Problem

Duże żądanie/odpowiedź mogą powodować przekroczenia limitu czasu. Na przykład załóżmy, że wartość limitu czasu skonfigurowany na komputerze klienckim wynosi 1 sekundę. Aplikacja żąda dwóch klawiszy (na przykład "A" i "B") w tym samym czasie (za pomocą tego samego połączenia sieci fizycznej). Większość klientów obsługuje "Pipelining" żądań, tak że oba "A" i "B" zostaną wysłane wezwania w sieci na serwerze, jeden po drugim nie czekając odpowiedzi. Serwer będzie wysyłać odpowiedzi w tej samej kolejności. Jeśli odpowiedź "A" jest duży wystarczająco go pochłaniają większość limit czasu dla kolejne żądania. 

Poniższy przykład pokazuje w tym scenariuszu. W tym scenariuszu szybko są wysyłane żądanie "A" i "B", serwera uruchamia się szybkie wysyłanie odpowiedzi "A" i "B", ale ze względu na czas transfer danych, "B" utkniesz za inne zaproszenie i limit mimo że szybko odpowiedź serwera.

  	|-------- 1 Second Timeout (A)----------|
  	|-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Miary

To jest trudne do pomiaru. Masz zasadniczo instrumentu kod klienta do śledzenia dużej zaproszenia i odpowiedzi. 

#### <a name="resolution"></a>Rozdzielczość

1.  Redis jest zoptymalizowana pod kątem dużej liczby małe wartości, a nie kilku dużych wartości. Preferowanym rozwiązaniem jest podziału dane powiązane z mniejszymi wartościami. Zobacz [co zakres idealny wartości rozmiaru dla redis? Jest zbyt duża, 100KB?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) wpis, aby uzyskać szczegółowe informacje wokół Dlaczego zalecane mniejszymi wartościami.
2.  Zwiększyć rozmiar do maszyn wirtualnych (w przypadku klienta i serwera pamięci podręcznej Redis), aby uzyskać wyższy możliwości przepustowości, zmniejszenia czasu przesyłania danych większych odpowiedzi. Należy zauważyć, że wprowadzenie większej przepustowości tylko z serwera lub po prostu na klienta może być za mało. Zmierz wykorzystanie przepustowości sieci i porównać ją do funkcji rozmiar maszyn wirtualnych obecnie masz.
3.  Zwiększenie liczby `ConnectionMultiplexer` obiekty są żądania okrężny i używanie różnych połączeń.


### <a name="what-happened-to-my-data-in-redis"></a>Co się stało z moimi danymi w Redis?

#### <a name="problem"></a>Problem

Oczekiwana dla określonych danych w moim wystąpieniu Azure Redis w pamięci podręcznej, ale nie wydaje się tam.

##### <a name="resolution"></a>Rozdzielczość

Zobacz [co się stało z moimi danymi w Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) możliwe przyczyny i rozwiązania.


## <a name="server-side-troubleshooting"></a>Rozwiązywanie problemów po stronie serwera

W tej sekcji omówiono problemów występujących ze względu na warunku na serwerze pamięci podręcznej.

-   [Ciśnienia pamięci na serwerze](#memory-pressure-on-the-server)
-   [Użycie procesora CPU wysoka / ładowanie serwera](#high-cpu-usage-server-load)
-   [Przekroczono przepustowość po stronie serwera](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-the-server"></a>Ciśnienia pamięci na serwerze

#### <a name="problem"></a>Problem

Ciśnienia pamięci po stronie serwera prowadzi do wszystkich rodzajów problemów z wydajnością, które może opóźnić przetwarzania żądania. Gdy trafienia pamięci, system zwykle musi być danych strony z pamięci fizycznej w pamięci wirtualnej, który znajduje się na dysku. Ten *błąd strony* powoduje, że system w celu znacznie spowolnić pracę. Istnieje kilka możliwych przyczyn tego ciśnienia pamięci: 

1.  Pamięć podręczną do pełnego wykorzystania możliwości wypełnił z danymi. 
2.  Redis jest wyświetlany fragmentacji pamięci wysokiej - najczęściej spowodowany przechowywania dużych obiektów (Redis jest zoptymalizowana pod kątem małych obiektów — zobacz [, co to jest zakres idealny wartości rozmiaru dla redis? Jest zbyt duża, 100KB?](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) Publikowanie Aby uzyskać szczegółowe informacje). 

#### <a name="measurement"></a>Miary

Redis udostępnia dwa metryk, które mogą ułatwić zidentyfikowanie tego problemu. Pierwszym jest `used_memory` , a druga `used_memory_rss`. [Te wskaźniki](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) są dostępne w Azure Portal lub za pomocą polecenia [Redis informacje](http://redis.io/commands/info) .

#### <a name="resolution"></a>Rozdzielczość

Istnieje kilka możliwych zmian, które można wprowadzić w celu zapewnienia pamięć prawidłowy:

1. [Konfigurowanie zasad pamięci](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) i ustaw czas wygaśnięcia na kluczy. Należy zauważyć, że to może nie być wystarczające, jeśli masz fragmentacji.
2. [Konfigurowanie wartość zastrzeżone maxmemory](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) , który jest duży, aby zadośćuczynienia fragmentacji pamięci.
3. Rozdziel dużych pamięci podręcznej obiektów do mniejszych powiązane z nim obiekty.
4. [Skala](cache-how-to-scale.md) rozmiar pamięci podręcznej.
5. Jeśli korzystasz z [pamięci podręcznej premium z klastrem Redis włączone](cache-how-to-premium-clustering.md) można [zwiększyć liczbę odłamki](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Użycie procesora CPU wysoka / ładowanie serwera

#### <a name="problem"></a>Problem

Wysoka użycie Procesora może oznaczać, że po stronie klienta może się nie powieść, przetwarzania odpowiedzi Redis w odpowiednim czasie, mimo że Redis wysyłane odpowiedzi bardzo szybko.

#### <a name="measurement"></a>Miary

Monitorowanie użycie Procesora szerokości systemu za pośrednictwem Azure Portal lub skojarzone licznika. Uważaj, aby nie monitorowanie Procesora *proces* , ponieważ jeden proces może mieć niskie użycie Procesora w tym samym czasie, które Procesora systemu ogólnego może być duży. Obserwowania maksymalnej w użycie Procesora, które odpowiadają limity czasu.

#### <a name="resolution"></a>Rozdzielczość

[Skala](cache-how-to-scale.md) do większej pamięci podręcznej poziomu z możliwości Procesora lub badanie, co powoduje Procesora tych najwyższych wartościach. 

### <a name="server-side-bandwidth-exceeded"></a>Przekroczono przepustowość po stronie serwera

#### <a name="problem"></a>Problem

Wystąpienia różnych wymiarach pamięci podręcznej mają ograniczenia na jak przepustowości sieci są dostępne. Jeśli serwer jest większa niż dostępna przepustowość, danych nie będzie można wysłać do klienta jako szybko. To może prowadzić do przekroczenia limitu czasu.

#### <a name="measurement"></a>Miary

Można monitorować `Cache Read` przeczytaj metryki, która jest ilości danych z pamięci podręcznej w megabajtach na sekundę (MB/s) w określonym interwale raportowania. Ta wartość odpowiada przepustowość sieci pamięci podręcznej. Jeśli chcesz skonfigurować alerty dotyczące ograniczenia przepustowości sieci po stronie serwera, możesz utworzyć je przy użyciu to `Cache Read` licznik. Porównanie programu odczyt z wartościami w [tej tabeli](cache-faq.md#cache-performance) limitów obserwowanych przepustowości dla różnych pamięci podręcznej ceny warstwy i rozmiarów.

#### <a name="resolution"></a>Rozdzielczość

Jeśli użytkownik spójne u obserwowanych maksymalna przepustowość rozmiar cennik warstwa i pamięci podręcznej, warto rozważyć [skalowania](cache-how-to-scale.md) cennik warstwa lub rozmiar, zawierającą większa przepustowość sieci, używając wartości w [tej tabeli](cache-faq.md#cache-performance) jako szablon.


## <a name="stackexchangeredis-timeout-exceptions"></a>Wyjątki przekroczenia limitu czasu StackExchange.Redis

StackExchange.Redis używa ustawienie nazwanych konfiguracji `synctimeout` synchroniczne operacji, które ma wartość domyślną 1000 ms. Jeśli połączenie synchroniczne nie ukończyć w określonym czasie, klienta StackExchange.Redis zgłasza błąd przekroczenia limitu czasu podobny do następującego przykładu.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Ten komunikat o błędzie zawiera metryk, które mogą być pomocne wskaż przyczyny i możliwych rozwiązaniu problemu. Poniższa tabela zawiera szczegółowe informacje o metryki komunikat o błędzie.

| Metryka komunikat o błędzie | Szczegóły                                                                                                                                                                                                                                          |
|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inst       | W ostatnim przedział czasu: zostały wydane polecenia 0                                                                                                                                                                                              |
| Menedżer        | Podczas wykonywania polecenia Menedżer socket `socket.select` co oznacza prosi systemu operacyjnego, aby wskazać socket, zawierającą coś robić; problem: czytnik nie aktywnie odczytuje z sieci, ponieważ nie Pomyśl, zawiera coś, co zrobić |
| kolejki      | Istnieją 73 operacje razem w trakcie wykonywania                                                                                                                                                                                                        |
| QU         | 6 w trakcie wykonywania operacji w kolejce niewysłanych i nie zostały zapisane w sieci wychodzącego                                                                                                                                                           |
| QS         | Wysłano 67 he w trakcie wykonywania operacji na serwerze, ale odpowiedź nie jest jeszcze dostępna. Odpowiedzi może być `Not yet sent by the server` lub`sent by the server but not yet processed by the client.`                                                   |
| QC         | 0 w trakcie wykonywania operacji przejrzane odpowiedzi, ale nie zostały oznaczone jako ukończone ze względu na oczekiwanie na pętli ukończenia                                                                                                                                      |
| wR         | Jest aktywne writer (co oznacza, że nie są ignorowane 6 żądania niewysłanych) bajtów/activewriters                                                                                                                                                   |
| w         | Istnieją nie aktywnego czytniki i zero bajtów są dostępne do odczytu na NIC bajtów/activereaders                                                                                                                                               |


### <a name="steps-to-investigate"></a>Czynności, aby badanie

1. Jak najlepiej upewnij się, że używasz następującego wzorca nawiązania połączenia podczas korzystania z klienta StackExchange.Redis.


        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
        });
    
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }


    Aby uzyskać więcej informacji zobacz [Nawiązywanie połączenia z pamięci podręcznej, za pomocą StackExchange.Redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

2. Upewnij się, czy pamięć podręczną Redis Azure i z aplikacją kliencką są w tym samym regionie platformy Azure. Na przykład możesz może być wyświetlany przekroczenia limitu czasu podczas pamięci podręcznej znajduje się w USA wschodniego, ale klient jest zachód w USA, a żądanie nie zakończyła się w ciągu `synctimeout` interwału lub może być wyświetlany przekroczenia limitu czasu podczas debugowania z komputera lokalnego rozwoju. 

    Zdecydowanie zalecamy ma pamięci podręcznej i na komputerze klienckim, w tym samym regionie Azure. Jeśli masz scenariusza, który zawiera połączenia krzyżowego regionu, należy ustawić `synctimeout` interwał na wartość większą niż domyślny interwał 1000 ms, dołączając `synctimeout` właściwość w parametrach połączenia. W poniższym przykładzie pokazano fragment ciągu StackExchange.Redis pamięci podręcznej połączenia z `synctimeout` programu 2000 ms.

        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...

3. Upewnij się, możesz korzystać z najnowszych wersji [pakietu StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Występują błędy stale usunięte w kodzie, aby była bardziej rozbudowany do przekroczenia limitu czasu, ważne jest najnowszą wersję.

4. W przypadku żądania powiązane wprowadzenie przez ograniczenia przepustowości na serwerze lub klienta, trwa dłużej zakończyć i spowodować przekroczenia limitu czasu. Aby sprawdzić, czy do limitu czasu jest z powodu przepustowości sieci na serwerze, zobacz [Przekroczono przepustowość po stronie serwera](#server-side-bandwidth-exceeded). Aby sprawdzić, czy do limitu czasu jest z powodu przepustowości sieci klienta, zobacz [Przekroczona przepustowość po stronie klienta](#client-side-bandwidth-exceeded).

6. Są możesz wprowadzenie Procesora powiązane na serwerze lub na komputerze klienckim?
    -   Sprawdzanie, jeśli użytkownik wprowadzenie związane są Procesora na komputerze klienckim, co może spowodować żądanie tak, aby nie zostanie przetworzony w `synctimeout` interwał, powodując przekroczenie limitu czasu. Przenoszenie o większym rozmiarze klienta lub dystrybucji obciążenia może pomóc do tego. 
    -   Sprawdzić, czy występują Procesora związana na serwerze monitorowania `CPU` [metryki wydajności pamięci podręcznej](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Żądania wchodzące, gdy Redis jest powiązana Procesora mogą powodować te żądania do przekroczenia limitu czasu. Aby rozwiązać ten rozłożenie obciążenia w wielu odłamki w pamięci podręcznej premium lub uaktualnienie do większe lub warstwa cennik. Aby uzyskać więcej informacji zobacz [Przekroczona przepustowość po stronie serwera](#server-side-bandwidth-exceeded).

7. Czy istnieją polecenia trwa długo przetwarzania na serwerze? Długotrwałe uruchamianie polecenia, których trwa długo na serwerze redis mogą powodować przekroczenia limitu czasu. Oto niektóre przykłady poleceń długotrwałych `mget` z dużej liczby klawiszy, `keys *` lub błędnie napisane lua skryptów. Możesz nawiązać połączenie przy użyciu klienta redis polecenie wystąpienia Azure Redis w pamięci podręcznej lub za pomocą [Konsoli Redis](cache-configure.md#redis-console) i uruchom polecenie [SlowLog](http://redis.io/commands/slowlog) , aby sprawdzić, czy są żądania trwa dłużej niż oczekiwano. Serwer redis i StackExchange.Redis są zoptymalizowane dla wielu żądań małych zamiast mniejszej liczby dużych żądania. Dzielenie danych na mniejsze fragmenty może poprawić czynności poniżej. 

    Aby uzyskać informacje dotyczące łączenia się punkt końcowy Azure Redis pamięci podręcznej SSL przy użyciu interfejsu wiersza polecenia redis i stunnel Zobacz wpis w blogu [O ASP.NET stan dostawcy sesji dla Redis wersji Preview](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) . Aby uzyskać więcej informacji zobacz [SlowLog](http://redis.io/commands/slowlog).

8. Duży obciążenie serwera Redis mogą powodować przekroczenia limitu czasu. Można monitorować obciążenie serwera monitorowania `Redis Server Load` [metryki wydajności pamięci podręcznej](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Obciążenie serwera 100 (wartość maksymalna) oznacza, że na serwerze redis był zajęty, bez czasu bezczynności przetwarzania żądania. Aby wyświetlić, jeśli niektórych żądań zajmują wszystkich możliwości serwera, uruchom polecenie SlowLog, zgodnie z opisem w poprzednim akapicie. Aby uzyskać więcej informacji, zobacz [Użycie Procesora wysoka / ładowanie serwera](#high-cpu-usage-server-load).

9. Po stronie klienta, że mógł spowodować blip sieci był innego zdarzenia? Wystąpił zdarzenia, takie jak skalowanie liczbę wystąpień klienta w górę lub w dół, czy wdrażanie nowa wersja klienta lub automatyczne skalowanie jest włączona, należy sprawdzić na komputerze klienckim (sieć web, roli Pracownik lub maszyn wirtualnych Iaas)? W naszym badania znalezionych mają mogą powodować autoscale lub skalowania wzrost spadek łączność sieciowa ruchu wychodzącego może być utracony przez kilka sekund. Kod StackExchange.Redis jest mechanizm do takich wydarzeń i połączy się ponownie. W tym czasie połączenia ponownego żądań w kolejce można limitu czasu.

10. Był żądanie duży poprzedzających kilka wniosków małych Redis pamięci podręcznej, który został przekroczony? Parametr `qs` błędów komunikat informujący, ile żądań zostały wysłane z klienta do serwera, ale jeszcze nie zostały przetworzone odpowiedź. Można zachować rośnie tej wartości, ponieważ StackExchange.Redis korzysta z jednego połączenia TCP i jedną odpowiedź mogą być odczytywane tylko pojedynczo. Mimo że pierwszej operacji został przekroczony, nie zatrzymuje wysyłanych z serwera danych i inne żądania są zablokowane, dopóki nie zostanie to zakończone, powoduje limity czasu. Rozwiązanie polega na zminimalizować prawdopodobieństwo przekroczenia limitu czasu, zapewnia, że pamięć podręczną jest wystarczająco duży z pracą i dzielenie duże wartości na mniejsze fragmenty. Innym rozwiązaniem możliwe jest za pomocą puli `ConnectionMultiplexer` obiektów w klienta, a następnie wybierz pozycję najmniej załadowanego `ConnectionMultiplexer` podczas wysyłania nowe wezwanie. To należy zapobiec pojedynczy przekroczenia limitu czasu powodujące inne żądania również przekroczenia limitu czasu.

11. Jeśli korzystasz z `RedisSessionStateprovider`, upewnij się, ponów próbę przekroczenia limitu czasu jest ustawiona prawidłowo. `retrytimeoutInMilliseconds`powinna być większa niż `operationTimeoutinMilliseonds`, w przeciwnym razie wystąpi bez ponawiania. W poniższym przykładzie `retrytimeoutInMilliseconds` jest ustawiona na 3000. Aby uzyskać więcej informacji zobacz [Dostawcy stan sesji programu ASP.NET Azure Redis w pamięci podręcznej](cache-aspnet-session-state-provider.md) i [jak używać parametry konfiguracji dostawcy stan sesji i dostawcy pamięci podręcznej danych wyjściowych](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).


    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


12. Sprawdź użycie pamięci na serwerze Azure Redis w pamięci podręcznej, [Monitorowanie](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` i `Used Memory`. W przypadku zasad eksmisji zwiększany w miejscu, zostanie uruchomiony Redis eksmitowania klawiszy, kiedy `Used_Memory` osiągnie rozmiar pamięci podręcznej. Najlepiej, jeśli `Used Memory RSS` powinny być tylko nieco wyżej niż `Used memory`. Różnica duże oznacza, że istnieje fragmentacji pamięci (wewnętrznego lub zewnętrznego. Gdy `Used Memory RSS` jest mniejsza niż `Used Memory`, oznacza to część pamięci podręcznej została zapisana przez system operacyjny. W takim przypadku można się spodziewać pewne istotne opóźnienia. Ponieważ Redis nie ma kontrolę nad jak alokacje są mapowane na stron pamięci wysokiej `Used Memory RSS` jest zazwyczaj wynik kolekcji użycie pamięci. Gdy Redis zwolnienia pamięci, pamięci znajduje się ponownie do przydzielania i program przydzielania mogą być lub może nie być pamięć powrót do systemu. Czasami może występować rozbieżności między `Used Memory` wartość i pamięci zużycie zgłoszone przez system operacyjny. Może być ze względu na to pamięci został użyty i opublikowany przez Redis, ale nie przyznał Wstecz w systemie. Aby rozwiązać problemy z pamięci można wykonywać następujące czynności.
    -   Uaktualnianie do rozmiar pamięci podręcznej, tak, aby nie używasz sprzętu ograniczenia ilości pamięci w systemie.
    -   Ustawianie czas wygaśnięcia na klucze, tak aby starsze wartości są usunięty z wyprzedzeniem.
    -   Monitorowanie `used_memory_rss` pamięci podręcznej metryki. Gdy ta wartość zbliża się rozmiar pamięci podręcznej, najprawdopodobniej będzie rozpocząć widzą występują problemy z wydajnością. Rozłożenie danych w wielu odłamki, jeśli są przy użyciu pamięci podręcznej premium lub uaktualnienie do rozmiar pamięci podręcznej.

    Aby uzyskać więcej informacji zobacz [Ciśnienia pamięci na serwerze](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Dodatkowe informacje

-   [Jakie oferująca Redis pamięci podręcznej i rozmiar należy używać?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
-   [Jak testu wydajności i testowanie wydajności Moje pamięci podręcznej?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
-   [Jak można uruchamiać polecenia Redis?](cache-faq.md#how-can-i-run-redis-commands)
-   [Jak można monitorować Azure Redis w pamięci podręcznej](cache-how-to-monitor.md)