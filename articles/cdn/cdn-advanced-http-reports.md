<properties
    pageTitle="Azure CDN zaawansowane raporty protokołu HTTP | Microsoft Azure"
    description="Zaawansowane raporty protokołu HTTP w programie Microsoft Azure CDN. Te raporty zawierają szczegółowe informacje na CDN aktywności."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="advanced-http-reports-in-microsoft-azure-cdn"></a>Zaawansowane raporty protokołu HTTP w programie Microsoft Azure CDN

## <a name="overview"></a>Omówienie

W tym dokumencie omówiono zaawansowane HTTP raportowania w programie Microsoft Azure CDN. Te raporty zawierają szczegółowe informacje na CDN aktywności.

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Uzyskiwanie dostępu do zaawansowanych raportów HTTP

1. Z sieci CDN karta Profil kliknij przycisk **Zarządzaj** .

    ![Przycisk Zarządzaj CDN karta profilu](./media/cdn-advanced-http-reports/cdn-manage-btn.png)

    Zostanie otwarty do portalu zarządzania CDN.

2. Umieść wskaźnik myszy na karcie **analizy** , a następnie umieść wskaźnik myszy na menu wysuwane **Zaawansowane raporty protokołu HTTP** .  Kliknij na **platformie dużych HTTP**.

    ![Portal zarządzania CDN — menu Zaawansowane raporty](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)

    Zostaną wyświetlone opcje raportu.

## <a name="geography-reports-map-based"></a>Raporty geograficznych (opartych na mapie)

Istnieje pięć raportów, które korzystają z mapę w celu wskazania regionów, z których żąda zawartości. Te raporty są Map świata, mapy Stanów Zjednoczonych, Kanadzie mapy, mapa Europy i Azji Pacyfiku mapy.

Każdy raport opartych na mapie rangę geograficzne podmioty (to znaczy, krajów, Stany i prowincji) według procent trafień, które pochodzą z tego regionu. Ponadto mapy znajduje się ułatwiając lokalizacji, z których żąda zawartości. Może to zrobić przy kolorowanie według wartości każdego regionu według ilości żądanie w danym regionie. Jaśniejsze Zacieniowane regiony wskazują dolnym żądanie dla zawartości, gdy obszarów ciemniejszych wskazać wyższego poziomu żądanie dla zawartości.

Szczegółowe informacje ruchu i przepustowości dla każdego regionu znajduje się bezpośrednio poniżej mapy. Dzięki temu możesz wyświetlić całkowitą liczbę trafień, procent trafień, łączną liczbę danych transferowane (w GB), a wartość procentowa danych przeniesione dla każdego regionu. Wyświetlanie opisu dla każdego z tych wskaźników. Ponadto po umieszczeniu wskaźnika nad obszar (to znaczy kraju, stanu lub prowincji), nazwę i procent trafień, które wystąpiły w regionie będą wyświetlane jako etykietka.

Krótki opis znajduje się poniżej dla każdego typu raportów opartych na mapie geograficznych.

Nazwa raportu | Opis
------------|------------
Mapa świata | Ten raport pozwala na wyświetlanie na całym świecie żądanie dla zawartości CDN. Każdego kraju jest kodowane kolorami na mapie świata, aby wskazać, procent trafień, które pochodzą z tego regionu.
Mapy Stanów Zjednoczonych | Ten raport pozwala na wyświetlanie żądanie dla zawartości CDN w Stanach Zjednoczonych. Każde Państwo jest kodowane w tym mapy, aby wskazać, procent trafień, które pochodzą z tego regionu.
Mapa Kanada | Ten raport pozwala na wyświetlanie żądanie dla zawartości CDN w Kanadzie. Każdy prowincji jest kodowane w tym mapy, aby wskazać, procent trafień, które pochodzą z tego regionu.
Mapa Europy | Ten raport pozwala na wyświetlanie żądanie dla zawartości CDN w Europie. W tym mapy, aby wskazać, procent trafień, które pochodzą z tego regionu jest kodowane każdego kraju.
Mapa Pacyfiku Azji | Ten raport pozwala na wyświetlanie żądanie dla zawartości CDN w Azji. W tym mapy, aby wskazać, procent trafień, które pochodzą z tego regionu jest kodowane każdego kraju.

## <a name="geography-reports-bar-charts"></a>Raporty geograficznych (wykresy słupkowe)

