<properties
   pageTitle="Obliczanie kontekstu opcje serwera R na HDInsight (wersja preview) | Microsoft Azure"
   description="Więcej informacji na temat różnych obliczeń kontekstu dostępnych opcji dla użytkowników z serwerem R dla HDInsight (wersja preview)"
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="jhubbard"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="10/18/2016"
   ms.author="jeffstok"
/>

# <a name="compute-context-options-for-r-server-on-hdinsight-preview"></a>Obliczanie kontekstu opcje serwera R na HDInsight (wersja preview)

Microsoft Server R w Azure HDInsight (wersja preview) zapewnia najnowszą możliwości oparte na R analiz. Używa danych, który jest przechowywany w HDFS w kontenerze konta magazyn [Obiektów Blob platformy Azure](../storage/storage-introduction.md "Magazyn obiektów Blob platformy Azure") lub lokalny system plików Linux. Ponieważ serwer R jest oparty na Otwórz źródło R, tworzone aplikacje oparte na R mogą korzystać z tych pakietów Otwórz źródło R 8000 +. Mogą one również korzystać z procedury w [ScaleR](http://www.revolutionanalytics.com/revolution-r-enterprise-scaler "ScaleR analizy obrotów"), pakiet analizy duży danych firmy Microsoft, która wchodzi w skład serwera R.  

Węzeł krawędzi klastrze Premium pozwala wygodnie Aby połączyć się z klastrem i uruchomić skrypty R. Węzeł krawędzi istnieje możliwość uruchomienia przez ScaleR parallelized funkcje rozłożone przez rdzenie serwera granicznego węzeł. Masz również odpowiednią opcję, aby uruchomić je w węzłach klaster przy użyciu osoby ScaleR Hadoop mapy zmniejszyć lub Spark obliczyć konteksty.

## <a name="compute-contexts-for-an-edge-node"></a>Obliczanie konteksty węzła krawędzi

Na ogół skrypt R uruchamianego na serwerze R w węźle krawędź jest uruchamiany w interpretera R w tym węźle. Wyjątkiem jest tych kroków, które Wywołaj ScaleR. Połączenia ScaleR uruchomić w środowisku obliczeń, która zależy od sposobu skonfigurowania kontekstu obliczeń ScaleR.  Po uruchomieniu skrypt R z węzła krawędzi możliwe wartości kontekstu obliczeń są lokalne sekwencyjne ("lokalne"), lokalne równolegle (localpar), zmniejszenie mapy i Spark.

Opcje "lokalne" i "localpar" różnią się tylko w sposób są wykonywane rxExec połączeń. Oba wykonywanie innych funkcji odbierania połączeń w sposób równoległych przez wszystkich dostępnych rdzenie chyba że inaczej przy użyciu opcji numCoresToUse ScaleR, np. rxOptions(numCoresToUse=6). Poniżej opisano różne opcje obliczeń w kontekście

| Obliczanie kontekstu  | Jak ustawić                      | Kontekst wykonania                                                                     |
|------------------|---------------------------------|---------------------------------------------------------------------------------------|
| Lokalne sekwencyjne | rxSetComputeContext('local')    | Parallelized wykonywanie wielu rdzenie serwera węzeł krawędzi, z wyjątkiem rxExec połączeń, które są uruchamiane pojedynczo |
| Lokalne równolegle   | rxSetComputeContext('localpar') | Parallelized wykonywanie wielu rdzenie serwera węzeł krawędzi                                 |
| Spark            | RxSpark()                       | Parallelized rozłożone wykonywania przy użyciu Spark w węzłach klaster HDI      |
| Zmniejszanie mapy       | RxHadoopMR()                    | Parallelized rozłożone wykonywania przy użyciu mapy zmniejszyć w węzłach klaster HDI |


Przy założeniu, że chcesz parallelized wykonanie na potrzeby wydajności, dostępne są trzy opcje. Którą opcję wybrać, zależy od rodzaj pracy analizy oraz rozmiar i położenie danych.

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Wskazówki dotyczące podejmowania w kontekście obliczeń

Obecnie jest nie formułę, która zawiera informację, której obliczyć kontekstu korzystać. Istnieją jednak pewne wytyczne, które pomagają wybrać odpowiednie opcje lub co najmniej ułatwiają zawęzić wybór przed uruchomieniem wzorca. Wytyczne należą:

1.  Lokalny system plików Linux jest większa niż HDFS.
2.  Jeśli dane są lokalne i znajduje się w XDF, są szybsze powtarzających się analizy.
3.  Zaleca się przesyłanie strumieniowe niewielkich ilości danych ze źródła danych tekst; w przypadku większej ilości danych, przekonwertować go XDF przed analizy.
4.  Ogólnych kopiowania lub strumieniowego przesyłania danych do węzła krawędź na potrzeby analizy staje się bezproblemowego zarządzania dla bardzo dużych ilości danych.
5.  Spark jest większa niż Mapy zmniejszyć do analizy w Hadoop.

Niektóre ogólne reguły przyjąć do zaznaczania kontekstu obliczeń mieć tych zasad, są następujące:

### <a name="local"></a>Lokalne

- Jeśli zakres danych do analizowania zmniejszając i nie wymaga oczekiwanego, jej strumienia bezpośrednio do procedury analizy i za pomocą "lokalną" lub "localpar".
- Jeśli zakres danych do analizowania jest małych i średnich i wymaga oczekiwanego, następnie skopiować go do lokalnego systemu plików, zaimportuj go do XDF i analizować je za pośrednictwem "lokalne" lub "localpar".

### <a name="hadoop-spark"></a>Hadoop Spark

- W przypadku dużej ilości danych, aby przeanalizować, następnie zaimportować je do XDF w HDFS (o ile miejsca do magazynowania jest problem dotyczący) i analizować je za pośrednictwem "Spark".

### <a name="hadoop-map-reduce"></a>Zmniejszanie Hadoop mapy

- Używanie tylko w przypadku wystąpienia uczestników problem z użyciem Spark obliczyć kontekstu, ponieważ zazwyczaj będą tempo.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Pomoc w tekście na rxSetComputeContext

Aby uzyskać dodatkowe informacje i przykłady konteksty obliczeń ScaleR Zobacz tekście pomoc w R metody rxSetComputeContext, na przykład:

    > ?rxSetComputeContext

Możesz również znaleźć "ScaleR Distributed przetwarzania instrukcji" który jest dostępny w bibliotece [MSDN serwera R](https://msdn.microsoft.com/library/mt674634.aspx "R serwera w witrynie MSDN") .


## <a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono sposób utworzyć nowy klaster HDInsight zawierającym R Server. Wiesz również podstawy za pomocą konsoli R z sesji SSH. Teraz można przeczytać następujące artykuły, aby odkryć inne sposoby pracy z serwerem R na HDInsight:

- [Omówienie serwera R Hadoop](hdinsight-hadoop-r-server-overview.md)
- [Rozpoczynanie pracy z serwerem R dla Hadoop](hdinsight-hadoop-r-server-get-started.md)
- [Dodawanie serwera RStudio do HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)
- [Azure Opcje miejsca do magazynowania dla serwera R na HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
