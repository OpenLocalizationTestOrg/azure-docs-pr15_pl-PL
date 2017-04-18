<properties
    pageTitle="Tworzenie funkcji dla danych w klastrze Hadoop korzystanie z kwerend gałęzi | Microsoft Azure"
    description="Przykłady kwerend gałęzi, generujących funkcjach w danych przechowywanych w klastrze Azure HDInsight Hadoop."
    services="machine-learning"
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


#<a name="create-features-for-data-in-an-hadoop-cluster-using-hive-queries"></a>Tworzenie funkcji dla danych w klastrze Hadoop korzystanie z kwerend gałęzi

Ten dokument pokazano, jak utworzyć funkcje dla danych przechowywanych w klastrze Azure HDInsight Hadoop korzystanie z kwerend gałęzi. Te kwerendy gałęzi za pomocą osadzonego gałęzi użytkownika zdefiniowane funkcje (UDF), skryptów, dla którego są dostarczane.

Operacje potrzebne do utworzenia funkcje mogą być pamięci. Wydajność kwerend gałęzi staje się bardziej krytyczne w takich przypadkach i może być zwiększona przez dostosowywanie określonych parametrów. Dostosowywanie tych parametrów został omówiony w sekcji ostateczne.

Przykłady kwerend, które są prezentowane są specyficzne dla [Danych podróży taksówki Warszawa](http://chriswhong.com/open-data/foil_nyc_taxi/) scenariusze znajdują się też w [repozytorium Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Te kwerendy już mają określonego schematu danych i gotowy do wysłania do uruchomienia. W sekcji ostateczne także omówiono parametry, które użytkownicy można dostosować tak, aby można poprawić wydajność kwerend gałęzi.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Ten **menu** łączy do tematów dotyczących sposobu tworzenia funkcje dla danych w różnych środowiskach. To zadanie jest krok w [Zespołu danych nauki proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule założono, że:

* Utworzone konto Azure miejsca do magazynowania. Aby uzyskać instrukcje, zobacz [Tworzenie konta magazynu platformy Azure](../storage/storage-create-storage-account.md#create-a-storage-account)
* Obsługi administracyjnej dostosowane klaster Hadoop z usługą HDInsight.  Aby uzyskać instrukcje, zobacz [Dostosowywanie Azure HDInsight Hadoop klastrów zaawansowanych analiz](machine-learning-data-science-customize-hadoop-cluster.md).
* Dane zostało ono przekazane do tabel gałęzi w klastrów Azure HDInsight Hadoop. Jeśli nie, należy wykonać [Tworzenie i ładowanie danych do tabel gałęzi](machine-learning-data-science-move-hive-tables.md) , aby najpierw przekazać danych do tabel gałęzi.
* Włączony dostęp zdalny do klaster. Aby uzyskać instrukcje, zobacz [dostępu węzeł głowy klaster Hadoop](machine-learning-data-science-customize-hadoop-cluster.md#headnode).


##<a name="hive-featureengineering"></a>Generowanie funkcji

W tej sekcji opisano kilka przykładowych sposobów, w którym można generowania funkcje korzystanie z kwerend gałęzi. Wygenerowany dodatkowe funkcje możesz dodać je jako kolumny do istniejącej tabeli lub utworzyć nową tabelę z dodatkowe funkcje i klucza podstawowego, który można następnie łączyć z pierwotnej tabeli. Poniżej przedstawiono w przykładach przedstawionych:

1. [Częstotliwość podstawie Generowanie funkcji](#hive-frequencyfeature)
2. [Czynniki ryzyka kategorii zmiennych w klasyfikacji binarny](#hive-riskfeature)
3. [Wyodrębnianie funkcje z pola daty/godziny](#hive-datefeatures)
4. [Wyodrębnianie funkcje w polu tekstowym](#hive-textfeatures)
5. [Obliczanie odległości między współrzędne GPS](#hive-gpsdistance)

###<a name="hive-frequencyfeature"></a>Częstotliwość podstawie Generowanie funkcji

Często jest przydatne do obliczania częstotliwości poziomów zmiennej kategorii lub częstotliwości niektórych kombinacji poziomy od wielu zmiennych kategorii. Użytkownicy umożliwia obliczenie tych częstotliwości następujący skrypt:

        select
            a.<column_name1>, a.<column_name2>, a.sub_count/sum(a.sub_count) over () as frequency
        from
        (
            select
                <column_name1>,<column_name2>, count(*) as sub_count
            from <databasename>.<tablename> group by <column_name1>, <column_name2>
        )a
        order by frequency desc;


###<a name="hive-riskfeature"></a>Czynniki ryzyka kategorii zmiennych w klasyfikacji binarny

Binarny klasyfikacji trzeba przekształcania nieliczbowy kategorii zmiennych w funkcje liczbowe modeli używany tylko wykonać funkcje liczbowe. Jest to, zamieniając każdego poziomu nieliczbowy liczbowe ryzyka. W tej sekcji pokazano niektóre ogólne kwerendy gałęzi, służące do obliczania wartości ryzyka (prawdopodobieństwo dziennika) zmiennej kategorii.


        set smooth_param1=1;
        set smooth_param2=20;
        select
            <column_name1>,<column_name2>,
            ln((sum_target+${hiveconf:smooth_param1})/(record_count-sum_target+${hiveconf:smooth_param2}-${hiveconf:smooth_param1})) as risk
        from
            (
            select
                <column_nam1>, <column_name2>, sum(binary_target) as sum_target, sum(1) as record_count
            from
                (
                select
                    <column_name1>, <column_name2>, if(target_column>0,1,0) as binary_target
                from <databasename>.<tablename>
                )a
            group by <column_name1>, <column_name2>
            )b

W tym przykładzie zmienne `smooth_param1` i `smooth_param2` są ustawione na gładkim wartości ryzyka obliczana na podstawie danych. Czynniki ryzyka pojawić się zakres między -Inf i Inf. Czynniki ryzyka > 0 wskazuje, że prawdopodobieństwo, że obiekt docelowy jest równy 1 jest większa niż 0,5.

Po ryzyka tabeli jest obliczana, użytkownicy mogą przypisywać ryzyka wartości do tabeli przez połączenie go z tabelą ryzyka. Kwerenda łączących gałęzi podano w poprzedniej sekcji.

###<a name="hive-datefeatures"></a>Wyodrębnianie funkcje z pól daty i godziny

Gałąź jest dostarczana z zestawu funkcji zdefiniowanych przez użytkownika dla przetwarzania pól daty i godziny. W gałęzi, jest domyślny format daty i godziny "RRRR MM-dd 00:00:00" ("1970-01-01 12:21:32" na przykład). W tej sekcji pokazano przykłady wyodrębnić dzień miesiąca, miesiąc z pola daty/godziny, a inne przykłady, które konwertują ciągu daty i godziny w formacie innym niż domyślny format daty i godziny ciąg domyślnych formatowanie.

        select day(<datetime field>), month(<datetime field>)
        from <databasename>.<tablename>;

Ta kwerenda gałęzi zakłada się, że *& #60; pola daty/godziny >* jest w polu Domyślny format daty i godziny.

Jeśli pola daty/godziny nie znajduje się w domyślnym formacie, należy najpierw konwertowanie pola daty/godziny systemu Unix sygnatura czasowa, a następnie przekonwertowanie sygnatura czasowa Unix na ciąg jest w polu Domyślny format daty i godziny. Gdy data/godzina w domyślnej jest format, użytkownicy mogą stosować osadzony funkcji zdefiniowanych przez użytkownika, aby wyodrębnić funkcje daty/godziny.

        select from_unixtime(unix_timestamp(<datetime field>,'<pattern of the datetime field>'))
        from <databasename>.<tablename>;

W tej kwerendzie Jeśli *& #60; pola daty/godziny >* występują deseniu podobnym *2015-03-26 12:04:39*, *"& #60; deseń pola daty/godziny >"* powinny być `'MM/dd/yyyy HH:mm:ss'`. Aby przetestować, użytkownicy mogą uruchamiać

        select from_unixtime(unix_timestamp('05/15/2015 09:32:10','MM/dd/yyyy HH:mm:ss'))
        from hivesampletable limit 1;

*Hivesampletable* w tej kwerendzie pochodzą preinstalowanych wszystkich klastrów Azure HDInsight Hadoop domyślnie po klastrów zainicjowano obsługę administracyjną.


###<a name="hive-textfeatures"></a>Wyodrębnianie funkcje z pól tekstowych

Gdy tabelę programu Hive zawiera pola tekstowego zawierającego ciąg wyrazów, które są rozdzielane znakami spacji, poniższe zapytanie wyodrębnia długość ciągu i liczbę wyrazów w ciągu.

        select length(<text field>) as str_len, size(split(<text field>,' ')) as word_num
        from <databasename>.<tablename>;

###<a name="hive-gpsdistance"></a>Obliczanie odległości między zestawami współrzędne GPS

Kwerendy w tej sekcji można bezpośrednio stosować do danych podróży taksówki Warszawa. Celem kwerendy jest pokazanie sposobu stosowania funkcji matematycznych osadzony w gałęzi, aby wygenerować funkcje.

Pola, które są używane w kwerendzie są współrzędne GPS odbiór i dropoff lokalizacji o nazwie *pobrania\_długości geograficznej*, *pobrania\_szerokości*, *dropoff\_długości geograficznej*, i *dropoff\_szerokości*. Kwerendy, obliczające bezpośrednie odległość między współrzędne odbiór i dropoff są:

        set R=3959;
        set pi=radians(180);
        select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude,
            ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
            *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
            *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
            /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
            +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
            pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
        from nyctaxi.trip
        where pickup_longitude between -90 and 0
        and pickup_latitude between 30 and 90
        and dropoff_longitude between -90 and 0
        and dropoff_latitude between 30 and 90
        limit 10;

Równania matematyczne, służące do obliczania odległości między dwoma współrzędne GPS można znaleźć w witrynie <a href="http://www.movable-type.co.uk/scripts/latlong.html" target="_blank">Typu ruchome skryptów</a> autorem Lapisu Peterowi. W jego Javascript, funkcja `toRad()` jest po prostu *lat_or_lon*pi/180*, która konwertuje stopnie na radiany. W tym miejscu *lat_or_lon * jest szerokości i długości geograficznej. Ponieważ gałąź nie umożliwia funkcja `atan2`, ale zapewnia funkcja `atan`, `atan2` funkcja jest wykonywane przez `atan` funkcja w powyższa kwerenda gałęzi przy użyciu definicji zawartej w <a href="http://en.wikipedia.org/wiki/Atan2" target="_blank">Wikipedia</a>.

![Tworzenie obszaru roboczego](./media/machine-learning-data-science-create-features-hive/atan2new.png)

Pełna lista gałęzi osadzony funkcji zdefiniowanych przez użytkownika można znaleźć w sekcji **Funkcje wbudowane** <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-MathematicalFunctions" target="_blank">gałęzi Apache typu wiki</a>).  

## <a name="tuning"></a>Tematy dotyczące zaawansowanego: parametry gałęzi dostosować, aby zwiększyć szybkość kwerend

Domyślne ustawienia parametru klaster gałęzi mogą nie być odpowiednie dla kwerend gałęzi i dane, które są przetwarzania kwerend. W tej sekcji omówimy niektóre parametry, które użytkownicy mogą dostosować które zwiększyć wydajność kwerend gałęzi. Należy dodać parametr Dostosowywanie kwerendy przed kwerend przetwarzania danych użytkownikom.

1. **Obszar stosu Java**: w przypadku kwerend dotyczących dołączania dużych zestawów danych lub przetwarzania długich rekordach **zaczyna brakować miejsca stosu** jest jednym z typowych błędów. Można to dostosowanych, ustawiając parametrów *mapreduce.map.java.opts* i *mapreduce.task.io.sort.mb* do odpowiedniej wartości. Oto przykład:

        set mapreduce.map.java.opts=-Xmx4096m;
        set mapreduce.task.io.sort.mb=-Xmx1024m;


    Ten parametr przydziela 4GB pamięci RAM do obszaru stosu Java i również ułatwia sortowanie efektywniejsze przydzielając więcej pamięci dla niego. Jest dobrym pomysłem jest odtwarzanie z tych przydziałów w przypadku każde zadanie błąd błędy związane z miejsca stosu.

2. **Rozmiaru bloku DFS** : ten parametr określa najmniejszą jednostką danych przechowywanych w systemie plików. Na przykład jeśli rozmiar bloku DFS jest 128MB, następnie wszelkie dane o rozmiarze mniej niż i do 128MB są przechowywane w pojedynczy blok, podczas danych, który jest większy niż 128MB przydzielono dodatkowe bloków. Wybieranie bardzo mały rozmiar bloku powoduje duże koszty ogólne Hadoop, ponieważ węzeł nazwa zawiera przetwarzanie wiele żądań więcej, aby znaleźć odpowiednie blok, odnoszących się do pliku. Zalecane ustawienie podczas dotyczących gigabajtów (lub więcej) dane są:

        set dfs.block.size=128m;

3. **Optymalizowanie dołączyć operacje w gałęzi** : podczas operacji join w ramach mapy/zmniejszanie zazwyczaj odbywa się w fazie zmniejszanie, czasami zyski ponad normę uzyskuje się dzięki planowania sprzężenia w fazie mapy (nazywane także "mapjoins"). Aby skierować gałęzi, aby to zrobić, jeśli to możliwe, możemy ustawić:

        set hive.auto.convert.join=true;

4. **Określanie liczby mappers do gałęzi** : podczas Hadoop pozwala użytkownikowi określić liczbę reduktory, zwykle jest to liczba mappers nie można ustawić przez użytkownika. Lewy, umożliwiający, że jest pewien stopień kontrolę na ten numer, aby wybrać zmienne Hadoop, *mapred.min.split.size* i *mapred.max.split.size* jako rozmiaru każdego zadania mapy jest określona przez:

        num_maps = max(mapred.min.split.size, min(mapred.max.split.size, dfs.block.size))

    Zazwyczaj wartość domyślna *mapred.min.split.size* jest równy 0, który *mapred.max.split.size* jest **Long.MAX** i które *dfs.block.size* jest 64 MB. Jak widać, dostarczonych danych, rozmiar dostosowywania tych parametrów "ustawienie" ich pozwala dostosować liczbę mappers używane.

5. Kilka innych więcej **Zaawansowane opcje** dotyczące optymalizowania wydajności gałęzi są wymienione poniżej. Te umożliwiają ustawianie pamięć przydzielona do zamapować i zmniejszyć zadania i może być przydatne w ta wydajności. Należy pamiętać, że *mapreduce.reduce.memory.mb* nie może być większy niż rozmiar pamięci fizycznej każdego węzła pracownika w klastrze Hadoop.

        set mapreduce.map.memory.mb = 2048;
        set mapreduce.reduce.memory.mb=6144;
        set mapreduce.reduce.java.opts=-Xmx8192m;
        set mapred.reduce.tasks=128;
        set mapred.tasktracker.reduce.tasks.maximum=128;
