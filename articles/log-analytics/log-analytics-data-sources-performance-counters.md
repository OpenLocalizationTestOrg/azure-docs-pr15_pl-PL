<properties 
   pageTitle="Wydajność systemu Windows i Linux oraz liczniki dziennika analizy | Microsoft Azure"
   description="Liczniki wydajności są zbierane przez analizy dziennika do analizowania wydajności systemu Windows i Linux.  Ten artykuł zawiera opis sposobu konfigurowania zbioru liczników wydajności dla obu systemu Windows i Linux oraz czynników, szczegóły one są przechowywane w repozytorium usługi OMS i analizowania ich w portalu usługi OMS."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows i Linux źródeł danych wydajności w analizy dziennika 

Liczniki wydajności w systemie Windows i Linux oraz udostępniają wgląd w wydajności sprzętowej, systemów operacyjnych i aplikacji.  Analizy dziennika może zbierać liczników wydajności w częstych odstępach czasu na potrzeby analizy w pobliżu czasu rzeczywistego (NRT) oprócz agregowania dane dotyczące wydajności dłużej analizy terminów i raportowania.

![Liczniki wydajności](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Konfigurowanie liczników wydajności

Konfigurowanie liczników wydajności z [menu dane w ustawieniach analizy dziennika](log-analytics-data-sources.md#configuring-data-sources).

Podczas pierwszego konfigurowania systemu Windows i Linux oraz wydajności liczniki dla nowego obszaru roboczego usługi OMS, są podane opcję, aby szybko utworzyć kilka typowych liczników.  Są one wyświetlane przy użyciu pola wyboru obok każdej.  Upewnij się, że żadnych liczników, które chcesz wstępnie utworzone są zaznaczone, a następnie kliknij przycisk **Dodaj liczniki zaznaczonego wydajności**.

![Konfigurowanie liczników wydajności systemu Windows](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Wykonaj poniższą procedurę, aby dodać nowy licznik wydajności systemu Windows na potrzeby zbierania.

1. Wpisz nazwę licznika w polu tekstowym w formacie *\counter obiektu (wystąpienia)*.  Po rozpoczęciu wpisywania tekstu, są prezentowane pasujące lista typowych liczników.  Licznik można albo wybierz z listy lub wpisz własny.  Można także zwracać wszystkie wystąpienia określonego licznika, określając *object\counter*. 
2. Kliknij pozycję **+** lub naciśnij klawisz **Enter** , aby dodać licznik do listy.
3. Po dodaniu licznik używa domyślnego 10 sekund dla jej **Interwału**.  Możesz zmienić to na wartość większą do 1800 sekund (30 minut) Jeśli chcesz zmniejszyć wymagania dotyczące miejsca do magazynowania danych zebranych wydajności.
4. Po zakończeniu dodawania liczników kliknij przycisk **Zapisz** w górnej części ekranu, aby zapisać konfigurację.

![Konfigurowanie liczników wydajności Linux](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Wykonaj poniższą procedurę, aby dodać nowy licznik wydajności Linux na potrzeby zbierania.

1. Domyślnie wszystkie zmiany konfiguracji są automatycznie przenoszone do wszystkich agentów.  Dla czynników Linux pliku konfiguracji są wysyłane do modułów zbierających dane Fluentd.  Jeśli chcesz zmodyfikować ten plik ręcznie w każdej agenta Linux, a następnie usuń zaznaczenie pola *Zastosuj poniżej konfiguracji na komputerach moje Linux*.
2. Wpisz nazwę licznika w polu tekstowym w formacie *\counter obiektu (wystąpienia)*.  Po rozpoczęciu wpisywania tekstu, są prezentowane pasujące lista typowych liczników.  Licznik można albo wybierz z listy lub wpisz własny.  
2. Kliknij pozycję **+** lub naciśnij klawisz **Enter** , aby dodać licznik do listy inne liczniki obiektu.
3. Wszystkie liczniki obiektu za pomocą samego **Interwału**.  Wartość domyślna to 10 sekund.  Możesz zmienić to na wartość większą do 1800 sekund (30 minut) Jeśli chcesz zmniejszyć wymagania dotyczące miejsca do magazynowania danych zebranych wydajności.
4. Po zakończeniu dodawania liczników kliknij przycisk **Zapisz** w górnej części ekranu, aby zapisać konfigurację.

## <a name="data-collection"></a>Zbieranie danych

Analizy dziennika zbiera wszystkie liczniki wydajności określony w ich określonego interwału na wszystkich czynników, które zostały zainstalowane licznik.  Dane nie jest agregowane i nieprzetworzonych danych jest dostępna we wszystkich widokach wyszukiwania dziennika na czas trwania wg subskrypcji usługi OMS.


## <a name="performance-record-properties"></a>Właściwości rekordu wyników

Wydajność rekordy mają typ **wydajności** i mają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--|:--|
| Komputer         | Komputer, który zdarzenie zostało pobrane od. |
| CounterName      | Nazwa licznika wydajności |
| Ścieżka_licznika      | Pełna ścieżka licznika w formularzu \\ \\ \<komputera >\\obiekt(wystąpienie)\\licznik. |
| Równowartości     | Wartość liczbowa licznika.  |
| Nazwa_wystąpienia     | Nazwa wystąpienia zdarzenia.  Pusty, jeśli nie ma wystąpień. |
| Nazwa obiektu       | Nazwa obiektu wydajności |
| SourceSystem  | Typ agenta zgromadzone dane z. <br> Łączenie OpsManager — agent systemu Windows, bądź bezpośrednio lub SCOM <br> Linux — wszystkich agentów Linux  <br> AzureStorage — Diagnostyka Azure |
| TimeGenerated       | Data i godzina, które zostało pobrane dane. |


## <a name="sizing-estimates"></a>Szacuje zmiany rozmiaru

 Oszacowanie kolekcji określonego licznika w odstępach 10-sekundowe to około 1 MB na dzień dla każdego wystąpienia.  Wymagania dotyczące magazynu określonego licznika przy użyciu następującej formuły można oszacować.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Wyszukiwanie dziennika z rekordami wydajności

Poniższa tabela zawiera różnych Przykłady wyszukiwania dziennika, które służą do pobierania wyników.

| Kwerendy | Opis |
|:--|:--|
| Typ = wydajności | Wszystkie dane dotyczące wydajności |
| Typ = komputer wydajności = "Mój komputer" | Wszystkie dane dotyczące wydajności z określonego komputera |
| Typ = CounterName wydajności = "Bieżąca długość kolejki dysku" | Wszystkie dane dotyczące wydajności dla określonego licznika |
| Typ = wydajności (nazwa_obiektu = procesor) CounterName = "% czasu procesora" InstanceName = _Total & #124; Miara Avg(Average) jako AVGCPU przez komputer | Średnia procesora na wszystkich komputerach |
| Typ = wydajności (CounterName = "% procesora Time") & #124;  Zmierz max(Max) przez komputer | Maksymalna liczba procesora na wszystkich komputerach |
| Typ = nazwa_obiektu wydajności = CounterName logiczny = komputer "Bieżącego dysku długość kolejki" = "MyComputerName" & #124; Miara Avg(Average) przez nazwa_wystąpienia | Obliczanie średniej wartości Bieżąca długość kolejki dysku we wszystkich wystąpieniach danego komputera |
| Typ = CounterName wydajności = "DiskTransfers/sec" & #124; Zmierz percentile95(Average) przez komputer | 95 percentylu z dysku przeniesienia/s na wszystkich komputerach |
| Typ = CounterName wydajności = "% procesora Time" InstanceName = "_Suma" & #124; Zmierz avg(CounterValue) przez komputer interwał 1 godzina | Co godzinę średnią użycie Procesora na wszystkich komputerach |
| Typ = komputer wydajności = "Mój komputer" CounterName = % * InstanceName = _Total & #124; Zmierz percentile70(CounterValue) od interwału CounterName 1 godzina | Co godzinę 70 percentyl każdej procentu licznik dla określonego komputera |
| Typ = CounterName wydajności = "% procesora Time" InstanceName = "_Suma" (komputera = "Mój komputer") & #124; Zmierz min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) przez komputer interwał 1 godzina | Co godzinę średnią, minimalną, maksymalną i 75 percentyl użycie Procesora dla danego komputera |

## <a name="viewing-performance-data"></a>Przeglądanie danych dotyczących wydajności

Po uruchomieniu wyszukiwania dziennika dane dotyczące wydajności, jest domyślnie wyświetlany widok **dziennika** .  Aby wyświetlić dane w formie graficznej, kliknij pozycję **wskaźniki**.  Aby uzyskać szczegółowy widok graficzny, kliknij **+** obok licznik.  

![Widok metryki zwinięte](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Jeśli zakres czasu, który wybrano jest 6 godzin lub mniej, wykres jest aktualizowana co kilka sekund.  Dynamiczne dane zostanie wyświetlony po prawej stronie wykresu w kolorze jasnoniebieskim.

![Rozszerzony widok metryki przy użyciu danych dynamicznych](media/log-analytics-data-sources-performance-counters/metricsexpanded.png)

Agregowanie danych dotyczących wydajności w wynikach wyszukiwania dziennika, zobacz [agregacji metryki na żądanie i wizualizacji w usługi OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/).

## <a name="next-steps"></a>Następne kroki

- Informacje na temat [wyszukiwania dziennika](log-analytics-log-searches.md) do analizowania danych zebranych ze źródła danych i rozwiązań.  
- Eksportowanie danych zebranych do [Usługi Power BI](log-analytics-powerbi.md) dodatkowe wizualizacje i analizy.