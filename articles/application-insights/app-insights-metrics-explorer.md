<properties 
    pageTitle="Poznawanie metryki w aplikacji wniosków | Microsoft Azure" 
    description="Jak interpretować wykresy w Eksploratorze metryczne i jak dostosować karty metryczne Eksploratora." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/15/2016" 
    ms.author="awills"/>
 
# <a name="exploring-metrics-in-application-insights"></a>Poznawanie metryki w aplikacji wniosków

Metryki w [Aplikacji wniosków] [ start] mierzonych i liczby zdarzeń, które są wysyłane w telemetrycznego z poziomu aplikacji. Ułatwiają one wykrywanie występują problemy z wydajnością i obejrzyj trendów w sposobu używania aplikacji. Jest szeroką gamę standardowy metryki i możesz również utworzyć własne niestandardowe metryki i zdarzeń.

Liczników wskaźników i wydarzenia są wyświetlane na wykresach zagregowane wartości, takie jak sum, średnie lub liczebności.

Oto przykładowy wykres:

![Otwórz karta Przegląd aplikacji w portalu Azure](./media/app-insights-metrics-explorer/01-overview.png)

Niektórych typów wykresów są podzielone: całkowita wysokość wykresu w dowolnym momencie jest sumą metryki wyświetlane. Legenda domyślnie wyświetlane są największych ilości.

Linie kropkowane wyświetlana wartość metryki tydzień wcześniej.



## <a name="time-range"></a>Zakres czasu

Możesz zmienić zakres czasu objęte siatek na dowolnym karta lub wykresów.

![Otwórz karta Przegląd aplikacji w portalu Azure](./media/app-insights-metrics-explorer/03-range.png)


Jeśli masz oczekiwano pewne dane, które jeszcze nie pojawiły się, kliknij przycisk Odśwież. Wykresy odświeżanie się w odstępach, ale interwały są dłużej większych zakresów czasu. W trybie wersji może minąć trochę czasu, zanim danych zostanie dodane później potoku analizy na wykres.

Aby powiększyć fragment wykresu, przeciągnij nad nim:

![Przeciągnij przez fragment wykresu.](./media/app-insights-metrics-explorer/12-drag.png)

Kliknij przycisk Cofnij powiększenie, aby go przywrócić.


## <a name="granularity-and-point-values"></a>Wartości dokładności i punkt

Umieść kursor myszy na wykresie, aby wyświetlić wartości miar w tym momencie.

![Umieść wskaźnik myszy na wykresie](./media/app-insights-metrics-explorer/02-focus.png)

Wartość metryki w określonym miejscu jest agregowane przedziale próbki. 

Interwał pobierania lub "dokładności" jest wyświetlany w górnej części karta. 

![Nagłówek kart.](./media/app-insights-metrics-explorer/11-grain.png)

Można dostosować szczegółowości w karta zakres czasu:

![Nagłówek kart.](./media/app-insights-metrics-explorer/grain.png)

Stopnie szczegółowości dostępne zależy od wybranego przedziału czasu. Jawne stopnie szczegółowości są rozwiązania alternatywne wobec "Automatyczny" szczegółowości dla przedziału czasu. 

## <a name="metrics-explorer"></a>Eksplorator metryki

Kliknij dowolny wykres na karta Przegląd, aby wyświetlić szczegółowe zestawu pokrewne wykresy i siatki. Możesz edytować te wykresy i siatki odbiorcom skoncentrowanie się na stronie Szczegóły, który Cię interesuje.

Lub wystarczy kliknąć przycisk Eksploratora metryki w nagłówku karta Przegląd.

Na przykład kliknij za pośrednictwem aplikacji sieci web wykres żądania nie powiodło się:

![Karta Przegląd wybierz polecenie wykresu](./media/app-insights-metrics-explorer/14-trix.png)


## <a name="what-do-the-figures-mean"></a>Co oznaczają dane?

