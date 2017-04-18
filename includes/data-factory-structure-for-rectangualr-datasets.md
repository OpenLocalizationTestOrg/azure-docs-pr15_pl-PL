## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Określanie struktury definicję prostokątnym zestawy danych
W sekcji Struktura zestawy danych JSON jest **Opcjonalna** sekcja prostokątnym tabel (wiersze i kolumny) i zawiera zestaw kolumn w tabeli. Za pomocą sekcji Struktura będzie dla któregokolwiek przekazywanie informacji o typie dla konwersje typów lub wykonywanie mapowania kolumn. W poniższych sekcjach opisano te funkcje szczegółowo. 

Każda kolumna zawiera następujące właściwości:

| Właściwość | Opis | Wymagane |
| -------- | ----------- | -------- |
| Nazwa | Nazwa kolumny. | Tak |
| Typ | Typ danych kolumny. Zobacz typ konwersji poniższej sekcji więcej szczegółów dotyczących kiedy należy określone informacje o typie | Brak |
| kultury | .NET podstawie kultury ma być używany podczas typu określono a jest .NET typu Data/Godzina lub Datetimeoffset. Wartością domyślną jest "en-us".  | Brak |
| Formatowanie | Ciąg formatu może być używany, gdy typu określono a jest .NET wpisz daty/godziny lub Datetimeoffset. | Brak |

Następujący przykład przedstawia sekcji Struktura JSON dla tabeli, która ma trzy kolumny Identyfikator użytkownika, nazwę i lastlogindate.

    "structure": 
    [
        { "name": "userid"},
        { "name": "name"},
        { "name": "lastlogindate"}
    ],

Użyj następujących wskazówek dotyczących umieszczania informacji "struktury" i elementów do uwzględnienia w sekcji **Struktura** .

- **Dla źródeł danych strukturalnych** przechowywanie danych schematu i wpisz informacje wraz z danymi (źródeł takich jak Azure tabeli programu SQL Server, Oracle, itd.), należy określić sekcji "struktury" tylko wtedy, gdy chcesz wykonać mapowania kolumn określone źródło kolumn do określonych kolumn w sink i ich nazwy nie są takie same (Zobacz szczegóły w sekcji mapowania kolumn poniżej). 

    Jak wcześniej wspomniano, informacje o typie jest opcjonalna w sekcji "struktury". Strukturalne źródeł, informacje o typie jest już dostępny jako część Definicja zestawu danych w magazynie danych, dlatego nie należy używać informacje o typie po dołączeniu sekcji "struktury".
- **Dla schematu na źródłach danych odczytu (specjalnie obiektów blob platformy Azure)** wybrane do przechowywania danych bez przechowywania schematu lub wpisz informacje z danych. Dla tych typów źródeł danych powinien zawierać "struktury" w następujących przypadkach 2:
    - Chcesz zrobić mapowania kolumn.
    - Po zestawu danych jest źródłem w działaniu Kopiuj, można udostępnić informacje o typie "strukturę" i factory danych użyje tych informacji typ konwersji do natywnej typy obiekt sink. Zobacz artykuł [Przenoszenie danych do i z obiektów Blob platformy Azure](../articles/data-factory/data-factory-azure-blob-connector.md) , aby uzyskać więcej informacji.

### <a name="supported-net-based-types"></a>Obsługiwane. Typy netto 
Factory danych obsługuje następujące CLS zgodności .NET podstawie typ wartości umożliwiające wpisz informacje w "Struktura" schematu na źródeł danych odczytu, takich jak obiektów blob platformy Azure.

- Int16
- Int32 
- Int64
- Pojedynczy
- Naciśnij dwukrotnie
- Dziesiętna
- Bajt]
- Wartość logiczna
- Ciąg 
- Identyfikator GUID
- Daty i godziny
- Datetimeoffset
- Przedziału czasu 

Dla daty i godziny i Datetimeoffset również opcjonalnie można określić ciąg "Kultura" i "format" ułatwiające analizy tekst niestandardowy daty i godziny. Zobacz próbki dla konwersji typów poniżej.

