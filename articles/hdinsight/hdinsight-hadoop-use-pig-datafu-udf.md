<properties
pageTitle="DataFu za pomocą świnka na HDInsight"
description="DataFu to zbiór bibliotek do użytku z Hadoop. Dowiedz się, jak można używać DataFu z świnka na klaster HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="use-datafu-with-pig-on-hdinsight"></a>DataFu za pomocą świnka na HDInsight

DataFu to zbiór bibliotek Otwórz źródło do użytku z Hadoop. W tym dokumencie dowiesz się, jak za pomocą DataFu w klastrze HDInsight i jak używać funkcji zdefiniowane przez użytkownika DataFu (UDF) z świnka.

##<a name="prerequisites"></a>Wymagania wstępne

* Subskrypcję usługi Azure.

* Klaster Azure HDInsight (Linux lub systemem Windows)

* Podstawowe znajomości [przy użyciu świnka na HDInsight](hdinsight-use-pig.md)

##<a name="install-datafu-on-linux-based-hdinsight"></a>Instalowanie DataFu na podstawie Linux HDInsight

> [AZURE.NOTE] DataFu jest zainstalowany w wersji systemem Linux klastrów 3.3 lub nowszy, a następnie na klastrów opartych na systemie Windows. Go nie jest zainstalowany na podstawie Linux klastrów wcześniejszych niż 3.3.
>
> Jeśli korzystasz z wersji systemem Linux klaster 3.3 lub nowszej lub klastrze opartych na systemie Windows, możesz pominąć tę sekcję.

DataFu można pobrać i zainstalować z repozytorium środowiska Maven. Aby dodać DataFu klaster usługi HDInsight, wykonaj następujące czynności:

1. Nawiązać połączenie przy użyciu SSH klaster HDInsight systemem Linux. Aby uzyskać więcej informacji na temat korzystania z usługi HDInsight SSH zobacz jeden z następujących dokumentów:

    * [Używanie SSH z systemem Linux Hadoop na HDInsight z Linux, OS X i Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Używanie SSH z systemem Linux Hadoop na HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-unix.md)
    
2. Użyj następującego polecenia, aby pobrać plik słoik DataFu za pomocą narzędzia wget lub skopiuj i Wklej łącze w przeglądarce, aby rozpocząć pobieranie.

        wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar

3. Następnie przekazać plik do domyślnego miejsca do magazynowania dla klaster HDInsight. Plik jest dostępny dla wszystkich węzłów w klastrze, a plik będzie wyświetlane w przestrzeni dyskowej, nawet jeśli usunięcie i ponowne klaster.

        hdfs dfs -put datafu-1.2.0.jar /example/jars
    
    > [AZURE.NOTE] Powyższy przykład przechowuje słoik w `wasbs:///example/jars` ponieważ ten katalog już istnieje w magazynie klaster. Możesz użyć dowolnej lokalizacji, w którym chcesz HDInsight klaster ilość miejsca do magazynowania.

##<a name="use-datafu-with-pig"></a>Używanie DataFu z świnka

Czynności opisane w tej sekcji przyjęto założenie, zaznajomieni z za pomocą świnka na HDInsight i zapewniają tylko łaciński świnka instrukcji, nie instrukcji dotyczących używania ich z klastrem. Aby uzyskać więcej informacji na temat korzystania z świnka z usługi HDInsight zobacz [Używanie świnka z usługi HDInsight](hdinsight-use-pig.md).

> [AZURE.IMPORTANT] Gdy używasz DataFu z świnka w klastrze systemem Linux HDInsight, należy najpierw zarejestrować plik słoik za pomocą następujących instrukcji łaciński świnka:
>
> ```register wasbs:///example/jars/datafu-1.2.0.jar```
>
> DataFu jest zarejestrowany domyślnie klastrów HDInsight systemu Windows.

Zazwyczaj zdefiniujesz aliasu dla funkcji DataFu. Na przykład:

    DEFINE SHA datafu.pig.hash.SHA();
    
Definiuje alias nazwany `SHA` dla Agent kondycji systemu mieszania w funkcji. Następnie można następująco w świnka łacińskim wygenerować mieszania dla danych wejściowych. Na przykład poniższa zastępuje imiona i nazwiska w danych wejściowych wartość skrótu:

    raw = LOAD '/data/raw/' USING PigStorage(',') AS  
        (name:chararray, 
        int1:int, 
        int2:int,
        int3:int); 
    mask = FOREACH raw GENERATE SHA(name), int1, int2, int3; 
    DUMP mask;

Jeśli stosuje się z następującymi danymi wprowadzania:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5
    
Generuje następujący wynik:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

##<a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o DataFu lub świnka zobacz następujące dokumenty:

* [Przewodnik świnka DataFu Apache](http://datafu.incubator.apache.org/docs/datafu/guide.html).

* [Świnka korzystanie z usługi HDInsight](hdinsight-use-pig.md)
