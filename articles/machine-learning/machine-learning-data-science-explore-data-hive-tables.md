<properties
    pageTitle="Eksplorowanie danych w tabelach gałęzi z kwerendami gałęzi | Microsoft Azure"
    description="Eksplorowanie danych w tabelach gałęzi korzystanie z kwerend gałęzi."
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="explore-data-in-hive-tables-with-hive-queries"></a>Eksplorowanie danych w tabelach gałęzi z kwerendami gałęzi

Niniejszy dokument zawiera przykładowe skrypty gałęzi są używane do Eksplorowanie danych w tabelach gałęzi w klastrze HDInsight Hadoop.

Następujące **menu** łącza do tematów opisujących, jak za pomocą narzędzia Eksplorowanie danych z różnych środowiskach miejsca do magazynowania.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule założono, że:

* Utworzone konto Azure miejsca do magazynowania. Aby uzyskać instrukcje, zobacz [Tworzenie konta magazynu platformy Azure](../storage/storage-create-storage-account.md#create-a-storage-account)
* Obsługi administracyjnej dostosowane klaster Hadoop z usługą HDInsight. Aby uzyskać instrukcje, zobacz [Dostosowywanie Azure HDInsight Hadoop klastrów zaawansowanych analiz](machine-learning-data-science-customize-hadoop-cluster.md).
* Dane zostało ono przekazane do tabel gałęzi w klastrów Azure HDInsight Hadoop. Jeśli nie, postępuj zgodnie z instrukcjami [Tworzenie i ładowanie danych do tabel programu Hive](machine-learning-data-science-move-hive-tables.md) aby najpierw przekazać danych do tabel gałęzi.
* Włączony dostęp zdalny do klaster. Aby uzyskać instrukcje, zobacz [dostępu węzeł głowy klaster Hadoop](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* W razie potrzeby instrukcje dotyczące przesyłać kwerendy gałęzi, zobacz, [jak przesyłanie kwerend gałęzi](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Przykład gałęzi kwerendy skrypty do badań danych

1. Zliczanie obserwacji na partition `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`

2. Zliczanie uwagi na dzień `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`

3. Uzyskiwanie poziomów w kategorii kolumny  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`

4. Uzyskiwanie liczbę poziomów w połączeniu z dwóch kolumn kategorii `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`

5. Uzyskiwanie rozkładu dla kolumn liczbowych  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`

6. Wyodrębnianie rekordy z łączenia dwóch tabel

        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>Skrypty dodatkowych kwerend dla scenariuszy danych taksówki podróży

Przykłady kwerend, które są specyficzne dla scenariuszy [Warszawa taksówki podróży danych](http://chriswhong.com/open-data/foil_nyc_taxi/) znajdują się też w [repozytorium Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Te kwerendy już mają określonego schematu danych i gotowy do wysłania do uruchomienia.
