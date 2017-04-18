<properties 
    pageTitle="Eksplorowanie danych w magazynie obiektów blob platformy Azure z Pandas | Microsoft Azure" 
    description="Jak Eksplorowanie danych, który jest przechowywany w kontenerze obiektów blob platformy Azure za pomocą Pandas." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-azure-blob-storage-with-pandas"></a>Eksplorowanie danych w magazynie obiektów blob platformy Azure z Pandas

W tym dokumencie opisano sposoby Eksplorowanie danych, który jest przechowywany w kontenerze obiektów blob platformy Azure za pomocą pakietu [Pandas](http://pandas.pydata.org/) Python.

Następujące **menu** łącza do tematów opisujących, jak za pomocą narzędzia Eksplorowanie danych z różnych środowiskach miejsca do magazynowania. To zadanie jest krokiem [Procesu nauki danych]().

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule założono, że:

* Utworzone konto Azure miejsca do magazynowania. Aby uzyskać instrukcje, zobacz [Tworzenie konta magazynu platformy Azure](../storage/storage-create-storage-account.md#create-a-storage-account)
* Dane przechowywane w konta magazyn obiektów blob platformy Azure. Aby uzyskać instrukcje, zobacz [Przenoszenie danych do i z miejsca do magazynowania Azure](../storage/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Ładowanie danych do Pandas DataFrame
Eksplorowanie i manipulowania zestawu danych, go należy najpierw można pobrać ze źródła obiektów blob do lokalnego pliku, który może zostać załadowana w Pandas DataFrame. Poniżej przedstawiono kroki wykonać tę procedurę, aby:

1. Pobieranie danych z Azure blob z Poniższy przykładowy kod Python przy użyciu obiektów blob usługi. Zastąp określonych wartości zmiennej poniższy kod: 

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


2. Dane wczytywane do Pandas danych — ramki pochodzącej z pobrany plik.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Teraz możesz przystąpić do Eksplorowanie danych oraz generowanie funkcje w tym zestawie danych.

##<a name="blob-dataexploration"></a>Przykłady badań danych przy użyciu Pandas

Oto kilka przykładów przedstawiających sposoby Eksplorowanie danych za pomocą Pandas:

1. Przeprowadzanie inspekcji **liczbę wierszy i kolumn** 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. **Inspekcja** pierwszego lub ostatniego kilka **wierszy** w zestawie danych następujących czynności:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Zaznacz odpowiedni **Typ danych** każdej kolumny zaimportowano jak przy użyciu następujący kod
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Zaznacz następujące **podstawowe statystyki** dla kolumn w zestawie danych
 
        dataframe_blobdata.describe()
    
5. Przyjrzyj się liczba wpisów dla każdej wartości w kolumnie w następujący sposób

        dataframe_blobdata['<column_name>'].value_counts()

6. **Brakujące wartości Liczba** rzeczywista liczba wpisów w każdej kolumnie przy użyciu następujący kod

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Jeśli **brakujące wartości** dla określonej kolumny danych, możesz upuść je w następujący sposób:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Innym sposobem Zamień brakujące wartości jest funkcją trybu:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Tworzenie wykresu **histogramu** przy użyciu zmienna liczba przedziałów kreślenia rozkład zmiennej 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Przyjrzyj się **korelacji** między zmiennymi przy użyciu scatterplot lub przy użyciu funkcji wbudowanych korelacji

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
