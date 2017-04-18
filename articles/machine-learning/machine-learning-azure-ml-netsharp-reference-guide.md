<properties 
    pageTitle="Przewodnik po netto # sieci neuronowych specyfikacja języka | Microsoft Azure" 
    description="Składnia netto # neuronowych sieci specyfikacja języka, wraz z przykładami sposobu tworzenia modelu neuronowych niestandardowego w programie Microsoft Azure ML przy użyciu sieci#" 
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
    ms.date="09/12/2016" 
    ms.author="jeannt"/>



# <a name="guide-to-net-neural-network-specification-language-for-azure-machine-learning"></a>Przewodnik po netto # neuronowych specyfikacja języka szkoleniowe maszynowego Azure

## <a name="overview"></a>Omówienie
Netto # to opracowany przez firmę Microsoft, który służy do definiowania architektury neuronowych dla modułów neuronowych Microsoft Azure maszynowego uczenia język. W tym artykule dowiesz się:  

-   Podstawowych pojęć związanych z sieci neuronowych
-   Wymagania neuronowych i sposobu definiowania podstawowe składniki
-   Składnia i słów kluczowych netto # specyfikacja języka
-   Przykłady niestandardowych sieci neuronowych utworzony przy użyciu sieci# 
    
[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]  

## <a name="neural-network-basics"></a>Podstawowe informacje o neuronowych
Struktura neuronowych składa się z ***węzłów*** są zorganizowane w ***warstwy***i ważona ***połączeń*** (lub ***krawędzie***) między węzły. Połączenia są kierunkowe i każdego połączenia ma węzeł ***źródła*** i węzeł ***docelowy*** .  

Każdej ***warstwy trainable*** (ukrytych lub warstwy dane wyjściowe) zawiera co najmniej jeden ***wiązki połączenia***. Pakiet połączenia składa się z warstwy źródła i specyfikacji połączeń z tej warstwy źródła. Wszystkie połączenia w pakiecie danego udostępnianie tej samej ***warstwa źródłowa*** i tej samej ***warstwy docelowej***. W sieci # pakietu połączenie jest traktowane jako należące do warstwy docelowej pakietu.  
 
Netto # obsługuje różne rodzaje połączeń zestawy, które pozwala dostosować sposób dane wejściowe są mapowane na warstwach ukrytych i mapowane na wydajność.   

Domyślny lub standardowy pakietu jest **Pełny pakiet**, w którym każdy węzeł w warstwie źródłowej jest połączony z każdym węźle warstwy docelowej.  

Ponadto netto # obsługuje następujące cztery typy pakietów zaawansowanego połączenia:  

-   **Przefiltrowana pakiety**. Użytkownik może zdefiniować predykatu przy użyciu lokalizacji węzeł warstwy źródła i węzeł warstwy docelowy. Węzły są połączone, gdy predykat ma wartość PRAWDA.
-   **Zestawy convolutional**. Użytkownik może zdefiniować małych kluby węzłów na warstwie źródłowej. Każdy węzeł w warstwie docelowej jest połączony z jednym klubu węzłów na warstwie źródłowej.
-   **Buforowanie pakiety** i **zestawy normalizacji odpowiedź**. Te są podobne do convolutional wiązki w użytkownik zdefiniuje małych kluby węzłów na warstwie źródłowej. Różnica polega na tym, że grubości krawędzi w te zestawy nie są trainable. Zamiast tego funkcji wstępnie zdefiniowanej jest stosowana do wartości węzeł źródła w celu określenia wartości węzeł docelowy.  

Za pomocą sieci # do definiowania struktury neuronowych umożliwia definiowanie złożonych struktur, takich jak sieci głębokich neuronowych lub splotowi dowolnego wymiarów, które są znane aby poprawić nauki na danych, takich jak obraz, dźwięk lub wideo.  

## <a name="supported-customizations"></a>Obsługiwane dostosowania
Architektura modeli neuronowych, które można tworzyć w Azure maszynowego uczenia można dostosować szerokim zakresie przy użyciu netto #. Można:  

-   Tworzenie warstwy ukryte i kontrolować liczby węzłów na poszczególnych warstw.
-   Określ, jak mają być połączone ze sobą warstwy.
-   Definiowanie struktury łączności specjalne, takie jak splotowi i grubości udostępniania wiązki.
-   Określ różne aktywacji funkcji.  

