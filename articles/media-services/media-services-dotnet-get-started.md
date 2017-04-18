<properties
    pageTitle="Wprowadzenie do dostarczania zawartości na żądanie przy użyciu .NET | Azure"
    description="Ten samouczek przeprowadzi Cię przez kroki implementacji aplikację dostarczania zawartości na żądanie z usługi multimediów Azure za pomocą .NET."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/17/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Wprowadzenie do dostarczania zawartości na żądanie przy użyciu zestawu SDK .NET

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

>[AZURE.NOTE]
> Aby użyć tego samouczka, potrzebne jest konto Azure. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](/pricing/free-trial/?WT.mc_id=A261C142F). 
 
##<a name="overview"></a>Omówienie 

Ten samouczek przeprowadzi Cię przez kroki stosowania aplikację dostarczania zawartości usługi wideo na żądanie (VoD) przy użyciu usługi multimediów Azure (AMS) SDK dla środowiska .NET.


Samouczek wprowadza podstawowe przepływu pracy usługi multimediów i najczęściej używanych obiektów programowania i zadania wymagane rozwoju usługi multimediów. Po zakończeniu samouczka będą mogli przesyłać strumieniowo lub stopniowo pobrać przykładowy plik multimedialny, przekazane, zakodowany i pobrać ustawienia.

## <a name="what-youll-learn"></a>Dowiesz się

Samouczek pokazano, jak wykonywać następujące zadania:

1.  Utwórz konto usługi multimediów (za pomocą portalu Azure).
2.  Skonfiguruj przesyłanie strumieniowe punkt końcowy (za pomocą portalu Azure).
3.  Tworzenie i konfigurowanie projektu programu Visual Studio.
5.  Nawiązywanie połączenia z kontem usługi multimediów.
6.  Utwórz nowy trwały i przekaż plik wideo.
7.  Kodowanie pliku źródłowego do zestawu plików MP4 adaptacyjne szybkość transmisji bitów.
8.  Publikowanie elementu i uzyskać adresy URL przesyłanie strumieniowe i stopniowego pobierania.
9.  Przetestuj odtwarzanie zawartości.

## <a name="prerequisites"></a>Wymagania wstępne

Następujące czynności są wymagane do zakończenia tego samouczka.

- Aby użyć tego samouczka, potrzebne jest konto Azure. 
    
    Jeśli nie masz konta, możesz utworzyć bezpłatne konto wersji próbnej na kilka minut. Aby uzyskać szczegółowe informacje zobacz [Azure bezpłatnej wersji próbnej](/pricing/free-trial/?WT.mc_id=A261C142F). Uzyskaj środków używane wypróbować płatnych usług Azure. Nawet w przypadku, gdy środki są używane w górę, możesz zachować konta i za pomocą bezpłatnej usługi Azure i funkcje, takie jak funkcja aplikacji sieci Web w usłudze Azure aplikacji.
- Systemy operacyjne: Windows 8 lub nowszy, Windows 2008 R2, Windows 7.
- .NET framework 4.0 lub nowszy
- Visual Studio 2010 z dodatkiem SP1 (Professional, Premium, Ultimate lub Express) lub nowszym.


##<a name="download-sample"></a>Pobieranie przykładowych

