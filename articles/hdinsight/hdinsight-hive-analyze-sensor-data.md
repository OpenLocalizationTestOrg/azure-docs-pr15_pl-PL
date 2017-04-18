<properties
    pageTitle="Analizowanie danych czujnika przy użyciu gałąź i Hadoop | Microsoft Azure"
    description="Dowiedz się, jak do analizowania danych czujnika przy użyciu konsoli kwerendy gałęzi HDInsight (Hadoop), a następnie wizualizowanie danych w programie Microsoft Excel PowerView."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="larryfr"/>

#<a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Analizowanie danych czujnik za pomocą konsoli kwerendy gałęzi na Hadoop w HDInsight

Dowiedz się, jak do analizowania danych czujnika przy użyciu konsoli kwerendy gałęzi HDInsight (Hadoop), a następnie wizualizacji danych w programie Microsoft Excel za pomocą programu Power View.

> [AZURE.NOTE] Kroki opisane w tym dokumencie działają tylko w klastrów HDInsight systemu Windows.

W tym przykładzie za pomocą gałęzi proces danych historycznych tworzone przez ogrzewania, wentylacji i systemy klimatyzacji (GRZEWCZO) do identyfikowania systemów, które nie są w stanie prawidłowo utrzymanie temperatury zestawu. Dowiesz się jak:

- Tworzenie tabel gałęzi do kwerendy danych przechowywanych w rozdzielanymi przecinkami (CSV).
- Tworzenie kwerendy gałęzi do analizy danych.
- Nawiązywanie połączenia z HDInsight (przy użyciu (ODBC) open database connectivity do pobierania danych analizowanych za pomocą programu Microsoft Excel.
- Wizualizowanie danych za pomocą programu Power View.

![Diagram architektury rozwiązania](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##<a name="prerequisites"></a>Wymagania wstępne

* Klaster HDInsight (Hadoop): zobacz [Hadoop świadczenia klastrów w HDInsight](hdinsight-provision-clusters.md) informacje na temat tworzenia klastrze.

* Program Microsoft Excel 2013

    > [AZURE.NOTE] Program Microsoft Excel jest używana do wizualizacji danych przy użyciu [Programu Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Sterownik ODBC gałęzi firmy Microsoft](http://www.microsoft.com/download/details.aspx?id=40886)

##<a name="to-run-the-sample"></a>Aby uruchomić przykład

1. W przeglądarce sieci web przejdź do następującego adresu URL. Zamienianie `<clustername>` o nazwie klaster HDInsight.

        https://<clustername>.azurehdinsight.net

    Po wyświetleniu monitu uwierzytelnianie za pomocą nazwy użytkownika i hasła użytych podczas inicjowania obsługi administracyjnej klaster.

2. Na stronie sieci web, która zostanie otwarta kliknij kartę **Galeria wprowadzenie wprowadzenie** , a następnie w kategorii **rozwiązań z przykładowymi danymi** kliknij Przykładowe **Czujnik analizy danych** .

    ![Wprowadzenie wprowadzenie Galeria](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Postępuj zgodnie z instrukcjami na stronie sieci web do końca próbki.
