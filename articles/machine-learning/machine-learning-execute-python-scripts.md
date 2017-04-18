<properties 
    pageTitle="Wykonywanie skryptów nauki komputera Python | Microsoft Azure" 
    description="Konspekty Projektowanie zasad źródłowego Obsługa skryptów Python w Azure maszynowego uczenia i zastosowania podstawowe scenariusze, funkcje i ograniczenia." 
    keywords="maszynowego Python uczenia, pandas, python pandas, skryptów python wykonywanie skryptów python"
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
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Wykonywanie skryptów nauki maszynowego Python w Azure maszynowego uczenia Studio

W tym temacie opisano zasady projektowania źródłowego bieżącego Obsługa skryptów Python w Azure maszynowego uczenia. Również głównym możliwości zostały opisane w tym obsługa importowania istniejącego kodu, eksportowanie wizualizacji, a na koniec omówiono niektóre ograniczenia i pracy w toku.

[Python](https://www.python.org/) to narzędzie niezbędne w piersi narzędzie wiele naukowców danych. Został:

-  Eleganckie i zwięzły składni, 
-  Obsługa wielu platform 
-  zbiór ogromną zaawansowanych bibliotek i 
-  narzędzia programistyczne dojrzałe. 

Python jest używany we wszystkich fazach zwykle używanych w maszynowego uczenia modelowania przepływu pracy, z danych mogły zjeść tej ostatniej i przetwarzania budowy funkcji Szkolenie modelu i sprawdzanie poprawności i wdrażania modeli. 

Azure maszynowego uczenia Studio obsługuje osadzanie skrypty Python do różnych części komputera nauki doświadczenia, a także bezproblemowo opublikowanie ich jako skalowalna, operationalized sieci web usługi Microsoft Azure.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Zasady projektowania Python skryptów w nauki komputera
Podstawowy interfejs, który Python w Azure maszynowego uczenia Studio jest przez [Wykonywanie skryptów Python] [ execute-python-script] modułu pokazano na rysunku 1.

![obraz1](./media/machine-learning-execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![obraz2](./media/machine-learning-execute-python-scripts/embedded-machine-learning-python-script.png)

Rysunek 1. Moduł **Wykonywanie skryptów Python** .

[Wykonywanie skryptów Python] [ execute-python-script] modułu akceptuje maksymalnie trzy wartości wejściowych i tworzy do dwóch wyjściowe (omówiony poniżej), podobnie jak jej analogowe R, [Wykonywanie skryptów R] [ execute-r-script] modułu. Wykonanie kodu Python zostanie wprowadzony w polu parametru jako specjalnie nazwanego punktu wejścia funkcji o nazwie `azureml_main`. Poniżej przedstawiono zasady projektowania kluczowe używane do implementacji moduł, w tym:

1.  *Musi być idiomatycznych Python użytkowników.* Większość użytkowników Python współczynnika kodu jako funkcje wewnątrz modułów, dzięki czemu wprowadzanie wiele wykonywalny instrukcji w module najwyższego poziomu jest stosunkowo rzadkie. W wyniku polu skrypt ma również specjalnej funkcji Python zamiast tylko sekwencji instrukcji. Obiekty w funkcji są standardowe typy bibliotek Python takich jak ramek danych [Pandas](http://pandas.pydata.org/) i tablice [NumPy](http://www.numpy.org/) .
2.  *Musi być zachowaniem wysokiej wierności między lokalnej i w chmurze wykonania.* Umożliwia wykonywanie kodu Python wewnętrznej bazy danych jest oparty na [Anaconda](https://store.continuum.io/cshop/anaconda/) 2.1 rozkładu powszechna Python naukowych i platform. Pochodzi z zbliżony 200 najczęściej pakietów Python. W związku z tym naukowców danych można debugowanie i kwalifikować ich kodu na ich lokalnego środowiska Anaconda zgodnego z programem nauki Azure komputera. Następnie użyj istniejącego środowiska projektowania, takich jak Notes [IPython](http://ipython.org/) lub [Python Tools for Visual Studio](http://aka.ms/ptvs) , aby uruchomić go jako części doświadczenia Azure maszynowego uczenia sprawne wysoki. Następnie `azureml_main` punkt wejścia jest funkcją wanilii Python i ustanawiać bez używania kodu określonych Azure maszynowego uczenia lub zestawu SDK zainstalowany.
3.  *Musi być bezproblemowo wszechstronnej z innych modułów Azure maszynowego uczenia.* [Wykonywanie skryptów Python] [ execute-python-script] modułu przyjmuje jako dane wejściowe i wyjściowe, standardowy Azure maszynowego uczenia zestawy danych. Podstawowej struktury przezroczysty i efektywne mostkuje obsługi Azure maszynowego uczenia się i Python, (pomocniczych funkcji, takich jak brakujące wartości). W związku z tym Python można używać w połączeniu z istniejących Azure maszynowego uczenia przepływy pracy, łącznie z tymi, które nawiązywanie połączeń ze spotkaniami R i SQLite. Można przewidzieć jedną przepływy pracy który:
  * Używanie Python i Pandas dla danych czyszczenia i wstępne przetwarzanie 
  * źródła danych do przekształcenia SQL, dołączanie do wielu zestawów danych do formularza funkcji, 
  * przeszkolenie modele przy użyciu obszernego zbioru algorytmów w Azure maszynowego uczenia, i 
  * ocenianie i po przetworzeniu wyników za pomocą R.


## <a name="basic-usage-scenarios-in-machine-learning-for-python-scripts"></a>Scenariusze zastosowania podstawowe w maszynowego uczenia Python skryptów
W tej sekcji pracujemy ankiety niektóre podstawowe zastosowania [Wykonywanie skryptów Python] [ execute-python-script] modułu.
Jak wspomniano wcześniej, nakładów w Python module są dostępne jako Pandas ramek danych. Więcej informacji na temat Python Pandas i jak go można zaznaczać dane i skuteczniejszego można znaleźć w *Python do analizy danych* (O'Reilly 2012) przez Zachodnia McKinney. Funkcja musi zwracać jedną ramkę danych Pandas spakowany umieszczona Python [sekwencji](https://docs.python.org/2/c-api/sequence.html) , takich jak krotki, na liście lub w tablicy NumPy. Pierwszy element sekwencji jest zwracany w pierwszym porcie dane wyjściowe modułu. Ten program jest wyświetlany na rysunku 2.

![image3](./media/machine-learning-execute-python-scripts/map-of-python-script-inputs-outputs.png)

Rysunek 2. Mapowania portów do parametrów wejściowych i zwracana wartość do portu wyjściowego.

Bardziej szczegółowe znaczeń właściwych: jak portów wejściowych są mapowane parametry `azureml_main` funkcji są wyświetlane w tabeli 1:

![image1T](./media/machine-learning-execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tabela 1. Mapowanie portów wejściowych do parametry funkcji.

Mapowanie między portów wejściowych i parametry funkcji jest pozycyjne. Pierwszy połączonego port wejściowy jest mapowanych na pierwszy parametr funkcji, a druga wprowadzania (Jeśli połączenie) są mapowane na drugi parametr funkcji.

## <a name="translation-of-input-and-output-types"></a>Tłumaczenie typy wejścia i wyjścia
Zgodnie z opisem, zestawy wprowadzania danych w Azure maszynowego uczenia są konwertowane na dane, które ramek w Pandas i ramek dane wyjściowe są konwertowane na powrót do nauki maszynowego Azure zestawy danych. Poniższe konwersje są wykonywane:

1.  Kolumny ciągu i liczbowe są konwertowane na-jest i brakujące wartości w zestawie danych są konwertowane na wartości "NA" w Pandas. Konwersja samej odbywa się w drodze powrotem (Brak wartości Pandas są konwertowane na brakujące wartości w Azure maszynowego uczenia).
2.  Kierunki indeksu w Pandas nie są obsługiwane w Azure maszynowego uczenia. Wszystkie ramki danych wejściowych w funkcji Python zawsze ma 64-bitowej indeksu liczbowych od 0 do liczby wierszy minus 1. 
3.  Azure maszynowego uczenia zestawy danych nie mogą mieć zduplikowanych nazw kolumn i nazwy kolumn, które nie są ciągami. Jeśli ramka dane wyjściowe zawiera kolumny wartością liczbową, ramach wywołuje `str` według nazw kolumn. Analogicznie zduplikowanych nazw kolumn są automatycznie zniekształcone aby upewnić się, że imiona i nazwiska są unikatowe. Sufiks (2) jest dodawany do pierwszego duplikat, (3) do drugiego zduplikowanych, itp.

## <a name="operationalizing-python-scripts"></a>Operationalizing Python skryptów
Dowolny [Wykonywanie skryptów Python] [ execute-python-script] moduły używane w eksperyment wyników są nazywane po opublikowaniu usługi sieci web. Na przykład na rysunku 3 przedstawiono eksperyment wyników zawierających kod, który ma być obliczona pojedyncze wyrażenie Python. 

![image4](./media/machine-learning-execute-python-scripts/figure3a.png)

![image5](./media/machine-learning-execute-python-scripts/python-script-with-python-pandas.png)

Rysunek 3. Usługa sieci Web do oceniania wyrażenie Python.

Usługa sieci web utworzona na podstawie tego doświadczenia przyjmuje jako wprowadź wyrażenie Python (jako ciąg) przesyła go do interpretera Python i zwraca tabelę zawierającą wyrażenie i obliczony wynik.

## <a name="importing-existing-python-script-modules"></a>Importowanie istniejącej moduły skrypt Python
Typowe przypadków użycia wielu naukowców danych jest włączenie istniejących skryptów Python do doświadczeń Azure maszynowego uczenia. Zamiast łączenia i wkleić cały kod w polu pojedynczy skrypt, [Wykonywanie skryptów Python] [ execute-python-script] modułu akceptuje trzecia port wejściowy, do którego można łączyć pliku zip, która zawiera moduły Python. Plik zostanie następnie rozpakowane Framework wykonanie w czasie rzeczywistym i zawartość jest dodawana do ścieżki biblioteki interpretera Python. `azureml_main` Punktu wejścia funkcja następnie można zaimportować te moduły bezpośrednio.

Na przykład należy rozważyć, czy plik Hello.py zawierająca prosty funkcję "Witaj, świecie".

![image6](./media/machine-learning-execute-python-scripts/figure4.png)

Rysunek 4. Funkcja zdefiniowana przez użytkownika.

Następnie tworzymy pliku Hello.zip, który zawiera Hello.py:

![image7](./media/machine-learning-execute-python-scripts/figure5.png)

Rysunek 5. Plik zip zawierający kod Python zdefiniowane przez użytkownika.

Następnie przekazać to jako zestaw danych do Azure maszynowego uczenia Studio. Tworzenie i uruchamianie eksperyment proste, korzystającego z kodu Python w pliku Hello.zip dołączając je do trzeciej port wejściowy skryptu Python wykonywanie, jak pokazano na poniższym rysunku.

![image8](./media/machine-learning-execute-python-scripts/figure6a.png)

![image9](./media/machine-learning-execute-python-scripts/figure6b.png)

Rysunek 6. Przykładowe doświadczenie z kodem Python zdefiniowane przez użytkownika przekazane jako pliku zip.

Moduł wyjściowy pokazuje, że plik zip został nieopakowane oraz funkcji `print_hello` faktycznie zostało uruchomione.
 
![image10](./media/machine-learning-execute-python-scripts/figure7.png)
 
Rysunek 7. Funkcja zdefiniowana przez użytkownika używany wewnątrz [Wykonywanie skryptów Python] [ execute-python-script] modułu.

## <a name="working-with-visualizations"></a>Praca z wizualizacji
Wykresy utworzone przy użyciu MatplotLib, który można zwizualizować w przeglądarce mogą zostać zwrócone przez [Wykonywanie skryptów Python][execute-python-script]. Ale powierzchni nie są automatycznie przekierowywane do obrazów w są podczas korzystania z R. Dlatego użytkownik musi zapisze wszystkie wykresy na pliki PNG jeśli mają zostać zwrócone do nauki maszynowego Azure. 

Aby wygenerować obrazów z MatplotLib, możesz współzawodniczenia poniższą procedurę:

* przełączanie z domyślne renderowanie Qt wewnętrznej bazy danych do "AGG" 
* Utwórz nowy obiekt rysunek 
* Uzyskiwanie osi oraz generowanie wszystkie wykresy w nim 
* Zapisywanie rysunku do pliku PNG 

Ten proces przedstawiono na poniższej rysunek 8 tworzy wykres punktowy macierzy przy użyciu funkcji scatter_matrix w Pandas.
 
![image1v](./media/machine-learning-execute-python-scripts/figure-v1-8.png)

Rysunek 8. Zapisywanie rysunków MatplotLib do obrazów.



Rysunek 9 pokazuje doświadczenia, który używa skryptu pokazano wcześniej, aby zwrócić kreśli przez drugiego portu wyjściowego.

![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9a.png) 
     
![image2v](./media/machine-learning-execute-python-scripts/figure-v2-9b.png) 

Rysunek 9. Wizualizowanie powierzchni wynikiem Python kodu.

Istnieje możliwość zwracana wiele rysunków, zapisując je w różnych obrazach, obsługi Azure maszynowego uczenia przejmuje wszystkich obrazów i łączy ich wizualizacji.


## <a name="advanced-examples"></a>Przykłady zaawansowanych
Środowiska Anaconda zainstalowanych w Azure maszynowego uczenia zawiera typowe pakietów, takie jak NumPy, SciPy i Dowiedz się Scikits i tych może być skutecznie używany do różnych zadań przetwarzania danych w potoku nauki typowe komputera. Na przykład doświadczenia następujące i skrypt pokazano, jak działa zespół uczestników w Scikits informacje do obliczenia funkcji ważność wyników zestawu danych. Wyniki następnie może służyć do wykonywania wybór funkcji pod nadzorem przed podawania do innego komputera nauki modelu.

Funkcja Python do obliczenia ważność wyników i kolejność funkcje oparte na nim przedstawiono poniżej:

![image11](./media/machine-learning-execute-python-scripts/figure8.png)

Rysunek 10. Funkcja funkcji pozycja przez wyniki.
 Następujące doświadczenia następnie oblicza i zwraca wynik ważność funkcji w zestawie danych "Pima indyjskiego cukrzyca" w Azure maszynowego uczenia:

![image12](./media/machine-learning-execute-python-scripts/figure9a.png)
![image13](./media/machine-learning-execute-python-scripts/figure9b.png)    
    
Rysunek 11. Wypróbuj funkcji pozycja w zestawie danych Pima indyjskiego cukrzyca.

## <a name="limitations"></a>Ograniczenia 
[Wykonywanie skryptów Python] [ execute-python-script] obecnie ma następujące ograniczenia:

1.  *Wykonywanie w trybie piaskownicy.* Środowisko uruchomieniowe Python jest obecnie w trybie piaskownicy, a w wyniku nie zezwala na dostęp do sieci lub do lokalnego systemu plików w sposób ciągły. Wszystkie pliki są zapisane lokalnie są samodzielnie i usuwane po zakończeniu modułu. Kod Python nie ma dostępu większość katalogów na komputerze, na którym działa, wyjątku bieżącego katalogu i jego podkatalogów.
2.  *Brak zaawansowanych rozwoju i obsługa debugowania.* Moduł Python nie obsługuje obecnie IDE funkcje, takie jak intellisense i debugowania. Ponadto jeśli moduł nie powiedzie się w czasie rzeczywistym, pełny śledzenia stosu Python jest dostępna, ale muszą być wyświetlane w dzienniku dane wyjściowe modułu. Zalecamy obecnie opracowywać i debugowanie ich Python skryptów w środowisku, takich jak IPython i zaimportowanie kod do modułu.
3.  *Wynik ramki pojedynczego danych.* Punkt wejścia Python jest dozwolony tylko zwraca ramki pojedynczego danych jako dane wyjściowe. Nie jest obecnie możliwe wrócić do obsługi Azure maszynowego uczenia dowolnego Python obiekty, takie jak przeszkolony modele bezpośrednio. [Wykonywanie skryptów R], takich jak[execute-r-script], która ma samym, jest jednak możliwe w wielu przypadkach do obiektów pickle do tablicy, a następnie powróć tego wewnątrz ramki danych.
4.  *Niemożność dostosowanie Python instalacji*. Jedynym sposobem dodania niestandardowe moduły Python jest obecnie, za pośrednictwem mechanizmu pliku zip opisany. Gdy jest to możliwe dla małych modułów, jest wygodna dla dużych modułów, (zwłaszcza z natywnych dll) lub dużej liczby modułów. 


##<a name="conclusions"></a>Wnioski
[Wykonywanie skryptów Python] [ execute-python-script] moduł pozwala naukowca danych, aby dołączyć istniejący kod Python do przepływów pracy nauki hostowana w chmurze komputera w Azure maszynowego uczenia i bezproblemowo operationalize je jako część usługi sieci web. Moduł skryptu Python sposób naturalny współdziała z innymi modułami w Azure maszynowego uczenia i może być używany do zakresu zadań z badań danych w celu przetworzenia wstępnego do wyodrębniania funkcji oceny i po przetwarzania wyników. Środowisko uruchomieniowe wewnętrznej bazy danych na potrzeby wykonanie jest oparty na Anaconda, rozkładu Python dokładne testowanie i powszechnie używane. Dzięki temu prostej dla Ciebie płycie istniejących trwałych kod w chmurze.

Oczekujemy umożliwiają [Wykonywanie skryptów Python] dodatkowe funkcje[ execute-python-script] modułu, takich jak możliwość szkolenie i operationalize modeli w Python oraz dodawanie lepszą obsługę rozwoju i debugowania kodu w Azure maszynowego uczenia Studio.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji zobacz [Centrum deweloperów Python](/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
