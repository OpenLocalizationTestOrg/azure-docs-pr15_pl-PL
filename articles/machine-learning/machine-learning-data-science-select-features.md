<properties
    pageTitle="Funkcja zaznaczenia w procesie nauki danych zespołu | Microsoft Azure" 
    description="W tym miejscu wyjaśniono przeznaczenie wybór funkcji oraz przykłady ich roli w procesie ulepszania danych maszynowego uczenia."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-selection-in-the-team-data-science-process-tdsp"></a>Wybieranie funkcji proces nauki danych (TDSP) zespołu

W tym artykule wyjaśniono, w celu zaznaczenia funkcji i znajdują się przykłady jego rolę w procesie ulepszania danych maszynowego uczenia. W tych przykładach są pobierane z Azure maszynowego uczenia Studio. 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


W tym temacie omówiono przeznaczenie wybór funkcji i znajdują się przykłady jego rolę w procesie ulepszania danych maszynowego uczenia. W tych przykładach są pobierane z Azure maszynowego uczenia Studio. 

Wybieranie funkcji i inżynierskie jest częścią TDSP przedstawionych w [Co to jest proces zespołu danych naukowych?](data-science-process-overview.md). Funkcja techniczny i wybierania są częściami w etapie **rozwoju funkcji** TDSP.

* **Funkcja techniczny**: ten proces pozwala podjąć próbę, aby utworzyć dodatkowe funkcje odpowiednich z istniejących funkcji nieprzetworzonych danych, a aby zwiększyć możliwości przewidywanych Algorytm uczenia.

* **Wybieranie funkcji**: ten proces zaznacza klucza podzestaw oryginalny funkcji danych w celu zmniejszenia wymiarze problemu szkolenie.

Zwykle **Funkcja techniczny** najpierw stosowana jest wygenerować dodatkowe funkcje, a następnie kroku **Wybór funkcji** jest wykonywana w celu wyeliminowania nie ma znaczenia, zbędne lub wysoce skorelowany funkcje.


## <a name="filtering-features-from-your-data---feature-selection"></a>Funkcji filtrowania danych — wybór funkcji 

Wybieranie funkcji jest procesem, który jest często stosowana budowy zestawy danych szkolenia dla przewidywanych modelowania zadań, takich jak klasyfikacji lub regresji zadań. Celem jest wybrać podzbiór funkcji z oryginalnego zestawu danych zmniejszyć wymiary przy użyciu minimalnego zestawu funkcji reprezentować maksymalnej ilości odchylenie w danych. Następnie ten podzestaw funkcje są tylko funkcje do uwzględnienia przeszkolenie modelu. Wybieranie funkcji służy dwie funkcje.

* Najpierw wybór funkcji często zwiększa dokładność klasyfikacji o usuwaniu nie ma znaczenia, zbędne lub ściśle powiązane funkcje.
* Drugi obniża wiele funkcji, dzięki czemu bardziej efektywne szkolenie modelu. To jest szczególnie istotne w przypadku uczestników, które są drogich na szkolenie, takich jak maszyny wektorowa pomocy technicznej.

Chociaż wybór funkcji Szukanie wyniku zmniejszyć liczbę funkcji w zestawie danych umożliwia przeszkolenie modelu, go nie zazwyczaj odwołuje się do terminów "wymiarze redukcji". Funkcja metody wyodrębnić podzbiór funkcji oryginalne dane bez zmieniania ich.  Metod redukcji wymiarze zastosować odtwarzania funkcje, które mogą Przekształcanie funkcji oryginalnego, a więc je zmodyfikować. Przykładami metod redukcji wymiarze analizy składnik kapitału, analiza korelacji kanonicznych i dekompozycji pojedynczą wartość.

