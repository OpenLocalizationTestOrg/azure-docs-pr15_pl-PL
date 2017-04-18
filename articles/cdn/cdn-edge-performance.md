<properties
    pageTitle="Analizowanie wydajności w Azure CDN | Microsoft Azure"
    description="Analizowanie wydajności węzeł w programie Microsoft Azure CDN. Krawędź wydajności analizy zawiera szczegółowe informacje o ruchu i przepustowość zastosowania CDN."
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

# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Analizowanie wydajności węzeł w programie Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Omówienie
Krawędź wydajności analizy zawiera szczegółowe informacje o ruchu i przepustowość zastosowania CDN. Te informacje można wygenerować popularnych statystycznych, które pozwalają na uzyskanie wglądu, w jaki sposób są aktywów przechowywanych w pamięci podręcznej i dostarczona do klientów. Z kolei pozwala na formularz strategii dotyczących optymalizacji dostarczania zawartości i do określenia, jakie powinny być problemy rozwiązywane lepiej dźwigni CDN. W wyniku nie tylko będzie można zwiększyć wydajność dostarczania danych, ale również można zmniejszyć koszty CDN.

> [AZURE.NOTE] Wszystkie raporty za pomocą notacji UTC-GMT podczas określania daty/godziny.

## <a name="reports-and-log-collection"></a>Raporty i zbieranie dzienników

Przed jego Generowanie raportów na nim należy zbierać dane aktywności CDN przez moduł analizy wydajności krawędzi. Ten proces zbierania występuje po dnia i opisano działania, które miały miejsce poprzedniego dnia. To oznacza, że dane w raporcie statystyczne reprezentują próbki statystyk w danym dniu w czasie została przetworzona, a nie musi zawierać kompletny zestaw danych dla bieżącego dnia. Podstawową funkcją tych raportów jest ocena wydajności. Nie powinny być używane do celów rozliczeń lub Porównaj statystyki liczbowe.

> [AZURE.NOTE] Dane, z którego mają być tworzone raporty krawędzi wydajności analitycznym jest dostępny co najmniej 90 dni.

## <a name="dashboard"></a>Pulpit nawigacyjny

Pulpit nawigacyjny analizy wydajności krawędzi śledzi ruch CDN bieżących i historycznych przy użyciu wykresów i statystyki. Użyj tego pulpitu nawigacyjnego wykrywanie trendów ostatnich i długotrwałe na wydajność sieci CDN ruch dla Twojego konta.

Ten pulpit nawigacyjny składa się z:

* Interaktywny wykres, który umożliwia wizualizacji kluczowych miar i trendów.
* Oś czasu, która zapewnia ujęciu desenie długoterminową podstawowych wskaźników i trendów.
* Kluczowe wskaźniki i informacje statystyczne o tym, jak naszej sieci CDN zwiększa ruchu w witrynie mierzonych według ogólną wydajność, zastosowania i efektywności.

### <a name="accessing-the-edge-performance-dashboard"></a>Uzyskiwanie dostępu do pulpitu nawigacyjnego wydajności krawędzi

1. Z sieci CDN karta profilu kliknij przycisk **Zarządzaj** .

    ![Przycisk Zarządzaj CDN karta profilu](./media/cdn-edge-performance/cdn-manage-btn.png)

    Zostanie otwarty do portalu zarządzania CDN.

2. Umieść wskaźnik myszy na karcie **analizy** , a następnie umieść wskaźnik myszy na menu wysuwane **Analizy wydajności krawędzi** .  Kliknij na **pulpicie nawigacyjnym**.

    Jest wyświetlany na pulpicie nawigacyjnym analizy węzeł krawędzi.

### <a name="chart"></a>Wykres

Pulpit nawigacyjny zawiera wykres, która śledzi Metryka okresie zaznaczona na osi czasu, który pojawia się bezpośrednio poniżej niego.  Wykresy up do ostatniej dwa lata działania CDN osi czasu jest wyświetlany bezpośrednio pod wykresem.

#### <a name="using-the-chart"></a>Używanie wykresu