Aby uzyskać szczegółowe informacje o Specyfikacja składni języka zobacz [Specyfikacja struktury](#Structure-specifications).  
 
Przykłady definiowania sieci neuronowych dla niektórych typowych maszynowego uczenia zadań, od simpleks do złożonych zobacz [Przykłady](#Examples-of-Net#-usage).  

## <a name="general-requirements"></a>Ogólne wymagania
-   Należy dokładnie jeden wynik warstwy, co najmniej jednej warstwy wprowadzania i zero lub więcej warstwy ukryte. 
-   Każda warstwa ma stałej liczby węzłów koncepcji rozmieszczone w tablicy prostokątnym dowolnego wymiarów. 
-   Warstwy wprowadzania mają bez skojarzone parametrów przeszkolony i reprezentują punktu gdzie dane wystąpienia wpływa sieci. 
-   Warstwy trainable (warstwy ukryte i wyjściowe) skojarzony przeszkolony parametry, nazywany grubości i odchyleń. 
-   Węzły źródłowa i docelowa muszą być w osobnych warstwach. 
-   Połączenia muszą być acykliczne; innymi słowy nie może być łańcuch połączenia prowadzące do węzła początkowego źródła.
-   Warstwy dane wyjściowe nie mogą być warstwa źródłowa wiązki połączenia.  

## <a name="structure-specifications"></a>Specyfikacje struktury
Specyfikacja struktury neuronowych składa się z trzech sekcji: **deklaracji stałej**, **deklaracji warstwy**, **deklaracji połączenia**. Istnieje także sekcję opcjonalne **Udostępnianie deklaracji** . Sekcje można określić w dowolnej kolejności.  

## <a name="constant-declaration"></a>Deklaracji stałej 
Deklaracji stałej jest opcjonalna. Zapewnia sposób, aby zdefiniować wartości używane w innych miejscach w definicji neuronowych. Instrukcja deklaracji składa się z identyfikatora, a po nim znak równości oraz wyrażenie wartości.   

Na przykład następująca instrukcja definiuje stałą **x**:  


    Const X = 28;  

Aby określić stałe dwóch lub więcej jednocześnie, należy ująć nazwy identyfikatorów i wartości w nawiasach klamrowych, a oddziel je średnikami. Na przykład:  

    Const { X = 28; Y = 4; }  

Prawej stronie każdego wyrażenia przydział może być liczbą całkowitą, liczba rzeczywista, wartość logiczną (PRAWDA lub FAŁSZ) lub wyrażenia matematycznego. Na przykład:  

    Const { X = 17 * 2; Y = true; }  

## <a name="layer-declaration"></a>Deklaracja warstwy
Deklaracja warstwa jest wymagana. Definiuje rozmiar i źródło warstwy, wraz z jej wiązki połączenia i atrybuty. Instrukcja deklaracji zaczyna się od nazwy warstwy (wprowadzania, ukryte lub wyjściowy) następuje wymiary warstwy (krotki dodatnie liczby całkowite). Na przykład:  

    input Data auto;
    hidden Hidden[5,20] from Data all;
    output Result[2] from Hidden all;  

-   Iloczyn wymiarów jest liczby węzłów na warstwie. W tym przykładzie istnieją dwa wymiary [5,20], co oznacza, że w warstwie znajdują się węzły 100.
-   Warstwy mogą być deklarowane w dowolnej kolejności, z jednym wyjątkiem: Jeśli zdefiniowano więcej niż jednej warstwy wprowadzania, kolejność, w której są one zgłoszone musi odpowiadać kolejności funkcje danych wejściowych.  


Aby określić, że liczby węzłów na warstwie ustalana automatycznie, użyj słowa kluczowego **automatycznie** . **Automatyczne** słów kluczowych obejmuje różnych efektów, w zależności od warstwy:  

-   W deklaracji warstwy wprowadzania liczby węzłów jest liczba funkcje danych wejściowych.
-   W deklaracji ukryte warstwy liczby węzłów jest numer, który jest określony przez wartość parametru dla **liczby węzłów**. 
-   W deklaracji warstwy dane wyjściowe liczby węzłów jest 2 dla dwóch klasy klasyfikacji, 1 dla regresji i równej liczby węzłów wynik klasyfikacji multiclass.   

Na przykład następujące definicje sieci umożliwia rozmiar wszystkie warstwy, aby automatycznie określane:  

    input Data auto;
    hidden Hidden auto from Data all;
    output Result auto from Hidden all;  


Deklarację warstwy dla warstwy trainable (warstw ukrytych lub wyjściowe) można opcjonalnie uwzględnić dane wyjściowe funkcję (zwanych również funkcję aktywacji), która zostanie przyjęta wartość domyślna **sigmoid** modeli klasyfikacji i **liniowy** w przypadku modeli regresji. (Nawet wtedy, gdy użytkownik korzysta z domyślnych można jawnie określane funkcji aktywacji, w razie potrzeby dla jasności.)

Obsługiwane są następujące funkcje wynik:  

-   sigmoid
-   liniowy
-   softmax
-   rlinear
-   kwadratowy
-   pierwiastek
-   srlinear
-   Moduł.liczby
-   TANH 
-   brlinear  

Na przykład następująca deklaracja użyto funkcji **softmax** :  

    output Result [100] softmax from Hidden all;  

## <a name="connection-declaration"></a>Deklaracja połączenia
Natychmiast po zdefiniowaniu trainable warstwy, należy zadeklarować powiązania między poszczególnymi warstwy, które zostały zdefiniowane. Deklaracja pakietu połączenia rozpoczyna się od słów kluczowych **z**, a po nim nazwa pakietu warstwa źródłowa i rodzaju pakietu połączenia, aby utworzyć.   

Obecnie obsługiwane są pięć rodzajów wiązki połączenia:  

-   Pakiety **Pełny** , wskazywana przez słowo kluczowe **wszystkich**
-   **Przefiltrowana** pakiety, wskazywana przez słowo kluczowe **miejsce, w którym**, wraz z wyrażeniem predykatu
-   Zestawy **Convolutional** , wskazywana przez słowo kluczowe **convolve**, a po nim atrybuty Konwolucja
-   Pakiety **Pooling** , wskazywana przez słów kluczowych **puli max** lub **oznacza puli**
-   Pakiety **normalizacji odpowiedzi** , wskazywana przez słowo kluczowe **normę odpowiedzi**      

## <a name="full-bundles"></a>Pełna zestawy  

Pakiet pełne połączenie zawiera połączenie z każdego węzła w warstwie źródłowej do każdego węzła w warstwie docelowej. To jest domyślny typ połączenia sieciowego.  

## <a name="filtered-bundles"></a>Pakiety filtrowane
Specyfikacja pakietu filtrowane połączenie zawiera predykatu, syntaktycznie, wyrażona fragment odpowiadający takich jak wyrażenia lambda C#. W poniższym przykładzie zdefiniowano dwa zestawy filtrowania:  

    input Pixels [10, 20];
    hidden ByRow[10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol[5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;  

-   W predykacie _ByRow_ **s** to parametr przedstawiającą indeks do tablicy prostokątnym węzłów warstwie wprowadzania _pikseli_i **d** jest parametr przedstawiającą indeks do tablicy węzły ukryte warstwy _ByRow_. Typ **d** i **s** jest krotki liczby całkowite długość dwóch. Koncepcji, **s** zakresy na wszystkich par wartości całkowitych z _0 < = s [0] < 10_ i _0 < = s[1] < 20_, i **d** zakresy na wszystkich par liczb całkowitych, z _0 < = d [0] < 10_ i _0 < = d[1] < 12_. 
-   Po prawej stronie predykatu wyrażenia istnieje warunku. W tym przykładzie dla wszystkich wartości **s** i **d** tak, aby warunek ma wartość PRAWDA, istnieje krawędź z węzła warstwy źródła do miejsca docelowego węzła warstwy. W związku z tym to wyrażenie filtru wskazuje, że pakietu zawiera połączenie z węzła zdefiniowanych przez **s** , aby węzeł zdefiniowanych przez **d** we wszystkich przypadkach, gdzie s [0] jest równa d [0].  

Opcjonalnie można określić zestaw masy filtrowanych pakietu. Wartość atrybutu **grubości** musi być krotki wartości zmiennoprzecinkowe o długości, która jest zgodna z liczbą połączeń zdefiniowanych przez pakietu. Domyślnie generowany losowo grubości.  

Waga wartości są grupowane według indeksu węzła docelowego. Oznacza to, że jeśli pierwszy węzeł docelowy jest podłączone do węzłów źródła K, pierwszy elementy _K_ krotki **grubości** są wagi pierwszy węzeł docelowy w kolejności indeks źródła. To samo dotyczy dla pozostałych węzłach miejsca docelowego.  

Użytkownik może określić grubości bezpośrednio jako wartości stałych. Na przykład jeśli wiesz już wagi, można je określić w stałych przy użyciu następującej składni:

    const Weights_1 = [0.0188045055, 0.130500451, ...]


## <a name="convolutional-bundles"></a>Zestawy convolutional
Dane szkolenia po jednorodnych struktury convolutional połączenia są powszechnie używane Aby dowiedzieć się, wysokiego poziomu funkcje danych. Na przykład w obszarze obraz dane audio lub wideo, wymiarze przestrzenna lub czasowy może być dość jednolite.  

Zestawy convolutional zastosować prostokątnym **jądra** , które są przesuwać za pośrednictwem wymiarów. W istocie każdy jądra definiuje zestaw grubości stosowane w lokalnym kluby określane jako **jądra aplikacji**. Każda aplikacja jądra odpowiada węzeł na warstwie źródła nosi nazwę **węzła centralnej**. Masy jądra są udostępniane między wiele połączeń. W łączu convolutional każdy jądra jest prostokątny i wszystkie aplikacje jądra mają taki sam rozmiar.  

Zestawy convolutional obsługuje następujące atrybuty:

**InputShape** Określa wymiary warstwa źródłowa na potrzeby tego pakietu convolutional. Wartość musi być krotki dodatnich liczb całkowitych. Iloczyn liczby całkowite musi być równa liczby węzłów na warstwie źródłowej, ale w przeciwnym razie nie potrzebuje zgodnie z wymiarze zadeklarowane na warstwie źródłowej. Długość tego krotki staje się wartość **argumentów** dla pakietu convolutional. (Zazwyczaj argumentów odwołuje się do liczby argumentów lub argumentów operacji, które można wykonać, funkcji.)  

Aby określić kształt i lokalizacje jądra, użyj atrybutów **KernelShape**, **pobieżne** **Dopełnienie**, **LowerPad**i **UpperPad**:   

-   **KernelShape**: (wymagane) definiuje wymiary każdego jądra dla pakietu convolutional. Wartość musi być krotki dodatnich liczb całkowitych o długości, która jest równe liczby argumentów pakietu. Każdą część tej krotki musi być nie większa niż odpowiedni składnik **InputShape**. 
-   **Pobieżne**: (opcjonalnie) określa przesuwania rozmiary kroku Konwolucja (rozmiar krok dla każdego wymiaru), jest odległości między centralnej węzły. Wartość musi być krotki dodatnich liczb całkowitych o długości, która jest liczby argumentów pakietu. Każdą część tej krotki musi być nie większa niż odpowiedni składnik **KernelShape**. Wartość domyślna to krotki z wszystkimi składnikami równe jeden. 
-   **Udostępnianie**: (opcjonalnie) określa grubość udostępniania dla każdego wymiaru Konwolucja. Wartość może być pojedynczą wartość logiczną lub krotki wartości logiczne o długości, która jest liczby argumentów pakietu. Pojedyncza wartość logiczna jest używane być krotki właściwą długość z wszystkimi składnikami równe określonej wartości. Wartość domyślna to krotki zawierający wszystkie wartości True. 
-   **MapCount**: (opcjonalnie) definiuje liczbę funkcji dla pakietu convolutional mapy. Wartość może być pojedynczy dodatniej liczby całkowitej lub krotki dodatnich liczb całkowitych o długości, która jest liczby argumentów pakietu. Pojedynczy całkowitą zostanie rozszerzony być krotki właściwą długość z pierwszej składniki równa się określona wartość i pozostałe składniki równe jeden. Wartość domyślna to. Całkowita liczba mapy funkcji jest iloczyn części krotki. Factoring to liczba składniki określa sposób grupowania wartości mapy funkcji w węzłach miejsca docelowego. 
-   **Waga**: (opcjonalnie) określa początkowej wagi pakietu. Wartość musi być krotki przestawnych liczbę grubości wartości o długości, która jest liczba jądra na jądra, zgodnie z definicją w dalszej części tego artykułu. Generowany losowo grubości domyślne.  

Istnieją dwa zestawy właściwości, które sterują dopełnienie właściwości są wzajemnie wykluczających się:

-   **Dopełnienie**: (opcjonalnie) określa czy dane wejściowe należy wypełniane przy użyciu **domyślnego dopełnienia schematu**. Wartość może być pojedynczą wartość logiczną lub może być krotki wartości logiczne o długości, która jest liczby argumentów pakietu. Pojedyncza wartość logiczna jest używane być krotki właściwą długość z wszystkimi składnikami równe określonej wartości. Jeśli wartość w polu Wymiar ma wartość PRAWDA, źródła jest logicznie wypełniane w tym wymiarze z wartości zero komórek do obsługi aplikacji dodatkowe jądra tak, aby były centralnej węzły jądra imię i nazwisko w tym wymiarze węzły imię i nazwisko w tym wymiarze w warstwie źródłowej. W związku z tym węzłów "fikcyjna" w każdym wymiarze jest określana automatycznie, aby dopasować dokładnie _(InputShape [d] - 1) / pobieżne [d] + 1_ jądra do warstwy wyściełane źródła. Jeśli wartość w polu Wymiar ma wartość FAŁSZ, jądra są definiowane tak, aby liczby węzłów na każdej stronie, które pozostają się takie same (maksymalnie różnica 1). Wartość domyślna dany atrybut jest krotki z wszystkimi składnikami równa FAŁSZ.
-   **UpperPad** i **LowerPad**: (opcjonalnie) Podaj większą kontrolę nad wielkość dopełnienia za pomocą. **Ważne:** Następujące atrybuty można zdefiniować tylko wtedy, gdy właściwość **Dopełnienie** powyżej jest ***nie*** zdefiniowano. Wartości powinny być wartości całkowitych krotki o długości, które są liczby argumentów pakietu. Gdy określono następujące atrybuty, "fikcyjna" węzły zostaną dodane do dolnym i górnym końcu każdego wymiaru wprowadzania warstwy. Liczby węzłów dodane do punktów końcowych dolnym i górnym w każdym wymiarze jest określona przez **LowerPad**[i] i **UpperPad**[i] odpowiednio. Aby upewnić się, że jądra odpowiadać tylko węzły "rzeczywistą" i nie chcesz, aby węzły "fikcyjna", muszą być spełnione następujące warunki:
    -   Każdy element **LowerPad** musi być ściśle mniejsze niż KernelShape [d] / 2. 
    -   Każdy element **UpperPad** musi być nie większa niż KernelShape [d] / 2. 
    -   Wartość domyślna następujące atrybuty jest krotki z wszystkimi składnikami równa 0. 

Ustawienie **Dopełnienie** = true umożliwia tyle dopełnienia odpowiednio do zachowania "Centrum" jądra wewnątrz "rzeczywistą" wprowadzania. Spowoduje to zmianę matematyczne nieco dla przetwarzania wyjściowy rozmiar. Ogólnie rzecz biorąc, wyjściowy rozmiar _D_ jest obliczana jako _D = (I – K)-S + 1_ _przypadku rozmiar wejściowy_ _K_ jest wielkością jądra, _S_ to krok, i _/_ jest dzielenia liczby całkowitej (ZAOKR w kierunku zera). Po ustawieniu UpperPad = [1, 1], rozmiar wejściowy _I_ wynosi ostatecznie 29 i w ten sposób _D = (29-5)-2 + 1 = 13_. Jednak, jeśli **Dopełnienie** = PRAWDA, zasadniczo, _I_ otrzymuje upadku w górę o _K - 1_; w związku z tym _D = ((28 + 4) - 5)-2 + 1 = 27 / 2 + 1 = 13 + 1 = 14_. Określając wartości **UpperPad** i **LowerPad** możesz uzyskać znacznie większą kontrolę nad dopełnienia niż Jeśli ustawione **Dopełnienie** = true.

Aby uzyskać więcej informacji o sieciach convolutional i ich zastosowań zobacz następujące artykuły:  

-   [http://deeplearning.NET/Tutorial/lenet.HTML](http://deeplearning.net/tutorial/lenet.html )
-   [http://Research.microsoft.com/pubs/68920/icdar03.PDF](http://research.microsoft.com/pubs/68920/icdar03.pdf) 
-   [http://People.csail.mit.edu/jvb/Papers/cnn_tutorial.PDF](http://people.csail.mit.edu/jvb/papers/cnn_tutorial.pdf)  

## <a name="pooling-bundles"></a>Buforowanie zestawy
**Buforowanie pakietu** dotyczy geometrii podobne do convolutional łączności, ale używa funkcji wstępnie zdefiniowanych wartości węzeł źródła do uzyskania wartości węzeł docelowy. W związku z tym buforowania pakiety mają nie trainable stan (grubości lub odchyleń). Pakiety buforowania obsługuje wszystkich atrybutów convolutional z wyjątkiem **Udostępnianie**, **MapCount**i **grubości**.  

Zazwyczaj jądra zsumowanych sąsiednich jednostkach buforowania nie pokrywają się. Jeśli krok [d] jest równa KernelShape [d] w każdym wymiarze, tradycyjna lokalnego buforowania warstwy, która jest powszechnie wykorzystywane w convolutional sieci neuronowych zostanie warstwa uzyskane w wyniku. Każdy węzeł docelowy oblicza maksymalną lub średnią działań jego jądra w warstwie źródłowej.  

Poniższy przykład przedstawia buforowania pakietu: 

    hidden P1 [5, 12, 12]
      from C1 max pool {
        InputShape  = [ 5, 24, 24];
        KernelShape = [ 1,  2,  2];
        Stride      = [ 1,  2,  2];
      }  

-   Liczba argumentów w pakiecie wynosi 3 (długość krotki **InputShape**, **KernelShape**i **pobieżne**). 
-   Liczba węzłów na warstwie źródłowej jest _5 *24* 24 = 2880_. 
-   Jest to tradycyjny lokalnego buforowania warstwy, ponieważ **KernelShape** i **pobieżne** są równe. 
-   Liczba węzłów na warstwie docelowej jest _5 *12* 12 = 1440_.  
    
Aby uzyskać więcej informacji na temat buforowania warstwy zobacz następujące artykuły:  

-   [http://www.cs.Toronto.edu/~hinton/absps/imagenet.PDF](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf) (Sekcji 3.4)
-   [http://CS.nyu.edu/~koray/Publis/lecun-iscas-10.PDF](http://cs.nyu.edu/~koray/publis/lecun-iscas-10.pdf) 
-   [http://CS.nyu.edu/~koray/Publis/jarrett-iccv-09.PDF](http://cs.nyu.edu/~koray/publis/jarrett-iccv-09.pdf)
    
## <a name="response-normalization-bundles"></a>Pakiety normalizacji odpowiedzi
**Odpowiedź normalizacji** jest schemat lokalne normalizacji, najpierw wprowadzonej przez Geoffrey Hinton i wsp w dokumencie [ImageNet Classiﬁcation z głębokości Convolutional sieci neuronowych](http://www.cs.toronto.edu/~hinton/absps/imagenet.pdf). Odpowiedź normalizacji jest używany do pomocy Generalizacja w sieci neuronowych. Gdy jeden neuronu jest uruchamiane na poziomie aktywacji bardzo wysoki, warstwy normalizacji lokalne odpowiedzi pomija poziom aktywacji otaczającego grupy neuronów. Jest to zrobić przy użyciu trzech parametrów (***α***, ***β***i ***k***) i struktury convolutional (lub klubu kształt). Co neuronu warstwy docelowej ***y*** odpowiada neuronu ***x*** w warstwie źródłowej. Poziom aktywacji ***y*** , wyznaczona przez następującą formułę, gdzie ***f*** jest poziom aktywacji neuronu i ***Nx*** jest jądro (lub zestaw, który zawiera grupy neuronów w klubie ***x***), zdefiniowane przez następującą strukturę convolutional:  

![][1]  

Pakiety normalizacji odpowiedzi obsługuje wszystkich atrybutów convolutional z wyjątkiem **Udostępnianie**, **MapCount**i **grubości**.  
 
-   Jeśli jądrze zawiera grupy neuronów w tym samym Mapa jako ***x***, schemat normalizacji nazywa się **tym samym normalizacji Mapa**. Aby zdefiniować samej normalizacji mapy, pierwszy współrzędnych **InputShape** musi mieć wartość 1.
-   Jeśli jądrze zawiera grupy neuronów w tej samej pozycji przestrzenna jako ***x***, ale grupy neuronów są dostępne w innych mapy, normalizacji schemat nosi nazwę **przez normalizacji mapy**. Ten rodzaj normalizacji odpowiedzi wykonuje formularza inhibicji boczne przez typ znaleziony w rzeczywistą grupy neuronów, tworzenie konkurencji dla poziomów duży aktywacji między wyjściowe neuronu obliczone za pomocą różnych mapy. Aby zdefiniować wielu normalizacji mapy, pierwszy współrzędnych musi być liczbą całkowitą, więcej niż jeden i nie większa niż liczba mapy, a pozostałe współrzędne musi mieć wartość 1.  

Ponieważ wiązki normalizacji odpowiedzi dotyczą funkcji wstępnie zdefiniowanej wartości węzeł źródła w celu określenia wartości węzeł docelowy, mają nie trainable stan (grubości lub odchyleń).   

**Alert**: węzłów na warstwie docelowej odpowiadają grupy neuronów, które centralnej węzłów jądra. Na przykład jeśli KernelShape [d] jest nieparzysta, następnie _KernelShape [d] / 2_ odpowiada węzeł centralnej jądra. Jeśli nie jest jeszcze _KernelShape [d]_ , węzeł centralnej znajduje się na _KernelShape [d] / 2-1_. W związku z tym jeśli **Dopełnienie**[d] jest wartość FAŁSZ, pierwszego i ostatniego _KernelShape [d] / 2_ węzły nie masz odpowiednich węzły warstwy docelowej. Aby tego uniknąć, należy zdefiniować **Dopełnienie** jako [true ma wartość PRAWDA, PRAWDA,...,].  

Oprócz czterech atrybutów opisanych wcześniej zestawy normalizacji odpowiedzi obsługują następujące atrybuty:  

-   **Alfa**: (wymagane) określa zmiennoprzecinkowa wartość, która odpowiada ***α*** w poprzednią formułę. 
-   **Beta**: (wymagane) określa zmiennoprzecinkowa wartość, która odpowiada ***β*** w poprzednią formułę. 
-   **Przesunięcie**: Określa (opcjonalnie) wartość zmiennoprzecinkowa, która odpowiada ***k*** w poprzednią formułę. Domyślnie 1.  

W poniższym przykładzie zdefiniowano pakietu normalizacji odpowiedzi przy użyciu następujące atrybuty:  

    hidden RN1 [5, 10, 10]
      from P1 response norm {
        InputShape  = [ 5, 12, 12];
        KernelShape = [ 1,  3,  3];
        Alpha = 0.001;
        Beta = 0.75;
      }  

-   Warstwa źródłowa zawiera pięć mapy, każda z wymiarów aof 12 x 12, sumowanie w węzłach 1440. 
-   Wartość **KernelShape** wskazuje, że jest tym samym warstwy normalizacji mapy, gdzie klubu jest prostokąt 3 x 3. 
-   Wartość domyślna **Dopełnienie** ma wartość FAŁSZ, więc warstwy docelowej ma tylko 10 węzły w każdym wymiarze. Aby dołączyć jeden węzeł odpowiadający każdy węzeł w warstwie źródłowej warstwy docelowej, Dodaj dopełnienia = [wartość PRAWDA, wartość PRAWDA, prawdziwe]; i zmienianie rozmiaru RN1 [5, 12, 12].  

## <a name="share-declaration"></a>Udostępnianie deklaracji 
Netto # obsługuje opcjonalnie definiowania wielu pakiety z udostępnionej grubości. Masy dowolne dwa zestawy mogą być udostępniane, jeśli ich struktur są takie same. Następującej składni definiuje pakiety z udostępnionej grubości:  

    share-declaration:
        share    {    layer-list    }
        share    {    bundle-list    }
       share    {    bias-list    }
    
    layer-list:
        layer-name    ,    layer-name
        layer-list    ,    layer-name
    
    bundle-list:
       bundle-spec    ,    bundle-spec
        bundle-list    ,    bundle-spec
    
    bundle-spec:
       layer-name    =>     layer-name
    
    bias-list:
        bias-spec    ,    bias-spec
        bias-list    ,    bias-spec
    
    bias-spec:
        1    =>    layer-name
    
    layer-name:
        identifier  

Na przykład w następującej deklaracji Udostępnij określa nazwy warstwy, wskazująca, że ma być udostępniany zarówno grubości i odchyleń:  

    Const {
      InputSize = 37;
      HiddenSize = 50;
    }
    input {
      Data1 [InputSize];
      Data2 [InputSize];
    }
    hidden {
      H1 [HiddenSize] from Data1 all;
      H2 [HiddenSize] from Data2 all;
    }
    output Result [2] {
      from H1 all;
      from H2 all;
    }
    share { H1, H2 } // share both weights and biases  

-   Funkcje wprowadzania są podzielone na równe wymiarach warstwy wprowadzania danych. 
-   Ukryte warstwy należy obliczyć wyższy poziom funkcji na dwie warstwy wprowadzania. 
-   Deklaracja Udostępnij Określa, że _H1_ i _H2_ musi zostać obliczona w taki sam sposób, z ich odpowiednich danych wejściowych.  
 
Możesz też to może być określony z dwóch oddzielnych deklaracji udział w następujący sposób:  

    share { Data1 => H1, Data2 => H2 } // share weights  

<!-- -->

    share { 1 => H1, 1 => H2 } // share biases  

Za pomocą formularza skróconego tylko wtedy, gdy warstwy zawierają jednym pakiecie. Na ogół jest możliwe udostępnianie tylko wtedy, gdy odpowiednia struktura jest identyczna, co oznacza, że mają ten sam rozmiar, tym samym geometrii convolutional i tak dalej.  

## <a name="examples-of-net-usage"></a>Przykłady zastosowania netto #
W tej sekcji podano kilka przykładów, jak można używać netto # Dodaj warstwy ukryte, definiowanie sposobu ukryte warstwy Interakcje z innymi warstwami i tworzenie convolutional sieci.   

### <a name="define-a-simple-custom-neural-network-hello-world-example"></a>Definiowanie prostych niestandardowe neuronowych: przykład "Witaj świecie"
Ten prosty przykład przedstawia sposób tworzenia modelu neuronowych, który zawiera jedną warstwę ukryte.  

    input Data auto;
    hidden H [200] from Data all;
    output Out [10] sigmoid from H all;  

W przykładzie zilustrowano niektóre podstawowe polecenia w następujący sposób:  

-   Pierwszy wiersz definiuje warstwie wprowadzania (o nazwie _danych_). Użycie **automatycznego** słowo kluczowe, neuronowych automatycznie uwzględnia wszystkie kolumny funkcji w przykładach wprowadzania. 
-   Drugi wiersz tworzy ukryte warstwy. Nazwa _H_ jest przypisana do ukryte warstwy, która ma węzły 200. Tej warstwy jest w pełni podłączony do wprowadzania warstwy.
-   Trzeci wiersz określa dane wyjściowe warstwy (o nazwie _O_), która zawiera 10 węzły dane wyjściowe. Użycie neuronowych klasyfikacji jest jeden węzeł dane wyjściowe na zajęć. Słowo kluczowe **sigmoid** wskazuje, że funkcja wynik jest stosowane do warstwy dane wyjściowe.   

### <a name="define-multiple-hidden-layers-computer-vision-example"></a>Definiowanie wielu warstw ukrytych: przykład wzroku komputera
Poniższy przykład ilustruje definiowanie nieco bardziej złożone neuronowych, z wielu niestandardowy warstw ukrytych.  

    // Define the input layers 
    input Pixels [10, 20];
    input MetaData [7];
    
    // Define the first two hidden layers, using data only from the Pixels input
    hidden ByRow [10, 12] from Pixels where (s,d) => s[0] == d[0];
    hidden ByCol [5, 20] from Pixels where (s,d) => abs(s[1] - d[1]) <= 1;
    
    // Define the third hidden layer, which uses as source the hidden layers ByRow and ByCol
    hidden Gather [100] 
    {
      from ByRow all;
      from ByCol all;
    }
    
    // Define the output layer and its sources
    output Result [10]  
    {
      from Gather all;
      from MetaData all;
    }  

W tym przykładzie przedstawiono kilka funkcji sieci neuronowych specyfikacja języka:  

-   Struktura ma dwie warstwy wprowadzania _pikseli_ i _metadanych_.
-   Warstwy _pikseli_ jest warstwę źródła dla dwa zestawy połączenia, przy użyciu warstwach docelowego, _ByRow_ i _ByCol_.
-   Warstwy, _gromadzenie_ i _wynik_ to miejsce docelowe warstw w wielu wiązki połączenia.
-   Dane wyjściowe warstwy, _wynik_jest warstwy docelowej w dwa zestawy połączenia; jeden z drugiego poziomu ukryte (zbieranie) jako warstwę miejsca docelowego, a drugi z warstwy wprowadzania (metadane) jako warstwy docelowej.
-   Warstwy ukryte, _ByRow_ i _ByCol_, określ łączności filtrowane za pomocą wyrażeń predykatu. Bardziej precyzyjne węzła w _ByRow_ w [x, y] jest podłączony do węzłów w _pikselach_ , które mają pierwszego współrzędnych indeks równe współrzędnych pierwszego węzła, x. Podobnie węzeł _ByCol u [x, y] jest podłączony do węzłów w _pikselach_ , które mają drugiego indeksu koordynowanie w jednej z drugim współrzędnych węzła, y.  

### <a name="define-a-convolutional-network-for-multiclass-classification-digit-recognition-example"></a>Definiowanie sieci convolutional klasyfikacji multiclass: przykład rozpoznawania cyfr
Definicji następującej sieci jest przeznaczona do rozpoznaje numerów, a jego ilustruje zaawansowanych technik dostosowywania neuronowych.  

    input Image [29, 29];
    hidden Conv1 [5, 13, 13] from Image convolve 
    {
       InputShape  = [29, 29];
       KernelShape = [ 5,  5];
       Stride      = [ 2,  2];
       MapCount    = 5;
    }
    hidden Conv2 [50, 5, 5]
    from Conv1 convolve 
    {
       InputShape  = [ 5, 13, 13];
       KernelShape = [ 1,  5,  5];
       Stride      = [ 1,  2,  2];
       Sharing     = [false, true, true];
       MapCount    = 10;
    }
    hidden Hid3 [100] from Conv2 all;
    output Digit [10] from Hid3 all;  


-   Struktura ma wprowadzania warstwę, _obraz_.
-   Słowo kluczowe **convolve** wskazuje, że warstwy o nazwie _Conv1_ i _Conv2_ convolutional warstwy. Każdy z tych deklaracji warstwy następuje listę atrybutów Konwolucja.
-   Sieć ma innego ukryte warstwy, _Hid3_, które pełni jest połączone z drugą warstwę ukrytych _Conv2_.
-   Warstwy dane wyjściowe, _cyfry_jest połączony tylko trzecich ukrytej warstwie, _Hid3_. Słowo kluczowe **wszystkich** wskazuje, że warstwy dane wyjściowe jest w pełni połączony _Hid3_.
-   Liczby argumentów Konwolucja to trzy (długość krotki **InputShape**, **KernelShape** **pobieżne**i **Udostępnianie**). 
-   Liczba grubości na jądra jest _1 + **KernelShape**\[0] * * *KernelShape**\[1]* **KernelShape**\[2] = 1 + 1 *5* 5 = 26. Lub 26 * 50 = 1300_.
-   Węzły w każdej warstwie ukrytych można obliczyć w następujący sposób:
    -   **NodeCount**\[0] = (5 - 1)-1 + 1 = 5.
    -   **NodeCount**\[1] = (13-5)-2 + 1 = 5. 
    -   **NodeCount**\[2] = (13-5)-2 + 1 = 5. 
-   Całkowita liczba węzły można obliczyć za pomocą zadeklarowanych wymiarze warstwy [50, 5, 5], w następujący sposób: _ **MapCount** * * *NodeCount**\[0]* **NodeCount**\[1] * * *NodeCount**\[2] = 10* 5 *5* 5_
-   Ponieważ **Udostępnianie**[d] ma wartość FAŁSZ tylko w przypadku _d == 0_, liczba jądra jest _ **MapCount** * * *NodeCount**\[0] = 10* 5 = 50_. 


## <a name="acknowledgements"></a>Potwierdzenia

Dostosowywanie architektury sieci neuronowych język netto # opracowano w firmie Microsoft, Shon Katzenberger (architektury, nauki komputera) i Kamenev Aleksieja (odtwarzania oprogramowania, Microsoft Research). Jest ono używane wewnętrznie dla maszynowego uczenia projektów i aplikacji od wykrywania obraz do analizy tekstu. Aby uzyskać więcej informacji zobacz [Sieci neuronowych na ML Azure — wprowadzenie do sieci #](http://blogs.technet.com/b/machinelearning/archive/2015/02/16/neural-nets-in-azure-ml-introduction-to-net.aspx)


[1]:./media/machine-learning-azure-ml-netsharp-reference-guide/formula_large.gif
 
