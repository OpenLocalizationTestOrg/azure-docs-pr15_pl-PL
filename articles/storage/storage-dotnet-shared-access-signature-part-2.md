<properties
    pageTitle="Tworzenie i używanie skojarzeń zabezpieczeń z magazynem obiektów Blob | Microsoft Azure"
    description="W tym samouczku pokazano, jak tworzenie podpisów udostępnienia do użytku z magazynem obiektów Blob i jak je korzystania z aplikacji do klienta."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="shared-access-signatures-part-2-create-and-use-a-sas-with-blob-storage"></a>Udostępniony podpisy programu Access, część 2: Tworzenie i używanie skojarzeń zabezpieczeń z magazynem obiektów Blob

## <a name="overview"></a>Omówienie

[Część 1](storage-dotnet-shared-access-signature-part-1.md) tego samouczka zbadane udostępnione podpisów programu access (SA) i wyjaśnione najważniejsze wskazówki dotyczące korzystania z nich. Część 2 pokazano, jak wygenerować, a następnie za pomocą podpisów udostępniania z magazynem obiektów Blob. Przykłady są zapisywane w języku C# i korzystanie z biblioteki klienta Azure miejsca do magazynowania dla środowiska .NET. Scenariusze, w których są te aspekty pracy z podpisami udostępniania:

- Generowanie podpisu udostępniania w kontenerze
- Generowanie podpisu udostępniania obiektów blob
- Tworzenie zasady przechowywane dostępu do zarządzania podpisów kontenera zasobów
- Testowanie podpisów udostępniania z aplikacji klienckiej

## <a name="about-this-tutorial"></a>Informacje o tym samouczku

W tym samouczku poświęconym tworzenia podpisów udostępniania dla kontenerów i obiektów blob przez utworzenie dwóch aplikacji konsoli. Pierwszy aplikacji konsoli generuje udostępnienia podpisów w kontenerze i obiektów blob. Ta aplikacja ma informacji na temat kluczy konta miejsca do magazynowania. Druga aplikacji konsoli będzie działał jako aplikacja klienta, uzyskuje dostęp do kontenera i zasoby obiektów blob korzystanie z podpisów udostępnienia utworzone za pomocą pierwszej aplikacji. Ta aplikacja używa podpisów udostępniania tylko do uwierzytelniania dostępu do kontenera i blob zasobów — nie ma wiedzy kluczy konta.

## <a name="part-1-create-a-console-application-to-generate-shared-access-signatures"></a>Część 1: Tworzenie aplikacji konsoli do generowania podpisów udostępniania