Istnieją dwa dodatkowe raporty zawierające informacje statystyczne według Geografia są miasta i krajów góry. Te raporty klasyfikowanie miasta i krajów, odpowiednio według liczbę trafień, które pochodzą z regionów. Po Generowanie tego typu raportu, wykresu słupkowego wskaże górny 10 miasta lub kraje, które zażądał zawartości przez określonej platformy. Ten wykres słupkowy pozwala szybko ocenić regiony, w których wygenerować największa liczba żądania dla zawartości.

Po lewej stronie wykresu (y) wskazuje, ile razy podczas określonego regionu. Bezpośrednio poniżej wykresu (osi x) spowoduje znalezienie etykietę dla każdej z góry regionów 10.

### <a name="using-the-bar-charts"></a>Za pomocą wykresy słupkowe

* Po zatrzymaniu wskaźnika myszy na pasku, nazwę i całkowita liczba trafień, które wystąpiły w regionie będzie wyświetlany jako etykietka narzędzia.
* Etykietka narzędzia miasta raportu służy do identyfikowania miasta przez jego nazwę, stan/prowincja i symbol kraju.
* Jeśli nie można określić miasta lub regionu (to znaczy, stan/prowincja), z której pochodzi żądanie, następnie go będzie wskazuje, że są nieznane. Jeśli kraj jest nieznany, a następnie dwa znaki zapytania (to znaczy?), będą wyświetlane.
* Raport może obejmować metryki "Europa" lub "Region Azji i Pacyfiku." Te elementy nie są przeznaczone do zawierają informacje statystyczne o wszystkich adresów IP w regionach. Zamiast nich są stosowane tylko do żądań pochodzących z adresów IP, które są rozproszone na Europie lub Azji i Pacyfiku, a nie do określonego miasta lub kraju.

Danych, który został użyty do wygenerowania na wykresie słupkowym mogą być wyświetlane pod nim. Można znaleźć przenoszone całkowitą liczbę trafień, procent trafień, ilości danych (w GB), a wartość procentowa danych zostaną przeniesione z góry 250 regionów. Wyświetlanie opisu dla każdego z tych wskaźników.

Krótki opis jest dostępne dla obu typów raportów poniżej.

Nazwa raportu | Opis
------------|------------
Miasta | Ten raport określa pozycję miast według liczbę trafień, które pochodzą z tego regionu.
Górny krajów | Ten raport określa pozycję krajów według liczbę trafień, które pochodzą z tego regionu.

## <a name="daily-summary"></a>Podsumowanie dzienne

Raport Podsumowanie dzienne umożliwia wyświetlanie całkowita liczba trafień i danych przesyłanych w konkretnej platformy codziennie. Te informacje można szybko wykrycia CDN wzorców aktywności. Na przykład ten raport może ułatwić wykrywanie dni doświadczonych wyższy lub niższy oczekiwanych ruch.

Po Generowanie tego typu raportu, wykresu słupkowego zapewni wizualne wskazanie ilości żądanie specyficzne dla platformy doświadczonych codziennie okresie objętego raportem. Będzie dokonywać, wyświetlając pasek dla każdego dnia w raporcie. Na przykład wybierając okres o nazwie "Ostatni tydzień" wygeneruje wykres słupkowy z paskami siedem. Każdy słupek oznacza całkowita liczba trafień doświadczonych w danym dniu.

Po lewej stronie wykresu (y) wskazuje, ile razy wystąpił na określony dzień. Bezpośrednio poniżej wykresu (osi x), możesz znaleźć etykiety, która wskazuje datę (Format: YYYY-MM-DD) dla każdego dnia uwzględnione w raporcie.

> [AZURE.TIP] Po umieszczeniu wskaźnika myszy na pasku całkowita liczba trafień, które wystąpiły w określonym dniu będzie wyświetlany jako etykietka narzędzia.

Danych, który został użyty do wygenerowania na wykresie słupkowym mogą być wyświetlane pod nim. Można znaleźć całkowita liczba trafień i ilość danych przesyłanych (w GB) dla każdego dnia omówione w raporcie.

## <a name="by-hour"></a>Przez godzinę

Raport przez godzinę umożliwia wyświetlanie całkowita liczba trafień i danych przesyłanych w konkretnej platformy co godzinę. Te informacje można szybko wykrycia CDN wzorców aktywności. Na przykład ten raport może ułatwić wykrywanie okresy w ciągu dnia występują wyższej lub niższej niż oczekiwane ruch.

