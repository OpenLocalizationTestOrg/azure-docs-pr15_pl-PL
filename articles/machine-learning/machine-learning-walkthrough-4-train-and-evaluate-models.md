<properties
    pageTitle="Krok 4: Szkolenie i ocenianie przewidywanych modeli analityczny | Microsoft Azure"
    description="Krok 4 opracowanie skorzystać z przewidywanych rozwiązanie: pociągu, wynik i oceny wielu modeli w Azure maszynowego uczenia Studio."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-4-train-and-evaluate-the-predictive-analytic-models"></a>Przewodnik krok 4: Szkolenie i ocenianie przewidywanych modeli analitycznego

Ten temat zawiera czwartym krokiem Instruktaż [opracowanie rozwiązania przewidywanych analizy w programie nauki maszynowego Azure](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Tworzenie obszaru roboczego nauki komputera](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Przekaż istniejące dane](machine-learning-walkthrough-2-upload-data.md)
3.  [Tworzenie nowego doświadczenia](machine-learning-walkthrough-3-create-new-experiment.md)
4.  **Szkolenie i ocenianie modeli**
5.  [Wdrażanie usługi sieci web](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Dostęp do usługi sieci web](machine-learning-walkthrough-6-access-web-service.md)

----------

Jedną z zalet stosowania Azure maszynowego uczenia Studio tworzenia modeli nauki maszynowego jest możliwość Wypróbuj więcej niż jeden typ modelu naraz w doświadczeniu i porównywanie wyników. Tego typu eksperyment ułatwia znajdowanie optymalny sposób rozwiązania problemu.

W doświadczeniu, które możemy opracowywania w tym instruktażu możemy utworzyć dwa różne typy modeli i porównać ich wyników wyniki zdecydować, który algorytm chcemy za pomocą w naszym doświadczeniu ostateczne.  

Istnieją różne modele, które firma Microsoft może wybierać. Aby wyświetlić dostępne modele, rozwiń węzeł **Nauki komputera** na palecie moduł, a następnie rozwiń **Zainicjować modelu** i węzły pod nim. Na potrzeby tego doświadczenia możemy wybierz maszynowego wektorowa pomocy technicznej (SVM) i moduły klasy dwóch zwiększane algorytmów.    

> [AZURE.TIP] Aby uzyskać pomoc dotyczącą Określanie, który algorytm maszynowego uczenia najlepiej pasuje konkretnego problemu, który próbujesz rozwiązać, zobacz [jak wybrać algorytmów Microsoft Azure maszynowego uczenia](machine-learning-algorithm-choice.md).

##<a name="train-the-models"></a>Przeszkolenie modeli
Najpierw skonfiguruj model drzewa decyzji zwiększane:  

1.  Znajdowanie [Drzewa decyzji zwiększane dwie klasy] [ two-class-boosted-decision-tree] moduł w module palety i przeciągnij go do obszaru roboczego.
2.  Znajdowanie [Modelu pociągu] [ train-model] moduł, przeciągnij go do obszaru roboczego, a następnie połącz dane wyjściowe modułu drzewa decyzji zwiększane po lewej stronie wprowadzania portu ("nieprzeszkolonych model") [Modelu pociągu] [ train-model] modułu.
    
    [Dwie klasy zwiększane drzewa decyzji] [ two-class-boosted-decision-tree] modułu inicjuje model ogólny i [Przeszkolenie modelu] [ train-model] przeszkolenie modelu na podstawie danych szkolenie. 
     
3.  Łączenie po lewej stronie danych wyjściowych ("zestawu danych wynik") po lewej stronie [Wykonywanie skryptów R] [ execute-r-script] modułu w prawo wprowadzania port ("zestawu danych") [Modelu pociągu] [ train-model] modułu.

    > [AZURE.TIP] Firma Microsoft nie są potrzebne dwie nakładów i jedną wyjściowe [Skrypt wykonywanie R] [ execute-r-script] moduł dla tego doświadczenia, więc możemy pozostawianie niedołączonej. 

4.  Wybierz odpowiedni [Model pociągu] [ train-model] modułu. W okienku **Właściwości** kliknij przycisk **Uruchom selektor kolumny**, zaznacz **Wszystkie typy** na liście rozwijanej w dół w obszarze **Dostępne kolumny** i wprowadź "Kredytowa ryzyka" w polu tekstowym. Zaznacz **Wszystkie typy** na liście rozwijanej w obszarze **Wybrane kolumny**. Wybierz "Kredytowa ryzyka" i kliknij przycisk strzałki wyróżniony, aby go przenieść **Zaznaczone**kolumny. 
5.  Kliknij przycisk **Zapisz**.


Teraz tej części doświadczenia wygląda mniej więcej tak:  

![Szkolenia modelu][1]

Następnie skonfiguruj możemy modelu SVM.  

