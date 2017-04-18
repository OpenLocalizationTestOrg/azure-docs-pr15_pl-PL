## <a name="fileshare-dataset-type-properties"></a>Właściwości typu zestawu danych lokalizacji

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawy danych zobacz artykuł [Tworzenie zestawów danych](../articles/data-factory/data-factory-create-datasets.md) . Sekcje, takich jak struktura, dostępność i zasad zestawu danych JSON są podobne dla wszystkich typów zestawu danych. 

W sekcji **typeProperties** różni się dla każdego typu zestawu danych. Zawiera informacje dotyczące typu zestawu danych. Sekcja typeProperties zestawu danych typu **lokalizacji** zestawu danych zawiera następujące właściwości:

Właściwość | Opis | Wymagane
-------- | ----------- | --------
ścieżkafolderu | Sub ścieżka do folderu. Używanie znaku anulowania "\" dla znaków specjalnych w ciągu. Przykłady, zobacz [Przykładowe połączone definicje usługi i zestawu danych](#sample-linked-service-and-dataset-definitions) .<br/><br/>Tej właściwości można połączyć z **partitionBy** mają ścieżki folderów według wycinek daty i godziny rozpoczęcia i zakończenia. | Tak
Nazwa pliku | Określ nazwę pliku w **ścieżkafolderu** , jeśli chcesz, aby tabeli, aby odwołać się do określonego pliku w folderze. Jeśli nie określisz wartości tej właściwości tabeli wskazuje wszystkie pliki w folderze.<br/><br/>Po dla zestaw danych wyjściowych nie określono nazwy pliku, nazwa wygenerowany plik jest w następującym formacie: <br/><br/>Dane. <Guid>txt (przykład: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Brak
partitionedBy | partitionedBy można określić dynamicznego ścieżkafolderu, nazwę czasu serii danych. Na przykład ścieżkafolderu sparametryzowane dla każdej godziny danych. | Brak
Formatowanie | Obsługiwane są następujące typy formatów: **TextFormat**, **AvroFormat**, **JsonFormat**i **OrcFormat**. Ustaw właściwości **Typ** w obszarze format jedną z następujących wartości. Zobacz [TextFormat określającą](#specifying-textformat), [Określając AvroFormat](#specifying-avroformat) [Określającą JsonFormat](#specifying-jsonformat)i [Określanie OrcFormat](#specifying-orcformat) sekcji, aby uzyskać szczegółowe informacje. Jeśli chcesz skopiować pliki jako-jest między opartych na plikach sklepy (binarne kopii) można pominąć sekcji format w obu definicjach wejściowe i wyjściowe zestawu danych. | Brak
obiektu fileFilter | Określ filtr może być używany do wybierania podzbioru plików w ścieżkafolderu zamiast wszystkie pliki.<br/><br/>Dozwolone wartości są: `*` (wielu znaków) i `?` (pojedynczy znak).<br/><br/>Przykłady 1:`"fileFilter": "*.log"`<br/>Przykład 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> obiektu fileFilter ma zastosowanie do wprowadzania danych lokalizacji. Ta właściwość nie jest obsługiwane z HDFS.  | Brak
| stopień kompresji | Określ typ i stopień kompresji dla danych. Obsługiwane typy to: **GZip**i **Deflate**i **BZip2** poziomy obsługiwane są: **optymalna** i **najszybciej**. Obecnie ustawienia kompresji dla danych w **AvroFormat** lub **OrcFormat**nie są obsługiwane. Aby uzyskać więcej informacji, zobacz sekcję [pomocy technicznej kompresji](#compression-support) .  | Brak |
| useBinaryTransfer | Określ, czy użyć trybu transfer binarny. Zwraca wartość true binarnym i ASCII FAŁSZ. Wartość domyślna: True. Tej właściwości można używać tylko w przypadku typu skojarzonej usługi połączone typu: SerwerFTP. | Brak | 
 

> [AZURE.NOTE] Nie można jednocześnie używać nazwy i obiektu fileFilter.

### <a name="using-partionedby-property"></a>Przy użyciu właściwości partionedBy

Jak wspomniano w poprzedniej sekcji, możesz określić ścieżkafolderu dynamiczne, nazwę czasu serii danych za pomocą partitionedBy. Aby to zrobić przy użyciu makra danych Factory i zmiennej systemowej SliceStart, SliceEnd, wskazujące logiczne okres dla danego wycinek. 

Aby dowiedzieć się, zestawy czasu serii danych, planowania i wycinki, zobacz [Tworzenie zestawów danych](../articles/data-factory/data-factory-create-datasets.md), [Planowanie i wykonanie](../articles/data-factory/data-factory-scheduling-and-execution.md)i artykuły [Procesy tworzenia](../articles/data-factory/data-factory-create-pipelines.md) . 

#### <a name="sample-1"></a>Przykład 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

W tym przykładzie {wycinek} jest zastępowany wartość zmiennej system Factory danych SliceStart w formacie (YYYYMMDDHH) określone. SliceStart odwołuje się do wycinek czas rozpoczęcia. Ścieżkafolderu różni się dla każdego wycinka. Przykład: wikidatagateway/wikisampledataout/2014100103 lub wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Przykład 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

W tym przykładzie rok, miesiąc, dzień i godzinę SliceStart są wyodrębniane do oddzielnych zmiennych, które są używane przez właściwości ścieżkafolderu i nazwę pliku.