* Domyślnie będzie wykresie stawka wydajności pamięci podręcznej dla ostatnich 30 dni.
* Ten wykres jest generowany na podstawie danych sortowane codziennie.
* Umieszczenie wskaźnika myszy na dzień na wykres liniowy wskaże daty i wartości metryki w określonym dniu.
* Kliknij pozycję Wyróżnij weekendy, aby przełączyć nakładką uproszczonej szarym pionowe paski, reprezentujących weekendy na wykres. Nakładki na tego typu są przydatne w przypadku identyfikowanie wzorców ruchu w weekendy.
* Kliknij przycisk Widok jednej temu roku do przełączania się między nakładką aktywność w poprzednim roku w okresie czasu na wykres. Ten typ porównania zapewnia wgląd w długoterminowe CDN upodobania. W prawym górnym rogu wykresu zawiera legendą wskazuje kod koloru każdego wykres liniowy.

#### <a name="updating-the-chart"></a>Aktualizowanie wykresu

* Zakres czasu: Wykonaj jedną z następujących czynności:
    * Wybierz odpowiedni region na osi czasu. Wykres zostanie zaktualizowany z danymi, która odpowiada wybranego okresu.
    * Kliknij dwukrotnie wykres, aby wyświetlić wszystkie dostępne dane historyczne maksymalnie dwa lata.
* Metryka: Kliknij ikonę wykresu wyświetlana obok odpowiedniej jednostki metryczne. Wykresu i osi czasu są odświeżane z danymi dla odpowiednich metryki.


### <a name="key-metrics-and-statistics"></a>Kluczowe wskaźniki oraz statystyk

#### <a name="efficiency-metrics"></a>Wskaźniki wydajności

Celem tych metryki jest aby sprawdzić, czy można poprawić efektywność pamięci podręcznej. Główne korzyści wynikające z efektywności pamięci podręcznej są następujące:

* Zmniejszenie obciążenia serwera pochodzenia, co może prowadzić do:
    * Lepszą wydajność serwera sieci web.
    * Zmniejszyć koszty operacyjne.
* Ulepszone funkcje przyspieszenia dostarczenia danych, ponieważ więcej żądań będzie udostępniania bezpośrednio z poziomu sieci CDN.

Pole | Opis
------|------------
Wydajność pamięci podręcznej | Wskazuje wartość procentowa transferowane danych, które zostało obsługiwane z pamięci podręcznej. Ten metryczne środków po buforowanej wersji żądaną zawartość została obsługiwane bezpośrednio z poziomu sieci CDN (serwery graniczne) wypełniania (np. w przeglądarce sieci web)
Częstotliwość trafień | Wskazuje wartość procentowa żądania, które zostało obsłużonych z pamięci podręcznej. Ten Metryka miary, gdy buforowanej wersji żądanej zawartości została dostarczono bezpośrednio z poziomu sieci CDN (serwery graniczne) wypełniania (np. w przeglądarce sieci web).
% bajtów zdalnego — nie konfiguracji pamięci podręcznej | Wskazuje procent ruchu, które zostało obsługiwane z serwerów origin do CDN (serwery graniczne) nie będą buforowane wyniku funkcji obejścia pamięci podręcznej (aparat reguł protokołu HTTP).
% Zdalnego bajtów - wygasła pamięci podręcznej | Wskazuje procent ruchu, które zostało obsługiwane na serwerach origin do sieci CDN (serwery graniczne) wyniku starych zawartości ponownego sprawdzania poprawności.

#### <a name="usage-metrics"></a>Metryki zastosowania

Celem tych metryki jest zapewnienie wgląd w następujących środków wycinania koszt:

* Minimalizowanie kosztów operacyjnych za pośrednictwem sieci CDN.
* Zmniejszenie wydatków CDN za pośrednictwem efektywności pamięci podręcznej i kompresji.

> [AZURE.NOTE] Ruch głośność liczby reprezentują danych, które zostało użyte do obliczeń współczynników i procentów i może być wyświetlane tylko część całkowitą ruch dla klientów.

