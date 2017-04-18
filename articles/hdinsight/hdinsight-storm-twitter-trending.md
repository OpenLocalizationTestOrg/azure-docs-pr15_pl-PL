<properties
   pageTitle="Serwisu Twitter popularnych tematy z Burza Apache na HDInsight | Microsoft Azure"
   description="Dowiedz się, jak używać Trident do tworzenia topologię Apache burza, która określa popularnych tematy w serwisie Twitter, hashtags."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Określanie Twitter popularnych tematy z Burza Apache na HDInsight

Dowiedz się, jak używać Trident do tworzenia topologię burza, która określa popularnych tematów (tagi mieszania) w serwisie Twitter.

Trident jest abstrakcji wysokiego poziomu, która udostępnia narzędzia, takie jak sprzężenia, agregacji grupowania, funkcji i filtry. Ponadto Trident dodaje pierwotnych wykonując stanowe, przyrostowe przetwarzania. W tym przykładzie pokazano, jak można tworzyć przy użyciu niestandardowej dziobek, funkcja i kilka wbudowanych funkcji dostarczony przez Trident topologii.

> [AZURE.NOTE] W tym przykładzie intensywnie jest oparty na przykład [Burza Trident](https://github.com/jalonsoramos/trident-storm) przez Juan Alonso.

##<a name="requirements"></a>Wymagania dotyczące

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java i JDK 1.7</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Środowiska maven</a>

* <a href="http://git-scm.com/" target="_blank">Cyfra</a>

* Konta dewelopera Twitter

##<a name="download-the-project"></a>Pobierz projektu

Poniższy kod umożliwia klonowanie lokalnie projektu.

    git clone https://github.com/Blackmist/TwitterTrending

##<a name="topology"></a>Topologia

Topologia w tym przykładzie jest w następujący sposób:

![Topologia](./media/hdinsight-storm-twitter-trending/trident.png)

> [AZURE.NOTE] To jest widok uproszczony topologii. Wielu wystąpień składników będzie rozkładany węzły w klastrze.

Kod Trident, który wykonuje topologii jest następująca:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Ten kod wykonuje następujące czynności:

1. Umożliwia utworzenie nowego strumienia z dziobek. Dziobek pobiera tweety z Twitter, a ich filtry dla określonych słów kluczowych (miłości, muzyka i kawy w tym przykładzie).

2. HashtagExtractor, funkcji niestandardowej, jest używany do wyodrębniania znaczniki mieszania każdego tweet. Te są wysyłane do strumienia.

3. Strumień jest pogrupowane według znacznika mieszania i przekazywane do agregator. Ten agregator tworzy liczba ile razy wystąpił każdego znacznika skrótu. Te dane są zachowywane w pamięci. Na koniec nowego strumienia jest emisji zawierającego znacznik mieszania oraz liczbę elementów.

4. Ponieważ opiszemy tylko w najpopularniejszych znaczniki skrótu dla danej partii tweety, zestawu **FirstN** jest stosowana zwraca tylko 10 najwyższych wartości, oparte na polu Statystyka.

> [AZURE.NOTE] Oprócz dziobek i HashtagExtractor użyto wbudowanej funkcji Trident.
>
> Uzyskać informacji na temat wbudowanych operacji zobacz <a href="https://storm.apache.org/apidocs/storm/trident/operation/builtin/package-summary.html" target="_blank">pakiet storm.trident.operation.builtin</a>.
>
> Stan Trident implementacji niż MemoryMapState zobacz następujące artykuły:
>
> * <a href="https://github.com/fhussonnois/storm-trident-elasticsearch" target="_blank">Wyszukiwanie elastyczne Trident Burza</a>
>
> * <a href="https://github.com/kstyrc/trident-redis" target="_blank">Trident redis</a>

###<a name="the-spout"></a>Dziobek

Dziobek, **TwitterSpout**używa <a href="http://twitter4j.org/en/" target="_blank">Twitter4j</a> , aby pobrać tweety z Twitter. Filtr jest tworzony (miłości, muzyka i kawy w tym przykładzie), i tweety przychodzących (stan), które są zgodne z filtr są przechowywane w połączonych kolejce blokowania. (Aby uzyskać więcej informacji, zobacz <a href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedBlockingQueue.html" target="_blank">Klasy LinkedBlockingQueue</a>). Na koniec elementy są pobierane wyłączanie kolejki i emisji do topologii.

###<a name="the-hashtagextractor"></a>HashtagExtractor

Aby wyodrębnić znaczniki skrótu, <a href="http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--" target="_blank">getHashtagEntities</a> jest używana do pobierania wszystkie znaczniki skrótu, które są zawarte w serwisie Twitter. Następnie są one wysyłane do strumienia.

##<a name="enable-twitter"></a>Włączanie Twitter

Aby zarejestrować nowej aplikacji Twitter i uzyskać dla klientów indywidualnych i access token informacje potrzebne do odczyt Twitter, wykonaj następujące czynności:

1. Przejdź do <a href="https://apps.twitter.com" target="_blank">Serwisu Twitter aplikacji</a> i kliknij przycisk **Utwórz nową aplikację** . Podczas wypełniania w formularzu, pozostaw puste pole **Adresu zwrotnego** .

2. Po utworzeniu aplikacji, kliknij kartę **klawiszy i tokeny dostępu** .

3. Kopiowanie informacji **Klucz** i **Tajny dla klientów indywidualnych** .

4. U dołu strony wybierz pozycję **Utwórz moje token dostępu** , jeśli istnieje nie tokenów. Po utworzeniu tokeny skopiować informacje z **Token dostępu** i **Hasło Token dostępu** .

5. W programie project **TwitterSpoutTopology** , które wcześniej sklonowany Otwórz plik **resources/twitter4j.properties** , Dodaj informacje zebrane w poprzednich krokach, a następnie zapisz ten plik.

##<a name="build-the-topology"></a>Tworzenie topologii

Poniższy kod umożliwia tworzenie projektu:

        cd [directoryname]
        mvn compile

##<a name="test-the-topology"></a>Testowanie topologii

Aby przetestować topologii lokalnie, użyj następującego polecenia:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Po uruchomieniu topologii powinny być widoczne informacje debugowania skrótu znaczniki i zlicza emitowanym przez topologii. Wynik powinna wyglądać podobnie do następującej:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

##<a name="next-steps"></a>Następne kroki

Teraz, gdy przetestowaniu topologię lokalnie, Dowiedz się, jak wdrożyć topologii: [rozmieszczanie i zarządzanie nimi topologii Burza Apache na HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Można również w następujących tematach burzy:

* [Można opracowywać topologii Java dla Burza na HDInsight przy użyciu środowiska Maven](hdinsight-storm-develop-java-topology.md)

* [Można opracowywać C# topologii dla Burza na HDInsight przy użyciu programu Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Aby uzyskać więcej przykładów Burza HDinsight:

* [Przykład topologii dla Burza na HDInsight](hdinsight-storm-example-topology.md)