Na początek nieco wyjaśnienie SVM. Zwiększane algorytmów podczas pracy z funkcji dowolnego typu. Jednak ponieważ moduł SVM generuje liniowej klasyfikatora, modelu, który generuje zawiera najlepsze błąd test, gdy wszystkie funkcje liczbowe mają tę samą skalę. Aby przekonwertować wszystkie funkcje liczbowe tę samą skalę, firma Microsoft korzysta z przekształcenia "Tanh" ( [Normalizowanie danych] [ normalize-data] modułu.) To przekształca naszych liczb w zakresie [0,1]. Parametry funkcji zostaną przekonwertowane przez moduł SVM do kategorii funkcji, a następnie do dwójkową funkcji 0-1, więc nie trzeba ręcznie Przekształcanie funkcje ciągu. Ponadto firma Microsoft nie chcesz, aby przekształcić ryzyko kredytowe kolumny (21) — jest liczbą, ale jest to wartość Firma Microsoft już szkolenia modelu do przewidywania, więc należy pozostawić go samodzielnie.  

Aby skonfigurować SVM model, wykonaj następujące czynności:

1.  Znajdowanie [Komputera wektorowa obsługa klasy dwóch] [ two-class-support-vector-machine] moduł w module palety i przeciągnij go do obszaru roboczego.
2.  Kliknij prawym przyciskiem myszy [Modelu pociągu] [ train-model] moduł, wybierz polecenie **Kopiuj**, a następnie kliknij prawym przyciskiem myszy obszar roboczy i wybierz pozycję **Wklej**. Kopię [Modelu pociągu] [ train-model] moduł ma tego samego wyboru w kolumnie jak oryginał.
3.  Wynik moduł SVM nawiązać połączenie po lewej stronie port wejściowy ("nieprzeszkolonych model") w drugim [Modelu pociągu] [ train-model] modułu.
4.  Znajdowanie [Normalizowanie danych] [ normalize-data] modułu i przeciągnij go do obszaru roboczego.
5.  Łączenie danych wejściowych tego modułu do lewej wyników po lewej stronie [Wykonywanie skryptów R] [ execute-r-script] modułu (należy zauważyć, że port wyjściowy modułu może być połączony z więcej niż jeden inny moduł).
6.  Połącz port wynik po lewej stronie ("przekształcania zestawu danych") [Normalizowanie danych] [ normalize-data] modułu w prawo wprowadzania port ("zestawu danych") w drugim [Modelu pociągu] [ train-model] modułu.
7.  W okienku **Właściwości** [Normalizowanie danych] [ normalize-data] moduł, wybierz pozycję **Tanh** parametru **metody transformacja** .
8.  Kliknij przycisk **Uruchom selektor kolumny**, wybierz "Bez kolumn" dla **Zaczyna się od**, zaznacz pole wyboru **Dołącz** na liście rozwijanej pierwszy, wybierz **Typ kolumny** na liście rozwijanej drugi i wybierz **liczbowe** na liście rozwijanej trzecim. To ustawienie określa przekształcania wszystkie kolumny liczbowe (i tylko liczbowe).
9.  Kliknij znak plus (+) po prawej stronie tego wiersza — spowoduje to utworzenie w wierszu list rozwijanych. W pierwszej listy rozwijanej wybierz pozycję **wykluczyć** , wybieranie **nazw kolumn** na liście rozwijanej drugi i kliknij przycisk pole tekstowe i wybierz pozycję "Kredytowa ryzyka" na liście kolumn. To ustawienie określa kolumnę ryzyko kredytowe powinny być ignorowane (należy wykonaj następujące czynności, ponieważ w tej kolumnie jest liczbą i, w przeciwnym razie można przekształcić).
10. Kliknij **przycisk OK**.  


[Normalizowanie danych] [ normalize-data] modułu jest teraz skonfigurowana do przekształcenie Tanh na wszystkich kolumn liczbowych z wyjątkiem ryzyko kredytowe.  

Tej części naszych doświadczenia powinna wyglądać podobnie następujące czynności:  

![Szkolenia w drugim modelu][2]  

##<a name="score-and-evaluate-the-models"></a>Wynik i oceny modeli

Firma Microsoft korzysta z testowania dane, które zostało oddzielone [Podzielone dane] [ split] moduł wynik naszych przeszkolony modeli. Firma Microsoft następnie porównać wyniki dwóch modeli, aby zobaczyć, który wygenerował lepszych wyników.  

1.  Znajdowanie [Modelu wynik] [ score-model] modułu i przeciągnij go do obszaru roboczego.
2.  Nawiązywanie połączenia z [Modelu pociągu] [ train-model] modułu, który jest połączony z [Dwoma klasy zwiększane drzewa decyzji] [ two-class-boosted-decision-tree] moduł po lewej stronie wprowadzania port [Modelu wynik] [ score-model] modułu.
3.  Łączenie prawo port wejściowy [Modelu wynik] [ score-model] modułu po lewej stronie danych wyjściowych prawo [Wykonywanie skryptów R] [ execute-r-script] modułu.

    [Wynik modelu] [ score-model] modułu można teraz pobierania informacji kredytową z testowania danych, uruchom go za pomocą modelu i porównywania przewidywań modelu generuje z kolumną ryzyka rzeczywisty kredytowej w testowania danych.

