<properties 
    pageTitle="Przetwarzanie danych z platformy SQL Azure | Microsoft Azure" 
    description="Proces danych z platformy SQL Azure" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Proces danych w maszyn wirtualnych serwera SQL Azure

W tym dokumencie opisano sposoby Eksplorowanie danych oraz generowanie funkcje dla danych przechowywanych w maszyny serwera SQL Azure. Można to zrobić, wrangling danych za pomocą SQL albo za pomocą języka programowania, takich jak Python.


> [AZURE.NOTE] Przykładowe instrukcji SQL w tym dokumencie przyjęto założenie, że dane są w programie SQL Server. Jeśli nie, zajrzyj do mapy procesu chmury danych naukowych, aby dowiedzieć się, jak przenieść dane programu SQL Server.

##<a name="SQL"></a>Za pomocą SQL

Opisano następujące zadania wrangling danych w tej sekcji za pomocą SQL:

1. [Poszukiwanie danych](#sql-dataexploration)
2. [Generowanie funkcji](#sql-featuregen)

###<a name="sql-dataexploration"></a>Poszukiwanie danych
Poniżej przedstawiono kilka przykładowe skrypty SQL, które mogą być używane Eksplorowanie magazynów danych w programie SQL Server.


> [AZURE.NOTE] Na przykład wszystkie stosowne można użyć [taksówki Warszawa zestawu danych](http://www.andresmh.com/nyctaxitrips/) i odwołują się do IPNB zatytułowany [wrangling Warszawa danych przy użyciu programu SQL Server i notesów IPython](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) dla samouczek zakończenia do końca.

1. Zliczanie uwagi na dzień

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Uzyskiwanie poziomów w kategorii kolumny

    `select  distinct <column_name> from <databasename>`

3. Uzyskiwanie liczbę poziomów w połączeniu z dwóch kolumn kategorii 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Uzyskiwanie rozkładu dla kolumn liczbowych

    `select <column_name>, count(*) from <tablename> group by <column_name>`


###<a name="sql-featuregen"></a>Generowanie funkcji

W tej sekcji opisano sposoby tworzenia funkcji przy użyciu SQL:  

1. [Zliczanie w oparciu Generowanie funkcji](#sql-countfeature)
2. [Generowanie funkcji binning](#sql-binningfeature)
3. [Stopniowe się z funkcjami z pojedynczej kolumny](#sql-featurerollout)


> [AZURE.NOTE] Po wygenerować dodatkowe funkcje, możesz dodać je jako kolumny do istniejącej tabeli lub utworzyć nową tabelę z dodatkowe funkcje i klucz podstawowy, przyłączonym z pierwotnej tabeli. 

###<a name="sql-countfeature"></a>Zliczanie w oparciu Generowanie funkcji

W tym dokumencie przedstawiono dwa sposoby tworzenia funkcji ile.liczb. Pierwsza metoda jest używana funkcja suma warunkowe i druga metoda za pomocą klauzuli 'where'. Te można następnie łączyć z pierwotnej tabeli (za pomocą kolumny klucza podstawowego) Aby skorzystać z funkcji ile.liczb razem z pakietem oryginalne dane.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

###<a name="sql-binningfeature"></a>Generowanie funkcji binning

W poniższym przykładzie pokazano jak Generowanie binned funkcje według binning (za pomocą przedziałów 5) kolumny liczbowe, która może służyć jako funkcji zamiast tego:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


###<a name="sql-featurerollout"></a>Stopniowe się z funkcjami z pojedynczej kolumny

W tej sekcji możemy pokazano sposób wdrożenie pojedynczej kolumny w tabeli, aby wygenerować dodatkowe funkcje. W przykładzie założono, że w tabeli, z której chcesz wygenerować funkcje jest kolumną szerokości i długości geograficznej.

Oto krótkie Elementarz szerokość/długość geograficzna lokalizacja danych (z zasobami z zdarzeń stackoverflow [jak zmierzyć dokładność szerokości i długości geograficznej?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). To jest przydatne poznać przed featurizing pola lokalizacji:

- Znak informuje nam możemy czy północ i południe, wschód lub zachód na całym świecie.
- Niezerową setki cyfrowy informuje, nam użyto programu długości geograficznej, nie szerokości!
- Dziesiątki cyfr udostępnia możliwość około 1000 km. Daje nam przydatnych informacji o jaki kontynent lub jesteśmy w Oceanie.
- Cyfry jednostki (jeden stopień dziesiętną) daje pozycji do 111 km (60 Mila morska, około 69 mila). Go może napisać około jakie Państwa duży lub kraju, w którym w.
- Pierwsze miejsce dziesiętne warto maksymalnie 11.1 km: można odróżnić położenie dużych miast z sąsiednich dużych miasta.
- Drugiego miejsca dziesiętne warto maksymalnie 1.1 km: go oddzielić wieś jeden do kolejnego.
- Trzeciego miejsca dziesiętnego warto do 110 m, że można określić, duże pola rolnych lub kampusie instytucjonalnych.
- Czwartego miejsca dziesiętnego warto do 11 m, że można określić zbiorczym strukturą. Jest porównywalna do Typowa precyzja Niepoprawione jednostki GPS bez zakłóceń.
- Piątym dziesiętnego jest warte maksymalnie 1.1 m odróżnić drzew od siebie. Dokładność do tego poziomu w przypadku handlowych GPS uzyskuje się tylko z różnicy korekty.
- Szóstego miejsca dziesiętnego warto maksymalnie uwzględnieniu wartości 0,11 m, których można używać to dotyczące układu struktur szczegóły dotyczące projektowania krajobrazów, konstruowania drogi. Należy go więcej niż niewystarczający do śledzenia przemieszczania glaciers i rzeki. Można to osiągnąć, nie podejmując painstaking środków GPS, takich jak differentially poprawioną GPS.

Informacje o lokalizacji można następująco featurized, oddzielanie region, lokalizację i Miasto. Zwróć uwagę, można również wywołać punkt końcowy pozostałych, takich jak API mapy Bing jest dostępne w [Znajdź lokalizację o punkt](https://msdn.microsoft.com/library/ff701710.aspx) Aby uzyskać informacje o region i okręg.

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Powyżej funkcje opartych na lokalizacji dodatkowo umożliwia generowanie Statystyka dodatkowe funkcje opisane wcześniej. 


> [AZURE.TIP] Można wstawiać programowy rekordów przy użyciu języka wybór. Może być konieczne wstawić dane w fragmentów w celu zwiększenia efektywności zapisu (na przykład jak to zrobić przy użyciu pyodbc, zobacz [Przykładowe HelloWorld, aby uzyskać dostęp do programu z python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)). Alternatywnego jest wstawienie danych w bazie danych przy użyciu [narzędzia BCP](https://msdn.microsoft.com/library/ms162802.aspx).

###<a name="sql-aml"></a>Nawiązywanie połączenia z komputera Azure nauki

Funkcja wygenerowanym można dodane jako kolumny do istniejącej tabeli lub przechowywane w nowej tabeli i połączone z pierwotnej tabeli szkoleniowe na komputerze. Funkcje mogą być wygenerowane lub dostępne Jeśli już utworzony za pomocą [Importowanie danych] [ import-data] moduł w Azure maszynowego uczenia tak jak pokazano poniżej:

![czytniki azureml][1] 

##<a name="python"></a>Za pomocą języka programowania, takich jak Python

Eksplorowanie danych oraz generowanie funkcje, jeśli dane znajdują się w programie SQL Server przy użyciu Python jest podobna do przetwarzania danych w obiektów blob platformy Azure za pomocą Python opisane w [obiektów Blob platformy Azure proces dane można nauki środowiska danych](machine-learning-data-science-process-data-blob.md). Dane musi być ładowane z bazy danych do ramki danych pandas, a następnie mogą być dalej przetwarzane. Firma Microsoft dokumentu procesu nawiązywania połączenia z bazą danych i ładowania danych do ramki danych w tej sekcji.

Następujący format parametrów połączenia umożliwia nawiązywanie połączenia z bazą danych programu SQL Server z Python przy użyciu pyodbc (nazwa_serwera Zamień, dbname, nazwy użytkownika i hasła z z określonymi wartościami):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Biblioteka Pandas](http://pandas.pydata.org/) w Python przewiduje manipulowania danych programowania Python bogatego zestawu struktury danych i narzędzi do analizy danych. Kod poniżej otrzymuje wyniki zwrócone z bazy danych programu SQL Server w ramce danych Pandas:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Teraz możesz pracować z Pandas ramki danych objętych w artykule [obiektów Blob platformy Azure proces danych w przypadku danych naukowych środowisku](machine-learning-data-science-process-data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Nauka Azure danych w przykładzie akcji

Instruktaż kompleksowe — przykład procesu nauki danych Azure za pomocą publicznej zestawu danych zobacz [Proces nauki danych Azure w działaniu](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
