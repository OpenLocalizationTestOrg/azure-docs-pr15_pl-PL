<properties
    pageTitle="Maszynowego uczenia interfejsów API: Analiza tekstu | Microsoft Azure"
    description="Interfejsy API firmy Microsoft maszynowego uczenia tekstu analizy może służyć do analizowania niestrukturalne tekst upodobania analizy, wyodrębnianie klucza frazę, wykrywanie języka i wykrywania tematu."
    services="machine-learning"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>


# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>Interfejsy API nauki komputera: Tekst analizy upodobania, klucza wyodrębniania frazę, wykrywanie języka i wykrywania tematu

>[AZURE.NOTE] Ten przewodnik jest dla wersji 1 interfejsu API. W przypadku wersji 2, [**odwołują się do tego dokumentu**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). W wersji 2 jest teraz preferowaną wersję tego interfejsu API.

## <a name="overview"></a>Omówienie

Interfejs API analizy tekst jest pakietu analizy tekstu [usług sieci web](https://datamarket.azure.com/dataset/amla/text-analytics) utworzone przy użyciu Azure maszynowego uczenia. Interfejs API może służyć do analizowania niestrukturalne tekstu dla zadań, takich jak analizy upodobania i wyodrębnianie klucza frazę, wykrywanie języka wykrywania tematu. Żadne dane szkolenia jest potrzebny do korzystania z tego interfejsu API: po prostu przesuń danych tekst. Ten interfejs API używa zaawansowane języku naturalnym przetwarzania technik do przeprowadzania najważniejsze w przewidywań zajęć.

Można wyświetlić analizy tekstu w działaniu w naszej [witryny pokaz](https://text-analytics-demo.azurewebsites.net/), gdzie również znajduje się [próbki](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) dotyczące implementowania analizy tekstu w C# i Python.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

---

## <a name="sentiment-analysis"></a>Analiza upodobania

Interfejs API zwraca wyniku liczbowego między 0 i 1. Wyniki zbliżony 1 wskazują dodatnia upodobania podczas wyniki zbliżony 0 wskazuje upodobania ujemne. Wynik upodobania, jest generowany przy użyciu techniki klasyfikacji. Do wprowadzania funkcji klasyfikatora należą n g, funkcje wynikiem część mowy znaczniki i osadzone w programie word. Obecnie angielski jest tylko obsługiwany język.
 
## <a name="key-phrase-extraction"></a>Wyodrębnianie klucza frazę

Interfejs API zwraca listę ciągów oznaczający najważniejszych rozmów w oknie wprowadzania tekstu. Firma Microsoft zastosować technik z zaawansowanych narzędzi przetwarzania w języku naturalnym Microsoft Office. Obecnie angielski jest tylko obsługiwany język.

## <a name="language-detection"></a>Wykrywanie języka

Interfejs API zwraca wykryto języka i wyniku liczbowego między 0 i 1. Wyniki zbliżony 1 wskazują pewności 100% dotyczy określonego języka. Liczba 120 języki są obsługiwane.

## <a name="topic-detection"></a>Wykrywanie tematu

To jest interfejs API nowo opublikowanej, która oblicza góry wykryte tematów, aby uzyskać listę przesłane rekordy tekstu. Temat jest identyfikowany klucza frazą, który może mieć jedną lub więcej zawartości związanej z wyrazów. Ten interfejs API wymaga co najmniej 100 rekordów tekstu zostaną przekazane, ale jest przeznaczona do wykrywania tematy przez setki do tysięcy rekordów. Zauważ, że ten interfejs API 1 transakcji na tekst rekord przesłane opłat. Interfejs API jest przeznaczona do pracy dla ludzi, krótka napisany tekst, takich jak recenzji i opinii użytkowników.

---

## <a name="api-definition"></a>Definicja interfejsu API

### <a name="headers"></a>Nagłówki

Upewnij się, uwzględnić poprawne nagłówków w wezwaniu powinna wyglądać następująco:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Możesz znaleźć swój klucz konta z konta usługi [Azure Data Market](https://datamarket.azure.com/account/keys). Należy zauważyć, że obecnie JSON zostanie odebrane formatów wejściowe i wyjściowe. XML nie jest obsługiwane.

---

## <a name="single-response-apis"></a>Interfejsy API pojedynczej odpowiedzi

### <a name="getsentiment"></a>GetSentiment

**ADRES URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Wezwanie na przykład**

W ramach połączenia poniżej prosimy analizy upodobania frazy "Witaj świecie":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

Spowoduje to przywrócenie odpowiedź w następujący sposób:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

### <a name="getkeyphrases"></a>GetKeyPhrases

**ADRES URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Wezwanie na przykład**

W ramach połączenia prosimy że klucza zwroty znaleziony w polu tekst "Było wspaniałe hotel, aby otrzymywać, z unikatowymi wnętrz i przyjazny personelu":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

Spowoduje to przywrócenie odpowiedź w następujący sposób:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }
 
---

### <a name="getlanguage"></a>GetLanguage

**ADRES URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Wezwanie na przykład**

W ramach połączenia GET poniżej prosimy dla upodobania klucza zwrotów w tekście *Witaj świecie*

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

Spowoduje to przywrócenie odpowiedź w następujący sposób:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Parametry opcjonalne**

`NumberOfLanguagesToDetect`jest opcjonalny parametr. Wartością domyślną jest 1.

---

## <a name="batch-apis"></a>Interfejsy API partii

Usługa analizy tekstu umożliwia upodobania i ekstrakcji klucza frazę w trybie partię. Uwaga każdego rekordu punktowane liczone jako jednej transakcji. Na przykład jeśli żądanie upodobania 1000 rekordów w jednym połączeniu transakcje 1000 będą odejmowane.

Zauważ, że identyfikatory wprowadzane do systemu są identyfikatory zwrócone przez system. Usługa sieci web nie sprawdza, czy te identyfikatory są unikatowe. Jest odpowiedzialny za wywołującego Weryfikuj unikatowość. 


### <a name="getsentimentbatch"></a>GetSentimentBatch

**ADRES URL** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Wezwanie na przykład**

W ramach połączenia WPIS poniżej prosimy dla opinie oznaczeń "Witaj świecie", "Witaj Foo świata" i "Witaj Mój świat" w treści wezwania:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Treść żądania:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

W poniższych odpowiedzi zostanie wyświetlony na liście wyniki skojarzone z tekstu identyfikatorów:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


---

### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch

**ADRES URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Wezwanie na przykład**

W tym przykładzie prosimy listy opinie klucza zwrotów w dokumentach: 

* "Było wspaniałe hotel, aby otrzymywać, z unikatowymi wnętrz i przyjazny personelu"
* "Było atrakcyjnych konferencji kompilacji z bardzo interesujące rozmów"
* "Ruch został tak trudny, I wydatków trzy godziny, przechodząc na lotnisko"

Ten wniosek jako połączenie WPIS do punktu końcowego:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Treść żądania:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

W poniższych odpowiedzi zostanie wyświetlony na liście fraz klucza skojarzonego z tekstu identyfikatorów:

    { "odata.metadata":"<url>",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

---

### GetLanguageBatch

In the POST call below, we are requesting language detection for two text inputs:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Request body:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

This returns the following response, where English is detected in the first input and French in the second input:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "Angielski",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "Francuski",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

---

## <a name="topic-detection-apis"></a>Interfejsy API wykrywania tematu

To jest interfejs API nowo opublikowanej, która oblicza góry wykryte tematów, aby uzyskać listę przesłane rekordy tekstu. Temat jest identyfikowany klucza frazą, który może mieć jedną lub więcej zawartości związanej z wyrazów. Zauważ, że ten interfejs API 1 transakcji na tekst rekord przesłane opłat.

Ten interfejs API wymaga co najmniej 100 rekordów tekstu zostaną przekazane, ale jest przeznaczona do wykrywania tematy przez setki do tysięcy rekordów.


### <a name="topics--submit-job"></a>Tematy — Prześlij zadania

**ADRES URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Wezwanie na przykład**


W ramach połączenia WPIS poniżej prosimy tematy zestawu 100 artykułów, miejsce, w którym przedstawiono artykuły wprowadzania imię i nazwisko, a dwa StopPhrases są uwzględniane.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Treść żądania:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

W poniższych odpowiedzi JobId uzyskać przesłane zadanie:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Lista pojedynczego wyrazu lub wiele fraz programu word, które nie powinny być zwracane jako tematy. Może służyć do filtrowania tematy bardzo ogólne. Na przykład w zestawie danych dotyczące recenzji hotel "hotel" i "hostel" może być fraz za pośrednictwem tabulatora.  

### <a name="topics--poll-for-job-results"></a>Tematy — wyniki ankiety dla zadania

**ADRES URL**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Wezwanie na przykład**

Przekazać JobId zwracane z kroku "Prześlij zadania" do pobierania wyników. Zaleca się połączenie tego punktu końcowego co minutę do stanu = "Pełna" w odpowiedzi. Potrwa około 10 minutach dla zadania do wykonania lub dłużej zadania tysiące rekordów.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Podczas przetwarzania, odpowiedź będzie w następujący sposób:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
        "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


Interfejs API zwraca wynik w formacie JSON w następującym formacie:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
        ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


Właściwości dla każdej części odpowiedzi są następujące:

**Właściwości TopicInfo**

| Klawisz | Opis |
|:-----|:----|
| TopicId | Unikatowy identyfikator dla każdego tematu. |
| Wynik | Liczba rekordów przypisane do tematu. |
| KeyPhrase | Summarizing wyraz lub frazę do tematu. Może być 1 lub wielu wyrazów. |

**Właściwości TopicAssignment**

| Klawisz | Opis |
|:-----|:----|
| Identyfikator | Identyfikator rekordu. Jest równa identyfikator zawarte w danych wejściowych. |
| TopicId | Identyfikator tematu, który rekord został przypisany do. |
| Odległość | UFNOŚĆ, że rekord należy do tematu. Odległość bliżej zero wskazuje ufność wyższą. |