Pole | Opis
------|------------
Zapisz bajtów się | Wskazuje średnią liczbę bajtów przetransferowanych dla każdego żądania obsługiwane z sieci CDN (serwery graniczne) do nadawcy (np. w przeglądarce sieci web).
Nie szybkość bajt konfiguracji pamięci podręcznej | Wskazuje procent ruchu obsługiwana z sieci CDN (serwery graniczne) do nadawcy (np. w przeglądarce sieci web) nie będą buforowane ze względu na funkcję obejścia pamięci podręcznej.
Stopa skompresowany bajt | Wskazuje procent wysyłania danych z sieci CDN (serwery graniczne) wypełniania (np. w przeglądarce sieci web) w formacie skompresowany.
Bajty wysłane | Wskazuje zakres danych w bajtach dostarczone z sieci CDN (serwery graniczne) do nadawcy (np. w przeglądarce sieci web).  
Bajtów | Wskazuje zakres danych w bajtach wysyłany przez osoby zleceniodawców (np. w przeglądarce sieci web) CDN (serwery graniczne).
Zdalne bajtów | Wskazuje zakres danych w bajtach wysyłany przez serwery origin CDN i klienta do sieci CDN (serwery graniczne).

#### <a name="performance-metrics"></a>Wskaźniki

Celem tych metryki jest śledzenie ogólnej wydajności sieci CDN dla ruchu.

Pole | Opis
------|------------
Szybkość przesyłania | Wskazuje kurs średni, w którym zawartość przeniesiono z CDN żądającego.
Czas trwania | Wskazuje, Średni czas (w milisekundach), zajęło dostarczanie środka trwałego żądającego (np. w przeglądarce sieci web).
Liczby żądań skompresowany | Wskazuje procent trafień, dostarczone z sieci CDN (serwery graniczne) do nadawcy (np. w przeglądarce sieci web) w formacie skompresowany.
Częstotliwość błędów 4xx | Wskazuje, procent trafień wygenerowanych kodu stanu 4xx.
Częstotliwość błędów 5xx | Wskazuje, procent trafień wygenerowanych kodu stanu 5xx.
Trafień | Wskazuje liczbę żądań CDN zawartości.

#### <a name="secure-traffic-metrics"></a>Metryki bezpiecznego ruchu

Celem tych metryki jest umożliwia śledzenie wydajności sieci CDN dla ruchu HTTPS.

Pole | Opis
------|------------
Zabezpiecz pamięć podręczną wydajności | Wskazuje wartość procentowa przeniesione do żądania HTTPS, które zostało obsłużonych z pamięci podręcznej danych. Ten metryczne środków po buforowanej wersji żądaną zawartość została dostarczono bezpośrednio z poziomu sieci CDN (serwery graniczne) wypełniania (np. w przeglądarce sieci web) za pomocą protokołu HTTPS.
Zabezpieczanie szybkość przesyłania | Wskazuje kurs średni, jaką zawartość została przesyłane z sieci CDN (serwery graniczne) wypełniania (przykład serwerów sieci web) za pomocą protokołu HTTPS.
Średni czas trwania bezpiecznego | Wskazuje, Średni czas (w milisekundach), zajęło dostarczanie środka trwałego za pomocą protokołu HTTPS żądającego (np. w przeglądarce sieci web).
Zabezpieczanie trafień | Wskazuje liczbę żądań protokół HTTPS CDN zawartości.
Zabezpieczanie bajtów się | Wskazuje ilość ruchu HTTPS w bajtach dostarczone z sieci CDN (serwery graniczne) do nadawcy (np. w przeglądarce sieci web).

## <a name="reports"></a>Raporty

Każdy raport, w tym module zawiera wykres i statystyki dotyczące wykorzystania przepustowości i ruch dla różnych typów metryki (np kody stanu HTTP, kodów statusu pamięci podręcznej, żądanie użytkownika adres URL itp.). Te informacje mogą służyć do delve szczegółowego do jak zawartość jest obsługiwana dla klientów i dostosować zachowanie CDN, aby zwiększyć wydajność dostarczania danych.

### <a name="accessing-the-edge-performance-reports"></a>Uzyskiwanie dostępu do raportów dotyczących wydajności krawędzi

1. Z sieci CDN karta profilu kliknij przycisk **Zarządzaj** .

    ![Przycisk Zarządzaj CDN karta profilu](./media/cdn-edge-performance/cdn-manage-btn.png)

    Zostanie otwarty do portalu zarządzania CDN.

