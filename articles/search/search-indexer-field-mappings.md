<properties
pageTitle="Mapowania pól w indeksatory Azure wyszukiwania"
description="Konfigurowanie mapowania pól indeksowanie wyszukiwania Azure pod uwagę różnice w nazwy pól i reprezentacji danych"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/17/2016"
ms.author="eugenesh" />

# <a name="field-mappings-in-azure-search-indexers"></a>Mapowania pól w indeksatory Azure wyszukiwania

Podczas korzystania z wyszukiwania Azure indeksatory od czasu do czasu można się znaleźć w sytuacjach, gdy wprowadzania danych w przeciwnym nie pasuje do schematu indeksu docelowej. W takich przypadkach **mapowania pól** umożliwia przekształcanie danych na odpowiedni kształt. 

Niektórych sytuacjach, gdy mapowania pól są przydatne:
 
- Źródła danych zawiera pole `_id`, Azure wyszukiwania nie mogli nazw pól, zaczynając od znaku podkreślenia. Mapowanie pól umożliwia "zmienić" nazwę pola. 
- Chcesz wypełnić kilka pól w indeksie przy użyciu tych samych danych źródła danych, na przykład, ponieważ chcesz zastosować różne programy do analizowania do tych pól. Mapowania pól umożliwiają "talerza" pole źródła danych.
- Możesz potrzebny do formatu Base64 kodowanie lub dekodowanie danych. Mapowania pól obsługuje kilka **funkcji mapowania**, łącznie z funkcjami Base64 kodowanie i dekodowanie.   


> [AZURE.IMPORTANT] Obecnie funkcja mapowania pól jest w podglądzie. Jest dostępny tylko w przypadku korzystania z wersji **2015-02-28-Podgląd**interfejsu API usługi REST. Należy pamiętać, Wyświetl podgląd interfejsy API są przeznaczone do testowania i oceny, a nie może być używana w środowisku produkcyjnym.

## <a name="setting-up-field-mappings"></a>Aby skonfigurować mapowania pól

