<properties
   pageTitle="Zadaj pytanie, możesz odebrać z danych — sformułowania pytania | Microsoft Azure"
   description="Dowiedz się, jak sformułowania pytanie nauki danych w nauce danych dla początkujących klip wideo 3. Zawiera porównanie pytania dotyczące klasyfikacji i regresji."
   keywords="pytania nauki danych, sformułowania pytania, regresji pytań i klasyfikacji pytania, ostrych pytanie"
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

# <a name="ask-a-question-you-can-answer-with-data"></a>Zadaj pytanie, na które można odpowiedzieć z danymi

## <a name="video-3-data-science-for-beginners-series"></a>Klip wideo 3: Nauki danych dla początkujących serii

Dowiedz się, jak sformułowania pytanie nauki danych w nauce danych dla początkujących klip wideo 3. Ten klip wideo zawiera porównanie pytania dotyczące algorytmów klasyfikacji i regresji.

Aby w pełni wykorzystać serii, obejrzyj je wszystkie. [Przejdź do listy klipów wideo](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-ask-a-question-you-can-answer-with-data]

## <a name="other-videos-in-this-series"></a>Inne klipy wideo z tej serii

*Nauka danych dla początkujących* jest krótkie wprowadzenie do nauki danych w pięciu krótkie klipy wideo.

  * Klip wideo 1: [5 pytaniach odpowiedzi nauki danych](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 min 14 s)*
  * Klip wideo 2: [jest teraz gotowy do nauki danych danych?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 s 56 min)*
  * Klip wideo 3: Zadaj pytanie, na które można odpowiedzieć z danymi
  * Klip wideo 4: [Przewidywanie odpowiedzi przy użyciu prostego modelu](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 s 42 min)*
  * Klip wideo 5: [Kopiowanie pracy innych osób nauki danych](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 s)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Przebieg: Zadaj pytanie, na które można odpowiedzieć z danymi

"Nauki danych dla początkujących." trzecim wideo w serii — Zapraszamy!  

W tym obiekcie otrzymasz wskazówki opracowywania pytanie, na które można odpowiedzieć z danymi.

Może zostać wyświetlony więcej poza ten klip wideo, jeśli najpierw Obejrzyj dwóch wcześniejszych klipów wideo z tej serii: "nauki danych 5 pytaniach może odpowiadać" i "Jest danych jest teraz gotowy do nauki danych?"

## <a name="ask-a-sharp-question"></a>Zadaj pytanie ostry

Firma Microsoft została zawsze mówię o tym, jak nauki danych proces używania nazwy (nazywanej również kategorie i etykiety) i numery do przewidywania odpowiedzi na pytanie. Ale nie można go tylko wszelkie kwestie; musi ona być *ostrych pytanie.*

Niejasne pytanie nie ma odpowiedzi przy użyciu nazwiska lub numeru. Najpierw ostrych pytanie.

Załóżmy, że znaleziona Magiczna światło z genie, która będzie wyczerpujących odbierać wszelkie pytanie, na które można zadawać. Ale jest mischievous genie, a zostanie on spróbuj nawiązać jego odpowiedzi jako niejasne i mylące, jako osoba może uzyskać dostęp z dala od komputera z. Chcesz przypiąć go w dół pytanie, dlatego szczelne, nie może pomóc ale informujący o tym, co chcesz wiedzieć.

W przypadku Zadaj pytanie niejasne, takie jak "Co się dzieje się zdarzyć z moim giełdowy?", może odebrać genie, "zmieni cenę". To truthful odpowiedzi, ale nie jest szczególnie przydatne.

Ale miały Zadaj pytanie ostre, jak "Co cenę sprzedaży Moje giełdowy będzie następny tydzień?", genie nie pomocy ale zapewniają konkretnej odpowiedzi i przewidywanie cenę sprzedaży.

## <a name="examples-of-your-answer-target-data"></a>Przykłady odpowiedzi na Twoje pytanie: dane docelowej

Po sformułowania pytanie, sprawdź, czy masz przykłady odpowiedź w danych.

W przypadku naszych pytanie "Co cenę sprzedaży Moje giełdowy będzie następny tydzień?" następnie mamy upewnij się, że Historia giełdowy zawiera dane.

W przypadku naszych pytanie "które samochodów w mojej floty ma się nie powieść, najpierw?" następnie mamy upewnij się, że dane znajdują się informacje dotyczące poprzednich błędy.

![Data docelowej — przykłady odpowiedzi na Twoje pytanie. Sformułowania pytanie nauki danych.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-target-data.png)

W tych przykładach odpowiedzi są określane jako elementu docelowego. Miejsce docelowe jest, co możemy próbujesz przewidywanie o punktów danych w przyszłości, czy jest kategorii lub liczby.

Jeśli nie masz jakiekolwiek dane docelowej, musisz uzyskać niektóre. Nie będzie możliwe odpowiedzi na pytania bez niego.

## <a name="reformulate-your-question"></a>Sformułować pytanie

Czasami może adnotacji swoje pytanie, aby uzyskać bardziej użyteczna odpowiedzi.

Pytanie "Jest to A punkt danych lub B?" przewiduje kategorii (lub nazwy lub etykiety) elementu. Aby odebrać go, firma Microsoft korzysta z *algorytmu klasyfikacji*.

Pytanie "Ilość?" lub "Ile?" przewiduje kwotę. Aby odebrać go firma Microsoft korzysta z *algorytmu regresji*.

Aby zobaczyć, jak firma Microsoft tych Przekształcanie, Przyjrzyjmy się pytanie "które wątku wiadomości jest najbardziej interesujące tego czytnika?" Sprawdza, czy do przewidywania pojedynczego wyboru z wielu możliwości - innymi słowy "Jest to A i B lub C lub D?" - i chcesz używać algorytmu klasyfikacji.

Ale to pytanie można łatwiej odpowiedzi, jeśli go jako adnotacji "jak interesujące jest każdej sekcji na tej liście tego czytnika?" Teraz możesz nadać każdy artykuł wyników liczbowych i ułatwia ustalenie artykuł najwyższe wyników. To jest ponownie sformułować pytanie klasyfikacji na pytanie regresji lub ile?

![Sformułować pytanie. Klasyfikacja pytanie a pytanie regresji.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-classification-question-vs-regression-question.png)

W jaki sposób można zadawać pytania jest clue, do którego algorytmu można uzyskać odpowiedzi.

Można znaleźć niektórych rodzin algorytmów - podobnych w naszym przykładzie wątku wiadomości — są ściśle powiązane. Można sformułować swoje pytanie, aby używać algorytmu zapewniającej najbardziej użyteczne odpowiedzi.

Ale najważniejszych poproś to pytanie ostrych - pytanie, na które można odpowiedzieć z danymi. I upewnij się, że masz właściwych danych, aby odebrać go.

Zostały zajmowaliśmy niektóre podstawowe zasady Zadaj pytanie czy może odpowiadać z danymi.

Pamiętaj wyewidencjonować inne klipy wideo "Danych nauki dla początkujących" z witryny Microsoft Azure maszynowego Learning.


## <a name="next-steps"></a>Następne kroki

  * [Spróbuj pierwszym doświadczeniu nauki danych z komputera nauki Studio](machine-learning-create-experiment.md)
  * [Wprowadzenie do nauki maszynowego w programie Microsoft Azure](machine-learning-what-is-machine-learning.md)