Między innymi jedną kategorię powszechnie stosowane metody funkcji w kontekście pod nadzorem nosi nazwę "Wybór funkcji filtr oparty". Tych metod obliczania korelacji między wszystkimi funkcjami i atrybut docelowy, zastosować środek statystyczne przypisuje wynik do poszczególnych funkcji. Funkcje są klasyfikowane następnie według wyników, które mogą być używane do ustalić próg zachowanie i usuwaniu określoną funkcję. Przykładami statystyczne miar używane w tych metodach korelacji osób, wzajemnego informowania i test chi-kwadrat.

W Azure maszynowego uczenia Studio istnieje moduły przewidziane wybór funkcji. Jak pokazano na poniższym rysunku, te moduły zawiera [filtr oparty wybór funkcji] [ filter-based-feature-selection] i [Fishera liniowej analizy Discriminant][fisher-linear-discriminant-analysis].

![Przykład zaznaczenia funkcji](./media/machine-learning-data-science-select-features/feature-Selection.png)


Na przykład, rozważ zastosowanie [filtr oparty wybór funkcji] [ filter-based-feature-selection] modułu. W celu ułatwienia możemy nadal korzystać w powyższym przykładzie wyszukiwania tekstu. Przyjęto założenie, potrzebne do utworzenia modelu regresji po utworzeniu zestawu funkcji 256 za pośrednictwem [Funkcji mieszania] [ feature-hashing] modułu, a zmienna odpowiedzi jest "Kol1" reprezentuje książki Przejrzyj ocen z zakresu od 1 do 5. Ustawienie "Funkcji wyników metody" jako "Pearsona korelacji", "kolumnie docelowej" jako "Kol1" i "Liczbę żądanych funkcji" do 50. Następnie moduł [filtr oparty wybór funkcji] [ filter-based-feature-selection] spowodują zestawu danych zawierającą 50 funkcji razem z atrybut docelowy "Kol1". Na poniższej ilustracji pokazano przepływu tego doświadczenia i parametrów wejściowych, po prostu opisane.

![Przykład zaznaczenia funkcji](./media/machine-learning-data-science-select-features/feature-Selection1.png)

Na poniższej ilustracji pokazano wyniku zestawy danych. Każdej funkcji jest uzyskał oparte na korelacji liniowej Pearsona między sobą i atrybut docelowy "Kol1". Funkcje z góry wyniki są zachowywane.

![Przykład zaznaczenia funkcji](./media/machine-learning-data-science-select-features/feature-Selection2.png)

Wyniki odpowiadające im wybranych funkcji przedstawiono na poniższym rysunku.

![Przykład zaznaczenia funkcji](./media/machine-learning-data-science-select-features/feature-Selection3.png)

Stosując ten [filtr oparty wybór funkcji] [ filter-based-feature-selection] moduł, 50 z 256 funkcje są zaznaczone, ponieważ zawierają funkcje najbardziej skorelowane ze zmienną docelowej "Kol1", na podstawie wyników metody "Korelacji liniowej Pearsona".

## <a name="conclusion"></a>Wnioski
Funkcja techniczny i wybór funkcji są dwa często odtwarzane i wybranych funkcji zwiększenia efektywności procesu szkolenia, którego próby wyodrębnić najważniejsze informacje zawarte w danych. Są również zwiększyć możliwości tych modeli dokładnie klasyfikowania danych wejściowych i bardziej trwała przewidywanie wyniki zainteresowania. Aby wprowadzić więcej praktyce tractable nauka można także połączyć techniczny funkcji i wybierania. Robi to ulepszanie, a następnie zmniejszanie liczby funkcje skalibrować lub przeszkolenie modelu. Matematycznie mówiąc, funkcje zaznaczone, aby przeszkolenie modelu są minimalny zestaw zmiennych niezależnych, które wyjaśnić trendy w danych, a następnie pomyślnie przewidywanie wyników.

Należy zauważyć, że nie zawsze jest zawsze przeprowadzić wybór funkcji techniczny lub funkcji. Czy jest potrzebna, czy nie zależy od tego, możemy mieć lub zbieranie dane, algorytmu, który firma Microsoft wybierz i celem doświadczenia.

<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
 
