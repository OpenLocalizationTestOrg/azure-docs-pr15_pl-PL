<properties
    pageTitle="Ustawianie i pobieranie właściwości i metadane dla obiektów w magazynie Azure | Microsoft Azure"
    description="Przechowywanie niestandardowe metadane dla obiektów w magazynie Azure i ustawianie i pobieranie właściwości systemu."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="set-and-retrieve-properties-and-metadata"></a>Ustawianie i pobieranie właściwości i metadane #

## <a name="overview"></a>Omówienie

Obiekty w właściwości systemu pomocy technicznej magazyn Azure i metadane zdefiniowane przez użytkownika, oprócz dane, które zawierają:

*   **Właściwości systemu.** Właściwości systemu istnieje dla każdego zasobu miejsca do magazynowania. Niektóre z nich można przeczytać lub ustawić, a inne są tylko do odczytu. W obszarze okładki niektóre właściwości systemu odpowiadają określonych standardowych nagłówków HTTP. Biblioteka klienta Azure magazynowania zachowuje są dla Ciebie.  

*   **Metadane zdefiniowane przez użytkownika.** Metadane zdefiniowane przez użytkownika są metadane Określ na danego zasobu w postaci pary wartości z pola Nazwa. Za pomocą metadanych do przechowywania wartości dodatkowych zasobów magazynowania; wartości te są tylko do celów własnego i nie wpływają na zachowanie tego zasobu.  

Pobieranie wartości właściwości i metadane dla zasobu miejsca do magazynowania jest procesem dwuetapowym. Przed rozpoczęciem czytania te wartości, jawnie należy je pobierać przez wywołanie metody **FetchAttributes** .

> [AZURE.IMPORTANT] Wartości właściwości i metadane dla zasobu miejsca do magazynowania nie są wypełnione, chyba że jedną z metod **FetchAttributes** połączeń. 

## <a name="setting-and-retrieving-properties"></a>Ustawianie i pobieranie właściwości

Aby pobrać wartości właściwości, wywołać metody **FetchAttributes** obiektów blob bądź w kontenerze do wypełniania właściwości, a następnie odczytać wartości.

Aby ustawić właściwości dla obiektu, określ wartość właściwości, a następnie wywołać metodę **SetProperties** .

W poniższym przykładzie tworzy kontenera i zapisuje niektóre z jego wartości właściwości w oknie konsoli:

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## <a name="setting-and-retrieving-metadata"></a>Ustawianie i pobieranie metadanych

Metadane można określić jako jeden lub więcej par wartości z pola Nazwa zasobu blob lub kontener. Aby ustawić metadanych, dodawanie pary nazwa wartość do zbierania **metadanych** od zasobu, a następnie wywołać metodę **SetMetadata** , aby zapisać wartości z usługą.

> [AZURE.NOTE] Nazwa metadanych musi spełniać konwencje nazewnictwa identyfikatorów C#.
 
W poniższym przykładzie ustawia metadanych w kontenerze. Jedną wartość przy użyciu metody **Add** kolekcji. Inne ma wartość przy użyciu składni niejawnej klucza/wartości. Obie są prawidłowe.

    public static void AddContainerMetadata(CloudBlobContainer container)
    {
        //Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        //Set the container's metadata.
        container.SetMetadata();
    }

Aby pobrać metadanych, wywołaj metodę **FetchAttributes** na obiektów blob lub kontener, aby wypełnić kolekcji **metadanych** , a następnie odczytać wartości, jak pokazano w poniższym przykładzie.

    public static void ListContainerMetadata(CloudBlobContainer container)
    {
        //Fetch container attributes in order to populate the container's properties and metadata.
        container.FetchAttributes();

        //Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }

## <a name="see-also"></a>Zobacz też  

- [Biblioteka klienta Azure miejsca do magazynowania dla odwołania .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [Azure Biblioteka klienta miejsca do magazynowania dla pakietu .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) 
