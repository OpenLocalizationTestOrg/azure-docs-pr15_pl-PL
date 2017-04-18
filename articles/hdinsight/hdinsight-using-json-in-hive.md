<properties
   pageTitle="Analizowanie i JSON proces dokumentów gałąź w HDInsight | Microsoft Azure"
   description="Dowiedz się, jak korzystać z dokumentów JSON i analizować je przy użyciu gałąź w HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Przetwarzanie i analizowanie JSON dokumentów przy użyciu gałąź w HDInsight

Dowiedz się, jak przetwarzania i analizowania JSON pliki przy użyciu gałąź w HDInsight. Następujący dokument JSON zostanie użyte w przerabiania samouczka

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Plik można znaleźć w wasbs://processjson@hditutorialdata.blob.core.windows.net/. Aby uzyskać więcej informacji na temat korzystania z magazynem obiektów Blob platformy Azure z usługi HDInsight zobacz [Magazyn obiektów Blob platformy Azure zgodnego z programem HDFS korzystanie z Hadoop w HDInsight](hdinsight-hadoop-use-blob-storage.md). Jeśli chcesz, możesz skopiować pliku do domyślnego kontenera klaster.

W tym samouczku użyje konsoli gałęzi.  Aby uzyskać instrukcje dotyczące otwierania konsoli gałęzi zobacz [Używanie gałęzi z Hadoop na HDInsight z pulpitu zdalnego](hdinsight-hadoop-use-hive-remote-desktop.md).

##<a name="flatten-json-documents"></a>Spłaszcz JSON dokumentów

Opisane w następnej sekcji metody wymagają dokumentu JSON w jednym wierszu. Dlatego należy spłaszczyć dokumentu JSON do ciągu. Jeśli dokument JSON jest już spłaszczoną, możesz pominąć ten krok i przejść bezpośrednio do następnej sekcji JSON analizy danych.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Plik JSON nieprzetworzonych znajduje się w **wasbs://processjson@hditutorialdata.blob.core.windows.net/**. Tabelę programu Hive *StudentsRaw* wskazuje nieprzetworzonych nie spłaszczoną dokumentu JSON.

