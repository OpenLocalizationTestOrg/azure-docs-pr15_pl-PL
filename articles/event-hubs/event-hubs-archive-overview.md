<properties
    pageTitle="Zdarzenia Azure koncentratory archiwum | Microsoft Azure"
    description="Omówienie funkcji Azure zdarzenia koncentratory archiwum."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="azure-event-hubs-archive"></a>Archiwum koncentratory Azure zdarzenia

Archiwum koncentratory zdarzenia Azure umożliwia automatycznego dostarczania danych strumieniowych w swojej koncentratory zdarzeń do konta magazyn obiektów Blob wyboru z elastycznie do określenia czasu lub rozmiar interwału wybrane. Konfigurowanie archiwum jest szybkie, istnieją żadnych kosztów administracyjnych, aby go uruchomić i automatycznie skale się z koncentratorów zdarzenia [przepustowość jednostki](event-hubs-overview.md#capacity-and-security). Archiwum koncentratory zdarzenie jest najprostszym sposobem na ładowanie przesyłania strumieniowego do Azure i umożliwia skoncentrowanie się na przetwarzania danych zamiast przechwytywania danych.

Azure archiwum koncentratory zdarzenia umożliwia przetwarzanie w czasie rzeczywistym i oparte na partię procesy w tym samym strumieniu. Umożliwia tworzenie rozwiązań, które dzięki potrzeb w czasie. Czy tworzysz partii systemów dzisiaj z okiem kierunku przyszłych przetwarzania w czasie rzeczywistym lub chcesz dodać wydajność zimnej ścieżkę do istniejącego rozwiązania w czasie rzeczywistym, archiwum koncentratory zdarzenia służy do pracy z strumieniowego przesyłania danych.

## <a name="how-event-hubs-archive-works"></a>Jak działa archiwum koncentratory zdarzenia

Koncentratory zdarzenie jest trwałe bufor przechowywania czasu na ingress telemetrycznego podobne do rozłożone dziennika. Klucz przeskalować w koncentratory zdarzenie jest [podzielone na partycje dla klientów indywidualnych modelu](event-hubs-overview.md#partition-key). Każdy partition jest niezależnych części danych i używana niezależnie. W czasie tych danych wieku, oparte na okres przechowywania można konfigurować. W wyniku koncentratora danego wydarzenia nigdy nie otrzymuje "zapełniony."

Archiwum koncentratory zdarzenia umożliwia określenie własnego konta magazyn obiektów Blob platformy Azure i kontenera, która będzie używana do przechowywania zarchiwizowanych danych. Tego konta może być w tym samym regionie jako Twoim Centrum zdarzeń lub w innym regionie, dodając do elastyczność to funkcja archiwum koncentratory zdarzenia.

Zarchiwizowane dane są zapisywane w formacie [Apache Avro][] ; format niewielkie, szybkie binarny zawierający struktury danych sformatowanego wbudowanego schematu. Ten format jest powszechnie używany w ekosystemie Hadoop, a także analizy strumieniu i Azure danych Factory. Więcej informacji na temat pracy z Avro jest dostępna w dalszej części tego artykułu.

### <a name="archive-windowing"></a>Obsługa archiwum okien

Archiwum koncentratory zdarzenia umożliwia konfigurowanie okna do sterowania archiwizacji. To okno jest minimalny rozmiar i Konfiguracja czasu za pomocą "pierwszy wins zasad," co oznacza, że pierwszego wyzwalacza napotkał spowoduje operacja archiwizacji. Jeśli masz okno archiwum 15 minutowe-100 MB i wysłać 1 MB/s, Zmień rozmiar okna wywoła przed z przedziału czasu. Każdy partition archiwa niezależnie i zapisuje blob złożonym bloku w momencie archiwum, o nazwie raz, kiedy wystąpił interwał archiwum. Konwencję nazewnictwa jest następująca:

```
<Namespace>/<EventHub>/<Partition>/<YYYY>/<MM>/<DD>/<HH>/<mm>/<ss>
```

### <a name="scaling-to-throughput-units"></a>Skalowanie jednostki przepustowość

Ruch koncentratory zdarzenie steruje [przepustowość jednostki](event-hubs-overview.md#capacity-and-security). Jednostka pojedynczy przepustowość umożliwia 1 MB/s lub 1000 wydarzeń na sekundę ingress i dwa razy ilość wyjściowe. Standardowy koncentratory zdarzenia można skonfigurować jednostki przepustowość 1 – 20, a więcej można kupić Zwiększ przydziałów [żądanie pomocy technicznej][]. Zastosowania poza jednostki kupowane przepustowość jest ograniczenie. Archiwum koncentratory zdarzenia kopiuje dane bezpośrednio z pamięci wewnętrznej koncentratory zdarzenia pominięcie przepustowość jednostki wyjściowe przydziałów i zapisywania do wyjściowego dla innych czytników przetwarzania, takich jak analizy strumieniu lub Spark.

Skonfigurowane archiwum koncentratory zdarzenie jest uruchamiany automatycznie po zainicjowaniu wysyłanie pierwszego wydarzenia. Będzie on nadal uruchomiony przez cały czas. Aby ułatwić usługi podrzędne przetwarzania wiedzieć, że proces działa, koncentratory zdarzenia zapisuje pustych plików, gdy nie ma żadnych danych. Dzięki temu cadence przewidywalne i znacznik, który można kanału procesorów partię.

## <a name="setting-up-event-hubs-archive"></a>Aby skonfigurować archiwum koncentratory zdarzenia

Archiwum koncentratory zdarzenia można skonfigurować w czasie tworzenia Centrum zdarzeń za pośrednictwem portalu lub Azure Menedżera zasobów. Po prostu włącz archiwum, klikając przycisk **Włącz** . Klikając sekcji **kontenera** karta skonfigurować konto miejsca do magazynowania i kontener. Ponieważ archiwum koncentratory zdarzenia używane uwierzytelnianie do usługi przechowywania, nie musisz określić parametry połączenia miejsca do magazynowania. Selektor zasobów automatycznie zaznacza identyfikator URI zasobu dla Twojego konta miejsca do magazynowania. Użycie Menedżera zasobów Azure, musisz podać ten identyfikator URI jawnie jako ciąg.

Okno czasu domyślny jest pięć minut. Minimalna wartość wynosi 1, maksymalna 15. **Zmień rozmiar** okna zawiera zakres 10 500 MB.

![][1]

## <a name="adding-archive-to-an-existing-event-hub"></a>Dodawanie archiwum do istniejącego Centrum zdarzenia

Archiwa można skonfigurować dla istniejącego koncentratory zdarzenia, które nazw koncentratory zdarzenia. Funkcja nie jest dostępna w starszych nazw typu wiadomości lub mieszane. Włącz archiwum na istniejące Centrum zdarzenie lub zmienić ustawienia archiwizowania, kliknij pozycję obszaru nazw ładowanie karta **Essentials** , a następnie kliknij koncentratora zdarzenia, dla którego chcesz włączyć lub zmień ustawienie archiwum. Na koniec kliknij w sekcji **Właściwości** karta otwarte, jak pokazano na poniższym rysunku.

![][2]

Archiwum koncentratory zdarzenia można również skonfigurować przy użyciu szablonów Azure Menedżera zasobów. Aby uzyskać więcej informacji, zobacz [Ten artykuł](event-hubs-resource-manager-namespace-event-hub-enable-archive.md).

## <a name="exploring-the-archive-and-working-with-avro"></a>Poznawanie archiwum i pracy z Avro

Skonfigurowane zdarzenia koncentratory archiwum tworzy pliki w konto Azure miejsca do magazynowania i opisane w oknie skonfigurowanym czasie. Te pliki można wyświetlać w dowolnym narzędziu, takich jak [Eksplorator magazynu Azure][]. Możesz pobrać pliki lokalnie w celu pracy z nimi.

Pliki tworzone przez zdarzenie koncentratory archiwum są następujące schematu Avro:

![][3]

Łatwe Eksplorowanie pliki Avro jest za pomocą słoik [Narzędzia Avro][] z Apache. Po pobraniu tego słoik schemat określonego pliku Avro można wyświetlić, uruchamiając następujące polecenie:

```
java -jar avro-tools-1.8.1.jar getschema \<name of archive file\>
```

To polecenie zwraca

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Za pomocą narzędzia Avro przekonwertować go na JSON format i wykonywać inne przetwarzania.

Aby wykonać przetwarzanie bardziej zaawansowane, Pobierz i zainstaluj Avro dla wybranej funkcji platformy. Podczas pisania tego dokumentu, są dostępne dla C, C++, C implementacji\#, Java, NodeJS Perl, PHP, Python i dopiskiem.

Apache Avro ma pełną przewodniki wprowadzenie do [języka Java][] i [Python][]. Można także przeczytaj artykuł [Wprowadzenie do archiwum koncentratory zdarzenia](event-hubs-archive-python.md) .

## <a name="how-event-hubs-archive-is-charged"></a>Jak jest naliczany archiwum koncentratory zdarzenia

Archiwum koncentratory zdarzenie jest taryfowe podobnie jednostki przepustowość jako dodatkowy co godzinę. Opłaty są bezpośrednio proporcjonalne do liczby jednostek przepustowość kupionej obszaru nazw. Jednostki przepustowość są zwiększane i zmniejszyła się, zdarzenia koncentratory archiwum powoduje zwiększenie i zmniejszenie o podanie pasujące wydajności. Metr wystąpić w połączeniu. Opłaty za archiwum koncentratory zdarzenie jest 0,10 $ na godzinę na jednostkę przepustowość oferowane z rabatem 50% w okresie Podgląd.

Archiwum koncentratory zdarzenia naprawdę jest najprostszym sposobem na pobieranie danych do Azure. Przy użyciu Lake danych Azure, Factory danych Azure i Azure HDInsight, można wykonać przetwarzanie wsadowe i innych analizy wybrane za pomocą znanych narzędzi i platformach w skali, wszelkie potrzebne.

## <a name="next-steps"></a>Następne kroki

Możesz więcej informacji o koncentratory zdarzenia, odwiedź witrynę z poniższych łączy:

- Ukończono [przykładowej aplikacji, która korzysta z koncentratorów zdarzenia][].
- Przykładowy [skali się zdarzenia przetwarzania z koncentratorów zdarzenia][] .
- [Omówienie koncentratory zdarzenia][]

[Apache Avro]: http://avro.apache.org/
[żądanie pomocy technicznej]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[1]: ./media/event-hubs-archive-overview/event-hubs-archive1.png
[2]: media/event-hubs-archive-overview/event-hubs-archive2.png
[Eksplorator magazynu platformy Azure]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-archive-overview/event-hubs-archive3.png
[Narzędzia Avro]: http://www-us.apache.org/dist/avro/avro-1.8.1/java/avro-tools-1.8.1.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Omówienie koncentratory zdarzenia]: event-hubs-overview.md
[Przykładowa aplikacja korzystającego z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Możliwość skalowania zdarzenia przetwarzania z koncentratorów zdarzenia]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3