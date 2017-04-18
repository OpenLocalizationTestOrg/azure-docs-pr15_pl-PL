<properties
    pageTitle="Połączenie danych: strumienia wartości wejściowych ze strumienia zdarzenia danych | Microsoft Azure"
    description="Informacje na temat konfigurowania połączenia danych z analizy strumieniu o nazwie "wartości wejściowych". Wartości wejściowych dodawania strumienia danych zdarzenia, a także odwołać danych."
    keywords="strumień danych, połączenia danych, strumienia zdarzenia"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Połączenie danych: więcej informacji na temat danych wejściowych strumienia z zdarzeń w celu analizy strumieniu

Połączenia danych z analizy strumieniu jest strumienia danych zdarzeń ze źródła danych. Jest to "dane wejściowe". Analizy strumieniu ma najlepszych Integracja z Azure strumienia źródeł Centrum zdarzenia, Centrum IoT i obiektów Blob magazynowanie danych można subskrypcję takich samych lub różnych Azure jako zadanie analizy.

## <a name="data-input-types-data-stream-and-reference-data"></a>Typy wprowadzania danych: danych strumienia i odwołań
Jak danych zostanie przypisany do źródła danych, jest używane przez zadanie analizy strumieniu i przetwarzania w czasie rzeczywistym. Dane wejściowe są podzielone na dwa różne typy: danych strumienia wartości wejściowych i odwołanie wartości danych wejściowych.

### <a name="data-stream-inputs"></a>Wartości wejściowych strumienia danych
Strumień danych jest bez ograniczeń kolejność zdarzeń pochodzące z czasem. Zadania analizy strumieniu musi zawierać co najmniej jeden dane wejściowe strumienia zużyte i przekształcania przez zadanie. Magazyn obiektów blob, koncentratory zdarzeń i koncentratory IoT są obsługiwane jako źródeł danych wejściowych strumienia danych. Koncentratory wydarzenia są używane do gromadzenia strumienie zdarzeń z wielu urządzeń i usług, takich jak źródła działań społecznościowych, informacji handlowych giełdowy lub dane z czujników. Koncentratory IoT są zoptymalizowane na potrzeby zbierania danych z połączenia urządzeń w scenariuszach Internet czynności (IoT).  Magazyn obiektów blob może służyć jako źródło wprowadzania dla ingesting zbiorczego danych jako strumienia.  

### <a name="reference-data"></a>Dane źródłowe
Analizy strumieniu obsługuje drugi typ danych wejściowych znany jako danych źródłowych. To jest statyczną lub powoli zmiany w czasie i jest zazwyczaj używany do wykonywania korelacji i wyszukania dane pomocnicze. Magazyn obiektów Blob platformy Azure jest obecnie obsługiwane tylko źródło wprowadzania dla danych źródłowych. Blob źródła danych odwołania są ograniczone do 100MB rozmiar.
Aby dowiedzieć się, jak utworzyć odwołanie wartości danych wejściowych, zobacz [Używanie odwołania danych](stream-analytics-use-reference-data.md)  

## <a name="create-a-data-stream-input-with-an-event-hub"></a>Tworzenie danych wejściowych strumienia z koncentratora zdarzenia