Legenda z boku domyślnie zazwyczaj zawiera wartość zagregowaną okresie wykresu. Po umieszczeniu wskaźnika myszy na wykresie, dzięki niemu wartość w tym momencie.

Każdy punkt danych na wykresie jest sumą wartości danych otrzymanych w poprzednim interwał pobierania lub "dokładności". Stopień szczegółowości jest wyświetlana u góry karta i zależy od ogólnej skali czasu wykresu.

Może być zagregowana metryki na różne sposoby: 

 * **Suma** sumuje wartości punktów danych odebranych przez interwał pobierania lub okres wykresu.
 * **Średnia** dzieli sumę przez liczbę punktów danych odebranych przez interwał.
 * Zlicza **unikatowe** są używane dla liczby użytkowników i konta. W okresie próbek lub w okresie wykres na rysunku pokazano liczba różnych użytkowników, w tym czasie.


Możesz zmienić metodę agregacji:

![Zaznacz wykres, a następnie wybierz pozycję agregacji](./media/app-insights-metrics-explorer/05-aggregation.png)

Metoda domyślna dla każdego metryki jest wyświetlana podczas tworzenia nowego wykresu lub gdy zaznaczona wszystkie metryki:

![Usuń zaznaczenie wszystkich metryki, aby wyświetlić ustawienia domyślne](./media/app-insights-metrics-explorer/06-total.png)



## <a name="editing-charts-and-grids"></a>Edytowanie siatki i wykresy

Aby dodać nowego wykresu do karta:

![W Eksploratorze metryki wybierz pozycję Dodaj wykresu](./media/app-insights-metrics-explorer/04-add.png)

Kliknij przycisk **Edytuj** na wykresie istniejącym lub nowym do edytowania, co zawiera:

![Zaznacz jeden lub więcej metryki](./media/app-insights-metrics-explorer/08-select.png)

Można wyświetlić więcej niż jeden Metryka na wykresie, jeśli istnieją ograniczenia dotyczące kombinacje, które mogą być wyświetlane razem. Jak wybrać jeden metryka, niektóre innych są wyłączone. 

Jeśli kodowane [niestandardowe metryki] [ track] do aplikacji (połączeń TrackMetric i TrackEvent) zostaną one wymienione w tym miejscu.

## <a name="segment-your-data"></a>Segmenty danych

Możesz podzielić metryki przez właściwość — na przykład, aby porównać wyświetleń stron na klientach z innych systemów operacyjnych. 

Zaznacz na wykresie lub siatce, Przełącz na grupowania i wybierz właściwość do Grupuj według:

![Wybierz grupowania na, a następnie ustaw wybierz właściwość Grupuj według](./media/app-insights-metrics-explorer/15-segment.png)

> [AZURE.NOTE] Użycie opcji grupowania, typy obszar i wykres słupkowy zapewniają skumulowany wyświetlania. To jest odpowiednie miejsce, w którym metoda agregacji to suma. Typ agregacji w przypadku średnia, wybierz typy wyświetlania linii lub siatkę, ale. 

Jeśli kodowane [niestandardowe metryki] [ track] do aplikacji i zawierają wartości nieruchomości, będziesz mieć możliwość wybierz właściwość z listy.

Wykres jest za mały dla podzielonych danych? Dopasowywanie wysokości:


![Suwak](./media/app-insights-metrics-explorer/18-height.png)


## <a name="filter-your-data"></a>Filtrowanie danych

Aby wyświetlić tylko metryki dla wybranego zestawu wartości właściwości:

![Kliknij pozycję Filtr, rozwiń pozycję właściwości i zaznacz kilka wartości](./media/app-insights-metrics-explorer/19-filter.png)

Jeśli nie zaznaczysz żadnych wartości dla określonej właściwości jest taka sama jak zaznaczając je wszystkie: istnieje bez filtru na tej właściwości.

