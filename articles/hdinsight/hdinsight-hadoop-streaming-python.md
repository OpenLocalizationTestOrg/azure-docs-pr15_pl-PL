<properties
   pageTitle="Można opracowywać Python MapReduce zadania HDInsight | Microsoft Azure"
   description="Dowiedz się, jak utworzyć i uruchomić Python MapReduce zadań dotyczących klastrów HDInsight systemem Linux."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="develop-python-streaming-programs-for-hdinsight"></a>Można opracowywać Python streaming programów dla HDInsight

Hadoop zapewnia przesyłanie strumieniowe interfejsu API MapReduce umożliwiający pisanie mapy i zmniejszyć funkcje w językach innych niż Java. W tym artykule dowiesz się, jak używać Python do wykonywania operacji MapReduce.

> [AZURE.NOTE] Chociaż kod Python w tym dokumencie mogą być używane z klastrem HDInsight systemu Windows, kroki opisane w tym dokumencie są specyficzne dla klastrów opartych na systemie Linux.

Ten artykuł jest oparty na informacje i przykłady publikowane przez Michael Noll u [Zapisywanie programu MapReduce Hadoop w Python](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/).

##<a name="prerequisites"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy:

* Z systemem Linux Hadoop w klastrze HDInsight

* Edytor tekstów

    > [AZURE.IMPORTANT] Edytor tekstów, należy użyć wysuwu wiersza jako koniec wiersza. Jeśli korzysta z CRLF, spowoduje to błędy podczas wykonywania zadania MapReduce dotyczących klastrów HDInsight systemem Linux. Jeśli nie masz pewności, wykonaj krok opcjonalny w sekcji [MapReduce uruchomić](#run-mapreduce) , aby przekonwertować dowolne CRLF wysuwu wiersza.

* Dla klientów systemu Windows, Kit i PSCP. Te narzędzia są dostępne z poziomu <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">Strony pobierania Kit</a>.

##<a name="word-count"></a>Statystyka wyrazów

W tym przykładzie zostanie zaimplementować podstawowe wyrazów przy użyciu mapowania i reduktorem. Mapowanie dzieli zdania na pojedyncze wyrazy, a reduktorem agreguje wyrazów i zlicza da wynik.

Na poniższym schemacie zilustrowano, co się dzieje podczas mapy i zmniejszyć fazy.

![Ilustracja przedstawiająca mapę zmniejszanie](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##<a name="why-python"></a>Dlaczego Python?

Python jest ogólnego przeznaczenia, wysokiego poziomu język programowania, który pozwala express pojęć w mniejszej liczby wierszy kodu niż wiele innych języków. Obejmuje ostatnio stało się popularne z naukowcami danych jako język prototypami ponieważ jego interpretowany charakter, wpisując dynamiczne i eleganckie składnia wprowadź odpowiednie dla szybkiego opracowywania aplikacji.

Python jest zainstalowana na wszystkich klastrów HDInsight.

##<a name="streaming-mapreduce"></a>Przesyłanie strumieniowe MapReduce

Hadoop pozwala określić plik, który zawiera mapy i zredukować logiczny, który jest używany przez zadanie. Szczegółowe wymagania mapy i zredukować logiczny są:

* **Wprowadzania**: mapy i zredukować składniki musi odczytać danych wejściowych stdin.

* **Wynik**: mapy i zredukować składniki musi zapisać dane wyjściowe STDOUT.

* **Format danych**: dane zużyte i wyprodukowano muszą być parę klucz wartość oddzielone znakiem tabulacji.

Python można łatwo obsługiwać te wymagania przy użyciu modułu **dotyczącego** odczytywanie STDIN i **Drukowanie** do wydrukowania stdout. Pozostałe zadania jest po prostu formatowanie danych za pomocą karty (`\t`) znaków między klucz i wartość.

##<a name="create-the-mapper-and-reducer"></a>Tworzenie mapowania i reduktorem

Mapowanie i reduktorem to pliki tekstowe, w tym przypadku **mapper.py** i **reducer.py** (Aby wyczyścić, który wykonuje co). Można tworzyć przy użyciu dowolnego edytora.

###<a name="mapperpy"></a>Mapper.PY

Tworzenie nowego pliku o nazwie **mapper.py** i użyj następującego kodu jako zawartość:

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

Poświęć chwilę na odczytane przez kod mogą zrozumieć, jak działa.

###<a name="reducerpy"></a>Reducer.PY

Tworzenie nowego pliku o nazwie **reducer.py** i użyj następującego kodu jako zawartość:

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##<a name="upload-the-files"></a>Przekazywanie plików

Zarówno **mapper.py** , jak i **reducer.py** muszą być głowy węzeł klaster przed możemy uruchomić je. Najłatwiejszym sposobem ich przekazania jest za pomocą **połączenia** (**pscp** Jeśli używasz klienta systemu Windows).

W kliencie w tym samym katalogu jako **mapper.py** i **reducer.py**, użyj następującego polecenia. Zamień **nazwę użytkownika** użytkownik SSH i **NazwaKlastra** o nazwie klaster.

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

Spowoduje to skopiowanie plików z systemu lokalnego do węzła głównego.

> [AZURE.NOTE] Jeśli hasło jest używany do zabezpieczenia konta SSH, zostanie wyświetlony monit o podanie hasła. Jeśli został użyty klucz SSH, może być konieczne używanie `-i` parametru i ścieżkę do klucz prywatny, na przykład `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

##<a name="run-mapreduce"></a>Uruchamianie MapReduce

1. Połącz się z klastrem przy użyciu SSH:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Jeśli hasło jest używany do zabezpieczenia konta SSH, zostanie wyświetlony monit o podanie hasła. Jeśli został użyty klucz SSH, może być konieczne używanie `-i` parametru i ścieżkę do klucz prywatny, na przykład `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`.

2. (Opcjonalnie) Użycie edytora tekstu, który używa CRLF w wierszu podczas tworzenia plików mapper.py i reducer.py, lub nie wiesz, co kończy się na linii edytora używa, użyj następujących poleceń, aby przekonwertować wystąpień CRLF w mapper.py i reducer.py wysuwu wiersza.

        perl -pi -e 's/\r\n/\n/g' mappery.py
        perl -pi -e 's/\r\n/\n/g' reducer.py

2. Użyj następującego polecenia, aby uruchomić zadanie MapReduce.

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasbs:///example/data/gutenberg/davinci.txt -output wasbs:///example/wordcountout

    To polecenie zawiera następujące segmenty:

    * **hadoop streaming.jar**: używane podczas wykonywania operacji MapReduce przesyłanie strumieniowe. Przy użyciu kodu MapReduce zewnętrznego podane go interfejsów Hadoop.

    * **-pliki**: Hadoop informuje, określone pliki są wymagane dla tego zadania MapReduce i powinny zostać skopiowane do wszystkich węzłów pracownika.

    * **— mapowania**: zawiera informację Hadoop, który plik ma zostać użyte jako usługę mapowania.

    * **-reduktorem**: zawiera informację Hadoop, który plik ma zostać użyte jako reduktorem.

    * **-wprowadzania**: plik wejściowy, który firma Microsoft należy zliczyć wyrazy z.

    * **— wynik**: dane wyjściowe zostaną zapisane w katalogu.

        > [AZURE.NOTE] Ten katalog zostanie utworzony przy użyciu zadania.

Należy wyświetlić więcej **informacji o** instrukcji podczas uruchamiania zadania i na końcu Zobacz operację **mapy** i **zredukować** wyświetlane jako wartości procentowe.

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

Po zakończeniu otrzymasz stan informacji o zadaniu.

##<a name="view-the-output"></a>Wyświetlanie danych wyjściowych

Po zakończeniu zadania umożliwia wyświetlanie danych wyjściowych następujące polecenie:

    hdfs dfs -text /example/wordcountout/part-00000

To należy wyświetlić listę wyrazów i ile razy wyraz wystąpił. Oto przykładowe dane wyjściowe:

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##<a name="next-steps"></a>Następne kroki

Teraz, gdy znasz sposobu używania przesyłanie strumieniowe zadania MapRedcue z usługi HDInsight, Poznaj inne sposoby pracy z usługi HDInsight Azure za pomocą poniższych łączy.

* [Gałąź za pomocą usługi HDInsight](hdinsight-use-hive.md)
* [Świnka korzystanie z usługi HDInsight](hdinsight-use-pig.md)
* [MapReduce zadań za pomocą usługi HDInsight](hdinsight-use-mapreduce.md)
