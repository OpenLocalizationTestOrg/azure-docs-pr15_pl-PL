<properties
    pageTitle="Tworzenie tekstu modeli analizy w Azure maszynowego uczenia Studio | Microsoft Azure"
    description="Jak utworzyć tekst modeli analizy w Studio nauki maszynowego Azure za pomocą modułów wstępne przetwarzanie tekstu, g N lub funkcji mieszania"
    services="machine-learning"
    documentationCenter=""
    authors="rastala"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="roastala" />


#<a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Tworzenie modeli analizy tekstu w Azure maszynowego uczenia Studio

Azure maszynowego uczenia służy do tworzenia i operationalize modeli analizy tekstu. Te modele mogą ułatwić rozwiązywanie, na przykład dokument klasyfikacji lub upodobania analizy problemów.

W eksperyment analizy tekstu jak zwykle:

 1. Oczyszczanie i przetwarzanie wstępne zestawu danych tekst
 2. Wyodrębnianie wektorów liczbowe funkcji z wstępnie przetworzone tekstu
 3. Model klasyfikacji lub regresji pociągu
 4. Wynik i sprawdzanie poprawności modelu
 5. Wdrożeniu modelu w produkcji

W tym samouczku się te kroki jako przeprowadzimy przez modelu analizy upodobania przy użyciu zestawu danych przeglądy książki Amazon (zobacz ten dokument badań "Biographies, Bollywood, wysięgnik pól i mieszalni: dostosowanie domeny upodobania klasyfikacji" Blitzer Jan, oznacz Dredze i Fernando Pereira; Skojarzenie lingwistyczne obliczeniowe (ACL) 2007.) Tego zestawu danych składa się z wyników przeglądu (1-2 lub 4-5) i dowolnego tekstu. Celem jest przewidywanie wyniku Recenzja: Niski (1 - 2) lub od dużych (4-5).

Możesz znaleźć doświadczeń omówione w tym samouczku w galerii analizy Cortana:

[Przewidywanie przeglądy książki] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Przewidywanie przeglądy książki - przewidywanych doświadczenia] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Krok 1: Czyszczenia i przetwarzanie wstępne zestawu danych tekst