Po Generowanie tego typu raportu, wykresu słupkowego zapewni wizualne wskazanie ilości żądanie specyficzne dla platformy wystąpił co godzinę w okresie objętych raportu. Będzie dokonywać, wyświetlając pasek dla każdej godziny objętych raportu. Na przykład wybierając 24-godzinnego okresu wygeneruje wykres słupkowy z paskami 24. Każdy słupek oznacza całkowita liczba trafień doświadczonych ciągu danej godziny.

Po lewej stronie wykresu (y) wskazuje, ile razy wystąpił na określoną godzinę. Bezpośrednio poniżej wykresu (osi x), spowoduje znalezienie etykietę, która wskazuje, Data/Godzina (Format: YYYY-MM-DD hh: mm) dla każdej godziny uwzględnione w raporcie. Czas jest zgłaszany przy użyciu formatu 24-godzinnego i jest określona przy użyciu strefy czasowej UTC-GMT.

> [AZURE.TIP] Po umieszczeniu wskaźnika myszy na pasku całkowita liczba trafień, które wystąpiły podczas danej godziny będzie wyświetlany jako etykietka narzędzia.

Danych, który został użyty do wygenerowania na wykresie słupkowym mogą być wyświetlane pod nim. Można znaleźć całkowita liczba trafień i ilość danych przesyłanych (w GB) dla każdej godziny objętych raportu.

## <a name="by-file"></a>W pliku

Raport przez plik umożliwia wyświetlenie liczby żądanie i ruch poniesionych w konkretnej platformy najczęściej poszukiwanych zasobów. Po Generowanie tego typu raportu, wykresu słupkowego generowanych na górnym 10 najczęściej poszukiwanych składników majątku w określonym okresie.

> [AZURE.NOTE] Na potrzeby tego raportu adresy URL CNAME krawędź są konwertowane na ich równoważne adresy URL CDN. Dzięki temu dokładne zgadza dla całkowita liczba trafień skojarzone z środka trwałego niezależnie od tego, czy CDN lub krawędź adres URL CNAME używany do niego poprosić.

Po lewej stronie wykresu (y) wskazuje liczbę żądań dla każdego środka trwałego w określonym okresie. Bezpośrednio poniżej wykresu (osi x), spowoduje znalezienie etykietę, która wskazuje nazwę pliku dla każdego z góry 10 wymagane elementy zawartości.

Danych, który został użyty do wygenerowania na wykresie słupkowym mogą być wyświetlane pod nim. Znajdziesz następujące informacje dla każdego z góry 250 wymagane elementy zawartości: ścieżka względna, liczba trafień, procent trafień, ilości danych przekazywane (w GB), a wartość procentowa danych zostaną przeniesione.

## <a name="by-file-detail"></a>Według szczegółów pliku

Raport dotyczący według szczegółów pliku umożliwia wyświetlenie liczby żądanie i ruch poniesionych w konkretnej platformy dla określonych elementów zawartości. Na samej górze ten raport jest opcja Szczegóły pliku dla. Opcja zawiera listę najczęściej poszukiwanych aktywów na platformie wybranego. Aby wygenerować raport przez szczegółowy plik, będzie konieczne wybierz żądane trwały z opcją szczegóły pliku dla. Po upływie którego wykresu słupkowego wskaże ilość dzienny żądanie, który wygenerował ją w określonym okresie.

Po lewej stronie wykresu (y) wskazuje całkowita liczba żądań, że środka trwałego wystąpił w określonym dniu. Bezpośrednio poniżej wykresu (osi x), możesz znaleźć etykiety, która wskazuje datę (Format: YYYY-MM-DD) dla których CDN Zgłoszono żądanie dla elementu.

Danych, który został użyty do wygenerowania na wykresie słupkowym mogą być wyświetlane pod nim. Można znaleźć całkowita liczba trafień i ilość danych przesyłanych (w GB) dla każdego dnia omówione w raporcie.

## <a name="by-file-type"></a>Według typu pliku

Raport według typu pliku umożliwia wyświetlenie liczby żądanie i ruch poniesione według typu pliku. Po Generowanie tego typu raportu, wykresu pierścieniowego wskaże procent trafień wygenerowane przez górny typy plików 10.

> [AZURE.TIP] Po zatrzymaniu wskaźnika myszy nad wycinek wykresu pierścieniowego Internet typ nośnika że typ pliku zostanie wyświetlona jako etykietka.

Dane użyte do generowania wykresu pierścieniowego mogą być wyświetlane pod nim. Można znaleźć typu multimediów rozszerzenia/Internet nazwę pliku, całkowita liczba trafień, procent trafień, ilość przesyłanych danych (w GB), a wartość procentowa danych przeniesione dla każdego typu pliku góry 250.

