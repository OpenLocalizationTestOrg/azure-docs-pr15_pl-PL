<properties 
    pageTitle="Przykładowe dane w programie SQL Server Azure | Microsoft Azure" 
    description="Przykładowe dane w programie SQL Server Azure" 
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
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Przykładowe dane w programie SQL Server Azure


Ten dokument zamieszczono przykładowe dane przechowywane w programie SQL Server Azure za pomocą SQL lub języka programowania Python. Ponadto przedstawia sposób przenoszenia danych próbki do nauki maszynowego Azure, zapisując go w pliku, przekazania go do obiektów blob platformy Azure i czytając do Azure maszynowego uczenia Studio.

Przy próbkowaniu Python korzysta z biblioteki ODBC [pyodbc](https://code.google.com/p/pyodbc/) nawiązać połączenia z programem SQL Server Azure i biblioteki [Pandas](http://pandas.pydata.org/) pobrania próbek.

>[AZURE.NOTE] Przykładowy kod SQL w tym dokumencie przyjęto założenie, że dane są w programie SQL Server Azure. Jeśli nie jest dostępne, zapoznaj się [Przenoszenie danych programu SQL Server Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) tematu, aby uzyskać instrukcje dotyczące przenoszenie danych do programu SQL Server Azure.

**Dlaczego przykładowych danych?**
Jeśli zestaw danych, które mają być analizowanie jest duży, zazwyczaj jest dobrym pomysłem próby szczegółów danych, aby zmniejszyć go o rozmiarze mniejszym, ale przedstawiciel oraz łatwiejsze. Ułatwia to opis danych, badań i inżynierskie funkcji. Jego rolę w [Zespołu danych nauki proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) jest umożliwienie szybkie tworzenie prototypów funkcji przetwarzania danych i maszynowego uczenia modeli.

**Menu** poniższych łączy do tematów, w których opisano przykładowe dane z różnych środowiskach miejsca do magazynowania. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

To zadanie pobierania jest krok w [Zespołu danych nauki proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

##<a name="SQL"></a>Za pomocą SQL

W tej sekcji opisano kilka metod wykonać pobieranie próbek losowych z danymi w bazie danych przy użyciu polecenia SQL. Wybierz metodę opartą na danych, rozmiar i jego dystrybucji.

Dwa elementy poniżej przedstawiono sposoby używania newid w programie SQL Server do wykonywania próbki. Wybór metody zależy od tego, jak przypadkowe ma próbki w celu (pk_id w kodzie przykładowych poniżej zostanie potraktowany jako klucz podstawowy generowane automatycznie).

1. Mniej rygorystyczne losowo

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. Więcej losowo 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample może być używana do pobierania podobnie jak pokazano poniżej. Może to być lepszym rozwiązaniem, jeśli rozmiar danych jest duży (zakładając, że nie jest powiązane dane na różnych stronach) i kwerendy do wykonania w odpowiednim czasie.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] Można eksplorować i generowanie funkcje z próbki danych, zapisując go w nowej tabeli


###<a name="sql-aml"></a>Nawiązywanie połączenia z komputera Azure nauki

Przykładowe kwerendy powyżej można używać bezpośrednio w Azure maszynowego uczenia [Importowanie danych] [ import-data] moduł próbkami szczegółów danych w czasie rzeczywistym i dostosowania go do doświadczenia Azure maszynowego uczenia. Zrzut ekranu: Korzystanie z modułu czytnika odczytać próbki danych przedstawiono poniżej:
   
![Czytnik sql][1]

##<a name="python"></a>Za pomocą Python języka programowania 

W tej sekcji przedstawiono za pomocą [biblioteki pyodbc](https://code.google.com/p/pyodbc/) w celu ustanowienia ODBC nawiązywanie połączenia z bazą danych programu SQL server w Python. Parametry połączenia bazy danych jest następująca: (zamiast nazwa_serwera, dbname, nazwę użytkownika i hasło z konfiguracją):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Biblioteka [Pandas](http://pandas.pydata.org/) w Python przewiduje manipulowania danych programowania Python bogatego zestawu struktury danych i narzędzi do analizy danych. Kod poniżej otrzymuje próbki 0,1% danych z tabeli w bazie danych Azure SQL w danych Pandas:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Teraz można pracować z danymi próbki w ramce danych Pandas. 

###<a name="python-aml"></a>Nawiązywanie połączenia z komputera Azure nauki

Poniższy przykład kodu umożliwia zapisywanie danych pobrane w dół do pliku, a następnie przekazanie go do obiektów blob platformy Azure. Dane w to można bezpośrednio odczytywać do doświadczenia nauki Azure komputera przy użyciu [Importowanie danych] [ import-data] modułu. Dostępne są następujące czynności: 

1. Pisanie pandas ramki danych do lokalnego pliku

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Przekaż plik lokalny do obiektów blob platformy Azure

        from azure.storage import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Odczytywanie danych z obiektów blob platformy Azure za pomocą Azure maszynowego uczenia [Importowanie danych] [ import-data] modułu, jak pokazano poniżej Przechwyć ekran:
 
![Czytnik obiektów blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Proces zespołu danych naukowych w przykładzie akcji

Na przykład Instruktaż zakończenia do końca procesu nauki danych zespołu publicznej zestawu danych przy użyciu zobacz [proces nauki danych zespołu w działaniu: za pomocą SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 [import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
