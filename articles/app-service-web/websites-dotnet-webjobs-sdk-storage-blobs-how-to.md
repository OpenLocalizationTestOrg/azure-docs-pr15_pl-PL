<properties 
    pageTitle="Jak korzystać z zestawu SDK WebJobs magazyn obiektów blob platformy Azure" 
    description="Dowiedz się, jak magazyn obiektów blob platformy Azure za pomocą WebJobs SDK. Wyzwalanie procesu, gdy nowy obiektów blob pojawi się w kontenerze i obsługiwać "blob poison"." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>Jak korzystać z zestawu SDK WebJobs magazyn obiektów blob platformy Azure

## <a name="overview"></a>Omówienie

Ten przewodnik zawiera C# przykłady pokazujące wyzwalanie procesu obiektów blob platformy Azure została utworzona lub zaktualizowana. Przykłady kodu przy użyciu wersji [WebJobs SDK](websites-dotnet-webjobs-sdk.md) 1.x.

Aby uzyskać przykłady kodu, pokazujące, jak tworzyć obiektów blob, Dowiedz się, [jak używać magazyn kolejek Azure z zestawu SDK WebJobs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 
        
Przewodnik założono, możesz dowiedzieć się, [jak utworzyć projekt WebJob w programie Visual Studio z parametry połączenia, które wskaż konta miejsca do magazynowania](websites-dotnet-webjobs-sdk-get-started.md) lub z [wieloma kontami miejsca do magazynowania](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Jak funkcja wyzwalacza obiektów blob została utworzona lub zaktualizowana

W tej sekcji przedstawiono sposób użycia `BlobTrigger` atrybut. 

> [AZURE.NOTE] WebJobs SDK skanuje pliki dziennika, aby obejrzeć dla nowych lub zmienionych obiektów blob. Ten proces jest w czasie rzeczywistym; funkcja może nie uzyskać wyzwalane Poczekaj kilka minut, aż albo już po utworzeniu obiektów blob. Ponadto podstawa [miejsca do magazynowania utworzone dzienniki na "starań"](https://msdn.microsoft.com/library/azure/hh343262.aspx) ; nie jest gwarantowana będzie można zarejestrować wszystkie zdarzenia. W niektórych sytuacjach może zostać pominięte dzienników. Jeśli szybkość i niezawodność ograniczenia dotyczące obiektów blob wyzwalacze nie są akceptowane dla aplikacji, zaleca się tworzenie wiadomości kolejki podczas tworzenia obiektów blob i używać atrybutu [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) zamiast `BlobTrigger` atrybutu w funkcji, która przetwarza to.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Pojedynczy symbol zastępczy dla nazwy obiektów blob z rozszerzeniem  

Poniższy przykład kodu kopiuje blob tekstu, występujące w kontenerze *wprowadzania* w kontenerze *wynik* :

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Konstruktor atrybutu ma parametr ciąg określający nazwę kontenera i symbol zastępczy dla nazwy obiektów blob. W tym przykładzie obiektów blob o nazwie *Blob1.txt* zostanie utworzona w kontenerze *wprowadzania* funkcji tworzy obiektów blob o nazwie *Blob1.txt* w kontenerze *dane wyjściowe* . 

Możesz określić wzorzec nazw z symbolem zastępczym nazwy obiektów blob, jak pokazano w poniższym przykładzie kodu:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Kod kopiuje tylko obiektów blob o nazwach rozpoczynających się od "oryginalnego-". Na przykład *Blob1.txt oryginał* w kontenerze *wprowadzania* są kopiowane do *Kopiowania Blob1.txt* w kontenerze *dane wyjściowe* .

Jeśli potrzebujesz służy do określania nazwy wzorca nazw obiektów blob, które muszą nawiasy klamrowe w nazwie, należy ustawić podwójne nawiasy klamrowe. Na przykład, jeśli chcesz znaleźć BLOB w kontenerze *obrazów* o nazwach w następujący sposób:

        {20140101}-soundfile.mp3

Użyj tego dla deseniu:

        images/{{20140101}}-{name}

W tym przykładzie wartości z *pola Nazwa* symbolu zastępczego jest *soundfile.mp3*. 

### <a name="separate-blob-name-and-extension-placeholders"></a>Symbole zastępcze nazwy i rozszerzenia osobnych blob

Poniższy przykład kodu zmieni rozszerzenie pliku kopiowania obiektów blob wyświetlanych w kontenerze *wprowadzania* w kontenerze *dane wyjściowe* . Kod rejestruje rozszerzenia *wprowadzania* obiektów blob i Ustawia rozszerzenie obiektów blob *dane wyjściowe* *txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a id="types"></a>Typy, które można powiązać z obiektami blob

Możesz użyć `BlobTrigger` atrybutu w następujących typów:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Inne typy rozszeregować przez [ICloudBlobStreamBinder](#icbsb) 

Jeśli chcesz pracować bezpośrednio z konta Azure miejsca do magazynowania, można również dodać `CloudStorageAccount` parametr w podpisie metody.

Przykłady można znaleźć w temacie [obiektów blob powiązanie kodu w repozytorium azure-webjobs-sdk na GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Wprowadzenie tekstu obiektów blob zawartości przez powiązanie ciąg

Jeżeli oczekuje blob tekstu `BlobTrigger` mogą być stosowane do `string` parametru. Poniższy przykład kodu powiąże blob tekstu, aby `string` parametr o nazwie `logMessage`. Funkcja użyje parametru do zapisywania zawartości to WebJobs SDK pulpitu nawigacyjnego. 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Wprowadzenie seryjnych obiektów blob zawartości przy użyciu ICloudBlobStreamBinder

W poniższym przykładzie kodu użyto zajęć, który wykonuje `ICloudBlobStreamBinder` umożliwiające `BlobTrigger` atrybut powiązać blob do `WebImage` typu.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

`WebImage` Wiązanie kod znajduje się w `WebImageBinder` klasy będącej `ICloudBlobStreamBinder`.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>Pobieranie ścieżki blob wyzwalającego blob

Aby uzyskać nazwę kontenera i obiektów blob nazwy obiektów blob, który powoduje uruchomienie funkcji, w tym `blobTrigger` ciąg parametru w podpisie funkcji.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Jak obsługiwać poison obiektów blob

Gdy `BlobTrigger` funkcja kończy się niepowodzeniem, zestawu SDK połączeń go ponownie, w przypadku niepowodzenia spowodowana przejściowych błędu. Jeśli błąd jest spowodowane zawartość obiektów blob, funkcja zakończy się niepowodzeniem, każdorazowo spróbuje procesu to. Domyślnie zestawu SDK połączeń funkcji do 5 godzin dla danego obiektów blob. Jeśli kończy się niepowodzeniem, spróbuj piątym, zestawu SDK dodaje wiadomości do kolejki o nazwie *webjobs-blobtrigger-poison*.

Konfiguruje się maksymalna liczba prób. Tego samego ustawienia [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) jest używana do obsługi poison obiektów blob i kolejki poison postępowanie z wiadomościami. 

Wiadomości kolejki dla poison obiektów blob jest obiektem JSON, który zawiera następujące właściwości:

* FunctionId (w formacie *{Nazwa WebJob}*. Funkcje. *{Nazwa funkcji}*, na przykład: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" lub "PageBlob")
* ContainerName
* BlobName
* ETag (identyfikator wersji obiektów blob, na przykład: "0x8D1DC6E70A277EF")

W następującym przykładzie kodu `CopyBlob` zawiera kod, powodującą niepowodzenie każdorazowo nazywany. Po zestawu SDK wymaga go maksymalna liczba prób, wiadomość zostanie utworzona w kolejce poison blob i tę wiadomość jest przetwarzana przez `LogPoisonBlob` funkcji. 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }
        
        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

Zestaw SDK automatycznie deserializes wiadomości JSON. Oto `PoisonBlobMessage` klasy: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>Algorytm ankieta obiektów blob

WebJobs SDK skanuje wszystkie kontenery określone przez `BlobTrigger` atrybuty na uruchamianie aplikacji. Na koncie dużych magazynowania to skanowanie może zająć trochę czasu, więc może być trochę czasu, zanim znajdują się w nowych obiektów blob i `BlobTrigger` funkcje są wykonywane.

Wykrywanie nowych lub zmienionych obiektów blob po uruchomieniu aplikacji, zestawu SDK okresowo odczytuje z dzienników magazyn obiektów blob. Dzienniki obiektów blob są buforowane i tylko fizycznie zapisane co 10 minut lub tak, dlatego mogą być istotne opóźnienie po obiektów blob została utworzona lub zaktualizowana przed odpowiednie `BlobTrigger` wykonuje funkcji. 

Istnieje wyjątek dla obiektów blob utworzone za pomocą `Blob` atrybut. Gdy WebJobs SDK jest tworzony nowy obiektów blob, przekazaniem nowych obiektów blob bezpośrednio do dowolnego pasujących `BlobTrigger` funkcji. W związku z tym jeśli masz łańcucha obiektów blob dane wejściowe i wyjściowe zestawu SDK może przetwarzać ich wydajność. Ale jeśli chcesz małe opóźnienia uruchamiania usługi blob funkcji przetwarzania dla obiektów blob, które są tworzone lub zaktualizowany w inny sposób, zaleca się używanie `QueueTrigger` zamiast `BlobTrigger`.

### <a id="receipts"></a>Potwierdzenia przeczytania obiektów blob

WebJobs SDK służy do sprawdzenia, która nie `BlobTrigger` funkcja jest wywoływana więcej niż jeden raz na tym samym obiektów blob nową lub zaktualizowaną. Jest to zachowanie *obiektów blob potwierdzeń* w celu ustalenia, czy wersję danego obiektów blob została przetworzona.

Potwierdzenia przeczytania obiektów blob są przechowywane w kontenerze o nazwie *azure webjobs hostów* w oknie konta magazynu platformy Azure określony przez ciąg połączenia AzureWebJobsStorage. Potwierdzenia obiektów blob obejmuje następujące informacje:

* Funkcja nazywanym dla obiektów blob ("*{Nazwa WebJob}*. Funkcje. *{Nazwa funkcji}*", na przykład:"WebJob1.Functions.CopyBlob")
* Nazwa kontenera
* Typ blob ("BlockBlob" lub "PageBlob")
* Nazwy obiektów blob
* ETag (identyfikator wersji blob, na przykład: "0x8D1DC6E70A277EF")

Jeśli chcesz wymusić przetwarzania obiektów blob, możesz ręcznie usunąć potwierdzenia obiektów blob dany obiekt blob z kontenera *azure webjobs hostów* .

## <a id="queues"></a>Tematy pokrewne objętych artykułem kolejki

Aby uzyskać informacje o obsługę przetwarzania obiektów blob wyzwalane przez komunikat kolejki lub WebJobs SDK scenariuszy, które nie dotyczą blob przetwarzania, zobacz, [jak korzystania z magazynu Azure kolejki z zestawu SDK WebJobs](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Tematy pokrewne w tym artykule omówione są następujące:

* Funkcje asynchroniczne
* Wielu wystąpień
* Bezpiecznie zamknięty
* Używanie atrybutów WebJobs SDK w treści funkcji
* Ustaw parametry połączenia SDK w kodzie.
* Ustawianie wartości dla WebJobs SDK konstruktora parametrów w kodzie
* Konfigurowanie `MaxDequeueCount` obsługi poison obiektów blob.
* Funkcja wyzwalacza ręcznie
* Pisanie dzienników

## <a id="nextsteps"></a>Następne kroki

Ten przewodnik udostępniła przykłady kodu, pokazujące sposób obsługi typowe scenariusze dotyczące pracy z obiektami blob Azure. Aby uzyskać więcej informacji o używaniu Azure WebJobs i WebJobs SDK, zobacz [Azure WebJobs zalecane zasoby](http://go.microsoft.com/fwlink/?linkid=390226).
 
