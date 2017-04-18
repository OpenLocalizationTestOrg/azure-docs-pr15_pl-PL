<properties
    pageTitle="Dodatek Microsoft Translator aplikacji logika | Microsoft Azure"
    description="Omówienie łącznik program Microsoft Translator z parametrami interfejsu API usługi REST"
    services=""
    suite=""
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

# <a name="get-started-with-the-microsoft-translator-connector"></a>Wprowadzenie do programu Microsoft Translator łącznika
Połącz ze program Microsoft Translator tłumaczenie tekstu, wykrywa język i nie tylko. Program Microsoft Translator można: 

- Tworzenie usługi przepływu biznesowych na podstawie danych, które otrzymujesz z Microsoft Translator. 
- Tłumaczenie tekstu, wykrywa język i nie tylko za pomocą akcji. Te akcje odpowiedzi, a następnie wprowadź dane wyjściowe dostępne dla innych akcji. Na przykład po utworzeniu nowego pliku w usłudze Dropbox można przetłumaczyć tekst w polu plik do innego języka Microsoft Translator.

Aby dodać operację w aplikacjach logiczny, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje
Program Microsoft Translator obejmuje następujące czynności. Istnieją wyzwalacze nie.

Wyzwalaczy | Akcje
--- | ---
Brak | <ul><li>Wykrywanie języka</li><li>Tekst na mowę</li><li>Przetłumacz tekst</li><li>Pobieranie języków</li><li>Pobieranie języków mowy</li></ul>

Wszystkie łączniki obsługuje danych w formatach XML i JSON.


## <a name="create-a-connection-to-microsoft-translator"></a>Tworzenie połączenia z Microsoft Translator

>[AZURE.INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]


## <a name="swagger-rest-api-reference"></a>Swagger odwołanie interfejsu API usługi REST
Dotyczy wersji: 1.0.

### <a name="detect-language"></a>Wykrywanie języka    
Wykrywa język źródłowy podane tekstu.  
```GET: /Detect```

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|kwerendy|ciąg|tak|kwerendy|Brak |Tekst, którego język są oznaczane|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="text-to-speech"></a>Tekst na mowę    
Konwertuje tekst danego mowy jako strumień audio w formacie wave.  
```GET: /Speak```

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|kwerendy|ciąg|tak|kwerendy|Brak |Tekst do przekonwertowania|
|język|ciąg|tak|kwerendy|Brak |Kod języka, aby wygenerować mowy (przykład: "en-us)|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="translate-text"></a>Przetłumacz tekst    
Przekształca tekst do określonego języka za pomocą program Microsoft Translator.  
```GET: /Translate```

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|kwerendy|ciąg|tak|kwerendy|Brak |Tekst do przetłumaczenia|
|languageTo|ciąg|tak|kwerendy| Brak|Kod języka docelowej (przykład: "fr")|
|languageFrom|ciąg|Brak|kwerendy|Brak |Język źródłowy; Jeśli nie zostanie podany, program Microsoft Translator spróbuje Autowykrywanie. (przykład: pl)|
|Kategoria|ciąg|Brak|kwerendy|Ogólne |Kategoria tłumaczenia (domyślny: "Ogólne")|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="get-languages"></a>Pobieranie języków    
Pobiera wszystkich języków obsługiwanych przez program Microsoft Translator.  
```GET: /TranslatableLanguages```

Nie ma żadnych parametrów dla tego połączenia. 

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|


### <a name="get-speech-languages"></a>Pobieranie języków mowy    
Pobiera językach synteza mowy.  
```GET: /SpeakLanguages``` 

Nie ma żadnych parametrów dla tego połączenia.

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|domyślne|Operacja nie powiodła się.|

## <a name="object-definitions"></a>Definicje obiektów

#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>Język: model języka dla języków można przetłumaczyć program Microsoft Translator

|Nazwa właściwości | Typ danych | Wymagane|
|---|---|---|
|Kod|ciąg|Brak|
|Nazwa|ciąg|Brak|


## <a name="next-steps"></a>Następne kroki

[Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

Wróć do [listy interfejsów API](apis-list.md).


<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png