Najpierw upewnij się, że masz Biblioteka klienta Azure miejsca do magazynowania dla .NET jest już zainstalowany. Możesz zainstalować [pakiet NuGet](http://nuget.org/packages/WindowsAzure.Storage/ "NuGet pakiet") zawierający aktualną zestawów dla biblioteki klienta; jest to zalecane metoda za zapewnienie, że masz najnowsze poprawki. Możesz również pobrać Biblioteka klienta w ramach najnowszej wersji [SDK Azure dla środowiska .NET](https://azure.microsoft.com/downloads/).

W programie Visual Studio Tworzenie nowej aplikacji konsoli systemu Windows i nadaj mu nazwę **GenerateSharedAccessSignatures**. Dodawanie odwołania do **Microsoft.WindowsAzure.Configuration.dll** i **Microsoft.WindowsAzure.Storage.dll**, przy użyciu jednej z następujących metod:

-   Jeśli chcesz zainstalować pakiet NuGet najpierw zainstalować [Klienta NuGet](https://docs.nuget.org/consume/installing-nuget). W programie Visual Studio, wybierz pozycję **projektu | Zarządzanie pakietów NuGet**, Wyszukaj w trybie online **Magazyn Azure**i postępuj zgodnie z instrukcjami instalacji.
-   Możesz też umieszczać je w instalacji pakietu Azure SDK i dodaj odwołania do nich.

U góry pliku Plik Program.cs Dodaj następujące instrukcje **za pomocą** :

    using System.IO;    
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

Edytowanie pliku app.config tak, aby zawierał ustawienie konfiguracji przy użyciu parametrów połączenia, która kieruje do swojego konta miejsca do magazynowania. Plik app.config powinna wyglądać podobnie do tego:

    <configuration>
      <startup>
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
      </startup>
      <appSettings>
        <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey"/>
      </appSettings>
    </configuration>

### <a name="generate-a-shared-access-signature-uri-for-a-container"></a>Generowanie podpisu udostępnienia URI do kontenera

Najpierw będziemy dodawać metody do generowania podpisu udostępniania na nowy kontener. W tym przypadku podpis nie jest skojarzone z zasadę dostępu przechowywane, więc prowadzi identyfikatora URI informacji wskazująca jego czasu wygaśnięcia i uprawnień, które udziela uprawnień.

Najpierw dodaj kod do **Main()** metodę uwierzytelniania dostępu do konta miejsca do magazynowania i utworzyć nowy kontener:

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Insert calls to the methods created below here...

        //Require user input before closing the console window.
        Console.ReadLine();
    }

Następnie dodaj nową metodę generuje podpis udostępnienia kontenera, który zwraca podpis identyfikatora URI:

    static string GetContainerSasUri(CloudBlobContainer container)
    {
        //Set the expiry time and permissions for the container.
        //In this case no start time is specified, so the shared access signature becomes valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List;

        //Generate the shared access signature on the container, setting the constraints directly on the signature.
        string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

Dodaj następujące wiersze w dolnej części metodę **Main()** przed połączenie **Console.ReadLine()**, aby nawiązać połączenie **GetContainerSasUri()** i wpisz podpis identyfikatora URI w oknie konsoli:

    //Generate a SAS URI for the container, without a stored access policy.
    Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
    Console.WriteLine();

Skompilować i uruchomić w celu wyprowadzenia podpisu udostępnienia identyfikatora URI dla nowego kontenera. Identyfikator URI będzie wyglądać podobnie do następujących identyfikatora URI:       

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2013-04-13T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3D

Po uruchomieniu kodu utworzony w kontenerze podpis udostępnienia będzie prawidłowy następnego 24 godziny. Podpis udziela uprawnienia klienta, aby liście BLOB w kontenerze i tworzenia nowych obiektów blob w kontenerze.

### <a name="generate-a-shared-access-signature-uri-for-a-blob"></a>Generowanie podpisu udostępnienia identyfikatora URI dla obiektów Blob

Następnie firma Microsoft będzie wpisz kod podobny do tworzenia nowych obiektów blob w kontenerze i Generowanie podpisu udostępniania dla niego. Podpis udostępniania nie jest skojarzone z zasadę dostępu przechowywane, więc zawiera czas rozpoczęcia, czas wygaśnięcia i informacji o uprawnieniach identyfikatora URI.

Dodawanie nowej metody, która umożliwia utworzenie nowego obiektów blob i zapisu fragment tekstu, a następnie generuje podpis udostępniania i zwraca podpis identyfikatora URI:

    static string GetBlobSasUri(CloudBlobContainer container)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblob.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a Shared Access Signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Set the expiry time and permissions for the blob.
        //In this case the start time is specified as a few minutes in the past, to mitigate clock skew.
        //The shared access signature will be valid immediately.
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();
        sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
        sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
        sasConstraints.Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write;

        //Generate the shared access signature on the blob, setting the constraints directly on the signature.
        string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

U dołu metodę **Main()** Dodaj następujące wiersze połączenie **GetBlobSasUri()**przed połączenie **Console.ReadLine()**i wpisz podpis udostępnienia identyfikatora URI w oknie konsoli:    

    //Generate a SAS URI for a blob within the container, without a stored access policy.
    Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
    Console.WriteLine();


Skompilować i uruchomić w celu wyprowadzenia podpisu udostępnienia identyfikatora URI dla nowych obiektów blob. Identyfikator URI będzie wyglądać podobnie do następujących identyfikatora URI:

    https://storageaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2012-02-12&st=2013-04-12T23%3A37%3A08Z&se=2013-04-13T00%3A12%3A08Z&sr=b&sp=rw&sig=dF2064yHtc8RusQLvkQFPItYdeOz3zR8zHsDMBi4S30%3D

### <a name="create-a-stored-access-policy-on-the-container"></a>Tworzenie zasad dostępu przechowywanych w kontenerze

Teraz tworzenie zasadę dostępu przechowywanych w kontenerze, określających ograniczenia dla dowolnego podpisy udostępniania, które są skojarzone z nim.

W poprzednich przykładach możemy określonej godziny rozpoczęcia (niejawnie lub jawnie), czas wygaśnięcia i uprawnienia na podpis udostępnienia URI się. W poniższych przykładach określono te zasady dostępu przechowywane, a nie na podpis udostępniania. Ten sposób pozwala na zmienianie tych ograniczeń bez ponownego podpisu udostępniania.

Użytkownik może mieć jedną lub więcej ograniczeń dotyczących podpisu udostępniania i reszta zasady dostępu przechowywane. Jednak można określić tylko czas rozpoczęcia, czas wygaśnięcia i uprawnienia w jednym miejscu lub w drugim; Nie można na przykład określanie uprawnień udostępniania podpisu i je także określić zasady dostępu przechowywane.

Należy zauważyć, że po dodaniu zasady dostępu do kontenera możesz musi uzyskać uprawnienia istniejącego kontenera, Dodaj nową zasadę dostępu, a następnie ustaw uprawnienia kontenera.

Dodaj nową metodę tworzy nową zasadę dostępu przechowywanych w kontenerze i zwraca nazwę zasad:

    static void CreateSharedAccessPolicy(CloudBlobClient blobClient, CloudBlobContainer container,
        string policyName)
    {
        //Create a new shared access policy and define its constraints.
        SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
        {
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List | SharedAccessBlobPermissions.Read
        };

        //Get the container's existing permissions.
        BlobContainerPermissions permissions = container.GetPermissions();

        //Add the new policy to the container's permissions, and set the container's permissions.
        permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
        container.SetPermissions(permissions);
    }

W dolnej części metodę **Main()** przed połączenie **Console.ReadLine()**Dodaj następujące wiersze do pierwszej Wyczyść wszelkie istniejące zasady dostępu, a następnie zadzwoń do metody **CreateSharedAccessPolicy()** :    

    //Clear any existing access policies on container.
    BlobContainerPermissions perms = container.GetPermissions();
    perms.SharedAccessPolicies.Clear();
    container.SetPermissions(perms);

    //Create a new access policy on the container, which may be optionally used to provide constraints for
    //shared access signatures on the container and the blob.
    string sharedAccessPolicyName = "tutorialpolicy";
    CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

Należy zauważyć, że po wyczyszczeniu zasady dostępu w kontenerze należy najpierw uzyskać kontenera istniejące uprawnienia, a następnie wyczyść uprawnienia, a następnie ponownie ustawić uprawnienia.

### <a name="generate-a-shared-access-signature-uri-on-the-container-that-uses-an-access-policy"></a>Generowanie podpisu udostępnienia URI w kontenerze, która używa zasady dostępu

Następnie utworzymy innym podpisem udostępniania w kontenerze utworzone wcześniej, ale tym razem będzie powiązane podpisu z zasady dostępu, utworzony w poprzednim przykładzie.

Dodawanie nowej metody do generowania innego podpisu udostępniania w kontenerze:

    static string GetContainerSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Generate the shared access signature on the container. In this case, all of the constraints for the
        //shared access signature are specified on the stored access policy.
        string sasContainerToken = container.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return container.Uri + sasContainerToken;
    }

W dolnej części metodę **Main()** przed połączenie **Console.ReadLine()**Dodaj następujące wiersze wywołać metody **GetContainerSasUriWithPolicy** :

    //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

### <a name="generate-a-shared-access-signature-uri-on-the-blob-that-uses-an-access-policy"></a>Generowanie podpisu udostępnienia identyfikatora URI w obiektów Blob, która używa zasady dostępu

Na koniec dodamy podobnej metody tworzenia innego obiektów blob i generowania podpisu udostępniania, który jest skojarzony z zasad dostępu.

Dodawanie nowej metody tworzenia obiektów blob i generowania podpisu udostępniania:

    static string GetBlobSasUriWithPolicy(CloudBlobContainer container, string policyName)
    {
        //Get a reference to a blob within the container.
        CloudBlockBlob blob = container.GetBlockBlobReference("sasblobpolicy.txt");

        //Upload text to the blob. If the blob does not yet exist, it will be created.
        //If the blob does exist, its existing content will be overwritten.
        string blobContent = "This blob will be accessible to clients via a shared access signature. " +
        "A stored access policy defines the constraints for the signature.";
        MemoryStream ms = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
        ms.Position = 0;
        using (ms)
        {
            blob.UploadFromStream(ms);
        }

        //Generate the shared access signature on the blob.
        string sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        //Return the URI string for the container, including the SAS token.
        return blob.Uri + sasBlobToken;
    }

W dolnej części metodę **Main()** przed połączenie **Console.ReadLine()**Dodaj następujące wiersze wywołać metody **GetBlobSasUriWithPolicy** :    

    //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
    Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
    Console.WriteLine();

Metoda **Main()** powinna teraz wyglądać następująco w całości. Uruchom go, aby wpisz podpis udostępnienia identyfikatory URI w oknie konsoli, a następnie skopiuj i wklej je do pliku tekstowego do użycia w drugiej części tego samouczka.

    static void Main(string[] args)
    {
        //Parse the connection string and return a reference to the storage account.
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

        //Create the blob client object.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        //Get a reference to a container to use for the sample code, and create it if it does not exist.
        CloudBlobContainer container = blobClient.GetContainerReference("sascontainer");
        container.CreateIfNotExists();

        //Generate a SAS URI for the container, without a stored access policy.
        Console.WriteLine("Container SAS URI: " + GetContainerSasUri(container));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, without a stored access policy.
        Console.WriteLine("Blob SAS URI: " + GetBlobSasUri(container));
        Console.WriteLine();

        //Clear any existing access policies on container.
        BlobContainerPermissions perms = container.GetPermissions();
        perms.SharedAccessPolicies.Clear();
        container.SetPermissions(perms);

        //Create a new access policy on the container, which may be optionally used to provide constraints for
        //shared access signatures on the container and the blob.
        string sharedAccessPolicyName = "tutorialpolicy";
        CreateSharedAccessPolicy(blobClient, container, sharedAccessPolicyName);

        //Generate a SAS URI for the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Container SAS URI using stored access policy: " + GetContainerSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        //Generate a SAS URI for a blob within the container, using a stored access policy to set constraints on the SAS.
        Console.WriteLine("Blob SAS URI using stored access policy: " + GetBlobSasUriWithPolicy(container, sharedAccessPolicyName));
        Console.WriteLine();

        Console.ReadLine();
    }

Po uruchomieniu aplikacji konsoli GenerateSharedAccessSignatures pojawi się dane wyjściowe podobne do następujących, w oknie konsoli. Są to podpisy udostępniania, które będą używane w części 2 samouczka.

![skojarzenia zabezpieczeń konsoli-dane wyjściowe-1][sas-console-output-1]

## <a name="part-2-create-a-console-application-to-test-the-shared-access-signatures"></a>Część 2: Tworzenie aplikacji konsoli próba udostępnienia podpisów

Aby przetestować podpisów udostępnienia utworzone w poprzednich przykładach, utworzymy drugiej aplikacji konsoli używającej podpisy do wykonywania operacji na kontenerze i obiektów blob.

> [AZURE.NOTE] Jeśli więcej niż 24 godziny upłynęło, ponieważ ukończona pierwsza część samouczka, podpisy, który został wygenerowany nie będzie już prawidłowe. W tym przypadku należy uruchomić kod w pierwszej aplikacji konsoli do generowania podpisów świeży udostępniania do użycia w drugiej części samouczka.

W programie Visual Studio Tworzenie nowej aplikacji konsoli systemu Windows i nadaj mu nazwę **ConsumeSharedAccessSignatures**. Dodawanie odwołania do **Microsoft.WindowsAzure.Configuration.dll** i **Microsoft.WindowsAzure.Storage.dll**, tak jak poprzednio.

U góry pliku Plik Program.cs Dodaj następujące instrukcje **za pomocą** :

    using System.IO;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;

W treści metodę **Main()** Dodaj następujące stałych, a aktualizacja ich wartości do podpisów udostępniania, wygenerowanych w części 1 samouczka.

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";
    }

