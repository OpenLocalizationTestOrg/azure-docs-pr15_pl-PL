<properties
    pageTitle="Indeksowanie plików multimedialnych z indeksatora multimediów Azure"
    description="Azure indeksatora multimediów można nawiązać można wyszukiwać zawartość plików multimedialnych, a także do generowania zapis pełnotekstowym dla podpisy kodowane oraz słów kluczowych. W tym temacie przedstawiono sposób za pomocą indeksatora multimediów."
    services="media-services"
    documentationCenter=""
    authors="Asolanki"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="adsolank;juliako;johndeu"/>


# <a name="indexing-media-files-with-azure-media-indexer"></a>Indeksowanie plików multimedialnych z indeksatora multimediów Azure


Azure indeksatora multimediów można nawiązać można wyszukiwać zawartość plików multimedialnych, a także do generowania zapis pełnotekstowym dla podpisy kodowane oraz słów kluczowych. Można przetwarzać plik multimedialny jednego lub wielu plików multimedialnych w serii.  

>[AZURE.IMPORTANT] Podczas indeksowania zawartości, upewnij się użyć pliki multimedialne, które mają bardzo jasne mowę (bez muzyki w tle, hałasu, efekty i mikrofon szumów). Oto niektóre przykłady odpowiedniej zawartości: zarejestrowanego spotkań, wykładów lub prezentacji. Następująca zawartość może nie być odpowiedni dla indeksowania: filmy i programy telewizyjne, cokolwiek z mieszanych audio i efekty dźwiękowe źle zarejestrowanego zawartości za pomocą szum w tle (szumów).


Indeksowania zadania można wygenerować wyjściowe następujące czynności:

- Zamknięty podpis pliki w następujących formatach: **LAPOŃSKI**, **TTML**i **WebVTT**.

    Pliki podpisów kodowanych zawierają znacznikiem o nazwie Recognizability, które wyniki indeksowania zadania na podstawie jak rozpoznać mowy w klipie wideo źródła jest.  Wartość Recognizability służy do ekranu plików wyjściowych dla użyteczności. Wynik niskim będzie oznaczać niskiej indeksowania wyników ze względu na jakość dźwięku.
- Plik słów kluczowych (XML).
- Audio indeksowania obiektów blob pliku (AIB) do użytku z programem SQL server.

    Aby uzyskać więcej informacji zobacz [Korzystanie z plików AIB z indeksatora multimediów Azure i SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).


W tym temacie przedstawiono sposób tworzenia indeksowania do **indeksu środka trwałego** oraz **indeksować wielu plików**.

