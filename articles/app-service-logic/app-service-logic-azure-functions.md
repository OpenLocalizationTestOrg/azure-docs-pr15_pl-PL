<properties
   pageTitle="Funkcje Azure za pomocą aplikacji logika | Microsoft Azure"
   description="Dowiedz się, jak używać funkcji Azure z aplikacjami logiczny"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="using-azure-functions-with-logic-apps"></a>Funkcje Azure za pomocą aplikacji warunków logicznych

Niestandardowe wstawki C# lub node.js może zostać uruchomiony przy użyciu funkcji Azure z poziomu aplikacji logiczny.  [Funkcje Azure](../azure-functions/functions-overview.md) oferuje serwera korzystanie platformy Microsoft Azure. To jest przydatne w przypadku wykonywania następujących zadań:

* Zaawansowane formatowanie lub obliczeń w polach daty w aplikacji logiczny
* Wykonywanie obliczeń w ramach przepływu pracy
* Rozszerzanie funkcji logicznych aplikacji przy użyciu funkcji, które są obsługiwane w C# lub node.js

## <a name="create-a-function-for-logic-apps"></a>Tworzenie funkcji dla aplikacji logika

Zaleca się utworzenie nowej funkcji w portalu funkcji Azure za pomocą szablonów **Ogólnego Webhook - węzeł** lub **Ogólnego Webhook - C#** . Spowoduje to automatyczne wypełnienie szablonu, który akceptuje `application/json` za pomocą aplikacji logicznych.  Wybierz kartę **Integracja** w funkcjach Azure powinien mieć **Tryb** **Webhook** i **Typ Webhook** **Ogólnego JSON**.  Funkcje, które używają tych szablonów będą automatycznie wykryte i wymienione w Projektancie logiczny aplikacje w obszarze **Funkcje Azure w moim regionie.**

Funkcje Webhook Akceptacja zlecenia i przekazać je do metody za pośrednictwem `data` zmiennej. Dostęp do właściwości usługi ładunku przy użyciu notacji kropkę, takich jak `data.foo`.  Na przykład prostej funkcji JavaScript, która konwertuje wartość DateTime ciąg daty wygląda następująco:

```
function start(req, res){
    var data = req.body;
    res = {
        body: data.date.ToDateString();
    }
}
```

## <a name="call-azure-functions-from-a-logic-app"></a>Wywołują funkcje Azure za pomocą aplikacji logika

W Projektancie po kliknięciu menu **Akcje** , można wybrać **Funkcje Azure w moim regionie**.  Wyświetla kontenery, w ramach subskrypcji, a pozwala wybrać funkcję, którą chcesz nawiązać połączenie.  

Po wybraniu funkcji zostanie wyświetlony monit o określić obiekt ładunku wprowadzania danych. Jest to wiadomość, którą aplikację logiczny wysyła funkcji i musi być obiekt JSON. Na przykład jeśli chcesz przekazać daty **Ostatniej modyfikacji** z wyzwalacza usług Salesforce ładunku funkcji może wyglądać następująco:

![Data ostatniej zmodyfikowanym kolorem][1]

## <a name="trigger-logic-apps-from-a-function"></a>Aplikacje logiczny wyzwalacza z funkcji

Użytkownik może również wyzwalanie aplikacji logika ze w funkcji.  Aby to zrobić, po prostu tworzenie aplikacji logika z ręcznego wyzwalacza. Aby uzyskać więcej informacji zobacz [aplikacje logiczny jako żądanie punkty końcowe](app-service-logic-http-endpoint.md).  Następnie w swojej funkcji Generowanie HTTP POST do adresu URL ręcznego wyzwalacza ładunek, który ma zostać wysłany do aplikacji logicznych.

### <a name="create-a-function-from-the-designer"></a>Tworzenie funkcji z projektanta

Można także tworzyć node.js funkcji webhook z w projektancie. Najpierw zaznacz **Funkcje Azure w moim regionie** , a następnie wybierz kontener funkcja.  Jeśli nie masz jeszcze kontenera, należy utworzyć [portal Azure funkcje](https://functions.azure.com/signin). Następnie wybierz polecenie **Utwórz nowy**.  

Aby wygenerować szablonu na podstawie danych, które chcesz obliczyć, określ obiekt kontekstu, które mają być przekazywane do funkcji. Musi to być obiektu JSON. Na przykład jeśli przekazać zawartość pliku z akcji FTP, ładunek kontekstu będzie wyglądać następująco:

![Kontekst ładunku][2]

>[AZURE.NOTE] Ponieważ ten obiekt nie został rzutowane jako ciąg, zawartość zostanie dodany bezpośrednio do ładunku JSON. Jednak go błędu się, jeśli zostanie nie jest token JSON (czyli ciągu lub JSON tablica i obiektów). Aby zrzutowania go jako ciąg, po prostu dodać ofert, jak pokazano na pierwszym rysunku, w tym artykule.

Projektant generuje następnie funkcja szablonu możesz utworzyć w tekście. Zmienne są tworzone wstępnie w zależności od kontekstu, które mają być przekazywane do funkcji.




<!--Image references-->
[1]: ./media/app-service-logic-azure-functions/callFunction.png
[2]: ./media/app-service-logic-azure-functions/createFunction.png
