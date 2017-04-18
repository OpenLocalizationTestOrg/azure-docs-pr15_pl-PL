<properties
    pageTitle="Przewodnik Szybki start: maszynowego uczenia tekstu analizy API | Microsoft Azure"
    description="Azure maszynowego uczenia analizy tekstu — przewodnik Szybki Start"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>Wprowadzenie do interfejsów API analizy tekstu wykrywanie upodobania, klucza fraz, tematy i języka

<a name="HOLTop"></a>

W tym dokumencie opisano sposób do wewnętrznego usłudze lub aplikacji używanie [Interfejsów API analizy tekstu](//go.microsoft.com/fwlink/?LinkID=759711).
Te interfejsy API umożliwia wykrywanie upodobania, klucza fraz, tematy i języka tekstu. [Kliknij tutaj, aby obejrzeć pokaz interakcyjne środowiska.](//go.microsoft.com/fwlink/?LinkID=759712)

Zajrzyj do [definicji interfejsu API](//go.microsoft.com/fwlink/?LinkID=759346) dokumentację techniczną za pośrednictwem interfejsów API.

Ten przewodnik jest interfejsów API w wersji 2. Aby uzyskać szczegółowe informacje w wersji 1 interfejsów API, [odwołują się do tego dokumentu](../machine-learning/machine-learning-apps-text-analytics.md).

Na koniec tego samouczka można programowo wykrywanie:

- **Upodobania** - jest tekstem, dodatnie lub ujemne?

- **Zwroty klucza** - co to są osoby omawiane w jednym artykule?

- **Tematy** - co to są omawiane przez wiele artykułów?

- **Języki** — język to tekst napisany w?

Zauważ, że ten interfejs API 1 transakcji na dokument przesłany opłat. Na przykład żądanie upodobania 1000 dokumentów w rozmowie w jednej transakcji 1000 będą odejmowane.



<a name="Overview"></a>
## <a name="general-overview"></a>Ogólne omówienie ##

Ten dokument jest przewodnik krok po kroku. Naszym celem jest użytkownik mógłby wykonać kroki niezbędne do przeszkolenie modelu i wskaż zasoby, które będzie można je umieścić w produkcji. Tego ćwiczenia potrwa około 30 minut.

Dla tych zadań należy edytor, a połączenie RESTful punkty końcowe w wybranym języku.

Zaczynamy!

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>1 - zadanie podpisywania do interfejsów API analizy tekstu ####

W tym zadaniu będzie Utwórz konto w usłudze analizy tekstu.

1. Przejdź do **Usługi poznawcze** w [Azure Portal](//go.microsoft.com/fwlink/?LinkId=761108) i upewnij się, że wybrano **Analizy tekstu** jako "typ interfejsu API".

1. Wybierz plan. Możesz wybrać **bezpłatne warstwa 5000 transakcje/miesiąc**. Tak jak plan bezpłatne, możesz nie zostanie obciążona dotyczące korzystania z usługi. Należy się zalogować do subskrypcji usługi Azure. 

1. Wypełnij inne pola i utworzyć konto.

1. Po zalogowaniu do analizy tekstu znaleźć swój **Klucz interfejsu API**. Skopiować klucza podstawowego, będą potrzebne podczas korzystania z usług interfejsu API.


## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>Zadanie 2 - wykryć upodobania, fraz klucza i języki ####

Jest proste wykrywanie upodobania, fraz klucza i języki w tekście. Programowo zostanie wyświetlony ten sam efekt jak zwraca [obsługi pokaz](//go.microsoft.com/fwlink/?LinkID=759712) .

>[AZURE.TIP] Na potrzeby analizy upodobania zalecamy dzielenie tekstu na zdań. Prowadzi to zazwyczaj większą dokładność w przewidywań upodobania.

Należy zauważyć, że obsługiwane języki w następujący sposób:

| Funkcja | Kody języków obsługiwanych |
|:-----|:----|
| Upodobania | `en`(W języku angielskim), `es` (hiszpański), `fr` (francuski), `pt` (portugalski) |
| Zwroty klucza | `en`(W języku angielskim), `es` (hiszpański), `de` (niemiecki), `ja` (japoński) |


1. Konieczne będzie następująca wartość nagłówków. Zauważ, że JSON jest obecnie tylko zaakceptowanych format wprowadzania danych za pośrednictwem interfejsów API. XML nie jest obsługiwane.

        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json

1. Następnie formatowanie do wprowadzania wierszy w formacie JSON. Upodobania fraz klucza i język format jest taki sam. Zauważ, że każdy identyfikator musi być unikatowa i identyfikator zwracaną przez system. Maksymalny rozmiar pojedynczy dokument, który może być przesyłany wynosi 10KB i sumy maksymalnego rozmiaru przesyłanych danych wejściowych jest 1MB. Nie więcej niż 1000 dokumenty mogą być przedstawiane w jednym połączenia. Ograniczanie szybkości istnieje wynosi 100 połączeń minutę — Dlatego zalecamy przesyłania dużych ilości dokumentów w jednej połączenia. Język jest parametr opcjonalny, który należy określić, jeśli analizowanie tekstu innej niż angielska. Przykład danych wejściowych przedstawiono poniżej, gdzie parametr opcjonalny `language` dla upodobania analizy lub klawisz wyodrębniania frazę jest dołączany:

        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }

1. Nawiązywanie połączenia **wpisu** z systemem przy danych wejściowych upodobania, fraz klucza i języka. Adresy URL będzie wyglądać następująco:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. To połączenie może zwrócić JSON sformatowane odpowiedzi identyfikatorami i wykryto właściwości. Poniżej przedstawiono przykład wyjściowych upodobania (z szczegóły błędu wyłączone). W przypadku upodobania zostanie zwrócony wynik od 0 do 1 dla każdego dokumentu:

        // Sentiment response
        {
            "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }

        // Key phrases response
        {
            "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }

        // Languages response
        {
            "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }


## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>Zadanie 3 — wykrywanie tematy w Boże tekstu ####

To jest interfejs API nowo opublikowanej, która oblicza góry wykryte tematów, aby uzyskać listę przesłane rekordy tekstu. Temat jest identyfikowany klucza frazą, który może mieć jedną lub więcej zawartości związanej z wyrazów. Interfejs API jest przeznaczona do pracy dla ludzi, krótka pisanych tekstu, takie jak przeglądy oraz opinii użytkowników.

Ten interfejs API wymaga **co najmniej 100 rekordów tekstu** przesłane, ale jest przeznaczona do wykrywania tematy przez setki do tysięcy rekordów. Dowolnej innej niż angielska rekordów lub rekordów przy użyciu wyrazów mniej niż 3 będą odrzucane i dlatego nie są przypisywane do tematów. Wykrywania tematu maksymalny rozmiar pojedynczy dokument, który może być przesyłany wynosi 30KB, a całkowita maksymalnego rozmiaru przesyłanych danych wejściowych jest 30MB. Wykrywanie tematu jest wskaźnikiem ograniczona do 5 przesłanych elementów co 5 minut.

Istnieją dwie dodatkowe **opcjonalne** parametrów wejściowych, które pomagają w celu poprawy jakości wyników:

- **Zatrzymywanie wyrazów.**  Te wyrazy i zamknij formularzy (np. liczba mnoga) zostaną wyłączone z proces wykrywania cały temat. Za pomocą tej typowych słów (na przykład "problem", "błąd" i "użytkownika" może być opcjami odpowiednimi dla skargi klientów dotyczących oprogramowania). Każdy ciąg należy pojedynczego wyrazu.
- **Zatrzymywanie fraz** - tych fraz zostaną wykluczone z listę tematów dotyczących zwracane. Umożliwia wykluczanie ogólnego tematy, które nie mają być wyświetlane w wynikach. Na przykład "Microsoft" i "Azure" będzie opcjami odpowiednimi dla tematów, aby wykluczyć. Ciągi mogą zawierać wiele wyrazów.

Wykonaj poniższe czynności wykrywanie tematów w tekście.

1. Formatowanie danych wejściowych w formacie JSON. Tym razem można zdefiniować wyrazy i zatrzymywać fraz.

        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }

1. Za pomocą samego nagłówków, zgodnie z definicją w zadaniu 2, wprowadź **WPIS** połączeń do punktu końcowego tematy:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. Spowoduje to przywrócenie `operation-location` w nagłówku odpowiedzi, której wartość jest adres URL do kwerendy w celu utworzenia tematach:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. Kwerenda zwracane `operation-location` okresowo z wezwaniem na **Pobieranie** . Raz na minutę jest zalecane.

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. Punkt końcowy może zwrócić odpowiedź, w tym `{"status": "notstarted"}` przed rozpoczęciem przetwarzania, `{"status": "running"}` podczas przetwarzania i `{"status": "succeeded"}` na dane wyjściowe po zakończeniu projektu. Następnie można wykorzystać dane wyjściowe, które będą w następującym formacie (szczegóły Uwaga takie jak błąd format i terminy zostały wyłączone w tym przykładzie):

        {
            "status": "succeeded",
            "operationProcessingResult": {
                "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

Należy zauważyć, że pomyślną odpowiedź, aby zapoznać się z tematami `operations` punkt końcowy ma następującego schematu:

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

Objaśnienia dla każdej części tej odpowiedzi są następujące:

**Tematy**

| Klawisz | Opis |
|:-----|:----|
| Identyfikator | Unikatowy identyfikator dla każdego tematu. |
| wynik | Liczba dokumenty przypisane do tematu. |
| keyPhrase | Summarizing wyraz lub frazę do tematu. |

**topicAssignments**

| Klawisz | Opis |
|:-----|:----|
| documentId | Identyfikator dokumentu. Jest równa identyfikator zawarte w danych wejściowych. |
| topicId | Identyfikator tematu, który został przypisany do dokumentu. |
| odległość | Wynik przynależność do dokumentu do tematu od 0 do 1. Dolny a odległość wynik jest silniejsza przynależność tematu. |

**błędy**

| Klawisz | Opis |
|:-----|:----|
| Identyfikator | Unikatowy identyfikator dokumentu wprowadzania komunikat o błędzie odwołuje się do. |
| Komunikat | Komunikat o błędzie. |

## <a name="next-steps"></a>Następne kroki ##

Gratulacje! Za pomocą analizy tekstu na danych zostało zakończone. Możesz teraz kwestii wizualizowanie danych za pomocą narzędzi, takich jak [Usługi Power BI](//powerbi.microsoft.com) , a także Automatyzowanie usługi wnioski, aby zapewnić sobie w czasie rzeczywistym widoku danych tekst.

Aby zobaczyć, jak funkcje analizy tekstu, takie jak upodobania, może służyć jako część Robot sieci, zobacz przykład [Emocjonalne robotów](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot) w witrynie Framework robot.