Firma Microsoft rozpocząć doświadczenia dzieląc wyniki przeglądu na kategorii przedziałów dolnego i górnego do sformułowania ten problem, co klasyfikacji dwóch zajęć. Firma Microsoft korzysta z [edytować metadane] (https://msdn.microsoft.com/library/azure/dn905986.aspx) i moduły [Grupa kategorii Values] (https://msdn.microsoft.com/library/azure/dn906014.aspx).

![Tworzenie etykiet](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Następnie usuwania tekstu za pomocą modułu [przetwarzanie wstępne tekst] (https://msdn.microsoft.com/library/azure/mt762915.aspx). Czyszczenie zmniejsza hałasu w zestawie danych ułatwiają znajdowanie najważniejszych funkcjach i zwiększyć dokładność funkcji ostatecznego modelu. Firma Microsoft usunąć stopword - typowe słowa, takie jak "" lub "-" i liczby, znaki specjalne, zduplikowane znaków, adresy e-mail i adresy URL. Możemy również konwertowanie tekstu na małe litery, lemmatize wyrazów i wykrywa granic zdanie, które następnie są oznaczone "|||" symbol w tekście wstępnie przetworzone.

![Przetwarzanie wstępne tekstu](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Co zrobić, jeśli chcesz używać niestandardowej listy stopword? Można przekazać go jako dane wejściowe opcjonalne. Można także zastąpić podciągów za pomocą niestandardowych C# składni wyrażeń regularnych i usunąć wyrazów, część mowy: rzeczowniki, zlecenia lub przymiotników.

Po zakończeniu wstępne przetwarzanie możemy danych można podzielić pociągu i przetestuj zestawów.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Krok 2: Wyodrębnianie wektorów liczbowe funkcji z wstępnie przetworzone tekst

Do tworzenia modelu dla danych tekstowych, zwykle musisz Konwertowanie tekstu dowolnych wektorów liczbowe funkcji. W tym przykładzie używamy [wyodrębnić możliwości g N z tekstu] modułu (https://msdn.microsoft.com/library/azure/mt762916.aspx) do przekształcania danych tekstowych, na przykład format. Moduł ten przyjmuje kolumnę oddzielone odstępy wyrazów i oblicza słownika zawierającego wyrazy lub N g wyrazy są wyświetlane w swojego zestawu danych. Następnie zlicza zlicza, ile razy każdego w programie word lub N g pojawia się we wszystkich rekordach, a tworzy wektorów funkcji spośród. W tym samouczku firma Microsoft Ustaw rozmiar N g 2, więc naszych wektorów funkcji zawierać pojedynczych wyrazów i kombinacje dwóch kolejnych wyrazów.

![Wyodrębnianie g N](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

Stosujemy TF * zlicza IDF (terminów częstotliwości arcus dokumentu częstotliwość) waga g N. Tej metody dodaje grubości wyrazy, które często są wyświetlane w jeden rekord, ale są rzadko przez cały zestaw danych. Inne opcje obejmują binarnego, TF, a także tworzyć wykresy ważenie.

Takie funkcje tekstu często mają wymiarze wysoki. Na przykład jeśli usługi Boże ma 100 000 unikatowych słów, obszaru funkcji woli wymiary 100 000 lub więcej użycie g N. Moduł wyodrębnić funkcje g N zapewnia zestaw opcji w celu zmniejszenia wymiarze. Istnieje możliwość wyłączenia wyrazy, które są krótki lub długi lub zbyt nietypowych lub zbyt częste ma znaczną wartość. W tym samouczku możemy wykluczenie g N, występujące w rekordach mniej niż 5 lub więcej niż 80% rekordów.

Ponadto umożliwia wybór funkcji zaznacz tylko te funkcje, które są najbardziej skorelowany z docelowym przewidywania. Aby zaznaczyć 1000 funkcje firma Microsoft korzysta z wybór funkcji chi-kwadrat. Słownictwo zaznaczonego słów lub g N można wyświetlić, klikając prawym wynik Wyodrębnij N-g modułu.

Jako alternatywny sposobem za pomocą funkcji wyodrębnić g N można użyć funkcji mieszania modułu. Uwaga Jeśli tego [funkcji mieszania] (https://msdn.microsoft.com/library/azure/dn906018.aspx) nie ma możliwości zaznaczania wbudowanego funkcji lub TF * ważenie IDF.

## <a name="step-3-train-classification-or-regression-model"></a>Krok 3: Szkolenie model klasyfikacji lub regresji

Teraz została przekształcona tekst jako kolumny liczbowe funkcji. Zestaw danych nadal zawiera ciąg kolumny z poprzednich etapów, więc stosujemy Wybieranie kolumn w zestawie danych, aby je wyłączyć.

Następnie używamy [klasy dwóch regresją] (https://msdn.microsoft.com/library/azure/dn905994.aspx) do przewidywania celem: wynik Recenzja wysoki lub niski. W tym momencie problem zwykła klasyfikacji została przekształcona problem analizy tekstu. Za pomocą narzędzi dostępnych w Azure maszynowego uczenia do ulepszania modelu. Na przykład możesz poeksperymentować z różnych klasyfikatorów, aby dowiedzieć się, jak dokładne wyniki dają lub użyć hyperparameter dostosowywania, aby zwiększyć dokładność.

![Pociągu i wynik](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>Krok 4: Wynik i sprawdzanie poprawności modelu

Jak sprawdzi poprawność przeszkolony modelu? Firma Microsoft wynik go przed test zestawu danych i oceń dokładności. Jednak modelu materiału słownictwo N g i ich wagi z szkolenie zestawu danych. W związku z tym możemy należy użyć tego słownika i tych grubości przy wyodrębnianie funkcje z testowymi danymi, a nie na przykład słownictwa od nowa. W związku z tym możemy Dodawanie wyodrębnić g N funkcje modułu do wyników gałęzi doświadczenia, łączenie słownictwo dane wyjściowe z gałęzią szkolenia i ustaw słownictwo tryb tylko do odczytu. Firma Microsoft także wyłączyć filtrowanie g N przez częstotliwość przez ustawienie wartość minimalna 1 wystąpienie i maksymalnie do 100% i wyłączanie wybór funkcji.

Po kolumny tekstu w testowymi danymi została przekształcona do kolumn funkcji liczbowe, możemy wykluczyć kolumny ciąg z poprzednich etapów podobnie jak w gałęzi szkolenie. Następnie wykonaj się moduł modelu wynik nawiązać przewidywań i moduł oceny modelu ma być obliczona dokładności.

## <a name="step-5-deploy-the-model-to-production"></a>Krok 5: Wdrożeniu modelu w produkcji

Model jest już prawie gotowe do wdrożenia do produkcji. Po wdrożeniu jako usługa sieci web go zajmie ciąg tekstowy dowolnych jako danych wejściowych, a zwracana przewidywania "Wysoki" lub "Niski". Użyto zapamiętanych słownictwo g N do Przekształcanie tekstu do funkcji i model przeszkolony regresją nawiązać przewidywania z tych funkcji. 

Aby skonfigurować przewidywanych doświadczenia, możemy najpierw zapisać słownictwo g N jako zestaw danych i modelu przeszkolony regresją od gałęzi szkolenie doświadczenia. Następnie zapisz doświadczenia za pomocą "Zapisz jako", aby utworzyć wykres doświadczenia dla przewidywanych doświadczenia. Firma Microsoft usunąć moduł podzielone dane i gałąź szkolenie z doświadczenia. Firma Microsoft następnie połącz zapisane wcześniej słownictwo N g i model do wyodrębniania funkcji N g i moduły modelu wynik odpowiednio. Możemy również usunąć moduł oceny modelu.

Firma Microsoft Wstaw wybierz kolumny w module zestawu danych przed Usuń kolumnę etykieta moduł przetwarzanie wstępne tekst i usuń zaznaczenie opcji "Dołącz wynik kolumnie do zestawu danych" w Module wynik. Dzięki temu usługi sieci web nie żąda etykietę, która próbuje przewidywanie i nie nie echo wprowadzenia funkcji w odpowiedzi.

![Przewidywanych doświadczenia](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Teraz mamy doświadczenia, który może być opublikowany jako usługi sieci web i o nazwie przy użyciu żądanie odpowiedź lub partii wykonanie interfejsów API.

## <a name="next-steps"></a>Następne kroki

Więcej informacji na temat modułów analizy tekstu [dokumentacji MSDN] (https://msdn.microsoft.com/library/azure/dn905886.aspx).