## <a name="by-directory"></a>W katalogu

Raport przez katalogów umożliwia wyświetlenie liczby żądanie i ruch poniesionych w konkretnej platformy dla zawartości z określonego katalogu. Po Generowanie tego typu raportu, wykresu słupkowego wskaże całkowita liczba trafień wygenerowane przez zawartości w górnym katalogów 10.

### <a name="using-the-bar-chart"></a>Za pomocą na wykresie słupkowym

* Umieść wskaźnik na pasku, aby wyświetlić ścieżkę względną do odpowiedniego katalogu.
* Zawartości przechowywanej w podfolderze katalogu nie zlicza podczas obliczania żądanie przez katalog. Obliczenie zależy od wyłącznie liczbę żądań wygenerowanych dla zawartości przechowywanej w katalogu rzeczywiste.
* Na potrzeby tego raportu adresy URL CNAME krawędź są konwertowane na ich równoważne adresy URL CDN. Dzięki temu dokładne zgadza dla wszystkich danych statystycznych skojarzone z środka trwałego niezależnie od tego, czy CDN lub krawędź adres URL CNAME używany do niego poprosić.

Po lewej stronie wykresu (y) wskazuje całkowitą liczbę żądań zawartości przechowywanych w katalogach z 10 pierwszych. Każdy słupek na wykresie reprezentuje katalogu. Schemat kolorów umożliwia pasek do katalogu wymienionych w sekcji górnej 250 pełny katalogów są zgodne.

Danych, który został użyty do wygenerowania na wykresie słupkowym mogą być wyświetlane pod nim. Znajdziesz następujące informacje dla każdego z katalogów góry 250: ścieżka względna, liczba trafień, procent trafień, ilości danych przekazywane (w GB), a wartość procentowa danych zostaną przeniesione.

## <a name="by-browser"></a>W przeglądarce

Raport przez przeglądarki umożliwia wyświetlanie, jakie przeglądarki była używana do żądania zawartości. Po Generowanie tego typu raportu, wykresu kołowego wskaże Procent żądań obsłużonych przez górny przeglądarki 10.

### <a name="using-the-pie-chart"></a>Za pomocą wykresu kołowego

* Umieść wskaźnik na wycinek wykresu kołowego, aby wyświetlić nazwy i wersji przeglądarki.
* Na potrzeby tego raportu kombinacjami unikatowe wersja przeglądarki i jest traktowany jako innej przeglądarki.
* Wycinek o nazwie "Inne" oznacza wartość procentową żądań obsłużonych przez wszystkich innych przeglądarek i wersji.

Dane, które został użyty w celu wygenerowania wykresu kołowego mogą być wyświetlane pod nim. Można znaleźć numer typu i wersji przeglądarki, całkowita liczba trafień i procent trafień dla każdego z góry 250 przeglądarki.

## <a name="by-referrer"></a>Przez odnośnika

Raport odwołania, pozwala wyświetlać główne osoby polecające do zawartości na platformie zaznaczonego. Odwołania wskazuje hostname, z której został wygenerowany żądanie. Po Generowanie tego typu raportu, wykresu słupkowego wskaże ilość żądanie (to znaczy trafień) wygenerowane przez główne osoby polecające 10.

Po lewej stronie wykresu (y) wskazuje całkowita liczba żądań, że środka trwałego wystąpił dla każdego odwołania. Każdy słupek na wykresie reprezentuje odnośnika. Schemat kolorów umożliwia pasek, aby odwołania, wymienionych w sekcji górnej 250 odwołania są zgodne.

Danych, który został użyty do wygenerowania na wykresie słupkowym mogą być wyświetlane pod nim. Można znaleźć adres URL, całkowita liczba trafień i procent trafień wygenerowane z każdego polecające 250.

## <a name="by-download"></a>Pobranie

Raport przez pobieranie umożliwia analizowanie desenie pobierania dla najczęściej poszukiwanych zawartości. Na początek raport zawiera wykres słupkowy, że porównanie próba pobrane z pobrania zakończone górny 10 trwałych wymagane. Każdy słupek jest kodowane kolorami według czy jest próby pobierania (niebieski) lub wykonanych pobierania (zielony).

> [AZURE.NOTE] Na potrzeby tego raportu adresy URL CNAME krawędź są konwertowane na ich równoważne adresy URL CDN. Dzięki temu dokładne zgadza dla wszystkich danych statystycznych skojarzone z środka trwałego niezależnie od tego, czy CDN lub krawędź adres URL CNAME używany do niego poprosić.