Tabelę programu Hive *StudentsOneLine* przechowywane dane z domyślnego systemu plików HDInsight w ścieżce */json/uczniów lub studentów /* .

Instrukcja INSERT wypełnienia tabeli StudentOneLine spłaszczoną danych JSON.

Instrukcja SELECT zwracają tylko wiersz 1.

Oto wyniki instrukcji SELECT:

![Spłaszczanie dokumentu JSON.][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>Analizowanie dokumentów JSON w gałęzi

Gałąź zawiera trzy różne mechanizmy uruchamianie kwerend w dokumentach JSON:

- Używanie składnika\_JSON\_UDF obiektu (funkcja zdefiniowana przez użytkownika)
- Używanie JSON_TUPLE UDF
- Używanie niestandardowej SerDe
- Napisz prawa własności UDF za pomocą Python lub innych języków. Zobacz [Ten artykuł] [ hdinsight-python] w systemie kodzie Python gałęzi.

### <a name="use-the-getjsonobject-udf"></a>Używanie składnika\_JSON_OBJECT UDF
Gałąź zawiera wbudowane UDF o nazwie [pobrać obiektu json](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) które mogą wykonywać JSON kwerendy podczas wykonywania. Ta metoda przyjmuje dwa argumenty — Nazwa tabeli i nazwy metody, który ma spłaszczoną dokumentu JSON i pole JSON, które musi być analizowane. Spójrzmy na przykład aby zobaczyć, jak działa ta UDF.

Imię i nazwisko dla każdego ucznia

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Oto wyniki, gdy działa ta kwerenda w oknie konsoli.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Istnieje kilka ograniczeń UDF get-json_object.

- Ponieważ każde z pól w kwerendzie wymaga ponownego podczas analizy kwerendy, ma wpływ na wydajność.
- Uzyskiwanie\_JSON_OBJECT() zwraca Reprezentacja tekstowa tablicy. Aby przekonwertować to tablicę gałęzi, musisz użyć wyrażeń regularnych, aby zamienić nawiasów kwadratowych "[" i "]", a następnie również wywołać Podziel, aby uzyskać tablicy.


Jest to, dlaczego typu wiki gałęzi zaleca używanie json_tuple.  

### <a name="use-the-jsontuple-udf"></a>Używanie JSON_TUPLE UDF

Inny UDF dostarczony przez gałąź nosi nazwę [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple) wykonuje się lepiej niż [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Ta metoda przyjmuje zestawu klawiszy i ciągu JSON i zwraca krotki wartości przy użyciu funkcji. Poniższa kwerenda zwraca identyfikator uczniów i kategorii z dokumentu JSON:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

Dane wyjściowe skrypt w konsoli gałęzi:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_KROTKI używa składni [Widok boczny](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) w gałęzi, dzięki czemu json\_krotki, aby utworzyć tabelę wirtualnych przez zastosowanie funkcji UDT do każdy wiersz oryginalnej tabeli.  Złożone JSONs być zbyt trudno obsługiwać ze względu na powtarzających się stosowania widok BOCZNY. Ponadto JSON_TUPLE nie obsługuje zagnieżdżonych JSONs.


###<a name="use-custom-serde"></a>Używanie niestandardowej SerDe

SerDe jest to najlepszy wybór dla analizy zagnieżdżony dokumentów JSON, umożliwia definiowanie schematu JSON i używać schematu, aby przeanalizować dokumenty. W tym samouczku użyjesz jedną z najpopularniejszych SerDe, która została utworzona przez [rcongiu](https://github.com/rcongiu).

**Aby użyć niestandardowej SerDe:**

1. W celu zainstalowania [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Wybierz wersję systemu Windows X64 JDK, jeśli zamierzasz korzystać z wdrażanie usługi HDInsight systemu Windows

    >[AZURE.WARNING] JDK 1.8 nie działa z tym SerDe.

    Po zakończeniu instalacji, Dodaj nową zmienną środowiska użytkownika:

    1. Na ekranie systemu Windows, otwórz **Widok Zaawansowane ustawienia systemu** .
    2. Kliknij przycisk **zmienne**.  
    3. Dodaj nową zmienną środowiska **JAVA_HOME** wskazuje **C:\Program Files\Java\jdk1.7.0_55** lub wszędzie tam, gdzie jest zainstalowany z JDK.

    ![Definiowanie konfiguracji poprawne wartości dla JDK][image-hdi-hivejson-jdk]

2. Instalowanie [środowiska Maven 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)

    Dodawanie folderu bin do ścieżki, przechodząc do kontrolki panelu--> Edytuj zmienne systemu dla zmiennych Environment konta. Zrzut ekranu poniżej pokazano, jak to zrobić.

    ![Konfigurowanie środowiska Maven][image-hdi-hivejson-maven]

3. Klonowanie projektu z witryny github [Gałęzi-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) . Można to zrobić, klikając przycisk "Pobierz Zip", jak pokazano poniżej ekranu.

    ![Klonowanie projektu][image-hdi-hivejson-serde]

4: Przejdź do folderu, w którym został pobrany ten pakiet i typu "pakiet mvn". To należy utworzyć pliki niezbędne słoik, które następnie można kopiować z klastrem przez.

5: Przejdź do folderu docelowego w folderze głównym folderu, do którego został pobrany pakiet. Przekaż plik json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar do węzła głowy klaster. Został zazwyczaj umieszczony w folderze binarne gałęzi: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin lub podobnej.

6: w wierszu gałęzi wpisz "Dodaj /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar słoik". Ponieważ w moim przypadku słoju znajduje się w folderze C:\apps\dist\hive-0.13.x\bin, można bezpośrednio dodać słoik o nazwie tak jak pokazano poniżej:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

Teraz możesz przystąpić do za pomocą SerDe w celu uruchomienia kwerendy dokumentu JSON.

Następująca instrukcja Utwórz tabelę z określonego schematu

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Aby wyświetlić listę imię i Nazwisko ucznia

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Oto wynik z konsoli gałęzi.

![Kwerendy SerDe 1][image-hdi-hivejson-serde_query1]

Aby obliczyć sumę wyników dokumentu JSON

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

W kwerendzie powyżej użyto [rozsuwanie widok boczny](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF, aby rozwinąć tablicę wyników, tak aby mogą być sumowane.

Poniżej przedstawiono dane wyjściowe z konsoli gałęzi.

![Kwerendy SerDe 2][image-hdi-hivejson-serde_query2]

Aby znaleźć wybierz które danej student ma uzyskał więcej niż 80 punktów tematów  
      JT. StudentClassCollection.ClassId z json_table jt widok boczny rozsuwanie (jt. Kolekcja StudentClassCollection.Score) jako wynik miejsce, w którym wynik > 80;

Powyżej kwerenda zwraca tablicę gałęzi w odróżnieniu od get\_json\_obiektu, który zwraca wartość typu ciąg.

![Kwerendy SerDe 3][image-hdi-hivejson-serde_query3]

Jeśli chcesz skil utworzonym JSON, a następnie zgodnie z opisem [strony typu wiki](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) ta SerDe można uzyskać, który, wpisując kod poniżej:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>Podsumowanie
Podsumowując typ operatora JSON w gałęzi, którą należy wybrać zależy od aktualnego scenariusza. Jeśli masz prosty dokument JSON i masz tylko jedno pole, aby wyszukać na — możesz wybrać umożliwia uzyskiwanie gałęzi UDF\_json\_obiektu. Jeśli masz więcej niż jedno klawiszy wyszukująca można użyć json_tuple. Jeśli masz zagnieżdżony dokumentu, należy używać JSON SerDe.

Aby uzyskać inne artykuły pokrewne zobacz

- [Za pomocą gałąź i HiveQL Hadoop w HDInsight do analizowania przykładowy plik log4j Apache](hdinsight-use-hive.md)
- [Analizowanie danych opóźnienia lotów przy użyciu gałąź w HDInsight](hdinsight-analyze-flight-delay-data.md)
- [Analizowanie danych Twitter przy użyciu gałąź w HDInsight](hdinsight-analyze-twitter-data.md)
- [Uruchom zadanie Hadoop przy użyciu DocumentDB i HDInsight](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
