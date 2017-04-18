<properties
    pageTitle="Skalowanie analizy strumieniu zadania, aby zwiększyć przepustowość | Microsoft Azure"
    description="Dowiedz się, jak skalowanie analizy strumieniu zadań konfigurowania wprowadzania partycje, dostosowywanie definicją zapytania i ustawiając zadania streaming jednostki."
    keywords="strumieniowe przesyłanie danych strumieniowego przesyłania przetwarzania danych dostosować analizy"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Skalowanie Azure analizy strumieniu zadania, aby zwiększyć przepustowość przetwarzania danych strumienia

Dowiedz się, jak dostosować analizy zadań i obliczanie *Przesyłanie strumieniowe jednostki* dla analizy strumieniu sposobu skalowania analizy strumieniu zadań konfigurowania wprowadzania partycje, dostosowywanie definicji zapytania analizy i ustawiając zadania streaming jednostki. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Co to są części zadania analizy strumieniu?
Analizy strumieniu definicji zadania zawiera dane wejściowe, kwerendy i dane wyjściowe. Dane wejściowe są w miejsce, w którym zadanie odczytuje strumienia danych, kwerenda służy do przekształcania strumienia wprowadzania danych, a wynik to miejsce, w którym zadanie wysyła wyniki zadania do.  

Zadanie wymaga co najmniej jedno źródło wprowadzania dla strumieniowego przesyłania danych. Źródło wprowadzania strumienia danych mogą być przechowywane w koncentratora zdarzenia Bus usługi Azure lub w magazynie obiektów Blob platformy Azure. Aby uzyskać więcej informacji zobacz [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md) i [rozpocząć korzystanie z platformy Azure analizy strumieniu](stream-analytics-get-started.md).

## <a name="configuring-streaming-units"></a>Konfigurowanie przesyłania strumieniowego jednostki
Przesyłanie strumieniowe jednostki (SUs) reprezentują zasobów i obliczeniowe wymagany do wykonania zadania Azure analizy strumieniu. SUs umożliwiają opisujący wydarzenie względne przetwarzania zdolności na podstawie przejścia miary procesora i pamięci, czytać i zapisywać stawki. Każdej jednostki przesyłanie strumieniowe odpowiada około 1MB na sekundę przepustowości. 

