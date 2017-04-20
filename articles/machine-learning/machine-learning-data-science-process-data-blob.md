<properties 
    pageTitle="Przetwarzanie danych typu blob Azure Zaawansowana analiza danych | Microsoft Azure" 
    description="Przetwarzanie danych w magazynu obiektów Blob." 
    services="machine-learning,storage" 
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

#<a name="heading"></a>Przetwarzanie danych typu blob Azure Zaawansowana analiza danych

W tym dokumencie opisano eksplorowania danych i funkcji służących do wytwarzania z danych przechowywanych w magazynu obiektów Blob. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Ładowanie danych do ramki danych Pandy
W celu zbadania i manipulować zestawu danych, to muszą być pobrane ze źródła obiektu blob do pliku lokalnego, które następnie mogą być ładowane w ramce danych Pandy. Poniżej przedstawiono kroki wykonać tę procedurę, aby:

1. Pobierz dane z Azure blob z następujący przykładowy kod Python za pomocą usługi blob. Zastąp określonych wartości zmiennej w kodzie poniżej: 

        from azure.storage.blob import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))


2. Odczytać danych do Pandy danych ramki z pobranego pliku.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Teraz można przystąpić do eksplorowania danych i wygenerować funkcje na ten zestaw danych.


##<a name="blob-dataexploration"></a>Eksploracji danych

Oto kilka przykładów przedstawiających sposoby eksplorowania danych przy użyciu Panda:

1. Sprawdź liczbę wierszy i kolumn 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. Należy sprawdzić pierwszy lub ostatni kilka wierszy w zestawie danych, jak pokazano poniżej:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Sprawdź typ danych, które każda kolumna została zaimportowane jako następujący przykładowy kod za pomocą
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Sprawdź następujące podstawowe statystyki dla kolumn w zestawie danych
 
        dataframe_blobdata.describe()
    
5. Sprawdź liczbę wpisów dla każdej wartości kolumny w następujący sposób

        dataframe_blobdata['<column_name>'].value_counts()

6. Liczba brakujących wartości, a rzeczywista liczba wpisów w każdej kolumnie przy użyciu następujący przykładowy kod

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Jeśli masz brakujących wartości dla określonej kolumny danych, można upuścić je w następujący sposób:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Innym sposobem zastąpić brakujące wartości jest z funkcją trybu:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Utworzyć wykres histogramu przy użyciu zmiennej liczbie pojemników do wykreślenia rozkład zmiennej 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Przyjrzyj się korelacje między zmiennymi przy użyciu Wykres rozrzutu lub przy użyciu funkcji wbudowanych korelacji

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    
    
##<a name="blob-featuregen"></a>Funkcja generowania
    
Możemy wygenerować funkcji przy użyciu Python w następujący sposób:

###<a name="blob-countfeature"></a>Funkcja generowania na podstawie wartości wskaźnika

Cechy jakościowe mogą być tworzone w następujący sposób:

1. Sprawdzić rozkład kategorii kolumny:
    
        dataframe_blobdata['<categorical_column>'].value_counts()

2. Generowanie wskaźnika wartości dla każdej z wartości w kolumnie

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Dołącz wskaźnik kolumna z oryginalną ramką danych 
 
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Usunięcie samej zmiennej oryginalny:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)
    
###<a name="blob-binningfeature"></a>Funkcja generowania odstępach

Do generowania funkcje binned, możemy postępować następująco:

1. Dodanie sekwencji kolumny do kolumny liczbowej pojemnika
 
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
        
2. Konwertowanie odstępach sekwencję zmienne typu boolean

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
    
3. Wreszcie Dołącz do manekina zmiennych powrót do oryginalnych ramki danych

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool) 


##<a name="sql-featuregen"></a>Zapisaniem danych z powrotem na Azure blob i używające analityków

Po zostały zbadane danych i utworzyć niezbędne funkcje, możesz przesłać dane (partia lub modeli) Azure obiektu blob i wykorzystywać go w usługą sieci Web, wykonując następujące czynności: należy zauważyć, że funkcje dodatkowe mogą być tworzone w omówienie jak również. 
1. Napisz ramki danych do lokalnego pliku

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Przesłać dane do obiektu blob Azure w następujący sposób:

        from azure.storage.blob import BlobService
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

3. Teraz dane mogą być odczytywane z obiektu blob, korzystając z usługą sieci Web [Importowanie danych] [ import-data] modułu, jak pokazano na poniższym ekranie:
 
![Czytnik typu blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
