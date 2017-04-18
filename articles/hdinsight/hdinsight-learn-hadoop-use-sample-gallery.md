<properties
   pageTitle="Dowiedz się, Hadoop w HDInsight z galerii przykładowe | Microsoft Azure"
   description="Szybkie informacje Hadoop uruchamiania aplikacji przykładowych z galerii HDInsight wprowadzenie wprowadzenie. Użyj danych przykładowych lub podać własne."
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
   ms.date="10/21/2016"
   ms.author="jgao"/>

# <a name="learn-hadoop-by-using-the-azure-hdinsight-getting-started-gallery"></a>Dowiedz się Hadoop przy użyciu galerii Azure HDInsight wprowadzenie wprowadzenie

Galeria wprowadzenie wprowadzenie jest dostępna tylko dla klastrów HDInsight systemu Windows. Galeria zawiera łatwy i szybki sposób ustalanie Hadoop, uruchamiając przykładowe aplikacje w HDInsight. Niektóre z próbki pochodzą z przykładowymi danymi. Można podać dane dla pozostałych próbek. Obecnie dostępne są następujące sześć próbek (z wejścia więcej):

- Rozwiązania z danymi Azure
    - Analiza dziennika witryny sieci Web Microsoft Azure
    - Microsoft Azure analizy miejsca do magazynowania
- Rozwiązania z przykładowymi danymi
    - Czujnik analiza danych
    - Analizy trendu Twitter
    - Analiza dziennika witryny sieci Web
    - Zalecenie film mahout

![Przykładowe dane w tym HDInsight Hadoop, Burza i HBase uzyskiwanie wprowadzenie galerii rozwiązań.][hdinsight.sample.gallery]

Poniższym klipie wideo pokazano, jak uruchomić przykład analizy trendu Twitter:

<center><a href="https://www.youtube.com/embed/7ePbHot1SN4">https://www.youtube.com/embed/7ePbHot1SN4></a></center>

Pulpit nawigacyjny jest możliwy przeglądając http://<YourHDInsightClusterName>.azurehdinsight.net/ lub z portalu Azure.

**Aby uruchomić próbki z galerii wprowadzenie wprowadzenie**

1. Zaloguj się do [portalu Azure][azure.portal].
2. Z menu po lewej stronie kliknij przycisk **Przeglądaj** , kliknij przycisk **HDInsight klastrów**, a następnie kliknij nazwę klaster.
3. Kliknij pozycję **pulpit nawigacyjny** w górnym menu.
4. Wprowadź nazwę użytkownika i hasło dla użytkownika HTTP (zwanych również użytkownika klaster).
6. Kliknij pozycję **Galeria wprowadzenie wprowadzenie** w górnej części strony.
7. Kliknij jeden z próbki. Każda próbka zawiera szczegółowe kroki służące do jego uruchamiania. Poniższa ilustracja przedstawia przykładowe analizy trendu Twitter:

    ![Przykładowe analizy trendu HDInsight Twitter][hdinsight.twitter.sample]

## <a name="next-steps"></a>Następne kroki
Inne sposoby Dowiedz się więcej o HDInsight obejmują:

- [Mapa nauki HDInsight][hdinsight.learn.map]
- [HDInsight infographic][hdinsight.infographic]

<!--Image references-->
[hdinsight.sample.gallery]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Getting-Started-Gallery.png
[hdinsight.twitter.sample]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Twitter-Trend-Analysis-sample.png

<!--Link references-->
[hdinsight.learn.map]: https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/
[hdinsight.infographic]: http://go.microsoft.com/fwlink/?linkid=523960
[azure.portal]:https://portal.azure.com