Zwróć uwagę, liczby wydarzeń obok każdej wartości właściwości. Po wybraniu wartości jednej właściwości są tak dostosowywane zlicza obok wartości innych właściwości.

Filtry są stosowane do wszystkich schematów na kartę. Jeśli chcesz różne filtry zastosowane do różne wykresy, tworzenie i zapisywanie różnych metryki karty. Jeśli chcesz, możesz przypiąć wykresów z innej karty do pulpitu nawigacyjnego, tak, aby były widoczne obok siebie.


### <a name="remove-bot-and-web-test-traffic"></a>Usuwanie ruchu test robot i sieci web

Za pomocą filtru **ruchu rzeczywistą lub syntetycznego** i sprawdź **rzeczywistą**.

Można również filtrować przez **źródło syntetycznych ruchu**.

### <a name="to-add-properties-to-the-filter-list"></a>Aby dodać do listy filtrów właściwości

Czy chcesz odfiltrować telemetrycznego w kategorii wybranej przez użytkownika? Na przykład może dzielenie konta użytkowników w różne kategorie, a potem segmentu danych według kategorii.

[Tworzenie własnych właściwości](app-insights-api-custom-events-metrics.md#properties). Ustaw go w [Inicjator telemetrycznego](app-insights-api-custom-events-metrics.md#telemetry-initializers) aby były wyświetlane wszystkie telemetrycznego — w tym telemetrycznego standardowy, wysyłane przez różne moduły SDK.


## <a name="edit-the-chart-type"></a>Edytowanie typu wykresu

Należy zauważyć, że możesz przełączać się między siatki i wykresy:

![Wybierz siatkę lub wykres, a następnie wybierz typ wykresu](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>Zapisywanie swojego karta metryki

Po utworzeniu niektórych typów wykresów, zapisz je jako ulubione. Możesz określić, czy udostępnić go innym członkom zespołu, jeśli korzystasz z konta organizacji.

![Wybierz ulubione](./media/app-insights-metrics-explorer/21-favorite-save.png)

Aby wyświetlić karta ponownie, **Przejdź do pozycji Karta Przegląd** i otwieranie ulubionych:

![Wybieranie elementów ulubionych w karta Przegląd](./media/app-insights-metrics-explorer/22-favorite-get.png)

Jeśli wybierzesz zakres czasu względne po zapisaniu, karta zostaną zaktualizowane przy użyciu najnowszych metryki. Jeśli wybierzesz zakres czasu bezwzględnego będzie widoczny te same dane co godzinę.

## <a name="reset-the-blade"></a>Resetowanie karta

Jeśli edytujesz karta, ale następnie chcesz wrócić do oryginalnej wersji zapisany zestaw, po prostu kliknij przycisk Resetuj.

![Na liście przyciski w górnej części Metryka Eksploratora](./media/app-insights-metrics-explorer/17-reset.png)

<a name="live-metrics-stream"></a>
## <a name="live-metrics-stream-instant-metrics-for-close-monitoring"></a>Strumień na żywo metryki: błyskawiczne metryki Zamknij monitorowania

Strumień na żywo metryki zawiera usługi metryki aplikacji bezpośrednio w tym samym momencie, z najbliższego opóźnienie czasu rzeczywistego 1 sekundę. Jest to bardzo przydatne, gdy masz udostępnia nową kompilację i chcesz upewnić się, że wszystko jest działają zgodnie z oczekiwaniami lub badania zdarzenia w czasie rzeczywistym.

![Karta Przegląd kliknij strumień na żywo](./media/app-insights-metrics-explorer/live-stream.png)

W przeciwieństwie do Eksploratora metryki strumień metryki na żywo Wyświetla ustalony zbiór metryki. Dane trwa tylko pod warunkiem jest na wykresie, a następnie jest pomijany. 

Strumień na żywo metryki jest dostępna z SDK wniosków aplikacji dla programu ASP.NET, wersji 2.1.0 lub nowszej.

## <a name="set-alerts"></a>Ustawianie alertów

Aby otrzymywać powiadomienia pocztą e-mail wartości dowolnym metryki, Dodaj alertu. Możesz albo wysłać wiadomość e-mail, administratorom konta lub do adresów e-mail określone.

![W Eksploratorze metryki Wybierz reguły alertów, dodawanie alertów](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

[Dowiedz się więcej o alerty][alerts].

## <a name="export-to-excel"></a>Eksportowanie do programu Excel

Możesz wyeksportować dane wyświetlane w Eksploratorze metryki do pliku programu Excel. Wyeksportowane dane zawiera dane z wszystkich wykresów i tabel w portalu. 


![W Eksploratorze metryki Wybierz reguły alertów, dodawanie alertów](./media/app-insights-metrics-explorer/31-export.png)

Na oddzielnym arkuszu w pliku programu Excel są eksportowane dane dla każdego wykres lub tabelę.

To co widzisz zakres eksportowanych. Jeśli chcesz zmienić zakres danych, wyeksportowane zmienić zakres czasu lub filtry. W przypadku tabel Jeśli polecenie **więcej ładowanie** jest wyświetlany, można kliknąć go przed kliknięciem przycisku Eksportuj ma więcej danych wyeksportowane.

*Eksportowanie działa tylko w przypadku programu Internet Explorer i Chrome obecnie. Pracujemy nad Dodawanie obsługi innych przeglądarek.*

## <a name="continuous-export"></a>Eksportowanie ciągły

Jeśli chcesz, aby dane przez cały czas wyeksportowane, dzięki czemu można przetwarzać go zewnętrznie, warto rozważyć użycie, [Eksportowanie ciągły](app-insights-export-telemetry.md).

### <a name="power-bi"></a>Power BI

Jeśli chcesz, aby jeszcze bardziej rozbudowane widoki danych, można [eksportować do usługi Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).

## <a name="analytics"></a>Analizy

[Analizy](app-insights-analytics.md) jest bardziej uniwersalny umożliwia analizowanie usługi telemetrycznego przy użyciu języka zaawansowanych kwerend. Używaj, jeśli chcesz połączyć lub obliczyć wyników z metryki lub wykonywanie badań w deph wydajności ostatnio używane do aplikacji. Z drugiej strony za pomocą Eksploratora metryki Chcąc automatycznego odświeżania wykresów na pulpicie nawigacyjnym i alerty.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

*Wszelkie dane nie jest wyświetlane na wykresie.*

* Filtry są stosowane do wszystkich schematów na karta. Upewnij się, że podczas jest skoncentrowanie się na jednym wykresie, nie ustawiono filtr, z wyłączeniem wszystkich danych na innym. 

    Jeśli chcesz ustawić różne filtry różne wykresy, utwórz je w innej karty, zapisz je jako osobne ulubionych. Jeśli chcesz, możesz je przypiąć do pulpitu nawigacyjnego, tak, aby były widoczne obok siebie.

* Jeśli wykres pogrupować właściwość, która nie jest zdefiniowana na metrykę, będą nic się nie na wykresie. Spróbuj wyczyścić "Grupuj według" lub wybierz właściwość grupowania.
* Dane dotyczące wydajności (CPU, Wy i tak dalej) jest dostępna dla języka Java usług sieci web, Windows aplikacji komputerowych [web usług IIS, aplikacji i usług w przypadku zainstalowania monitor stanu](app-insights-monitor-performance-live-website-now.md)i [Usług w chmurze Azure](app-insights-azure.md). Nie jest dostępna dla Azure witryn sieci Web.



## <a name="next-steps"></a>Następne kroki

* [Monitorowanie użycia w aplikacji wniosków](app-insights-overview-usage.md)
* [Używanie funkcji wyszukiwania diagnostyczne](app-insights-diagnostic-search.md)


<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md

 