### <a name="add-a-method-to-try-container-operations-using-a-shared-access-signature"></a>Dodawanie metody do wypróbowania operacje kontenera, korzystając z podpisu udostępniania

Następnie dodamy metodę, która sprawdza niektóre operacje przedstawiciela kontenera, korzystając z podpisu udostępniania w kontenerze. Pamiętaj, że podpis udostępniania jest używane zwraca odwołanie do kontenera uwierzytelniania dostępu do kontenera według samego podpisu.

Dodać do plik Program.cs metodę następujące czynności:

    static void UseContainerSAS(string sas)
    {
        //Try performing container operations with the SAS provided.

        //Return a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(sas));

        //Create a list to store blob URIs returned by a listing operation on the container.
        List<ICloudBlob> blobList = new List<ICloudBlob>();

        //Write operation: write a new blob to the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference("blobCreatedViaSAS.txt");
            string blobContent = "This blob was created with a shared access signature granting write permissions to the container. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //List operation: List the blobs in the container.
        try
        {
            foreach (ICloudBlob blob in container.ListBlobs())
            {
                blobList.Add(blob);
            }
            Console.WriteLine("List operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("List operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Get a reference to one of the blobs in the container and read it.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            MemoryStream msRead = new MemoryStream();
            msRead.Position = 0;
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                Console.WriteLine(msRead.Length);
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }
        Console.WriteLine();

        //Delete operation: Delete a blob in the container.
        try
        {
            CloudBlockBlob blob = container.GetBlockBlobReference(blobList[0].Name);
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Aktualizować metody **Main()** połączenie **UseContainerSAS()** zarówno podpisów udostępniania, utworzone w kontenerze:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        Console.ReadLine();
    }


### <a name="add-a-method-to-try-blob-operations-using-a-shared-access-signature"></a>Dodawanie metody do wypróbowania operacje obiektów Blob, korzystając z podpisu udostępniania

Na koniec dodamy metodę, która sprawdza niektóre operacje przedstawiciela obiektów blob, korzystając z podpisu udostępniania na to. W tym przypadku używamy konstruktora **CloudBlockBlob(String)**, przekazując w podpisie udostępniania zwraca odwołanie do obiektów blob. Uwierzytelnianie nie jest wymagane; zależy samego podpisu.

Dodać do plik Program.cs metodę następujące czynności:

    static void UseBlobSAS(string sas)
    {
        //Try performing blob operations using the SAS provided.

        //Return a reference to the blob using the SAS URI.
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(sas));

        //Write operation: Write a new blob to the container.
        try
        {
            string blobContent = "This blob was created with a shared access signature granting write permissions to the blob. ";
            MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
            msWrite.Position = 0;
            using (msWrite)
            {
                blob.UploadFromStream(msWrite);
            }
            Console.WriteLine("Write operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Write operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Read operation: Read the contents of the blob.
        try
        {
            MemoryStream msRead = new MemoryStream();
            using (msRead)
            {
                blob.DownloadToStream(msRead);
                msRead.Position = 0;
                using (StreamReader reader = new StreamReader(msRead, true))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        Console.WriteLine(line);
                    }
                }
            }
            Console.WriteLine("Read operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Read operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }

        //Delete operation: Delete the blob.
        try
        {
            blob.Delete();
            Console.WriteLine("Delete operation succeeded for SAS " + sas);
            Console.WriteLine();
        }
        catch (StorageException e)
        {
            Console.WriteLine("Delete operation failed for SAS " + sas);
            Console.WriteLine("Additional error information: " + e.Message);
            Console.WriteLine();
        }        
    }


Aktualizować metody **Main()** połączenie **UseBlobSAS()** zarówno podpisów udostępniania, tworzone na to:

    static void Main(string[] args)
    {
        string containerSAS = "<your container SAS>";
        string blobSAS = "<your blob SAS>";
        string containerSASWithAccessPolicy = "<your container SAS with access policy>";
        string blobSASWithAccessPolicy = "<your blob SAS with access policy>";

        //Call the test methods with the shared access signatures created on the container, with and without the access policy.
        UseContainerSAS(containerSAS);
        UseContainerSAS(containerSASWithAccessPolicy);

        //Call the test methods with the shared access signatures created on the blob, with and without the access policy.
        UseBlobSAS(blobSAS);
        UseBlobSAS(blobSASWithAccessPolicy);

        Console.ReadLine();
    }

Uruchom aplikację konsoli i obserwować dane wyjściowe, aby zobaczyć, dla których podpisy są dozwolone operacje. Wynik w oknie konsoli będzie wyglądać podobnie do następującej:

![skojarzenia zabezpieczeń konsoli-dane wyjściowe-2][sas-console-output-2]

## <a name="next-steps"></a>Następne kroki

[Udostępniony podpisy programu Access, część 1: Opis modelu skojarzenia zabezpieczeń](storage-dotnet-shared-access-signature-part-1.md)

[Zarządzanie dostępem anonimowym odczytu kontenerów i obiektów blob](storage-manage-access-to-resources.md)

[Delegowanie dostępu za pomocą podpisu udostępniania (interfejsu API usługi REST)](http://msdn.microsoft.com/library/azure/ee395415.aspx)

[Wprowadzenie do tabel i kolejki skojarzenia zabezpieczeń](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)

[sas-console-output-1]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-1.PNG
[sas-console-output-2]: ./media/storage-dotnet-shared-access-signature-part-2/sas-console-output-2.PNG
