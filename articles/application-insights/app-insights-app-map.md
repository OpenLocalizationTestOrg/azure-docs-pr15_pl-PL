<properties 
    pageTitle="Mapowanie aplikacji w aplikacji wniosków | Microsoft Azure" 
    description="Wizualnej prezentacji zależności między składnikami aplikacji etykietą kluczowych wskaźników wydajności i alerty." 
    services="application-insights" 
    documentationCenter=""
    authors="SoubhagyaDash" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2016" 
    ms.author="awills"/>
 
# <a name="application-map-in-application-insights"></a>Mapowanie aplikacji w aplikacji wniosków

W [Visual Studio aplikacji wniosków](app-insights-overview.md)mapowanie aplikacji jest wizualny układ relacji zależności składników aplikacji. Każdy składnik wyświetlane kluczowe wskaźniki wydajności, takie jak ładowanie, wydajności, błędów i alerty, aby pomagają identyfikować każdy składnik przyczyną problemu z wydajnością lub błąd. Możesz kliknąć pozycję za pośrednictwem z każdego składnika do bardziej szczegółowe diagnostyki, zarówno z wniosków aplikacji oraz aplikacji korzysta z usługi Azure — Azure diagnostyki, takie jak zalecenia Advisor bazy danych SQL.

Przykład innych wykresach można przypiąć mapowanie aplikacji do pulpitu nawigacyjnego Azure, gdzie jest w pełni funkcjonalny. 

## <a name="open-the-application-map"></a>Otwórz mapę aplikacji

Otwórz mapę z karta Przegląd aplikacji:

![Otwórz aplikację mapy](./media/app-insights-app-map/01.png)

![Aplikacja mapy](./media/app-insights-app-map/02.png)

Wyświetla mapy:

* Sprawdza dostępności
* Składniki po stronie klienta (monitorować za JavaScript SDK)
* Składniki po stronie serwera
* Zależności składniki klienta i serwera

Można rozwijać i zwijać współzależności łączenie grup:

![Zwiń](./media/app-insights-app-map/03.png)
 
Jeśli masz dużą liczbą zależności dla jednego typu (SQL, HTTP itp.), może być wyświetlany zgrupowane. 


![zgrupowane współzależności](./media/app-insights-app-map/03-2.png)
 
 
## <a name="spot-problems"></a>Problemy z miejsca

Każdy węzeł ma wskaźniki wydajności odpowiednich, takich jak ładowanie, wydajności i błąd stawek dla tego składnika. 

Ikony Ostrzeżenie Wyróżnij potencjalnych problemów. Pomarańczowy ostrzeżenie oznacza, że w żądania, liczbę wyświetleń stron lub połączeń współzależności występują błędy. Czerwony oznacza, że stawki błąd powyżej 5%.


![Błąd ikony](./media/app-insights-app-map/04.png)

 
Aktywne powiadamia też Pokaż w górę: 


![aktywne alertów](./media/app-insights-app-map/05.png)
 
Jeśli korzystasz z platformy SQL Azure, jest wyświetlana jako ikona, wyjaśnieniem, kiedy istnieją zalecenia, w jaki sposób można zwiększyć wydajność. 


![Zalecenie Azure](./media/app-insights-app-map/06.png)

Kliknij dowolną ikonę, aby uzyskać więcej szczegółów:


![zalecenie Azure](./media/app-insights-app-map/07.png)
 
 
## <a name="diagnostic-click-through"></a>Kliknij diagnostyczne za pośrednictwem

Każdy węzłów na mapie oferuje docelowych kliknij za pośrednictwem diagnostyki pakietu. Dostępne opcje różnią się w zależności od typu węzła.

![Opcje serwera](./media/app-insights-app-map/09.png)

 
Dla składników, które znajdują się w Azure opcje obejmują bezpośrednie łącza do nich.


## <a name="filters-and-time-range"></a>Filtry i przedziału czasu

Domyślnie mapy zawiera podsumowanie wszystkich dostępnych dla zakresu czasu wybranych danych. Ale filtrowania, aby uwzględnić tylko nazwy operacji lub zależności.

* Nazwa operacji: Ta opcja uwzględnia zarówno wyświetleń stron i typów wniosków po stronie serwera. Po wybraniu tej opcji mapa zawiera kluczowy wskaźnik wydajności w węźle po stronie klienta i serwera tylko zaznaczonego operacji. Pokazuje zależności o nazwie w kontekście tych określone czynności.
* Nazwa podstawowa współzależności: Ta opcja uwzględnia zależności po stronie przeglądarki AJAX i zależności po stronie serwera. Jeśli raport telemetrycznego zależność niestandardowa przy użyciu interfejsu API TrackDependency one zostaną wyświetlone również tutaj. Możesz wybrać zależności w celu pokazania na mapie. Pamiętaj, że w tym momencie to będzie Filtruj żądania po stronie serwera lub widoki strony po stronie klienta.


![Ustaw filtry](./media/app-insights-app-map/11.png)

 
 
## <a name="save-filters"></a>Zapisywanie filtrów

Aby zapisać filtry, które zostały zastosowane, kliknij pozycję Przypnij widoku filtrowanego na [pulpicie nawigacyjnym](app-insights-dashboards.md).


![Przypinanie do pulpitu nawigacyjnego](./media/app-insights-app-map/12.png)
 


## <a name="feedback"></a>Opinie

Zapoznaj się z [przekazywanie opinii za pomocą opcji portalu opinii](app-insights-get-dev-support.md).


![Obraz MapLink-1](./media/app-insights-app-map/13.png)


