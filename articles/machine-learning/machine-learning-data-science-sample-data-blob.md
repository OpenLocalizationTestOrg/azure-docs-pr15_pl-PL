<properties 
    pageTitle="Przykładowe dane w Azure blob miejsca do magazynowania | Microsoft Azure" 
    description="Przykładowe dane w magazynie obiektów Blob platformy Azure" 
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

#<a name="heading"></a>Przykładowe dane w Azure blob miejsca do magazynowania


W tym dokumencie opisano przy próbkowaniu dane przechowywane w magazynie obiektów blob platformy Azure, pobierając ją programowy i pobierania za pomocą procedury napisane w Python próbek.

**Dlaczego przykładowych danych?**
Jeśli zestaw danych, które mają być analizowanie jest duży, zazwyczaj jest dobrym pomysłem próby szczegółów danych, aby zmniejszyć go o rozmiarze mniejszym, ale przedstawiciel oraz łatwiejsze. Ułatwia to opis danych, badań i inżynierskie funkcji. Jego rolę w procesie analizy Cortana jest umożliwienie szybkie tworzenie prototypów funkcji przetwarzania danych i maszynowego uczenia modeli.

**Menu** poniższych łączy do tematów, w których opisano przykładowe dane z różnych środowiskach miejsca do magazynowania. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

To zadanie pobierania jest krok w [Zespołu danych nauki proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="download-and-down-sample-data"></a>Plik do pobrania i w dół przykładowych danych
1. Pobieranie danych z magazynem obiektów blob Azure za pomocą usługi obiektów blob z następujący kod Python: 

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

2. Dane wczytywane do Pandas-ramki danych z pliku pobierane powyżej.

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Próbkami szczegółów danych przy użyciu `numpy`i `random.choice` w następujący sposób:

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Teraz możesz pracować z powyższych ramki danych z przykładowymi 1 procent dla funkcji Generowanie i uzyskać więcej informacji.

##<a name="heading"></a>Przekazywanie danych i czytać do nauki maszynowego Azure

Poniższy przykład kodu umożliwia dół przykładowych danych i używać go bezpośrednio w Azure ML:

1. Pisanie ramki danych do lokalnego pliku

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Przekaż plik lokalny do obiektów blob platformy Azure za pomocą następujący kod:

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
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. Przeczytaj dane z obiektów blob platformy Azure za pomocą Azure ML [Importowanie danych](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) , jak pokazano na poniższej ilustracji:
 
![Czytnik obiektów blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

 
