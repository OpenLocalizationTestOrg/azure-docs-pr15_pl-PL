<properties 
    pageTitle="Eksplorowanie danych w maszyn wirtualnych serwera SQL Azure | Microsoft Azure" 
    description="Jak Eksplorowanie danych, który jest przechowywany w maszyny serwera SQL Azure." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Eksplorowanie danych w maszyn wirtualnych serwera SQL Azure


W tym dokumencie opisano sposoby Eksplorowanie danych, który jest przechowywany w maszyny serwera SQL Azure. Można to zrobić, wrangling danych za pomocą SQL albo za pomocą języka programowania, takich jak Python.

Następujące **menu** łącza do tematów opisujących, jak za pomocą narzędzia Eksplorowanie danych z różnych środowiskach miejsca do magazynowania. To zadanie jest krokiem w Cortana proces analizy (zakończenie).

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


> [AZURE.NOTE] Przykładowe instrukcji SQL w tym dokumencie przyjęto założenie, że dane są w programie SQL Server. Jeśli nie, zapoznaj się z mapy procesu chmury danych naukowych, aby dowiedzieć się, jak przenieść dane programu SQL Server.



## <a name="sql-dataexploration"></a>Eksplorowanie danych SQL za pomocą skryptów SQL

Poniżej przedstawiono kilka przykładowe skrypty SQL, które mogą być używane Eksplorowanie magazynów danych w programie SQL Server.

1. Zliczanie uwagi na dzień

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Uzyskiwanie poziomów w kategorii kolumny

    `select  distinct <column_name> from <databasename>`

3. Uzyskiwanie liczbę poziomów w połączeniu z dwóch kolumn kategorii 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Uzyskiwanie rozkładu dla kolumn liczbowych

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] Na przykład wszystkie stosowne można użyć [taksówki Warszawa zestawu danych](http://www.andresmh.com/nyctaxitrips/) i odwołują się do IPNB zatytułowany [wrangling Warszawa danych przy użyciu programu SQL Server i notesów IPython](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) dla samouczek zakończenia do końca.

##<a name="python"></a>Eksplorowanie danych SQL z Python

Eksplorowanie danych oraz generowanie funkcje, jeśli dane znajdują się w programie SQL Server przy użyciu Python jest podobna do przetwarzania danych w obiektów blob platformy Azure za pomocą Python, opisane w [danych obiektów Blob platformy Azure proces w środowisku nauki danych](machine-learning-data-science-process-data-blob.md). Dane musi być ładowane z bazy danych do pandas DataFrame, a następnie mogą być dalej przetwarzane. Firma Microsoft dokumentu procesu nawiązywania połączenia z bazą danych i ładowania danych do DataFrame w tej sekcji.

W następującym formacie parametry połączenia umożliwia nawiązywanie połączenia z bazą danych programu SQL Server z Python przy użyciu pyodbc (nazwa_serwera Zamień, dbname, nazwę użytkownika i hasło z określonej wartości):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Biblioteka Pandas](http://pandas.pydata.org/) w Python przewiduje manipulowania danych programowania Python bogatego zestawu struktury danych i narzędzi do analizy danych. Poniższy kod otrzymuje wyniki zwrócone z bazy danych programu SQL Server w ramce danych Pandas:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Teraz możesz pracować z Pandas DataFrame jak to opisano w temacie [obiektów Blob platformy Azure proces danych w środowisku nauki danych](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>Proces analizy Cortany w przykładzie akcji

Przykład Instruktaż zakończenia do końca proces analizy Cortana przy użyciu publicznego zestawu danych, zobacz [proces nauki danych zespołu w działaniu: przy użyciu programu SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

 