Po lewej stronie wykresu (y) wskazuje nazwę pliku dla każdego z góry 10 wymagane elementy zawartości. Bezpośrednio poniżej wykresu (osi x) spowoduje znalezienie etykiety, które wskazują całkowitą liczbę prób wykonane pliki do pobrania.

Bezpośrednio poniżej na wykresie słupkowym następujące informacje zostaną wyświetlone górny 250 trwałych wymagane: ścieżka względna (łącznie z nazwą pliku), liczbę został pobrany do ukończenia, liczbę ich zażądano i procent żądania, które spowodowało zakończona pobieranie.

> [AZURE.TIP] Nasze CDN nie zostanie poinformowany przez klienta HTTP (to znaczy w przeglądarce) po środka trwałego całkowitym pobraniu. W wyniku mamy do obliczania, czy zasób całkowitym pobraniu według kodów stanu i zakres bajtów żądania. Najpierw możemy poszukaj podczas wprowadzania tych obliczeń jest, czy żądanie wyników w kodzie 200 OK stanu. Jeśli tak, następnie możemy Spójrz na zakres bajtów żądania, aby upewnić się, że będą one obejmować całej zawartości. Na koniec możemy porównanie ilość danych przesyłanych do rozmiaru żądanego elementu. Jeśli przekazania danych jest równa lub większa niż rozmiar pliku i żądania zakres bajtów są odpowiednie dla tego zasobu, przewaga będą zliczane jako ukończone pobierania.
>
>Ze względu na interpretive rodzaj ten raport należy mieć na uwadze następujące punkty, które mogą zmieniać spójności i dokładności tego raportu.
>
>* Wzorce ruchu dokładnie nieprzewidywalny po agentów użytkownika zachowywać się inaczej. Może to powodować złożonym Pobierz wyniki, których wartość jest większa niż 100%.
>* Zasoby, które korzystają z protokołu HTTP stopniowe pobieranie nie mogą być dokładnie reprezentowane przez ten raport. Jest to spowodowane użytkowników poszukiwania w inne miejsca w klipie wideo.

## <a name="by-404-errors"></a>Przez błędami 404

Raport przez błędy 404 umożliwia wskazują typ zawartości, która generuje najwięcej 404 Nie można odnaleźć kody stanu. Na początek raport zawiera wykres słupkowy trwałych 10 głównych, dla których został zwrócony 404 Nie można odnaleźć kodu stanu. Ten wykres słupkowy porównuje całkowita liczba żądań żądaniami, które spowodowało 404 Nie można odnaleźć kodu stanu dla tych aktywów. Każdy słupek jest kodowane kolorami. Żółty pasek jest używana w celu wskazania, że żądania spowodowało 404 Nie można odnaleźć kodu stanu. Czerwony pasek służy do wskazywania całkowita liczba żądań dla elementu.

> [AZURE.NOTE] Na potrzeby tego raportu Pamiętaj o następujących kwestiach:
>
>* Przewaga reprezentuje wniosku o zawartości niezależnie od kodu stanu.
>* Adresy URL CNAME krawędź są konwertowane na ich równoważne adresy URL CDN. Dzięki temu dokładne zgadza dla wszystkich danych statystycznych skojarzone z środka trwałego niezależnie od tego, czy CDN lub krawędź adres URL CNAME używany do niego poprosić.

Po lewej stronie wykresu (y) wskazuje nazwę pliku dla każdej z nich górny 10 aktywów wymagane, które spowodowało 404 Nie można odnaleźć kodu stanu. Bezpośrednio poniżej wykresu (osi x) spowoduje znalezienie etykiety, które wskazują całkowita liczba żądań i liczba żądań, które spowodowało 404 Nie można odnaleźć kodu stanu.

Bezpośrednio poniżej na wykresie słupkowym następujące informacje zostaną wyświetlone górny 250 trwałych wymagane: ścieżka względna (łącznie z nazwą pliku), liczba żądań, które spowodowało 404 Nie można odnaleźć kodu stanu, całkowitą liczbę godzin, które zażądano elementu i procent żądania, które spowodowało 404 Nie można odnaleźć kodu stanu.

## <a name="see-also"></a>Zobacz też
* [Omówienie Azure CDN](cdn-overview.md)
* [W czasie rzeczywistym statystykę w programie Microsoft Azure CDN](cdn-real-time-stats.md)
* [Zastępuje domyślne zachowanie HTTP za pomocą aparatu reguł](cdn-rules-engine.md)
* [Analizowanie wydajności](cdn-edge-performance.md)
