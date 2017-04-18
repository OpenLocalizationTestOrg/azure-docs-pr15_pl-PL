<properties
   pageTitle="Używanie składników Python w topologii Burza na HDinsight | Microsoft Azure"
   description="Dowiedz się, jak za pomocą składniki Python z Burza Apache na Azure HDInsight. Przedstawiono sposoby za pomocą składników Python z obu Java, podstawie i Clojure oparte Burza topologii."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Można opracowywać topologii Burza Apache za pomocą Python na HDInsight

Burza Apache obsługuje wiele języków, nawet umożliwia łączenie składniki w kilku językach w jednym topologii. W tym dokumencie dowiesz się, jak używać składniki Python w swojej Java i oparte na Clojure Burza topologii na HDInsight.

##<a name="prerequisites"></a>Wymagania wstępne

* Python 2.7 lub nowszy

* Java JDK 1.7 lub nowszy

* [Leiningen](http://leiningen.org/)

##<a name="storm-multi-language-support"></a>Obsługa wielu języków Burza

Burza została przeznaczona do pracy z składniki napisane przy użyciu dowolnego języka programowania, jednak wymaga to, że składniki zrozumieć, jak pracować z [Thrift definicję Burza](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift). W przypadku Python moduł jest dostępna w ramach projektu Burza Apache, która umożliwia łatwe łączenie się z burzy. Moduł ten można znaleźć w [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Ponieważ Burza Apache jest procesem Java uruchamianej na środowiska Java (maszyny wirtualnej Java), składników napisanych w innych językach są wykonywane jako podprocesów. Bitów Burza uruchomione w maszyny wirtualnej Java komunikuje się z następujących podprocesów przy użyciu JSON wiadomości przesyłane stdin-stdout. Więcej informacji na temat komunikacji między składnikami można znaleźć w dokumentacji [wielu lang Protocol (protokół)](https://storm.apache.org/documentation/Multilang-protocol.html) .

###<a name="the-storm-module"></a>Moduł Burza

Moduł Burza (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py), zapewnia bitów potrzebne do utworzenia składniki Python, które współpracują z burzy.

Dzięki temu czynności, takie jak `storm.emit` na emitowanie krotki, i `storm.logInfo` napisać do dzienników. Czy zaproszenie na tym pliku i opis, co zapewnia

##<a name="challenges"></a>Wyzwania

Korzystanie z modułu __storm.py__ , możesz utworzyć spouts Python, które używają danych i tekst "Śruby", które przetwarzanie danych, jednak ogólnej definicji Burza topologii druty prędkość komunikacji między składnikami nadal są zapisywane przy użyciu języka Java lub Clojure. Ponadto jeśli korzystasz z języka Java, można także tworzyć składników Java, które działają jako interfejs składniki Python.

Ponadto ponieważ klastrów Burza uruchamiane w sposób rozłożone, musi się upewnić, że wszystkie wymagane przez składniki Python moduły są dostępne we wszystkich węzłach pracownika w klastrze. Burza nie umożliwia dowolną łatwo to zrobić dla wielu lang zasobów — albo trzeba uwzględnić wszystkie zależności jako część pliku słoik topologii, lub ręcznie zainstalować zależności na każdym węźle pracownika w klastrze.

###<a name="java-vs-clojure-topology-definition"></a>Definicja topologii Java a Clojure

Z dwóch metod określania topologii Clojure jest najprostszym/cleanest można bezpośrednio składniki python referenc w definicji topologii. Definicje opartego na języku Java topologii należy zdefiniować składniki języka Java, których uchwyt poleceń, takich jak deklarowanie pola krotki zwrócone przez składniki Python.

Obu metod opisanych w tym dokumencie, wraz z przykładowe projekty.

##<a name="python-components-with-a-java-topology"></a>Składniki Python w topologii Java

> [AZURE.NOTE] W tym przykładzie jest dostępna w [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) w katalogu __JavaTopology__ . To jest projekt środowiska Maven podstawie. Jeśli użytkownik nie zna środowiska Maven, zobacz [opartego na języku Java można opracowywać topologii z Burza Apache na HDInsight](hdinsight-storm-develop-java-topology.md) Aby uzyskać więcej informacji na temat tworzenia projektu środowiska Maven topologii Burza.

Topologii opartego na języku Java Python (lub inne składniki języka maszyny wirtualnej Java) początkowo wydaje się za pomocą składników Java; Jednak jeśli Szukaj w każdej z języka Java spouts-tekst "Śruby" pojawi się kod podobny do następującego:

    public SplitBolt() {
        super("python", "countbolt.py");
    }

Jest to miejsce, w którym Java wywołuje Python i uruchamia skrypt, który zawiera logikę błyskawicy rzeczywiste. Java spouts śruby (na przykład) po prostu deklarować pola krotki, które będzie wysyłane przez składnik Python źródłowych.

Rzeczywiste Python pliki są przechowywane w `/multilang/resources` katalogu, w tym przykładzie. `/multilang` Katalogu odwołuje się do __pom.xml__:

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>${basedir}-multilang</directory>
    </resource>
</resources>

Ta opcja uwzględnia wszystkie pliki w `/multilang` folder w słoju, która zostanie utworzona na podstawie tego projektu.

> [AZURE.IMPORTANT] Należy zauważyć, że tylko Określa `/multilang` katalogu, a nie `/multilang/resources`. Burza oczekuje-maszyny wirtualnej Java zasobów w `resources` katalogu, aby go wewnętrznie już tablicą. Umieszczanie składniki w tym folderze umożliwia odwołanie tylko według nazwy w kodzie Java. Na przykład `super("python", "countbolt.py");`. Innym sposobem go traktować jest, że Burza widzi `resources` katalog główny (/) podczas uzyskiwania dostępu do wielu lang zasobów.
>
> Dla tego projektu przykład `storm.py` moduł znajduje się w `/multilang/resources` katalogu.

###<a name="build-and-run-the-project"></a>Tworzenie i uruchamianie projektu

Aby uruchomić ten projekt lokalnie, użyj następującego polecenia środowiska Maven do tworzenia i uruchamiania w trybie lokalnym:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

Użyj klawisza ctrl + c, aby zakończyć proces.

Aby wdrożyć projektu klaster HDInsight uruchomiony Burza Apache, wykonaj następujące czynności:

1. Tworzenie słoik uber:

        mvn package

    Spowoduje to utworzenie pliku o nazwie __WordCount — 1.0 SNAPSHOT.jar__ w `/target` katalogu dla tego projektu.

2. Przekaż plik słoik do klastrów Hadoop przy użyciu jednej z następujących metod:

    * Dla klastrów HDInsight __systemem Linux__ : używanie `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar` skopiuj plik słoik z klastrem, zamieniając nazwę użytkownika SSH nazwę użytkownika i NAZWAKLASTRA o nazwie klaster HDInsight.

        Po zakończeniu przekazywania, połącz się z klastrem przy użyciu SSH i Rozpoczynanie korzystania z topologii`storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * W przypadku klastrów HDInsight __systemu Windows__ : łączenie do pulpitu nawigacyjnego burza, przechodząc do HTTPS://CLUSTERNAME.azurehdinsight.net/ w przeglądarce. Zamień NAZWAKLASTRA z nazwą klaster HDInsight i podać nazwę administratora i hasło po wyświetleniu monitu.

        Przy użyciu formularza, wykonaj następujące czynności:

        * __Plik JAR__: wybierz pozycję __Przeglądaj__, a następnie wybierz plik __WordCount-1.0-SNAPSHOT.jar__
        * __Nazwa klasy__: wprowadzanie`com.microsoft.example.WordCount`
        * __Dodatkowe parametrów__: wprowadzać przyjazną nazwę, takie jak `wordcount` do identyfikowania topologii

        Na koniec wybierz pozycję __Prześlij__ , aby rozpocząć topologii.

> [AZURE.NOTE] Po rozpoczęciu topologii Burza uruchamia do zatrzymania (zabitych). Aby zatrzymać topologia, użyj jednej `storm kill TOPOLOGYNAME` polecenia z wiersza polecenia (SSH sesji do klastrów Linux na przykład) lub za pomocą interfejsu użytkownika burza, wybierz topologię, a następnie wybierz przycisk __skasować__ .

##<a name="python-components-with-a-clojure-topology"></a>Składniki Python w topologii Clojure

> [AZURE.NOTE] W tym przykładzie jest dostępna w [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) w katalogu __ClojureTopology__ .

Ta topologia został utworzony przy użyciu [Leiningen](http://leiningen.org) , aby [utworzyć nowy projekt Clojure](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project). Po wykonaniu tej wprowadzono następujące zmiany w projekcie scaffolded:

* __Project.CLJ__: dodane zależności Burza i wykluczeń dla elementów, które mogą powodować problemy po wdrożeniu na serwerze HDInsight.
* __zasoby i zasoby__: Leiningen tworzy domyślnie `resources` katalogu, jednak pliki przechowywane w tym miejscu ma zostać dodane do głównego pliku słoik utworzone na podstawie tego projektu, a burza oczekuje plików w katalogu podrzędnego o nazwie `resources`. Aby katalog podrzędny zostało dodane i Python pliki są przechowywane w `resources/resources`. W czasie wykonywania to jest traktowana jako główny (/) do uzyskiwania dostępu do Python składniki.
* __src/WordCount/Core.CLJ__: ten plik zawiera definicję topologii i odwołuje się z pliku __project.clj__ . Aby uzyskać więcej informacji na temat korzystania z Clojure w celu zdefiniowania topologii Burza zobacz [Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html).

###<a name="build-and-run-the-project"></a>Tworzenie i uruchamianie projektu

__Do tworzenia i uruchamiania projektu lokalnie__, użyj następującego polecenia:

    lein clean, run

Aby zatrzymać topologii, za pomocą __Klawiszy Ctrl + C__.

__Do tworzenia uberjar i wdrażanie usługi HDInsight__, wykonaj następujące czynności:

1. Tworzenie uberjar zawierającej topologia i wymaganych zależności:

        lein uberjar

    Spowoduje to utworzenie nowego pliku o nazwie `wordcount-1.0-SNAPSHOT.jar` w `target\uberjar+uberjar` katalogu.
    
2. Wdrażać i uruchamiać topologii z klastrem HDInsight, użyj jednej z następujących metod:

    * __Systemem Linux HDInsight__
    
        1. Skopiuj plik za pomocą węzła głównego klaster HDInsight `scp`. Na przykład:
        
                scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
                
            Nazwa_użytkownika należy zastąpić użytkownika SSH klaster i NAZWAKLASTRA z nazwą klaster HDInsight.
            
        2. Po plik został skopiowany do klaster, należy używać SSH, aby połączyć się z klastrem i przesłać zadanie. Aby uzyskać informacje na temat korzystania z usługi HDInsight SSH zobacz jeden z następujących czynności:
        
            * [Używanie SSH z systemem Linux HDInsight z Linux, Unix lub systemu OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
            * [Używanie SSH z systemem Linux HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
            
        3. Po połączeniu, uruchom topologii należy wykonać następujące kroki:
        
                storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
    
    * __Usługa HDInsight systemu Windows__
    
        1. Podłącz do pulpitu nawigacyjnego burza, przechodząc do HTTPS://CLUSTERNAME.azurehdinsight.net/ w przeglądarce. Zamień NAZWAKLASTRA z nazwą klaster HDInsight i podać nazwę administratora i hasło po wyświetleniu monitu.

        2. Przy użyciu formularza, wykonaj następujące czynności:

            * __Plik JAR__: wybierz pozycję __Przeglądaj__, a następnie wybierz plik __wordcount-1.0-SNAPSHOT.jar__
            * __Nazwa klasy__: wprowadzanie`wordcount.core`
            * __Dodatkowe parametrów__: wprowadzać przyjazną nazwę, takie jak `wordcount` do identyfikowania topologii

            Na koniec wybierz pozycję __Prześlij__ , aby rozpocząć topologii.

> [AZURE.NOTE] Po rozpoczęciu topologii Burza uruchamia do zatrzymania (zabitych). Aby zatrzymać topologia, użyj jednej `storm kill TOPOLOGYNAME` polecenia z wiersza polecenia (SSH sesji z klastrem Linux) lub za pomocą interfejsu użytkownika burza, wybierz topologię, a następnie wybierz przycisk __skasować__ .

##<a name="next-steps"></a>Następne kroki

W tym dokumencie przedstawiono sposób używania składników Python z topologii Burza. Zobacz następujące dokumenty dla innych sposobów korzystania z usługi HDInsight Python:

* [Jak używać Python przesyłania strumieniowego MapReduce zadań](hdinsight-hadoop-streaming-python.md)
* [Jak używać Python użytkownika zdefiniowane funkcje (UDF) i gałęzi](hdinsight-python.md)
