<properties
    pageTitle="Dodawanie łącznika w polu do aplikacji logiki | Microsoft Azure"
    description="Omówienie łącznik pole z parametrami interfejsu API usługi REST"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-box-connector"></a>Wprowadzenie do łącznika w polu
Nawiązywanie połączenia z pola i tworzenie plików, Usuń pliki i inne. 

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych.

Pole należy wykonać następujące czynności:

- Tworzenie usługi przepływu biznesowych na podstawie danych, które otrzymujesz w polu od. 
- Jeśli plik został utworzony lub zaktualizowany z użyciem wyzwalaczy.
- Używanie akcji, które skopiowanie pliku, usuń plik i nie tylko. Te akcje odpowiedzi, a następnie wprowadź dane wyjściowe dostępne dla innych akcji. Na przykład po zmianie pliku w polu, możesz zabrać ten plik i poczty e-mail przy użyciu usługi Office 365.

Aby dodać operację w aplikacjach logiczny, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje
Pole zawiera następujące wyzwalacza i akcje.

| Wyzwalaczy | Akcje|
| --- | --- |
|<ul><li>Po utworzeniu pliku</li><li>Modyfikacji pliku</li></ul> | <ul><li>Tworzenie pliku</li><li>Po utworzeniu pliku</li><li>Skopiuj plik</li><li>Usuwanie pliku</li><li>Wyodrębnianie archiwum do folderu</li><li>Pobierz plik zawartości za pomocą identyfikatora</li><li>Pobieranie zawartości pliku przy użyciu ścieżki</li><li>Pobieranie metadanych pliku przy użyciu identyfikatora</li><li>Pobieranie metadanych pliku przy użyciu ścieżki</li><li>Aktualizowanie plików</li><li>Modyfikacji pliku</li></ul>

Wszystkie łączniki obsługuje danych w formatach XML i JSON.

## <a name="create-a-connection-to-box"></a>Tworzenie połączenia z pola
Po dodaniu tego łącznika do aplikacji logika musi zezwolić logiczny aplikacje, aby nawiązać połączenie pole listy.

>[AZURE.INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]

Po utworzeniu połączenia, możesz wprowadzić właściwości pola. **Odwołanie interfejsu API usługi REST** w tym temacie opisano następujące właściwości.

>[AZURE.TIP] Za pomocą tego samego połączenia pola w innych aplikacjach logiki.

## <a name="swagger-rest-api-reference"></a>Swagger odwołanie interfejsu API usługi REST
Dotyczy wersji: 1.0.

### <a name="create-file"></a>Tworzenie pliku
Przekazywanie pliku do pola.  
```POST: /datasets/default/files```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|ścieżkafolderu|ciąg|Tak|kwerendy|Brak |Ścieżka folderu, aby przekazać plik do pola|
|Nazwa|ciąg|Tak|kwerendy|Brak |Nazwa pliku, aby utworzyć w polu|
|Treść|String(Binary) |Tak|Treść|Brak |Zawartość pliku do przekazania do pola|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="when-a-file-is-created"></a>Po utworzeniu pliku
Uaktywnia przepływu, gdy jest tworzony nowy plik w folderze pola.  
```GET: /datasets/default/triggers/onnewfile```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator folderu|ciąg|Tak|kwerendy|Brak |Unikatowy identyfikator folderu w polu|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="copy-file"></a>Skopiuj plik
Skopiowanie pliku do pola.  
```POST: /datasets/default/copyFile```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|źródła|ciąg|Tak|kwerendy|Brak |Adres URL z plikiem źródłowym|
|miejsce docelowe|ciąg|Tak|kwerendy| Brak|Ścieżka pliku docelowego w polu Nazwa pliku docelowego w tym|
|Zastąp|wartość logiczna|Brak|kwerendy| Brak|Zastępuje pliku docelowego, jeśli ma wartość "true"|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="delete-file"></a>Usuwanie pliku
Usuwa plik z pola.  
```DELETE: /datasets/default/files/{id}```


| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|ciąg|Tak|Ścieżka|Brak |Unikatowy identyfikator pliku, aby usunąć z pola|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="extract-archive-to-folder"></a>Wyodrębnianie archiwum do folderu
Wyodrębnia plik archiwum do folderu w polu (przykład: zip).  
```POST: /datasets/default/extractFolderV2```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|źródła|ciąg|Tak|kwerendy| |Ścieżka do pliku archiwum|
|miejsce docelowe|ciąg|Tak|kwerendy| |Ścieżkę w polu, aby wyodrębnić zawartość archiwum|
|Zastąp|wartość logiczna|Brak|kwerendy| |Zastępuje plików docelowych, jeśli ma wartość "true"|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="get-file-content-using-id"></a>Pobierz plik zawartości za pomocą identyfikatora
Pobiera zawartość pliku z pola przy użyciu identyfikatora.  
```GET: /datasets/default/files/{id}/content```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|ciąg|Tak|Ścieżka|Brak |Unikatowy identyfikator pliku w polu|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="get-file-content-using-path"></a>Pobieranie zawartości pliku przy użyciu ścieżki
Pobiera zawartość pliku z pola przy użyciu ścieżki.  
```GET: /datasets/default/GetFileContentByPath```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Ścieżka|ciąg|Tak|kwerendy|Brak |Unikatowe ścieżkę do pliku w polu|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="get-file-metadata-using-id"></a>Pobieranie metadanych pliku przy użyciu identyfikatora
Pobiera metadanych pliku z pola przy użyciu identyfikatora pliku.  
```GET: /datasets/default/files/{id}```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|ciąg|Tak|Ścieżka| Brak|Unikatowy identyfikator pliku w polu|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="get-file-metadata-using-path"></a>Pobieranie metadanych pliku przy użyciu ścieżki
Pobiera metadanych pliku z pola przy użyciu ścieżki.  
```GET: /datasets/default/GetFileByPath```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Ścieżka|ciąg|Tak|kwerendy|Brak |Unikatowe ścieżkę do pliku w polu|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="update-file"></a>Aktualizowanie plików
Aktualizuje plik w polu.  
```PUT: /datasets/default/files/{id}```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator|ciąg|Tak|Ścieżka| Brak|Unikatowy identyfikator pliku aktualizacji w polu|
|Treść|String(Binary) |Tak|Treść|Brak |Zawartość pliku aktualizacji w polu|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="when-a-file-is-modified"></a>Modyfikacji pliku
Uaktywnia przepływu modyfikacji pliku w folderze pola.  
```GET: /datasets/default/triggers/onupdatedfile```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator folderu|ciąg|Tak|kwerendy|Brak |Unikatowy identyfikator folderu w polu|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


## <a name="object-definitions"></a>Definicje obiektów

#### <a name="datasetsmetadata"></a>DataSetsMetadata

|Nazwa właściwości | Typ danych | Wymagane|
|---|---|---|
|tabelaryczny|nie zdefiniowano|Brak|
|obiektów blob|nie zdefiniowano|Brak|

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|źródła|ciąg|Brak|
|displayName|ciąg|Brak|
|urlEncoding|ciąg|Brak|
|tableDisplayName|ciąg|Brak|
|tablePluralName|ciąg|Brak|

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|źródła|ciąg|Brak|
|displayName|ciąg|Brak|
|urlEncoding|ciąg|Brak|

#### <a name="blobmetadata"></a>BlobMetadata

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Identyfikator|ciąg|Brak|
|Nazwa|ciąg|Brak|
|DisplayName|ciąg|Brak|
|Ścieżka|ciąg|Brak|
|Ostatnia modyfikacja|ciąg|Brak|
|Rozmiar|Liczba całkowita|Brak|
|Typ nośnika|ciąg|Brak|
|IsFolder|wartość logiczna|Brak|
|ETag|ciąg|Brak|
|FileLocator|ciąg|Brak|

## <a name="next-steps"></a>Następne kroki

[Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).
