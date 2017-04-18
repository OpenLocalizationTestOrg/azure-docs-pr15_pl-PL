<properties
    pageTitle="Przechowywanie Azure Premium: Projektowanie zapewnia wydajność | Microsoft Azure"
    description="Projektowanie przy użyciu magazynu Premium Azure aplikacji wysokiej wydajności. Magazyn Premium oferuje dysku wysokiej wydajności i małych opóźnień obsługę obciążenia I-O-intensywną uruchomione na maszyn wirtualnych Azure."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>

# <a name="azure-premium-storage-design-for-high-performance"></a>Przechowywanie Azure Premium: Projekt wysokiej wydajności

## <a name="overview"></a>Omówienie  
Ten artykuł zawiera wskazówki dotyczące tworzenia aplikacji wysokiej wydajności przy użyciu magazynu Premium Azure. Można skorzystać z instrukcji podanych w tym dokumencie, w połączeniu z wydajnością najważniejsze wskazówki dotyczące technologii używanych przez aplikację. Aby przedstawić wytyczne, użyliśmy program SQL Server uruchomiony ilość miejsca do magazynowania Premium, na przykład w tym dokumencie.

Podczas wydajności scenariusze warstwy miejsca do magazynowania w tym artykule można rozwiązać, należy zoptymalizować warstwy aplikacji. Na przykład jeśli przechowujesz farmy programu SharePoint ilość miejsca do magazynowania Premium Azure umożliwia przykłady programu SQL Server, z tego artykułu zoptymalizować serwer bazy danych. Ponadto można zoptymalizować, serwer sieci Web i serwer aplikacji do większości wydajność farmie programu SharePoint.

Ten artykuł pomoże odpowiedzi po często zadawane pytania dotyczące optymalizowania wydajności aplikacji Azure Premium ilość miejsca do magazynowania,

-   Jak do pomiaru wydajności aplikacji?  
-   Dlaczego użytkownik nie widzą oczekiwanych wysokiej wydajności?  
-   Jakie czynniki mają wpływ na wydajność aplikacji ilość miejsca do magazynowania Premium?  
-   Jak następujących czynników wpływać wydajność aplikacji ilość miejsca do magazynowania Premium  
-   Jak należy zoptymalizować dla operacji i/o na SEKUNDĘ, przepustowość i opóźnienie  

Tych wskazówek zostały zamieszczone specjalnie dla magazynu Premium, ponieważ obciążenie pracą, uruchomiony ilość miejsca do magazynowania Premium jest wysoce wydajności wielkość liter. Przykłady zostały zamieszczone odpowiednio. Można również zastosować niektóre z tych wskazówek do aplikacje na IaaS maszyny wirtualne dyski standardowego magazynu.