Pobieranie i uruchamianie próbki [tutaj](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Tworzenie konta usługi multimediów Azure za pomocą portalu Azure

W tej sekcji pokazano, jak utworzyć konto AMS.

1. Zaloguj się w [portalu Azure](https://portal.azure.com/).
2. Kliknij pozycję **+ Nowy** > **multimediów + CDN** > **usługi multimediów**.

    ![Tworzenie usługi multimediów](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. **Tworzenie** konta usługi multimediów wprowadź żądane wartości.

    ![Tworzenie usługi multimediów](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. W polu **Nazwa konta**wpisz nazwę nowego konta AMS. Nazwa konta usługi multimediów są wszystkie liczby litera lub litery bez spacji i długości 3-24 znaków.
    2. W subskrypcji wybierz jeden z różnych subskrypcjach Azure, które mają dostęp do.
    
    2. **Grupa zasobów**wybierz zasób nowym lub istniejącym.  Grupa zasobów to zbiór zasobów, które współużytkują cyklu życia, uprawnień i zasady. Dowiedz się, [w tym miejscu](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. W **lokalizacji**wybierz pozycję regionu geograficznego służy do przechowywania rekordów multimedia i metadanych dla Twojego konta usługi multimediów. Ten obszar jest używany do przetwarzania i przesyłanie strumieniowe multimediów. Tylko dostępne regiony usługi multimediów są wyświetlane w polu listy rozwijanej. 
    
    3. **Miejsca do magazynowania konta**wybierz konto miejsca do magazynowania o podanie magazyn obiektów blob zawartości multimedialnej z konta usługi multimediów. Można wybrać istniejące konto miejsca do magazynowania w tym samym regionu geograficznego jako konta usługi multimediów, lub można utworzyć konto miejsca do magazynowania. W tym samym regionie tworzone jest nowe konto miejsca do magazynowania. Zasady przechowywania nazwy kont są takie same jak w przypadku kont usługi multimediów.

        Dowiedz się więcej o miejsca do magazynowania [w tym miejscu](storage-introduction.md).

    4. Wybierz pozycję **Przypnij do pulpitu nawigacyjnego** , aby wyświetlić postęp wdrożenia konta.
    
7. Kliknij przycisk **Utwórz** w dolnej części formularza.

    Po pomyślnym utworzeniu konta zostanie zmieniony na **Uruchamianie**. 

    ![Ustawienia usługi multimediów](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Aby zarządzać swoim kontem AMS (na przykład przekazywanie klipów wideo, kodowanie składników majątku, monitorować postęp zadania) za pomocą okna **Ustawienia** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Konfigurowanie przesyłania strumieniowego punkty końcowe za pomocą portalu Azure

Podczas pracy z jedną z najbardziej typowe scenariusze jest przedstawiania wideo za pośrednictwem szybkość transmisji bitów adaptacyjne streaming dla klientów usługi multimediów Azure. Usługi multimediów obsługuje następujące adaptacyjne szybkość transmisji bitów streaming technologii: HTTP Live Streaming (HLS), gładkie Streaming ŁĄCZNIKA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp).

Usługi multimediów zawiera opakowania dynamiczne, umożliwiające dostarczanie usługi adaptacyjne szybkość transmisji bitów zawartości MP4 zakodowany w streaming formaty obsługiwane przez usługi multimediów (KRESKOWANIA MPEG, HLS, gładkie Streaming, obr. / min) tylko na czas, bez konieczności przechowywanie wersji wstępnie detaliczny każdego z nich streaming format.

Aby skorzystać z opakowania dynamiczne, należy wykonać następujące czynności:

- Kodowanie pliku kodery (źródło) do zestawu plików adaptacyjne szybkość transmisji bitów MP4 (kodowania czynności przedstawiono w dalszej części tego samouczka).  
- Tworzenie co najmniej jeden jednostki przesyłanie strumieniowe *Przesyłanie strumieniowe punktu końcowego* z której zamierzasz dostarczania zawartości. Poniżej pokazano, sposób zmieniania liczby jednostek przesyłanie strumieniowe.

Dynamiczne opakowania, potrzebne do przechowywania i zapłacić za pliki w formacie jednego miejsca do magazynowania i usługi multimediów tworzy i służy właściwej reakcji według żądaniami od klienta.

Aby utworzyć i zmień liczbę streaming zastrzeżone jednostki, wykonaj następujące czynności:


1. W oknie **Ustawienia** kliknij pozycję **punkty końcowe strumieniowych**. 

2. Kliknij przycisk domyślne streaming punktu końcowego. 

    Zostanie wyświetlone okno **Domyślne STREAMING szczegóły punktu KOŃCOWEGO** .

3. Aby określić liczbę jednostek przesyłanie strumieniowe, przesuń suwak **Streaming jednostki** .

    ![Przesyłanie strumieniowe jednostki](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Kliknij przycisk **Zapisz** , aby zapisać zmiany.

    >[AZURE.NOTE]Przydział wszelkie nowe jednostki może potrwać do 20 minut.

##<a name="create-and-configure-a-visual-studio-project"></a>Tworzenie i konfigurowanie projektu programu Visual Studio

1. Tworzenie nowej aplikacji konsoli C# Visual Studio 2013, Visual Studio 2012 lub program Visual Studio 2010 z dodatkiem SP1. Wprowadź **nazwę**, **lokalizację**i **nazwę rozwiązanie**, a następnie kliknij **przycisk OK**.

2. Użyj funkcji Pakowanie NuGet [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) zainstalować **Rozszerzenia SDK .NET usług multimediów Azure**.  Multimedia usługi .NET SDK rozszerzenia to zestaw funkcji Pomocnik, które będą Uprość kod i ułatwić rozwinąć za pomocą usługi multimediów i metody rozszerzenia. Również instalowania tego pakietu, instaluje **Multimediów usług .NET SDK** oraz dodaje wszystkie wymagane zależności.

3. Dodaj odwołanie do zestawu System.Configuration. Ten zestaw zawiera klasy **System.Configuration.ConfigurationManager** , która jest używana na dostęp do plików konfiguracji, na przykład App.config.

4. Otwieranie pliku App.config (Dodaj go do projektu, jeśli nie został dodany domyślnie) i dodać sekcję *appSettings* do pliku. Ustaw wartość dla usługi multimediów Azure nazwy i konta klucz konta, jak pokazano w poniższym przykładzie. Aby uzyskać informacje o kluczu oraz nazwę konta, przejdź do [portalu Azure](https://portal.azure.com/) i wybierz konto AMS. Następnie wybierz **Ustawienia** > **klawiszy**. Zarządzanie windows klawiszy zawiera nazwę konta, a zostanie wyświetlona kluczy podstawowych i pomocniczych.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. Zastąpienie istniejącego **przy użyciu** raportów na początku pliku Plik Program.cs poniższy kod.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Utwórz nowy folder w katalogu projektów i skopiuj chcesz kodowanie i przesyłać strumieniowo lub stopniowe pobieranie pliku MP4 lub wmv. W tym przykładzie jest używana ścieżka "C:\VideoFiles".

##<a name="connect-to-the-media-services-account"></a>Nawiązywanie połączenia z kontem usługi multimediów

Podczas używania usługi multimediów z .NET, należy użyć klasy **CloudMediaContext** dla większości usług multimediów zadania programowania: połączenie z kontem usługi multimediów; Tworzenie, aktualizowanie, uzyskiwanie dostępu do i usuwanie następujących obiektów: składniki majątku trwałego, zadania, zasady dostępu, Locator, plików itp.

Zastąpienie domyślnej klasy programu poniższy kod. Kod pokazano, jak odczytywać wartości połączenia z pliku App.config i jak utworzyć obiektu **CloudMediaContext** , aby połączyć się z usługami multimediów. Aby uzyskać więcej informacji o nawiązywaniu połączeń z usługi multimediów zobacz [Łączenie się usługi multimediów z Media SDK usług dla środowiska .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx).

Funkcja **główne** wywołuje metody, które zostaną określone w tej sekcji.

    class Program
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;

        static void Main(string[] args)
        {
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##<a name="create-a-new-asset-and-upload-a-video-file"></a>Utwórz nowy trwały i przekaż plik wideo

Usługi multimediów możesz przekazać (lub mogły zjeść tej ostatniej) cyfrowych plików do środka trwałego. Jednostki **zasobów** może zawierać klip wideo, audio, obrazy, miniatur zbiorów, tekst ścieżki i napisy plików (i metadanych dotyczących tych plików.)  Po przesłaniu są pliki zawartości są bezpiecznie przechowywane w chmurze dla dalszego przetwarzania i przesyłania strumieniowego. Pliki w tym składniku aktywów są nazywane **Plikami zasobów**.

Metoda **UploadFile** określonych poniżej połączeń **CreateFromFile** (zdefiniowane w rozszerzeniach SDK .NET). **CreateFromFile** tworzy nowy trwały, do którego zostało przesłane określony plik źródłowy.

Metoda **CreateFromFile** pobiera **AssetCreationOptions** , które pozwala określić jedną z następujących opcji tworzenia zawartości:

- **Brak** — bez szyfrowania jest używany. Jest to wartość domyślna. Należy zauważyć, że podczas korzystania z tej opcji, zawartość nie jest chroniony podczas przesyłania lub spoczywa w magazynie.
Jeśli planujesz do przeprowadzania MP4 za pomocą stopniowego pobierania, użyj tej opcji.
- **StorageEncrypted** - Użyj tej opcji powoduje szyfrowanie Wyczyść zawartość lokalnie używa szyfrowania-256 bitowego szyfrowania AES (Advanced Standard), które przekazuje go do magazynu Azure, w której jest przechowywany szyfrowane na pozostałych. Zasoby chronione za pomocą szyfrowania miejsca do magazynowania automatycznie bez szyfrowania i umieszczone w systemie zaszyfrowanego pliku przed kodowanie i opcjonalnie ponownie szyfrowane przed przekazaniem jako nowy trwały dane wyjściowe. W przypadku użycia podstawowego szyfrowania miejsca do magazynowania jest, gdy chcesz zabezpieczyć pliki multimedialne wprowadzania wysokiej jakości z silnego szyfrowania spoczynku na dysku.
- **CommonEncryptionProtected** - Użyj tej opcji, jeśli przekazujesz zawartość, która już szyfrowane i chronione za pomocą szyfrowania typowych lub DRM PlayReady (na przykład wygładzonymi przesyłanie strumieniowe chroniony z PlayReady DRM).
- **EnvelopeEncryptionProtected** — Użyj tej opcji, jeśli wysyłasz HLS zaszyfrowanych za pomocą AES. Należy zauważyć, że pliki musi zostały zakodowany i szyfrowane przez Menedżera Przekształcanie.

Metoda **CreateFromFile** umożliwia też określić zwrotnego, aby raportować postęp przekazywania pliku.

W poniższym przykładzie jest określona **Brak** opcji zawartości.

Dodaj następujące metody klasy programu.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


##<a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Kodowanie pliku źródłowego do zestawu plików MP4 adaptacyjne szybkość transmisji bitów

Po ingesting elementów do usługi multimediów, może być multimediów zakodowany, transmuxed oznaczenie znakiem wodnym i tak dalej, przed przekazaniem do klientów. Działania te są planowane i wykonywane wielu wystąpień roli tła zapewnienie wysokiej wydajności i dostępności. Działania te są nazywane zadania, a każde zadanie składa się z Atomowej zadania, które wykonać pracy rzeczywistej w pliku zasobów.

Jak wspomniano wcześniej, podczas pracy z usługi multimediów Azure, jest jednym z najbardziej typowych scenariuszy przedstawiania szybkość transmisji bitów adaptacyjne streaming dla klientów. Usługi multimediów dynamicznie spakować zestawu adaptacyjne szybkość transmisji bitów MP4 plików do jednego z następujących formatów: HTTP Live Streaming (HLS), gładkie Streaming ŁĄCZNIKA MPEG i obr. / min (w przypadku tylko licencjobiorców Adobe PrimeTime-dostęp).

Aby skorzystać z dynamicznego opakowania, musisz wykonaj następujące czynności:

- Kodowanie lub kod transakcji usługi kodery (źródło) plików na zbiór adaptacyjne szybkość transmisji bitów MP4 pliki lub pliki wygładzonymi Streaming adaptacyjne szybkość transmisji bitów.  
- Uzyskaj co najmniej jedną jednostkę przesyłanie strumieniowe przesyłanie strumieniowe punktu końcowego, z której ma zostać dostarczania zawartości.

Poniższy kod przedstawia sposób przesyłania kodowania zadania. Zadania zawiera jednego zadania, która określa kod transakcji plik kodery w zestawie adaptacyjne szybkość transmisji bitów MP4s przy użyciu **Media Encoder standardowy**. Kod przesyła zadanie i czeka, dopóki nie zakończy się.

Po zakończeniu zadania będzie można przesyłać strumieniowo do zawartości lub stopniowe pobieranie plików MP4, które zostały utworzone na podstawie przekodowanie.
Zauważ, że nie muszą mieć więcej niż 0 jednostki przesyłanie strumieniowe aby stopniowo pobierać pliki MP4.

Dodaj następujące metody klasy programu.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##<a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Publikowanie elementu i uzyskać adresy URL przesyłanie strumieniowe i stopniowego pobierania

Przesyłanie strumieniowe lub pobieranie środka trwałego, należy najpierw "przed opublikowaniem", tworząc locator. Locator zapewniają dostęp do plików przechowywanych w elementu. Usługi multimediów obsługuje dwa typy Locator: Locator OnDemandOrigin, używanym do przesyłania strumieniowego multimediów (na przykład MPEG KRESKI, HLS lub gładkie Streaming) i Locator podpis programu Access (SA) używany do pobierania plików multimedialnych.

Po utworzeniu Locator, można tworzyć adresy URL, które są używane do przesyłania strumieniowego lub pobierania plików.


Gładkie przesyłanie strumieniowe przesyłanie strumieniowe adres URL ma następujący format:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Przesyłanie strumieniowe adresem URL HLS ma następujący format:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Przesyłanie strumieniowe adresem URL MPEG ŁĄCZNIKA ma następujący format:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

Adres URL skojarzeń zabezpieczeń używany do pobierania plików ma następujący format:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Rozszerzenia multimediów usług .NET SDK zapewniają wygodny Pomocnik metody, które zwracają sformatowany adresy URL dla opublikowanych elementów zawartości.

Poniższy kod używa rozszerzenia SDK .NET do tworzenia Locator i uzyskiwanie streaming i adresy URL pobierania progresywnego. Kod zawiera również sposób pobierania plików w folderze lokalnym.

Dodaj następujące metody klasy programu.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##<a name="test-by-playing-your-content"></a>Testowanie odtwarzając zawartości  

Po uruchomieniu programu zdefiniowane w poprzedniej sekcji, adresy URL podobny do następującego będą wyświetlane w oknie konsoli.

Adaptacyjne przesyłanie strumieniowe adresy URL:

Gładkie strumieniowego przesyłania

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG ŁĄCZNIKA

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Adresy URL pobierania progresywnego (audio i wideo).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Do strumieniowego przesyłania wideo, za pomocą [Usługi Azure odtwarzacza](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Aby przetestować stopniowego pobierania, wklej adres URL w przeglądarce (na przykład programu Internet Explorer, Chrome lub Safari).


##<a name="next-steps-media-services-learning-paths"></a>Następne kroki: Ścieżki szkoleniowe dla użytkowników usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### <a name="looking-for-something-else"></a>Szukasz czegoś innego?

Jeśli w tym temacie nie zawiera, co możesz zostały oczekiwano, czy nie brakuje czegoś lub w inny sposób nie spełnia określonych wymagań, podaj nam swoją opinię za pomocą wątku Disqus poniżej.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/
