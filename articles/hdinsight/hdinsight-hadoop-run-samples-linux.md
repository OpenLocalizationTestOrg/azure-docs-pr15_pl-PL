<properties
    pageTitle="Uruchom Hadoop MapReduce próbki na podstawie Linux HDInsight | Microsoft Azure"
    description="Wprowadzenie do korzystania z systemem Linux HDInsight MapReduce próbki. Nawiązywanie połączenia z klastrem za pomocą SSH, a następnie przykładowe zadań za pomocą polecenia Hadoop."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>




#<a name="run-the-hadoop-samples-in-hdinsight"></a>Uruchamianie próbki Hadoop w HDInsight

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Systemem Linux klastrów HDInsight zawierają zestaw próbek MapReduce, które mogą być używane w celu zapoznania się z systemem Hadoop MapReduce zadania. W tym dokumencie Dowiedz się więcej o dostępnych próbki i szczegółową uruchomiony niektóre z nich.

##<a name="prerequisites"></a>Wymagania wstępne

- **Subskrypcja Azure**: zobacz [Azure pobieranie bezpłatnej wersji próbnej](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

- **Klaster systemem Linux HDInsight**: zobacz [Wprowadzenie do korzystania z Hadoop z gałąź w HDInsight w systemie Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

- **Klient SSH**: Aby uzyskać informacje na temat korzystania z usługi HDInsight SSH, zobacz następujące artykuły:

    - [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="the-samples"></a>Przykłady ##

**Lokalizacja**: przykłady znajdują się w klastrze HDInsight na **/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar**

**Zawartość**: następujące próbki są zawarte w archiwum:

- **aggregatewordcount**: Mapa/zmniejszanie program, który prowadzi statystykę wpisywanych wyrazów w plikach wprowadzania na podstawie agregacji
- **aggregatewordhist**: Mapa/zmniejszanie program, który oblicza histogram wyrazy w polu wprowadzania pliki na podstawie agregacji
- **BBP**: mapy i zmniejszyć program, który używa Bailey-Borwein-Plouffe do obliczenia dokładne dwucyfrowej liczby Pi.
- **dbcount**: zadanie przykład licząca dzienniki widok strony przechowywanych w bazie danych
- **distbbp**: Mapa/zmniejszanie program, który jest używana formuła typu BBP do obliczenia dokładnie bitów liczby Pi.
- **GREP**: Mapa/zmniejszanie programu licząca dopasowania regex w danych wejściowych
- **Dołącz**: zadanie efekty sprzężenia sortowane przekraczających, jednakowo podzielona zestawy danych
- **multifilewc**: zadanie zlicza wyrazy z kilku plików
- **pentomino**: Mapa/zmniejszanie kafelka zniesienia programowi rozwiązania problemów pentomino
- **pi**: programu mapy i zmniejszyć szacuje Pi przy użyciu quasi-Monte Carlo metody
- **randomtextwriter**: Mapa/zmniejszanie program, który zapisuje 10 GB losowe danych tekstowych na węzeł
- **randomwriter**: Mapa/zmniejszanie program, który zapisuje 10 GB danych losowe na węzeł
- **secondarysort**: przykład określające sortowania dodatkowego zmniejszanie
- **Sortowanie**: Mapa/zmniejszanie programu sortujące zapisanych przez przypadkowe autora danych
- **sudoku**: sudoku dodatku solver
- **teragen**: generować dane o terasort
- **terasort**: uruchamianie terasort
- **teravalidate**: Sprawdzanie wyników terasort
- **WordCount**: Mapa/zmniejszanie program, który prowadzi statystykę wpisywanych wyrazów w plikach wprowadzania
- **wordmean**: licząca Średni czas wyrazy w polu wprowadzania pliki programu mapy/zmniejszanie
- **wordmedian**: Mapa/zmniejszanie program, który oblicza medianę długość wyrazy w polu wprowadzania pliki
- **wordstandarddeviation**: Mapa/zmniejszanie program, który oblicza odchylenie standardowe o długości wyrazy w polu wprowadzania pliki

**Kod źródłowy**: kod źródłowy dla tych przykładów znajduje się w klastrze HDInsight na **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples**

> [AZURE.NOTE] `2.2.4.9-1` w ścieżce jest wersja platformy danych Hortonworks dla klastrów HDInsight oraz mogą ulec zmianie jako HDInsight zostanie zaktualizowany.

## <a name="how-to-run-the-samples"></a>Jak uruchomić przykłady ##

1. Nawiązywanie połączenia z usługą hdinsight za pomocą SSH, zgodnie z opisem w następujących artykułach:

    - [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Z `username@#######:~$` monit, użyj następującego polecenia, aby wyświetlić listę próbki:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar

    Na liście przykładowe generuje od poprzedniej sekcji tego dokumentu.

3. Użyj następującego polecenia, aby uzyskać pomoc dotyczącą określonych próbki. W tym przypadku próbki **wordcount** :

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount

    Czy jest wyświetlany następujący komunikat:

        Usage: wordcount <in> [<in>...] <out>

    Oznacza to, które można udostępniać kilka ścieżek wprowadzania dokumentów źródłowych. Ostateczne ścieżka to gdzie są przechowywane dane wyjściowe (liczba wyrazów w dokumentach źródłowych).

4. Zliczanie wszystkie wyrazy w notesów programu Leonardo Da Vinci, które są dostarczane jako przykładowe dane z klaster należy wykonać następujące kroki:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount

    Danych wejściowych dla tego zadania jest odczytywana z **wasbs:///example/data/gutenberg/davinci.txt**.

    Wyjściowy w tym przykładzie są przechowywane w **wasbs: / / / przykład/danych i davinciwordcount**.

    > [AZURE.NOTE] Jak podano w pomocy dla próbki wordcount, można także określić kilka plików wejściowych. Na przykład `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` chcesz zliczyć wyrazy w davinci.txt i ulysses.txt.

5. Po zakończeniu zadania, użyj następującego polecenia, aby wyświetlić wynik:

        hdfs dfs -cat /example/data/davinciwordcount/*

    Łączy wszystkie pliki wyjściowe tworzone przez zadanie i wyświetl je. W tym przykładzie podstawowe jest tylko jeden plik, jednak jeśli dokonano więcej tego polecenia chcesz przejść przez każdy z nich.

    Wynik jest podobny do następującego:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Każdy wiersz reprezentuje wyrazu i ile razy wystąpił danych wejściowych.

## <a name="sudoku"></a>Sudoku

Przykład Sudoku zawiera instrukcje dotyczące użytkowania nieco nieprzydatne; "Zawierać zagadki w wierszu polecenia".

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) jest zagadki logiczny składa się z siatki dziewięciu 3 x 3. Niektóre komórki w siatce numerami, a inne są puste, chodzi o rozwiązywaniu dla pustych komórek. Łącze powyżej zawiera więcej informacji na temat zagadki, ale w tym przykładzie ma na celu rozwiązywanie dla pustych komórek. Dlatego nasze dane wejściowe powinny być pliku znajdującego się w następującym formacie:

- Dziewięć wierszy dziewięciu kolumn

- Każda kolumna może zawierać albo liczbą lub `?` (który wskazuje pusta komórka)

- Komórki są oddzielone spacją

Istnieje określony sposób tworzenia puzzli Sudoku; Nie można powtórzyć liczby w kolumnie lub wierszu. Klaster HDInsight poprawnie jest tworzona jest przykładem. On znajduje się w **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta** i zawiera następujące elementy:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

> [AZURE.NOTE] `2.2.4.9-1` Część ścieżki może zmienić aktualizacje zostały wprowadzone do klastrów HDInsight.

Aby uruchomić to za pośrednictwem przykład Sudoku, użyj następującego polecenia:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/2.2.9.1-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta

Wyniki powinna wyglądać podobnie do następującej:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi-"></a>PI (π)

Przykładowe pi używa statystyczne (quasi-Monte Carlo) metody, aby oszacować wartość liczby pi. Punkty losowo umieścić wewnątrz jednostka kwadratowa również się mieścić w okręgu wpisane w tym kwadracie przy prawdopodobieństwie równej powierzchni koła, pi/4. Od wartości 4R, gdzie R jest liczba punktów, które znajdują się wewnątrz okręgu do całkowitej liczbie punktów, które znajdują się w kwadrat współczynnika można oszacować wartość liczby pi. Im więcej przykładowych punkty używane jest lepiej oszacować.

Mapowanie, aby ten przykład generuje liczba punktów losowo umieszczona w kwadracie jednostki i następnie oblicza liczbę punktów, które znajdują się wewnątrz okręgu.

Następnie reduktorem sumuje punktów liczony według mappers i szacuje wartość liczby pi z 4R formuły, gdzie R jest stosunek liczby punktów zliczane okręgu do całkowitej liczbie punktów, które znajdują się w kwadrat.

Użyj następującego polecenia, aby uruchomić ten przykład. To jest używane 16 mapy z 10 000 000 próbki oszacowanie wartość liczby pi:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000

Wartość zwracana przez to powinna być podobna do **3.14159155000000000000**. Odwołania są 3.1415926535 10 pierwszych miejsc dziesiętnych liczby pi.

##<a name="10gb-greysort"></a>10GB Greysort

GraySort jest sortowanie testowych, którego metryki jest kurs sortowania (TB i minuty), który uzyskuje się podczas sortowania bardzo dużych ilości danych, zwykle 100TB minimalne.

W przykładzie użyto niewielkie 10GB danych, tak aby mogą być uruchamiane stosunkowo szybko. Używa aplikacji MapReduce opracowane przez Owen O'Malley oraz organizacji i Arun Murthy, które zdobyły rocznej testu sortowanie terabajtów ogólnego przeznaczenia ("daytona") w 2009 o częstotliwości 0.578 TB/min (100 TB w 173 minut). Aby uzyskać więcej informacji na to i innych stawek sortowania odwiedź witrynę [Sortbenchmark](http://sortbenchmark.org/) .

W tym przykładzie używa trzech zestawów MapReduce programów:

- **TeraGen**: program MapReduce generujący wierszy danych do posortowania

- **TeraSort**: próbki danych wejściowych i używa MapReduce, aby posortować dane w kolejności sumy

    TeraSort jest sortowanie standardowej funkcji MapReduce, z wyjątkiem niestandardowe partitioner, korzystającego z sortowaną listę klawiszy N-1 próbki, definiujące klucza zakres dla każdego zmniejszanie. W szczególności, wszystkie klucze takie przykładowe [i-1] < = klucz < próbki [i] są wysyłane w celu zmniejszenia i. To gwarancje, że wydajność zmniejszyć i są mniejsze niż dane wyjściowe zmniejszyć i + 1.

- **TeraValidate**: program MapReduce sprawdza, czy dane wyjściowe globalnie są sortowane

    Tworzy jedną mapę na plik w katalogu dane wyjściowe, a każda mapa sprawia, że każdy klucz jest mniejsza niż lub równa poprzedniego slajdu. Funkcja mapy generuje również rekordy kluczy imię i nazwisko każdego pliku, a funkcja zmniejszanie sprawia, że pierwszy klawisz pliku jest większa niż klucz ostatniego i-1 pliku. Problemy są zgłoszone jako wynik zmniejszanie przy użyciu klawiszy, których kolejność.

Korzystać z następujących czynności, aby wygenerować danych, sortowanie i sprawdzanie poprawności danych wyjściowych:

1. Generowanie 10GB danych, który będzie przechowywana w klastrze HDInsight domyślne miejsce do magazynowania przy **wasbs: / / / przykład /-10GB — sortowanie wprowadzania danych**:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input

    `-Dmapred.map.tasks` Zawiera informację Hadoop, ile zadań mapy do używania dla tego zadania. Ostateczne dwoma parametrami poinstruuj zadania, aby utworzyć 10GB warte danych i przechowywanie go w **wasbs: / / / przykład /-10GB — sortowanie wprowadzania danych**.

2. Aby posortować dane, użyj następującego polecenia:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output

    `-Dmapred.reduce.tasks` Hadoop wskazuje, ile zmniejszyć zadania dla zadania. Ostateczne dwoma parametrami są tylko lokalizację dane wejściowe i wyjściowe.

3. Sprawdzanie poprawności danych generowanych przez sortowanie należy wykonać następujące kroki:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate

##<a name="next-steps"></a>Następne kroki ##

W tym artykule przedstawiono sposób uruchomić przykłady dołączone do klastrów systemem Linux HDInsight. Samouczki dotyczące świnka, gałęzi i MapReduce za pomocą usługi HDInsight zobacz następujące tematy:

* [Używanie świnka z Hadoop na HDInsight][hdinsight-use-pig]
* [Gałąź za pomocą Hadoop na HDInsight][hdinsight-use-hive]
* [MapReduce za pomocą Hadoop na HDInsight] [hdinsight-use-mapreduce]



[hdinsight-errors]: hdinsight-debug-jobs.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
