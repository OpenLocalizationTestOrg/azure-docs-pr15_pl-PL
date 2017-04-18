<properties 
    pageTitle="Wywołaj programy Spark z fabryki danych Azure" 
    description="Dowiedz się, jak wywołać programy Spark z fabryki Azure danych przy użyciu aktywności MapReduce." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>Wywołaj programy Spark z fabryki danych
## <a name="introduction"></a>Wprowadzenie
Działanie MapReduce w potoku Factory danych służy do uruchamiania programów Spark w klastrze HDInsight Spark. Zobacz artykuł [Aktywności MapReduce](data-factory-map-reduce.md) Aby uzyskać szczegółowe informacje na temat korzystania z działania przed przeczytaniem tego artykułu. 

## <a name="spark-sample-on-github"></a>Przykładowe Spark na GitHub
[Spark - Factory danych przykładowych na GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) przedstawiono działanie MapReduce służy do wywołania programu Spark. Spark program kopiuje tylko dane z jeden kontener obiektów Blob platformy Azure innemu użytkownikowi. 

## <a name="data-factory-entities"></a>Obiekty Factory danych
Folder **Spark-ADF-src-ADFJsons** zawiera pliki dla obiektów Factory danych (usługi połączone, zestawy danych, potoku).  

Istnieją dwa **połączone usług** w tym przykładzie: Usługa Azure HDInsight i magazynowania Azure. Określ nazwę magazynu Azure i wartości klucza usługi w **StorageLinkedService.json** i clusterUri, nazwy użytkownika i hasła w **HDInsightLinkedService.json**.

Istnieją dwa **zestawy danych** w tym przykładzie: **input.json** i **output.json**. Te pliki znajdują się w folderze **zestawy danych** .  Te pliki reprezentują wejściowe i wyjściowe baz danych dla aktywności MapReduce

Przykładowe procesy można znaleźć w folderze **ADFJsons i planowana** . Przejrzyj potok, aby dowiedzieć się, jak wywołać programu Spark przy użyciu aktywności MapReduce. 

Działanie MapReduce jest skonfigurowany do wywołania **com.adf.sparklauncher.jar** w kontenerze **adflibs** w Twoim magazynie Azure (określone w StorageLinkedService.json). Kod źródłowy ten program jest w iskrowym-ADF-src główne java-com i adf/nawiąże połączenie z folderu i przesyłanie spark i uruchamianie Spark zadania. 

> [AZURE.IMPORTANT] 
> Zapoznaj się z [— Plik README. TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt) uzyskać najnowszą i dodatkowe informacje przed użyciem próbki. 
>  
> Za pomocą klaster HDInsight Spark tej metody można wywołać Spark programów używających aktywności MapReduce. Używanie klastrze HDInsight na żądanie nie jest obsługiwane.   


## <a name="see-also"></a>Zobacz też
- [Gałąź aktywności](data-factory-hive-activity.md)
- [Świnka aktywności](data-factory-pig-activity.md)
- [Działanie MapReduce](data-factory-map-reduce.md)
- [Hadoop strumienia aktywności](data-factory-hadoop-streaming-activity.md)
- [Wywoływanie skryptów R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
