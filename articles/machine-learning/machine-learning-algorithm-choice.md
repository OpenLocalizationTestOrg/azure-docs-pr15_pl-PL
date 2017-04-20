<properties
    pageTitle="Jak wybrać algorytmów uczenia maszynowego | Microsoft Azure"
    description="Jak wybrać uczenia algorytmów do nauki nadzorowane i bez nadzoru w doświadczeniach klastrowanie, klasyfikacji lub regresji."
    services="machine-learning"
    documentationCenter=""
    authors="brohrer"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>
    
<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/09/2016"
    ms.author="brohrer;garye" />

# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>Jak wybrać algorytmy dla analityków firmy Microsoft

Odpowiedź na pytanie "Co algorytmu uczenia maszynowego należy używać?" jest zawsze "To zależy." To zależy od rozmiaru, jakości i charakteru danych. To zależy to, co chcesz zrobić z tą odpowiedzią. Zależy to od sposobu zapisu matematycznego algorytmu została przetłumaczona na instrukcje dla komputera, którego używasz. I zależy na czas, jaki masz. Nawet najbardziej doświadczonych przetwarzających dane nie wiadomo algorytmu, który będzie działał najlepiej przed próbuje je.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Algorytm uczenia maszynowego oszustwo arkusz

**Microsoft Azure nauki algorytm oszukiwać arkuszu maszyny** pomaga wybrać odpowiednią maszynę algorytmu dla rozwiązań predykcyjną z biblioteki Microsoft uczenia algorytmów uczenia.
W tym artykule krok po kroku dotyczące używania go.

> [AZURE.NOTE] Aby pobrać arkusz oszukiwać je wraz z tego artykułu, przejdź do [uczenia algorytmu oszukiwać arkusz Microsoft Azure Machine Learning Studio maszynowego](machine-learning-algorithm-cheat-sheet.md).

Ten arkusz oszukiwać ma bardzo określonej grupy odbiorców w umyśle: naukowiec danych początku uczenie maszynowe poziomu studiów, próbuje wybrać algorytm odbywało się w studiu uczenia maszynowego Azure. Oznacza to, że to sprawia, że niektóre generalizacji i oversimplifications, ale będzie wskazywać w bezpiecznym kierunku. Oznacza to również, że istnieje wiele algorytmów niewymienionych tutaj. Wraz ze wzrostem usługą sieci Web aby objąć bardziej kompletny zestaw dostępnych metod dodamy je.

Zalecenia te są skompilowane opinii i porad z partii danych naukowców i ekspertów uczenia się maszyny. Nie zgadzamy się na wszystko, ale próbowałem zharmonizowanie nasze opinie do surowca konsensusu. Większość instrukcji zastrzeżeń zaczyna się od "To zależy..."

### <a name="how-to-use-the-cheat-sheet"></a>Jak używać ściągawki

Odczytać ścieżka i algorytm etykiety na wykresie jako "dla * &lt;etykiety ścieżki&gt; * użyć * &lt;algorytm&gt;*." Na przykład *"szybkość* użycia *dwóch regresją klasy*." Czasami więcej niż jeden oddział będzie stosowana.
Czasami żaden z nich nie będzie dokładne dopasowanie. Są one przeznaczone jako zasada kciuka zalecenia, więc nie martw się o to jest dokładny.
Kilka przetwarzających dane I Rozmawialiśmy o wspomnianej które jedynym pewnym sposobem znalezienia najlepszej algorytm jest próba ich wszystkich.

