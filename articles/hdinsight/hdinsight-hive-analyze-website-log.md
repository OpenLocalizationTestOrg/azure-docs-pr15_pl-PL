<properties 
    pageTitle="Za pomocą programu Hive Hadoop do witryny sieci Web dziennika analizy | Microsoft Azure" 
    description="Dowiedz się, jak używać gałęzi z usługi HDInsight do analizowania dzienniki witryny sieci Web. Będzie jako dane wejściowe do tabeli HDInsight za pomocą pliku dziennika, a kwerendę danych przy użyciu HiveQL." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/17/2016" 
    ms.author="nitinme"/>

# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>Analizowanie dzienników z witryn sieci Web przy użyciu gałęzi z usługi HDInsight

Dowiedz się, jak używać HiveQL z usługi HDInsight do analizowania dzienniki z witryny sieci Web. Analiza dziennika witryny sieci Web może służyć do segmentu odbiorców na podstawie podobne działania, kategoryzowanie odwiedzających witrynę przez demograficzne i Dowiedz się, zawartość ich widoku, pochodzą z witryny sieci Web i tak dalej.

W tym przykładzie użyje klaster HDInsight analizowanie plików dziennika witryny sieci Web uzyskiwania wgląd częstotliwość wizyty w witrynie sieci Web z zewnętrznych witryn sieci Web w ciągu dnia. Będzie również tworzyć podsumowanie błędów witryny sieci Web, które wystąpić użytkowników. Dowiesz się jak:

- Nawiązywanie połączenia z magazynem obiektów Blob platformy Azure, zawierającego pliki dziennika witryny sieci Web.
- Tworzenie tabel gałęzi do kwerendy tych dzienników.
- Tworzenie kwerendy gałęzi do analizy danych.
- Nawiązywanie połączenia z usługi HDInsight (przy użyciu (ODBC) open database connectivity do pobierania danych analizowanych za pomocą programu Microsoft Excel.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

##<a name="prerequisites"></a>Wymagania wstępne

- Musisz przygotowana klaster Hadoop na Azure HDInsight. Aby uzyskać instrukcje, zobacz [Świadczenia usługi HDInsight klastrów][hdinsight-provision]. 
- Zainstalowany program Excel 2010 lub musi być Microsoft Excel 2013.
- Musi być [Sterownik ODBC gałęzi firmy Microsoft](http://www.microsoft.com/download/details.aspx?id=40886) do importowania danych z gałęzi do programu Excel.


##<a name="to-run-the-sample"></a>Aby uruchomić przykład

1. W [Azure Portal](https://portal.azure.com/)z Startboard (jeśli przypięte klaster) kliknij Kafelek klaster, na którym chcesz uruchomić próbki.

2. Z karta klaster, w obszarze **Szybkie łącza**kliknij pozycję **Pulpit nawigacyjny klaster**, a następnie z karta **Pulpit nawigacyjny klaster** kliknij **Pulpit nawigacyjny klaster HDInsight**. Alternatywnie możesz bezpośrednio otworzyć pulpitu nawigacyjnego za pomocą następujący adres URL:

        https://<clustername>.azurehdinsight.net
    
    Po wyświetleniu monitu uwierzytelnianie za pomocą nazwy użytkownika i hasła użytych podczas inicjowania obsługi administracyjnej klaster.
  
2. Na stronie sieci web, która zostanie otwarta kliknij kartę **Galeria wprowadzenie wprowadzenie** , a następnie w kategorii **rozwiązań z przykładowymi danymi** kliknij przykładowa **Analiza dziennika witryny sieci Web** .

3. Postępuj zgodnie z instrukcjami na stronie sieci web do końca próbki.

##<a name="next-steps"></a>Następne kroki
Spróbuj Poniższy przykładowy: [Analizowanie danych czujnika przy użyciu gałęzi z usługi HDInsight](hdinsight-hive-analyze-sensor-data.md).


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
 
