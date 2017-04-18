## <a name="specifying-formats"></a>Określanie formatów

Azure Factory danych obsługuje następujące typy formatów: 

- [Formatowanie tekstu](#specifying-textformat)
- [JSON Format](#specifying-jsonformat)
- [Avro Format](#specifying-avroformat)
- [ORC Format](#specifying-orcformat)
- [Parquet Format](#specifying-parquetformat)

### <a name="specifying-textformat"></a>Określanie TextFormat

Jeśli odpowiedni format **TextFormat**można określić następujące **opcjonalne** właściwości w sekcji **Format** .

| Właściwość | Opis | Dopuszczalne wartości | Wymagane |
| -------- | ----------- | -------- | -------- | 
| columnDelimiter | Znak separatora kolumny w pliku. | Może znajdować się tylko jeden znak. Wartością domyślną jest przecinek (','). <br/><br/>Aby użyć znaku Unicode, skorzystaj z [Znaków Unicode](https://en.wikipedia.org/wiki/List_of_Unicode_characters) , aby uzyskać odpowiedni kod. Na przykład można rozważyć rzadkich niedrukowalne znak, który prawdopodobnie nie istnieje w danych za pomocą: Określ "\u0001" co stanowi rozpocząć z pozycji (raport). | Brak |
| rowDelimiter | Znak używany do rozdzielania wierszy w pliku. | Może znajdować się tylko jeden znak. Wartość domyślna to dowolną z następujących wartości odczytu: ["\r\n", "\r", "\n"] i "\r\n" podczas zapisu. | Brak |
| escapeChar | Znak specjalny wykorzystywane w celu uniknięcia ogranicznika kolumny w zawartości pliku wejściowego. <br/><br/>Nie można określić escapeChar i quoteChar dla tabeli. | Może znajdować się tylko jeden znak. Brak wartości domyślnej. <br/><br/>Przykład: Jeśli masz przecinek (",") jako ogranicznika kolumny, ale mają znak przecinka w tekście (przykład: "Witaj, świecie"), można zdefiniować "$" jako znaku anulowania i użyć ciągu "Witaj$, świata" ze źródła. | Brak | 
| quoteChar | Znak używany do oferty wartość ciągu. Ograniczniki kolumn i wierszy wewnątrz znakami cudzysłowu będzie traktowane jako część wartość ciągu. Ta właściwość ma zastosowanie do wejścia i wyjścia zestawy danych.<br/><br/>Nie można określić escapeChar i quoteChar dla tabeli. | Może znajdować się tylko jeden znak. Brak wartości domyślnej. <br/><br/>Na przykład jeśli masz przecinek (",") jako ogranicznika kolumny, ale mają znak przecinka w tekście (przykład: < Witaj, świecie >), można zdefiniować "(podwójny cudzysłów) jako znaku cudzysłowu i używanie ciąg"Witaj, świecie"w źródle. | Brak |
| nullValue | Jeden lub więcej znaków reprezentuje wartość null. | Jeden lub więcej znaków. Domyślne wartości są "\N" oraz "Zero" na odczyt i "\N" podczas zapisu. | Brak |
| encodingName | Określ nazwę kodowania. | Nieprawidłowa nazwa kodowania. zobacz [Właściwość Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx). Przykład: 1 250 systemu windows lub shift_jis. Wartość domyślna to UTF-8. | Brak | 
| firstRowAsHeader | Określa, czy brać pod uwagę pierwszy wiersz jako nagłówek. Dla zestawu danych wejściowych Factory danych odczytuje pierwszy wiersz jako nagłówek. W przypadku zestaw danych wyjściowych Factory danych zapisuje pierwszy wiersz jako nagłówek. <br/><br/>Zobacz [scenariusze korzystania z **firstRowAsHeader** i **skipLineCount** ](#scenarios-for-using-firstrowasheader-and-skiplinecount) dla przykładowe scenariusze. | PRAWDA<br/>Wartość FAŁSZ (ustawienie domyślne) | Brak |
| skipLineCount | Określa liczbę wierszy, aby pominąć podczas odczytu danych z plików wejściowych. Jeśli określono zarówno skipLineCount, jak i firstRowAsHeader wiersze są pomijane najpierw i odczytaj informacje o nagłówkach z pliku wejściowego. <br/><br/>Zobacz [scenariusze korzystania z firstRowAsHeader i skipLineCount](#scenarios-for-using-firstrowasheader-and-skiplinecount) dla przykładowe scenariusze. | Liczba całkowita | Brak | 
| treatEmptyAsNull | Określa, czy traktowania wartości null lub pusty ciąg jako wartość null podczas odczytu danych z pliku wejściowego. | Wartość PRAWDA (ustawienie domyślne)<br/>FAŁSZ | Brak |  

#### <a name="textformat-example"></a>Przykład TextFormat
Poniżej pokazano niektóre z właściwości format dla TextFormat.

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

Aby użyć escapeChar zamiast quoteChar, Zastąp linię quoteChar z następujących escapeChar:

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Scenariusze korzystania z firstRowAsHeader i skipLineCount

- Jest kopiowany źródłem — do pliku tekstowego i chcesz dodać wiersz nagłówka zawierającą metadane schematu (na przykład: SQL schematu). Określ **firstRowAsHeader** jako wartość PRAWDA w zestawie danych wyjściowych danych dla tego scenariusza. 
- Jest kopiowany plik tekstowy zawierający wiersz nagłówka do zainstalowania stołu innych niż plik i chcesz upuść tego wiersza. Określ **firstRowAsHeader** jako wartość PRAWDA w zestawie danych wejściowych.
- Są kopiowane z pliku tekstowego i chcesz pominąć kilka wierszy na początku, które nie zawierają żadnych informacji danych lub nagłówka. Określ **skipLineCount** do wskazywania liczby wierszy, które mają zostać pominięte. Jeśli reszta pliku zawiera wiersz nagłówka, można również określić **firstRowAsHeader**. Jeśli określono zarówno **skipLineCount** i **firstRowAsHeader** wiersze są pomijane najpierw i Odczytaj informacji o nagłówkach z pliku wejściowego

### <a name="specifying-avroformat"></a>Określanie AvroFormat
Format jest ustawiony na AvroFormat, nie musisz określić dowolne właściwości w sekcji Format w sekcji typeProperties. Przykład:

    "format":
    {
        "type": "AvroFormat",
    }

Aby użyć formatu Avro w tabeli gałęzi, może dotyczyć [gałęzi Apache samouczka](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Zwróć uwagę następujące punkty:  

- [Złożonymi typami danych](http://avro.apache.org/docs/current/spec.html#schema_complex) nie są obsługiwane (rekordów wyliczenia, tablice, mapy, związków i stałe)

### <a name="specifying-jsonformat"></a>Określanie JsonFormat

Jeśli odpowiedni format jest ustawiona na **JsonFormat**, można określić następujące **opcjonalne** właściwości w sekcji **Format** .

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- |
| filePattern | Wskazują, struktury danych przechowywanych w każdym pliku JSON. Dozwolone wartości są: **setOfObjects** i **arrayOfObjects**. Wartość **Domyślna** to: **setOfObjects**. Zobacz, wykonując sekcjach, aby uzyskać szczegółowe informacje o tych deseni.| Brak |
| encodingName | Określ nazwę kodowania. Aby uzyskać listę prawidłowych nazw kodowania, zobacz: właściwość [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) . Na przykład: 1 250 systemu windows lub shift_jis. Wartość **Domyślna** to: **UTF-8**. | Brak | 
| nestingSeparator | Znak, który jest używany do oddzielania poziomów zagnieżdżenia. Wartość domyślna to "." (kropka). | Brak | 


#### <a name="setofobjects-file-pattern"></a>Wzorzec plików setOfObjects

Każdy plik zawiera jeden obiekt lub linii rozdzielany łączone wiele obiektów. Jeśli ta opcja jest zaznaczona w zestawie danych dane wyjściowe, Kopiuj działalność prowadzi do jednego pliku JSON z każdego obiektu dla każdego wiersza (wiersz rozdzielany przecinkami).

**jeden obiekt** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**rozdzielany linii JSON** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**połączone JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern"></a>Wzorzec plików arrayOfObjects. 

Każdy plik zawiera tablicę obiektów. 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>Przykład JsonFormat

Jeśli masz plik JSON o następującej treści:  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

i chcesz skopiować go do tabeli programu Azure SQL w następującym formacie: 

Identyfikator  | Name.First | Name.Middle | Name.Last | Znaczniki
--- | ---------- | ----------- | --------- | ----
1 | Jan | wartości null | Kowalski | ["Danych Factory", "Azure"]

Zestaw wprowadzania danych typu JsonFormat jest definiowana następująco: (częściowa definicja z odpowiednich części)

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

Jeśli nie zdefiniowano struktury, działania kopii spłaszcza struktury domyślnie i skopiuj każdy element. 

#### <a name="supported-json-structure"></a>Obsługiwane struktury JSON
Zwróć uwagę następujące punkty: 

- Każdy obiekt ze zbiorem pary nazwa wartość są mapowane do jednego wiersza danych w formacie tabelarycznym. Obiekty mogą być zagnieżdżane i można określić, jak Spłaszcz struktury w zestawie danych z separatorem zagnieżdżania (.) domyślnie. Zobacz [przykład JsonFormat](#jsonformat-example) poprzedzającego sekcji, na przykład.  
- Jeśli struktura nie jest zdefiniowana w zestawie danych Factory danych, działanie kopii wykrywa schematu z pierwszego obiektu i Spłaszcz całego obiektu. 
- Jeśli JSON wejściowy tablicę, działania kopii konwertuje wartość całą macierz ciągu. Możesz pominąć go za pomocą [mapowania kolumn lub filtrowania](#column-mapping-with-translator-rules).
- W przypadku zduplikowanych nazw na tym samym poziomie, to działanie kopii wybiera ostatnią.
- Nazwy właściwości jest uwzględniana wielkość liter. Dwie właściwości o takiej samej nazwie, ale różnych osłonki są traktowane jako dwóch oddzielnych właściwości. 

### <a name="specifying-orcformat"></a>Określanie OrcFormat
Format jest ustawiony na OrcFormat, nie musisz określić dowolne właściwości w sekcji Format w sekcji typeProperties. Przykład:

    "format":
    {
        "type": "OrcFormat"
    }

> [AZURE.IMPORTANT] Jeśli nie są kopiowane pliki ORC **jako-jest** między wdrożeniem lokalnym a chmurą magazynów danych, należy zainstalować na komputerze bramy 8 JRE (środowisko Java Runtime). Brama 64-bitowej wymaga JRE 64-bitowej, a brama 32-bitowej JRE 32-bitowej. Można znaleźć obie wersje [tutaj](http://go.microsoft.com/fwlink/?LinkId=808605). Wybierz odpowiedni.

Zwróć uwagę następujące punkty:

-   Złożone dane, które typy nie są obsługiwane (struktury, mapy, listy, Unii)
-   Plik ORC zawiera trzy [Opcje związane z kompresji](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): Brak ZLIB, SNAPPY. Factory danych obsługuje odczytywania danych z pliku ORC w innych formatach skompresowany. Używa kompresji koder-dekoder znajduje się w metadanych Odczyt danych. Jednak podczas zapisywania pliku ORC, Factory danych wybiera ZLIB, która jest domyślną dla ORC. Obecnie jest opcja Brak, aby zastąpić to zachowanie. 

### <a name="specifying-parquetformat"></a>Określanie ParquetFormat
Format jest ustawiony na ParquetFormat, nie musisz określić dowolne właściwości w sekcji Format w sekcji typeProperties. Przykład:

    "format":
    {
        "type": "ParquetFormat"
    }

> [AZURE.IMPORTANT] Jeśli nie są kopiowane pliki Parquet **jako-jest** między wdrożeniem lokalnym a chmurą magazynów danych, należy zainstalować na komputerze bramy 8 JRE (środowisko Java Runtime). Brama 64-bitowej wymaga JRE 64-bitowej, a brama 32-bitowej JRE 32-bitowej. Można znaleźć obie wersje [tutaj](http://go.microsoft.com/fwlink/?LinkId=808605). Wybierz odpowiedni.

Zwróć uwagę następujące punkty:

-   Złożone dane, które typy nie są obsługiwane (tablica, listy)
-   Plik parquet dostępne są następujące opcje dotyczące kompresji: Brak, SNAPPY, GZIP i LZO. Factory danych obsługuje odczytywania danych z pliku ORC w innych formatach skompresowany. Koder-dekoder kompresji używa w metadanych Odczyt danych. Jednak podczas zapisywania pliku Parquet, Factory danych wybiera SNAPPY, która jest domyślnym formacie Parquet. Obecnie jest opcja Brak, aby zastąpić to zachowanie. 