4.  Kopiowanie i wklejanie [Modelu wynik] [ score-model] moduł, aby utworzyć drugą kopię lub przeciągnij nowy moduł obszaru roboczego.
5.  Połącz po lewej stronie port wprowadzania tego modułu modelu SVM (oznacza to, że nawiązywanie połączenia z portu wyjściowego [Modelu pociągu] [ train-model] modułu, który jest podłączony do [Komputera wektorowa obsługa klasy dwóch] [ two-class-support-vector-machine] modułu).
6.  Z modelu SVM mamy wykonaj przekształcenia danych test, jak firma Microsoft dane szkolenia. Aby kopiować i wklejać [Normalizowanie danych] [ normalize-data] moduł, aby utworzyć drugą kopię i połączyć go z lewej wynik prawo [Wykonywanie skryptów R] [ execute-r-script] modułu.
7.  Łączenie prawo port wejściowy [Modelu wynik] [ score-model] modułu po lewej stronie danych wyjściowych [Normalizowanie danych] [ normalize-data] modułu.  

Aby ocenić dwóch wyników wyników, firma Microsoft korzysta z [Modelu są interpretowane] [ evaluate-model] modułu.  

1.  Znajdowanie [Oceny modelu] [ evaluate-model] modułu i przeciągnij go do obszaru roboczego.
2.  Połącz po lewej stronie port wprowadzania danych do portu wyjściowego [Modelu wynik] [ score-model] moduł skojarzony z modelem drzewa decyzji zwiększane.
3.  Połącz prawym port wprowadzania danych do innych [Modelu wynik] [ score-model] modułu.  

Aby uruchomić doświadczenia, kliknij przycisk **Uruchom** , poniżej obszaru roboczego. Może potrwać kilka minut. Wskaźnik Obracająca w każdym module wskazuje, że jest uruchomiony, a następnie zielony znacznik wyboru po zakończeniu modułu. Jeśli wszystkie moduły znajduje się znacznik wyboru, doświadczenia zakończeniu.

Doświadczenia powinna wyglądać mniej więcej tak:  

![Oceny obu modeli][3]

Aby sprawdzić wyniki, kliknij port wyjściowy [Oceny modelu] [ evaluate-model] moduł i wybierz pozycję **Wizualizacja**.  

[Szacowanie modelu] [ evaluate-model] modułu tworzy parę krzywych i miar, które umożliwiają porównywanie wyników dwóch modeli scored. Można wyświetlać wyniki jako krzywe cechy Operator odbiorcy (ROC), dokładność i odwoływanie krzywe lub Krzywe dźwigu. Dodatkowe dane wyświetlane zawiera macierz zamieszania wartości skumulowanego obszarze krzywej (AUC) i innymi wskaźnikami. Można zmienić wartość progowa, przesuwając suwak w lewo lub w prawo i zobacz, jak wpłynie na zbiór metryki.  

Po prawej stronie wykresu kliknij przycisk **Scored dataset** lub **Scored zestawu danych, aby porównać** , aby wyróżnić krzywej skojarzone i wyświetlić skojarzone metryki poniżej. W legendzie krzywych "Uzyskał zestawu danych" odpowiada po lewej stronie port wejściowy [Oceny modelu] [ evaluate-model] moduł — w naszym przypadku jest to model drzewa decyzji zwiększane. "Uzyskał zestawu danych w celu porównania" odnosi się do prawej port wprowadzania — modelu SVM w naszym przypadku. Po kliknięciu jednej z tych etykiet krzywej dla tego modelu wyróżniona i wyświetlanie odpowiednich metryki, jak pokazano na poniższym rysunku.  

![Krzywe ROC dla modeli][4]

Sprawdzając te wartości, możesz określić, model, który ma się najbliżej daje rezultatów, którego szukasz. Można wrócić i przejść na swojej doświadczenia przez zmianę wartości w różnych modelach. 

> [AZURE.TIP] Każdym uruchomieniu doświadczenia rekordu tego iteracji jest przechowywana w historii Uruchom. Można wyświetlić te iteracji i powrócić do każdej z nich, klikając pozycję **WYŚWIETL HISTORIĘ uruchamianie** poniżej obszaru roboczego. Można również kliknij polecenie **Uruchom poprzednich** w okienku **Właściwości** , aby powrócić do iteracji bezpośrednio poprzedzającego jeden jest otwarte.
> 
Można utworzyć kopię dowolnego iteracji swojego doświadczenia, klikając pozycję **ZAPISZ jako** poniżej kanwy. Umożliwia rejestrowanie jakie zostały wypróbowane w swojej iteracji doświadczenia doświadczenia **Podsumowanie** i **Opis** właściwości.

>  Aby uzyskać więcej informacji zobacz [Zarządzanie poeksperymentować iteracji w Azure maszynowego uczenia Studio](machine-learning-manage-experiment-iterations.md).  


----------

**Następny: [Wdrażanie usługi sieci web](machine-learning-walkthrough-5-publish-web-service.md)**

[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train1.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train2.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train3.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train4.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
