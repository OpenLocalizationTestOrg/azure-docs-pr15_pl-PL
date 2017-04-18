<properties
    pageTitle="Pliki multimedialne Hyperlapse z Hyperlapse multimediów Azure | Microsoft Azure"
    description="Azure Hyperlapse multimediów tworzy wygładzonymi czas, jaki upłynął klipów wideo z pierwszej osoby lub aparatu fotograficznego akcji zawartość. W tym temacie przedstawiono sposób za pomocą indeksatora multimediów."
    services="media-services"
    documentationCenter=""
    authors="asolanki"
    manager="johndeu"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/19/2016"  
    ms.author="adsolank"/>


# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Pliki multimedialne Hyperlapse z Hyperlapse multimediów Azure

Azure Hyperlapse multimediów jest multimediów procesora (CR) tworzące wygładzonymi czas, jaki upłynął klipów wideo z pierwszej osoby lub aparatu fotograficznego akcji zawartość.  Równorzędne opartej na chmurze [Hyperlapse Pro pulpitu Microsoft Research](http://aka.ms/hyperlapse)i oparte na telefon komórkowy Hyperlapse Hyperlapse firmy Microsoft dla usługi multimediów Azure wykorzystuje ogromną skalę platformy przetwarzania multimediów usługi multimediów Azure poziomie skalowanie i parallelize należy Hyperlapse zbiorczo. przetwarzania.

>[AZURE.IMPORTANT]Microsoft Hyperlapse jest przeznaczony do najlepiej na pierwszej osoby zawartości za pomocą aparatu ruchomą.  Mimo że materiału kamery są nadal można nadal będą działać, wydajności i jakości procesor multimediów Hyperlapse multimediów Azure nie zagwarantować dla pozostałych typów zawartości.  Aby dowiedzieć się więcej o Hyperlapse firmy Microsoft dla usługi multimediów Azure i zobaczyć niektóre filmy przykład, zapoznaj się z [wprowadzający w blogu](http://aka.ms/azurehyperlapseblog) z poziomu publicznych okna podglądu.

Hyperlapse multimediów Azure zadania przyjmuje jako dane wejściowe MP4, MOV lub WMV zawartości pliku wraz z pliku konfiguracji, który określa, które ramki wideo powinny być czas, jaki upłynął i jakie szybkość (np. pierwszych 10 000 ramki x 2).  Wynik to ustalonych i czas, jaki upłynął odwzorowanie danych wejściowych wideo.

Aby uzyskać najnowsze aktualizacje Hyperlapse multimediów Azure zobacz [blogów usługi multimediów](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Hyperlapse środka trwałego

Najpierw należy przesłać odpowiedni plik wprowadzania danych do usługi multimediów Azure.  Aby dowiedzieć się więcej na temat pojęć związanych z przekazywaniem i zarządzanie zawartością, przeczytaj [artykuł Zarządzanie zawartością](media-services-portal-vod-get-started.md).

###  <a id="configuration"></a>Wstępne ustawienie konfiguracji Hyperlapse

Gdy zawartość znajduje się na koncie usługi multimediów, należy utworzyć ustawienia konfiguracji.  W poniższej tabeli opisano pola zdefiniowane przez użytkownika:

 Pole | Opis
-------|-------------
StartFrame|Ramka, na której należy rozpocząć przetwarzanie Hyperlapse firmy Microsoft.
NumFrames|Liczba ramek przetwarzania
Szybkość|Współczynnik umożliwiające przyspieszanie wideo wprowadzania danych.

Oto przykładowy plik konfiguracji zgodność w JSON i języka XML:

**Ustawienie wstępne XML:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**Ustawienie wstępne JSON:**

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

###  <a id="sample_code"></a>Microsoft Hyperlapse z AMS .NET SDK

Poniższa metoda przekazywania pliku multimedialnego jako środka trwałego i tworzy zadanie z procesorem multimediów Hyperlapse multimediów Azure.

> [AZURE.NOTE] Czy jest już CloudMediaContext w zakresie nazwę "kontekst" dla tego kodu do pracy.  Aby uzyskać więcej informacji na ten temat, przeczytaj [artykuł Zarządzanie zawartością](media-services-dotnet-get-started.md).

> [AZURE.NOTE] Argument ciągu "hyperConfig" ma być konfiguracji zgodność wstępnie ustawione w JSON lub XML, zgodnie z powyższym opisem.

wartość logiczna statyczne RunHyperlapseJob (ciąg na wejściu, wynik ciąg hyperConfig ciągu) {- / Utwórz trwały z pliku wejściowego IAsset zawartości = kontekst. Trwałe. CreateAssetAndUploadSingleFile (wejście "Moje wprowadzania Hyperlapse", AssetCreationOptions.None);

chwyć wystąpień IMediaProcessor CR Hyperlapse multimediów Azure CR = kontekst. MediaProcessors. GetLatestMediaProcessorByName ("multimediów Azure Hyperlapse");

Tworzenie zadania przy użyciu Hyperlapse zadanie IJob = kontekst. Zadania. Tworzenie (String.Format ("Hyperlapse {0}", wprowadź));

Jeśli (String.IsNullOrEmpty(hyperConfig)) {/ / konfiguracji nie może być puste zwracana wartość FAŁSZ;}

hyperConfig = File.ReadAllText(hyperConfig);

ITask hyperlapseTask = zadania. Tasks.AddNew ("Hyperlapse zadania" CR, hyperConfig, TaskOptions.None); hyperlapseTask.InputAssets.Add(asset); hyperlapseTask.OutputAssets.AddNew ("Hyperlapse wyjście", AssetCreationOptions.None);


zadanie. Submit();

Tworzenie postęp drukowania i wykonywanie zapytań za zadania zadania progressPrintTask = nowe Task(() = > {

IJob jobQuery = null; Wykonaj {var progressContext = kontekstu; jobQuery = progressContext.Jobs. Miejsce, w którym (j = > j.Id == zadania. Identyfikator). First(); Console.WriteLine (ciąg. Format ("\t \t {1} {0} {2}", DateTime.Now, jobQuery.State, jobQuery.Tasks[0]. Informacje o postępie)); Thread.Sleep(10000); } Podczas (jobQuery.State! = JobState.Finished & & jobQuery.State! = JobState.Error & & jobQuery.State! = JobState.Canceled); }); progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
            progressJobTask.Wait();

            // If job state is Error, the event handling
            // method for job progress should log errors.  Here we check
            // for error state and exit if needed.
            if (job.State == JobState.Error)
            {
                ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                Console.WriteLine(string.Format("Error: {0}. {1}",
                                                error.Code,
                                                error.Message));  
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <a id="file_types"></a>Obsługiwane typy plików

- MP4
- MOV
- WMV



##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-links"></a>Łącza pokrewne

[Omówienie analizy usługi multimediów Azure](media-services-analytics-overview.md)

[Pokazy analizy multimediów Azure](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
