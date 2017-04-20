<properties
   pageTitle="Za pomocą Hadoop świni SSH na automatyzację | Microsoft Azure"
   description="Dowiedz się, jak ustanowić połączenie z klastrem opartych na systemie Linux Hadoop z SSH, a następnie użyj polecenia świnia interakcyjne uruchamianie łaciński świnia instrukcji lub jako zadanie wsadowe."
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

#<a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Uruchamianie zadań świnia na opartych na systemie Linux klastra przy użyciu polecenia świń (SSH)

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

W tym dokumencie poprowadzi przez proces łączenia się z klastrem opartych na systemie Linux usługi HDInsight przy użyciu Secure Shell (SSH), uruchamiane interaktywnie, łaciński świnia instrukcji przy użyciu polecenia świni lub jako zadanie wsadowe.

Język programowania łaciński świnia pozwala opisać przekształcenia, które są stosowane do wprowadzania danych do wyprodukowania wymaganych towarów.

> [AZURE.NOTE] Jeśli są dobrze zaznajomieni z wykorzystaniem serwerów opartych na systemie Linux Hadoop, ale pojawią się w HDInsight, zobacz [Wskazówki HDInsight opartych na systemie Linux](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Wymagania wstępne

Aby wykonać kroki opisane w tym artykule, będą potrzebne.

* Klastrów opartych na systemie Linux HDInsight (Hadoop na HDInsight).

* Klient SSH. Linux, Unix i Macintosh powinny pochodzić z klientem SSH. Użytkownicy systemu Windows muszą pobrać klienta, takich jak [Kit](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Połączyć się z SSH

Podłącz do w pełni kwalifikowaną nazwę domeny (FQDN) klastra HDInsight przy użyciu polecenia SSH. Nazwa FQDN będzie nazwa nadana klastra, a następnie **. azurehdinsight.net**. Na przykład następujące łączyłaby się do klastra o nazwie **myhdinsight**.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Jeśli podałeś klucza certyfikatu do uwierzytelniania SSH** , podczas tworzenia automatyzację, należy określić lokalizację klucza prywatnego w systemie klienta.

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Jeśli podane hasło do uwierzytelniania SSH** , podczas tworzenia automatyzację, należy podać to hasło, po wyświetleniu monitu.

Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz [Użyj SSH z systemem Linux Hadoop na HDInsight z Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kit (klientów opartych na systemie Windows)

System Windows nie udostępnia wbudowany klient SSH. Zalecane jest użycie **Kit**, który można pobrać z [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Aby uzyskać więcej informacji dotyczących używania Kit Zobacz [Korzystanie SSH z Hadoop na HDInsight z systemu Windows opartych na systemie Linux ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="pig"></a>Użyj polecenia trzody chlewnej

2. Po połączeniu, uruchom świnia interfejsu wiersza polecenia (CLI) przy użyciu następującego polecenia.

        pig

    Po chwili, powinien zostać wyświetlony `grunt>` wiersza.

3. Wprowadź następującą instrukcję.

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    To polecenie ładuje zawartość pliku sample.log do DZIENNIKÓW. Za pomocą następujących można wyświetlić zawartość pliku.

        DUMP LOGS;

4. Następnie przekształcać dane, stosując wyrażenia regularnego, aby wyodrębnić tylko poziom rejestrowania z każdego rekordu za pomocą następujących.

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    **ZRZUĆ** służy do wyświetlania danych po transformacji. W takim przypadku należy użyć `DUMP LEVELS;`.

5. Kontynuować stosowanie przekształceń za pomocą następujących instrukcji. Użyj `DUMP` do wyświetlania wyniku przekształcenia po wykonaniu każdego kroku.

    <table>
    <tr>
    <th>Instrukcja</th><th>Co robi</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = poziom filtru przez LOGLEVEL nie jest równa null;</td><td>Usuwa wiersze zawierające wartość null dla poziomu dziennika i przechowuje wyniki w FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = FILTEREDLEVELS grupy przez LOGLEVEL;</td><td>Grupuje wiersze według poziomu dziennika i przechowuje wyniki w GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>CZĘSTOTLIWOŚCI = foreach GROUPEDLEVELS Generowanie grupy jako LOGLEVEL COUNT (FILTEREDLEVELS. LOGLEVEL) jak POLICZYĆ;</td><td>Tworzy nowy zestaw danych, który zawiera każdy unikatowy dziennik wartość poziomu i ile razy występuje. To jest przechowywane w częstotliwości.</td>
    </tr>
    <tr>
    <td>WYNIK = zamówienia częstotliwości przez licznik desc;</td><td>Zamówienia poziomy rejestrowania count (malejąco) i są przechowywane w wyniku.</td>
    </tr>
    </table>

6. Można także zapisać wyniki przekształcania za pomocą `STORE` instrukcji. Na przykład, następujące zapisuje `RESULT` do katalogu **/example/data/pigout** na domyślnego kontenera magazynu dla klastra.

        STORE RESULT into 'wasbs:///example/data/pigout';

    > [AZURE.NOTE] Dane są przechowywane w katalogu określonym w plikach o nazwach **nnnnn części**. Jeśli katalog już istnieje, otrzymasz komunikat o błędzie.

7. Aby zamknąć wiersz grunt, należy wprowadzić następującą instrukcję.

        QUIT;

###<a name="pig-latin-batch-files"></a>Pliki wsadowe świnia łaciński

Polecenie świnia umożliwia również uruchomić łaciński świnia zawarte w pliku.

3. Po wyjściu z wiersza grunt, następujące polecenie do potoku STDIN do pliku o nazwie **pigbatch.pig**. Ten plik zostanie utworzony w katalogu macierzystego dla konta, na którym użytkownik jest zalogowany do sesji SSH.

        cat > ~/pigbatch.pig

4. Wpisz lub wklej następujące wiersze, a następnie użyj klawiszy Ctrl + D, po zakończeniu.

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Uruchom plik **pigbatch.pig** za pomocą polecenia świń należy wykonać następujące kroki.

        pig ~/pigbatch.pig

    Po zakończeniu pracy zadania wsadowego powinien pojawić następujące dane wyjściowe, która powinna być taka sama, jak w czasie korzystania z `DUMP RESULT;` w poprzednich krokach.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Podsumowanie

Jak widać, polecenie świnia umożliwia interakcyjne uruchomienie operacji MapReduce za pomocą trzody chlewnej Łacińskiej, a także uruchomić sprawozdania są przechowywane w pliku wsadowym.

##<a id="nextsteps"></a>Następne kroki

Ogólne informacje na temat przesyłania strumieniowego w.

* [Należy użyć wieprzowych z Hadoop na HDInsight](hdinsight-use-pig.md)

Aby uzyskać informacje na temat innych sposobów można pracować z Hadoop na HDInsight.

* [Gałąź rejestru za pomocą Hadoop na HDInsight](hdinsight-use-hive.md)

* [Należy użyć MapReduce z Hadoop na HDInsight](hdinsight-use-mapreduce.md)
