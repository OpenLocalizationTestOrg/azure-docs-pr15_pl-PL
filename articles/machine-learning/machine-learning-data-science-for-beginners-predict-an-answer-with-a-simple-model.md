<properties
   pageTitle="Przewidywanie odpowiedzi przy użyciu prostego modelu - model regresji | Microsoft Azure"
   description="Jak utworzyć model regresja prosta przewidywanie cena w nauce danych dla początkujących wideo 4. Zawiera regresji liniowej danymi docelowej."                                  
   keywords="Tworzenie modelu, prostego modelu, przewidywania cena, model prostej regresji"
   services="machine-learning"
   documentationCenter="na"
   authors="cjgronlund"
   manager="jhubbard"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="cgronlun;garye"/>

# <a name="predict-an-answer-with-a-simple-model"></a>Przewidywanie odpowiedzi przy użyciu prostego modelu

## <a name="video-4-data-science-for-beginners-series"></a>Klip wideo 4: Nauki danych dla początkujących serii

Dowiedz się, jak tworzyć modele prostej regresji w celu przewidywanie cena rombu w nauce danych dla początkujących wideo 4. Firma Microsoft będzie rysować modelu regresji z danymi docelowej.

Aby w pełni wykorzystać serii, obejrzyj je wszystkie. [Przejdź do listy klipów wideo](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-predict-an-answer-with-a-simple-model]

## <a name="other-videos-in-this-series"></a>Inne klipy wideo z tej serii

*Nauka danych dla początkujących* jest krótkie wprowadzenie do nauki danych w pięciu krótkie klipy wideo.

  * Klip wideo 1: [5 pytaniach odpowiedzi nauki danych](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 s)*
  * Klip wideo 2: [jest teraz gotowy do nauki danych danych?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 s 56 min)*
  * Klip wideo 3: [Zadaj pytanie możesz odebrać z danymi](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 s 17 min)*
  * Klip wideo 4: Przewidywanie odpowiedzi przy użyciu prostego modelu
  * Klip wideo 5: [Kopiowanie pracy innych osób nauki danych](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 s)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Przebieg: Przewidywanie odpowiedzi przy użyciu prostego modelu

Witamy w czwartym wideo w "danych nauki dla początkujących" serii. W tym obiekcie pracujemy Tworzenie prostego modelu i wprowadź przewidywania.

*Model* jest uproszczone artykułu o naszych danych. I opisano I średnia.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Zbierz odpowiednich, dokładności, połączenie, za mało danych

Załóżmy że chcę sklep rombu. Mam dzwonienia, że do mojej babka z ustawieniem dla rombu 1.35 pojawia się znak daszka i chcę uzyskać ogólny obraz tego, ile będzie kosztować. Notatnik i pióra można wykonać w magazynie Biżuteria i I Zanotuj cena wszystkich romby w przypadku i ile oceny w carats. Zaczynając od pierwszego romb - carats 1.01 jego i $7,366.

Teraz przejść i wykonaj następujące czynności dla innych romby w magazynie.

