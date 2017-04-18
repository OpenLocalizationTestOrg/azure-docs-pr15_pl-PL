<properties 
    pageTitle="Wprowadzenie do aplikacji wniosków z języka Java w Zaćmienie" 
    description="Dodawanie wydajności i użycia monitorowania do witryny Java z wniosków aplikacji za pomocą wtyczki Zaćmienie" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/02/2016" 
    ms.author="awills"/>
 
# <a name="get-started-with-application-insights-with-java-in-eclipse"></a>Wprowadzenie do aplikacji wniosków z języka Java w Zaćmienie

SDK wniosków aplikacji wysyła telemetrycznego aplikacji sieci web Java tak, aby można analizować użycie i wydajność. Zaćmienie wtyczkę w przypadku aplikacji wniosków automatycznie instaluje zestawu SDK w projekcie, aby wyjść z telemetrycznego pola, a także interfejs API, który służy do zapisu telemetrycznego niestandardowe.   


## <a name="prerequisites"></a>Wymagania wstępne

Obecnie wtyczki działa dla środowiska Maven i dynamicznego projektów sieci Web w Zaćmienie. ([Dodawanie aplikacji wniosków z innymi typami projektu Java][java].)

Potrzebujesz:

* Oracle JRE 1,6 lub nowszy
* Subskrypcję usługi [Microsoft Azure](https://azure.microsoft.com/). (Może rozpoczynać się od [bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/)).
* [Zaćmienie IDE dla deweloperów Estonia Java](http://www.eclipse.org/downloads/)indygo lub nowszym.
* Windows 7 lub nowszy lub Windows Server 2008 lub nowszego

## <a name="install-the-sdk-on-eclipse-one-time"></a>Zainstaluj zestaw SDK na Zaćmienie (raz)

Musisz zrobić to jeden raz na komputerze. Ten krok jest instalowana zestaw narzędzi, które następnie można dodać zestawu SDK do każdego dynamiczne projektu sieci Web.

1. W Zaćmienie kliknij pozycję Pomoc, zainstaluj nowe oprogramowanie.

    ![Pomoc, instalowanie oprogramowania nowy](./media/app-insights-java-eclipse/0-plugin.png)

2. Zestaw SDK znajduje się w http://dl.windowsazure.com/eclipse w obszarze zestaw narzędzi Azure. 
3. Wyczyść pole wyboru, **skontaktuj się z wszystkich witryn aktualizacji...**

    ![Dla aplikacji wniosków SDK, wyczyść kontaktu Aktualizuj wszystkie witryny](./media/app-insights-java-eclipse/1-plugin.png)

Wykonaj pozostałe kroki dla każdego języka Java projektu.

## <a name="create-an-application-insights-resource-in-azure"></a>Tworzenie zasób wniosków aplikacji platformy Azure

1. Zaloguj się do [portalu Azure](https://portal.azure.com).
2. Tworzenie nowego zasobu wniosków aplikacji.  

    ![Kliknij przycisk + i wybierz pozycję wniosków aplikacji](./media/app-insights-java-eclipse/01-create.png)  
3. Ustaw typ aplikacji Java aplikacji sieci web.  

    ![Wypełnianie nazwę, wybierz Java aplikacji sieci web, a następnie kliknij przycisk Utwórz](./media/app-insights-java-eclipse/02-create.png)  
4. Znajdź klucz oprzyrządowania nowego zasobu. Konieczne będzie wkrótce Wklej to w projekcie kodu.  

    ![W nowej Przegląd zasobów kliknij polecenie Właściwości, a następnie skopiuj klucz oprzyrządowania](./media/app-insights-java-eclipse/03-key.png)  


## <a name="add-application-insights-to-your-project"></a>Dodawanie aplikacji wniosków do projektu

1. Dodawanie aplikacji wniosków z menu kontekstowego Java projektu sieci web.

    ![W nowej Przegląd zasobów kliknij polecenie Właściwości, a następnie skopiuj klucz oprzyrządowania](./media/app-insights-java-eclipse/02-context-menu.png)

2. Wklej klucz oprzyrządowania uzyskanego od Azure portal.

    ![W nowej Przegląd zasobów kliknij polecenie Właściwości, a następnie skopiuj klucz oprzyrządowania](./media/app-insights-java-eclipse/03-ikey.png)


Klucz są wysyłane wraz z każdego elementu telemetrycznego i informuje wniosków aplikacji, aby ją wyświetlić w zasobu.

## <a name="run-the-application-and-see-metrics"></a>Uruchom aplikację i Zobacz metryka

Uruchamianie aplikacji.

Wróć do zasobu wniosków aplikacji platformy Microsoft Azure.

Dane żądania HTTP pojawią się na karta Przegląd. (Jeśli nie istnieje, poczekaj chwilę i kliknij przycisk Odśwież).

![Odpowiedź serwera, liczone żądania i błędy ](./media/app-insights-java-eclipse/5-results.png)
 

Kliknij dowolny wykres, aby wyświetlić szczegółowe metryki. 

![Żądanie liczniki według nazwy](./media/app-insights-java-eclipse/6-barchart.png)


[Dowiedz się więcej na temat metryki.][metrics]

 

I podczas przeglądania właściwości żądania, można zobaczyć zdarzenia telemetrycznego skojarzone z nim, takie jak żądania i wyjątki.
 
![Wszystkie śledzenia dla tego żądania](./media/app-insights-java-eclipse/7-instance.png)


## <a name="client-side-telemetry"></a>Telemetrycznego po stronie klienta

Na karta Szybki Start kliknij kod Get monitorowanie stron sieci web: 

![Na swojej karta Przegląd aplikacji wybierz pozycję Szybki Start, wyświetlony kod monitorowanie stron sieci web. Skopiuj skrypt.](./media/app-insights-java-eclipse/02-monitor-web-page.png)

Wstawianie wstawkę kodu w nagłówku pliki HTML.

#### <a name="view-client-side-data"></a>Wyświetlanie danych po stronie klienta

Otwórz zaktualizowaną stron sieci web i używać ich. Zaczekaj minutę lub dwie, a następnie wróć do wniosków aplikacji i otwórz karta zastosowania. (Od karta Przegląd przewiń w dół i kliknij pozycję użycie).

Strony widoku, użytkownik i sesja metryki pojawią się na karta zastosowania:

![Sesje, użytkowników i liczba wyświetleń strony](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[Dowiedz się więcej o konfigurowaniu telemetrycznego po stronie klienta.][usage]

## <a name="publish-your-application"></a>Publikowanie aplikacji

Teraz można publikować aplikacji na serwerze, użyj Poinformuj osoby, a także czujki telemetrycznego wyświetlane w portalu.

* Upewnij się, że ustawienia zapory umożliwiają aplikacji z wysyłką telemetrycznego następujące porty:

 * DC.Services.VisualStudio.com:443
 * DC.Services.VisualStudio.com:80
 * F5.Services.VisualStudio.com:443
 * F5.Services.VisualStudio.com:80


* Na serwerach systemu Windows należy zainstalować:

 * [Microsoft Visual C++ do dystrybucji](http://www.microsoft.com/download/details.aspx?id=40784)

    (Dzięki temu liczniki wydajności).

## <a name="exceptions-and-request-failures"></a>Wyjątki i niepowodzenia żądań

Nieobsługiwany wyjątki są zbierane automatycznie:

![](./media/app-insights-java-eclipse/21-exceptions.png)

Zbieranie danych na inne wyjątki, masz dwie opcje:

* [Wstawianie połączenia do TrackException w kodzie](app-insights-api-custom-events-metrics.md#track-exception). 
* [Instalowanie agenta Java na serwerze](app-insights-java-agent.md). Określanie metody, które chcesz obserwować.


## <a name="monitor-method-calls-and-external-dependencies"></a>Monitorowanie metody połączenia i zależności zewnętrznych

[Instalowanie agenta Java](app-insights-java-agent.md) do logowania określone metody wewnętrznych i wywołań za pośrednictwem JDBC, z danymi chronometraż.


## <a name="performance-counters"></a>Liczniki wydajności

Na swojej karta Przegląd przewiń w dół i kliknij **serwerów** . Pojawi się zakres liczników wydajności.


![Przewiń w dół do kliknij kafelka serwerów](./media/app-insights-java-eclipse/11-perf-counters.png)

### <a name="customize-performance-counter-collection"></a>Dostosowywanie wydajności licznika

Aby wyłączyć zbiór standardowy zestaw liczników wydajności, Dodaj poniższy kod w obszarze węzła głównego pliku ApplicationInsights.xml:

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>

### <a name="collect-additional-performance-counters"></a>Zbieranie liczników wydajności dodatkowe

Możesz określić liczniki dodatkowe zbierane.

#### <a name="jmx-counters-exposed-by-the-java-virtual-machine"></a>Liczniki JMX (udostępniane przez środowiska Java)

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>

*   `displayName`— Nazwa wyświetlana w portalu wniosków aplikacji.
*   `objectName`— JMX nazwę obiektu.
*   `attribute`Atrybut nazwę obiektu JMX do pobrania
*   `type`(opcjonalnie) — typ atrybutu JMX obiektu:
 *  Wartość domyślna: prosty typ przykład int lub początkowym.
 *  `composite`: danych liczników wydajności jest w formacie "Attribute.Data"
 *  `tabular`: danych liczników wydajności jest w formacie wiersza tabeli



#### <a name="windows-performance-counters"></a>Liczniki wydajności systemu Windows

Każdy [Licznik wydajności systemu Windows](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) jest członkiem kategorii (w taki sam sposób, że pole jest członkiem klasy). Kategorie mogą być globalnego, można mieć numerowana lub nazwane wystąpienia.

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>

*   displayName — Nazwa wyświetlana w portalu wniosków aplikacji.
*   NazwaKategorii — kategorii licznika wydajności (obiekt wydajności), z którą jest skojarzony ten licznik wydajności.
*   counterName — nazwę licznika wydajności.
*   nazwa_wystąpienia — nazwę wystąpienia kategorii licznika wydajności lub ciąg pusty (""), jeśli kategoria zawiera jedno wystąpienie. Jeśli CategoryName proces, a licznika chcesz zebrać pochodzi z bieżącego procesu maszyny wirtualnej Java, na której aplikacji jest uruchomiony, określ `"__SELF__"`.

Liczniki wydajności są wyświetlane jako niestandardowe metryki w [Eksploratorze metryki][metrics].

![](./media/app-insights-java-eclipse/12-custom-perfs.png)


### <a name="unix-performance-counters"></a>Liczniki wydajności systemu UNIX

* [Instalowanie collectd z wtyczkę wnioski aplikacji](app-insights-java-collectd.md) uzyskanie szeroką gamę systemu i sieci danych.

## <a name="availability-web-tests"></a>Dostępność testy sieci web

Wnioski aplikacji można sprawdzić witryny sieci Web w regularnych odstępach czasu, aby sprawdzić, czy jest i odpowiada również. [Aby skonfigurować][availability], przewiń w dół do kliknij pozycję Dostępność.

![Przewiń w dół, kliknij pozycję Dostępność, a następnie dodaj Web test](./media/app-insights-java-eclipse/31-config-web-test.png)

Zostanie wyświetlony wykresy czasu odpowiedzi oraz powiadomienia e-mail, jeśli awarii witryny.

![Przykład test sieci Web](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[Więcej informacji o dostępności testy sieci web.][availability] 

## <a name="diagnostic-logs"></a>Dzienniki diagnostyczne

Jeśli korzystasz z Logback lub Log4J (wersja 1.2 lub 2.0) do śledzenia, możesz mieć do dzienników automatycznie wysyłane do aplikacji wniosków miejsce, w którym można eksplorować i wyszukiwanie od nich.

[Dowiedz się więcej o dzienniki diagnostyczne][javalogs]

## <a name="custom-telemetry"></a>Niestandardowe telemetrycznego 

Wstawianie kilka wierszy kodu w aplikacji sieci web języka Java, aby dowiedzieć się, co robią użytkowników z nim lub diagnozowanie problemów. 

Kod można wstawić zarówno na stronie sieci web JavaScript i Java po stronie serwera.

[Dowiedz się więcej o telemetrycznego niestandardowe][track]



## <a name="next-steps"></a>Następne kroki

#### <a name="detect-and-diagnose-issues"></a>Wykrywanie i diagnozowanie problemów

* [Dodawanie telemetrycznego klienta sieci web] [ usage] uzyskanie telemetrycznego wydajności w kliencie sieci web.
* [Ustawianie testów web] [ availability] aby upewnić się, pozostaje aplikacji i odpowiadać na żywo.
* [Wyszukiwanie zdarzeń i dzienników] [ diagnostic] diagnozowanie problemów.
* [Przechwytywanie śledzenia Log4J lub Logback][javalogs]

#### <a name="track-usage"></a>Śledzenie użycia

* [Dodawanie telemetrycznego klienta sieci web] [ usage] monitor liczbę wyświetleń stron i metryk podstawowe użytkownika.
* [Śledzenie zdarzeń niestandardowych i metryk] [ track] Aby dowiedzieć się, jak używać aplikacji zarówno klienta i serwera.


<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 