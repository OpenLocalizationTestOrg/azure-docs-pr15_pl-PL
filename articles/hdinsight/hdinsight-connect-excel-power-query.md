<properties
    pageTitle="Łączenie programu Excel z Hadoop dodatku Power Query | Microsoft Azure"
    description="Dowiedz się, jak korzystać z składniki analizy biznesowej i dostęp do danych zapisanych w Hadoop na HDInsight za pomocą dodatku Power Query dla programu Excel."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


#<a name="connect-excel-to-hadoop-by-using-power-query"></a>Łączenie programu Excel do Hadoop za pomocą dodatku Power Query

Jedną z najważniejszych funkcji rozwiązania duży danych firmy Microsoft jest integracja składników analiz biznesowych firmy Microsoft z klastrów Hadoop w Azure HDInsight. Podstawowy przykładem Integracja ta jest możliwość łączenia z programu Excel z tym kontem magazyn Azure, zawierający dane skojarzone z klaster Hadoop przy użyciu dodatku Microsoft Power Query dla programu Excel. W tym artykule opisano sposób konfigurowania i dane kwerendy skojarzone z klastrem Hadoop zarządzane z usługi HDInsight za pomocą dodatku Power Query.

> [AZURE.NOTE] Chociaż opisanych w tym artykule mogą być używane z klastrem Linux lub HDInsight systemu Windows, systemu Windows jest wymagana roboczą klienta.

### <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem tego artykułu, musisz mieć następujące czynności:

- **Klaster HDInsight**. Aby skonfigurować jedną, zobacz [Wprowadzenie do usługi Azure HDInsight][hdinsight-get-started].
- **A pracy** z systemem Windows 7, Windows Server 2008 R2 lub nowszy system operacyjny.
- **Pakietu office 2013 Professional Plus, usługi Office 365 ProPlus, Wersja autonomiczna programu Excel 2013 lub Office 2010 Professional Plus**.


## <a name="install-power-query"></a>Instalowanie dodatku Power Query

Dodatek Power Query umożliwia importowanie danych z różnych źródeł w programie Microsoft Excel, w którym go power narzędzi analizy Biznesowej, takich jak dodatek PowerPivot i programu Power View. W szczególności dodatku Power Query można importować dane, który ma zostały wyprowadzania lub który został wygenerowany przez zadanie Hadoop uruchamiania w klastrze HDInsight.

Pobierz dodatek Microsoft Power Query dla programu Excel z [Centrum pobierania Microsoft] [ powerquery-download] i zainstaluj go.

## <a name="import-hdinsight-data-into-excel"></a>Importowanie danych z usługi HDInsight do programu Excel

Dodatek Power Query dla programu Excel można łatwo importować dane z usługi HDInsight klaster w programie Excel, gdzie narzędzi analizy Biznesowej, takich jak dodatek PowerPivot i dodatku Power Map można sprawdzić, analizowanie i prezentowanie danych.

**Aby zaimportować dane z klaster HDInsight**

1. Otwórz program Excel.

2. Utwórz nowy pusty skoroszyt.

3. Kliknij menu **Dodatku Power Query** , kliknij przycisk **Z Azure**, a następnie kliknij **Z usługi Microsoft Azure HDInsight**.

    ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **Uwaga:** Jeśli nie widzisz menu **Dodatku Power Query** , przejdź do **pliku** > **Opcje** > **Dodatki**, a następnie wybierz pozycję **Dodatki COM** z pola listy rozwijanej **Zarządzaj** w dolnej części strony. Kliknij przycisk **Przejdź...** i sprawdź, czy zostało zaznaczone pole dla dodatku Power Query dla programu Excel.

    **Uwaga:** Dodatek Power Query umożliwia importowanie danych z plików HDFS, klikając pozycję **Z innych źródeł**.

3. W polu **Nazwa konta**wprowadź nazwę konta magazynu obiektów Blob platformy Azure skojarzonego z klaster, a następnie kliknij **przycisk OK**. Może to być [domyślne miejsca do magazynowania konto](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) lub konto połączone miejsca do magazynowania.  Format jest *https://<StorageAccountName>.blob.core.windows.net/*.

4. **Klucz konta**wprowadź klucz konta magazyn obiektów Blob, a następnie kliknij przycisk **Zapisz**. (Musisz to zrobić tylko po raz pierwszy uzyskujesz dostęp do tego magazynu.)

5. W okienku **Nawigator** po lewej stronie Edytor zapytań kliknij dwukrotnie nazwę kontenera magazyn obiektów Blob. Domyślnie nazwa kontenera jest taką samą nazwę jak nazwa klaster.

6. Znajdź **HiveSampleData.txt** w kolumnie **Nazwa** (ścieżka folderu jest **... / gałęzi/magazyn/hivesampletable/**), a następnie kliknij pozycję **binarne** po lewej stronie HiveSampleData.txt. HiveSampleData.txt zawiera wszystkie klaster. Opcjonalnie można użyć własnego pliku.

    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. Jeśli chcesz, można zmienić nazwy kolumn. Gdy skończysz, kliknij przycisk **Zamknij i załaduj**.  Dane zostały załadowane do skoroszytu:

    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono sposób pobieranie danych z usługi HDInsight do programu Excel za pomocą dodatku Power Query. Podobnie możesz pobierać dane z usługi HDInsight do bazy danych SQL Azure. Użytkownik może również do przekazania danych do HDInsight. Aby uzyskać więcej informacji, zobacz następujące artykuły:

* [Nawiązywanie połączenia programu Excel z usługą hdinsight za pomocą sterownika ODBC gałęzi firmy Microsoft][hdinsight-ODBC]
* [Przekazywanie danych do HDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
