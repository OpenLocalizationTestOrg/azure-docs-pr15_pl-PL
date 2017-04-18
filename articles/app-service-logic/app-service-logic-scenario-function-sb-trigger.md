<properties
   pageTitle="Scenariusz aplikacji logiczny: Tworzenie wyzwalacza Azure funkcje usługi Bus | Microsoft Azure"
   description="Tworzenie wyzwalacza Bus usługi aplikacji logika za pomocą funkcji Azure"
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
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Scenariusz aplikacji logiczny: Tworzenie wyzwalacza Bus usługi Azure za pomocą funkcji Azure

Funkcje Azure umożliwia tworzenie wyzwalacza dla aplikacji logika, gdy trzeba wdrożyć długotrwałych odbiornika, lub zadania. Na przykład można utworzyć funkcję, która będzie odsłuchać w kolejce i natychmiast uruchomienie aplikacji dla logiki jako wyzwalacz wypychania.

## <a name="build-the-logic-app"></a>Tworzenie aplikacji warunków logicznych

W tym przykładzie masz funkcji uruchomiony dla każdej aplikacji logiczny, który ma zostać wyzwolone. Najpierw należy utworzyć aplikację logiczny, która ma wyzwalacza żądania HTTP. Funkcja połączeń tego punktu końcowego, po otrzymaniu wiadomości kolejki.  

1. Tworzenie nowej aplikacji logika; Wybierz pozycję wyzwalacz **odebraniu ręczny — po HTTP żądanie** .  
   Opcjonalnie można określić schemat JSON korzystania z tą wiadomością kolejki przy użyciu narzędzi, takich jak [jsonschema.net](http://jsonschema.net). Wklej schematu wyzwalacza. Dzięki temu projektant opis kształtu dane i łatwo przepływ więcej właściwości w przepływie pracy.
1. Dodaj wszelkie dodatkowe kroki, które ma zostać wykonana po odebraniu wiadomości kolejki. Na przykład Wyślij wiadomość e-mail przy użyciu usługi Office 365.  
1. Zapisywanie aplikacji logiki do generowania zwrotnego adresu URL wyzwalacza dla tej aplikacji logicznych. Adres URL zostanie wyświetlone na karcie wyzwalacza.

![Adres URL pojawi się na karcie wyzwalacza wywołanie zwrotne][1]

## <a name="build-the-function"></a>Tworzenie funkcji

Następnie należy utworzyć funkcję, która będzie uruchamiać i odsłuchiwanie kolejki.

1. W [portalu funkcji Azure](https://functions.azure.com/signin)wybierz **Nowej funkcji**, a następnie wybierz szablon **ServiceBusQueueTrigger - C#** .

    ![Portal Azure funkcje][2]

2. Konfigurowanie połączenia do kolejki Bus usługi (który użyje SDK Bus usługi Azure `OnMessageReceive()` odbiornika).
3. Napisać prosty funkcję połączenia punkt końcowy aplikacji logiczny (utworzone wcześniej) przy użyciu wiadomości kolejki jako wyzwalacz. Oto przykład pełny funkcji. W przykładzie użyto `application/json` typu zawartości wiadomości, ale można to zmienić w razie potrzeby.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Aby przetestować, Dodaj wiadomość kolejki przez narzędzie, takie jak [Usługa Bus Eksploratora](https://github.com/paolosalvatori/ServiceBusExplorer). Zobacz aplikacji logika uruchomienie natychmiast po funkcja odbiera wiadomości.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG
