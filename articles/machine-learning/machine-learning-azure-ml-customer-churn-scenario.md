<properties
    pageTitle="Analizowanie Churn klienta przy użyciu komputera nauki | Microsoft Azure"
    description="Analiza przypadku rozwoju zintegrowanego modelu do analizowania i wyników pochodząca klienta"
    services="machine-learning"
    documentationCenter=""
    authors="jeannt"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="jeannt"/>

# <a name="analyzing-customer-churn-by-using-azure-machine-learning"></a>Analizowanie Churn klienta przy użyciu nauki maszynowego Azure

##<a name="overview"></a>Omówienie
W tym temacie przedstawiono implementacji wbudowanej przy użyciu Azure maszynowego uczenia Studio projektu analizy pochodząca klienta. Go w tym artykule omówiono skojarzone ogólnego modeli całościowe rozwiązanie problemu pochodząca przemysłowe klienta. Firma Microsoft również Zmierz dokładności modeli utworzonych przy użyciu komputera szkoleniowe i możemy ocenić wskazówkami dotyczącymi dalszego rozwoju.  

### <a name="acknowledgements"></a>Potwierdzenia

Tego doświadczenia opracowano i testowanych Serge Berger, kapitału naukowca danych firmy Microsoft i Roger Barga, wcześniej Menedżer produktów firmy Microsoft Azure maszynowego szkoleniowe. Azure zespołem gratefully potwierdza ich wiedzy i dzięki ich udostępniania ten dokument.

>[AZURE.NOTE] Dane użyte do tego doświadczenia nie jest dostępna publicznie. Na przykład sposób tworzenia modelu nauki komputera na potrzeby analizy pochodząca zobacz: [Telco churn modelu szablonu](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) w [Galerii analizy Cortana](http://gallery.cortanaintelligence.com/)


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

##<a name="the-problem-of-customer-churn"></a>Problem pochodząca klienta
Firmy w rynku dla klientów indywidualnych i wszystkie sektory enterprise Musisz zająć się pochodząca. Czasami pochodząca jest nadmiarowe i wpływa na zasady decyzji. Tradycyjne rozwiązaniem jest przewidywanie churners skłonność wysoki i adres potrzebami za pośrednictwem usługi Konsjerż kampanii marketingowych lub przez zastosowanie specjalnego zwolnień. Tych metod może być różna z branży branżowe, nawet z klastrze określonego dla klientów indywidualnych w obrębie jednej branżowe (na przykład telekomunikacyjnych).

Typowe czynniki jest firmy potrzeby zminimalizować tych działań przechowywania specjalne klienta. W związku z tym naturalne metodologii będzie wynik każdego klienta przy prawdopodobieństwie pochodząca i adres N pierwszych z nich. Główni kontrahenci mogą być przynoszące największe zyski z nich; na przykład w bardziej zaawansowanych scenariuszy, funkcja zysku zatrudnienia podczas wybór kandydatów specjalne zwolnienia. Jednak tylko w ramach kompleksowy strategii dotyczącej pochodząca są następujące zagadnienia. Firmy muszą uwzględniać ryzyka konta (i skojarzone ryzyka uszkodzenia), poziom i koszt interwencji i segmentacji wiarygodne klienta.  

##<a name="industry-outlook-and-approaches"></a>Program outlook branżowe i metod
Zaawansowane obsługi pochodząca jest znak dojrzałych branży. Przykład klasyczny jest branży telekomunikacyjnej, gdzie subskrybentów wiadomo, że często przełączać się między jeden dostawca. Ten dobrowolne pochodząca jest podstawowym. Ponadto dostawców mają (skumulowane) znaczną wiedzę na temat *churn sterowniki*, które są czynniki że klienci dysk, aby przełączyć się.

Na przykład wybór słuchawki lub urządzenie jest znanego sterownik pochodząca firmy telefonu komórkowego. W wyniku popularne zasad jest subsydiują cenę słuchawka dla nowych subskrybentów, ładowana pełną cenę dla obecnych klientów do uaktualnienia. Ze względów historycznych tych zasad doprowadziło klientom skaczące z jednego dostawcę innemu użytkownikowi uzyskanie nowego rabatu z kolei ma monit dostawców, aby dostosować ich strategii.

Wysoka zmienności ofertą słuchawki jest bardzo szybko unieważnia modeli pochodząca, które są oparte na modelach słuchawki bieżącego współczynnik. Ponadto telefony komórkowe są tylko telekomunikacyjnej urządzeń; są one również instrukcje sposób (należy rozważyć, czy telefonu iPhone), a te społecznościowych zmienne predykcyjne wykraczają poza zakres zwykła telekomunikacyjnych zestawów danych.

Wynikiem umożliwia modelowanie to, że wystarczy usuwaniu znane przyczyny pochodząca nie opracowywania zasady dźwięku. W rzeczywistości strategii ciągły modelowania, w tym klasyczny modeli, w których określenie kategorii zmienne (na przykład drzewa decyzji), jest **wymagane**.

Za pomocą dużych zestawów danych na swoich klientów, organizacje wykonywania analizy danych duży (w szczególności, na podstawie danych duży wykrywania pochodząca) jako skutecznych rozwiązaniem tego problemu. Można znaleźć więcej informacji na temat danych duży sposobem problem pochodząca w zalecenia dotyczące sekcji ETL.  

##<a name="methodology-to-model-customer-churn"></a>Metodologii pochodząca klienta modelu
Typowych proces rozwiązywania problemów rozwiązać pochodząca klienta są przedstawione na ilustracji 1-3:  

1.  Model ryzyka umożliwia należy rozważyć wpływ akcje prawdopodobieństwa i ryzyka.
2.  Model interwencji umożliwia należy rozważyć, czy jak poziom interwencji może mieć wpływ na prawdopodobieństwo pochodząca i ilość klienta wartość okresu istnienia (CLV).
3.  Analiza ta jest przydatna w analizie jakościowej, który zostanie przekazany do kampanii marketingowej aktywne jest przeznaczony dla segmentów klientów do przeprowadzania optymalnego oferty.  

![][1]

Tej metody do przodu wyglądających jest najlepszy sposób traktowania pochodząca, ale pochodzi z złożoność: mamy opracowywaniu wiele modeli archetype i Śledź zależności między modelami. Może być encapsulated interakcji między modelami, jak pokazano na poniższym rysunku:  

![][2]

*Rysunek 4: Unified archetype wiele modeli*  

Interakcji między modelami jest kluczem Jeśli nie będziemy do przeprowadzania kompleksowe podejście do przechowywania klienta. Każdy model niekoniecznie obniża w czasie; Dlatego architektura jest niejawne pętli (podobnie jak archetype określonych przez standard wyszukiwania danych WYSOKĄ DM, [***3***]).  

Cykl ogólnego ryzyka decyzji obrotu segmentacji-rozkładu jest nadal uogólniony struktury, która ma zastosowanie do wielu problemów. Analiza pochodząca jest po prostu przedstawiciela silnych tej grupy problemów, ponieważ wskazuje wszystkie cechy problemu biznesowych, które nie zezwala na rozwiązanie przewidywanych uproszczone. Społeczne aspekty nowoczesny sposobem churn nie są szczególnie wyróżnione w ujęciu, ale społeczne aspekty znajdują się w archetype modelowania, tak jak powinny, w dowolnym modelu.  

W tym miejscu interesujące dodanie jest duży danych analizy. Telekomunikacyjnej dzisiejszą a datą sprzedaży detalicznej firmy zbieranie pełne dane o swoich klientów, a firma Microsoft łatwo zapewnienie, że trzeba się wiele modeli łączności staną się trendu typowych podane pojawiających się trendów, takich jak Internet czynności i powszechnie stosowana urządzeń, które pozwalają business, aby zastosować inteligentne rozwiązań u wielu warstw.  

 
##<a name="implementing-the-modeling-archetype-in-machine-learning-studio"></a>Wykonania archetype modelowania maszynowego uczenia Studio
Mieć problem, po prostu opisano, co to jest najlepszym sposobem na wdrożenie zintegrowanej modelowanie i wyników metody? W tej sekcji możemy przedstawi, jak firma Microsoft osiągnąć ten przy użyciu Azure maszynowego uczenia Studio.  

Wiele modeli podejście jest musi podczas projektowania globalnej archetype dla pochodząca. Nawet wyników (przewidywanych) część podejście powinny być wiele modeli.  

Na poniższym diagramie przedstawiono prototypu utworzonej firma Microsoft, która używa czterech wyników algorytmy maszynowego uczenia Studio przewidywanie pochodząca. Przyczyna przy użyciu metody wiele modeli nie jest tylko do tworzenia klasyfikatora zespół, aby zwiększyć dokładność, ale również chronić się przed nadmierne instalowania i poprawić wybór funkcji porady.  

![][3]

*Rysunek 5: Prototypu: pochodząca modelowania podejście*  

W poniższych sekcjach przedstawiono szczegółowe informacje o prototypu wyników modelu, który wprowadziliśmy przy użyciu komputera nauki Studio.  

###<a name="data-selection-and-preparation"></a>Zaznaczanie danych i przygotowywania
Dane użyte do utworzenia modeli i klienci wynik pochodzi z rozwiązania pionowa CRM z danymi zastosowanym mieszaniem nazw ochrony prywatności klientów. Dane zawierają informacje dotyczące 8000 subskrypcji w Stanach Zjednoczonych i składa się z trzech źródeł: inicjowania obsługi administracyjnej danych (metadane subskrypcji), dane działania (użycie systemu) i dane obsługi klientów. Dane nie zawiera każdej firmy pokrewne informacje dotyczące klientów; na przykład nie zawiera wyniki lojalności metadanych lub faktury.  

Dla uproszczenia ETL i procesów oczyszczania danych są poza zakresem ponieważ przyjęto założenie, że przygotowanie danych ma już zostały wykonane w innych miejscach.   

Wybieranie funkcji dla modelowania jest oparty na wstępny istotność wyników zestawu zmienne predykcyjne, zawarte w procesie używa modułu las losowe. Do wykonania w komputerze nauki Studio możemy obliczana średnia, mediana i zakresów przedstawiciela funkcji. Na przykład dodana agregacji dla jakościowy danych, takich jak wartości minimalne i maksymalne dla aktywności użytkownika.    

Możemy również przechwytywane czasowy informacje o najnowszych sześć miesięcy. Firma Microsoft analizowane dane jeden rok i firma Microsoft ustalone, że nawet jeśli dokonano statystyczne znaczną trendów, wpływ na pochodząca będzie znacznie mniejsza po sześć miesięcy.  

Najbardziej istotne jest całego procesu, łącznie z ETL, wybór funkcji i modelowania został wdrożony Studio nauki komputera przy użyciu źródeł danych w programie Microsoft Azure.   

Następujące plany przedstawianie danych, który został użyty.  

![][4]

*Rysunek 6: Fragment źródła danych (zastosowanym mieszaniem nazw)*  

![][5]


*Rysunek 7: Funkcje wyodrębnionych z źródła danych*
 
> Należy zauważyć, że te dane są prywatne i dlatego nie można udostępnić modelu i danych.
> Jednak dla modelu podobne za pomocą dostępnych publicznie danych, zobacz ten przykład eksperymentować w [Galerii analizy Cortana](http://gallery.cortanaintelligence.com/): [Telco Churn klienta](http://gallery.cortanaintelligence.com/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Aby dowiedzieć się więcej na temat sposoby implementowania modelu analizy pochodząca za pomocą pakietu analizy Cortana, zaleca się też [w tym klipie wideo](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) przez Menedżera programów wyższych Wee Hyong Tok. 
> 

###<a name="algorithms-used-in-the-prototype"></a>Algorytmów w prototypu

Użyliśmy następujące cztery algorytmy nauki maszynowego do utworzenia prototypu (bez dostosowania):  

1.  Regresją (LR)
2.  Drzewa decyzji zwiększane (BT)
3.  Średnią perceptron (aplikacji)
4.  Obsługa wektorowa komputera (SVM)  


Na poniższym diagramie przedstawiono część powierzchni projektowej doświadczenia wskazuje sekwencji, w której utworzono wzorów:  

![][6]  


*Rysunek 8: Tworzenie modeli w komputerze nauki Studio*  

###<a name="scoring-methods"></a>Metody punktów
Firma Microsoft uzyskał czterech modeli przy użyciu zestawu danych szkolenie oznaczona etykietą.  

Możemy również złożone wyników zestawu danych do modelu porównywalna utworzono przy użyciu klasycznej wersji programu 12 analizatora Enterprise skojarzeń zabezpieczeń. Firma Microsoft mierzone dokładności modelu skojarzeń zabezpieczeń i wszystkich czterech modeli Studio nauki komputera.  

##<a name="results"></a>Wyniki
W tej sekcji firma Microsoft przedstawia uzyskanych danych o dokładności modeli, opartych na wyników zestawu danych.  

###<a name="accuracy-and-precision-of-scoring"></a>Dokładność i precyzja wyników
Implementacja w Azure maszynowego uczenia jest zwykle za skojarzeń zabezpieczeń w dokładność o około 10 do 15% (obszar w obszarze krzywej lub AUC).  

Jednak najważniejszych metryki w pochodząca jest wskaźnikiem błędu klasyfikacji: oznacza to, że z churners góry N jako przewidywane przez klasyfikator, które z nich faktycznie czy **nie** pochodząca i nieodebranych jeszcze specjalnego traktowania? Poniższy diagram porównuje tego kursu błędną klasyfikację dla wszystkich modeli:  

![][7]


*Rysunek 9: Passau prototypu obszarze krzywej*

###<a name="using-auc-to-compare-results"></a>Porównanie wyników za pomocą AUC
Obszar w obszarze krzywej (AUC) jest metryki reprezentujący globalnej miarę *odrębność* między podziału wyniki dla populacji dodatnie i ujemne. Jest podobne do tradycyjnych wykresu cechy Operator odbiorcy (ROC), ale jedna różnica ważne jest, że metryka AUC nie wymaga można wybrać wartość progowa. Zamiast tego podsumowuje wyniki na **Wszystkie** możliwe opcje. Natomiast tradycyjnych wykresie ROC są wyświetlane dodatniej stawki na osi pionowej i FAŁSZ dodatniej stawki na osi poziomej, a próg klasyfikacji zmienia się.   

AUC jest zazwyczaj używany jako miara wartości dla różnych algorytmów (lub inne systemy) ponieważ umożliwia modeli do porównania za pomocą ich wartości AUC. Jest to podejście popularne w branży, takich jak meteorologia i biosciences. W związku z tym AUC reprezentuje popularne narzędzia do oceny wydajności klasyfikatora.  

###<a name="comparing-misclassification-rates"></a>Porównanie stawek błędów klasyfikacji
Firma Microsoft porównywane stawki błędną klasyfikację w zestawie danych w danym za pomocą danych CRM około 8000 subskrypcji.  

-   Wskaźnik błędu klasyfikacji skojarzeń zabezpieczeń był 10 15%.
-   Wskaźnik błędu klasyfikacji maszynowego uczenia Studio był 15-20% dla churners góry 200-300.  

W branży telekomunikacyjnej ważne jest adresem tylko klientów, którzy mają najwyższe ryzyko churn oferując im usługi Konsjerż lub innych specjalnego traktowania. W tym zakresie implementacji maszynowego uczenia Studio uzyskuje wyniki równi modelu skojarzeń zabezpieczeń.  

W ten sam sposób dokładność jest od dokładności, ponieważ firma Microsoft interesuje głównie poprawnie klasyfikacji potencjalnych churners.  

Poniższy diagram z serwisu Wikipedia przedstawia relacje w grafice barwnym, łatwych do zrozumienia:  

![][8]

*Rysunek 10: Zależnościami między dokładność i precyzja*

###<a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Dokładność i precyzja wyników dla modelu drzewa decyzji zwiększane  

Poniżej zostaną wyświetlone wyniki nieprzetworzonych wyników za pomocą prototypu maszynowego uczenia modelu drzewa decyzji zwiększane się najbardziej dokładne między czterech modeli:  

![][9]

*Rysunek 11: Właściwości modelu drzewa decyzji zwiększane*

##<a name="performance-comparison"></a>Porównanie wydajności
Firma Microsoft porównywane szybkości co danych został uzyskał przy użyciu modeli maszynowego uczenia Studio i model porównywalna utworzone przy użyciu klasycznej wersji programu 12.1 analizatora Enterprise skojarzeń zabezpieczeń.  

W poniższej tabeli przedstawiono działanie algorytmów:  

*Tabela 1. Ogólnej wydajności (dokładność) algorytmów*

| LR|BT|APLIKACJI|SVM|
|---|---|---|---|
|Model średni|Najlepsze modelu|Gorszych wynikach w przypadku|Model średni|

Modele obsługiwany w outperformed nadzoru przez 15-25% na szybkość wykonanie, ale dokładność została głównie na Cena_nom Studio nauki komputera.  

##<a name="discussion-and-recommendations"></a>Dyskusji i zalecenia
W branży telekomunikacyjnej pojawiło się kilka wskazówek, aby przeanalizować pochodząca, w tym:  

-   Pochodzić metryki dla cztery podstawowe kategorie:
    -   **Jednostki (na przykład subskrypcji)**. Obsługa administracyjna podstawowe informacje o subskrypcji i/lub klienta, który jest temat pochodząca.
    -   **Działania**. Uzyskanie wszystkie informacje o możliwych zastosowania związanego z obiektu, na przykład numer logowania.
    -   **Pomoc techniczna dla klientów**. Zebrać informacje z dzienników pomocy technicznej klienta w celu wskazania, czy subskrypcja nie problemy lub interakcji z obsługą klienta.
    -   **Dane biznesowe i konkurencyjności**. Uzyskiwać możliwe informacji na temat klienta (na przykład, może być niedostępny lub trudne do śledzenia).
-   Za pomocą znaczenie wybór funkcji dysk. Oznacza to, model drzewa decyzji zwiększane jest zawsze podejście składając.  

Użyj tych czterech kategoriach tworzy wrażenie, który prosty *deterministyczne* podejście, na podstawie indeksów utworzonych w odpowiednim czynników według kategorii, czy wystarczy do identyfikowania odbiorców na ryzyko dla pochodząca. Niestety pojęcie to wydaje się wiarygodne, ale jest FAŁSZ opis. Przyczyny jest pochodząca to czasowy efekt oraz czynników uczestniczy churn są zwykle zapisywane w stanach przejściowych. Co prowadzi klienta brać pod uwagę opuszczania dzisiaj może być inny dzień, a na pewno będzie różnych sześć miesięcy od teraz. Dlatego *prawdopodobieństwa* model jest konieczność.  

Ten ważne obserwacji często są pomijane w firmie, które zwykle chce podejście zorientowane na analiz biznesowych analizy, głównie, ponieważ jest łatwiejszy sprzedaży i dopuszcza proste automatyzacji.  

Gwarantuje Samoobsługowe analizy przy użyciu komputera nauki Studio jest jednak, że cztery kategorie informacji, sortowane według oddział lub dział stają się przydatne źródła maszynowego poznania pochodząca.  

Inną możliwość ciekawy przychodzącym Azure maszynowego uczenia jest możliwość dodawania modułu niestandardowych do repozytorium wstępnie zdefiniowanych modułów, które są już dostępne. Ta funkcja tworzy zasadniczo możliwość wybierz bibliotek i Utwórz szablony w pionie rynkach. Jest ważne informatyczne Azure maszynowego uczenia rynku.  

Firma Microsoft dotyczy nadal w tym temacie w przyszłości, szczególnie dotyczące analizy danych duży.
  
##<a name="conclusion"></a>Wnioski
W tym dokumencie opisano za pośrednictwem sposobem rozwiązaniu problemu typowych pochodząca klienta przy użyciu struktury ogólne. Pracujemy traktowane jako prototypu wyników modeli i zaimplementowana przy użyciu Azure maszynowego uczenia. Na koniec możemy ocenić dokładność i wydajność rozwiązania prototypu w odniesieniu do porównywalna algorytmy skojarzeń zabezpieczeń.  

**Aby uzyskać więcej informacji:**  

Czy ten dokument pomagają? Należy przekazać nam opinię. Przekaż nam w skali od 1 (niska) do 5 (doskonałe), jak oceniasz tego dokumentu i dlaczego możesz spowodowały go tę ocenę? Na przykład:  

-   Są możesz ocenianie go wysoka z powodu konieczności przykłady dobrych, zrzuty ekranu doskonałe, wyczyść pole wyboru pisania lub innego powodu?
-   Są możesz ocenianie go niski z powodu niskiej przykładów, zrzutów ekranu rozmyty lub niejasne pisania?  

Ta opinia będzie pomoc w ulepszaniu jakości oficjalnych, które wydania.   

[Przesyłanie opinii](mailto:sqlfback@microsoft.com).
 
##<a name="references"></a>Odwołania
Analizy przewidywanych [1] : poza przewidywań, Zachodnia McKnight zarządzania informacjami, lipca i sierpnia 2011, p.18 20.  

Artykuł Wikipedia [2] : [dokładność i precyzja](http://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [WYSOKĄ DM 1.0: przewodnik krok po kroku danych wyszukiwania dla](http://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [Marketing big Data: prowadzenie bardziej efektywną komunikację klientów i dysk wartość](http://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Telco churn modelu szablonu](http://gallery.cortanaintelligence.com/Experiment/Telco-Customer-Churn-5) w [Galerii analizy Cortana](http://gallery.cortanaintelligence.com/) 
 
##<a name="appendix"></a>Dodatek

![][10]

*Rysunek 12: Migawki prezentacji w pochodząca prototypu*


[1]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-1.png
[2]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-2.png
[3]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-3.png
[4]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-4.png
[5]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-5.png
[6]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-6.png
[7]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-7.png
[8]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-8.png
[9]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-9.png
[10]: ./media/machine-learning-azure-ml-customer-churn-scenario/churn-10.png
