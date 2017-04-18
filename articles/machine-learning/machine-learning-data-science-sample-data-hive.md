<properties
    pageTitle="Przykładowe dane w tabelach Azure HDInsight gałęzi | Microsoft Azure"
    description="W dół próbek danych w tabelach gałęzi Azure HDInsight (Hadopop)"
    services="machine-learning,hdinsight"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />

# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Przykładowe dane w tabelach gałęzi HDInsight platformy Azure

W tym artykule firma Microsoft opisano sposoby dół — przykładowe dane przechowywane w tabelach Azure HDInsight gałęzi korzystanie z kwerend gałęzi. Kwestie omawiane trzy popularly używanych przeprowadzania:

* Jednolity losowe pobieranie próbek
* Losowe pobieranie próbek według grup
* Stratyfikowana pobierania

**Dlaczego przykładowych danych?**
Jeśli zestaw danych, które mają być analizowanie jest duży, zazwyczaj jest dobrym pomysłem próby szczegółów danych, aby zmniejszyć go o rozmiarze mniejszym, ale przedstawiciel oraz łatwiejsze. Ułatwia to opis danych, badań i inżynierskie funkcji. Jego rolę w procesie nauki danych zespołu jest umożliwienie szybkie tworzenie prototypów funkcji przetwarzania danych i maszynowego uczenia modeli.

**Menu** poniższych łączy do tematów, w których opisano przykładowe dane z różnych środowiskach miejsca do magazynowania.

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

To zadanie pobierania jest krok w [Zespołu danych nauki proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="how-to-submit-hive-queries"></a>Sposób przesyłania kwerend gałęzi
Gałąź kwerendy można wysłać z konsoli wiersza polecenia Hadoop głowy węzeł klaster Hadoop. W tym celu należy zalogować się do węzła głównego klastrze Hadoop, otwórz konsolę wiersza polecenia Hadoop i przesyłać kwerendy gałęzi stamtąd. Aby uzyskać instrukcje dotyczące przesyłania kwerend gałęzi w konsoli wiersza polecenia Hadoop zobacz [jak przesyłanie kwerend gałęzi](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="uniform"></a>Jednolity losowe pobieranie próbek
Jednolitego losową przy próbkowaniu oznacza, że każdego wiersza w zestawie danych jest równa prawdopodobieństwo pobrania próbki. Można to zaimplementować przez dodanie rand() dodatkowych pól zestawu danych w "Wybierz" zapytanie wewnętrzne i zewnętrzne "Wybierz" zapytanie tego warunku tego pola losowe.

Oto przykładowe zapytania:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

W tym miejscu `<sample rate, 0-1>` określa część rekordy, które chcesz przykładowe użytkowników.

## <a name="group"></a>Losowe pobieranie próbek według grup

Podczas pobierania danych kategorii, warto uwzględnić lub wykluczyć wszystkie wystąpienia niektórych określoną wartość zmiennej kategorii. Jest to, co oznacza "próbek według grupy".
Na przykład jeśli masz zmiennej kategorii "Stan", który ma wartości, NY MA, urzędu certyfikacji, NJ, PA, itp, mają rekordy tego samego Państwa był zawsze razem, czy są pobierane, czy nie.

Oto przykładowa kwerenda tej próbki według grup:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Stratyfikowana pobierania

Losowe pobieranie próbek jest uporządkować w odniesieniu do kategorii zmiennej uzyskane próbki zawierających wartości że kategorii są w tej samej proporcji, tak jak w populacji nadrzędnej, z której zostały uzyskane próbki. W tym samym przykładzie jako powyżej, załóżmy, że dane mają grupy państw, powiedz NJ ma 100 uwagi, NY ma 60 spostrzeżeń i WA ma 300 uwagi. Jeśli użytkownik określi stawkę stratyfikowana przy próbkowaniu jako 0,5, następnie otrzymaną próbkę powinien mieć około 50, 30 i 150 uwag NJ, NY i WA odpowiednio.

Oto przykładowe zapytania:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
            rank() over (partition by state order by rand()) as state_rank
        from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Aby uzyskać informacji na temat bardziej zaawansowane przeprowadzania, które są dostępne w gałęzi zobacz [LanguageManual próbki](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).
