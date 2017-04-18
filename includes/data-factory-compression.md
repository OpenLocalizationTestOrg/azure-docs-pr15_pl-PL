### <a name="compression-support"></a>Obsługa kompresji  
Przetwarzanie dużych zestawów danych mogą powodować gardeł We/Wy i sieci. W związku z tym skompresowane dane w sklepach można nie tylko przyspieszyć transfer danych przez sieć zaoszczędzić miejsce na dysku, a także wyświetlić znaczący wzrost wydajności przetwarzania danych duży. Obecnie kompresja jest obsługiwana dla magazynów opartych na plikach danych takich jak obiektów Blob platformy Azure lub lokalnego systemu plików.  

> [AZURE.NOTE] Ustawienia kompresji nie są obsługiwane dla danych w **AvroFormat**, **OrcFormat**lub **ParquetFormat**. 

Aby określić stopień kompresji zestawu danych, należy użyć właściwości **kompresji** w zestawie danych JSON, tak jak w poniższym przykładzie:   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
W sekcji **kompresji** ma dwie właściwości:  
  
- **Typ:** kompresji koder-dekoder, który może być **GZIP**i **Deflate** **BZIP2**.  
- **Poziom:** stopień kompresji, który może być **optymalna** lub **najszybciej**. 
    - **Najszybszy:** Operacji kompresowania należy ukończyć tak szybko, jak to możliwe, nawet wtedy, gdy utworzony plik nie jest skompresowany optymalnie. 
    - **Optymalna**: operacji kompresowania powinny być optymalnie kompresowany, nawet w przypadku operacji trwa dłużej. 
    
    Aby uzyskać więcej informacji zobacz temat [Poziom kompresji](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) . 

Załóżmy, że powyższego zestawu danych przykładowych jest używany jako wynik działań podjętych w ramach kopii, działania kopii kompresuje danych wyjściowych z GZIP kodera-dekodera, za pomocą optymalny stosunek, a następnie wpisz skompresowane dane w pliku o nazwie pagecounts.csv.gz w magazynie obiektów Blob platformy Azure.   

Po określeniu właściwości kompresji w zestawie danych wejściowych JSON proces można przeczytać skompresowane dane ze źródła i określić właściwość zestaw danych wyjściowych JSON, działania kopii można napisać skompresowane dane do miejsca docelowego. Poniżej przedstawiono kilka przykładowych scenariuszy: 

- Skompresowany GZIP odczytu danych z obiektów blob platformy Azure wyodrębnić go i zapisać wynik danych do bazy danych programu Azure SQL. W tym przypadku należy zdefiniować wprowadzania zestawu obiektów Blob platformy Azure danych z kompresji właściwość JSON. 
- Odczytywanie danych z pliku tekstowego z lokalnego systemu plików, go w formacie GZip skompresować i do obiektów blob platformy Azure zapisu skompresowane dane. W tym przypadku należy zdefiniować zestaw obiektów Blob platformy Azure dane wyjściowe z kompresji właściwość JSON.  
- Odczyt danych skompresowany GZIP obiektów blob platformy Azure, wyodrębnić go, kompresowanie go przy użyciu BZIP2 i zapisać dane wynikowe do obiektów blob platformy Azure. Wprowadzania zestawu obiektów Blob platformy Azure danych do definiowania typu kompresji ustawionym na wartość GZIP i dane wyjściowe zestawu danych w tym przypadku ustawieniu BZIP2 typu kompresji.   
