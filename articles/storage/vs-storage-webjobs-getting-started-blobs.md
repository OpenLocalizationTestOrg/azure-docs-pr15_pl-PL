<properties
    pageTitle="Wprowadzenie do obiektów blob miejsca do magazynowania i Visual Studio połączenia usług (WebJob projektów) | Microsoft Azure"
    description="Jak rozpocząć korzystanie z magazynem obiektów Blob w projekcie WebJob po nawiązaniu połączenia z magazynem Azure za pomocą programu Visual Studio połączony usług."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Wprowadzenie do obiektów Blob platformy Azure miejsca do magazynowania i Visual Studio połączenia usług (WebJob projektów)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Omówienie

Ten artykuł zawiera C# przykłady pokazujące wyzwalanie procesu obiektów blob platformy Azure została utworzona lub zaktualizowana. Przykłady kodu przy użyciu wersji [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) 1.x. Po dodaniu konta miejsca do magazynowania do projektu WebJob za pomocą okna dialogowego Visual Studio **Dodaj usługi połączone** odpowiedniego pakietu Azure NuGet miejsca do magazynowania jest zainstalowany, odpowiednie odwołania .NET są dodawane do projektu i parametry połączenia dla konta przestrzeni dyskowej są aktualizowane w pliku App.config.



## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Jak funkcja wyzwalacza obiektów blob została utworzona lub zaktualizowana

W tej sekcji przedstawiono sposób użycia atrybutu **BlobTrigger** .

 **Uwaga:** WebJobs SDK skanuje pliki dziennika, aby obejrzeć dla nowych lub zmienionych obiektów blob. Ten proces jest powolne założenia; funkcja może nie uzyskać uruchomienie Poczekaj kilka minut, aż lub już po utworzeniu obiektów blob.  Jeśli aplikacja wymaga natychmiast przetwarza obiektów blob, zaleca się tworzenie wiadomości kolejki podczas tworzenia obiektów blob i użyj atrybutu [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) zamiast atrybutu **BlobTrigger** w funkcji, która przetwarza to.

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

Kod kopiuje tylko obiektów blob o nazwach rozpoczynających się od "oryginalny-". Na przykład *Blob1.txt oryginał* w kontenerze *wprowadzania* są kopiowane do *Kopiowania Blob1.txt* w kontenerze *dane wyjściowe* .

Jeśli musisz określić wzorzec nazw nazw obiektów blob, zawierające nawiasy klamrowe w nazwie dwukrotnie nawiasy klamrowe. Na przykład, jeśli chcesz znaleźć obiektów blob w kontenerze *obrazy* o nazwach w następujący sposób:

        {20140101}-soundfile.mp3

Użyj tego dla deseniu:

        images/{{20140101}}-{name}

W tym przykładzie wartości z *pola Nazwa* symbolu zastępczego jest *soundfile.mp3*.

### <a name="separate-blob-name-and-extension-placeholders"></a>Symbole zastępcze nazwy i rozszerzenia oddzielnych obiektów blob

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

## <a name="types-that-you-can-bind-to-blobs"></a>Typy, które można powiązać obiektów blob

Za pomocą atrybutu **BlobTrigger** na następujących typów:

