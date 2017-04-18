<properties
    pageTitle="Generowanie zalecenia przy użyciu Mahout i systemem Linux HDInsight | Microsoft Azure"
    description="Dowiedz się, jak za pomocą komputera Apache Mahout nauki biblioteki Generuj recommendations filmu z systemem Linux HDInsight (Hadoop)."
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
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>Generuj recommendations filmu przy użyciu Apache Mahout z systemem Linux Hadoop w HDInsight

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Dowiedz się, jak za pomocą komputera [Apache Mahout](http://mahout.apache.org) nauki bibliotece przy użyciu Azure HDInsight Generuj recommendations filmu.

Mahout jest [nauki machine] [ ml] biblioteki Apache Hadoop. Mahout zawiera algorytmów przetwarzania danych, takich jak filtrowanie klasyfikacji, a klaster. W tym artykule zostanie użyta aparat rekomendacji do wygenerowania zalecenia dotyczące filmu, które są oparte na filmy, które miały znajomych.

> [AZURE.NOTE] Kroki opisane w tym dokumencie wymagają Hadoop systemem Linux w klastrze HDInsight. Uzyskać informacji na temat korzystania z Mahout z klastrem bazującym na systemie Windows zobacz [Generuj recommendations filmu przy użyciu Mahout Apache z systemem Windows Hadoop w HDInsight](hdinsight-mahout.md)

##<a name="prerequisites"></a>Wymagania wstępne

* Systemem Linux Hadoop w klastrze HDInsight. Aby dowiedzieć się, jak utworzyć jedną zobacz [rozpocząć korzystanie z systemem Linux Hadoop w HDInsight][getstarted]

##<a name="mahout-versioning"></a>Przechowywanie wersji mahout

Aby uzyskać więcej informacji o wersji Mahout dostępny w pakiecie klaster HDInsight zobacz [HDInsight wersji i składniki Hadoop](hdinsight-component-versioning.md).

> [AZURE.WARNING] Gdy użytkownik może przekazywać do klastrów HDInsight innej wersji programu Mahout tylko składniki dostarczony z klastrem HDInsight są w pełni obsługiwane i Microsoft Support pomogą izolowanie i rozwiązywanie problemów związanych z tych składników.
>
> Niestandardowe składniki otrzymają komercyjnego rozsądne pomocy technicznej, aby pomóc rozwiązać ten problem. Może to spowodować w rozwiązaniu problemu lub pytaniem nawiązanie dostępnych kanałów technologiami Otwórz źródło miejsce, w którym znajduje się głębokości specjalizacji tej technologii. Na przykład, istnieje wiele witryn społeczności, których można używać, takich jak: [forum w witrynie MSDN HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Projekty Apache mieć także witryn projektów na [http://apache.org](http://apache.org), na przykład: [Hadoop](http://hadoop.apache.org/), [iskrowy](http://spark.apache.org/).

##<a name="recommendations"></a>Opis zalecenia

Jedna z funkcji, które są udostępniane przez Mahout jest aparatem rekomendacji. Ten aparat wprowadzania danych w formacie `userID`, `itemId`, i `prefValue` (preferencji użytkowników dla elementu). Mahout można wykonywać analizy Współtworzenie wystąpienia w celu oznaczenia: _użytkowników, którzy mają preferencji dla elementu mieć także preferencji elementy skoroszytu_. Mahout określa użytkownikom preferencje podobnych elementów, które mogą być używane zaleceń.

Oto przykład bardzo proste, z zastosowaniem filmy:

* __Współtworzenie wystąpienia__: Joe, Alicja i Robert polubiły _słów_, _Ponownie ataki Empire_i _powrotu Jedi_. Mahout Określa, że użytkownicy, którzy również, takich jak dowolną spośród następujących filmy, takich jak pozostałe dwa.

* __Współtworzenie wystąpienia__: Roman i Alicja również łączone _Zagrożenie fantom_, _atakami klonów_i _zemsty Sith_. Mahout Określa, że użytkownicy, którzy również łączone poprzedniego filmy trzy, takich jak te trzy.

* __Podobieństwa zalecenie__: ponieważ Joe polubiły pierwsze trzy filmy, Mahout analizuje filmy tego innym użytkownikom podobne preferencje łączone, ale Joe nie ma obserwowane (polubiły klasyfikowane). W tym przypadku Mahout zaleca _Zagrożenie fantom_, _atakami klonów_i _zemsty Sith_.

###<a name="understanding-the-data"></a>Opis danych

Wygodne, [Poszukaj GroupLens] [ movielens] zawiera dane klasyfikacji w przypadku filmów w formacie, który jest zgodny z Mahout. Te dane są dostępne na w klastrze domyślne miejsce do magazynowania przy `/HdiSamples/HdiSamples/MahoutMovieData`.

Istnieją dwa pliki `moviedb.txt` (informacje na temat filmów,) i `user-ratings.txt`. Podczas analizy, zostanie użyty plik ratings.txt użytkownika, gdy moviedb.txt służą do zapewnienia informacji o tekst zrozumiałe dla użytkownika, wyświetlając wyniki analizy.

Danych znajdujących się w ratings.txt użytkownik ma strukturę z `userID`, `movieID`, `userRating`, i `timestamp`, który wskazuje nam jak wysoce każdego użytkownika ocenianie filmu. Oto przykład danych:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>Przeprowadzić analizę

Użyj następującego polecenia, aby uruchomić zadanie zalecenie:

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] To zadanie może potrwać kilka minut, a może wykonywanie wielu zadań MapReduce.

##<a name="view-the-output"></a>Wyświetlanie danych wyjściowych

1. Po zakończeniu zadania, użyj następującego polecenia, aby wyświetlić wynik wygenerowane:

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    Wynik będzie wyglądać następująco:

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    Pierwsza kolumna zawiera `userID`. Wartości zawarte w "[" i "]" są `movieId`:`recommendationScore`.

2. Za pomocą dane wyjściowe, wraz z moviedb.txt, aby wyświetlić więcej informacji przyjazne dla użytkownika. Najpierw należy skopiować pliki lokalnie przy użyciu następujących poleceń:

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    Spowoduje to skopiowanie danych wyjściowych w pliku o nazwie **recommendations.txt** w bieżącym katalogu, plików danych filmu.

3. Aby utworzyć nowy skrypt Python, który będzie wyglądać nazw filmu danych w wyniku zalecenia, użyj następującego polecenia:

        nano show_recommendations.py

    Gdy zostanie otwarty Edytor, jako zawartość pliku:

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    Naciśnij klawisze **CTRL + X**, **Y**i na końcu **Enter** zapisanie danych.

3. Aby wprowadzić plik wykonywalny, użyj następującego polecenia:

        chmod +x show_recommendations.py

4. Uruchom skrypt Python. Poniższy założono, że jesteś w katalogu, w którym zostały pobrane wszystkie pliki:

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    Będzie Przyjrzyj się od zaleceń podstawą 4 identyfikator użytkownika.

    * Plik **ratings.txt użytkownika** jest używana do pobierania filmów, które użytkownik ma ocenianie
    * Plik **moviedb.txt** jest używana do pobierania nazwy filmów
    * **Recommendations.txt** jest używana do pobierania zalecenia filmu dla tego użytkownika

    Dane wyjściowe tego polecenia będzie podobny do następującego:

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>Usuwanie tymczasowych danych

Zadania mahout zrobić Usuń dane utworzony podczas przetwarzania zadania. `--tempDir` Parametr jest określony w zadaniu przykład w celu wyodrębnienia tymczasowych plików do określonej ścieżki do usunięcia łatwe. Aby usunąć pliki tymczasowe, użyj następującego polecenia:

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] Jeśli chcesz ponownie uruchom polecenie, możesz również usunąć katalog wyjściowy. Usuwanie katalogu należy wykonać następujące kroki:
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>Następne kroki

Teraz, gdy znasz sposobu używania Mahout, poznawanie innych sposobów pracy z danymi w HDInsight:

* [Gałąź z usługi HDInsight](hdinsight-use-hive.md)
* [Świnka z usługi HDInsight](hdinsight-use-pig.md)
* [MapReduce z usługi HDInsight](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
