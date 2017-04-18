<properties
   pageTitle="Świnka Hadoop za pomocą pulpitu zdalnego w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak za pomocą polecenia świnka uruchamianie instrukcji łaciński świnka z połączenia pulpitu zdalnego z klastrem bazującym na systemie Windows Hadoop w HDInsight."
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

#<a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Uruchamianie zadania świnka z połączenia pulpitu zdalnego

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Niniejszy dokument zawiera instrukcje dla uruchamianie instrukcji łaciński świnka z połączenia pulpitu zdalnego z klastrem HDInsight systemu Windows za pomocą polecenia świnka. Łaciński świnka umożliwia tworzenie aplikacji MapReduce opisując przekształcenia danych zamiast mapowanie i zmniejszyć funkcje.

W tym dokumencie, Dowiedz się, jak

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać czynności opisane w tym artykule, będą potrzebne następujące elementy.

* Klaster HDInsight systemu Windows (Hadoop na HDInsight)

* Komputer kliencki z systemem Windows 10, Windows 8 lub Windows 7

##<a id="connect"></a>Łączenie się z pulpitu zdalnego

Włączanie pulpitu zdalnego dla klastrów HDInsight, a następnie postępując zgodnie z instrukcjami na [Nawiązywanie połączenia przy użyciu RDP klastrów HDInsight](hdinsight-administer-use-management-portal.md#rdp)się z nim połączyć.

##<a id="pig"></a>Za pomocą polecenia świnka

2. Po utworzeniu połączenia pulpitu zdalnego, należy rozpocząć **wiersza polecenia Hadoop** przy użyciu ikony na pulpicie.

2. Uruchom polecenie świnka należy wykonać następujące kroki:

        %pig_home%\bin\pig

    Zostanie wyświetlona z `grunt>` wiersza.

3. Wprowadź następującą instrukcję:

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    To polecenie ładuje zawartość pliku sample.log do pliku DZIENNIKÓW. Można wyświetlić zawartość pliku przy użyciu następującego polecenia:

        DUMP LOGS;

4. Przekształcanie danych przez zastosowanie wyrażenie, aby wyodrębnić poziom rejestrowania z każdego rekordu:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **ZRZUT** służy do wyświetlania danych po transformacji. W tym przypadku `DUMP LEVELS;`.

5. Kontynuuj stosowanie przekształcenia przy użyciu poniższych stwierdzeń. Używanie `DUMP` Aby wyświetlić wynik przekształcenia po wykonaniu każdej czynności.

    <table>
    <tr>
    <th>Instrukcja</th><th>Działanie</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = poziomy FILTRUJ według LOGLEVEL jest inna niż zero;</td><td>Usuwa wiersze, które zawierają wartość null w poziom rejestrowania i przechowuje wyniki w FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = FILTEREDLEVELS grupy przez LOGLEVEL;</td><td>Grupowanie wierszy według poziom rejestrowania i zapisuje wyniki w GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>CZĘSTOTLIWOŚCI = foreach GROUPEDLEVELS generowania grup jako LOGLEVEL, liczba (FILTEREDLEVELS. LOGLEVEL) podczas ZLICZANIA;</td><td>Tworzy nowy zestaw danych, który zawiera każdego dziennika wartość poziomu i ile razy występuje. Jest ona przechowywana w częstotliwości</td>
    </tr>
    <tr>
    <td>WYNIK = kolejności częstotliwości przez desc Statystyka;</td><td>Zamówień poziomy rejestrowania według liczby (malejąco), a są przechowywane w wynik</td>
    </tr>
    </table>

6. Możesz również zapisać wyniki przekształcania za pomocą `STORE` instrukcji. Na przykład następujące polecenie zapisuje `RESULT` do katalogu **/example/data/pigout** w kontenerze domyślnym miejsca do magazynowania dla klaster:

        STORE RESULT into 'wasbs:///example/data/pigout'

    > [AZURE.NOTE] Dane są przechowywane w określonym katalogu w plikach o nazwie **nnnnn części**. Jeśli katalog już istnieje, otrzymasz komunikat o błędzie.

7. Aby zakończyć grunt monit, wprowadź następującą instrukcję.

        QUIT;

###<a name="pig-latin-batch-files"></a>Pliki wsadowe łaciński świnka

Za pomocą polecenia świnka do uruchomienia łaciński świnka, która znajduje się w pliku.

3. Po zakończeniu działania wierszu grunt, otwórz **Notatnik** i utworzenie nowego pliku o nazwie **pigbatch.pig** w katalogu **PIG_HOME %** .

4. Wpisz lub wklej następujące wiersze do pliku **pigbatch.pig** , a następnie zapisz go:

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Uruchom plik **pigbatch.pig** za pomocą polecenia świnka należy wykonać następujące kroki.

        pig %PIG_HOME%\pigbatch.pig

    Po zakończeniu zadaniu powinien zostać wyświetlony następujący wynik, która powinna być taka sama, jak w przypadku, gdy używany `DUMP RESULT;` w poprzednich krokach:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Podsumowanie

Jak widać, polecenie świnka umożliwia interakcyjne uruchamianie operacji MapReduce lub wykonywanie zadań łaciński świnka, które są przechowywane w plik wsadowy.

##<a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje o świnka w HDInsight:

* [Używanie świnka z Hadoop na HDInsight](hdinsight-use-pig.md)

Aby uzyskać informacje o innych sposobach możesz pracować z Hadoop na HDInsight:

* [Gałąź za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

* [MapReduce za pomocą Hadoop na HDInsight](hdinsight-use-mapreduce.md)
