<properties
    pageTitle="Błąd braku pamięci (za mało pamięci) — ustawienia gałęzi | Microsoft Azure"
    description="Popraw błąd braku pamięci (za mało pamięci) z kwerendy gałęzi w Hadoop w HDInsight. Scenariusz klienta to zapytania przez wiele dużych tabel."
    keywords="Wylogowywanie się z ustawieniami gałęzi błąd, za mało pamięci, pamięci"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>Naprawianie błędu o pamięci (za mało pamięci) z ustawieniami pamięci gałąź Hadoop w Azure HDInsight

Jeden z typowych problemów nominalnej naszym klientom jest wyświetlany komunikat o błędzie z pamięci (za mało pamięci) podczas przy użyciu gałęzi. W tym artykule opisano scenariusz klienta oraz ustawienia gałęzi, których zaleca się, aby rozwiązać ten problem.

## <a name="scenario-hive-query-across-large-tables"></a>Scenariusz: Gałęzi kwerendy w dużych tabel

Klient uruchomienia kwerendy poniżej przy użyciu gałęzi.

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Niektóre drobne kwerendy:

* T1 jest to alias wielką tabelę Tabela1, która zawiera wiele typów kolumn ciąg.
* Inne tabele są że duży, ale masz dużą liczbę kolumn.
* Wszystkie tabele są dołączania do siebie, w niektórych przypadkach z wieloma kolumnami w tabeli TABLE1 i innych osób.

Gdy klient uruchomienia kwerendy przy użyciu gałęzi na MapReduce w węźle 24 klaster A3, kwerenda uruchomiono w około 26 minut. Klient zauważyć następujące ostrzeżenia uruchomienie kwerendy przy użyciu gałęzi na MapReduce:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Ponieważ kwerendy na koniec wykonywania w około 26 minut klienta ignorowane te ostrzeżenia i zamiast tego pracę skoncentrować się na jak zwiększyć to dalszych wyników kwerendy.

Klient konsultacji [zoptymalizować gałęzi kwerend dla Hadoop w HDInsight](hdinsight-hadoop-optimize-hive-query.md)i program aparat wykonania Tez. Po tej samej kwerendy była uruchamiana z ustawieniem Tez włączone kwerendy uruchomiono przez 15 minut, a następnie zwrócił następujący komunikat o błędzie:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

Klienta, następnie zadecydować o zastosowaniu większy zastanawiająca większy maszyn wirtualnych maszyn wirtualnych (to znaczy D12) wymaga więcej miejsca na stosu. Nawet wówczas wyświetlić błąd ciąg dalszy klienta. Klienta osiągnięto do zespołu HDInsight pomocy debugowania tego problemu.

## <a name="debug-the-out-of-memory-oom-error"></a>Debugowanie błędów z pamięci (za mało pamięci)

Nasze pomocy technicznej i grupy inżynierów razem znajdzie problemów powoduje błąd z pamięci (za mało pamięci) jest [znany problem opisany w Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306). Od opisu w JIRA:

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

Firma Microsoft potwierdzającego tego **hive.auto.convert.join.noconditionaltask** faktycznie została ustawiona na **wartość PRAWDA** , wyświetlając w obszarze plik gałęzi site.xml:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

Na podstawie ostrzeżenia i JIRA, naszych hipotezy was dołączanie Map był przyczyną błędu Java stosu spacji za mało pamięci. Dlatego firma Microsoft wykopanych szczegółowego do tego problemu.

Zgodnie z opisem w blogu [Ustawienia pamięci przędzy Hadoop HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), kiedy Tez aparat wykonania jest używane miejsca stosu rzeczywiście należy do kontenera Tez. Zobacz obraz poniżej opisem Tez pamięci kontener.

![Diagram pamięci kontenera tez: gałęzi poza błąd pamięci za mało pamięci](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


Jak sugeruje wpis w blogu, następujące ustawienia pamięci dwóch Definiowanie pamięci kontenera stosu: **hive.tez.container.size** i **hive.tez.java.opts**. Z naszych doświadczeń wyjątku za mało pamięci nie oznacza, że rozmiar kontener jest za mały. Oznacza to, że jest za mały rozmiar stosu Java (hive.tez.java.opts). Aby zawsze, gdy zostanie wyświetlony za mało pamięci, możesz spróbować zwiększyć **hive.tez.java.opts**. W razie potrzeby może być konieczne zwiększenie **hive.tez.container.size**. Ustawienie **java.opts** powinno być około 80% **container.size**.

> [AZURE.NOTE]  Ustawienie **hive.tez.java.opts** zawsze musi być mniejszy niż **hive.tez.container.size**.

Ponieważ maszynowego D12 ma 28GB pamięci RAM, firma Microsoft zdecydowała użyć rozmiaru kontenera równego 10GB (10240MB) i przypisz 80% java.opts. Co miało na konsoli gałęzi przy użyciu poniższych ustawień:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

W zależności od tych ustawień, kwerenda działa prawidłowo w obszarze dziesięć minut.

## <a name="conclusion-oom-errors-and-container-size"></a>Wniosek: Błędy za mało pamięci i rozmiaru kontenera

Widzisz błąd za mało pamięci, nie oznacza, że rozmiar kontener jest za mały. Zamiast tego należy skonfigurować ustawienia pamięci, dzięki czemu rozmiar stosu zostaje zwiększona i jest co najmniej 80% rozmiar pamięci kontener.