Zanim zaczniesz, jeśli jesteś nowym użytkownikiem magazynowania Premium, zapoznać się z tematem [magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla obciążenia maszyn wirtualnych Azure](storage-premium-storage.md) artykuł i [skalowalność magazynowania Premium Azure i cele](storage-scalability-targets.md#premium-storage-accounts).

## <a name="application-performance-indicators"></a>Wskaźniki wydajności aplikacji  
Możemy ocenić, czy aplikacja działa również lub nie używa wydajności, które wskaźników, takich jak szybkość aplikacji przetwarza żądanie użytkownika ilości danych przetwarzania aplikacji na żądanie, ile żądania jest przetwarzanie aplikacji w danym okresie czasu, ile czasu użytkownik musi czekać na odpowiedzi po przesłaniu ich żądanie. Terminy techniczne dla tych wskaźników wydajności są operacji i/o na SEKUNDĘ przepustowość lub przepustowość i opóźnienie.

W tej sekcji omówimy typowych wskaźniki wydajności w kontekście Premium miejsca do magazynowania. W sekcji następujące wymagania aplikacji zbieranie, dowiesz się, jak zmierzyć te wskaźniki wydajności aplikacji. W dalszej części optymalizowania wydajności aplikacji poznasz czynników wpływających na tych wskaźników wydajności i zalecenia, aby zoptymalizować pod.

## <a name="iops"></a>OPERACJI I/O NA SEKUNDĘ  
Operacji i/o na SEKUNDĘ jest liczbą żądań aplikacji wysyła na dyskach magazynu w jednej sekundy. Operacja wejścia i wyjścia może odczytać lub zapisać, kolejno lub losowo. OLTP aplikacji, takich jak witryny sieci Web Sklep internetowy muszą natychmiast przetwarzania wezwań na wielu użytkowników jednocześnie. Żądania użytkownika są wstawianie i aktualizowanie transakcji bazy danych, które aplikacja przetwarzać szybko. Dlatego aplikacji OLTP wymagają bardzo wysoki operacji i/o na SEKUNDĘ. Takie aplikacje obsługują miliony żądania Jo małych i losowe. Jeśli masz aplikację pakietu, należy zaprojektować infrastruktury aplikacji, aby zoptymalizować pod kątem operacji i/o na SEKUNDĘ. W dalszej części, *Optymalizowania wydajności aplikacji*omówimy szczegółowo wszystkie czynniki, które należy wziąć pod uwagę uzyskanie wysoka operacji i/o na SEKUNDĘ.

Po można dołączyć dysku magazynu premium usługi wysoka skali Głosowa, Azure przepisy dla Ciebie gwarantowanej liczbę operacji i/o na SEKUNDĘ według specyfikacji dysku. Na przykład na dysku P30 przepisy 5000 operacji i/o na SEKUNDĘ. Każda skala duży rozmiar pamięci Wirtualnej ma również określonych limit operacji i/o na SEKUNDĘ, która może kontynuować działanie. Na przykład standardowy maszyny GS5 ma 80 000 operacji i/o na SEKUNDĘ ograniczyć.

## <a name="throughput"></a>Przepustowość  
Przepustowość lub przepustowość jest ilości danych, aplikacja wysyłania do dysków przestrzeni dyskowej w określonym interwale. Jeśli aplikacja operacji wejście i wyjście z dużych rozmiarów jednostki Jo, wymaga wysokiej wydajności. Zwykle aplikacje magazynu danych do wykonania skanowania intensywnie operacje dostępu dużych części danych w czasie i często operacji zbiorczych. Innymi słowy takie aplikacje wymagają wyższej przepustowości. Jeśli masz aplikację pakietu należy zaprojektować swoją infrastrukturę w celu zoptymalizowania przepustowości. W następnej sekcji omówimy szczegółowo czynniki należy dostosować, aby osiągnąć ten cel.

Po dołączeniu dysku magazynu premium wysoka skali Głosowa, Azure przepisy przepustowość zgodnie z tej specyfikacji dysku. Na przykład na dysku P30 przepisy 200 MB na dysku drugi przepustowość. Każda skala duży rozmiar pamięci Wirtualnej ma także jako określonego ograniczenia przepustowości, który może kontynuować działanie. Na przykład standardowy GS5 maszyn wirtualnych ma maksymalnej przepustowości 2000 MB na sekundę.

Istnieje relacja między przepustowość i operacji i/o na SEKUNDĘ jak pokazano w poniższej formule.

![](media/storage-premium-storage-performance/image1.png)

W związku z tym należy ustalić optymalną wydajność operacji i/o na SEKUNDĘ wartości i wymagane przez aplikację. Podczas próby zoptymalizować jedną drugi otrzymuje dotyczy także. W dalszej części *Optymalizowania wydajności aplikacji*będzie omówimy w szczegółowe informacje dotyczące optymalizowania operacji i/o na SEKUNDĘ i przepustowość.

## <a name="latency"></a>Czas oczekiwania  
Opóźnienie jest czas aplikacji do odbierania pojedynczego żądania, wyślij go do dysków miejsca do magazynowania i wysyłania odpowiedzi do klienta. Jest to krytyczne miara wydajności aplikacji oprócz operacji i/o na SEKUNDĘ i przepustowość. Opóźnienie dysku miejsca do magazynowania premium jest czas potrzebny na pobieranie informacji na żądanie i przekazuje go aplikacji. Magazyn Premium zapewnia spójne opóźnienia niski. Po włączeniu buforowanie na dyskach magazynu premium hosta tylko do odczytu można uzyskać wiele mniejsze opóźnienia odczytu. Będzie omówimy buforowania dysku bardziej szczegółowo w dalszej części na *Optymalizowania wydajności aplikacji*.

Kiedy ma być zoptymalizowany aplikacji, aby uzyskać wyższy operacji i/o na SEKUNDĘ i przepustowość, wpłynie to opóźnienie aplikacji. Po Dostosowywanie wydajności aplikacji, zawsze ocenia opóźnienie aplikacji, aby uniknąć zachowania nieoczekiwane długim czasem oczekiwania.

## <a name="gather-application-performance-requirements"></a>Gromadzenie wymagania aplikacji  
Pierwszym krokiem w projektowaniu wysokiej wydajności aplikacje ilość miejsca do magazynowania Azure Premium jest, aby zrozumieć wymagania aplikacji. Po zgromadzeniu wymagania możesz optymalizować aplikacji uzyskanie najbardziej optymalną wydajność.

W poprzedniej sekcji możemy opisano typowe wskaźniki wydajności operacji i/o na SEKUNDĘ przepustowość i opóźnienie. Należy określić, którego z tych wskaźników wydajności są krytyczne aplikacji do przeprowadzania obsługi odpowiedniego użytkownika. Na przykład operacji i/o na SEKUNDĘ wysoka najważniejszych do aplikacji OLTP przetwarzania miliony transakcji w drugiej. Wysokiej wydajności jest krytyczne dla aplikacji Data Warehouse przetwarzania dużych ilości danych w drugiej. Bardzo krótki czas oczekiwania zależy dla aplikacji w czasie rzeczywistym, takich jak wideo na żywo streaming witryn sieci Web.

Następnie zmierzyć maksymalna wymagania aplikacji w jej okresie istnienia. Używanie listy kontrolnej próbki poniżej jako rozpoczęcia. Rekord wymagania maksymalną wydajność podczas normalny, okresy obciążenie pracą Szczyt i po godzinach pracy. Wskazując wymagania dla wszystkich poziomów obciążenie pracą, można określić ogólne wymagania dotyczące działania aplikacji. Na przykład zwykłe obciążenie pracą elektronicznego witryny sieci Web będzie transakcje, który w większości dni w roku. Obciążenie pracą Szczyt witryny sieci Web będzie transakcje, który podczas świąt lub zdarzeń specjalnych sprzedaży. Obciążenie pracą Szczyt jest zazwyczaj doświadczonych jest ograniczony, ale może wymagać aplikacja przeskalować dwa lub więcej razy jego normalnego działania. Dowiedz się, 50 percentyl, percentyl 90 i wymagania dotyczące percentyl 99. Dzięki temu odfiltrować wszystkie wartości odstających w wymagania dotyczące wydajności i z biznesową można skupić się na optymalizowanie pod kątem właściwych wartości.

**Lista kontrolna wymagań wydajności aplikacji**

| **Wymagania dotyczące wydajności** | **50 percentyl** | **Percentyl 90** | **Percentyl 99** |
|---|---|---|---|
| Maksymalna liczba. Transakcje na sekundę | | | |
| Operacje odczytu %            | | | |
| Operacje zapisu %           | | | |
| Operacje przekształcania przypadkowych pomysłów %          | | | |
| Kolejne operacje %      | | | |
| Rozmiar żądania ei              | | | |
| Średnia produktywność           | | | |
| Maksymalna liczba. Przepustowość              | | | |
| Min. Czas oczekiwania                 | | | |
| Średni czas oczekiwania              | | | |
| Maksymalna liczba. PROCESOR                     | | | |
| Procesor średnia                  | | | |
| Maksymalna liczba. Pamięci                  | | | |
| Średnia pamięci               | | | |
| Głębokość kolejki                  | | | |

>**Ważna Uwaga:**  
>Należy rozważyć skalowania liczby te są oparte na oczekiwany przyszłego zwiększenia ilości aplikacji. Jest dobrym pomysłem jest planowanie wzrostu wyprzedzeniem, ponieważ może być trudniejsze infrastruktury dotyczące zwiększania wydajności później zmienić.

Jeśli masz istniejącą aplikację i chcesz przenieść do magazynowania Premium, należy najpierw utworzyć listę kontrolną powyżej istniejącej aplikacji. Następnie utwórz prototypu aplikacji ilość miejsca do magazynowania Premium i projektowania aplikacji według wytycznych opisanych w dalszej części tego dokumentu *Optymalizowania wydajności aplikacji* . W następnej sekcji opisano narzędzia, które umożliwiają uzyskanie pomiary wydajności.

Tworzenie listy kontrolnej podobne do istniejącej aplikacji dla prototypu. Za pomocą narzędzi Benchmarking można symulować obciążenie pracą i pomiaru wydajności na pasku aplikacji prototypu. Na [Benchmarking](#benchmarking) , aby dowiedzieć się więcej, zobacz sekcję. Wykonując, dzięki czemu można określić, czy miejsca do magazynowania Premium można dostosowania lub przekroczenia wymagań dotyczących wydajności aplikacji. Następnie można zaimplementować tych samych wskazówek dla aplikacji produkcji.

### <a name="counters-to-measure-application-performance-requirements"></a>Liczniki do pomiaru wymagania aplikacji  
Najlepszym sposobem do pomiaru wydajności wymagania dotyczące aplikacji, jest za pomocą narzędzi monitorowania wydajności dostarczony przez system operacyjny serwera. Za pomocą Monitora wydajności dla systemu Windows i iostat Linux. Te narzędzia Przechwytywanie liczniki odpowiadające każdego środka opisano w sekcji powyżej. Wartości tych liczników musi przechwytywanie, gdy aplikacja jest uruchomiona, jej normalny, obciążenia Szczyt i po godzinach pracy.

Liczniki monitora są dostępne dla procesora, pamięci i, każdego dysku logicznego i dysk fizyczny serwera. Jeśli używasz dysków w magazynie premium z maszyny liczników dysków fizycznych są dla każdego dysku miejsca do magazynowania premium, a liczniki dysków logicznych są dla każdego woluminu tworzone na dyskach miejsca do magazynowania premium. Musisz przechwycić wartości dyski obsługujące obciążenie pracą z aplikacji. W przypadku mapowania jeden-do-jednego między logicznych i fizycznych dysków może dotyczyć liczników dysków fizycznych; w przeciwnym razie dotyczą liczniki dysków logicznych. W systemie Linux polecenie iostat generuje raport wykorzystania Procesora i dysku. Raport wykorzystania dysku zawiera statystyki urządzenia fizycznego lub partycją. Jeśli masz serwera bazy danych z jej danych i dziennika na oddzielnych dyskach zbieranie danych dla obu dysków. Poniższej tabeli opisano liczniki dysków, procesora i pamięci:

| Licznik | Opis | Monitora | Iostat |
|---|---|---|---|
| **Operacji i/o na SEKUNDĘ lub transakcje na sekundę** | Liczba żądań We/Wy wydawane dla dysków miejsca do magazynowania na sekundę. | Odczyt z dysku/s <br> Zapisy dysku/s | tps <br> r-s <br> w/s |
| **Odczyt dysku i zapis** | % Odczytu i zapisu operacji wykonywanych na dysku. | Czas odczytu dysku (%) <br> Czas zapisu dysku (%) | r-s <br> w/s |
| **Przepustowość** | Ilość danych odczytu lub zapisu na dysku na sekundę. | Dysk Bajty odczytane/s <br> Bajty zapisu dysku/s | kB_read/s <br> kB_wrtn/s |
| **Czas oczekiwania** | Całkowity czas wykonać żądania Jo dysku. | Średnia dysku s/Odczyt <br> Średnia dysku w s/Zapis | poczekać na <br> svctm |
| **Rozmiar ei** | Rozmiar we/wy żąda problemów na dyskach miejsca do magazynowania. | Bajtów średnia dysku/Odczyt <br> Średnia dysku bajtów/zapisu | avgrq sz |
| **Głębokość kolejki** | Liczba zaległych we/wy żądań oczekiwania do odczytu formularza lub zapisane na dysku miejsca do magazynowania. | Bieżąca długość kolejki dysku | avgqu sz |
| **Maksymalna liczba. Pamięci** | Ilość pamięci wymagane do uruchamiania aplikacji sprawniej | % Zadeklarowane bajty w użyciu | Za pomocą vmstat |
| **Maksymalna liczba. PROCESOR** | Kwota Procesora wymagane do uruchamiania aplikacji sprawniej | % Czasu procesora | Narzędzie % |

Dowiedz się więcej o [iostat](http://linuxcommand.org/man_pages/iostat1.html) i [monitora](https://msdn.microsoft.com/library/aa645516.aspx).


## <a name="optimizing-application-performance"></a>Optymalizacja wydajności aplikacji  
Główne czynniki, które wpływają na wydajność aplikacji uruchomionych Premium miejsca do magazynowania są natury z Jo żądania, rozmiar pamięci Wirtualnej, rozmiar dysku, liczbę dysków, buforowanie dysku, Multithreading i głębokość kolejki. Możesz sterować niektóre z tych czynników za pomocą pokręteł systemu. Większość aplikacji może nie być możesz opcję, aby zmienić rozmiar Jo głębokość kolejki bezpośrednio. Na przykład jeśli korzystasz z programu SQL Server, nie można wybrać głębokość rozmiar i kolejki Jo. Program SQL Server wybiera optymalnego Jo rozmiar i kolejki głębokości wartości do większości wydajność. Ważne jest zrozumienie skutków tej czynności dla obu rodzajów czynników wpływających na wydajność aplikacji, aby umożliwia obsługę odpowiednich zasobów do potrzeb wydajności.

W tej sekcji dotyczą listy kontrolnej wymagania dotyczące aplikacji, której został utworzony, do identyfikowania, ile chcesz Optymalizowanie aplikacji. W zależności od której, będzie możliwość określenia, które czynniki z tej sekcji należy dostosować. Obecności skutków tej czynności dla każdego współczynnika wydajności aplikacji, uruchom narzędzia najlepszymi w ustawieniach aplikacji. Zapoznaj się z sekcją [Benchmarking](#Benchmarking) na końcu tego artykułu czynności, aby uruchomić typowych narzędzi najlepszymi w systemach Windows i Linux oraz maszyny wirtualne.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Optymalizacja operacji i/o na SEKUNDĘ przepustowość i opóźnienie rzut oka  
W poniższej tabeli zestawiono wszystkie czynniki wydajności i czynności, aby zoptymalizować operacji i/o na SEKUNDĘ przepustowość i opóźnienie. W sekcjach poniżej Podsumowanie to będzie opisy poszczególnych współczynnik jest o wiele bardziej szczegółowo.

| | **OPERACJI I/O NA SEKUNDĘ** | **Przepustowość** | **Czas oczekiwania** |
|---|---|---|---|
| **Przykładowy scenariusz** | Wymaganie bardzo wysoki transakcji według druga rata aplikacji OLTP przedsiębiorstwa.                                                                                                                                 | Dane na poziomie przedsiębiorstwa składu aplikacji przetwarzania dużych ilości danych. | W aplikacji w czasie rzeczywistym wymagających błyskawicznego odpowiedzi na wezwania na użytkownika, takie jak gier online. |
| Czynniki wydajności  | | | |
| **Rozmiar ei** | Mniejszy rozmiar Jo daje wyższą operacji i/o na SEKUNDĘ.                                                                                                                                                                           | Rozmiar Jo do daje wyższej przepustowości. | |
| **Rozmiar pamięci Wirtualnej** | Za pomocą rozmiar pamięci Wirtualnej, który oferuje operacji i/o na SEKUNDĘ większa niż z wymaganiami aplikacji. Zobacz rozmiarów maszyn wirtualnych i ich limity operacji i/o na SEKUNDĘ. | Rozmiar pamięci Wirtualnej za pomocą limit przepustowości większa niż z wymaganiami aplikacji. Zobacz rozmiarów maszyn wirtualnych i ich limity przepustowość. | Za pomocą rozmiar pamięci Wirtualnej, że ofert skalowanie limity większa niż z wymaganiami aplikacji. Zobacz rozmiarów maszyn wirtualnych oraz ich limity tutaj. |
| **Rozmiar dysku** | Za pomocą rozmiar dysku, który oferuje operacji i/o na SEKUNDĘ większa niż z wymaganiami aplikacji. Zobacz rozmiarów dysków i ich limity operacji i/o na SEKUNDĘ. | Rozmiar dysku za pomocą limit przepustowości większa niż z wymaganiami aplikacji. Zobacz rozmiarów dysków i ich limity przepustowość. | Za pomocą rozmiar dysku, że ofert skalowanie limity większa niż z wymaganiami aplikacji. Zobacz rozmiarów dysków oraz ich limity tutaj. |
| **Maszyn wirtualnych i limitów skali na dysku** | Limit operacji i/o na SEKUNDĘ wybrany rozmiar maszyn wirtualnych powinna być większa niż całkowita operacji i/o na SEKUNDĘ prowadzone przez premium miejsca do magazynowania dysków dołączonych do niego. | Limit przepustowości wybrany rozmiar maszyn wirtualnych powinna być większa niż ogólnej przepustowości prowadzone przez premium miejsca do magazynowania dysków dołączonych do niego. | Skala wielkości wybrany rozmiar maszyn wirtualnych musi być większa niż całkowita skali limity dysków miejsca do magazynowania dołączonym premium. |
| **Buforowanie dysku** | Włącz tylko do odczytu w pamięci podręcznej na dyskach miejsca do magazynowania premium z dużą ilością operacje odczytu, aby uzyskać wyższy odczytu operacji i/o na SEKUNDĘ. | | Włącz tylko do odczytu w pamięci podręcznej na dyskach miejsca do magazynowania premium z gotowe intensywnie operacje, aby uzyskać bardzo niskie odczytu opóźnienia. |
| **Rozkładanie** | Przy użyciu wielu dysków i paskowych je uzyskać Scalonej wyższy limit operacji i/o na SEKUNDĘ i przepustowość. Należy zauważyć, że Scalonej limit na maszyn wirtualnych powinna być większa niż wartości graniczne Scalonej załączonych premium dysków. | |
| **Pasek rozmiar** | Mniejszy rozmiar pasek deseń losowe małych Jo widoczne w aplikacji OLTP. Przykład użyj rozmiaru pasek 64 KB dla aplikacji OLTP serwera SQL. | Rozmiar pasek deseń sekwencyjne dużych Jo widoczne w aplikacjach magazynu danych. Przykład użyj rozmiaru pasek 256KB dla aplikacji magazynu danych programu SQL Server. | |
| **Wielowątkowość** | Aby przekazać większej liczby żądań do magazynowania Premium, który będzie powodowało wyższą operacji i/o na SEKUNDĘ i przepustowość wielowątkowość do użycia. Na przykład w programie SQL Server wartość Wysoki MAXDOP przydzielić więcej procesorów do programu SQL Server. | |
| **Głębokość kolejki** | Większe głębokość kolejki daje wyższą operacji i/o na SEKUNDĘ. | Większe głębokość kolejki daje wyższej przepustowości. | Mniejsze głębokość kolejki powoduje opóźnienia dolnym. |

## <a name="nature-of-io-requests"></a>Rodzaj żądania ei  
Żądanie Jo jest jednostka operacji wejścia i wyjścia, która będzie wykonywania aplikacji. Identyfikowanie rodzaj Jo żądania, losowe lub kolejne, odczytu lub zapisu, mały lub duży, może pomóc w określeniu wymagania aplikacji. Ważne zrozumieć istotę żądania Jo, aby wprowadzić prawidłowych decyzji podczas projektowania infrastruktury aplikacji jest.

Rozmiar Jo jest jednym z ważniejszych czynników. Rozmiar Jo jest rozmiarem żądania operacji wejścia i wyjścia wygenerowanych przez aplikację. Rozmiar Jo ma wpływ na wydajność, zwłaszcza w przypadku operacji i/o na SEKUNDĘ i przepustowość aplikacja jest możliwość uzyskania. W poniższej formule pokazano relacje między operacji i/o na SEKUNDĘ, rozmiar Jo i przepustowość i przepustowość.  
    ![](media/storage-premium-storage-performance/image1.png)

Niektóre aplikacje pozwalają zmieniać ich rozmiaru Jo podczas niektóre aplikacje nie. Na przykład programu SQL Server określa optymalny rozmiar Jo samej, a nie umożliwia użytkownikom z dowolnym pokręteł, aby go zmienić. Z drugiej strony, Oracle zawiera parametr, o nazwie [DB\_ZABLOKOWAĆ\_rozmiar](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) za pomocą którego można skonfigurować rozmiaru żądania We/Wy bazy danych.

Jeśli korzystasz z aplikacji, których nie zezwala na zmianę rozmiaru Jo, aby zoptymalizować kluczowego wskaźnika wydajności, który najlepiej pasuje do aplikacji za pomocą wytyczne w tym artykule. Na przykład

-   Aplikacja OLTP generuje miliony żądania Jo małych i losowe. Do obsługi tych typów żądania Jo, należy zaprojektować infrastrukturę aplikacji, aby uzyskać wyższy operacji i/o na SEKUNDĘ.  
-   Źródła danych składu aplikacji generuje duże i kolejne żądania Jo. Do obsługi tych typów żądania Jo, należy zaprojektować infrastrukturę aplikacji, aby uzyskać wyższy przepustowości lub przepustowość.

Jeśli korzystasz z aplikacji, dzięki czemu można zmienić rozmiar Jo, za pomocą tego zasada rozmiaru Jo oprócz inne wytyczne dotyczące wydajności,

-   Mniejszy rozmiar Jo, aby uzyskać wyższy operacji i/o na SEKUNDĘ. Na przykład 8 KB dla aplikacji OLTP.  
-   Rozmiar Jo uzyskanie wyższej przepustowości i przepustowości. Na przykład 1024 KB dla aplikacji magazynu danych.

Oto przykład na obliczenie operacji i/o na SEKUNDĘ i przepustowość i przepustowość aplikacji. Należy rozważyć, czy aplikacji przy użyciu dysku P30. Maksimum, które można uzyskać operacji i/o na SEKUNDĘ i przepustowość i przepustowość dysku P30 jest 5000 operacji i/o na SEKUNDĘ i 200 MB na sekundę odpowiednio. Teraz aplikacja wymaga maksymalna operacji i/o na SEKUNDĘ z dysku P30, możesz użyć mniejszego rozmiaru Jo podobnie jak 8 KB przepustowość wynikowy będzie mógł uzyskać jest 40 MB na sekundę. Jednak jeśli aplikacja wymaga maksymalna przepustowość i przepustowość z dysku P30 i używanie większy rozmiar Jo, takich jak 1024 KB, wyniku operacji i/o na SEKUNDĘ jest mniejsza i 200 operacji i/o na SEKUNDĘ. Dlatego dostosować rozmiar Jo tak, aby go spełnia wymagania operacji i/o na SEKUNDĘ i przepustowość i przepustowość obu aplikacji. W poniższej tabeli podsumowano różne rozmiary Jo i ich odpowiednich operacji i/o na SEKUNDĘ i przepustowość dysku P30.

| **Wymagania aplikacji** | **Rozmiar wejścia/wyjścia** | **OPERACJI I/O NA SEKUNDĘ** | **Wydajność i przepustowość** |
|-----------------------------|--------------|----------|--------------------------|
| Maksymalna liczba operacji i/o na SEKUNDĘ                    | 8 KB         | 5000    | 40 MB na sekundę         |
| Przepustowość Max              | 1024 KB      | 200      | 200 MB na sekundę        |
| Maksymalna liczba przepustowość + wysoka operacji i/o na SEKUNDĘ  | 64 KB        | 3 200    | 200 MB na sekundę        |
| Maksymalna liczba operacji i/o na SEKUNDĘ + wysokiej wydajności  | 32 KB        | 5000    | 160 MB na sekundę        |

Aby uzyskać operacji i/o na SEKUNDĘ i przepustowość jest większa niż wartość maksymalna dysku magazynu premium pojedynczy, użyj wielu dyskach premium rozłożone razem. Na przykład pasek dwa P30 dyski uzyskać Scalonej operacji i/o na SEKUNDĘ z 10 000 operacji i/o na SEKUNDĘ lub Scalonej przepustowości 400 MB na sekundę. Zgodnie z opisem w następnej sekcji, należy użyć rozmiar pamięci Wirtualnej, która obsługuje połączonych dysku operacji i/o na SEKUNDĘ i przepustowość.

>**Uwaga:**  
>Zwiększenie operacji i/o na SEKUNDĘ lub drugi także zwiększa produktywność, upewnij się, że nie trafisz przepustowość lub ograniczenia operacji i/o na SEKUNDĘ dysku lub maszyn wirtualnych podczas jednego z rosnącymi.

Obecności efekty rozmiar Jo na wydajność aplikacji, można uruchamiać narzędzia najlepszymi na maszyn wirtualnych i dyski. Tworzenie wielu testów i umożliwia wyświetlanie wpływu inny rozmiar Jo dla każdej serii. Zapoznaj się z sekcją [Benchmarking](#Benchmarking) na końcu tego artykułu, aby uzyskać więcej informacji.

## <a name="high-scale-vm-sizes"></a>Skala wysoka maszyn wirtualnych rozmiarów  
Po rozpoczęciu projektowania aplikacji, jedną z pierwszej co należy zrobić to wybierz maszyn wirtualnych do obsługi aplikacji. Magazyn Premium zawiera rozmiary wysoka skali maszyn wirtualnych, które można uruchamiać zastosowań wymagających wyższą dodatku obliczeń i dysk lokalny wysoka wydajność wejścia/wyjścia. Te maszyny wirtualne zapewniają szybsze procesory, wyższy stosunek pamięci do podstawowych i Solid-State sterują (SSD) na dysk lokalny. Przykłady wysoka skali maszyny wirtualne pomocniczych magazynowania Premium serii DS, DSv2 i GS maszyny wirtualne.

Duży skali maszyny wirtualne są dostępne w różnych rozmiarach z inną liczbę rdzenie Procesora, pamięci, systemu operacyjnego i rozmiar dysku tymczasowym. Każdy rozmiar pamięci Wirtualnej też ma maksymalną liczbę dysków danych, które można dołączać do maszyn wirtualnych. Dlatego wybrany rozmiar pamięci Wirtualnej zauważą, ile przetwarzania, pamięci i pojemność jest dostępna dla aplikacji. To także zmianę obliczeń i kosztów miejsca do magazynowania. Poniżej przedstawiono specyfikacji najdłuższego maszyn wirtualnych w serii DS, serii DSv2 i serii GS:

| Rozmiar pamięci Wirtualnej | Rdzenie Procesora | Pamięci | Rozmiary dysku maszyn wirtualnych | Maksymalna liczba. dyski danych | Rozmiar pamięci podręcznej | OPERACJI I/O NA SEKUNDĘ | Limity pamięci podręcznej Jo przepustowości |
|---|---|---|---|---|---|---|---|
| Standard_DS14 | 16 | 112 GB | OS = 1023 GB <br> Lokalne SSD = 224 GB | 32 | 576 GB | 50 000 OPERACJI I/O NA SEKUNDĘ <br> 512 MB na sekundę | 4 000 operacji i/o na SEKUNDĘ i 33 MB na sekundę |
| Standard_GS5 | 32 | 448 GB | OS = 1023 GB <br> Lokalne SSD = 896 GB | 64 | 4224 GB | 80 000 OPERACJI I/O NA SEKUNDĘ <br> 2000 MB na sekundę | 5000 operacji i/o na SEKUNDĘ i 50 MB na sekundę |

Aby wyświetlić pełną listę wszystkich dostępnych rozmiarów maszyn wirtualnych Azure, zapoznaj się z [rozmiarów maszyn wirtualnych systemu Windows](../virtual-machines/virtual-machines-windows-sizes.md) i [Linux oraz maszyn wirtualnych rozmiary](../virtual-machines/virtual-machines-linux-sizes.md). Wybierz rozmiar pamięci Wirtualnej, które spełniają i skalowanie do własnych potrzeb wydajności żądaną aplikację. Oprócz tego brać pod uwagę po ważne zagadnienia, wybierając rozmiarów maszyn wirtualnych.


*Limity skali*  
Maksymalna wartość wielkości operacji i/o na SEKUNDĘ na maszyn wirtualnych i na dysku są różne i niezależnie od siebie. Upewnij się, że aplikacja bo operacji i/o na SEKUNDĘ w granicach maszyn wirtualnych, a także dyski premium podłączone do niego. W przeciwnym razie wydajnością będą wrażenia ograniczania.

Na przykład załóżmy, że wymagania aplikacji jest maksymalnie 4 000 operacji i/o na SEKUNDĘ. Aby osiągnąć ten cel, obsługi administracyjnej dysku P30 maszyny DS1. Dysk P30 zapewnia do 5000 operacji i/o na SEKUNDĘ. Jednak maszyn wirtualnych DS1 jest ograniczone do 3,200 operacji i/o na SEKUNDĘ. W związku z tym wydajność aplikacji będzie ograniczone przez limit maszyn wirtualnych w operacji i/o na SEKUNDĘ 3,200 i będą ograniczone wydajności. Aby zapobiec tej sytuacji, wybierz rozmiar maszyn wirtualnych i dyskiem, że oba spełnia wymagania dotyczące aplikacji.

*Koszt pracy*  
W większości przypadków jest możliwe, że usługi całkowity koszt operacji przy użyciu magazynu Premium jest niższy niż za pomocą standardowego magazynu.

Na przykład można rozważyć aplikacji wymagających 16 000 operacji i/o na SEKUNDĘ. Aby osiągnąć ten wydajności, konieczne będzie Standard\_D14 Azure IaaS Głosowa, który może zapewnić maksymalną operacji i/o na SEKUNDĘ: 16000 przy użyciu 32 dysków 1TB miejsca do magazynowania standardowy. Każdy dysk standardowego magazynu 1TB można uzyskać maksymalnie 500 operacji i/o na SEKUNDĘ. Szacowany koszt ten maszyn wirtualnych miesięcznie będzie $1,570. Koszt miesięczny 32 dysków standardowego magazynu będzie $1,638. Szacowany całkowity koszt miesięczny będzie $3,208.

Jednak jeśli obsługiwany tej samej aplikacji ilość miejsca do magazynowania Premium, konieczne będzie mniejszy rozmiar maszyn wirtualnych i mniej miejsca do magazynowania dysków premium, co powoduje zmniejszenie całkowity koszt. Standardowe\_DS13 maszyn wirtualnych można wymaganie 16 000 operacji i/o na SEKUNDĘ za pomocą czterech dysków P30. Maszyn wirtualnych DS13 ma maksymalną operacji i/o na SEKUNDĘ: 25,600 i każdego dysku P30 ma maksymalną operacji i/o na SEKUNDĘ: 5000. Ogólnie mówiąc, tej konfiguracji można uzyskać 5000 x 4 = 20 000 operacji i/o na SEKUNDĘ. Szacowany koszt ten maszyn wirtualnych miesięcznie będzie $1,003. Koszt miesięczny czterech P30 premium miejsca do magazynowania dysków będzie $544.34. Szacowany całkowity koszt miesięczny będzie $1,544.

Poniższej tabeli podsumowano podział kosztów w tym scenariuszu standardowe i przechowywania Premium.

| | **Standardowe** | **Premium** |
|---|---|---|
| **Koszt maszyn wirtualnych na miesiąc** | $1,570.58 (standardowy\_D14)   | $1,003.66 (standardowy\_DS13) |
| **Koszt dysków na miesiąc** | $1,638.40 (32 x 1 TB dyski) | $544.34 (4 dyski x P30) |
| **Całkowity koszt na miesiąc**  | $3,208.98 | $1,544.34 |

*Linux Distros*  

Z miejscem do magazynowania Azure Premium możesz uzyskać taki sam poziom wyników za pośrednictwem SMS systemem Windows i Linux. Firma Microsoft obsługuje wiele podtypy Linux distros i można wyświetlić pełną listę [poniżej](../virtual-machines/virtual-machines-linux-endorsed-distros.md). Należy pamiętać, że różne distros nadaje się lepiej dla różnych typów obciążenia. Zostaną wyświetlone różne poziomy wydajności w zależności od distro, który pracuje z pracą. Testowanie distros Linux z aplikacją i wybierz ten, który najlepiej.


Gdy systemem Linux z nośnikami Premium, Sprawdź najnowsze informacje o wymaganych sterowników zapewnienie wysokiej wydajności.

## <a name="premium-storage-disk-sizes"></a>Rozmiary dysku miejsca do magazynowania Premium  
Azure magazynowania Premium obecnie oferuje trzy rozmiarów dysków. Rozmiar każdego dysku ma limit inną skalę dla operacji i/o na SEKUNDĘ, przepustowości i miejsca do magazynowania. Wybierz rozmiar dysku magazynu Premium w zależności od wymagania aplikacji i skali duży rozmiar pamięci Wirtualnej po prawej stronie. Poniższa tabela zawiera trzy dyski rozmiarów i możliwości.

| **Typ dysku**       | **P 10**           | **P20**           | **P30**           |
|---------------------|-------------------|-------------------|-------------------|
| Rozmiar dysku           | 128 nosek           | 512 nosek           | 1024 nosek (1 TB)   |
| Operacji i/o na SEKUNDĘ na dysku       | 500               | 2300              | 5000              |
| Przepustowość na dysku | 100 MB na sekundę | 150 MB na sekundę | 200 MB na sekundę |

Ile dysków zależy dysku rozmiaru wybrane. Można użyć jednego dysku P30 lub wiele dysków p 10 zgodnie z wymaganiami aplikacji. Uwzględnić uwagi dotyczące konta wymienionych poniżej, kiedy decyzji.

*Limity skali (operacji i/o na SEKUNDĘ i przepustowość)*  
Limity operacji i/o na SEKUNDĘ i przepustowość każdego rozmiaru dysku Premium jest inna i niezależnie od limitów skali maszyn wirtualnych. Upewnij się, że całkowita operacji i/o na SEKUNDĘ i przepustowość z dysków znajduje się w granicach skali wybranego rozmiaru maszyn wirtualnych.

Jeśli na przykład wymagania aplikacji jest więcej niż 250 MB/s przepustowości i maszyny DS4 za pomocą jednego dysku P30. Maszyn wirtualnych DS4 przekazać maksymalnie 256 MB/s przepustowości. Jednak jednego dysku P30 ma limit przepustowości 200 MB na sekundę. W związku z tym aplikacja będzie ograniczone na 200 MB/sec ze względu na limit dysku. Aby rozwiązać ten limit, dodawać więcej niż jedna dyski danych do maszyn wirtualnych.

>**Uwaga:**  
>Obsługiwane przez pamięć podręczną Odczyt nie są objęte dysku operacji i/o na SEKUNDĘ i przepustowości, więc nie podlega limity dysku. Pamięć podręczna ma osobnych operacji i/o na SEKUNDĘ i przepustowość limit na maszyn wirtualnych.
>
>Na przykład początkowo do odczytu i zapisu są 60MB/s i 40MB/s odpowiednio. Czasem pamięci podręcznej warms w górę i służy więcej i nie tylko odczytywane z pamięci podręcznej. Następnie możesz uzyskać wyższy zapisu przepustowości z dysku.

*Liczba dysków*  
Określenie liczby dysków, które mają być według oceny wymagania dotyczące aplikacji. Każdy rozmiar pamięci Wirtualnej również ma limit liczby dysków, które można dołączać do maszyn wirtualnych. Zazwyczaj jest to dwa razy liczby rdzeni. Upewnij się, że rozmiar pamięci Wirtualnej, które możesz wybrać można obsługiwać liczbę wymagane dyski.

Należy pamiętać, że dyski magazynu Premium mają wyższą możliwości wydajności w porównaniu z dysków standardowego magazynu. W związku z tym jeśli aplikacja są migrowane z maszyn wirtualnych IaaS Azure za pomocą standardowego magazynu do magazynu Premium, prawdopodobnie konieczne będzie mniej dysków premium uzyskanie samego lub wyższego wydajności aplikacji.

## <a name="disk-caching"></a>Buforowanie dysku  
Duży maszyny wirtualne skali, które używają magazyn Premium Azure mają wielu technologię buforowania o nazwie BlobCache. BlobCache używa kombinacji pamięci RAM maszyn wirtualnych i lokalnych SSD w pamięci podręcznej. Pamięć podręczną jest dostępna dla dysków trwałych magazynowania Premium i lokalnych dyskach maszyn wirtualnych. Domyślnie to ustawienie pamięci podręcznej ma wartość do odczytu/zapisu dysków systemu operacyjnego i tylko do odczytu na dyskach danych obsługiwanych w magazynie Premium. Z włączoną na dyskach magazynowania Premium pamięcią podręczną dysku, wysoka skali maszyny wirtualne można uzyskać bardzo wysoki poziom wydajności, przekraczające podstawowej wydajności dysku.

>[AZURE.WARNING] Zmiana ustawienia pamięci podręcznej dysku Azure odłączenie i ponownie dołącza dysk docelowy. Jeśli jest to dysk systemu operacyjnego, maszyn wirtualnych jest ponownie. Zatrzymaj wszystkie aplikacje i usługi, które mogą mieć wpływ ten zakłócenia przed zmianą ustawienia pamięci podręcznej dysku.

Aby dowiedzieć się więcej o sposobie działania BlobCache, należy zapoznać się z wewnątrz [Magazyn Premium Azure](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) wpis w blogu.

Należy włączyć pamięci podręcznej w prawo zestawie dysków. Czy należy włączyć buforowanie dysku na dysku premium lub nie zależy od wzorca obciążenie pracą dysku będzie obsługa. Poniższa tabela zawiera domyślne ustawienia pamięci podręcznej w przypadku dysków systemu operacyjnego i danych.

| **Typ dysku** | **Domyślne ustawienia pamięci podręcznej** |
|---|---|
| Dysk systemu operacyjnego | Odczytu i zapisu |
| Dysku danych | Brak |

Poniżej przedstawiono ustawienia pamięci podręcznej dysków zalecany w przypadku dysków danych

| **Ustawienie buforowania na dysku** | **Zalecenie dotyczące kiedy należy używać tego ustawienia** |
|---|---|
| Brak | Konfigurowanie pamięci podręcznej hosta jako Brak tylko do zapisu i dużej zapisu dysków. |
| Tylko do odczytu | Konfigurowanie pamięci podręcznej hosta jako tylko do odczytu, tylko do odczytu i odczytu i zapisu dysków. |
| Odczytu i zapisu | Konfigurowanie pamięci podręcznej hosta jako odczytu i zapisu tylko wtedy, gdy aplikacja poprawnie obsługuje zapisywania zapisujące dane do trwałych dysków w razie potrzeby. |

*Tylko do odczytu*  
Dzięki skonfigurowaniu buforowania na dane z magazynu Premium dysków tylko do odczytu, możesz uzyskać małe opóźnienia odczytu i uzyskać bardzo wysoki operacji i/o na SEKUNDĘ odczytu i przepustowość aplikacji. Jest to ukończenia dwa powody

1.  Odczyt wykonać z pamięci podręcznej, która znajduje się w pamięci maszyn wirtualnych i lokalnych SSD, są szybciej niż odczyt z dysku danych, który jest w magazynie obiektów blob platformy Azure.  
2.  Magazyn Premium nie zlicza odczytywane z pamięci podręcznej na dysku operacji i/o na SEKUNDĘ i przepustowość. W związku z tym aplikacja jest w stanie osiągać wyższą całkowitą operacji i/o na SEKUNDĘ i przepustowość.

*Odczytu i zapisu*  
Dyski OS zawierają domyślnie włączone buforowanie odczytu i zapisu. Ostatnio Dodaliśmy obsługę pamięci podręcznej danych dysków odczytu i zapisu. Jeśli korzystasz z pamięci podręcznej odczytu i zapisu, musisz mieć właściwy sposób na dyskach trwałych podczas zapisywania danych z pamięci podręcznej. Na przykład programu SQL Server obsługuje zapisywania pamięci podręcznej danych na dyskach wygenerowanie samodzielnie. Pamięć podręczna odczytu i zapisu za pomocą aplikacji, która nie obsługuje utrzymuje wymagane dane może prowadzić do utraty danych, jeśli maszyn wirtualnych ulega awarii.

Na przykład można nadawać tych wskazówek program SQL Server uruchomiony ilość miejsca do magazynowania Premium, wykonując poniższe czynności,

1.  Konfigurowanie pamięci podręcznej "Tylko do odczytu" na dyskach miejsca do magazynowania premium hostingu pliki danych.  
    .  Szybka odczytuje z pamięci podręcznej dolnym, że czas kwerendy programu SQL Server, ponieważ stron danych są pobierane z pamięci podręcznej w znacznie szybsze porównaniu bezpośrednio z danych dysków.  
    b.  Serwowania odczyt z pamięci podręcznej, oznacza to, że dodatkowe przepustowość jest dostępna z premium danych dysków. SQL Server można użyć tej dodatkowe przepustowość do pobierania kolejnych stron danych oraz innych działań, takich jak tworzenie kopii zapasowych i przywracanie, partii obciążenia i odbudowywania indeksu.  
2.  Konfigurowanie pamięci podręcznej "Brak" na dyskach magazynowania premium hostingu pliki dziennika.  
    .  Pliki dziennika mają przede wszystkim operacji zapisu dużej. W związku z tym że nie korzystają z pamięci podręcznej tylko do odczytu.

## <a name="disk-striping"></a>Rozkładanie  
Gdy skala wysoki, której maszyn wirtualnych jest dołączony z kilku dysków trwałych magazynu premium, dysków można paski ze sobą, aby agregowanie ich operacji i/o na sekundę, przepustowość i pojemność.

W systemie Windows umożliwia spacje miejsca do magazynowania dysków pasek razem. Należy skonfigurować jedną kolumnę dla każdego dysku w puli. W przeciwnym razie ogólnej wydajności wielkości rozłożone może być mniejsze niż oczekiwane ze względu na rozkład normalny ruchu na dyskach.

Ważne: Za pomocą interfejsu użytkownika Menedżer serwera, można ustawić całkowita liczba kolumn do 8 woluminu rozłożony. Podczas dołączania więcej niż 8 dysków, użyj programu PowerShell, aby utworzyć głośność. Przy użyciu programu PowerShell, można ustawić liczbę kolumn równa liczbie dysków. Na przykład w przypadku 16 dysków w jednym rozłożony; Określ 16 kolumn w parametrze *NumberOfColumns* polecenia cmdlet programu PowerShell *VirtualDisk nowy* .


W systemie Linux Użyj narzędzia MDADM na dyskach pasek razem. Aby uzyskać szczegółowe instrukcje na dyskach rozkładanie na Linux odwołują się do [Skonfigurowania RAID oprogramowania w systemie Linux](../virtual-machines/virtual-machines-linux-configure-raid.md).


*Pasek rozmiar*  
Ważne konfiguracji w rozkładanie jest wielkością pasek. Rozmiar pasek lub rozmiar bloku jest najmniejszą fragmentu dane aplikacji można rozwiązać na woluminu paskowego. Rozmiar pasek, które możesz skonfigurować zależy od typu aplikacji oraz jej wzorzec wezwanie. Jeśli wybierzesz rozmiar pasek problem, może prowadzić do niezgodność Jo, które prowadzą do ograniczone wydajność aplikacji.

Na przykład jeśli żądanie Jo wygenerowane przez aplikację jest większy niż rozmiar pasek dysku, system magazynowania zapisuje granicami jednostki pasek na więcej niż jednym dysku. Gdy nadchodzi czas, aby uzyskać dostęp do tych danych, będą musiały Szukanie wyniku w większej liczbie jednostek pasek wykonać żądania. Skrócenie takie zachowanie może prowadzić do obniżenia znacznych wydajności. Z drugiej strony Jeśli rozmiar żądania Jo jest mniejszy niż rozmiar pasek i jest przypadkowe charakter, żądania Jo może sumować na tym samym dysku przyczyną gardła i ostatecznie obniżeniu wydajności Jo.


W zależności od typu obciążenie pracą aplikacja jest uruchomiona, wybierz pozycję Pasek odpowiedni rozmiar. Przypadkowe małych żądań Jo użyć mniejszego rozmiaru pasek. Dla dużych Jo kolejne żądania stosować rozmiar pasek. Dowiedz się, pasek rozmiar zalecenia dotyczące aplikacji czy będzie używana ilość miejsca do magazynowania Premium. Dla programu SQL Server skonfiguruj pasek rozmiar 64KB dla obciążenia OLTP i 256KB dla obciążenia magazynowania danych. Zobacz [Wydajność najważniejsze wskazówki dotyczące programu SQL Server na maszyny wirtualne Azure](../virtual-machines/virtual-machines-windows-sql-performance.md#disks-and-performance-considerations) , aby dowiedzieć się więcej.


>**Uwaga:**  
>Czy można paskowych razem maksymalnie 32 premium magazynowania dyski na serię Zasadami maszyn wirtualnych i 64 premium przestrzeni dyskowej w serii GS maszyn wirtualnych.

## <a name="multi-threading"></a>Wiele wątków  
Azure opracowano z myślą platformy magazynowania Premium się znacznie równoległe. W związku z tym aplikacją wielowątkowe uzyskuje znacznie większą wydajność niż aplikacja pojedynczym wątku. Wielowątkowe aplikacji zostaje jej zadań między wiele wątków i zwiększanie wydajności jego wykonanie wykorzystując maszyn wirtualnych i dysku zasoby maksymalnie.

Na przykład jeśli aplikacja działa w jednym core maszyn wirtualnych za pomocą dwóch wątków, Procesora przełączać się między dwa wątki uzyskanie efektywności. Gdy jeden wątek oczekuje na dysku Jo do wykonania, Procesora przełączyć się do innego wątku. W ten sposób wątki dwóch można wykonywać więcej niż jednym wątku. Jeśli maszyn wirtualnych zawiera więcej niż jeden, dodatkowo zmniejsza czas działania od każdego core można wykonywać zadania równolegle.

Nie można zmienić sposób gotowych aplikacji wykonuje pojedynczy wątków lub wiele wątków. Na przykład programu SQL Server jest w stanie obsługi wielu procesorów i wielu podstawowych. Jednak programu SQL Server zdecyduje, na jakich warunkach będzie używana jeden lub więcej wątków przetwarzania kwerendy. Jego uruchamianie kwerend i tworzenie indeksów przy użyciu wielu wątków. Kwerendy, która obejmuje dołączania dużych tabel i sortowanie danych przed powrót do użytkownika SQL Server prawdopodobnie Użyj wielu wątków. Jednak użytkownika nie można kontrolować, czy program SQL Server wykonuje zapytanie za pomocą wątku jednego lub wielu wątków.

Istnieją ustawienia konfiguracji, które można zmieniać wpływ na to wiele wątków lub równoległego przetwarzania aplikacji. Na przykład w przypadku programu SQL Server jest maksymalna konfiguracja stopień równoległości. To ustawienie o nazwie MAXDOP, można skonfigurować maksymalna liczba procesorów, które programu SQL Server można używać, gdy równoległe przetwarzanie. MAXDOP można skonfigurować dla poszczególnych kwerend lub operacji indeksu. Jest to korzystne, gdy chcesz saldo zasoby systemu dla aplikacji krytyczne wydajności.

Załóżmy, że aplikację za pomocą programu SQL Server jest wykonywanie dużych kwerend i operacji indeksowania w tym samym czasie. Załóżmy Chcę teraz operacji indeksowania należy performant więcej w porównaniu z dużej kwerendy. W takim przypadku można ustawić wartość MAXDOP operacji indeksowania być większa niż wartość MAXDOP kwerendy. Dzięki temu program SQL Server ma więcej liczby procesorów, które mogą korzystać dla operacji indeksowania w porównaniu z liczby procesorów, które można przypisać do dużego kwerendy. Należy pamiętać, że nie decyduje o liczbie wątków używanego programu SQL Server dla każdej operacji. Możesz sterować maksymalna liczba procesorów jest przeznaczony do wielokrotnego wątków.

Dowiedz się więcej na temat [Stopni równoległości](https://technet.microsoft.com/library/ms188611.aspx) programu SQL Server. Dowiedz się, tych ustawień, które wpływają na wiele wątków w aplikacji i ich konfiguracji, aby zoptymalizować.

## <a name="queue-depth"></a>Głębokość kolejki  
Głębokość kolejki lub długość kolejki lub rozmiar kolejki jest liczbę oczekujących żądań Jo w systemie. Wartość głębokość kolejki określa liczbę operacji Jo aplikacji można wyrównać, którego będzie przetwarzania dysków w magazynie. Wpływa na wszystkich wskaźników wydajności trzy aplikacji wspomniano w tym tj artykuł operacji i/o na SEKUNDĘ przepustowość i opóźnienie.

W kolejce głębokość i wiele wątków są blisko powiązane. Wartość głębokość kolejki wskazuje, ile wiele wątków może być osiągnięte przez aplikację. Jeśli głębokość kolejki jest duży, aplikacja może wykonywać operacje więcej jednocześnie, innymi słowy, bardziej wiele wątków. W przypadku głębokość kolejki małe, mimo że aplikacja jest wielowątkowe, nie będzie miał za mało wyrównane wykonywania równoczesne żądania.

Zazwyczaj wyłączanie shelf aplikacji nie pozwalają na zmianę głębokość kolejki, ponieważ jeśli ustawione niepoprawnie wykonaj szkody więcej niż dobre. Aplikacje będą wartość prawo głębokość kolejki, aby uzyskać optymalną wydajność. Jednak należy zrozumieć tę zasadę, tak aby rozwiązać problemy z wydajnością, z aplikacją. Można również obserwować efekty głębokość kolejki, uruchamiając najlepszymi narzędzia w systemie.

Niektóre aplikacje przekazywać ustawienia wpływać głębokość kolejki. Na przykład ustawienie MAXDOP (maksymalny stopień równoległości) w programie SQL Server wyjaśnione w poprzedniej sekcji. MAXDOP jest można wpływać na głębokość kolejki i wiele wątków, mimo że nie bezpośrednio zmienić wartość głębokość kolejki programu SQL Server.

*Głębokość kolejki wysoki*  
Głębokość wysoka kolejki linii więcej operacji na dysku. Dysk zna następnego żądania w kolejki wyprzedzeniem. W związku z tym dysk można zaplanować operacje wyprzedzeniem i procesu ich w sekwencji optymalnego. Ponieważ aplikacja wysyła więcej żądań na dysku, dysku może przetworzyć więcej IOs równoległe. Ostatecznie aplikacji będą mogli uzyskać wyższy operacji i/o na SEKUNDĘ. Ponieważ aplikacji przetwarzania więcej żądań, zwiększa się też całkowita przepustowość aplikacji

Zazwyczaj aplikacji można osiągnięcia maksymalnej przepustowości z 8-16 + zaległych IOs na dysku dołączonym. Jeśli głębokość kolejki, aplikacja nie jest naciśnięcie za mało IOs do systemu i działającą mniejszej ilości w danym okresie. Innymi słowy mniej przepustowość.

Na przykład w programie SQL Server, ustawienie wartości MAXDOP dla zapytania "4" informuje programu SQL Server, że do czterech rdzeni można używać do wykonywania kwerendy. Program SQL Server określi, co jest zalecane wartość głębokości kolejki i liczby rdzeni wykonywania zapytań.

*Głębokość optymalnego kolejki*  
Wartość głębokości bardzo wysoki kolejki zawiera również jego wady. Jeśli wartość głębokości kolejki jest zbyt duża, aplikacja próbuje sterują bardzo wysoki operacji i/o na SEKUNDĘ. Jeśli aplikacji nie ma trwałych dysków przy użyciu wystarczających ustanawianie operacji i/o na SEKUNDĘ, to negatywnie wpłynąć na opóźnienia aplikacji. Obserwowanie formuły pokazano relacje między operacji i/o na SEKUNDĘ, opóźnienia i głębokość kolejki.  
    ![](media/storage-premium-storage-performance/image6.png)

Nie należy konfigurować głębokość kolejki do wartości wysoki, ale optymalnego wartość, która zapewnia wystarczająco dużo operacji i/o na SEKUNDĘ aplikacji bez wpływu na opóźnienia. Na przykład jeśli opóźnienie aplikacji musi być 1 milisekundy, głębokość kolejki wymaganego do osiągnięcia operacji i/o na SEKUNDĘ 5000 jest, QD = 5000 x 0,001 = 5.

*Głębokość kolejki rozłożone głośności*  
Woluminu rozłożone Obsługa głębokość wystarczająco wysoka kolejki tak, aby każdy dysk ma głębokość kolejki Szczyt pojedynczo. Na przykład można rozważyć aplikację, która umieszcza głębokość kolejki 2, a w pasek znajdują się 4 dysków. Dwa żądania Jo zostaną przekierowane do dwóch dysków, a pozostałe dwa dyski zostanie bezczynne. W związku z tym skonfiguruj głębokość kolejki tak, aby wszystkie dysków może być zajęty. Formuła poniżej pokazano sposób ustalania głębokość kolejki wielkości rozłożony.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Ograniczanie  
Azure przepisy magazynowania Premium określone liczbę operacji i/o na SEKUNDĘ i przepustowość w zależności od rozmiarów maszyn wirtualnych i rozmiarów dysków, które możesz wybrać. W dowolnym momencie aplikacja próbuje dysku operacji i/o na SEKUNDĘ lub przepustowość powyżej tych wielkości co maszyn wirtualnych lub dysk może obsługiwać, będą ograniczane przez Premium miejsca do magazynowania. Manifesty to w formularzu zmniejszoną wydajność w aplikacji. To oznacza opóźnienie wyższą, dolny przepustowość lub Zmniejsz operacji i/o na SEKUNDĘ. Jeśli miejsca do magazynowania Premium nie ograniczania, aplikacja całkowicie może nie działać przekroczenia co zasoby są w stanie realizacji. Tak aby uniknąć problemów z wydajnością z powodu ograniczania, zawsze obsługi administracyjnej wystarczających zasobów dla aplikacji. Uwzględnia wspomniano w sekcjach rozmiarów dysku powyżej i rozmiary maszyn wirtualnych. Wzorców jest najlepszy sposób, aby dowiedzieć się, jakie zasoby potrzebne do obsługi aplikacji.

## <a name="benchmarking"></a>Wzorców  
Wzorców jest proces symulowanie różnych obciążenia w swojej aplikacji i pomiaru wydajności aplikacji dla każdego obciążenie pracą. Wykonując kroki opisane w sekcji wcześniejszych, zostały zebrane wymagania aplikacji. Za pomocą narzędzia najlepszymi na maszyny wirtualne, hostingu aplikacji, można określić poziomów wydajności, które aplikacja można uzyskać z nośnikami Premium. W tej sekcji otrzymasz Przykłady wzorców standardowy maszyn wirtualnych DS14 obsługi administracyjnej magazyn Premium Azure dysków.

Odpowiednio użyliśmy typowych narzędzi najlepszymi Iometer i FIO, dla systemu Windows i Linux. Te narzędzia zduplikować wiele wątków symulowanie produkcji, takich jak obciążenie pracą i zmierzyć wydajność systemu. Korzystanie z narzędzi można także skonfigurować parametrów takich jak głębokości blok rozmiar i kolejki, które zwykle nie można zmienić dla aplikacji. Umożliwia większej elastyczności do kierowania maksymalną wydajność skali wysoka maszyn wirtualnych obsługi administracyjnej premium dysków dla różnych typów aplikacji. Aby dowiedzieć się więcej na temat poszczególnych narzędzi najlepszymi odwiedzaj [Iometer](http://www.iometer.org/) [FIO](http://freecode.com/projects/fio).

Aby wykonać poniższe przykłady, Utwórz standardowe maszyny DS14 i Dołącz 11 dysków w magazynie Premium do maszyn wirtualnych. 11 dysków skonfiguruj 10 dysków hosta pamięci podręcznej jako "Brak", a paskowych je do woluminu o nazwie NoCacheWrites. Konfigurowanie hosta pamięci podręcznej jako "Tylko do odczytu" na drugim dysku i utworzenia woluminu o nazwie CacheReads z dysku. Przy użyciu tej konfiguracji, będzie widoczny maksymalną wydajność odczytu i zapisu ze standardowego maszyny DS14. Aby uzyskać szczegółowe instrukcje dotyczące tworzenia maszyny DS14 dysków premium przejdź do [Tworzenie i używanie konta Premium miejsca do magazynowania dla maszyn wirtualnych dysku danych](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk).

*Ogrzewania pamięci podręcznej*  
Dysk z pamięci podręcznej hosta tylko do odczytu będzie można udzielić operacji i/o na SEKUNDĘ wyższymi niż limit dysku. Aby uzyskać ten Maksymalna wydajność odczytu z pamięci podręcznej hosta, najpierw należy musi rozgrzania pamięci podręcznej dysku. Dzięki temu, że IOs Odczyt, które narzędzie wzorcowe będzie dysków woluminu CacheReads faktycznie trafienia w pamięci podręcznej, a nie dysku bezpośrednio. Wynik trafień pamięci podręcznej w dodatkowych operacji i/o na SEKUNDĘ z jednym pamięci podręcznej włączone dysku.

>**Ważne:**  
>Przed uruchomieniem wzorców, każdorazowo po uruchomieniu maszyn wirtualnych musi rozgrzania pamięci podręcznej.

#### <a name="iometer"></a>Iometer   
[Pobierz narzędzie Iometer](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) na maszyn wirtualnych.

*Plik testowy*  
Iometer używa pliku test, który znajduje się na wielkość, na którym będzie uruchomiona najlepszymi test. Dyski Odczyt, a zapisuje na ten plik testowy do pomiaru dysku operacji i/o na SEKUNDĘ i przepustowość. Iometer tworzy ten plik testowy, jeśli nie podano jedną. Utwórz plik testowy 200GB o nazwie iobw.tst ilości CacheReads i NoCacheWrites.

*Specyfikacje programu Access*  
Specyfikacje, żądanie Jo rozmiar % odczytu/zapisu, % losowe sekwencyjne są skonfigurowane na karcie "Specyfikacje programu Access" w Iometer. Tworzenie Specyfikacja programu access dla każdego z scenariuszy opisane poniżej. Tworzenie specyfikacje programu access i "Zapisz" z odpowiednią nazwę jak — RandomWrites\_8K, RandomReads\_8K. Wybierz odpowiednie Specyfikacja podczas uruchamiania Scenariusz testów.

Przykład specyfikacje programu access do maksymalna scenariusza pisanie operacji i/o na SEKUNDĘ znajduje się poniżej,  
    ![](media/storage-premium-storage-performance/image8.png)

*Wymagania dotyczące badań maksymalna operacji i/o na SEKUNDĘ*  
Wykazać maksymalna operacji i/o na sekundę, użyj mniejszy rozmiar wezwanie. Użyj rozmiaru żądania 8K i Utwórz specyfikacje dotyczące losowe zapisuje i Odczyt.

| Specyfikacja programu Access | Żądanie rozmiar | Przypadkowe % | % Odczytu |
|----------------------|--------------|----------|--------|
| RandomWrites\_8K     | 8K           | 100      | 0      |
| RandomReads\_8K      | 8K           | 100      | 100    |

*Wymagania dotyczące badań maksymalnej przepustowości*  
Wykazać maksymalnej przepustowości, użyj większe wezwanie. Użyj rozmiaru żądania 64 KB i Utwórz specyfikacje dotyczące losowe zapisuje i Odczyt.

| Specyfikacja programu Access | Żądanie rozmiar | Przypadkowe % | % Odczytu |
|----------------------|--------------|----------|--------|
| RandomWrites\_64 KB    | 64 KB          | 100      | 0      |
| RandomReads\_64 KB     | 64 KB          | 100      | 100    |

*Uruchamiania testu Iometer*  
Wykonaj poniższe czynności, aby rozgrzania pamięci podręcznej

1.  Tworzenie dwóch specyfikacje programu access przy użyciu wartości, jak pokazano poniżej,

  	| Nazwa              | Żądanie rozmiar | Przypadkowe % | % Odczytu |
  	|-------------------|--------------|----------|--------|
  	| RandomWrites\_1MB | 1MB          | 100      | 0      |
  	| RandomReads\_1MB  | 1MB          | 100      | 100    |

2.  Uruchom test Iometer inicjowania pamięci podręcznej dysk z poniższych parametrów. Za pomocą trzech wątków wielkość docelowej i głębokość kolejki 128. Ustaw czas trwania badania "Czas wykonywania" 2hrs na karcie "Przetestuj ustawienia".

  	| Scenariusz              | Głośność docelowej | Nazwa              | Czas trwania |
  	|-----------------------|---------------|-------------------|----------|
  	| Inicjowanie dysku pamięci podręcznej | CacheReads    | RandomWrites\_1MB | 2hrs     |

3.  Uruchom test Iometer dla ogrzewania dysk pamięci podręcznej z poniższych parametrów. Za pomocą trzech wątków wielkość docelowej i głębokość kolejki 128. Ustaw czas trwania badania "Czas wykonywania" 2hrs na karcie "Przetestuj ustawienia".

  	| Scenariusz           | Głośność docelowej | Nazwa             | Czas trwania |
  	|--------------------|---------------|------------------|----------|
  	| Ciepły dysk pamięci podręcznej | CacheReads    | RandomReads\_1MB | 2hrs     |

Po rozgrzane dysku pamięci podręcznej, należy kontynuować scenariuszy testowania wymienione poniżej. Aby uruchomić Iometer test, użyj co najmniej trzy wątków dla **każdego** woluminu docelowej. Dla każdego wątku wybierz wielkość docelowej, Ustaw głębokość kolejki i wybierz jedną z specyfikacji zapisanego test, jak pokazano w poniższej tabeli, do uruchomienia odpowiednich Scenariusz testów. W tabeli pokazano również oczekiwanych wyników dla operacji i/o na SEKUNDĘ i przepustowość podczas uruchamiania testy. W przypadku wszystkich scenariuszy mały rozmiar IE 8KB i głębokości kolejki wysoki 128 jest używana.

| Scenariusz testów      | Głośność docelowej | Nazwa              | Wynik       |
|--------------------|---------------|-------------------|--------------|
| Maksymalna liczba. Przeczytaj operacji i/o na SEKUNDĘ     | CacheReads    | RandomWrites\_8K  | 50 000 OPERACJI I/O NA SEKUNDĘ  |
| Maksymalna liczba. Pisanie operacji i/o na SEKUNDĘ    | NoCacheWrites | RandomReads\_8K   | 64 000 OPERACJI I/O NA SEKUNDĘ  |
| Maksymalna liczba. Połączone operacji i/o na SEKUNDĘ | CacheReads    | RandomWrites\_8K  | 100 000 OPERACJI I/O NA SEKUNDĘ |
|                    | NoCacheWrites | RandomReads\_8K   |              |
| Maksymalna liczba. Przeczytaj MB/s   | CacheReads    | RandomWrites\_64 KB | 524 MB/s   |
| Maksymalna liczba. Pisanie MB/s  | NoCacheWrites | RandomReads\_64 KB  | 524 MB/s   |
| Połączone MB/s    | CacheReads    | RandomWrites\_64 KB | 1000 MB/s  |
|                    | NoCacheWrites | RandomReads\_64 KB  |              |

Poniżej znajdują się, że zrzuty ekranu przedstawiające Iometer wyniki testu dla Scalonej scenariusze operacji i/o na SEKUNDĘ i przepustowość.

*Połączone Odczyt i zapis operacji i/o na maksymalna SEKUNDĘ*  
![](media/storage-premium-storage-performance/image9.png)

*Połączone Odczyt i zapis maksymalnej przepustowości*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO  
FIO to narzędzie popularne do magazynu testu na maszyny wirtualne Linux. Ma możliwość wybierz różne rozmiary Jo sekwencyjne lub przypadkowe odczytuje i zapisuje. Spawns go wątków lub procesów do wykonania określonych czynności we/wy. Można określić typ operacji We/Wy każdego wątku należy wykonać przy użyciu plików zadania. Możemy utworzyć jeden plik zadania na scenariusz przedstawione w poniższych przykładach. Możesz zmienić specyfikacji w tych plików zadania porównania różne obciążenia uruchomionych Premium miejsca do magazynowania. W przykładach użyto standardowy maszyny 14 DS działa **Ubuntu**. Korzystanie z tej samej konfiguracji opisane na początku [sekcji Benchmarking](#Benchmarking) i ciepłej pamięci podręcznej przed uruchomieniem testów porównawczych.

Przed rozpocząć, [Pobierz FIO](https://github.com/axboe/fio) i zainstalować go na komputerze wirtualnych.

Uruchom następujące polecenie dla Ubuntu,

        apt-get install fio

Użyjemy cztery wątków do kierowania operacji zapisu i cztery wątków do kierowania wykonywania operacji odczytu na dyskach. Pracowników zapisu zostaną ruchu ruch na obrót "właściwość nocache", który ma 10 dysków z pamięci podręcznej "Brak". Pracowników odczytu zostanie ruchu ruch na obrót "readcache", w której znajduje się dysk 1 z pamięci podręcznej ustawiono "Tylko do odczytu".

*Maksymalna liczba zapisu operacji i/o na SEKUNDĘ*  
Utwórz plik zadania z następujących specyfikacji uzyskanie maksymalna operacji i/o pisanie na SEKUNDĘ. Nadaj mu nazwę "fiowrite.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Uwaga Obserwuj kluczowe elementy zgodnie z wytycznymi projektu omówiony w poprzednich sekcjach. Specyfikacje te są niezbędne do kierowania maksymalna operacji i/o na SEKUNDĘ,  
-   Długość kolejki wysoka 256.  
-   Mały rozmiar bloku 8 KB.  
-   Zapisuje wiele wątków wykonywania losowe.

Uruchom następujące polecenie, aby rozpocząć test FIO przez 30 sekund  

    sudo fio --runtime 30 fiowrite.ini

Test uruchomiony, można wyświetlić liczbę pisanie operacji i/o na SEKUNDĘ maszyn wirtualnych i przedstawiania Premium dysków. Jak pokazano w przykładzie poniżej, maszyn wirtualnych DS14 dostarcza jego zapisu maksymalny limit operacji i/o na SEKUNDĘ 50 000 operacji i/o na SEKUNDĘ.  
    ![](media/storage-premium-storage-performance/image11.png)

*Maksymalna liczba odczytu operacji i/o na SEKUNDĘ*  
Utwórz plik zadania z następujących specyfikacji uzyskanie maksymalna odczytu operacji i/o na SEKUNDĘ. Nadaj mu nazwę "fioread.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Uwaga Obserwuj kluczowe elementy zgodnie z wytycznymi projektu omówiony w poprzednich sekcjach. Specyfikacje te są niezbędne do kierowania maksymalna operacji i/o na SEKUNDĘ,

-   Długość kolejki wysoka 256.  
-   Mały rozmiar bloku 8 KB.  
-   Zapisuje wiele wątków wykonywania losowe.

Uruchom następujące polecenie, aby rozpocząć test FIO przez 30 sekund

    sudo fio --runtime 30 fioread.ini

Test uruchomiony, można Zobacz liczba odczytu operacji i/o na SEKUNDĘ maszyn wirtualnych i przedstawiania Premium dysków. Jak pokazano w przykładzie poniżej, maszyn wirtualnych DS14 dostarcza więcej niż 64 000 operacji i/o na SEKUNDĘ odczytu. Jest to kombinacja dysku i wydajności pamięci podręcznej.  
    ![](media/storage-premium-storage-performance/image12.png)

*Maksymalnie do odczytu i zapisu operacji i/o na SEKUNDĘ*  
Tworzenie pliku zadania następujące specyfikacje uzyskanie maksymalna połączone odczytu i zapisu operacji i/o na SEKUNDĘ. Nadaj mu nazwę "fioreadwrite.ini".

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Uwaga Obserwuj kluczowe elementy zgodnie z wytycznymi projektu omówiony w poprzednich sekcjach. Specyfikacje te są niezbędne do kierowania maksymalna operacji i/o na SEKUNDĘ,

-   Długość kolejki wysoki 128.  
-   Mały rozmiar bloku 4 KB.  
-   Wiele wątków wykonywania losowe odczytuje i zapisuje.

Uruchom następujące polecenie, aby rozpocząć test FIO przez 30 sekund

    sudo fio --runtime 30 fioreadwrite.ini

Test uruchomiony, będzie można wyświetlić liczbę Scalonej odczytu i zapisu operacji i/o na SEKUNDĘ maszyn wirtualnych i przedstawiania Premium dysków. Jak pokazano w przykładzie poniżej, maszyn wirtualnych DS14 dostarcza więcej niż 100 000 Scalonej odczytu i zapisu operacji i/o na SEKUNDĘ. Jest to kombinacja dysku i wydajności pamięci podręcznej.  
    ![](media/storage-premium-storage-performance/image13.png)

*Maksymalny łączny przepustowość*  
Uzyskanie maksymalna połączonych odczytu i zapisu przepustowość, Zablokuj rozmiar i głębokość kolejki dużych za pomocą wielu wątków wykonywania Odczyt i zapis. Rozmiar bloku 64 KB i służy głębokość kolejki 128.

## <a name="next-steps"></a>Następne kroki  

Dowiedz się więcej na temat magazyn Premium Azure:

- [Przechowywanie Premium: Wysokiej wydajności miejsca do magazynowania dla obciążenia Azure maszyn wirtualnych](storage-premium-storage.md)  

Dla użytkowników programu SQL Server, przeczytaj artykuły na temat wydajności najlepszych rozwiązań dla programu SQL Server:

- [Wydajność najważniejsze wskazówki dotyczące programu SQL Server w środowisku maszyn wirtualnych Azure](../virtual-machines/virtual-machines-windows-sql-performance.md)
- [Azure magazyn Premium oferuje najwyższą wydajność dla programu SQL Server w maszyn wirtualnych Azure](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx) 