* **ciąg**
* **Elementu TextReader**
* **Strumień**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Inne typy rozszeregować przez [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

Jeśli chcesz pracować bezpośrednio z konta Azure miejsca do magazynowania, możesz dodać parametr **CloudStorageAccount** w podpisie metody.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Wprowadzenie tekstu obiektów blob zawartości przez powiązanie ciąg

Jeżeli oczekuje blob tekstu **BlobTrigger** można stosować do parametru **ciągu** . Poniższy przykład kodu wiąże blob tekst parametru **ciągu** o nazwie **logMessage**. Funkcja użyje parametru do zapisywania zawartości to WebJobs SDK pulpitu nawigacyjnego.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Wprowadzenie seryjnych obiektów blob zawartości przy użyciu ICloudBlobStreamBinder

W poniższym przykładzie kodu użyto klasy, która wykonuje **ICloudBlobStreamBinder** , aby włączyć atrybut **BlobTrigger** chcesz powiązać obiektów blob typu **WebImage** .

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

Kod powiązania **WebImage** znajduje się w klasie **WebImageBinder** , która pochodzi z **ICloudBlobStreamBinder**.

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

## <a name="how-to-handle-poison-blobs"></a>Jak obsługiwać poison obiektów blob

Gdy funkcja **BlobTrigger** nie powiedzie się, zestawu SDK połączeń go ponownie, w przypadku niepowodzenia spowodowana przejściowych błędu. Jeśli błąd jest spowodowane zawartość obiektów blob, funkcja zakończy się niepowodzeniem, każdorazowo spróbuje procesu to. Domyślnie zestawu SDK połączeń funkcji do 5 godzin dla danego obiektów blob. Jeśli kończy się niepowodzeniem, spróbuj piątym, zestawu SDK dodaje wiadomości do kolejki o nazwie *webjobs-blobtrigger-poison*.

Konfiguruje się maksymalna liczba prób. Tego samego ustawienia [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) jest używana do obsługi poison obiektów blob i kolejki poison postępowanie z wiadomościami.

Wiadomości kolejki dla poison obiektów blob jest obiektem JSON, który zawiera następujące właściwości:

* FunctionId (w formacie *{Nazwa WebJob}*. Funkcje. *{Nazwa funkcji}*, na przykład: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" lub "PageBlob")
* ContainerName
* BlobName
* ETag (identyfikator wersję obiektów blob, na przykład: "0x8D1DC6E70A277EF")

W następującym przykładzie kodu funkcja **CopyBlob** ma kod powodującą niepowodzenie każdorazowo nazywany. Po zestawu SDK wymaga maksymalną liczbę prób, wiadomość zostanie utworzona w kolejce poison obiektów blob i tę wiadomość jest przetwarzana przez funkcję **LogPoisonBlob** .

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

Zestaw SDK automatycznie deserializes wiadomości JSON. Oto klasy **PoisonBlobMessage** :

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>Algorytm ankieta obiektów blob

WebJobs SDK skanuje wszystkie kontenery określony przez atrybuty **BlobTrigger** na uruchamianie aplikacji. Na koncie dużych magazynowania to skanowanie może potrwać trochę czasu, więc może być trochę czasu, zanim znajdują się w nowych obiektów blob i **BlobTrigger** funkcje są wykonywane.

Wykrywanie nowych lub zmienionych obiektów blob po uruchomieniu aplikacji, zestawu SDK okresowo odczytuje z dzienników magazyn obiektów blob. Dzienniki obiektów blob są buforowane i tylko fizycznie zapisane co 10 minut lub tak, więc może być istotne opóźnienie, po obiektów blob została utworzona lub zaktualizowana przed odpowiadający mu wykonuje funkcji **BlobTrigger** .

Istnieje wyjątek dla obiektów blob tworzonych przy użyciu atrybutu **obiektów Blob** . Gdy WebJobs SDK jest tworzony nowy obiektów blob, przekazuje nowych obiektów blob bezpośrednio do dowolnego pasujące funkcji **BlobTrigger** . W związku z tym jeśli masz łańcucha obiektów blob dane wejściowe i wyjściowe zestawu SDK może przetwarzać ich wydajność. Jeśli chcesz małe opóźnienia uruchamiania usługi obiektów blob przetwarzania funkcje dla obiektów blob, które są tworzone lub zaktualizowany w inny sposób, zalecamy jednak za pomocą **QueueTrigger** zamiast **BlobTrigger**.

### <a name="blob-receipts"></a>Potwierdzenia przeczytania obiektów blob

WebJobs SDK służy do sprawdzenia, czy nie zawiera funkcji **BlobTrigger** jest wywoływana więcej niż jeden raz na tym samym obiektów blob nową lub zaktualizowaną. Jest to zachowanie *obiektów blob potwierdzeń* w celu ustalenia, czy wersję danego obiektów blob została przetworzona.

Potwierdzenia przeczytania obiektów blob są przechowywane w kontenerze o nazwie *azure webjobs hostów* w oknie konta magazynu platformy Azure określony przez ciąg połączenia AzureWebJobsStorage. Potwierdzenia obiektów blob obejmuje następujące informacje:

* Funkcja nazywanym dla obiektów blob ("*{Nazwa WebJob}*. Funkcje. *{Nazwa funkcji}*", na przykład:"WebJob1.Functions.CopyBlob")
* Nazwa kontenera
* Typ obiektów blob ("BlockBlob" lub "PageBlob")
* Nazwy obiektów blob
* ETag (identyfikator wersji obiektów blob, na przykład: "0x8D1DC6E70A277EF")

Jeśli chcesz wymusić przetwarzania obiektów blob, możesz ręcznie usunąć potwierdzenia obiektów blob dany obiekt blob z kontenera *azure webjobs hostów* .

## <a name="related-topics-covered-by-the-queues-article"></a>Tematy pokrewne objętych artykułem kolejki

Aby uzyskać informacje o obsługę przetwarzania obiektów blob wyzwalane przez komunikat kolejki lub WebJobs SDK scenariuszy, które nie dotyczą blob przetwarzania, zobacz, [jak korzystania z magazynu Azure kolejki z zestawu SDK WebJobs](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Tematy pokrewne w tym artykule omówione są następujące:

* Funkcje asynchroniczne
* Wielu wystąpień
* Bezpiecznie zamknięty
* Używanie atrybutów WebJobs SDK w treści funkcji
* Ustaw parametry połączenia SDK w kodzie.
* Ustawianie wartości dla WebJobs SDK konstruktora parametrów w kodzie
* Konfigurowanie **MaxDequeueCount** obsługi poison obiektów blob.
* Funkcja wyzwalacza ręcznie
* Pisanie dzienników

## <a name="next-steps"></a>Następne kroki

W tym artykule udostępniła przykłady kodu, pokazujące sposób obsługi typowe scenariusze dotyczące pracy z obiektami blob Azure. Aby uzyskać więcej informacji o używaniu Azure WebJobs i WebJobs SDK, zobacz [zasoby dokumentacji Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).
