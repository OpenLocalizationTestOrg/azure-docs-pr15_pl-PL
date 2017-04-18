
<properties 
    pageTitle="Zarządzanie zasobami i jednostek powiązanych z usługi multimediów .NET SDK" 
    description="Dowiedz się, jak zarządzać składniki majątku i jednostek powiązanych z SDK usługi multimediów dla środowiska .NET." 
    authors="juliako" 
    manager="dwrede" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016"
    ms.author="juliako"/>


#<a name="managing-assets-and-related-entities-with-media-services-net-sdk"></a>Zarządzanie zasobami i jednostek powiązanych z usługi multimediów .NET SDK


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-manage-entities.md)
- [POZOSTAŁE](media-services-rest-manage-entities.md)


W tym temacie przedstawiono sposób wykonywania następujących zadań zarządzania usługi multimediów:

- Uzyskiwanie odwołanie składników majątku 
- Odwołać zadania 
- Lista wszystkich środków trwałych 
- Wyświetlanie listy zadań i zasobów 
- Lista wszystkich zasady dostępu 
- Lista wszystkich Locator
- Wyliczanie za pośrednictwem dużych zbiorach jednostek
- Usuwanie elementów zawartości 
- Usuwanie zadania 
- Usuwanie zasady dostępu 

##<a name="prerequisites"></a>Wymagania wstępne 

Zobacz [Konfigurowanie środowiska](media-services-set-up-computer.md)

##<a name="get-an-asset-reference"></a>Uzyskiwanie odwołanie składników majątku

Często wykonywane zadania jest odwołać się do istniejącej zawartości w usługach multimediów. W poniższym przykładzie pokazano, jak można uzyskać odwołanie zawartości z kolekcji elementów na serwerze obiekt kontekstu, oparte na środka trwałego identyfikatora.
W poniższym przykładzie użyto kwerendy Linq, aby odwołać się do istniejącego obiektu IAsset.

    static IAsset GetAsset(string assetId)
    {
        // Use a LINQ Select query to get an asset.
        var assetInstance =
            from a in _context.Assets
            where a.Id == assetId
            select a;
        // Reference the asset as an IAsset.
        IAsset asset = assetInstance.FirstOrDefault();
    
        return asset;
    }

##<a name="get-a-job-reference"></a>Odwołać zadania

Podczas pracy z przetwarzania zadań w kodzie usługi multimediów, często zachodzi potrzeba odwołać się do istniejącego zadania według identyfikatora. W poniższym przykładzie pokazano, jak odwołać się do obiektu IJob z kolekcji zadania.
WarningWarning może być konieczne odwołać zadania podczas uruchamiania zadania kodowania długim i chcesz sprawdzić stan zadania w wątku. W przypadku tak gdy metoda zwraca wartość z wątku, jest potrzebne do pobierania odświeżone odwołania do zadania.

    static IJob GetJob(string jobId)
    {
        // Use a Linq select query to get an updated 
        // reference by Id. 
        var jobInstance =
            from j in _context.Jobs
            where j.Id == jobId
            select j;
        // Return the job reference as an Ijob. 
        IJob job = jobInstance.FirstOrDefault();
    
        return job;
    }

##<a name="list-all-assets"></a>Lista wszystkich środków trwałych