[Azure zdarzenia koncentratory](https://azure.microsoft.com/services/event-hubs/) są bardzo skalowalna publikowanie Subskrybuj ingestor zdarzenia. Można go zbieranie miliony wydarzeń na sekundę, dzięki czemu można przetwarzać i analizowanie dużych ilości danych tworzone przez połączenia urządzeń i aplikacji. Jest jednym z najczęściej używanych danych wejściowych dla analizy strumieniu. Koncentratory zdarzeń i analizy strumieniu razem zapewniają klientom kompleksowe rozwiązania analiz w czasie rzeczywistym. Zdarzenia Koncentratory umożliwiają klientom kanału informacyjnego zdarzeń w Azure w czasie rzeczywistym i analizy strumieniu zadania można przetwarzać je w czasie rzeczywistym. Na przykład klientów można wysłać kliknięć sieci web, czujnik Odczyt, zdarzeń dziennika online do koncentratorów zdarzenia i Utwórz analizy strumieniu zadań w celu filtrowania w czasie rzeczywistym, agregowanie i korelacji za pomocą koncentratorów zdarzenia jako strumienie danych wejściowych.

Należy pamiętać, że sygnatura czasowa domyślne zdarzeń pochodzące z koncentratorów zdarzenia w analizy strumieniu jest sygnatura czasowa, które otrzymano wydarzenia w Centrum zdarzenia, czyli EventEnqueuedUtcTime. Do przetwarzania danych jako strumień przy użyciu sygnatura czasowa w ładunku zdarzenia, należy użyć [Sygnatura CZASOWA według](https://msdn.microsoft.com/library/azure/dn834998.aspx) słowa kluczowego.

### <a name="consumer-groups"></a>Grupy konsumentów

Każdy obraz wejściowy Centrum zdarzeń analizy strumieniu należy skonfigurować mieć własną grupę dla klientów indywidualnych. Jeśli zadanie zawiera samosprzężenia lub wielu wartości wejściowych, podjęcia jakichś działań, mogą być odczytywane przez więcej niż jeden czytnik poniżej, co wpływa na liczbę czytelników w grupie pojedynczy dla klientów indywidualnych. Aby uniknąć przekroczenie limitu Centrum zdarzeń czytników 5 dla każdej grupy dla klientów indywidualnych na partition, jest najlepszym rozwiązaniem grupę dla klientów indywidualnych, dla każdego zadania analizy strumieniu. Należy zauważyć, że jest również limit 20 grupy dla klientów indywidualnych na Centrum zdarzenia. Aby uzyskać szczegółowe informacje zobacz [Przewodnik programowania koncentratory zdarzenia](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-event-hub-as-an-input-data-stream"></a>Konfigurowanie Centrum zdarzenia jako strumień danych wejściowych

W poniższej tabeli opisano każdej właściwości, w przypadku, gdy karta z jej opis wprowadzania Centrum:

| NAZWA WŁAŚCIWOŚCI | OPIS |
|------|------|
| Alias wprowadzania danych | Przyjazną nazwę, które są używane w kwerendzie zadania do odwołanie tych danych wejściowych |
| Usługa Bus Namespace | Przestrzeń nazw Bus usługi jest kontenerem zestawu wiadomości podmiotów. Po utworzeniu nowego koncentratora zdarzenia utworzono nazw Bus usługi. |
| Centrum zdarzenia | Nazwa Twoim Centrum zdarzeń wprowadzania. |
| Nazwa zasady Centrum zdarzenia | Zasady dostępu do udostępnionego można tworzyć na karcie Konfigurowanie Centrum zdarzenia. Każdy zasady dostępu udostępnione będzie mieć nazwę uprawnienia, które można ustawić i klawisze dostępu. |
| Klucz zasad Centrum zdarzenia | Klawisz dostępu udostępnione używane do uwierzytelnienia dostęp do nazw Bus usługi. |
| Grupa dla klientów indywidualnych Centrum wydarzenia (opcjonalnie) | Grupa dla klientów indywidualnych, aby mogły zjeść tej ostatniej danych z poziomu Centrum zdarzenia. Jeśli nie zostanie określony, analizy strumieniu zadań przy użyciu grupy domyślne dla klientów indywidualnych mogły zjeść tej ostatniej danych z poziomu Centrum zdarzenia. Zaleca się używanie odrębnych dla klientów indywidualnych grupy dla każdego zadania analizy strumieniu. |
| Format szeregowania zdarzenia | Aby upewnić się, że zapytań działają w ten sposób można się spodziewać, analizy strumieniu musi wiedzieć, który format szeregowania (JSON, CSV lub Avro) używanego do obsługi poczty przychodzącej strumienie danych. |
| Kodowanie | UTF-8 jest obsługiwany tylko format kodowania w tej chwili. |

Gdy pochodzą dane ze źródła Centrum zdarzenia, masz dostęp do kilku pól metadanych w kwerendzie analizy strumieniu. Poniższa tabela zawiera listę pól i ich opis.

| WŁAŚCIWOŚĆ | OPIS |
|------|------|
| EventProcessedUtcTime | Data i godzina zdarzenia została przetworzona przez analizy strumieniu. |
| EventEnqueuedUtcTime | Data i godzina zdarzenia została odebrana przez koncentratory zdarzenia. |
| PartitionId | Identyfikator od zera partition wprowadzania karty. |

Na przykład może tworzyć zapytania podobnej do następującej:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-an-iot-hub-data-stream-input"></a>Tworzenie wprowadzania strumienia IoT centrum danych

Azure Centrum Iot jest wysoce skalowalna publikowanie Subskrybuj zoptymalizowana pod kątem scenariuszy IoT ingestor zdarzenia.
Należy pamiętać, że sygnatura czasowa domyślne zdarzeń pochodzące z koncentratorów IoT do analizy strumieniu jest sygnatura czasowa, która zdarzenie otrzymano w Centrum IoT, czyli EventEnqueuedUtcTime. Do przetwarzania danych jako strumień przy użyciu sygnatura czasowa w ładunku zdarzenia, należy użyć [Sygnatura CZASOWA według](https://msdn.microsoft.com/library/azure/dn834998.aspx) słowa kluczowego.

> [AZURE.NOTE] Mogą być przetwarzane tylko wiadomości wysyłane z właściwością DeviceClient.

### <a name="consumer-groups"></a>Grupy konsumentów

Każdy obraz wejściowy Centrum IoT analizy strumieniu należy skonfigurować mieć własną grupę dla klientów indywidualnych. Jeśli zadanie zawiera samosprzężenia lub wielu wartości wejściowych, podjęcia jakichś działań, mogą być odczytywane przez więcej niż jeden czytnik poniżej, co wpływa na liczbę czytelników w grupie pojedynczy dla klientów indywidualnych. Aby uniknąć przekroczenie limitu Centrum IoT czytników 5 dla każdej grupy dla klientów indywidualnych na partition, jest najlepszym rozwiązaniem grupę dla klientów indywidualnych, dla każdego zadania analizy strumieniu.

### <a name="configure-iot-hub-as-an-data-stream-input"></a>Konfigurowanie Centrum IoT jako strumień danych wejściowych

W poniższej tabeli opisano każdą właściwość na karcie wprowadzania Centrum IoT z jej opis:

| NAZWA WŁAŚCIWOŚCI | OPIS |
|------|------|
| Alias wprowadzania danych | Przyjazna nazwa będzie używana w kwerendzie zadania zostać utworzone odwołanie tych danych wejściowych. |
| Centrum IoT | Centrum IoT jest kontenerem dla zestawu podmioty wiadomości. |
| Punkt końcowy | Nazwa punktu końcowego usługi IoT Centrum. |
| Nazwa zasady udostępniania | Zasady udostępniania udzielić dostępu do Centrum IoT. Każdy zasady dostępu udostępnione będzie mieć nazwę uprawnienia, które można ustawić i klawisze dostępu. |
| Klucz zasad udostępniania | Klawisz dostępu udostępnione używane do uwierzytelnienia dostęp do Centrum IoT. |
| Grupa dla klientów indywidualnych (opcjonalnie) | Grupa dla klientów indywidualnych, aby mogły zjeść tej ostatniej danych z poziomu Centrum IoT. Jeśli nie zostanie określony, analizy strumieniu zadań przy użyciu grupy domyślne dla klientów indywidualnych mogły zjeść tej ostatniej danych z poziomu Centrum IoT. Zaleca się używanie odrębnych dla klientów indywidualnych grupy dla każdego zadania analizy strumieniu. |
| Format szeregowania zdarzenia | Aby upewnić się, że zapytań działają w ten sposób można się spodziewać, analizy strumieniu musi wiedzieć, który format szeregowania (JSON, CSV lub Avro) używanego do obsługi poczty przychodzącej strumienie danych. |
| Kodowanie | UTF-8 jest obsługiwany tylko format kodowania w tej chwili. |

Gdy pochodzą dane ze źródła Centrum IoT, masz dostęp do kilku pól metadanych w kwerendzie analizy strumieniu. Poniższa tabela zawiera listę pól i ich opis.

| WŁAŚCIWOŚĆ | OPIS |
|------|------|
| EventProcessedUtcTime | Data i godzina, która została przetworzona, wydarzenia. |
| EventEnqueuedUtcTime | Data i godzina zdarzenia została odebrana przez Centrum IoT. |
| PartitionId | Identyfikator od zera partition wprowadzania karty. |
| IoTHub.MessageId | Umożliwia przeniesionym dwustronną komunikację w Centrum IoT. |
| IoTHub.CorrelationId | Używane w odpowiedzi na wiadomości i przesyłanie opinii IoT Centrum. |
| IoTHub.ConnectionDeviceId | Identyfikator uwierzytelniony służy do wysyłania tej wiadomości, naniesione wiadomości servicebound przez Centrum IoT. |
| IoTHub.ConnectionDeviceGenerationId | GenerationId uwierzytelnionych urządzenie można wysłać tę wiadomość naniesione wiadomości servicebound przez Centrum IoT. |
| IoTHub.EnqueuedTime | Godzina otrzymania wiadomości przez Centrum IoT. |
| IoTHub.StreamId | Właściwość zdarzenia niestandardowego dodane przez urządzenia nadawcy. |

## <a name="create-a-blob-storage-data-stream-input"></a>Tworzenie wprowadzania strumienia danych magazyn obiektów Blob

W przypadku scenariuszy z dużą ilością niestrukturalne dane mają być przechowywane w chmurze magazyn obiektów Blob oferuje efektywne pod względem kosztów i skalowalność rozwiązanie. Dane w [magazynie obiektów Blob](https://azure.microsoft.com/services/storage/blobs/) zazwyczaj jest traktowany jako danych "spoczynku", ale mogą być przetwarzane jako strumień danych przez analizy strumieniu. Jeden typowy scenariusz dla wartości wejściowych magazyn obiektów Blob z analizy strumieniu jest przetwarzanie dziennika, gdzie telemetrycznego jest rejestrowany systemu i musi być analizowane i przetwarzane w celu wyodrębnienia ważnych danych.

Należy pamiętać, że sygnatura czasowa domyślne zdarzeń magazyn obiektów Blob podczas analizy strumieniu jest sygnatura czasowa to ostatniej modyfikacji które *isBlobLastModifiedUtcTime*. Do przetwarzania danych jako strumień przy użyciu sygnatura czasowa w ładunku zdarzenia, należy użyć [Sygnatura CZASOWA według](https://msdn.microsoft.com/library/azure/dn834998.aspx) słowa kluczowego.

Należy również zauważyć, że CSV sformatowane wartości wejściowych **wymagają** wiersz nagłówka, aby zdefiniować pola dla zestawu danych. Kolejne pola wierszy nagłówka muszą być wszystkie **unikatowe**.

> [AZURE.NOTE] Analizy strumieniu nie obsługuje dodawanie zawartości do istniejących obiektów blob. Analizy strumieniu będzie wyświetlać tylko obiektów blob raz i wszelkie zmiany wykonać po tym odczytu nie będą przetwarzane. Najlepszym rozwiązaniem jest raz przekazać wszystkie dane i nie dodawać wszelkie dodatkowe zdarzenia w magazynie obiektów blob.

W poniższej tabeli opisano poszczególnych właściwości karta wprowadzania magazyn obiektów Blob z jej opis:

<table>
<tbody>
<tr>
<td>NAZWA WŁAŚCIWOŚCI</td>
<td>OPIS</td>
</tr>
<tr>
<td>Alias wprowadzania danych</td>
<td>Przyjazna nazwa będzie używana w kwerendzie zadania zostać utworzone odwołanie tych danych wejściowych.</td>
</tr>
<tr>
<td>Konto miejsca do magazynowania</td>
<td>Nazwa konta miejsca do magazynowania, gdzie znajdują się pliki obiektów blob.</td>
</tr>
<tr>
<td>Klucz konta miejsca do magazynowania</td>
<td>Klucz tajny, skojarzone z kontem miejsca do magazynowania.</td>
</tr>
<tr>
<td>Kontener miejsca do magazynowania
</td>
<td>Kontenery zapewniają logiczną grupę dla obiektów blob przechowywane w usłudze Microsoft Azure Blob. Gdy wysyłasz obiektów blob usługi obiektów Blob należy określić kontener dla tego obiektów blob.</td>
</tr>
<tr>
<td>Ścieżka prefiks wzorzec [opcjonalne]</td>
<td>Ścieżka pliku używane do znajdowania z obiektami BLOB w określonym kontenerze.
W ścieżce można określić wystąpienia jeden lub więcej z następujących zmiennych 3:<BR>{date}, {czas}<BR>{partition}<BR>Przykład 1: cluster1/dzienniki / {dat} / {time} / {partycje}<BR>Przykład 2: cluster1/dzienniki / {dat}<P>Należy zauważyć, że "*" nie jest dozwolona wartość pathprefix. Tylko prawidłowe <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">znaki obiektów blob platformy Azure</a> są dozwolone.</td>
</tr>
<tr>
<td>Format daty [opcjonalne]</td>
<td>Jeśli token Data jest używana w ścieżce prefiks, można wybrać format daty, w której są zorganizowane plików. Przykład: RRRR MM-DD</td>
</tr>
<tr>
<td>Format godziny [opcjonalne]</td>
<td>Jeśli token czasu jest używany w ścieżce prefiks, określ format czasu, w którym są zorganizowane plików. Jedyna obsługiwana wartość jest obecnie HH.</td>
</tr>
<tr>
<td>Format szeregowania zdarzenia</td>
<td>Aby upewnić się, że zapytań działają w ten sposób można się spodziewać, analizy strumieniu musi wiedzieć, który format szeregowania (JSON, CSV lub Avro) używanego do obsługi poczty przychodzącej strumienie danych.</td>
</tr>
<tr>
<td>Kodowanie</td>
<td>CSV i JSON UTF-8 jest obsługiwany tylko format kodowania w tej chwili.</td>
</tr>
<tr>
<td>Ogranicznika</td>
<td>Analizy strumieniu obsługuje wiele typowych ograniczniki szeregowania danych w formacie CSV. Obsługiwane są następujące wartości przecinek, średnik, odstęp, karta i pionowy pasek.</td>
</tr>
</tbody>
</table>

Gdy pochodzą dane ze źródła magazyn obiektów Blob, masz dostęp do kilku pól metadanych w kwerendzie analizy strumieniu. Poniższa tabela zawiera listę pól i ich opis.

| WŁAŚCIWOŚĆ | OPIS |
|------|------|
| BlobName | Nazwa wprowadzania blob, w którym zdarzenie pochodzi z. |
| EventProcessedUtcTime | Data i godzina zdarzenia została przetworzona przez analizy strumieniu. |
| BlobLastModifiedUtcTime | Datę i godzinę ostatniej modyfikacji obiektów blob. |
| PartitionId | Identyfikator od zera partition wprowadzania karty. |

Na przykład może tworzyć zapytania podobnej do następującej:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````


## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc spróbuj nasze [forum analizy strumieniu Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Następne kroki
Opcje połączeń danych platformy Azure znasz dla zadań analizy strumieniu. Aby dowiedzieć się więcej na temat analizy strumieniu, zobacz:

- [Wprowadzenie do korzystania z analizy strumieniu Azure](stream-analytics-get-started.md)
- [Skalowanie Azure analizy strumieniu zadania](stream-analytics-scale-jobs.md)
- [Dokumentacja języka kwerend analizy strumieniu Azure](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Strumień Azure analizy zarządzania pozostałych interfejsu API odwołania](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
