<properties
    pageTitle="Omówienie nauki danych za pomocą Spark na Azure HDInsight | Microsoft Azure"
    description="Zestaw narzędzi Spark MLlib powoduje nauki znaczące maszynowego modelowania możliwości rozłożone środowiska HDInsight."
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Omówienie nauki danych za pomocą Spark na Azure HDInsight

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Ten pakiet tematy pokazano, jak za pomocą HDInsight Spark wykonywanie typowych zadań nauki danych, takich jak spożyciu danych, funkcja techniczny modelowanie i oceny modelu. Dane używane są próbki 2013 Warszawa taksówki podróży i taryfy zestawu danych. Modele wbudowany zawiera regresji logistycznych i liniowy, losowe lasów i gradientu zwiększane drzewa. Tematy przedstawiono również sposobu przechowywania tych modeli w magazynie obiektów blob platformy Azure (WASB) oraz wynik i oceny ich przewidywanych wydajności. Tematy bardziej zaawansowane dotyczą, jak można modeli szkolenia przy użyciu kominów krzyżowego sprawdzania poprawności oraz funkcji hyper parametru. W tym temacie omówienie także opisano, jak skonfigurować klaster Spark, który należy wykonać czynności opisane w trzech instruktaży, dostarczanych. 

[Spark](http://spark.apache.org/) jest równolegle Otwórz źródło przetwarzania struktury, która obsługuje przetwarzanie w pamięci, aby zwiększyć wydajność aplikacji analityczny duży danych. Aparatu przetwarzania Spark jest przeznaczony dla szybkość łatwość użytkowania i zaawansowanych analiz. Możliwości obliczeń rozłożone w pamięci w iskrowym ułatwiają dobrym rozwiązaniem iteracyjne algorytmów w obliczeniach maszynowego uczenia się i wykres. [MLlib](http://spark.apache.org/mllib/) to w iskrowym skalowalna komputera nauki biblioteka, która zapewnia możliwości modelowania do tego środowiska rozłożone. 

[Usługa HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) jest Azure hostowanej oferowania Spark Otwórz źródło. Zawiera także obsługi **Jupyter PySpark notesów** na klaster Spark, który może zostać uruchomiony interakcyjnych zapytania Spark SQL Przekształcanie, filtrowania i Środek wywołujący dane przechowywane w obiektów blob platformy Azure (WASB). PySpark to interfejs API Python dla Spark. Fragmenty kodu, które zapewniają rozwiązania i przedstawiono odpowiednich powierzchni wizualizowanie danych tutaj Uruchom w notesach Jupyter zainstalowanym klastrów Spark. Modelowanie etapami następujące tematy zawierają kod, który przedstawia sposób przeszkolenie, oceny, zapisywanie i używanie każdego typu modelu. 

Kroki konfigurowania i kod podany w tym instruktażu dotyczy HDInsight 3.4 Spark 1.6. Jednak kod tutaj w notesach jest rodzajowy i powinna działać w dowolnym klastrze Spark. Jeśli nie używasz usługi HDInsight Spark, kroki konfiguracji i zarządzania klaster może być nieznacznie różni się od tego, co przedstawiono poniżej.

## <a name="prerequisites"></a>Wymagania wstępne

1. musisz mieć subskrypcję usługi Azure. Jeśli nie masz już jeden, zobacz [Uzyskiwanie Azure bezpłatną wersję próbną](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. konieczne klaster HDInsight 3.4 Spark 1.6 do przeprowadzenia tego instruktażu. Aby utworzyć ankietę, zobacz czynności opisane w sekcji [Wprowadzenie: tworzenie Apache Spark na Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Z menu **Wybierz typ klaster** określonego typu klaster i wersji. 


![](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [AZURE.NOTE] Tematu, która pokazuje, jak używać Scala zamiast Python do wykonania zadania związane z procesem nauki zakończenia do końca danych zobacz [nauki danych przy użyciu Scala z Spark Azure](machine-learning-data-science-process-scala-walkthrough.md).

<!-- -->

>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="the-nyc-2013-taxi-data"></a>Dane taksówki 2013 Warszawa

Dane Warszawa taksówki podróży jest około 20 GB plików skompresowany wartości rozdzielanych przecinkami (CSV) (~ 48 GB nieskompresowane), zawierających więcej niż miliona 173 pojedynczych podróży i opłaty opłacona każdej podróży. Każdy podróży rekord zawiera wybierz konto i umawiania i czas, zastąpione anonimowymi próba złamania (sterownik) liczbę licencji i liczba Medalionu (taksówki i unikatowy identyfikator). Dane obejmuje wszystkie podróży w roku 2013 i znajduje się w następujących dwa zestawy danych dla każdego miesiąca:

1. Z plików CSV "trip_data" zawierają szczegóły podróży, takie jak liczba osób, skompletowanie i dropoff punktów, rzeczy przed wyjazdem czas trwania i długości podróży. Poniżej przedstawiono kilka przykładowych rekordów:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. Pliki CSV "trip_fare" zawierają szczegóły taryfy za każdym razem, takie jak typ płatności, kwota taryfy, opłaty dodatkowej i podatki, porady i drogowe i całkowitą kwotę zapłaconą. Poniżej przedstawiono kilka przykładowych rekordów:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5


Firma Microsoft próbkę 0,1% tych plików i sprzężone podróż\_danych i podróży\_taryfy plików CSV do jednego zestawu danych, aby użyć jako danych wejściowych dla tego instruktażu. Unikatowy klucz, aby dołączyć do podróży\_danych i podróży\_taryfy składa się z polami: Medalionu, próba złamania\_licencji oraz odbiór\_daty i godziny. Każdy rekord zestawu danych zawiera następujące atrybuty reprezentującą wycieczki taksówki Warszawa:


|Pole| Krótki opis
|------|---------------------------------
| Medalionu |Zastąpione anonimowymi taksówki Medalionu (taksówki Unikatowy identyfikator)
| hack_license |    Liczba licencji karetki Hackney zastąpione anonimowymi
| vendor_id |   Identyfikator dostawcy taksówki
| rate_code | Warszawa taksówki stopień Taryfy
| store_and_fwd_flag | Przechowywanie i flagi do przodu
| pickup_datetime | Podnieś Data i godzina
| dropoff_datetime | Dropoff Data i godzina
| pickup_hour | Wybierz godzinę
| pickup_week | Podnieś tydzień roku
| dzień tygodnia | Weekday (zakres od 1 do 7)
| passenger_count | Liczba osób w podróży taksówki
| trip_time_in_secs | Czasem w sekundach
| trip_distance | W czasie podróży w mila odległość podróży
| pickup_longitude | Podnieś długości geograficznej
| pickup_latitude | Podnieś szerokości
| dropoff_longitude | Długość geograficzna Dropoff
| dropoff_latitude | Dropoff szerokości
| direct_distance | Bezpośrednie odległość między wybierz pozycję w górę i lokalizacji dropoff
| payment_type | Typ płatności (urzędów certyfikacji, karty kredytowej itp.)
| fare_amount | Kwota taryfy w
| Opłata dodatkowa | Opłata dodatkowa
| mta_tax | Podatek MTA
| tip_amount | Porada kwota
| tolls_amount | Kwota drogowe
| total_amount | Kwota całkowita
| Przechylony | Przechylony (0-1, nie lub tak)
| tip_class | Porada klasy (0: $0, 1: $0-5, 2: $6-10, 3: 11-20 zł, 4: > 20 zł)


## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Wykonanie kodu z notesu Jupyter w klastrze Spark 

Możesz uruchomić notesu Jupyter z portalem Azure. Znajdź klaster Spark na pulpicie nawigacyjnym, a następnie kliknij go, aby wprowadzić zarządzania strony klaster. Aby otworzyć Notes skojarzone z klastrem Spark, kliknij przycisk **Pulpity nawigacyjne klaster** -> **Jupyter notesu** .

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

Można też przejść do ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** , aby uzyskać dostęp do notesów Jupyter. Zamień NAZWAKLASTRA część tego adresu URL z nazwą własny klaster. Potrzebne hasło do konta administratora uzyskać dostęp do notesów.

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Wybierz PySpark, aby wyświetlić katalog, który zawiera kilka przykładów wstępnie detaliczny notesy, które za pomocą interfejsu API PySpark. Notesy, które zawierają przykłady kodu tego pakietu tematu Spark są dostępne na [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)


Możesz przekazać notesów bezpośrednio z Github na serwerze notes Jupyter w klastrze Spark. Na stronie głównej usługi Jupyter kliknij przycisk **Przekaż** z prawej strony ekranu. Zostanie otwarty Eksploratora plików. W tym miejscu można Wklej adres URL Github (zawartość nieprzetworzonych) notesu i kliknij przycisk **Otwórz**. Notesy PySpark są dostępne w następujących adresów URL:

1.  [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb)
2.  [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-model-consumption.ipynb)
3.  [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb)

Ponownie wyświetlić nazwy pliku na liście plik Jupyter przycisk **Przekaż** . Kliknij ten przycisk **Przekaż** . Teraz zaimportowano notesu. Powtórz te kroki, aby przekazać inne notesy w tym instruktażu.

> [AZURE.TIP] Możesz kliknąć prawym przyciskiem myszy łącza w przeglądarce i wybierz pozycję **Kopiuj łącze** do uzyskania github nieprzetworzonych zawartości adresu URL. Można wkleić ten adres URL do Jupyter przekazać plik Eksploratora okna dialogowego.

Teraz możesz wykonać następujące czynności:

- Zobacz kodu, klikając pozycję Notes.
- Wykonywanie każdej komórki, naciskając klawisze **SHIFT ENTER**.
- Uruchamianie całego notesu, klikając **komórkę** -> **Uruchamianie**.
- Za pomocą automatycznych wizualizacji zapytań.

> [AZURE.TIP] Jądro PySpark automatycznie spowoduje wizualizację dane wyjściowe zapytania SQL (HiveQL). Podano opcję, aby wybrać między kilka różnych rodzajów wizualizacji (tabela, kołowego, linia, obszar lub pasek) przy użyciu przycisków menu **Typ** w notesie:

![Krzywa ROC regresją podejściu ogólne](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Co to jest dalej?

Teraz, gdy są skonfigurowane z klastrem HDInsight Spark i przekazano Jupyter notesy, możesz przystąpić do pracy ze względu na tematy, które odpowiadają trzy notesów PySpark. Przedstawiają sposób Eksplorowanie danych oraz tworzenie i stosowanie modeli. Notes badań i modelowanie danych zaawansowane pokazano, jak dołączyć krzyżowego sprawdzania poprawności, funkcji hyper parametru zmiennymi, i modelowanie oceny. 

**Badań danych i modelowanie z Spark:** Eksplorowanie zestawu danych i tworzenie wynik, ocenianie maszynowego uczenia modeli dzięki pracy przez [Tworzenie klasyfikacji binarne i regresji modeli danych za pomocą narzędzi Spark MLlib](machine-learning-data-science-spark-data-exploration-modeling.md) tematu.

**Modelu zużycie:** Aby dowiedzieć się, jak wynik modeli klasyfikacji i regresji, utworzonych w tym temacie, zobacz [wynik i oceny modeli nauki wbudowany Spark komputera](machine-learning-data-science-spark-model-consumption.md).

**Krzyżowego sprawdzania poprawności i hyperparameter zmiennymi**: zobacz [Zaawansowane badań danych i modelowanie z Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) jak można modeli szkolenia przy użyciu kominów krzyżowego sprawdzania poprawności oraz funkcji hyper parametru

