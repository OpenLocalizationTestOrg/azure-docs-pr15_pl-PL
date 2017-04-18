<properties
    pageTitle="Cortana analizy rozwiązanie szablonu Playbook do prognozowania żądanie energii | Microsoft Azure"
    description="Szablon rozwiązanie ze analizy Cortana Microsoft, który ułatwia prognozy żądanie firmy narzędzie energii."
    services="cortana-analytics"
    documentationCenter=""
    authors="ilanr9"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/24/2016"
    ms.author="ilanr9;yijichen;garye"/>

# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Cortana analizy rozwiązanie szablonu Playbook do prognozowania żądanie energii  

## <a name="executive-summary"></a>Podsumowanie dla kierownictwa  

W ciągu ostatnich kilku lat Internet czynności (IoT), alternatywnych źródeł energii i duży danych zostały scalone w celu utworzenia ogromną szanse sprzedaży w domenie narzędzie i energii. W tym samym czasie narzędzie i sektor energii całego przejrzane zużycie spłaszczanie z konsumentów wymagających ulepszone sposoby sterowania ich wykorzystanie energii. W związku z tym narzędzia i firmy inteligentne siatki są w potrzeba wprowadzaniu nich zmian i odnawiać się. Ponadto wiele siatek narzędzia i power stają się przestarzałe lub bardzo kosztów utrzymania i zarządzać nimi. W ostatnim roku pracował zespołu wielokrotnie pozyskiwaniu w domenie energii. Podczas tych pozyskiwaniu mają wystąpił większości przypadków, w których narzędzia lub niezależnych dostawców oprogramowania (niezależnych dostawców oprogramowania) zostały odnaleźć do prognozowania przyszłych energii żądanie. Te prognozy odtwarzanie ważną rolę w ich działalności obecne i przyszłe i stały podstawę dla różnych przypadków użycia. Należą prognozy obciążenia power krótkoterminowy i długoterminowy, obrotu, równoważenia obciążenia, optymalizacja siatki itp. Duży danych i metody zaawansowane analizy AA takich jak szkoleń komputera (ML) są kluczowe enablers produkcji dokładne i niezawodne prognozy.  

W tym playbook możemy umieścić razem firm i analitycznych wskazówki wymagane do pomyślnego rozwoju i wdrażania żądanie energii prognozy rozwiązanie. Te proponowane wskazówki służą narzędzia, danych naukowców i inżynierów danych ustalając rozwiązań w pełni operationalized, oparte na chmurze, prognozowania żądanie. Dla firm, które rozpoczynają właśnie ich big data i podróży zaawansowanej analizy takie rozwiązanie może oznaczać początkowe siewnego w ich długotrwałe strategii inteligentne siatki.

>[AZURE.TIP] Aby pobrać diagramu, który zawiera omówienie architektury tego szablonu, zobacz [Architektura szablon rozwiązanie analizy Cortana do prognozowania żądanie energii](cortana-analytics-architecture-demand-forecasting-energy.md).  

## <a name="overview"></a>Omówienie  

W tym dokumencie opisano firm, dane i technicznych aspektów za pomocą analizy Cortana i w określonym Azure maszynowego nauki (AML) implementacji i wdrażanie rozwiązań prognozowania energii. Dokument składa się z trzech głównych części:  

1. Opis firm  
2. Opis danych  
3. Implementacja techniczne

Część **Opis firm** przedstawiono proporcji firm, które należy zrozumieć i należy rozważyć przed podjęciem decyzji inwestycji. Wyjaśniono, jak kwalifikować się problemu biznesowego wykonywanego zapewniające przewidywanych analizy i nauka maszynowego faktycznie skutecznych i jest stosowane. Dokument dodatkowo omówiono podstawy maszynowego uczenia się i jak są one używane do rozwiązywania problemów prognozowania energii. Ją opisano wymagania wstępne i kryteria kwalifikacji w przypadku użycia. Kilka przykładowych za pomocą procesów sądowych i zagadnienia również dostępnymi scenariuszy.

Dane są głównym składnika dla dowolnego maszynowego uczenia rozwiązanie. **Opis danych** część tego dokumentu dotyczy kilka ważnych aspektów danych. Ją opisano rodzaj danych, które są potrzebne do prognozowania energii, wymagania odnośnie do jakości danych i istnieją zazwyczaj źródeł danych. Również wyjaśnić, jak nieprzetworzonych danych służy do przygotowywania funkcje danych, które rzeczywiście sterują części modelowania.

Trzecia część dokumentu obejmuje proporcji **Implementacji technicznej** rozwiązania. Funkcja techniczny i modelowanie mają procesu nauki danych i w związku z tym jest opisano niektóre szczegółowo. Obejmuje pojęcie usług sieci web, które są ważne pojazdu wdrożenia chmury analizy przewidywanych rozwiązań. Możemy również utworzyć taki konspekt architekturę typowe rozwiązania operationalized zakończenia do końca.

Ponadto dokument zawiera materiały źródłowe, którego można użyć w celu uzyskania dodatkowych opis domeny i technologii.