![Kolumny danych romb](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Zwróć uwagę, że naszej listy ma dwie kolumny. Każda kolumna ma innym — waga w carats i cena — każdy wiersz jest jeden punkt danych reprezentujący pojedynczy rombu.

Firma Microsoft faktycznie utworzono niewielki danych Ustaw tutaj - tabeli. Zwróć uwagę, czy spełnia nasze kryteria jakości:

* Dane są **odpowiednie** - grubość ostatecznie dotyczy ceny
* Jest **dokładne** — firma Microsoft double-checked ceny modyfikujące w dół
* Jest **połączony** — na jeden z tych kolumn są dostępne nie spacji
* I poznamy, jest **wystarczająco** danych odpowiedzieć nasze pytanie

## <a name="ask-a-sharp-question"></a>Zadaj pytanie ostry

Teraz możemy będzie stanowić nasze pytanie w sposób ostrych: "ile będzie kosztować kupić rombu 1.35 pojawia się znak daszka?"

Naszej listy nie ma rombu 1.35 pojawia się znak daszka, dlatego będziemy musieli pozostałą część naszych danych umożliwia uzyskiwanie odpowiedzi na pytanie.

## <a name="plot-the-existing-data"></a>Kreśl istniejących danych

Najpierw robimy jest rysowanie linii poziomej numer, o nazwie osi na wykresie wagi. Zakres wagi jest 0-2, więc możemy będzie narysować linię zajmującego zakresu i umieszczenie znaczników dla każdego pół pojawia się znak daszka.

Firma Microsoft będzie dalej Rysowanie osi pionowej, aby zarejestrować ceny i połączyć go na osi poziomej grubość. Są to w jednostce dollars. Teraz mamy zestaw współrzędnych osi.

![Ciężar i cena osi](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Firma Microsoft zacząć teraz wykonać te dane i przekształcić *punktowe Wykreśl*. Jest to doskonały sposób na wizualizowanie zestawów danych liczbowych.

Dla pierwszego punktu danych Firma Microsoft eyeball linię pionową na 1.01 carats. Następnie firma Microsoft eyeball linii poziomej na $7,366. W przypadku, gdy spełniają, możemy rysować kropka. Jest to nasz pierwszy romb.

Teraz możemy przejść przez każdego romb na tej liście i wykonaj te same kroki. Kiedy jesteśmy za pośrednictwem to będziemy korzystać: wiele punktów, po jednej dla każdego romb.

![Wykres punktowy](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Rysowanie modelu punktów danych

Teraz możesz obejrzeć kropki i squint, kolekcji będzie wygląda tak: tłuszczu rozmyty linii. Firma Microsoft można wykonać naszych znacznik i narysować prostą linię przebiegającą przez nią.

Narysowanie linii, możemy utworzyć *modelu*. Pomyśl o tej jako sporządzanie rzeczywiste i wprowadzania wersję kreskówki simplistic go. Teraz kreskówki jest nieprawidłowy — w tym wierszu nie przejdź do wszystkich punktów danych. Ale jest przydatne uproszczenia.

![Regresji liniowej.](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Fakt, że wszystkie kropki nie przechodzą dokładnie wiersza jest przycisk OK. Naukowców danych to wyjaśnić przez informujący, że jest model — jest to linia - i następnie każdego punktu zawiera niektóre *hałasu* lub *Odchylenie* skojarzone z nim. Istnieje podstawowych idealnego relacji, a ma świata niepowtarzalne, rzeczywistą, który zapewnia hałasu i wątpliwości.

Ponieważ firma Microsoft próbujesz odpowiedzi na pytanie *Ilość?* jest to *regresji*. I użyto programu prostej, dlatego jest *regresji liniowej*.

## <a name="use-the-model-to-find-the-answer"></a>Aby znaleźć odpowiedzi za pomocą modelu

Teraz mamy modelu i prosimy go nasze pytanie: ile rombu 1.35 pojawia się znak daszka będzie kosztować?

Aby odebrać nasze pytanie, możemy oka 1.35 carats i narysować linię pionową. Gdy przecina ona linii modelu możemy eyeball linii poziomej do osi dolara. Trafienia go w prawo na 10 000. Wysięgnik! To odpowiedź: rombu 1.35 pojawia się znak daszka koszty około 10 000 zł.

![Znaleźć odpowiedzi na modelu](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Tworzenie przedział ufności

To naturalne do zastanawiasz się, jak dokładne jest to przewidywania. Jest przydatne dowiedzieć się, czy romb 1.35 pojawia się znak daszka będzie bardzo zbliżony do 10 000 zł, lub znacznie wyższy lub niższy. Aby znaleźć ten limit, Przejdźmy rysować koperty wokół zawierający najczęściej punktów linii regresji. Ten koperty nosi nazwę naszych *ufności*: jesteśmy zasadzie pewność, że ceny mieszczą się w tym koperta, ponieważ w ciągu ostatnich większość z nich są dostępne. Firma Microsoft rysować dwie linie poziome więcej, z którym prosta 1.35 pojawia się znak daszka przecina u góry i u dołu tego koperty.

![Przedział ufności](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Teraz mówi czegoś o naszych ufności: Firma Microsoft powiedzieć bezproblemowe że cena rombu 1.35 pojawia się znak daszka jest około $ 10 000 -, ale może to być możliwie jak 8000 zł i może być możliwie najwyższa 12 000.

## <a name="were-done-with-no-math-or-computers"></a>Firma Microsoft gotowe, bez matematycznych i komputerach

NAS, jakie naukowców danych uzyskiwanie poświęcona wykonaj i NAS tylko rysując:

* Firma Microsoft zadawane pytania firma Microsoft może odebrać z danymi
* Firma Microsoft wbudowany *modelu* przy użyciu *regresji liniowej.*
* Wprowadzono *przewidywania*, wraz z *przedziału ufności*

I firma Microsoft nie korzysta z matematyczne lub komputerów to zrobić.

Teraz, jeśli był było więcej informacji, takich jak...

* Przecięcie romb
* odmiany kolorów (jak Zamknij romb jest jest biały)
* Liczba włączeniem do romb

.. .i czy mamy więcej kolumn. W takim przypadku matematyczne staje się przydatne. Jeśli masz więcej niż dwie kolumny jest trudne do rysowania kropki na papierze. Funkcje matematyczne pozwala bardzo właściwego dopasowania tego wiersza lub tej płaszczyzny do danych.

Ponadto jeśli zamiast tylko kilku romby, było dwa tysiące lub miliony dwóch, a następnie wykonaj działające znacznie szybsze z komputerem.

Dzisiaj zostały zajmowaliśmy sposobów wykonywania regresji liniowej, a wprowadzono przewidywania, przy użyciu danych.

Pamiętaj wyewidencjonować inne klipy wideo "Danych nauki dla początkujących" z witryny Microsoft Azure maszynowego Learning.



## <a name="next-steps"></a>Następne kroki

  * [Spróbuj pierwszym doświadczeniu nauki danych z komputera nauki Studio](machine-learning-create-experiment.md)
  * [Wprowadzenie do nauki maszynowego w programie Microsoft Azure](machine-learning-what-is-machine-learning.md)
