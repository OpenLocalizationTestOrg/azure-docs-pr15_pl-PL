<properties
    pageTitle="Azure wyzwalacza czasomierza funkcje | Microsoft Azure"
    description="Zapoznanie się z użyciem wyzwalaczy czasomierza w funkcjach Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Funkcje Azure, funkcje, przetwarzanie zdarzenia, dynamiczne obliczeń pliki architektura"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-timer-trigger"></a>Azure wyzwalacza czasomierza funkcje

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule wyjaśniono, jak skonfigurować wyzwalacze czasomierz w funkcjach Azure. Czasomierz Uaktywnia funkcje połączenia na podstawie serii rozłożonych w czasie, jeden raz lub cykliczne.  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>Function.JSON wyzwalacza czasomierz

Plik *function.json* zawiera wyrażenie harmonogramu. Na przykład następującym harmonogramem uruchamia funkcję co minutę:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Wyzwalacza czasomierza obsługi wielu wystąpień skala w nowym oknie automatycznie: tylko jedno wystąpienie funkcji określonego czasomierza będzie działać we wszystkich wystąpieniach.

## <a name="format-of-schedule-expression"></a>Format wyrażenia harmonogramu

Wyrażenie harmonogram jest [CRON wyrażenie](http://en.wikipedia.org/wiki/Cron#CRON_expression) , które zawiera pola 6: `{second} {minute} {hour} {day} {month} {day of the week}`. 

Uwaga wiele wyrażeń cron, które możesz znaleźć online pominąć {drugiego} pola, aby po skopiowaniu z jednej z tych musisz dostosować dodatkowe pola. 

Oto kilka innych harmonogram przykłady wyrażeń:

Aby wyzwolić co 5 minut:

```json
"schedule": "0 */5 * * * *"
```

Aby wyzwolić raz, w górnej części każdej godziny:

```json
"schedule": "0 0 * * * *",
```

Aby wyzwolić co dwie godziny:

```json
"schedule": "0 0 */2 * * *",
```

Aby wyzwolić co godzinę z 9 AM do 17: 00:

```json
"schedule": "0 0 9-17 * * *",
```

Aby wyzwolić na 9:30 AM każdego dnia:

```json
"schedule": "0 30 9 * * *",
```

Aby wyzwolić na 9:30 AM każdy dzień tygodnia:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>Przykład kod wyzwalacza C# czasomierz

W tym przykładzie kodu C# zapisuje jeden dziennik każdym razem, gdy zostanie wywołana funkcja.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>Następne kroki

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