W miarę liczbę elementów, które masz w magazynie jest przydatne do wyświetlania listy aktywów. W poniższym przykładzie pokazano sposób do iteracji kolekcji składników majątku na obiekt kontekstu serwera. Z każdym trwałym przykładowy kod zapisuje także niektóre z jego wartości właściwości konsoli. Na przykład każdego trwałego może zawierać wiele plików multimedialnych. Przykładowy kod zapisuje się wszystkie pliki skojarzone z poszczególnych elementów zawartości.



    static void ListAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IAsset asset in _context.Assets)
        {
            // Display the collection of assets.
            builder.AppendLine("");
            builder.AppendLine("******ASSET******");
            builder.AppendLine("Asset ID: " + asset.Id);
            builder.AppendLine("Name: " + asset.Name);
            builder.AppendLine("==============");
            builder.AppendLine("******ASSET FILES******");
    
            // Display the files associated with each asset. 
            foreach (IAssetFile fileItem in asset.AssetFiles)
            {
                builder.AppendLine("Name: " + fileItem.Name);
                builder.AppendLine("Size: " + fileItem.ContentFileSize);
                builder.AppendLine("==============");
            }
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-jobs-and-assets"></a>Wyświetlanie listy zadań i zasobów

Ważne zadania pokrewne jest lista składników majątku z pracą skojarzone w usługach multimediów. W poniższym przykładzie pokazano, jak listy każdy obiekt IJob dla każdego zadania Wyświetla właściwości o zadaniu, wszystkie powiązane z nim zadania, wprowadzania wszystkie składniki majątku i wszystkich trwałych dane wyjściowe. Kod w tym przykładzie może być przydatne w przypadku wielu innych zadań. Na przykład jeśli chcesz lista składników majątku dane wyjściowe z co najmniej jeden kodowania zadań, które wcześniej uruchomiono kod pokazano, jak dostępu składniki majątku dane wyjściowe. Jeśli odwołanie do środka trwałego dane wyjściowe, możesz następnie dostarcz zawartości do inni użytkownicy lub aplikacje, pobierając ją lub udostępniając adresy URL. 

Aby uzyskać więcej informacji na temat opcji dostarczania składników majątku zobacz [Przeprowadzania zasobami za pomocą SDK usługi multimediów dla środowiska .NET](media-services-deliver-streaming-content.md).

    // List all jobs on the server, and for each job, also list 
    // all tasks, all input assets, all output assets.

    static void ListJobsAndAssets()
    {
        string waitMessage = "Building the list. This may take a few "
            + "seconds to a few minutes depending on how many assets "
            + "you have."
            + Environment.NewLine + Environment.NewLine
            + "Please wait..."
            + Environment.NewLine;
        Console.Write(waitMessage);
    
        // Create a Stringbuilder to store the list that we build. 
        StringBuilder builder = new StringBuilder();
    
        foreach (IJob job in _context.Jobs)
        {
            // Display the collection of jobs on the server.
            builder.AppendLine("");
            builder.AppendLine("******JOB*******");
            builder.AppendLine("Job ID: " + job.Id);
            builder.AppendLine("Name: " + job.Name);
            builder.AppendLine("State: " + job.State);
            builder.AppendLine("Order: " + job.Priority);
            builder.AppendLine("==============");
    
    
            // For each job, display the associated tasks (a job  
            // has one or more tasks). 
            builder.AppendLine("******TASKS*******");
            foreach (ITask task in job.Tasks)
            {
                builder.AppendLine("Task Id: " + task.Id);
                builder.AppendLine("Name: " + task.Name);
                builder.AppendLine("Progress: " + task.Progress);
                builder.AppendLine("Configuration: " + task.Configuration);
                if (task.ErrorDetails != null)
                {
                    builder.AppendLine("Error: " + task.ErrorDetails);
                }
                builder.AppendLine("==============");
            }
    
            // For each job, display the list of input media assets.
            builder.AppendLine("******JOB INPUT MEDIA ASSETS*******");
            foreach (IAsset inputAsset in job.InputMediaAssets)
            {
    
                if (inputAsset != null)
                {
                    builder.AppendLine("Input Asset Id: " + inputAsset.Id);
                    builder.AppendLine("Name: " + inputAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
            // For each job, display the list of output media assets.
            builder.AppendLine("******JOB OUTPUT MEDIA ASSETS*******");
            foreach (IAsset theAsset in job.OutputMediaAssets)
            {
                if (theAsset != null)
                {
                    builder.AppendLine("Output Asset Id: " + theAsset.Id);
                    builder.AppendLine("Name: " + theAsset.Name);
                    builder.AppendLine("==============");
                }
            }
    
        }
    
        // Display output in console.
        Console.Write(builder.ToString());
    }

##<a name="list-all-access-policies"></a>Lista wszystkich zasady dostępu

W usługi multimediów można zdefiniować zasadę dostępu na środka trwałego lub jej plików. Zasady dostępu określa uprawnienia dla pliku lub środka trwałego (typ dostępu i czas trwania). W kodzie usługi multimediów zwykle określają tworząc obiektu IAccessPolicy, a następnie kojarząc ją z istniejącego trwałego zasady dostępu. Następnie możesz utworzyć obiektu ILocator, który pozwala zapewnić bezpośredni dostęp do trwałych usługi multimediów. Projekt programu Visual Studio, dostarczonej tej serii dokumentacji zawiera przykłady kodu, pokazujące, jak utworzyć i przypisać zasady dostępu i Locator składników majątku.

W poniższym przykładzie pokazano sposób wyświetlić listę wszystkich zasady dostępu na serwerze oraz typ uprawnień skojarzonych z każdym. Inny sposób przydatne, aby wyświetlić zasady dostępu jest aby wyświetlić listę wszystkich obiektów ILocator na serwerze, a następnie dla każdego locator, można wyświetlić listę jej zasady skojarzone dostępu za pomocą właściwości AccessPolicy.

    static void ListAllPolicies()
    {
        foreach (IAccessPolicy policy in _context.AccessPolicies)
        {
            Console.WriteLine("");
            Console.WriteLine("Name:  " + policy.Name);
            Console.WriteLine("ID:  " + policy.Id);
            Console.WriteLine("Permissions: " + policy.Permissions);
            Console.WriteLine("==============");
    
        }
    }

##<a name="list-all-locators"></a>Lista wszystkich Locator

Adres URL, który zawiera bezpośrednią ścieżkę, aby uzyskać dostęp do środka trwałego, wraz z uprawnieniami do środka, zdefiniowane przez zasady dostępu skojarzone locator jest locator. Każdy trwały może mieć kolekcji obiektów ILocator skojarzonych z nim na jego właściwość Locator. Kontekst serwera jest również zbioru Locator, który zawiera wszystkie Locator.

W poniższym przykładzie zawiera listę wszystkich Locator na serwerze. Dla każdego locator zawiera ono identyfikator zasady powiązanych elementów zawartości i access. Wyświetla również typ uprawnienia, datę wygaśnięcia oraz pełną ścieżkę do środka.

Należy zauważyć, że ścieżka locator, do środka trwałego tylko podstawowego adresu URL elementu. Aby utworzyć bezpośrednią ścieżkę do pojedynczych plików, które użytkownik lub aplikacja może przejdź do, kodzie należy dodać ścieżkę określonego pliku do ścieżki lokalizacji. Aby uzyskać więcej informacji o tym, jak to zrobić zobacz temat [Przeprowadzania zasobami za pomocą SDK usługi multimediów dla środowiska .NET](media-services-deliver-streaming-content.md).

    static void ListAllLocators()
    {
        foreach (ILocator locator in _context.Locators)
        {
            Console.WriteLine("***********");
            Console.WriteLine("Locator Id: " + locator.Id);
            Console.WriteLine("Locator asset Id: " + locator.AssetId);
            Console.WriteLine("Locator access policy Id: " + locator.AccessPolicyId);
            Console.WriteLine("Access policy permissions: " + locator.AccessPolicy.Permissions);
            Console.WriteLine("Locator expiration: " + locator.ExpirationDateTime);
            // The locator path is the base or parent path (with included permissions) to access  
            // the media content of an asset. To create a full URL to a specific media file, take 
            // the locator path and then append a file name and info as needed.  
            Console.WriteLine("Locator base path: " + locator.Path);
            Console.WriteLine("");
        }
    }

## <a name="enumerating-through-large-collections-of-entities"></a>Wyliczanie za pośrednictwem dużych zbiorach jednostek

Podczas wysyłania kwerend jednostki, istnieje ograniczenie 1000 jednostek zwracane w tym samym czasie, ponieważ publicznej pozostałych wersji 2 ogranicza wyniki kwerendy do 1000 wyników. Należy używać Pomiń i Cofnij, gdy wyliczania dużych zbiorów podmiotów. 
    
Poniższa funkcja w pętli wszystkie zadania w podanej konta usługi multimediów. Usługi multimediów zwraca 1000 zadań w zbiorze zadania. Funkcja korzysta z Pomiń i Cofnij, aby upewnić się, że wszystkie zadania są wyliczane (w przypadku masz więcej niż 1000 zadań na koncie).
    
    static void ProcessJobs()
    {
        try
        {
    
            int skipSize = 0;
            int batchSize = 1000;
            int currentBatch = 0;
    
            while (true)
            {
                // Loop through all Jobs (1000 at a time) in the Media Services account
                IQueryable _jobsCollectionQuery = _context.Jobs.Skip(skipSize).Take(batchSize);
                foreach (IJob job in _jobsCollectionQuery)
                {
                    currentBatch++;
                    Console.WriteLine("Processing Job Id:" + job.Id);
                }
    
                if (currentBatch == batchSize)
                {
                    skipSize += batchSize;
                    currentBatch = 0;
                }
                else
                {
                    break;
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }

##<a name="delete-an-asset"></a>Usuwanie elementów zawartości

W następującym przykładzie środka trwałego.

    static void DeleteAsset( IAsset asset)
    {
        // delete the asset
        asset.Delete();
    
        // Verify asset deletion
        if (GetAsset(asset.Id) == null)
            Console.WriteLine("Deleted the Asset");
    
    }

##<a name="delete-a-job"></a>Usuwanie zadania

Aby usunąć zadanie, możesz sprawdzić stan zadania, jak wskazano w polu właściwości stan. Można usunąć zadania, które zostały zakończone lub anulowane, podczas zadania, które znajdują się w niektórych innych państw, na przykład kolejce, według harmonogramu lub przetwarzania, należy najpierw anulować, a następnie można je usunąć.
W poniższym przykładzie pokazano metody usuwania zadania sprawdzanie stany zadań, a następnie usuwając stanu jest zakończone lub anulowane. Kod zależy od poprzedniej sekcji, w tym temacie uzyskiwania odwołanie do zadania: odwołać zadania.

    static void DeleteJob(string jobId)
    {
        bool jobDeleted = false;
    
        while (!jobDeleted)
        {
            // Get an updated job reference.  
            IJob job = GetJob(jobId);
    
            // Check and handle various possible job states. You can 
            // only delete a job whose state is Finished, Error, or Canceled.   
            // You can cancel jobs that are Queued, Scheduled, or Processing,  
            // and then delete after they are canceled.
            switch (job.State)
            {
                case JobState.Finished:
                case JobState.Canceled:
                case JobState.Error:
                    // Job errors should already be logged by polling or event 
                    // handling methods such as CheckJobProgress or StateChanged.
                    // You can also call job.DeleteAsync to do async deletes.
                    job.Delete();
                    Console.WriteLine("Job has been deleted.");
                    jobDeleted = true;
                    break;
                case JobState.Canceling:
                    Console.WriteLine("Job is cancelling and will be deleted "
                        + "when finished.");
                    Console.WriteLine("Wait while job finishes canceling...");
                    Thread.Sleep(5000);
                    break;
                case JobState.Queued:
                case JobState.Scheduled:
                case JobState.Processing:
                    job.Cancel();
                    Console.WriteLine("Job is scheduled or processing and will "
                        + "be deleted.");
                    break;
                default:
                    break;
            }
    
        }
    }


##<a name="delete-an-access-policy"></a>Usuwanie zasady dostępu

W poniższym przykładzie pokazano sposób odwołać się do zasad programu access na podstawie zasad Id, a następnie usunąć zasady.

    static void DeleteAccessPolicy(string existingPolicyId)
    {
        // To delete a specific access policy, get a reference to the policy.  
        // based on the policy Id passed to the method.
        var policyInstance =
                from p in _context.AccessPolicies
                where p.Id == existingPolicyId
                select p;
        IAccessPolicy policy = policyInstance.FirstOrDefault();
    
        policy.Delete();
    
    }
    


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