Podczas tworzenia nowego indeksatora, przy użyciu [Tworzenie indeksatora](search-api-indexers-2015-02-28-preview.md#create-indexer) API można dodać mapowania pól. Możesz zarządzać mapowania pól na indeksatora indeksowania za pomocą [Aktualizacji indeksatora](search-api-indexers-2015-02-28-preview.md#update-indexer) API. 

Mapowanie pól składa się z części 3: 

1. A `sourceFieldName`, które odpowiadają polem w źródle danych. Ta właściwość jest wymagane. 

2. Opcjonalny `targetFieldName`, która reprezentuje pole w indeksie wyszukiwania. Pominięcie taką samą nazwę jak źródło danych jest używany. 

3. Opcjonalny `mappingFunction`, który można przekształcać dane przy użyciu jednego z kilku wstępnie zdefiniowanych funkcji. Pełna lista funkcji jest [poniżej](#mappingFunctions).

Mapowanie pól zostaną dodane do `fieldMappings` tablica w definicji indeksowania. 

Na przykład Oto jak można uwzględnić różnice w nazwach pól: 

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 
} 
```

Indeksowanie może zawierać wiele mapowania pól. Na przykład, w tym jak użytkownik może "talerza" pola:

```JSON

"fieldMappings" : [ 
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" }, 
] 
```

> [AZURE.NOTE] Azure wyszukiwania używa porównania bez uwzględniania wielkości liter do rozpoznawania nazw pól i funkcji w mapowania pól. Jest to wygodny (nie musisz uzyskać wszystkich obudowy prawym), ale oznacza to, że źródła danych lub indeks nie może mieć pola, które różnią się tylko w przypadku.  

<a name="mappingFunctions"></a>
## <a name="field-mapping-functions"></a>Funkcje mapowania pola

Te funkcje są obecnie obsługiwane: 

- [base64Encode](#base64EncodeFunction)
- [base64Decode](#base64DecodeFunction)
- [extractTokenAtPosition](#extractTokenAtPositionFunction)
- [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>
### <a name="base64encode"></a>base64Encode 

Wykonuje Base64 *bezpiecznych adresu URL* kodowanie ciągu wejściowego. Przyjęto założenie, że dane wejściowe są kodowane UTF-8. 

#### <a name="sample-use-case"></a>Przykładowy diagram przypadków użycia 

Tylko bezpieczne adresu URL znaki mogą być wyświetlane klucz dokumentu Azure wyszukiwania (ponieważ jest klienci muszą mieć możliwość dokumentu przy użyciu interfejsu API wyszukiwania, na przykład adres). Jeśli dane zawierają adres URL niebezpiecznych znaków i chcesz go używać, aby wypełnić pole klucza w indeksie wyszukiwania, należy użyć tej funkcji.   

#### <a name="example"></a>Przykład 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Path", 
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" } 
  }] 
```

<a name="base64DecodeFunction"></a>
### <a name="base64decode"></a>base64Decode

Wykonuje Base64 dekodowanie parametrów wejściowych. Dane wejściowe zakłada się, że *adres URL bezpiecznych* ciąg zakodowany Base64. 

#### <a name="sample-use-case"></a>Przykładowy diagram przypadków użycia 

Wartości niestandardowe metadane BLOB musi być zakodowany ASCII. Za pomocą kodowania Base64 reprezentować dowolnego ciągi Unicode w metadanych niestandardowych obiektów blob. Jednak aby wyszukiwania zrozumiałej, umożliwia ta funkcja przekształcić dane kodowane ponownie ciągów "zwykła" podczas wypełniania indeksu wyszukiwania.  

#### <a name="example"></a>Przykład 

```JSON

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "Base64EncodedMetadata", 
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" } 
  }] 
```

<a name="extractTokenAtPositionFunction"></a>
### <a name="extracttokenatposition"></a>extractTokenAtPosition

Dzieli polem ciągu za pomocą określonego ogranicznika i wybiera tokenu w określonej pozycji w wyniku podziału.

Na przykład w przypadku wprowadzenia `Jane Doe`, `delimiter` jest `" "`(spacja) oraz `position` jest równy 0, wynik jest `Jane`; Jeśli `position` ma wartość 1, wynikiem funkcji jest `Doe`. Jeśli położenie odwołuje się do tokenu, który nie istnieje, zostanie zwrócony błąd.

#### <a name="sample-use-case"></a>Przykładowy diagram przypadków użycia 

Źródło danych zawiera `PersonName` pola i chcesz indeksować go jako dwie osobne `FirstName` i `LastName` pola. Ta funkcja umożliwia dzielenie danych wejściowych przy użyciu znaku spacji jako ogranicznik.

#### <a name="parameters"></a>Parametry

- `delimiter`: ciąg ma zostać użyte jako separator podczas dzielenia parametrów wejściowych.
- `position`: od zera pozycji całkowitą tokenu wybierz po podzieleniu parametrów wejściowych.    

#### <a name="example"></a>Przykład

```JSON 

"fieldMappings" : [ 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } } 
  }, 
  { 
    "sourceFieldName" : "PersonName", 
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } } 
  }] 
```

<a name="jsonArrayToStringCollectionFunction"></a>
### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection

Przekształca ciąg sformatowany jako tablicę JSON ciągów do tablicy ciąg, który może być używany do wypełnienia `Collection(Edm.String)` pola w indeksie. 

Na przykład ciąg wejściowy jest `["red", "white", "blue"]`, następnie pola docelowego typu `Collection(Edm.String)` zostanie wypełniona trzy wartości `red`, `white` i `blue`. Dla wartości wejściowych, których nie można przeanalizować jako tablice ciągu JSON zostanie zwrócony błąd. 

#### <a name="sample-use-case"></a>Przykładowy diagram przypadków użycia

Baza danych SQL Azure nie ma typ danych wbudowane sposób naturalny mapy do `Collection(Edm.String)` pola wyszukiwania Azure. Wypełnij pola zbioru ciągów, formatować dane źródłowe jako tablicę ciągu JSON i działania tej funkcji. 

#### <a name="example"></a>Przykład 

```JSON

"fieldMappings" : [ 
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
] 
```

## <a name="help-us-make-azure-search-better"></a>Pomóż nam udoskonalić Azure wyszukiwania

Jeśli używasz funkcji lub pomysły dotyczące ich udoskonalenia, sprawdź docieranie z nami w naszej [witryny możliwości](https://feedback.azure.com/forums/263029-azure-search/).