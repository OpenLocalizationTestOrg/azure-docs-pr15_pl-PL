<properties
    pageTitle="Tworzenie funkcji dla obiektów blob platformy Azure miejsca do magazynowania danych przy użyciu Panda | Microsoft Azure"
    description="Jak utworzyć funkcje związane z danymi przechowywanymi w kontenerze obiektów blob platformy Azure z pakietem Panda Python."
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
    ms.author="bradsev;garye" />

#<a name="create-features-for-azure-blob-storage-data-using-panda"></a>Tworzenie funkcji dla obiektów blob platformy Azure miejsca do magazynowania danych przy użyciu Panda

Ten dokument pokazano, jak utworzyć funkcje związane z danymi przechowywanymi w kontenerze obiektów blob platformy Azure za pomocą pakietu [Pandas](http://pandas.pydata.org/) Python. Po konspektu jak załadować dane do ramki danych Panda, jest wyświetlany jak wygenerować kategorii funkcji skryptów Python wartościami wskaźnik i binning funkcje.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Ten **menu** łączy do tematów dotyczących sposobu tworzenia funkcje dla danych w różnych środowiskach. To zadanie jest krok w [Zespołu danych nauki proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Wymagania wstępne

W tym artykule założono, że zostały utworzone konta magazyn obiektów blob platformy Azure i są przechowywane dane. Aby uzyskać instrukcje dotyczące konfigurowania konta, zobacz [Tworzenie konta magazynu platformy Azure](../storage/storage-create-storage-account.md#create-a-storage-account)


## <a name="load-the-data-into-a-pandas-data-frame"></a>Ładowanie danych do ramki danych Pandas
Aby eksplorować i manipulowania zestawu danych, jego musi zostać pobrany ze źródła obiektów blob w lokalnym pliku, który może zostać załadowana w ramce Pandas danych. Poniżej przedstawiono kroki wykonać tę procedurę, aby:

1. Pobieranie danych z Azure blob z następujący Python kod przy użyciu obiektów blob usługi. Zastąp określonych wartości zmiennej w poniższy kod:

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

##<a name="blob-featuregen"></a>Generowanie funkcji

Następnych dwóch sekcjach przedstawiono sposób generowanie kategorii z wartościami wskaźnik i binning funkcje Python skryptów.

###<a name="blob-countfeature"></a>Wartość wskaźnika podstawie Generowanie funkcji

Funkcje kategorii można utworzyć w następujący sposób:

1. Przeprowadzanie inspekcji rozkład kategorii kolumny:

        dataframe_blobdata['<categorical_column>'].value_counts()

2. Generowanie wskaźnika wartości dla każdej wartości kolumny

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Dołączanie do kolumny wskaźnik z oryginalną ramką danych

            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Usuń oryginalny samej zmiennej:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

###<a name="blob-binningfeature"></a>Generowanie funkcji binning

Do wygenerowania funkcje binned, możemy wykonać następujące czynności:

1. Dodawanie sekwencji kolumn do przedziałów kolumny liczbowe

        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)

2. Konwertowanie binning sekwencji zmiennych logiczna

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')

3. Na koniec połączenia fikcyjna zmienne oryginalną ramką danych

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

##<a name="sql-featuregen"></a>Zapisywania danych z powrotem do obiektów blob platformy Azure i zajmowanie się z informacjami komputera Azure

Po zostały zbadane dane i utworzony funkcji to konieczne, możesz przekazać danych (pobrane lub featurized) Azure blob i używanie go w Azure nauki komputera, wykonaj następujące czynności: należy zauważyć, że dodatkowe funkcje mogą być tworzone w Azure maszynowego uczenia Studio także.
1. Zapisywać ramki danych do pliku lokalnego

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Przekaż dane do obiektów blob platformy Azure w następujący sposób:

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

3. Teraz danych mogą być odczytywane z obiektów blob, za pomocą modułu Azure maszynowego uczenia [Importowanie danych](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) , jak pokazano na ekranie poniżej:

![Czytnik obiektów blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)