Wybieranie SUs ile są wymagane do określonego zadania zależy od konfiguracji partycją danych wejściowych i kwerendy zdefiniowanych dla tego zadania. Możesz wybrać maksymalnie przydziału w streaming jednostki dla zadania za pomocą portalu klasyczny Azure. Każdej subskrypcji Azure domyślnie ma limit maksymalnie 50 jednostek przesyłanie strumieniowe dla wszystkich zadań analizy w określonym regionie. Aby zwiększyć przesyłanie strumieniowe jednostki dla subskrypcji, kontakt z [Pomocą techniczną firmy Microsoft](http://support.microsoft.com).

Liczba jednostek przesyłanie strumieniowe korzystające zadanie zależy od konfiguracji partycją danych wejściowych i kwerendy zdefiniowanych dla tego zadania. Zwróć uwagę, należy prawidłową wartość dla jednostek strumienia. Prawidłowe wartości zaczynają się od 1, 3, 6, a następnie w górę w przyrostach 6, tak jak pokazano poniżej.

![Analizy strumieniu Azure strumienia skali jednostek][img.stream.analytics.streaming.units.scale]

W tym artykule zostanie wyświetlona Obliczanie i dostosowywanie zapytania, aby zwiększyć przepustowość dla zadań analizy.

## <a name="embarrassingly-parallel-job"></a>Embarrassingly równoległe zadania
Embarrassingly równoległe zadanie jest najbardziej skalowalna scenariusza, który mamy w Azure analizy strumieniu. Jedną partycją dane wejściowe do jednego wystąpienia kwerendy łączy się jedną partycją danych wyjściowych. Osiągnięcie tego równoległości wymaga kilka rzeczy:

1.  Jeśli logiki kwerendy zależy od tego samego klucza przetwarzanych przez tego samego wystąpienia kwerendy, następnie należy się upewnić że zdarzenia przejdź do partycją dane wejściowe. W przypadku koncentratorów zdarzenia to oznacza, że dane zdarzenia musi mieć **PartitionKey** Ustaw lub można użyć podzielone na partycje nadawców. W przypadku obiektów Blob oznacza to, że zdarzenia są wysyłane do tego samego folderu partycją. Jeśli logiki zapytania nie wymaga tego samego klucza zostanie przetworzony przez tego samego wystąpienia kwerendy, można zignorować to wymaganie. Na przykład może być prostej kwerendy filtru wybierz/projektu.  
2.  Po danych jest rozmieszczona, takich jak musi znajdować się na stronie wprowadzania danych, należy upewnić się, że kwerenda jest podzielona. W tym celu za pomocą **Partition** we wszystkich czynności. Wiele kroków są dozwolone, ale muszą być one podzielone według tego samego klucza. Niekiedy pamiętać, to, że obecnie klucz podziału musi być ustawiona do **PartitionId** pełni równoległe zadania.  
3.  Tylko zdarzenie koncentratory i obiektów Blob obsługuje obecnie podzielone na partycje dane wyjściowe. Koncentratory zdarzenia wyników musisz skonfigurować pole **PartitionKey** być **PartitionId**. Dla obiektów Blob nie musisz wykonywać żadnych dodatkowych czynności.  
4.  Niekiedy pamiętać, liczba wprowadzania partycje musi być równa Liczba partycje dane wyjściowe. Wynik obiektów blob obecnie nie obsługuje partycje, ale jest to błąd, ponieważ będzie dziedziczyć schemat podziału nadrzędny kwerendy. Przykłady wartości partition, umożliwiające pełni równoległe zadania:  
    1.  8 koncentratory zdarzenie wejściowe partycje i 8 koncentratory zdarzenia wyjściowe partycje
    2.  8 koncentratory zdarzenie wprowadzania partycje i wynik obiektów Blob  
    3.  8 obiektów blob partycje wprowadzania i wynik obiektów Blob  
    4.  Partycje wyjściowe 8 obiektów blob partycje wprowadzania i 8 koncentratory zdarzenia  

Poniżej przedstawiono kilka przykładowych scenariuszy, które są embarrassingly równoległe.

### <a name="simple-query"></a>Prosta kwerenda
Centrum zdarzeń wprowadzania — koncentratory wydarzenia z 8 partycje dane wyjściowe — z 8 partycje

**Kwerenda:**

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Ta kwerenda jest prosty filtr i jako takie, firma Microsoft nie musisz martwić podziału danych wejściowych, wysyłanych do koncentratorów zdarzenia. Można zauważyć, że kwerenda zawiera **Partition By** programu **PartitionId**, więc możemy spełnić wymagania #2 z góry. Dane wyjściowe trzeba skonfigurować wynik koncentratory zdarzenia w pole **PartitionKey** ustawiono **PartitionId**zadania. Jeden ostatniego czeku, wprowadzania partycje == partycje dane wyjściowe. Ta topologia jest embarrassingly równoległe.

### <a name="query-with-grouping-key"></a>Kwerendy z kluczem grupowania
Wprowadzania — koncentratory wydarzenia z 8 partycje dane wyjściowe — obiektów Blob

**Kwerenda:**

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Ta kwerenda zawiera klucz grupowania i jako takie, tego samego klucza musi być przetwarzane przez tego samego wystąpienia kwerendy. Oznacza to, że trzeba wysłać naszych zdarzeń do koncentratorów zdarzeń w sposób podzielone na partycje. Firma Microsoft który klucz zwracać uwagę na? **PartitionId** koncepcja logiczny zadania, klawisz rzeczywistą poświęca wiele **TollBoothId**. Oznacza to, że firma Microsoft należy skonfigurować **PartitionKey** danych zdarzenia wyślemy do koncentratorów zdarzenia być **TollBoothId** zdarzenia. Kwerenda zawiera **Partition By** programu **PartitionId**, więc możemy istnieją dobre. Wyników ponieważ jest to obiektów Blob, firma Microsoft nie muszą martwić się o konfigurowaniu **PartitionKey**. Wymagania #4 ponownie, jest to obiektów Blob, więc nie trzeba o to martwić. Ta topologia jest embarrassingly równoległe.

### <a name="multi-step-query-with-grouping-key"></a>Kwerenda kroku wielu z kluczem grupowania ###
Wprowadzanie — Centrum wydarzenia z 8 partycje dane wyjściowe — Centrum wydarzenia z 8 partycje

**Kwerenda:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Ta kwerenda zawiera klucz grupowania i jako takie, tego samego klucza musi być przetwarzane przez tego samego wystąpienia kwerendy. Firma Microsoft korzysta samej strategii jako poprzedniej kwerendy. Kwerenda zawiera wiele kroków. Każdy krok ma **Partition By** programu **PartitionId**? Tak, więc możemy dobre. W wynikach należy ustawić **PartitionKey** **PartitionId** , takich jak opisanych powyżej i można zobaczyć, czy ma on taką samą liczbę partycje jako danych wejściowych. Ta topologia jest embarrassingly równoległe.


## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Przykładowe scenariusze, które nie są embarrassingly równoległego

### <a name="mismatched-partition-count"></a>Niezgodność liczby Partition ###
Centrum zdarzeń wprowadzania — koncentratory wydarzenia z 8 partycje dane wyjściowe — z 32 partycje

Nie ma znaczenia, co kwerenda jest w tym przypadku ponieważ dane wejściowe partycje liczba! = Liczba partition dane wyjściowe.

### <a name="not-using-event-hubs-or-blobs-as-output"></a>Nie korzystasz z koncentratorów zdarzenie lub obiektów blob jako wynik
Wprowadzania — koncentratory wydarzenia z 8 partycje dane wyjściowe — PowerBI

Wynik PowerBI nie obsługuje obecnie podziału.

### <a name="multi-step-query-with-different-partition-by-values"></a>Wielokrotne krok zapytania o różnych wartościach Partition By
Wprowadzanie — Centrum wydarzenia z 8 partycje dane wyjściowe — Centrum wydarzenia z 8 partycje

**Kwerenda:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
Jak widać, drugim krokiem używa **TollBoothId** jako klucz podziału. To nie jest taka sama jak najpierw i dlatego będzie wymagało nam robić losowo. 

Oto niektóre przykłady i counterexamples analizy strumieniu zadań, które będą mogły uzyskać embarrassingly równoległe topologii i z nim możliwości maksymalną skalę. Dla zadań, które nie mieszczą się jeden z tych profili będą przyszłych aktualizacji informacji o sposobach maksymalnie skalowanie innych kanonicznych scenariuszy analizy strumieniu.

Rozpoczynanie teraz Użyj ogólne wskazówki poniżej:

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Obliczanie maksimum streaming jednostki zadania
Całkowita liczba jednostek przesyłanie strumieniowe, które mogą być używane przez zadanie analizy strumieniu zależy od liczby kroków w kwerendzie zdefiniowanych dla zadania i liczba partycje dla każdego kroku.

### <a name="steps-in-a-query"></a>Kroki opisane w kwerendzie
Kwerenda może mieć jednego lub kilku kroków. Każdy krok jest podkwerendy zdefiniowane przy użyciu słowa kluczowego **WITH** . Tylko kwerendy, który wykracza poza słowa kluczowego **WITH** również jest liczony jako krok, na przykład instrukcji **SELECT** w następującej kwerendy:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Poprzedniej kwerendy ma dwa kroki.

> [AZURE.NOTE] Ta przykładowa kwerenda opisano w dalszej części tego artykułu.

### <a name="partition-a-step"></a>Partycje kroku

Podział kroku wymaga następujących warunków:

- Musi być podzielony źródła danych wejściowych. Aby uzyskać więcej informacji, zobacz [Przewodnik programowania koncentratory zdarzenia](../event-hubs/event-hubs-programming-guide.md).
- Instrukcji **SELECT** kwerendy muszą przeczytaj ze podzielone na partycje źródła danych wejściowych.
- Kwerenda w kroku musi mieć **Partition według** słów kluczowych

Po kwerendy jest podzielona, wprowadzania zdarzenia będą przetwarzane i zagregowane w osobnym partition grup i zdarzenia wyjściowe są generowane dla każdej z grup. Pożądane jest połączony agregacji, należy utworzyć drugi krok — podzielona do agregacji.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Obliczanie max streaming jednostki dla zadania

Razem wszystkich kroków partycją można skalować maksymalnie sześć przesyłanie strumieniowe jednostek zadanie analizy strumieniu. Aby dodać dodatkowe streaming jednostki kroku musi być podzielony. Każdy partition może mieć sześć przesyłanie strumieniowe jednostki.

<table border="1">
<tr><th>Kwerendy zadania</th><th>Maksymalna liczba streaming jednostki dla zadania</th></td>

<tr><td>
<ul>
<li>Kwerenda zawiera krok.</li>
<li>Krok nie jest podzielona.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Strumień danych wejściowych jest podzielona przez 3.</li>
<li>Kwerenda zawiera krok.</li>
<li>Krok jest podzielona.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>Kwerenda zawiera dwa kroki.</li>
<li>Nie zaznaczaj żadnej czynności jest podzielona.</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>Dane wejściowe strumienia jest podzielona przez 3.</li>
<li>Kwerenda zawiera dwa kroki. Wprowadzania kroku jest podzielony na partycje i nie jest drugim krokiem.</li>
<li>Instrukcja SELECT odczytuje z podzielone na partycje danych wejściowych.</li>
</ul>
</td>
<td>24 (18 podzielone na partycje instrukcje) + 6-partycją kroki</td></tr>
</table>

### <a name="examples-of-scale"></a>Przykłady skali
Poniższe zapytanie oblicza liczbę samochodów przechodzące przez stację płatne z trzech tollbooths w oknie trzy minuty. Ta kwerenda można skalować maksymalnie sześć przesyłanie strumieniowe jednostki.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Aby użyć większej liczby jednostek przesyłanie strumieniowe kwerendy, zarówno dane strumienia danych wejściowych i kwerendy muszą być podzielone. Zakładając, że partycją strumienia danych jest ustawiona na 3, poniższe zapytanie modyfikacji można skalować maksymalnie 18 jednostki przesyłanie strumieniowe:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Po kwerendy jest podzielona, wprowadzania wydarzenia są przetwarzane i agregacją w grupach osobne partycje. Dane wyjściowe zdarzenia są również generowane dla każdej z grup. Podziału mogą powodować pewne nieoczekiwane wyniki, gdy pole **Grupuj według** nie jest kluczem Partition w danych wejściowych strumienia. Na przykład pole **TollBoothId** w poprzednim przykładzie kwerendy nie jest partycją klucza Input1. Dane z budki nr 1 można rozmieszczać w wielu partycje.

Każdy partycje Input1 będą przetwarzane oddzielnie przez analizy strumieniu i wielu rekordów liczba samochodu przebiegu wskroś dla samej budki w tym samym oknie tumbling zostanie utworzona. Jeśli nie można zmienić klucza wprowadzania partition, ten problem można ustalić, dodając dodatkowych czynności-partition, na przykład:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Ta kwerenda można skalować do 24 streaming jednostki.

>[AZURE.NOTE] Jeśli się dwoma strumieniami, upewnij się, że strumienie oddzielone od siebie przez klucz partition kolumnę, którą możesz wykonać sprzężenia, i mieć taką samą liczbę partycje w obu strumieni.


## <a name="configure-stream-analytics-job-partition"></a>Konfigurowanie analizy strumieniu zadania partition

**Aby dostosować przesyłanie strumieniowe jednostek dla zadania**

1. Zaloguj się do [portalu zarządzania](https://manage.windowsazure.com).
2. W lewym okienku kliknij pozycję **Analizy strumieniu** .
3. Kliknij zadanie analizy strumieniu, który chcesz skalować.
4. Kliknij pozycję **Skala** w górnej części strony.

![Analizy strumieniu Azure strumienia skali jednostek][img.stream.analytics.streaming.units.scale]

W portalu Azure ustawień skali są dostępne w obszarze Ustawienia:

![Konfiguracji zadań analizy strumieniu Portal Azure][img.stream.analytics.preview.portal.settings.scale]

## <a name="monitor-job-performance"></a>Monitorowanie wydajności zadania

Za pomocą portalu zarządzania, można śledzić przepustowość zadanie w wydarzeń na sekundę:

![Azure analizy strumieniu monitorowanie zadań][img.stream.analytics.monitor.job]

Obliczanie oczekiwanych przepustowość obciążenie pracą w wydarzeń na sekundę. Jeśli przepustowość jest mniejszy niż oczekiwano, dostosowywanie partycją wprowadzania, dostosowywanie kwerendy i dodać dodatkowe jednostki przesyłanie strumieniowe do zadania.

## <a name="stream-analytics-throughput-at-scale---raspberry-pi-scenario"></a>Przepustowość analizy strumieniu w skali - Pi malina scenariusz


Aby dowiedzieć się, jak zadania analizy strumieniu skalowanie typowy scenariusz pod względem przepustowości przetwarzania przez wiele jednostek Streaming, Oto doświadczenia wysyła dane czujnika (klienci) do koncentratora zdarzenia, przetwarza je i wysyła alertu lub statystyki jako wynik do innego koncentratora zdarzenia.

Klient wysyła dane syntetyzowanej czujnika do koncentratorów zdarzenia w formacie JSON analizy strumieniu i danych wyjściowych jest również w formacie JSON.  Poniżej opisano, jak będą wyglądać dane przykładowe podobne —  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Kwerenda: "wysyłania alertu po wyłączeniu uproszczonej"  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

Mierzenie przepustowość: Przepustowość w tym kontekście jest kwotą wprowadzania danych przetwarzanych przez analizy strumieniu w stałej ilość czasu (10 minut). Aby uzyskać najlepsze przepustowość przetwarzania dla danych wejściowych, zarówno dane strumienia wprowadzania i kwerendy muszą być podzielone. Również **COUNT()**znajduje się do pomiaru zrealizowanych ile wydarzeń wprowadzania danych w kwerendzie. Aby upewnić się, że zadanie nie jest po prostu oczekiwanie na wprowadzania zdarzeń zostanie dodane później, każdego partition wprowadzania Centrum zdarzeń został wstępnie załadowane z wystarczających danych wejściowych (około 300 MB).  

Poniżej przedstawiono wyniki z zwiększenie liczby jednostek Streaming i partycją odpowiednie liczby w koncentratory zdarzenia.  

<table border="1">
<tr><th>Partycje wprowadzania danych</th><th>Dane wyjściowe partycje</th><th>Przesyłanie strumieniowe jednostki</th><th>Trwały przepustowość
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

![img.stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum Azure analizy strumieniu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Następne kroki

- [Wprowadzenie do analizy strumieniu Azure](stream-analytics-introduction.md)
- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings.png

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