Oto przykład z [Galerii Cortana Analytics](http://gallery.cortanaintelligence.com/) eksperymentu, który próbuje kilku algorytmów na tych samych danych i porównuje wyniki: [porównać klasyfikatorów wielu klas: list uznanie](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

>[AZURE.TIP] Aby pobrać i wydrukować diagram, który zawiera omówienie możliwości Studio uczenia maszynowego, zobacz [diagram ogólny Omówienie możliwości](machine-learning-studio-overview-diagram.md).

## <a name="flavors-of-machine-learning"></a>Aromaty analityków

### <a name="supervised"></a>Nadzorowana

Algorytmy uczenia nadzorowanego prognoz opartych na zestaw przykładów. Na przykład historyczne notowania giełdowe może służyć do prób zagrożenia w przyszłości cenach. Każdy przykład używane do szkolenia jest opatrywany etykietą zawierającą wartość odsetek — w tym przypadku ceny giełdowej. Algorytm uczenia nadzorowanego utrwala pewne wzorce te etykiety wartości. Można użyć wszelkie informacje, które mogłyby być istotne — dzień tygodnia, sezon, danych finansowych firmy, rodzaju przemysłu, obecność zdarzenia uciążliwych geopolicitical — a każdy algorytm szuka różnego rodzaju wzorców. Po Algorytm wykrył najlepszego deseniu może, używa tego wzorca do prognoz dla testowania danych bez etykiety — ceny w przyszłości.

Jest to popularne i przydatne typ uczenia się maszyny. Poza jednym wyjątkiem wszystkie moduły w uczenia są nadzorowane nauki algorytmów. Istnieje kilka typów określonych nadzorowane uczenia się, które są reprezentowane w ramach uczenia: wykrywanie klasyfikacji, regresji i anomalii.

* **Klasyfikacja**. Gdy dane są używane do przewidywania kategorię, uczenia nadzorowanego jest również nazywany klasyfikacji. Dotyczy to podczas przypisywania obraz jako obraz "kot" lub "dog". Gdy istnieją tylko dwie opcje, nazywa się **dwie klasy** lub **klasyfikacji dwumianowy**. Gdy istnieje więcej kategorii jako, przy przewidywaniu zwycięzca turnieju NCAA Madness marca, ten problem jest znany jako **wielu klas klasyfikacji**.

* **Regresji**. Gdy wartość jest jest przewidywane, podobnie jak w przypadku cen akcji, uczenia nadzorowanego nosi regresji.

* **Wykrywanie anomalii**. Czasami celem jest zidentyfikowanie punktów danych, które są po prostu niezwykłe. W wykrywanie oszustw na przykład, wszelkie wzorce wydatków wyjątkowa karty kredytowej są podejrzane. Ewentualne zmiany są tak liczne i wygląda tak małą, że nie jest możliwe, aby dowiedzieć się, jakie oszukańczej działalności przykładów szkoleniowych. Podejście, które przekieruje wykrywania anomalii jest po prostu dowiedzieć się jakie normalnej działalności wygląda jak (przy użyciu historię transakcji niezwiązanych z) i zidentyfikować wszystko, co różni się znacząco.

### <a name="unsupervised"></a>Bez nadzoru

W nauce bez nadzoru, punkty danych mają etykiety nie związanych z nimi. Zamiast tego celem Algorytm uczenia się bez nadzoru jest do organizowania danych w inny sposób lub do opisania jego struktury. Oznacza to zgrupowanie w klastry lub znalezienie ogląda złożonych danych, tak aby była wyświetlana prostsze i bardziej zorganizowane na różne sposoby.

### <a name="reinforcement-learning"></a>Wzmocnienia edukacji

Wzmacnia się nauki algorytm pobiera wybierz akcję w odpowiedzi na każdy punkt danych. Algorytm uczenia się również odbiera sygnały za wynagrodzeniem po krótkim czasie, wskazującą, jak dobre była decyzja.
Na tej podstawie algorytm modyfikuje strategii w celu osiągnięcia najwyższego za wynagrodzeniem. Obecnie nie ma żadnych wzmocnienie nauki modułów Algorytm uczenia. Często w robotics, gdzie zestaw odczyty czujnika w jednym punkcie w czasie jest punkt danych i algorytm musisz wybrać następną akcję robota wzmocnienia edukacji. Jest to także fizyczną nadające się do Internetu rzeczy aplikacji.

## <a name="considerations-when-choosing-an-algorithm"></a>Zagadnienia dotyczące wybierania algorytmu

### <a name="accuracy"></a>Dokładność

Uzyskania możliwie najbardziej dokładne odpowiedzi nie zawsze jest konieczne.
Czasami przybliżenie jest odpowiednie, w zależności od tego, czy ma być ona stosowana. Jeśli tak się stanie, może być znacznie obniżyć swój czas przetwarzania przez trzyma więcej metod przybliżone. Inną zaletą więcej metod przybliżonych jest, że naturalnie zwykle nie [nadmierne dopasowanie](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Czas szkolenia

Liczbę minut lub godzin należy przeszkolić modelu różni wiele algorytmów. Czas szkolenia jest często ściśle powiązane do trafność — jeden zazwyczaj towarzyszy drugiej. Ponadto niektóre algorytmy są bardziej wrażliwe na liczbę punktów danych niż inne.
Gdy czas jest ograniczony to dysk wybór algorytmu, zwłaszcza w przypadku, gdy zestaw danych jest duży.

### <a name="linearity"></a>Liniowość

Wiele algorytmów uczenia maszynowego należy użyć liniowości. Algorytmów klasyfikacji liniowa zakłada, że klas mogą być oddzielone przez linię prostą (lub jego wielowymiarowego analogowe). Te obejmują regresją i obsługi maszyn vector (zaimplementowanego analityków).
Algorytmy regresji liniowej założono, że trendów danych wykonaj linię prostą. Założenia te nie są szkodliwe dla niektórych problemów, ale na innych przynoszą dokładności w dół.

![Nieliniowa klasy bounday][1]

***Nieliniowa granicy klasy*** *-opierając się na algorytm klasyfikacji liniowej spowodowałoby niskiej dokładności*

![Dane z trendem nieliniowych][2]

***Dane z trendem nieliniowych*** *-za pomocą metody regresji liniowej może wygenerować wiele błędów większy niż jest to konieczne*

Pomimo ich niebezpieczeństwa liniowej algorytmy są bardzo popularne jako pierwszą linię ataku. Wydają się być algorytmicznie prosty i szybki do pociągu.

### <a name="number-of-parameters"></a>Liczba parametrów

Parametry są pokręteł, które pobiera analitykiem danych, aby włączyć podczas konfigurowania algorytmu. Są one numery, które mają wpływ na zachowanie algorytm, takich jak Tolerancja błędu lub liczba iteracji lub opcje między wariantów algorytm zachowuje się jak. Szkolenia i programy algorytmu czasami może być bardzo czułe na uzyskiwanie odpowiednich ustawień. Zwykle algorytmy z dużą liczbą parametrów wymagają większości metodą prób i błędów, aby znaleźć dobre połączenie.

Alternatywnie znajduje się blok [szerokimi parametr](machine-learning-algorithm-parameters-optimize.md) modułu analityków usiłującą automatycznie wszystkie kombinacje parametrów niezależnie od wybrania ziarnistość. Jest świetnym sposobem upewnij się, że zostały objęte parametr miejsca, czasu wymaganego do szkolić modelu wykładniczo zwiększa się liczba parametrów.

Plusem jest to, że posiadanie wielu parametrów zazwyczaj wskazuje, że algorytm ma większą elastyczność. Często można osiągnąć bardzo dużą dokładność rozpoznawania mowy. Pod warunkiem że można znaleźć właściwą kombinację ustawień parametrów.

### <a name="number-of-features"></a>Wiele funkcji

Dla niektórych typów danych, wiele funkcji mogą być bardzo duże w porównaniu z liczbą punktów danych. Często jest to miejsce w przypadku genetyka lub danych tekstowych. Wiele funkcji można zapełniać niektóre algorytmy kształcenia, szkolenia i Odkryj długa. Obsługa wektor maszyny są szczególnie dobrze nadaje się do tej sprawy (patrz poniżej).

### <a name="special-cases"></a>Przypadki specjalne

Niektóre algorytmy uczenia się zakładają określonej struktury danych lub pożądanych wyników. Jeśli można znaleźć taki, który pasuje do Twoich potrzeb, to to daje wyniki bardziej użyteczne, bardziej dokładne prognozy lub krótszy czas szkolenia.

|**Algorytm**|**Dokładność**|**Czas szkolenia**|**Liniowość**|**Parametry**|**Notatki**|
|---|:---:|:---:|:---:|:---:|---|
|**Dwie klasy klasyfikacji**| | | | | |
|[regresji logistycznej](https://msdn.microsoft.com/library/azure/dn905994.aspx)                    | |●|●|5| |
|[decyzja lasu](https://msdn.microsoft.com/library/azure/dn906008.aspx)|●|○| |6| |
|[Dżungla decyzji](https://msdn.microsoft.com/library/azure/dn905976.aspx)|●|○| |6|Niskie zużycie pamięci|
|[drzewo decyzyjne promowane](https://msdn.microsoft.com/library/azure/dn906025.aspx)|●|○| |6|Rozmiaru pamięci|
|[sieci neuronowych](https://msdn.microsoft.com/library/azure/dn905947.aspx)|●| | |9|[Możliwe jest dostosowanie dodatkowe](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[uśrednionych perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx)|○|○|●|4| |
|[Maszyna wektorów nośnych](https://msdn.microsoft.com/library/azure/dn905835.aspx)| |○|●|5|Dobre dla zestawów funkcji Duży|
|[Maszyna wektorów nośnych lokalnie głęboki](https://msdn.microsoft.com/library/azure/dn913070.aspx)|○| | |8|Dobre dla zestawów funkcji Duży|
|[Maszyny punktu Bayesa](https://msdn.microsoft.com/library/azure/dn905930.aspx)| |○|●|3| |
|**Wielu klas klasyfikacji**| | | | | |
|[regresji logistycznej](https://msdn.microsoft.com/library/azure/dn905853.aspx)| |●|●|5| |
|[decyzja lasu](https://msdn.microsoft.com/library/azure/dn906015.aspx)|●|○| |6| |
|[Dżungla decyzji](https://msdn.microsoft.com/library/azure/dn905963.aspx)|●|○| |6|Niskie zużycie pamięci|
|[sieci neuronowych](https://msdn.microsoft.com/library/azure/dn906030.aspx)|●| | |9|[Możliwe jest dostosowanie dodatkowe](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[jeden v wszystkich](https://msdn.microsoft.com/library/azure/dn905887.aspx)|-|-|-|-|Wyświetlić właściwości wybranej metody dwie klasy|
|**Regresji**| | | | | |
|[liniowy](https://msdn.microsoft.com/library/azure/dn905978.aspx)| |●|●|4| |
|[Liniowy Bayesa](https://msdn.microsoft.com/library/azure/dn906022.aspx)| |○|●|2| |
|[decyzja lasu](https://msdn.microsoft.com/library/azure/dn905862.aspx)|●|○| |6| |
|[drzewo decyzyjne promowane](https://msdn.microsoft.com/library/azure/dn905801.aspx)|●|○| |5|Rozmiaru pamięci|
|[kwantyl Fast lasu](https://msdn.microsoft.com/library/azure/dn913093.aspx)|●|○| |9|Zamiast prognoz punktu dystrybucji|
|[sieci neuronowych](https://msdn.microsoft.com/library/azure/dn905924.aspx)|●| | |9|[Możliwe jest dostosowanie dodatkowe](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[Funkcja ROZKŁAD.POISSON](https://msdn.microsoft.com/library/azure/dn905988.aspx)| | |●|5|Technicznie dziennika liniowa. Do przewidywania liczy się|
|[Liczba porządkowa](https://msdn.microsoft.com/library/azure/dn906029.aspx)| | | |0|Do przewidywania zamawiania rangi|
|**Wykrywanie anomalii**| | | | | |
|[Maszyna wektorów nośnych](https://msdn.microsoft.com/library/azure/dn913103.aspx)|○|○| |2|Szczególnie dobre dla zestawów funkcji Duży|
|[Wykrywanie na podstawie UPW anomalii](https://msdn.microsoft.com/library/azure/dn913102.aspx)| |○|●|3| |
|[Oznacza K](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)| |○|●|4|Algorytm klastrowania|


**Właściwości algorytmu:**

**●** - pokazuje wysokie oceny dokładności, szybkie kształcenie razy i stosowania liniowości

**○** - pokazuje dużą dokładność rozpoznawania mowy i terminy szkolenia umiarkowany

## <a name="algorithm-notes"></a>Algorytm notatki

### <a name="linear-regression"></a>Regresja liniowa

Jak już wspomniano, [regresji liniowej](https://msdn.microsoft.com/library/azure/dn905978.aspx) dopasowuje linii (lub płaszczyzny lub hyperplane) do zestawu danych. Jest siłą napędową, prosty i szybki, ale może być zbyt proste dla niektórych problemów.
Skorzystaj z [samouczka regresji liniowej](machine-learning-linear-regression-in-azure.md).

![Dane z trendu liniowego][3]

***Dane z trendu liniowego***

### <a name="logistic-regression"></a>Regresji logistycznej

Chociaż zawiera złudzenia "regresji" w nazwie, regresją jest faktycznie zaawansowane narzędzie [dwie klasy](https://msdn.microsoft.com/library/azure/dn905994.aspx) i [wielu klas](https://msdn.microsoft.com/library/azure/dn905853.aspx) klasyfikacji. To szybkie i proste. Fakt, że używa "-kształt krzywej zamiast prostej sprawia, że naturalne dopasowanie dla dzielenie danych na grupy. Granice liniowe klasy daje regresją, więc podczas używania go, upewnij się, że zbliżenia liniowej jest coś, co można żyć.

![Regresją, aby dwie klasy danych z tylko jednej funkcji][4]

***Regresją, aby dwie klasy danych z tylko jednej funkcji*** *-granica klasy jest punkt, w którym krzywa logistyczna jest blisko obu klas*

### <a name="trees-forests-and-jungles"></a>Drzewa, lasy i Dżungla

Decyzja lasów ([regresji](https://msdn.microsoft.com/library/azure/dn905862.aspx), [dwie klasy](https://msdn.microsoft.com/library/azure/dn906008.aspx)i [wielu klas](https://msdn.microsoft.com/library/azure/dn906015.aspx)), decyzja Dżungla ([dwie klasy](https://msdn.microsoft.com/library/azure/dn905976.aspx) i [wielu klas](https://msdn.microsoft.com/library/azure/dn905963.aspx)) i drzewa decyzji promowanego ([Regresja](https://msdn.microsoft.com/library/azure/dn905801.aspx) i [dwie klasy](https://msdn.microsoft.com/library/azure/dn906025.aspx)) wszystkie opierają się na drzewa decyzji fundamentalnych koncepcji uczenia maszynowego. Istnieje wiele wariantów drzewa decyzji, ale wszystkie one zrobić to samo — należy podzielić obszar funkcji w regionach głównie tę samą etykietę. Mogą to być regionów spójne kategorii lub o stałej wartości, w zależności od tego, czy czynności klasyfikacji lub regresji.

![Drzewo decyzyjne dzieli obszar funkcji][5]

***Drzewo decyzyjne dzieli obszar funkcji w regionach mniej więcej jednolitego wartości***

Miejsca funkcji mogą zostać podzielone na arbitralnie małych regionach, dlatego łatwo sobie wyobrazić, dzieląc go wystarczająco drobno mieć jeden punkt danych w danym regionie — skrajnym przykładem nadmierne dopasowanie. Aby tego uniknąć, duży zestaw drzewa są wykonane z matematycznych szczególną uwagę drzewa nie są skorelowane. Średnia ta "decyzja"lasu jest drzewa, która pozwala uniknąć nadmierne dopasowanie. Decyzja lasów można użyć dużej ilości pamięci. Dżungla decyzji są wariant, który zużywa mniej pamięci kosztem nieco więcej czasu szkolenia.

Drzewa decyzyjne promowanego uniknąć nadmierne dopasowanie poprzez ograniczenie, ile razy można należy podzielić i jak najmniejszej liczby punktów danych są dozwolone w każdym regionie. Algorytm konstrukcje sekwencji drzew, z których każdy dowiaduje się, aby zrekompensować błąd pozostawione przez drzewo przed. Wynik jest bardzo dokładne uczącego się, które korzystają z dużej ilości pamięci. Pełny techniczny opis Sprawdź [oryginalny papieru Friedmana](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Szybkie lasu kwantyl regresji](https://msdn.microsoft.com/library/azure/dn913093.aspx) jest odmianą drzewa decyzji za szczególny przypadek, w którym chcesz wiedzieć, nie tylko typowe (Mediana) wartości danych w obrębie regionu, ale także jego dystrybucji w postaci kwantyli.

### <a name="neural-networks-and-perceptrons"></a>Sieci neuronowych i perceptrons

Sieci neuronowych są INSPIROWANE nauki algorytmów obejmujące problemy [wielu klas](https://msdn.microsoft.com/library/azure/dn906030.aspx), [dwie klasy](https://msdn.microsoft.com/library/azure/dn905947.aspx)i [regresji](https://msdn.microsoft.com/library/azure/dn905924.aspx) . Pochodzą one w nieskończoną różnorodność, ale wszystkie formy ukierunkowanego acykliczne wykresu są sieci neuronowych w ramach usługą sieci Web. Oznacza to, że funkcji wprowadzania są przekazywane do przodu (nigdy nie do tyłu) przez sekwencję warstwy po czym zwrócił się do wyjścia. W każdej warstwie danych wejściowych są ważone w różnych kombinacjach, sumowane i przekazywane do następnej warstwy. Ta kombinacja prostych obliczeń powoduje możliwość uczenia się wyrafinowane klasy granice i trendy w danych, pozornie przez magic. Z warstwami wielu sieci tego rodzaju wykonać "głębokie uczenie się" tyle tech raportowania i fantastyki.

Wysoka wydajność nie przychodzi za darmo, choć. Sieci neuronowych może zająć dużo czasu do pociągu, szczególnie w przypadku dużych zestawów danych z wielu funkcji. Mają też więcej parametrów niż większość algorytmów, które oznacza kominów parametr wiele rozszerza czasu szkoleń.
A dla tych overachievers, którzy chcą [określić własną strukturę sieci](http://go.microsoft.com/fwlink/?LinkId=402867), możliwości są niewyczerpane.

<a name="boundaries-learned-by-neural-networks6"></a>![Granice dowiedział się przez sieci neuronowych][6]
---------------------------

***Granice dowiedział się przez sieci neuronowych mogą być złożone i nieregularne***

[Dwie klasy uśredniane perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) jest odpowiedzi sieci neuronowych filmami razy szkolenia. Używa struktury sieci, który daje liniowe klasy granice. Jest prawie pierwotnych na dzisiejsze standardy, ale ma długą historię pracy solidnie i jest na tyle mała, aby dowiedzieć się szybko.

### <a name="svms"></a>Bezpieczne maszyny wirtualne

Obsługa wektor maszyny (Svm) znaleźć granicy, który oddziela klasy według jako szeroki margines, jak to możliwe. Gdy dwie klasy nie może być wyraźnie oddzielone, algorytmy znaleźć najlepsze granicę, którą mogą one. Jak napisano analityków, [dwie klasy SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) robi to tylko linię prostą. (W SVM speak używa jądra liniowe.) Ponieważ sprawia, że tego zbliżenia liniowej, jest w stanie działać dość szybko. Tam, gdzie naprawdę świeci jest intensywny funkcji danych, takich jak tekst lub genomu. W tych przypadkach bezpieczne maszyny wirtualne mogą służyć do oddzielania klas, szybciej i przy mniejszej nadmierne dopasowanie niż większość innych algorytmów, oprócz wymogu niewielką ilość pamięci.

![Klasa pomocy technicznej wektor granicę][7]

***Granicę techniczną wektor maszyny klasy maksymalizuje margines między dwoma klasami***

Inny produkt Microsoft Research, [SVM lokalnie głębokie dwie klasy](https://msdn.microsoft.com/library/azure/dn913070.aspx) jest nieliniowy wariant SVM, który zachowuje większość efektywności szybkość i pamięć liniowy wersji. To idealne rozwiązanie dla przypadków, gdzie liniowe podejście nie daje wystarczająco dokładnych odpowiedzi. Deweloperzy w posiadaniu go szybko rozbijając problemu na kilka małych liniowych problemów SVM. Przeczytaj [Pełny opis](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) szczegółowe informacje na temat jak ściągnął one tej lewie.

Za pomocą rozszerzenia mądry svms nieliniowych, [jedna klasa SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) rysuje granicy, który ściśle przedstawia cały zestaw danych. Jest to przydatne do wykrywania anomalii. Wszelkie nowe punkty danych, które wykraczają daleko poza tej granicy są nietypowe jest godny uwagi.

### <a name="bayesian-methods"></a>Metody Bayesa

Metody Bayesa mają wysoce pożądane jakości: unikają nadmierne dopasowanie. Robią to przez założenie wcześniej o dystrybucji prawdopodobnie odpowiedź. Inny produkt uboczny tego podejścia jest to, że mają bardzo mało parametrów. Uczenie maszynowe Azure ma Oba algorytmy Bayesa dla klasyfikacji ([maszyny punktu Bayesa dwie klasy](https://msdn.microsoft.com/library/azure/dn905930.aspx)) i Regresja ([regresji liniowej Bayesa](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Należy zauważyć, że te założono, że dane można podzielić lub mieści się z linią prostą.

Na nocie historyczne maszyny punktu Bayesa zostały opracowane w Microsoft Research. Mają one niektóre wyjątkowo piękne prace teoretyczne za nimi. Zainteresowane uczeń jest skierowany do [oryginalnego artykułu w JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) i [wnikliwe blog przez Chris Bishop](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Specjalne algorytmy

Jeśli masz bardzo specyficzny cel może być szczęście. W zbiorze usługą sieci Web istnieją algorytmy, które specjalizują się w rangi przewidywania ([Liczba porządkowa regresji](https://msdn.microsoft.com/library/azure/dn906029.aspx)), przewidywania count ([regresji Poissona](https://msdn.microsoft.com/library/azure/dn905988.aspx)) i wykrywania anomalii, (jeden na podstawie [analizy głównych komponentów](https://msdn.microsoft.com/library/azure/dn913102.aspx) i opartym na s [obsługuje maszyna wektorów](https://msdn.microsoft.com/library/azure/dn913103.aspx)).
I ma lone klastrowanie algorytmu również ([K oznacza](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)).

![Wykrywanie na podstawie UPW anomalii][8]

***Wykrywanie na podstawie UPW anomalii*** *-zdecydowana większość danych wypada w stereotypowe dystrybucji; punkty nieodpowiadających znacznie że dystrybucji są podejrzane*

![Grupowane przy użyciu K oznacza zestaw danych][9]

***Zestaw danych jest zgrupowane w 5 klastry przy użyciu środków K***

Istnieje również zespół [wspólnotowy klasyfikatora jeden v wszystko](https://msdn.microsoft.com/library/azure/dn905887.aspx), co dzieli problem klasyfikacji klasy N na problemy związane z klasyfikacją dwie klasy n-1. Dokładność, czas szkolenia i liniowość właściwości są określane przez klasyfikatorów dwie klasy używane.

![Dwie klasy klasyfikatorów połączone, tworząc klasyfikatora trzy klasy][10]

***Para klasyfikatorów dwie klasy składają się na trzy klasy klasyfikatora***

Uczenie maszynowe Azure oferuje również dostęp do struktury learning wydajny komputer pod tytułem [Vowpal w zestawieniu](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf).
VW przeczy kategoryzację tutaj, ponieważ dowiedzieć się klasyfikacji i Regresja problemy, a nawet dowiedzieć się od częściowo bez etykiety danych. Można skonfigurować go do używania dowolną liczbę nauka algorytmów, utrata funkcji i algorytmów optymalizacji. Została zaprojektowana od podstaw się być efektywne, równoległych i niezwykle szybki. Obsługuje on zestawy funkcji nadzwyczaj wielkie przy minimalnym nakładzie pracy jawnego.
Rozpoczęte i prowadzone przez Microsoft Research własnych John Langford, VW jest wpis formuły w polu algorytmów samochodów. Nie każdy problem pasuje do VW, ale jeśli Ciebie nie może być warto poświęcić chwilę pokonanie krzywej uczenia się na jego interfejsie. Jest również dostępna jako [autonomiczny otwartego kodu źródłowego](https://github.com/JohnLangford/vowpal_wabbit) w kilku językach.


<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