Aby uzyskać najnowsze aktualizacje indeksatora multimediów Azure zobacz [blogów usługi multimediów](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Korzystanie z plików konfiguracji i manifest dla zadania indeksowania

Więcej informacji można określić indeksowania zadań przy użyciu konfiguracji zadania. Na przykład można określić, które metadanych do użytku w pliku multimediów. Metadane jest używane przez aparat języka, aby rozwinąć jego słownictwo i znacznie zwiększa dokładność rozpoznawania mowy.  Widoczny jest również określić żądany plików.

Wiele plików multimedialnych można także przetwarzać jednocześnie przy użyciu pliku manifestu.

Aby uzyskać więcej informacji zobacz [Zadania wstępnie ustawione dla indeksowania multimediów Azure](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Indeksowanie środka trwałego

Następująca metoda przekazywania pliku multimedialnego jako środka trwałego i tworzy zadanie elementu.

Należy zauważyć, że jeśli plik konfiguracji nie zostanie określony, plik multimedialny będzie indeksowany przy użyciu wszystkich ustawień domyślnych.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }  
<!-- __ -->
### <a id="output_files"></a>Pliki wyjściowe

Domyślnie zadania indeksowania generuje następujące pliki danych wyjściowych. Pliki są przechowywane w pierwszym trwałego dane wyjściowe.

W przypadku więcej niż jeden plik multimedialny wprowadzania indeksatora wygeneruje pliku manifestu dla wyjściowe zadania, o nazwie "JobResult.txt". Dla każdego wprowadzania plik multimedialny, AIB wyniku, LAPOŃSKI, TTML, WebVTT i pliki słowo kluczowe kolejno są numerowane i o nazwie "aliasu".

Nazwa pliku | Opis
----------|------------
__InputFileName.aib__ | Plik obiektów blob indeksowania audio. <br /><br /> Plik dźwiękowy obiektów Blob indeksowania (AIB) jest plik binarny, który można wyszukiwać w programie Microsoft SQL server przy użyciu wyszukiwanie pełnotekstowe.  Plik AIB jest bardziej zaawansowane niż pliki podpis prosty, ponieważ zawiera on rozwiązania alternatywne dla każdego wyrazu, co pozwala znacznie więcej możliwości wyszukiwania. <br/> <br/>Wymaga instalacji dodatku indeksatora SQL na serwerze komputera uruchomionego programu Microsoft SQL server 2008 lub nowszego. Wyszukiwanie AIB przy użyciu programu Microsoft SQL wyszukiwanie pełnotekstowe serwera zawiera dokładniejsze wyników wyszukiwania niż wyszukiwania plików podpisów kodowanych generowanych przez WAMI. To jest powodujący AIB słowa zastępcze dźwięk, który podobne pliki podpisów kodowanych zawierają najwyższe wyraz zaufania dla każdej części audio. Jeśli wyszukiwanie mówionego ma znaczenie upmost, a następnie zaleca się używanie AIB w połączeniu z programu Microsoft SQL Server.<br/><br/> Aby pobrać dodatek, kliknij pozycję <a href="http://aka.ms/indexersql">Dodatek SQL indeksatora multimediów Azure</a>. <br/><br/>Istnieje także możliwość korzystanie z innych aparatów wyszukiwania, takie jak Apache Lucene-Solr po prostu indeksowania wideo na podstawie napisy i pliki XML słów kluczowych, ale spowoduje to mniej dokładne wyniki wyszukiwania.
__InputFileName.smi__<br />__InputFileName.ttml__<br />__InputFileName.vtt__ |Zamknięty podpis (DW) plików w formatach LAPOŃSKI, TTML i WebVTT.<br/><br/>Ta osoba może służyć do była dostępna dla osoby niepełnosprawne słuchu plików audio i wideo.<br/><br/>Zamknięty plików podpis należą znacznikiem o nazwie <b>Recognizability</b> jest wyniki, które indeksowania zadania według jak rozpoznać mowy w źródłowym wideo.  Wartość <b>Recognizability</b> służy do ekranu plików wyjściowych dla użyteczności. Niska wynik będzie oznaczać niskiej indeksowania wyników ze względu na jakość dźwięku.
__InputFileName.kw.xml<br />InputFileName.info__ |Pliki słów kluczowych i informacje. <br/><br/>Plik słów kluczowych znajduje się plik XML zawiera słowa kluczowe wyodrębnionych z zawartością mowy o częstotliwości i informacje dotyczące przesunięcia. <br/><br/>Plik INFO jest plik tekstowy, który zawiera szczegółowe informacje dotyczące każdego terminu rozpoznany. Pierwszy wiersz jest specjalne i zawiera wynik Recognizability. Każdy wiersz kolejny znajduje się lista tabulatorami poniższe dane: rozpoczynanie czasu, czas zakończenia, program word frazę, funkcja UFNOŚĆ. Godziny są podane w sekundach i zaufania jest podawana jako liczba z 0-1. <br/><br/>Przykład wiersz: "word 1,20 wysokości 1,45 0,67" <br/><br/>Te pliki można używane dla liczby celów, takich jak przeprowadzić analizę mowy lub na aparatów wyszukiwania, takie jak Bing, Google lub programu Microsoft SharePoint, aby pliki multimedialne więcej odnajdowania lub nawet użyty do przeprowadzania bardziej odpowiednich reklam.
__JobResult.txt__ |Dane wyjściowe oczywiste, tylko wtedy, gdy indeksowania wielu plików, zawierające następujące informacje:<br/><br/><table border="1"><tr><th>Plik_wejściowy</th><th>Alias</th><th>MediaLength</th><th>Błąd</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/>



Jeśli nie wszystkie pliki multimedialne wprowadzania są indeksowane pomyślnie, indeksowania zadanie zakończy się niepowodzeniem z kodem błędu 4000 znaków. Aby uzyskać więcej informacji zobacz [kody błędów](#error_codes).

## <a name="index-multiple-files"></a>Indeksowanie wielu plików

Poniższej metody przekazywanie wielu plików multimedialnych jako środka trwałego i tworzy zadanie te pliki w serii.

Manifestu plik z rozszerzeniem .lst jest utworzone i przekazywania do elementu. Plik manifestu zawiera listę wszystkich plików zawartości. Aby uzyskać więcej informacji zobacz [Zadania wstępnie ustawione dla indeksowania multimediów Azure](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Częściowo pomyślnie zadania

Jeśli nie wszystkie pliki multimedialne wprowadzania są indeksowane pomyślnie, indeksowania zadanie zakończy się niepowodzeniem z kodem błędu 4000 znaków. Aby uzyskać więcej informacji zobacz [kody błędów](#error_codes).


Tym samym wyjściowe (jako pomyślnie zadań) są generowane. Można to sprawdzić pliku manifestu wyjściowym, aby dowiedzieć się, pliki wprowadzania danych, które są nie powiodło się, według wartości kolumny błędu. Dla wprowadzania plików, które nie powiodło się, wynikowy AIB, LAPOŃSKI, TTML, WebVTT i słów kluczowych plików nie zostaną wygenerowane.

### <a id="preset"></a>Ustawienie wstępne zadania dla indeksowania multimediów Azure

Przetwarzanie z indeksatora multimediów Azure można dostosować, dostarczając opcjonalne zadania wstępnie ustawione obok zadania.  Poniżej opisano format xml tej konfiguracji.

Nazwa | Wymaganie | Opis
----|----|---
__dane wejściowe__ | FAŁSZ | Pliki zawartości, które chcesz indeksować.</p><p>Indeksowanie multimediów Azure obsługuje następujące formaty plików multimedialnych: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Możesz określić nazwę pliku (s) w atrybucie **nazwy** lub **listy** **wprowadzania** elementu (jak pokazano poniżej). Jeśli nie określisz plików zawartości do indeksu, jest pobierany plik podstawowy. Jeśli plik nie podstawowych elementów zawartości jest ustawiona, pierwszy plik wprowadzania zasobów jest indeksowane.</p><p>Aby jawnie określić nazwę pliku zawartości, wykonaj:<br />`<input name="TestFile.wmv">`<br /><br />Można również indeksowanie wielu elementów zawartości pliki jednocześnie (maksymalnie 10). Aby to zrobić:<br /><br /><ol class="ordered"><li><p>Tworzenie pliku tekstowego (plik manifestu) i nadaj rozszerzeniem .lst. </p></li><li><p>Dodać listę wszystkich nazw plików zawartości w swojej wprowadzania zawartości do tego pliku manifestu. </p></li><li><p>Dodawanie pliku thanifest (przekazanie) do elementu.  </p></li><li><p>Określ nazwę pliku manifestu w atrybut Lista dane wejściowe.<br />`<input list="input.lst">`</li></ol><br /><br />Uwaga: Jeśli dodasz więcej niż 10 plików do pliku manifestu indeksowania zadanie zakończy się niepowodzeniem z kodem błędu 2006 roku.
__metadane__ | FAŁSZ | Metadane dla plików określonej zawartości na potrzeby dostosowania słownictwo.  Warto przygotować indeksatora rozpoznanie słownictwo niestandardowe słowa, takie jak nazwy własne.<br />`<metadata key="..." value="..."/>` <br /><br />Można podać __wartości__ dla wstępnie zdefiniowane __klawiszy__. Obecnie obsługiwane są następujące klucze:<br /><br />"tytuł" i "opis" - dostosowania słownictwo dodawać języka modelu dla zadania i poprawić dokładność rozpoznawania mowy.  Wartości zapełnić przeszukiwania Internetu, aby znaleźć dokumenty contextually odpowiedniego tekstu, uzupełnić wewnętrznych słownika na czas trwania zadania indeksowania za pomocą zawartości.<br />`<metadata key="title" value="[Title of the media file]" />`<br />`<metadata key="description" value="[Description of the media file] />"`
__Funkcje__ <br /><br /> Dodany w wersji 1.2. Obecnie tylko obsługiwana jest rozpoznawania mowy ("automatycznego odzyskiwania systemu").| FAŁSZ | Funkcja rozpoznawania mowy występują następujące klucze ustawienia:<table><tr><th><p>Klawisz</p></th>     <th><p>Opis</p></th><th><p>Przykładowa wartość</p></th></tr><tr><td><p>Język</p></td><td><p>Język naturalny uznawane plik multimedialny.</p></td><td><p>Angielski, hiszpański</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>formaty rozdzielaną średnikami listę podpisu żądany (jeśli istnieje)</p></td><td><p>Lapoński ttml; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Flagę logiczną, określając, czy plik AIB jest wymagany (do użycia z programu SQL Server i obsługi klientów IFilter indeksatora).  Aby uzyskać więcej informacji zobacz <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">Korzystanie z plików AIB z indeksatora multimediów Azure i SQL Server</a>.</p></td><td><p>Wartości PRAWDA; FAŁSZ</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Flagę logiczną, określając, czy plik XML słowo kluczowe jest wymagane.</p></td><td><p>Wartości PRAWDA; Wartość logiczna FAŁSZ. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Flagę logiczną, określając, czy chcesz wymusić pełny podpisów (bez względu na ufności).  </p><p>Domyślna to false, w takim przypadku wyrazów i fraz, które mają mniej niż 50% ufności zostaną pominięte wyjściowe ostateczne podpis i zastępuje wielokropek ("...").  Wielokropek są przydatne do kontroli jakości podpis i inspekcja.</p></td><td><p>Wartości PRAWDA; Wartość logiczna FAŁSZ. </p></td></tr></table>

### <a id="error_codes"></a>Kody błędów

W przypadku błędu, zgłoś indeksatora multimediów Azure kopii jedną z poniższych kodów błędów:

Kod | Nazwa | Możliwe przyczyny
-----|------|------------------
2000 | Nieprawidłowa konfiguracja | Nieprawidłowa konfiguracja
2001 | Nieprawidłowe wprowadzania składników majątku | Brak wprowadzania aktywów lub pustych elementów zawartości.
2002 | Błędny manifest | Manifest jest pusta lub manifest zawiera nieprawidłowe elementy.
2003 | Nie można pobrać pliku multimedialnego | Nieprawidłowy adres URL w pliku manifestu.
2004 | Nieobsługiwane Protocol (protokół) | Protokół adresu URL multimediów nie jest obsługiwany.
2005 | Nieobsługiwany typ pliku | Typy plików multimedialnych wprowadzania nie jest obsługiwane.
2006 roku | Zbyt wiele plików wprowadzania danych | Istnieje więcej niż 10 plików w manifeście wprowadzania danych.
3000 | Nie można odszyfrować pliku multimedialnego | Nieobsługiwane kodera-dekodera <br/>lub<br/> Uszkodzony plik <br/>lub<br/> Nie strumieniu audio w wprowadzania multimediów.
4000 znaków | Indeksowanie partii częściowo zakończyła się pomyślnie | Niektóre pliki multimedialne wprowadzania są nie można być indeksowane. Aby uzyskać więcej informacji zobacz <a href="#output_files">pliki wyjściowe</a>.
inne | Błędy wewnętrzne | Skontaktuj się z zespołem pomocy technicznej. indexer@microsoft.com


## <a id="supported_languages"></a>Obsługiwane języki

Obecnie są obsługiwane języki angielski i hiszpański. Aby uzyskać więcej informacji zobacz [wpis w blogu wydania wersji 1.2](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).


##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Łącza pokrewne

[Omówienie analizy usługi multimediów Azure](media-services-analytics-overview.md)

[Korzystanie z plików AIB z indeksatora multimediów Azure i SQL Server](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Indeksowanie plików multimedialnych z podglądem indeksatora 2 multimediów Azure](media-services-process-content-with-indexer2.md)
