<properties
    pageTitle="Uaktualnianie do wersji 2 analizy tekst interfejsu API | Microsoft Azure"
    description="Azure maszynowego uczenia analizy tekstu — uaktualnienie do wersji 2"
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

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>Uaktualnianie do wersji 2 analizy tekst interfejsu API #

Ten przewodnik spowoduje przejście przez proces uaktualniania kodu z [pierwszej wersji interfejsu API](../machine-learning/machine-learning-apps-text-analytics.md) do korzystania z wersji drugiej. 

Jeśli nie użyto funkcji API i chcesz dowiedzieć się więcej, możesz **[dowiedzieć się więcej informacji na temat interfejsu API w tym miejscu](//go.microsoft.com/fwlink/?LinkID=759711)** lub **[Wykonaj Przewodnik Szybki Start](//go.microsoft.com/fwlink/?LinkID=760860)**. Informacje techniczne można znaleźć w **[Definicji interfejsu API](//go.microsoft.com/fwlink/?LinkID=759346)**.

### <a name="part-1-get-a-new-key"></a>Część 1. Uzyskiwanie nowego klucza ###

Najpierw należy uzyskać nowy klucz interfejsu API z **Azure Portal**:

1. Przejdź do usługi analizy tekstu za pomocą [Galerii analizy Cortana](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2). W tym miejscu będzie można również znaleźć łącza do przykładów dokumentacji i kod.

1. Kliknij przycisk **Zarejestruj**. To łącze spowoduje przejście do portalu zarządzania Azure miejsce, w którym można utworzyć konto w usłudze.

1. Wybierz plan. Możesz wybrać **bezpłatne warstwa 5000 transakcje/miesiąc**. Tak jak plan bezpłatne, możesz nie zostanie obciążona dotyczące korzystania z usługi. Należy się zalogować do subskrypcji usługi Azure. 

1. Po zalogowaniu do analizy tekstu będziesz mieć **Klucz interfejsu API**. Skopiować ten klucz będzie potrzebny podczas korzystania z usług interfejsu API.

### <a name="part-2-update-the-headers"></a>Część 2. Aktualizowanie nagłówków ###

Zaktualizuj wartości przesłanych nagłówka, tak jak pokazano poniżej. Należy zauważyć, że nie jest już jest zakodowany klucz konta.

**Wersja 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**W wersji 2**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>Część 3. Aktualizowanie podstawowy adres URL ###

**Wersja 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**W wersji 2**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>Część 4a. Aktualizowanie formatów upodobania, fraz klucza i języki ###

#### <a name="endpoints"></a>Punkty końcowe ####

Uzyskiwanie punkty końcowe mają teraz została zastąpiona, aby wszystkie dane wejściowe powinny być przekazywane w żądania POST. Zaktualizuj punkty końcowe do nich, jak pokazano poniżej.

| |Jeden punkt końcowy w wersji 1|Punkt końcowy partii w wersji 1|Punkt końcowy w wersji 2|
|---|---|---|---|
|Typ połączenia|Pobierz|WPIS|WPIS|
|Upodobania|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Zwroty klucza|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Języki|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>Formaty wprowadzania ####

Należy zauważyć, że teraz zaakceptowaniu tego tylko formatu WPIS, należy zmienić wszelkie wprowadzone dane, które poprzednio używane w związku z tym punkty końcowe pojedynczy dokument. Wartości wejściowych nie jest uwzględniana wielkość liter.

**W wersji 1 (partii)**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**W wersji 2**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>Dane wyjściowe upodobania ####

**Wersja 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**W wersji 2**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>Wynikiem wyrażenia klucza ####

**Wersja 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**W wersji 2**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>Dane wyjściowe z języków ####


**Wersja 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**W wersji 2**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>Część 4b. Aktualizowanie formatów tematów ###

#### <a name="endpoints"></a>Punkty końcowe ####

| |Punkt końcowy w wersji 1 | Punkt końcowy w wersji 2|
|---|---|---|
|Przesyłanie do wykrywania tematu (POST)|```StartTopicDetection```|```topics```|
|Uzyskiwanie zdalnego dostępu do wyników tematu (Pobierz)|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>Formaty wprowadzania ####

**Wersja 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**W wersji 2**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>Wyniki przesyłania ####

**W wersji 1 (POST)**

Wcześniej po zakończeniu wykonywania zadania, zostanie wyświetlony następujący komunikat JSON, miejsce, w którym jobId może być dołączane do adresu URL do pobierania danych wyjściowych.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**W wersji 2 (POST)**

Odpowiedź teraz będzie zawierać wartość nagłówka w następujący sposób, gdzie `operation-location` jest używany jako punkt końcowy pobierania wyników:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>Wyniki operacji ####

**W wersji 1 (Pobierz)**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**W wersji 2 (Pobierz)**

Jak poprzednio zwracana jest **regularnie sprawdzają wynik** (sugerowanego okresu jest co minutę) do wyniku. 

Po zakończeniu występują interfejsu API tematy odczytu stanu `succeeded` zostaną zwrócone. To będzie zawierać wynik w formacie, jak pokazano poniżej:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>Część 5. Należy je przetestować! ###

Teraz należy kontynuować! Testowanie kodu przy użyciu mały przykładowy, aby upewnić się, że możesz pomyślnie przetwarzanie danych.