2. Umieść wskaźnik myszy na karcie **analizy** , a następnie umieść wskaźnik myszy na menu wysuwane **Analizy wydajności krawędzi** .  Kliknij **obiekt dużych HTTP**.

    Zostanie wyświetlony ekran raporty analizy węzeł krawędzi.

Raport | Opis
-------|------------
Podsumowanie dzienne | Umożliwia wyświetlanie dzienny trendów ruchu w określonym okresie. Każdy słupek na tym wykresie odpowiada wybranej daty. Rozmiar paska wskazuje całkowita liczba trafień, które wystąpiły w określonym dniu.
Podsumowanie co godzinę | Umożliwia wyświetlanie co godzinę trendów ruchu w określonym okresie. Każdy słupek na tym wykresie odpowiada jednej godziny w określonym dniu. Rozmiar paska wskazuje całkowita liczba trafień, które wystąpiły podczas danej godziny.
Protokoły | Wyświetla podziału ruchu między protokoły HTTP i HTTPS. Wykres pierścieniowy wskazuje procent trafień, które wystąpiły dla każdego typu Protocol (protokół).
Metody HTTP | Pozwala zorientować szybkie HTTP, które są używane metody żądania danych. Zazwyczaj najczęstsze metody żądania HTTP są GET, głowy i OPUBLIKUJ. Wykres pierścieniowy wskazuje procent trafień, które wystąpiły dla każdego typu Metoda żądania HTTP.
Adresy URL | Zawiera wykresu, który jest wyświetlany górny 10 wymagane adresów URL. Powoduje wyświetlenie paska dla każdego adresu URL. Wysokość paska wskazuje, ile razy wygenerowanych w okresie czasu określonego adresu URL objętych raportu. Statystyki 100 pierwszych zażądał, adresy URL są wyświetlane bezpośrednio pod ten wykres.
Rekordów CNAME | Zawiera wyświetlającego góry 10 rekordów CNAME używana do żądania składników majątku w czasie zakresu raportu w formie wykresu. Statystyki 100 pierwszych żądanych rekordów CNAME są wyświetlane bezpośrednio pod ten wykres.
Miejsc pochodzenia | Zawiera wykresu, który jest wyświetlany górny CDN 10 lub klienta origin serwerów, z których składniki majątku skierowano przez określony czas. Statystyki 100 pierwszych zażądał, aby serwery origin CDN lub klienta są wyświetlane bezpośrednio pod ten wykres. Serwery origin klienta są identyfikowane według nazwy zdefiniowane w polu Nazwa katalogu.
Geo POP | Pokazuje, ile ruchu jest rozsyłana za pośrednictwem określonego punktu z obecności (POP). Trzyliterowego skrótu reprezentuje POP w naszej sieci CDN.
Klienci | Zawiera wykresu, który jest wyświetlany górny 10 klientów, którzy wymagane elementy zawartości przez określony czas. Na potrzeby tego raportu wszystkie żądania, które pochodzą z tego samego adresu IP są uznawane za z tego samego klienta. Statystyki górny 100 klientami są wyświetlane bezpośrednio pod ten wykres. Ten raport jest przydatne w przypadku określenia wzorców aktywności pobierania dla klientów korzystających z góry.
Statusy pamięci podręcznej | Zawiera szczegółowy podział zachowanie pamięci podręcznej może ujawnić metod poprawy ogólnego przez użytkownika końcowego. Ponieważ największą wydajność pochodzi z trafień w pamięci podręcznej, możesz optymalizować szybkości dostarczania danych, minimalizując Chybienia w pamięci podręcznej i trafień w pamięci podręcznej wygasły.
Brak szczegółów | Zawiera wykresu, który jest wyświetlany górny 10 adresów URL dla środków trwałych, dla których aktualności zawartości pamięci podręcznej nie zostało zaznaczone przez określony czas. Statystyki górny 100 adresy URL dla tych typów zasobów są wyświetlane bezpośrednio pod ten wykres.
Szczegóły CONFIG_NOCACHE | Zawiera wykresu, który zawiera adresy URL 10 głównych elementów, które nie były buforowane z powodu konfiguracji sieci CDN klienta. Różne typy zbiorów były obsługiwane bezpośrednio z serwera pochodzenia. Statystyki górny 100 adresy URL dla tych typów zasobów są wyświetlane bezpośrednio pod ten wykres.
Szczegóły UNCACHEABLE | Zawiera wykresu, który jest wyświetlany górny 10 adresów URL dla elementów, których nie można buforować dane nagłówka żądania. Statystyki górny 100 adresy URL dla tych typów zasobów są wyświetlane bezpośrednio pod ten wykres.
Szczegóły TCP_HIT | Zawiera wykresu, który jest wyświetlany górny 10 adresów URL zasoby, które są obsługiwane bezpośrednio z pamięci podręcznej. Statystyki górny 100 adresy URL dla tych typów zasobów są wyświetlane bezpośrednio pod ten wykres.
Szczegóły TCP_MISS | Zawiera wykresu, który jest wyświetlany górny 10 adresów URL dla środków, które mają stan pamięci podręcznej TCP_MISS. Statystyki górny 100 adresy URL dla tych typów zasobów są wyświetlane bezpośrednio pod ten wykres.
Szczegóły TCP_EXPIRED_HIT | Zawiera wykresu, który jest wyświetlany górny 10 adresów URL przestarzałe zasobów, które zostało obsłużonych bezpośrednio z POP. Statystyki górny 100 adresy URL dla tych typów zasobów są wyświetlane bezpośrednio pod ten wykres.
Szczegóły TCP_EXPIRED_MISS | Zawiera wykresu, który jest wyświetlany górny 10 adresów URL stare elementy zawartości, dla których nowa wersja sprzedał mają być pobierane z serwera pochodzenia. Statystyki górny 100 adresy URL dla tych typów zasobów są wyświetlane bezpośrednio pod ten wykres.
Szczegóły TCP_CLIENT_REFRESH_MISS | Zawiera wykres słupkowy, wyświetlającego adresy URL 10 głównych dla zasobów zostały pobrane z serwera pochodzenia na żądanie nie pamięci podręcznej w kliencie. Statystyki górny 100 adresy URL dla tych typów żądania są wyświetlane bezpośrednio pod ten wykres.
Typy żądania klienta | Wskazuje typ żądania, które zostały wprowadzone przez klientów HTTP (na przykład o przeglądarkach). Ten raport zawiera wykresu pierścieniowego, który zawiera właściwe rozwiązanie jako sposób obsługi wezwań. Informacje przepustowość i ruch dla każdego typu żądania są wyświetlane poniżej wykresu.
Agent użytkownika | Zawiera wykres słupkowy, wyświetlając górny wykluczonych agentów użytkownika 10, aby zażądać zawartości przez naszych CDN. Zazwyczaj agenta użytkownika jest przeglądarki sieci web, media player lub przeglądarce telefonu komórkowego. Statystyki górny agentów użytkownika 100 są wyświetlane bezpośrednio pod ten wykres.
Osoby polecające | Zawiera wykres słupkowy, wyświetlanie polecające 10 do zawartości dostępna za pośrednictwem naszej sieci CDN. Zazwyczaj odwołania jest adres URL strony sieci web lub zasobu, która odwołuje się do zawartości. Szczegółowe informacje znajduje się pod wykresem odwołań 100 pierwszych.
Typy kompresji | Zawiera wykres pierścieniowy, który dzieli wymagane elementy zawartości, czy zostały skompresowane przez naszych serwerów krawędzi. Procent aktywów skompresowany jest rozbiciu typ kompresji używanej. Szczegółowe informacje są dostępne pod wykresem dla każdego typu kompresji i stan.
Typy plików | Zawiera wykres słupkowy, wyświetlającego górny typów plików 10, które zażądano za pośrednictwem naszej sieci CDN dla Twojego konta. Na potrzeby tego raportu typ pliku jest zdefiniowana przez rozszerzenia nazwy pliku zasobu i typ multimediów Internet (np HTML \[tekst i html\], htm \[tekst i html\], .aspx \[tekst i html\]itp.). Szczegółowe informacje są dostarczane pod wykresem górny typów plików 100.
Unikatowe plików | Zawiera wykres kreśli całkowitą liczbę unikatowe trwałych skierowano w określonym dniu przez określony czas.
Token uwierzytelniania podsumowania | Zawiera wykres kołowy, który zawiera krótki przegląd na czy wymagane elementy zawartości był chroniony tokenów uwierzytelniania. Elementy zawartości chronionej są wyświetlane na wykresie, zgodnie z wynikami ich próby uwierzytelniania.
Token Auth odmówić szczegóły | Zawiera wykres słupkowy, która umożliwia wyświetlanie Najczęstsze żądania 10, które zostały odrzucone z powodu tokenów uwierzytelniania.
Kody odpowiedzi HTTP | Udostępnia podział kodów stanu HTTP (np 200 OK 403 Dostęp zabroniony, 404 — Nie znaleziono, itp.) którego zostały dostarczone dla klientów HTTP przez naszych serwerów krawędzi. Wykres kołowy pozwala szybko ocenić, jak zostało obsłużonych aktywów. Szczegółowe dane statystyczne są udostępniane dla każdego kodu odpowiedzi poniżej wykresu.
Błędy 404 | Zawiera wykres słupkowy, która umożliwia wyświetlanie Najczęstsze żądania 10, które spowodowało 404 Nie można odnaleźć kodu odpowiedzi.
Błędy 403 | Zawiera wykres słupkowy, która umożliwia wyświetlanie Najczęstsze żądania 10, które spowodowało 403 kod odpowiedzi dostęp zabroniony. 403 kod odpowiedzi zabroniony występuje, gdy żądanie zostało odrzucone przez serwer pochodzenia klienta lub serwera granicznego w naszym POP.
4xx błędów | Zawiera wykres słupkowy, która umożliwia wyświetlanie Najczęstsze żądania 10, które spowodowało kod odpowiedzi w zakresie 400. Ten raport to 403 404 kody odpowiedzi zabroniony i nie można odnaleźć. Zazwyczaj kodu odpowiedzi 4xx występuje, gdy żądanie zostało odrzucone powodu klienta.
Błędy 504 | Zawiera wykres słupkowy, która umożliwia wyświetlanie Najczęstsze żądania 10, które spowodowało 504 kod odpowiedzi limit czasu bramy. 504 kod odpowiedzi limit czasu bramy występuje, gdy przekroczenie limitu czasu podczas serwer proxy HTTP można komunikować się z innego serwera. W przypadku naszych CDN 504 kod odpowiedzi limit czasu bramy zazwyczaj występuje po serwer graniczny nie można nawiązać komunikacji z serwerem pochodzenia klienta.
502 błędów | Zawiera wykres słupkowy, która umożliwia wyświetlanie Najczęstsze żądania 10, które spowodowało 502 kod odpowiedzi Nieprawidłowa brama. 502 kod odpowiedzi Nieprawidłowa brama występuje, gdy wystąpi błąd protokołu HTTP między komputerami serwerów i serwer proxy HTTP. W przypadku naszych CDN 502 kod odpowiedzi Nieprawidłowa brama zwykle występuje, gdy serwer pochodzenia klienta zwraca nieprawidłową odpowiedź na serwerze granicznym. Odpowiedź jest nieprawidłowy, jeśli nie można przeanalizować lub jeśli jest niepełny.
Błędy 5xx | Zawiera wykres słupkowy, która umożliwia wyświetlanie Najczęstsze żądania 10, które spowodowało kod odpowiedzi w zakresie 500.  Ten raport to 502 — Nieprawidłowa brama i 504 kody odpowiedzi limit czasu bramy.

## <a name="see-also"></a>Zobacz też
* [Omówienie Azure CDN](cdn-overview.md)
* [W czasie rzeczywistym statystykę w programie Microsoft Azure CDN](cdn-real-time-stats.md)
* [Zastępuje domyślne zachowanie HTTP za pomocą aparatu reguł](cdn-rules-engine.md)
* [Raporty zaawansowane HTTP](cdn-advanced-http-reports.md)