Należy pamiętać, że firma Microsoft nie zamierzasz obejmuje w tym dokumencie proces szczegółowego nauki danych aspektami matematycznych i technicznych. Szczegółowe informacje można znaleźć w [dokumentacji Azure ML](http://azure.microsoft.com/services/machine-learning/) i [blogów](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Docelowych odbiorców   
Przeznaczony dla tego dokumentu jest biznesowych i technicznych pracowników, którzy chcą uzyskanie wiedzy i zrozumienia maszynowego uczenia oparte rozwiązaniach i jak są one używane przede wszystkim w domenie prognozowania energii.

Naukowców danych można również korzystać z tego dokumentu, aby lepiej zrozumieć proces wysokiego poziomu, który dyski wdrożenia energii prognozowania rozwiązanie do czytania. W tym kontekście go można również ustalenie planu bazowego warto i punkt początkowy, aby uzyskać więcej szczegółowych i zaawansowanych materiałów.

### <a name="industry-trends"></a>Trendów branżowych  
W ciągu ostatnich kilku lat IoT, alternatywnych źródeł energii i duży danych zostały scalone w celu utworzenia ogromną szanse sprzedaży w obszarze Narzędzia i energii. W tym samym czasie sektorów całej energii i narzędzie przejrzane zużycie spłaszczanie z konsumentów wymagających ulepszone sposoby sterowania ich wykorzystanie energii.

Wiele narzędzia i firmy inteligentne energii mają zostały pioneering [inteligentne siatki](https://en.wikipedia.org/wiki/Smart_grid) wdrażając liczbę użyć spraw, które ułatwiają korzystanie z danych generowanych siatki. Wiele przypadków użycia obracać się wokół cech właściwych produkcji energii elektrycznej: nie skumulowaną ani pomijając przechowywane jako magazyn. Tak co to jest wyprodukowano musi być używany. Narzędzia, które powinny być bardziej wydajną pracę należy po prostu prognozy zużycie energii ponieważ którego będą nadać im większe możliwości **i saldo zapotrzebowania**, a więc zapobiegania wibracjom energii, **zmniejszenie emisji gazu cieplarnianego**i koszt kontrolki.

Gdy rozmawiać kosztów, to innym ważnym aspektem, który jest cena. Nowe możliwości handlu power między narzędzia wprowadzenia w potrzeba **prognozy przyszłych żądanie i przyszłych ceny energii elektrycznej**. Może to pomóc określić ich wielkości produkcji firmy.

Użycie wyrazu "inteligentne", możemy faktycznie odwołują się do siatki, który można dowiedzieć się i wprowadź przewidywań. Można go przewiduje sezonowy zmian w zużycie, jak również **przewidzieć sytuacje tymczasowe przeciążeń i automatycznie dopasować do niego**. Zdalne regulacji zużycie (za pomocą tych inteligentne metr), można obsługiwać zlokalizowanym przeciążeń sytuacji. **Najpierw przewidywania, a następnie przetwarzania**siatki sprawia, że sam sprawniejszą w czasie.

W dalszej części tego dokumentu, firma Microsoft poświęcona określonych rodziny przypadków użycia, obejmujące prognozowania z przyszłości krótkim i długotrwałe żądanie energii. Pracujemy pracy w następujących obszarach kilka miesięcy i uzyskały niektórych wiedzy i umiejętności, umożliwiająca nam w celu uzyskania wyników ocen branży. Inne przypadków użycia zostaną objęte także w dokumencie w najbliższej przyszłości.

## <a name="business-understanding"></a>Opis firm

### <a name="business-goals"></a>Celów biznesowych
Cel **Pokaz energii** jest pokazują typowe analizy przewidywanych a maszynowego uczenia rozwiązanie, które mogą być rozmieszczone w ramce bardzo krótki czas. W szczególności naszych bieżącego skoncentrować się na włączanie rozwiązań prognozy energii żądanie tak, aby jej wartość firm można szybko zrealizowanych i użyć po. Informacje zawarte w tym playbook mogą ułatwić wykonywanie następujących celów klienta:
-   Godzina krótka wartość maszynowego uczenia podstawie rozwiązanie
-   Możliwość rozwiń wdrożenia pilotażowego za pomocą liter do innych przypadków użycia lub do szerszego zakresu według ich potrzeb biznesowych
-   Szybko uzyskać pakiet analizy Cortana produktu wiedzy

Do tych celów Pamiętaj ten playbook ma na celu dostarczania firm i wiedzy technicznej, które są pomocne w osiągnięciu tych celów.

### <a name="power-load-and-demand-forecasting"></a>Ładowanie Power i prognozowania żądanie
W obszarze energii może być wiele sposobów, w których żądanie prognozowania może pomóc w rozwiązywaniu problemów krytycznego, biznesowego. W rzeczywistości prognozowania żądanie można uznać za podstawę dla większości przypadków użycia core w branży. Na ogół firma Microsoft należy rozważyć, czy dwa typy prognoz żądanie energii: krótkim i długoterminową. Każdy z nich może służyć innych celów i wykorzystywać różne metody. Główna różnica między tymi dwoma jest horyzont prognozowania, co oznacza przedział czasu w przyszłości, dla której możemy prognozy.

#### <a name="short-term-load-forecasting"></a>Krótka ładowania terminów prognozowania
W ramach żądanie energii krótki termin ładowanie prognozowania (STLF) jest definiowana jako zagregowane ładowania, który jest prognozowanych w najbliższej przyszłości w różnych części siatki (lub siatce jako całość). W tym kontekście krótkim jest zdefiniowany horyzont czasowy w zakresie 1 godzinę do 24 godzin. W niektórych przypadkach horyzont 48 godzin jest również możliwe. W związku z tym STLF jest często w przypadku użycia operacyjne siatki. Oto kilka przykładów STLF zmiennych przypadków użycia:
-   I zapotrzebowania równoważenia
-   Obsługa handlowych Power
-   Tworzenie rynku (cena power ustawienie)
-   Optymalizacja operacyjne siatki
-   [Odpowiedź na żądanie](https://en.wikipedia.org/wiki/Demand_response)
-   Szczytowy żądanie prognozowania
-   Zarządzanie po stronie żądanie
-   Równoważenia obciążenia i przeciążeń zapobiegania
-   Dłuższy czas ładowania prognozowania
-   Wykrywanie błędów i anomalii
-   Alokacja maksymalna ograniczenie i bilansowanie 

STLF model głównie zależą od najbliższego przeszłości (ostatniego dnia lub tygodnia) dane zużycia i używanie prognozowanych temperatury jako ważne predykcyjne. Uzyskiwanie temperatury dokładności prognozy przez godzinę i w górę do 24 godzin staje się mniej wezwanie teraz dni. Te modele są mniej poufne do sezonowy długoterminowe trendy zużycie i.

Rozwiązania SLTF prawdopodobnie także do generowania dużej liczby połączeń przewidywania (żądania obsługi) od ich jest wywołania na godzinę, a w niektórych przypadkach nawet w przypadku wyższa częstotliwość. Jest również bardzo popularne, aby wyświetlić zagnieżdżenia miejsce, w którym każdej poszczególnych podstacji lub przekształcania jest reprezentowane jako modelu autonomicznej, dlatego liczba żądań przewidywania jeszcze większą.

#### <a name="long-term-load-forecasting"></a>Dłuższy czas ładowania prognozowania
Cel: długich terminów obciążenia prognozowania (LTLF) jest prognoz żądanie power horyzont czasowy, począwszy od 1 tydzień na wiele miesięcy (i w niektórych przypadkach dla liczby lat). Ten zakres horyzont przeważnie ma zastosowanie do planowania i przypadki użycia inwestycji.

W przypadku scenariuszy długoterminowe jest dostęp do danych wysokiej jakości, które obejmuje zakres wielu lat (minimalne 3 lata). Tych modeli wyodrębniania sezonowości wzorców danych historycznych zwykle i korzysta z zewnętrznego predicators takie jako desenie pogody i klimatu.

Ważne wskazać, że jest dłużej horyzont prognozowania, mniejsze dokładności prognozy może być jest. W związku z tym ważne jest da niektórych przedziału ufności wraz z rzeczywista prognozy umożliwiająca ludzi do współczynnika odmianę możliwe do procesu planowania.

Ponieważ głównie planowania scenariusz zużycie LTLF choć oczekiwanym znacznie niższych poziomach przewidywania (w porównaniu STLF). Widzimy czy zazwyczaj te przewidywania osadzony w wizualizacji narzędzi, takich jak program Excel lub PowerBI i uruchomić ręcznie przez użytkownika.

### <a name="short-term-vs-long-term-prediction"></a>Krótki termin a początkowym przewidywania terminów
W poniższej tabeli porównano STLF i LTLF w odniesieniu do najważniejszych atrybuty:

|Atrybut|Ładowanie krótkim prognozy|Dłuższy czas ładowania prognozy|
|---|---|---|
|Prognozowanie horyzont|Od 1 godzinę do 48 godzin|Od 1 do 6 miesięcy lub więcej|
|Dokładności danych|Co godzinę|Co godzina lub codziennie|
|Typowe zastosowanie spraw|<ul><li>Żądanie/dostaw równoważenia</li><li>Wybierz godzinę prognozowania</li><li>Odpowiedź na żądanie</li></ul>|<ul><li>Dłuższy czas planowania</li><li>Składniki majątku siatki planowania</li><li>Planowanie zasobów</li></ul>|
|Typowe zmienne predykcyjne|<ul><li>Dnia lub tygodnia</li><li>Godzina dnia</li><li>Temperatura co godzinę</li></ul>|<ul><li>Miesiąc roku</li><li>Dzień miesiąca</li><li>Temperatura długoterminową i klimatu</li></ul>|
|Zakres danych historycznych|Dwa lub trzy lata, przez które danych|Wartość 5 do 10 lat danych|
|Typowa precyzja|MAPE * 5% lub niższej|MAPE * 25% lub niższej|
|Częstotliwość prognozy|Co godzinę lub raz na 24 godziny|Wyprodukowano po miesięczny, kwartalny i roczny|
\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) — średniego średnia błędu procentu

Jak wynika z tej tabeli, należy dość odróżniać krótkim i długoterminową prognozowania scenariusze, jak te reprezentują różne potrzebami i może być rozmieszczania i desenie zużycie.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Przykład użycia w przypadku 1: eSmart systemów — Optymalizacja przeciążeń
Istotne znaczenie [inteligentne siatki](https://en.wikipedia.org/wiki/Smart_grid) jest dynamicznie i nieustannie optymalizowanie i dopasowywanie zmienianie wzorców zużycie. Zużycie energii może mieć negatywny wpływ krótkoterminowy zmiany, które głównie spowodowanych zmianami temperatury (*np.*więcej power jest używana dla warunku air lub ogrzewania). W tym samym czasie zużycie energii jest również wpływ trendy długoterminowe. Mogą one obejmować sezonowości efekty, krajowych dni wolne, długoterminowe wzrostu zużycia i nawet ekonomicznych czynników, takich jak indeks dla klientów indywidualnych, cena oil i PKB.

W tym przypadku [eSmart](http://www.esmartsystems.com/) chce wdrażania rozwiązania opartego na chmurze, umożliwiającą przewidywania skłonność: sytuacja na dowolnej danej podstacji siatki. W szczególności eSmart przetransponowane do identyfikowania podstacji, które mogą przeciążeń w ciągu godziny, więc można podjąć natychmiastowej działanie w celu uniknięcia lub rozwiązania tej sytuacji.

Dokładne i szybkie wykonywanie przewidywania wymaga wykonania trzech modeli przewidywanych:
-   Czas modelu terminu, który umożliwia prognozowanie zużycia energii w każdym podstacji podczas następnego kilka tygodni lub miesięcy
-   Krótkim modelu, który pozwala przewidywania sytuacja na danym podstacji podczas przez godzinę
-   Temperatura modelu, który zawiera prognozowania przyszłych temperatury przez wielu scenariuszy

Celem długoterminowe modelu jest klasyfikowanie podstacjami przez ich skłonność do przeciążeń (podane zdolności przenoszenia power) podczas następnego tygodnia lub miesiąca. Umożliwia tworzenie skrócony wykaz podstacji służących jako dane wejściowe do przewidywania krótkoterminowy. Temperatura jest ważne predykcyjnych długoterminowe modelu, istnieje potrzeba stale przygotowania prognoz temperatury wielu scenariusz i kanał je jako dane wejściowe do długotrwałych modelu. Następnie wywoływana jest prognozy krótkim przewidywanie, które podstacji prawdopodobnie przeciążeń przez godzinę.

Krótkoterminowy i długoterminowy modele są wdrażane indywidualnie dla każdego podstacji. Praktyczne wykonanie tych modeli wymaga obszernego aranżacji. Na uzyskanie większej dokładności przewidywania w krótkim, bardziej szczegółowego modelu jest przeznaczony dla każdej godziny dnia. Modele te są wykonywane co godzinę i zakończenia wykonanie za kilka minut, aby umożliwić wystarczająco dużo czasu na odpowiedź i wykonać czynności zapobiegania, w razie potrzeby. Ten zbiór modeli jest uaktualniany przez okresowe przekwalifikowania przy użyciu najnowszych danych.

Więcej informacji o tym przypadku można znaleźć [tutaj](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Używanie kryteriów kwalifikacji wielkości liter — wymagania wstępne
Główny siła analizy Cortana jest zaawansowanych możliwości wdrażanie i skalowanie rozwiązań wyśrodkowany na komputerze nauki. Jest przeznaczony do obsługi tysięcy przewidywań, które są wykonywane jednocześnie. Spełniają zmienianie deseniu zużycie automatycznie można skalować. Rozwiązanie fokus znajduje się w związku z tym, w dokładność i obliczeniowej. Na przykład firmę jest zainteresować wytwarzania energii dokładne Prognoza przez godzinę, a dla każdej godziny dnia. Z drugiej strony, firma Microsoft interesuje mniej odpowiedzi na pytanie, dlaczego żądanie jest przewidziane za jego jest (samego modelu będą zajmować tego).

Dlatego jest zdać sobie sprawę, że nie wszystkie przypadki użycia i firm możliwe będzie skuteczne rozwiązanie problemów za pomocą komputera szkoleń.

Cortana analizy i nauka machine może być bardzo wydajny w rozwiązaniu problemu danej firmy, gdy są spełnione następujące warunki:
-   Problem biznesowych w kształcie dłoni **przewidywanych** charakter. Przykład wielkości liter Użyj przewidywanych to firmy narzędzie, którego chcesz przewidywanie power obciążenie danego podstacji podczas przez godzinę. Z drugiej strony analizowania i klasyfikowania sterowniki historycznych żądanie będzie **opisową** charakter i w związku z tym mniej stosowane.
-   Jakie należy podjąć po prognozowania jest dostępna jest wyczyść **ścieżkę akcji** . Na przykład przewidywania przeciążeń na podstacji podczas przez godzinę może wyzwolić aktywne akcji zmniejszenie obciążenia skojarzonego z tym podstacji oraz więc potencjalnie zapobiegania przeciążeń.
-   Przypadek użycia reprezentuje **Typowe typ problemu** takie aby rozwiązać go można przygotować do rozwiązywania innych podobnych za pomocą spraw.
-   Klienta można ustawić **cele skład jakościowy i** w celu zademonstrowania implementacji pomyślnego rozwiązania. Na przykład, warto cel ilościowych dla energii prognozy będzie progu wymaganej dokładności (*np.*maksymalnie 5% błąd jest dozwolone) lub gdy przewidywania podstacji przeciążeń następnie dokładności (stopa PRAWDA wyniki dodatnie) i odwołanie (zakres PRAWDA wyniki dodatnie) powinny być powyżej określonego poziomu. Te cele powinna pochodzić z celów biznesowych klienta.
-   Istnieje wyczyść **scenariuszy integracji** z przepływem pracy firm firmy. Na przykład prognozy obciążenia podstacji mogą być zintegrowane centrum sterowania siatki, aby umożliwić działania zapobiegania przeciążeń.
-   Klient ma gotowy do pomocy technicznej przypadków użycia (Zobacz więcej w następnej sekcji, **Jakość danych**, w tym playbook) przy użyciu **danych przy użyciu odpowiedniej jakości** .
-   Klienta obejmuje Architektura danych na chmurze lub **nauki opartej na chmurze komputera**, w tym Azure ML i innych składników pakietu analizy Cortana.
-   Odbiorca chce ustanowić **przepływu danych kompleksowego** tego urządzenia dostarczanie danych do chmury na bieżąco i gotowego do **operationalize** rozwiązanie.
-   Klient jest gotowy do **przeznaczoną zasobów** kto będzie aktywnie biorą podczas początkowej implementacja pilotażowa tak, aby wiedzy i praw własności rozwiązanie, które mogą zostać przeniesione do klienta po pomyślnym zdaniu.
-   Zasób klienta powinny być **specjalistycznej danych profesjonalny**, najlepiej naukowca danych.

Kwalifikacji przypadków użycia, na podstawie powyższych kryteriów można znacznie zwiększyć stawki sukcesu przypadków użycia i ustanawiania dobre beachhead stosowania przypadków użycia w przyszłości.

### <a name="cloud-based-solutions"></a>Rozwiązania oparte na chmurze
Pakiet analizy Cortana Azure to zintegrowane środowisko, który znajduje się w chmurze. Wdrożenie rozwiązania zaawansowanej analizy w środowisku chmury zawierające świadczeń dla firm i w tym samym czasie może oznaczać Zmień duży dla firm, które nadal korzystać z lokalnego rozwiązania IT. W obszarze energii istnieje trendu wyczyść migracji stopniowej operacji w chmurze. Ten trend idzie w parze wraz z rozwoju inteligentne siatki opisane powyżej w **Trendów branżowych**. Zgodnie z tym playbook jest ograniczony do rozwiązania opartego na chmurze w domenie energii, należy wyjaśnić zalet i inne zagadnienia: Wdrażanie rozwiązania opartego na chmurze.

Być może Największą zaletą rozwiązania opartego na chmurze jest koszt. Jako rozwiązanie wykorzystuje składników wdrożony w chmurze, istnieje poświęcić koszty lub KWS (koszt z towarów sprzedawanych) składnika koszty skojarzone z nim. Oznacza to, że istnieje potrzeba inwestycji w sprzęt, oprogramowania i konserwacja IT i w związku z tym jest znaczące zmniejszenie ryzyka firm.

Inną zaletą ważne jest struktury kosztów płatne rozwiązań opartych na chmurze. Oparte na chmurze serwerów do korzystania z komputera lub magazynowania można wdrożony i skalowany na podstawie tylko jako wymagane. Jest to korzyści koszt rozwiązania opartego na chmurze.

Na koniec jest potrzebę poświęcania konserwacji IT lub rozwoju przyszłych infrastruktury, jak to wszystko jest częścią oferująca opartej na chmurze. W tym zakresie Cortana analizy pakiet zawiera najlepsze w usługach zajęć i jego mapy drogowej zachowuje rozwijających się. Nowe funkcje, składniki i funkcje są stale wprowadzane i rozwijać.

Dla firmy, która jest uruchamiana tylko jego przejścia w chmurze zdecydowanie zalecamy podjęcie podejście stopniowe wdrażając mapy drogowej migracji chmury. Naszym zdaniem pakietu Narzędzia i firmy w domenie energii przypadków użycia, które zostały omówione w tym playbook reprezentują doskonałe możliwości pilotażowe rozwiązań przewidywanych analizy w chmurze.

#### <a name="business-case-justification-considerations"></a>Zagadnienia biznesowe uzasadnienie wielkości liter
W większości przypadków klienta mogą Cię zainteresować wprowadzania biznesowe uzasadnienie przypadek użycia danej rozwiązania opartego na chmurze i nauka komputera znajdują się składniki ważne. W przeciwieństwie do rozwiązania, w przypadku rozwiązania opartego na chmurze składnik poświęcić kosztów są minimalne i najczęściej elementy kosztów są skojarzone z rzeczywistym. Jeśli chodzi o wdrażanie energii prognozowania rozwiązania w pakiecie analizy Cortana, wiele usług można zintegrować z jednym typowe struktury kosztów. Na przykład baz danych (*np.*platformy SQL Azure) może służyć do przechowywania nieprzetworzonych danych, a następnie rzeczywista prognoz Azure ML służy do prognozowania usług. W tym przykładzie struktury kosztów mogą obejmować miejsca do magazynowania i składniki transakcji.

Jedną z drugiej strony, powinien mieć dobrze zapoznać się z wartości biznesowej operacyjnych żądanie energii prognozowania (krótkim lub długim terminie). W rzeczywistości jest zdać sobie sprawę wartości biznesowej każdej operacji prognozy. Na przykład dokładnie prognozowania obciążenia power następnego 24 godzin zapobiec nadprodukcji lub można zapobiec overloads na siatce i to można określić pod względem oszczędności finansowe codziennie.

Podstawowa formuła do obliczania finansowe zaletą żądanie prognozy rozwiązaniem jest: ![podstawową formułę do obliczenia finansowe zaletą żądanie prognozy rozwiązanie](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Ponieważ pakiet analizy Cortana udostępnia model cennik płatne, nie jest konieczne dla naliczania kosztu stałego składnik, aby ta formuła. Ta formuła może zostać obliczona na podstawie dzienny, miesięczny lub roczny.

Bieżący pakiet analizy Cortana i ML Azure cennik planów można znaleźć [tutaj](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Proces opracowywania rozwiązania
Cykl opracowywania żądanie energii prognozowania, rozwiązanie najczęściej 4 faz, w które udzielamy za pomocą technologii opartej na chmurze i usług w pakiecie analizy Cortana.

Jest to pokazano na poniższym rysunku:

![Cykl inteligentne siatki](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

Akapit opisuje proces ten krok 4:

1.  **Zbieranie danych** — dowolne rozwiązanie zaawansowanej analizy podstawie zależy od danych (zobacz **Opis danych**). W szczególności jeśli chodzi o przewidywanych analizy i prognozowania, firma Microsoft oparte na bieżąco, dynamicznych przepływu danych. W przypadku energii prognozowania żądanie, tych danych może być pochodzące bezpośrednio z miernikami inteligentne lub już być zagregowana na lokalna baza danych. Możemy również są oparte na innych zewnętrznych źródeł danych, takich jak pogody i temperatury. Ten trwającego przepływu danych musi orchestrated, zaplanowane i przechowywane. [Factory Azure danych](http://azure.microsoft.com/services/data-factory/) (ADF) jest najważniejszą metodą roboczą naszych głównym do wykonywania tego zadania.
2.  **Modelowanie** — energii dokładne i niezawodne prognoz, jedną należy opracować (pociągu) i obsługa doskonałe modelu, że używa danych historycznych oraz wyodrębnia zrozumiałej i przewidywanych trendy w danych. Obszar nauki komputera (ML) występują zostały szybko rośnie z bardziej zaawansowane algorytmy opracowywane regularnie. Azure ML Studio zawiera obsługi użytkowników, który ułatwia wykorzystanie najbardziej zaawansowane algorytmy ML w ramach przepływu pracy nad. Przepływ pracy przedstawiono na diagramie intuicyjny i zawiera przygotowywanie danych, wyodrębniania funkcji, modelowania i oceny modelu. Użytkownik może uwzględniał setki różnych modeli, które znajdują się w tym środowisku. Na koniec tej fazy naukowca danych mają model pracy, który jest w pełni szacowane i gotowe do wdrożenia.

    Poniższy diagram jest zapoznać się z ilustracją typowe przepływu pracy:

    ![Modelowanie przepływów pracy](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)

3.  **Wdrożenie** — z modelem pracy w kształcie dłoni, następnym krokiem jest wdrożenie. W tym miejscu modelu jest konwertowany na usługi sieci web, która udostępnia RESTful interfejsu API, które można jednocześnie wywołać w Internecie z różnych klientów zużycie. Azure ML umożliwia łatwe wdrażania modelu bezpośrednio z Studio ML Azure za pomocą jednego kliknięcia przycisku. Cały proces wdrażania tak się dzieje w obszarze Ustawienia zaawansowane. To rozwiązanie może automatyczne skalowanie spotkania wymagane zużycie.

4.  **Zużycie** — w tej fazy faktycznie udzielamy w celu określenia przewidywań użyć modelu prognozowania. Zużycie można przeprowadzić z aplikacji użytkownika*(, pulpit nawigacyjny)*lub bezpośrednio od systemu operacyjnego, takich jak żądanie/dostawy równoważenia systemu lub rozwiązanie optymalizacji siatki. Wiele przypadków użycia można przeprowadzić z pojedynczy model.

## <a name="data-understanding"></a>Opis danych
Po obejmujące zagadnienia business (zobacz **Opis Business**) żądanie energii prognozowania rozwiązanie, możemy przystąpić do omówienia części danych. Dowolne rozwiązanie przewidywanych analizy zależy od danych. Do prognozowania żądanie energii było oparte na danych historycznych zużycie z różnymi poziomami szczegółowości. Historycznych danych jest używany jako surowiec. Będzie ją przejść dokładnej analizy, w którym naukowca danych określi zmienne predykcyjne (nazywane także funkcje), które można umieścić w modelu, który ostatecznie wygeneruje wymagane prognozy.

W dalszej części tej sekcji możemy opisano różne kroki i zagadnienia związane z opis danych i jak przełączyć go do formularza można używać.

### <a name="the-model-development-cycle"></a>Cykl opracowywania modelu
Produkcji dobre prognozowania modeli wymaga pewnych przygotowań staranne i planowania. Podziału proces modelowania do wielu kroków i skoncentrowanie się na jednym kroku w danej chwili można znacznie zwiększyć wynik całego procesu.

Na poniższym diagramie przedstawiono, jak proces modelowania można podzielić na wiele kroków:

![Cykl opracowywania modelu](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Jak widać, że cyklu składa się z sześciu kroków:
-   Skład problem
-   Spożyciu danych i badań danych
-   Przygotowanie danych i funkcji inżynierskie
-   Modelowanie
-   Oceny modelu
-   Opracowywania

W dalszej części tej sekcji możemy opisano poszczególne kroki i elementów do rozważenia na poszczególnych etapach.

### <a name="problem-formulation"></a>Skład problem
Firma Microsoft należy rozważyć sformułowania problem jako najważniejszych krok, który należy wykonać przed wykonania każdego rozwiązania przewidywanych analizy. W tym miejscu będzie Przekształcanie problemu biznesowego i których przedstawiany go do określonych elementów, które można rozwiązać przez przy użyciu danych i technik modelowania. Jest dobrym rozwiązaniem jest sformułowania problem jako zbiór chcemy odpowiedzi na pytania. Oto kilka możliwych pytań, które mogą być stosowane w zakresie prognozowania żądanie energii:
-   Co to jest oczekiwane obciążenie poszczególnych podstacji w następnym godzina lub dzień?
-   W jakiej porze dnia Moje siatki wystąpią Szczyt żądanie?
-   Jak prawdopodobnie jest Mój siatki w celu wyważonego Załaduj Szczyt oczekiwanych?
-   Ile power powinien generować elektrowni w każdej godziny dnia?

Sformułowania tych pytań umożliwia skoncentrowanie się na wprowadzenie właściwych danych i implementowania rozwiązanie, które jest w pełni wyrównany wykonywanego problemu biznesowego. Ponadto możemy następnie można ustawić kilka kluczowych metryk zezwalają na Ocenianie wydajności modelu. Na przykład jak dokładności prognozy powinni i co to jest zakres błąd, który może być akceptowane przez firmę?

### <a name="data-sources"></a>Źródła danych
Nowoczesna siatki inteligentne zbiera dane z różnych części i części siatki. Te dane reprezentują różne aspekty operacji i wykorzystania siatki power. W zakresie Prognoza energii możemy są ograniczanie dyskusji w źródeł danych, które odzwierciedlają zużycie rzeczywiste żądanie. Jedno źródło ważnych zużycia energii są inteligentne metr. Narzędzia na całym świecie szybkiego rozmieszczania inteligentne metr dla klientów indywidualnych, ich. Inteligentne miernikami nagrywanie zużycie energii rzeczywiste i nieustannie przekazywania tych danych do narzędzia firmy. Dane są zbierane i wysłane ponownie w stałych odstępach od co 5 minut na 1 godzinę. Bardziej zaawansowane metr inteligentne mogą być zaplanowane zdalnie do kontrolowania i saldo rzeczywiste zużycie w domu. Miernik inteligentne dane są stosunkowo zaufanego i zawiera sygnaturę czasową. Dzięki temu ważne składnika Prognoza. Miernik danych może być zagregowana (równy określonemu procentowi w górę) na różnych poziomach w topologii siatka: przekształcania, podstacji, region *itp*. Firma Microsoft następnie wybierz poziom agregacji wymaganych do tworzenia modelu prognozowania dla niego. Na przykład firmy narzędzie chcą prognozy przyszłych obciążenie każdego z jego podstacji siatki następnie metr wszystkich danych można być agregacją dla każdego poszczególnych podstacji i użyć jako danych wejściowych dla modelu prognozowania. Firma Microsoft odwołują się do liczników inteligentne jako wewnętrznego źródła danych.

Prognozy zaufanego energii będzie również zależeć innych źródeł danych zewnętrznych. Jeden duże znaczenie, który ma wpływ na zużycie energii jest pogody lub bardziej precyzyjne temperatury. Danych historycznych zawiera silne powiązania między temperatury na zewnątrz i zużycia energii. Dni ciepłej letnich wprowadź konsumentów przy użyciu ich klimatyzacja i podczas uruchamiania zima na grzewczych. Zaufanego źródła historycznych temperaturach w lokalizacji siatki w związku z tym jest kluczem. Ponadto możemy również bazować na dokładności prognozy temperatury jako predykcyjnych zużycia energii.

Innych źródeł danych zewnętrznych mogą również pomóc w tworzenie modeli prognozy żądanie energii. Mogą one obejmować długoterminową zmian klimatu, ekonomicznych indeksów (*np.*PKB) i innych osób. W tym dokumencie firma Microsoft nie będzie zawierać te innych źródeł danych.

### <a name="data-structure"></a>Struktura danych
Po odnalezieniu źródeł danych wymagane, chcemy upewnij się, że dane, które zostały zebrane zawiera funkcje właściwych danych. Aby utworzyć żądanie zaufanego modelu prognozy, czy należy upewnij się, że dane zebrane zawiera elementy danych, które mogą ułatwić przewidywanie przyszłych żądanie. Poniżej przedstawiono niektóre podstawowe wymagania dotyczące struktury danych (schemat) nieprzetworzonych danych.

Dane składa się z wierszy i kolumn. Każda miara jest reprezentowane jako pojedynczy wiersz danych. Każdy wiersz danych zawiera wiele kolumn (nazywane również funkcje lub pól).

1.  **Sygnatura czasowa** — pola sygnatury czasowej reprezentuje rzeczywisty czas, kiedy zarejestrowano odpowiednią wartość. Powinny być zgodne z jednym z typowych formatów daty/godziny. Części zarówno daty i godziny, należy włączyć. W większości przypadków wystarczy raz do można zarejestrować do drugiego poziomu szczegółowości. Należy określić strefę czasową, w którym dane są zapisywane.
2.  **Miernik ID** — to pole służy do identyfikowania miernik lub urządzenia miary. Jest zmienną kategorii i może być kombinację cyfr i znaków.
3.  **Wartość zużycia** — jest to rzeczywiste zużycie w danej daty/godziny. Zużycie może być mierzony w kWh (kilowatt-hour) lub inne preferowane jednostki. Należy pamiętać, że przez wszystkie miary w danych musi pozostaną niezmienione jednostki miary. W niektórych przypadkach zużycie można podanych powyżej 3 fazy power. W takim przypadku będzie potrzebne w fazach zużycie niezależnie.
4.  **Temperatury** — temperatury zazwyczaj są zbierane od niezależne źródło. Jednak powinny być zgodne z dane dotyczące zużycia. Powinny one obejmować sygnatura czasowa zgodnie z powyższym opisem umożliwiająca go do zsynchronizowania z danymi rzeczywistego zużycia. Wartość temperatury można określić w stopniach Celsjusza lub Fahrenheita, ale należy pozostaną niezmienione przez wszystkie miary.
5.  **Lokalizacja —** Pole Lokalizacja jest zwykle skojarzony z miejsce, w którym zostały zebrane dane dotyczące temperatury. Może być reprezentowany, jako liczbę kod pocztowy lub szerokość/długość geograficzna (lat długa) format.

Poniższej tabeli pokazano przykłady dobrych zużycie i temperatury format danych:

|**Data**|**Czas**|**Miernik identyfikator**|**Faza 1**|**Faza 2**|**Faza 3**|
|--------|--------|------------|-----------|-----------|-----------|
|2015-7-1|10:00:00|ABC1234     |7.0        |2.1        |5.3        |
|2015-7-1|10:00:01|ABC1234     |7.1        |2.2        |4.3        |
|2015-7-1|10:00:02|ABC1234     |6.0        |2.1        |4.0        |

|**Data**|**Czas**|**Lokalizacja**|**Temperatura**|
|--------|--------|-------------|---------------|
|2015-7-1|10:00:00|11242        |24.4           |
|2015-7-1|10:00:01|11242        |24.4           |
|2015-7-1|10:00:02|11242        |24.5           |

Jak widać powyżej, w tym przykładzie zawiera 3 różne wartości zużycie skojarzone z 3 fazy power. Należy również zauważyć, że pola daty i godziny są rozdzielane, jednak ich mogą być również połączone w jednej kolumnie. W tym przypadku kolumna lokalizacji jest wyświetlana w formacie 5-cyfrowy kod pocztowy i temperatury w formacie stopień Celsjusza.

### <a name="data-format"></a>Format danych
Pakiet analizy Cortana mogą obsługiwać najbardziej typowych formatach danych, takich jak CSV, TSV, JSON, *itp*. Należy pamiętać, format danych pozostaje spójne dla całego cyklu życia projektu.

### <a name="data-ingestion"></a>Spożyciu danych
Ponieważ energii prognozy jest stale i często przewidziane, możemy się zapewnić, że nieprzetworzonych danych jest ułożony za pomocą procesu spożyciu pełnego i wiarygodnych danych. Proces spożyciu musi zagwarantować, że nieprzetworzonych danych jest dostępna dla procesu prognozowania w czasie wymagane. Oznacza to, że częstotliwość spożyciu danych powinna być większa niż częstotliwość prognozowania.

Na przykład: Jeśli naszych żądanie prognozowania rozwiązanie wywoła nowej prognozy o 8:00 codziennie, a następnie należy upewnić się, że wszystkie dane, które zostały zebrane w ciągu ostatnich 24 godzin występują została całkowicie wchłonięte do tego punktu i ma nawet dołączyć ostatniej godziny danych.

W tym celu Cortana analizy Suite oferuje różne sposoby obsługi procesu spożyciu wiarygodnych danych. To będzie można opisane w sekcji **wdrażania** tego dokumentu.

### <a name="data-quality"></a>Jakość danych
Źródło nieprzetworzonych danych, które jest wymagane do wykonywania prognozowania żądanie wiarygodnych i dokładności musi spełniać kilka kryteriów jakości podstawowych danych. Mimo że zaawansowanych metod statystycznych może służyć do zadośćuczynienia dla niektórych problem jakości danych, trzeba nadal upewnij się, że są możemy przekraczających próg jakości niektóre dane podstawowe po ingesting nowe dane. Oto kilka zagadnień dotyczących jakości nieprzetworzonych danych:
-   **Brak wartości** — to odnosi się do sytuacji, gdy nie zostało pobrane określony rozmiar. Tutaj podstawowe wymagania to, że stopa brakujące wartości nie powinna być większa niż 10% dowolnego danego okresu. W przypadku, gdy określona wartość nie ma on należy wskazać przy użyciu wstępnie zdefiniowanych wartości (na przykład: "9999") ale nie "0", który może być nieprawidłowa.
-   **Dokładność miary** — rzeczywista wartość zużycia lub temperatury powinny być dokładnie zarejestrowane. Pomiary niedokładne da niedokładne prognozy. Zazwyczaj błąd miary powinna być mniejsza niż 1% względem wartość true.
-   **Czas pomiaru** — jest to wymagane że sygnatura czasowa rzeczywistych danych zbierane będą nie różni się przez dłużej niż 10 sekund względem PRAWDA czasu rzeczywistego miary.
-   **Synchronizacja** — po wielu źródeł danych są używane (*np.*, zużycie i temperatury) firma Microsoft musi zapewnić, że są problemy nie synchronizacji czasu między nimi. Oznacza to, że różnicę czasu między zebrane sygnatura czasowa z dowolnym dwa niezależne źródła danych nie powinna przekraczać dłużej niż 10 sekund.
-   **Opóźnienie** - opisane powyżej w **Spożyciu danych**, możemy są zależne od proces blokowy i pokarmową wiarygodnych danych. Aby kontrolować, czy firma Microsoft upewnić, że sterować opóźnienie danych. Jest to określone różnicę czasu między dokonano pomiaru rzeczywisty czas i czas, po którym załadowaniu do magazynu pakietu analizy Cortany i jest gotowa do użycia. Obciążenia krótkim prognozowania całkowity czas oczekiwania nie należy większa niż 30 minut. Obciążenia długoterminową prognozowania całkowity czas oczekiwania nie należy większe niż 1 dzień.

### <a name="data-preparation-and-feature-engineering"></a>Przygotowanie danych i funkcji inżynierskie
Po nieprzetworzonych danych zostało zasysanego (patrz **Spożyciu danych**) i bezpiecznie przechowywane, jest gotowe do przetwarzania. Zasadniczo trwa nieprzetworzonych danych i konwertowanie (Przekształcanie, zmiana kształtu) fazy przygotowywania danych do formularza w fazie modelowania. Który może zawierać proste czynności, takie jak przy użyciu kolumny nieprzetworzonych danych, tak jak jej wartość mierzona, znormalizowanych wartości bardziej złożonych operacji, takich jak [opóźnionych czasu](https://en.wikipedia.org/wiki/Lag_operator)i innych osób. Kolumny danych nowo utworzonego są określane jako funkcji danych i procesu tworzenia tych nosi nazwę funkcji techniczny. Na koniec tego procesu czy mamy nowy zestaw danych, zostały uzyskane od nieprzetworzonych danych, który może być używany do modelowania. Ponadto fazy przygotowania danych musi zajmować brakujące wartości (zobacz **Jakości danych**) i ich wyrównania. W niektórych przypadkach może również należy znormalizować danych, aby upewnić się, że wszystkie wartości są przedstawione w tę samą skalę.

W tej sekcji niektóre typowe funkcje danych, które znajdują się w energii na listę modele prognozy żądanie.

**Czas pracy funkcje:** Te funkcje są obliczane na podstawie danych Data/sygnatura czasowa. Te są wyodrębniane i przekonwertowana na kategorii funkcji, takich jak:
-   Godzina dnia — jest to godziny dnia, która pobiera wartości z zakresu od 0 do 23
-   Dzień tygodnia — to reprezentuje dzień tygodnia i przyjmuje wartości od 1 (niedziela) do 7 (sobota)
-   Dzień miesiąca — to odpowiada rzeczywistej daty i może przyjmować wartości od 1 do 31
-   Miesiąc roku — to reprezentuje miesiąca i przyjmuje wartości od 1 (styczeń) do 12 (grudzień)
-   Weekend — jest to funkcję binarny wartość, która ma wartość 0 dla dni tygodnia lub 1 "weekend"
-   Dzień wolny — jest to funkcję binarny wartość, która ma wartość 0 na zwykłą dzień lub 1 Święto
-   Warunki Fouriera — warunki Fouriera są grubości, które pochodzą z sygnatura czasowa i służą do przechwytywania sezonowości (cykli) w danych. Ponieważ firma Microsoft może mieć wiele okresów w naszych danych Firma Microsoft może być konieczne wiele terminów Fouriera. Na przykład wartości żądanie może być roczny, tygodniowy i dziennych okresów cykli, które spowoduje 3 postanowień Fouriera.

**Funkcje pomiarów niezależnych:** Niezależne funkcje obejmują wszystkich elementów danych, które chcemy ma zostać użyte jako zmienne predykcyjne w naszym modelu. Tutaj możemy wyłączyć funkcję zależne, która będzie trzeba przewidywanie.
-   Funkcji zwłoki — są to czas przesuniętą wartości rzeczywiste żądanie. Na przykład funkcje czasu zwłoki 1 będzie zawierać wartość żądanie poprzedniej godziny (przy założeniu dane godzinne) względem bieżącą sygnaturę czasową. Podobnie, możemy dodać zwłoki 2, zwłoki 3 *itd*. Rzeczywisty kombinacji funkcji zwłoki, które są używane są określane w fazie modelowania przez oceny wyników modelu.
-   Dłuższy czas popularnych — ta funkcja stanowi liniowej wzrost żądanie latach.

**Funkcji zależne:** Funkcja zależne jest kolumny danych, które chcemy naszego modelu do prognozowania. Z [komputera pod nadzorem nauki](https://en.wikipedia.org/wiki/Supervised_learning)trzeba najpierw przeszkolenie modelu przy użyciu funkcji zależne (określany także jako etykiety). Dzięki temu model dowiedzieć się, trendy w danych skojarzonych z funkcją zależne. W energii Prognoza zwykle chcemy prognozować zapotrzebowanie rzeczywiste i w związku z tym firma Microsoft może użyć go jako funkcję zależne.

**Obsługa brakujące wartości:** W fazie przygotowywania danych będzie trzeba Określanie strategii zalecane do obsługi brakujące wartości. Zwykle jest to za pomocą różnych [metod przypisywania danych](https://en.wikipedia.org/wiki/Imputation_(statistics))statystycznych. W przypadku energii prognozowania żądanie, możemy zwykle naliczenie brakujące wartości przy użyciu średnią ruchomą z poprzedniego punktów danych.

**Normalizacji danych:** Normalizacji danych jest innego typu transformacji służący do wprowadzenia wszystkie dane liczbowe, takie jak prognozę do skali podobne. Pomaga to zazwyczaj poprawić dokładność modelu i dokładności. Firma Microsoft może zwykle w tym dzieląc rzeczywista wartość według określonego zakresu danych.
Powoduje to skalowanie oryginalną wartość w dół do mniejszego zakresu, zwykle od -1 do 1.

## <a name="modeling"></a>Modelowanie
Faza modelowania to, gdzie odbywa się konwersji danych do modelu. W podstawowych tego procesu są zaawansowane algorytmów przeglądania danych historycznych (szkolenie dane), wyodrębnianie desenie i tworzenie modelu. Tego modelu można później przewidywanie na nowych danych, który nie został użyty do utworzenia modelu.

Gdy mamy pracy wiarygodnego modelu następnie używać go do wyniku nowych danych, które ma struktury, aby uwzględnić wymagane funkcje (X). Proces tworzenia wyników spowoduje, że za pomocą modelu trwałych (obiekt z fazy szkolenie) i przewidywanie zmiennej docelowej jest oznaczany przez Ŷ.

### <a name="demand-forecasting-modeling-techniques"></a>Żądanie prognozowania modelowania techniki
W przypadku żądanie prognozowania udzielamy za pomocą danych historycznych porządkowania przez godzinę. Firma Microsoft na ogół odnoszą się do danych, który zawiera wymiar czasu jako [szeregu czasowego](https://en.wikipedia.org/wiki/Time_series). Celem modelowania serii czasu jest znalezienie czas powiązanych trendów sezonowości, automatyczne korelacji (korelacji w czasie) i sformułowania te do modelu.

W ostatnich latach zaawansowane algorytmy opracowano, aby zezwalały na prognozowania szeregu czasowego i aby zwiększyć dokładność prognozowania. Krótko omówimy niektóre z nich w tym miejscu.

> [AZURE.NOTE] W tej sekcji nie mają być używane jako komputerze nauki i prognozowania Przegląd, lecz raczej krótką ankietę modelowania technik najczęściej używanych do prognozowania żądanie. Aby uzyskać więcej informacji i materiały edukacyjne o prognozowania szeregu czasowego zdecydowanie zalecamy książki online [Prognozowanie: zasady i ćwiczenia](https://www.otexts.org/book/fpp).

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (średniej ruchomej)**](https://www.otexts.org/fpp/6/2)
Średniej ruchomej jest jednym z pierwszej technik analitycznych, używane do prognozowania szeregu czasowego i jest nadal jedną z najczęściej używanych technik obecnie. Jest również podstawę dla bardziej zaawansowanych technik prognozowania. Ze średnią ruchomą możemy są Prognozowanie następnego punktu danych przez uśrednianie K ostatnich punktów, gdzie K oznacza kolejności średnią ruchomą.

Przenoszenie technika średnia powoduje wygładzanie prognozy i dlatego nie mogą obsługiwać również dużych zmienności danych.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**ETS (Wygładzanie wykładnicze)**](https://www.otexts.org/fpp/7/5)
Wygładzania wykładniczego (ETS) jest rodzina różnych metod, które było przewidywanie następnego punktu danych za pomocą średniej ważonej ostatnim punktom danych. Koncepcja jest Przypisz nowszy grubości do nowych wartości i stopniowe zmniejszanie tej masy dla starszych mierzonych. Istnieje kilka różnych metod tej rodzinie, obsługi sezonowości niektóre z nich obejmować dane, takie jak [Metody sezonowy Holt Winters](https://www.otexts.org/fpp/7/5).

Również niektóre z tych metod współczynnika w sezonowości danych.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**ARIMA (automatyczne regresji zintegrowanego przenoszenia średnia)**](https://www.otexts.org/fpp/8)
Automatyczne regresji zintegrowane przenoszenia średnia (ARIMA) jest innej rodziny metod, który jest powszechnie używany do prognozowania szeregu czasowego. Łączy praktycznie metody regresji automatyczne ze średnią ruchomą. Metody regresji automatycznie używać modeli regresji, nie podejmując poprzednie czasu serii wartości w celu obliczenia następnego punktu daty. Metody ARIMA również zastosować różnicowego metod, które obejmują Obliczanie różnicy między punktami danych i przy użyciu tych zamiast oryginalną wartość mierzona. Na koniec ARIMA również korzysta z przenoszenie technik średnia, które zostały omówione powyżej. Kombinacja wszystkich tych metod na różne sposoby to, co tworzy rodzinie metod ARIMA.

ETS i ARIMA powszechnie są używane dzisiaj prognozowania żądanie energii i wiele innych problemów prognozowania. W większości przypadków są one łączone ze sobą do przeprowadzania bardzo dokładnych wyników.

**Ogólne wielokrotności regresji** Modele regresji mogą być najważniejszych metoda modelowania w domenie maszynowego uczenia się i statystyki. W kontekście szeregu czasowego używamy regresji aby prognozować przyszłe wartości (*np.*: żądanie). W regresji możemy wykonać kombinacja liniowy zmienne predykcyjne i ustalanie masy (nazywane również współczynniki) te zmienne predykcyjne podczas procesu szkolenia. Celem jest przygotowanie linii regresji, który będzie REGLINX naszych prognozowanej wartości. Metody regresji są odpowiednie, gdy zmienną docelową jest liczbą i w związku z tym mieści się także prognozowania szeregu czasowego. Istnieje szereg metod regresji w tym modeli bardzo prostej regresji takich jak [Regresji liniowej](https://en.wikipedia.org/wiki/Linear_regression) i bardziej zaawansowane z nich przykład drzewa decyzji, [Lasów losowo](https://en.wikipedia.org/wiki/Random_forest), [Sieci neuronowych](https://en.wikipedia.org/wiki/Artificial_neural_network)i zwiększane drzewa decyzji.

Budowa żądanie energii prognozowania jako problem regresji daje wiele elastyczność przy wyborze funkcji naszych danych, które mogą być łączone z żądanie rzeczywisty czas serie danych i zewnętrznych czynników, takich jak temperatury. Więcej informacji na temat funkcji zaznaczonych zostały omówione w funkcji rysunek techniczny sekcji (zobacz **Przygotowanie danych i funkcji inżynierskich**) to playbook.

Z naszych doświadczeń z implementacji i wdrożenia pilotażowego prognoz żądanie energii mają znalezionych modeli zaawansowane regresji, które są dostępne w Azure ML polecenia w celu uzyskania najlepszych wyników, a my przekażemy korzystanie z nich.

## <a name="model-evaluation"></a>Oceny modelu
Oceny modelu ma kluczową rolę w ramach **Cyklu opracowywania modelu**. Na tym etapie możemy Sprawdź do sprawdzania poprawności modelu i jego wydajności za pomocą rzeczywistych danych. W kroku modelowania korzystamy części dostępnych danych dla szkolenia modelu. Podczas fazy oceny przejść do końca dane, aby sprawdzić modelu. Praktycznie oznacza to, że firma Microsoft są podawania modelu nowymi danymi, których występują został przekształcony, a zawiera te same funkcje co szkolenie zestawu danych. Jednak podczas procesu sprawdzania poprawności firma Microsoft korzysta z modelu przewidywanie zmienną docelową, a nie zapewniają zmienną dostępne docelową. Firma Microsoft często można znaleźć ten proces jako modelu wyników. Czy firma Microsoft następnie użyj wartości docelowej PRAWDA i porównania z nich przewidywane. W tym miejscu celem jest Zmierz i zminimalizować błąd przewidywania, czyli różnica między przewidywań i wartość true. Oszacowania pomiaru błędów jest klucz, ponieważ chcemy precyzyjnie dostosować modelu i sprawdź, czy rzeczywiście zmniejsza się komunikat o błędzie. Dostosowywania modelu można przeprowadzić zmieniając parametry modelu, które sterują procesem nauki lub przez dodanie lub usunięcie funkcji danych (nazywane [Wyczyść parametry](https://channel9.msdn.com/Blogs/Windows-Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Praktycznie oznacza to, że firma Microsoft może być konieczne powtarzać inżynierskie funkcji modelowania i modelu fazy oceny wielokrotnie, dopóki nie możemy zmniejszyć komunikat o błędzie do wymaganego poziomu.

Ważne jest, aby wyróżnienia, że błąd przewidywania nigdy nie będą zero nigdy nie jest modelu, który można idealnie przewidywanie każdej wynik. Istnieje jednak wielkość błędu, która jest do przyjęcia przez firmę. W trakcie procesu sprawdzania poprawności chcemy w celu zapewnienia na poziomie naszych błędu przewidywania modelu lub większą niż poziom uszkodzenia firm. W związku z tym należy ustawić poziom dopuszczalnego błędu na początku cyklu fazie **Sformułowania Problem** .

### <a name="typical-evaluation-techniques"></a>Typowe oceny technik
Istnieją różne metody, w których przewidywania błędu może być mierzony i określić. W tej sekcji możemy fokus zostanie ustawiony dyskusji na temat metod oceny właściwe szeregu czasowego i w specyficzne dla energii Prognoza.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE oznacza oznacza błędu bezwzględnego wartość procentową. Z MAPE możemy Obliczanie różnicy między prognozowanych każdego punktu i rzeczywista wartość tego punktu. Następnie możemy oszacowanie błędu dla każdego punktu obliczania odsetek różnicy względem rzeczywista wartość. W ostatnim kroku możemy średnia tych wartości. Formuły matematycznej używanej dla MAPE jest następująca:

![Formuła MAPE](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*gdzie<sub>t</sub> jest wartością rzeczywistą, F,<sub>t</sub> jest prognozowanej wartości, a n jest Horyzont prognozy.*

## <a name="deployment"></a>Wdrożenie
Po możemy ma zająć fazy modelowanie i sprawdzone wydajności modelu pracujemy gotowa do wysłania do fazy wdrożenia. W tym kontekście wdrożenia oznacza, że włączenie klienta do korzystania z modelu, uruchamiając przewidywań rzeczywista nad nim w dużych. Koncepcja wdrażania jest klucz w Azure ML, ponieważ naszym głównym celem jest stale wywołania przewidywań zamiast tylko uzyskania wgląd w danych. Faza rozmieszczania jest częścią miejsce, w którym możemy Włączanie modelu, który ma zostać zużyta na dużą skalę.

W ramach energii prognozy naszym celem jest wywoływanie prognoz ciągły i okresowych podczas zapewnienie, że nowe dane jest dostępna dla modelu i że prognozowaną danych jest wysyłana do dużo klienta.

### <a name="web-services-deployment"></a>Wdrażanie usług sieci Web
Główny możliwością rozmieszczania bloków konstrukcyjnych w Azure ML to usługa sieci web. To jest najlepszym sposobem na włączanie zużycie modelu przewidywanych w chmurze. Usługa sieci Web obejmuje modelu i otacza go z [RESTful](http://www.restapitutorial.com/) interfejsu API (Application Programming Interface). Interfejs API może służyć jako część dowolnego kodu klienta, jak pokazano na poniższej ilustracji.

![Firma Microsoft wdrażanie usługi i zużycie](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Jak ma być widoczne usługi sieci web zostanie wdrożony w chmurze pakietu analizy Cortana i może być wywoływana za pośrednictwem jego udostępnionej końcowego interfejsu API usługi REST. Inny rodzaj klientów w różnych domenach można jednocześnie wywoływanie usługi za pośrednictwem interfejsu API sieci Web. Usługa sieci web można również stosowane do obsługi tysiące wywołania.

### <a name="a-typical-solution-architecture"></a>Architekturę rozwiązania typowych
Wdrażając żądanie energii prognozowania rozwiązanie, możemy zainteresować: Wdrażanie rozwiązania kompleksowego wykracza poza przewidywania usługi sieci web, która ułatwia przepływu danych. W tym czasie możemy wywołania nowej prognozy czy należy upewnij się, że model jest rana przy użyciu funkcji aktualne dane. Oznacza to, że nowo zebranych nieprzetworzonych danych jest stale wchłonięte, przetwarzania i przekształceniu wymagane skonfigurowaną na został utworzony model. W tym samym czasie chcemy udostępnić prognozowaną dane w celu używające klientów. Na poniższej ilustracji przedstawiono przykład danych Cykl blokowy (lub planowana danych):

![Żądanie energii prognozy przepływu danych kompleksowego](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Poniżej przedstawiono czynności, które w ramach cyklu prognozy żądanie energii:
1.  Miliony metr wdrożonym danych są stale Generowanie dane zużycia energii w czasie rzeczywistym.
2.  Ten dane są zbierane i przekazane do repozytorium chmury (*np.*obiektów Blob platformy Azure).
3.  Przed przetwarzane, nieprzetworzonych danych jest agregowane podstacji lub regionalnym, zdefiniowane przez firmę.
4.  Funkcja przetwarzanie (zobacz **Przygotowanie danych i funkcji przetwarzania**) następnie odbywa się i przedstawi modelu danych, które są wymagane do szkolenia lub wyników — dane zestawu funkcji są przechowywane w bazie danych (*np.*platformy SQL Azure).
5.  Wywoływania ponownie szkoleń usług do ponownego przeszkolenie model prognozowania — tej zaktualizowanej wersji modelu jest zachowywane tak, aby mogą być używane przez usługę sieci web wyników.
6.  Zgodnie z harmonogramem, pasującą wymagane częstotliwości prognozy wywoływania wyników usług sieci web.
7.  Prognozowaną dane są przechowywane w bazie danych, które są dostępne dla klienta zużycie zakończenia.
8.  Klient zużycie pobiera prognozy, stosuje je do siatki i używa go zgodnie z przypadków użycia wymagane.

Należy zauważyć, że cały cykl zautomatyzowana jest w pełni i uruchamia zgodnie z harmonogramem. Cały aranżacji cyklu danych jest możliwe przy użyciu narzędzi, takich jak [Azure danych Factory](http://azure.microsoft.com/services/data-factory/).

### <a name="end-to-end-deployment-architecture"></a>Architektura kompleksowe wdrożenie
Aby praktycznie wdrażanie rozwiązania prognozy żądanie energii w analizy Cortana, należy zapewnić wymagane składniki ustanowioną skonfigurowane poprawnie.

Na poniższym diagramie przedstawiono typowe architekturę analizy Cortana podstawie, która służy do instalowania i orchestrates cyklu przepływu danych, który jest opisany powyżej:

![Architektura kompleksowe wdrożenie](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Więcej informacji na temat poszczególnych składników i całej architektury można znaleźć w szablonie rozwiązanie energii.
