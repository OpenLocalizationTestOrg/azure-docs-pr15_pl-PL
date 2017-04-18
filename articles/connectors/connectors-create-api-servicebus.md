<properties
pageTitle="Dowiedz się, jak używać łącznika Bus usługi Azure w aplikacji logiki | Microsoft Azure"
description="Tworzenie aplikacji logika z usługą Azure aplikacji. Podłącz do Bus usługi Azure do wysyłania i odbierania wiadomości. Można było wykonywać czynności, takich jak wysyłanie do kolejki, Wyślij do tematu odbierać z kolejki i odbierać z subskrypcji."
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/02/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-azure-service-bus-connector"></a>Wprowadzenie do łącznika Bus usługi Azure

Podłącz do Bus usługi Azure do wysyłania i odbierania wiadomości. Można było wykonywać czynności, takie jak Wyślij do kolejki, Wyślij do tematu odbierać z kolejki i odbierać z subskrypcji.

Aby użyć [dowolny łącznik](./apis-list.md), najpierw należy utworzyć aplikację logicznych. Można rozpocząć, [tworząc aplikację logiczny, który jest teraz](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-service-bus"></a>Nawiązywanie połączenia z Bus usługi

Przed aplikacji logika uzyskać dostęp do dowolnej usługi, należy najpierw utworzyć połączenie z usługą. [Połączenia](./connectors-overview.md) umożliwia łączność aplikacji logiki i innej usługi.  

>[AZURE.INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## <a name="use-a-service-bus-trigger"></a>Użyj wyzwalacza Bus usługi

Wyzwalacz to zdarzenie, które może służyć do uruchamiania przepływu pracy, zdefiniowane w aplikacji logicznych. [Dowiedz się więcej o wyzwalaczy](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]  

## <a name="use-a-service-bus-action"></a>Za pomocą akcji Bus usługi

Akcja jest czynność wykonaną przez przepływ pracy zdefiniowane w aplikacji logicznych. [Dowiedz się więcej o akcje](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[AZURE.INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]  

## <a name="technical-details"></a>Szczegóły techniczne

Poniżej przedstawiono szczegółowe informacje dotyczące wyzwalacze, akcje i odpowiedzi, które obsługuje to połączenie.

### <a name="service-bus-triggers"></a>Usługa Bus wyzwalaczy

Usługa Bus ma wyzwalaczy następujące czynności:  

|Wyzwalacza | Opis|
|--- | ---|
|[Po odebraniu wiadomości w kolejce](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue)|Operacja uaktywnia przepływu po odebraniu wiadomości w kolejce.|
|[Po odebraniu wiadomości subskrypcji tematu](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription)|Operacja uaktywnia przepływu wiadomość zostanie odebrana subskrypcji tematu.|


### <a name="service-bus-actions"></a>Akcje Bus usługi

Usługa Bus obejmuje następujące czynności:


|Akcja|Opis|
|--- | ---|
|[Wysyłanie wiadomości](connectors-create-api-servicebus.md#send-message)|Operacja wysyła wiadomość do kolejki lub temat.|
### <a name="action-and-trigger-details"></a>Szczegóły akcji i wyzwalacz

Oto szczegóły akcji i wyzwalaczy dla tego łącznika, wraz z ich odpowiedziami.



#### <a name="send-message"></a>Wysyłanie wiadomości

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|ContentData *|Zawartość|Zawartość wiadomości.|
|Typ zawartości|Typ zawartości|Typ zawartości treść wiadomości.|
|Właściwości|Właściwości|Pary klucz wartość dla każdego brokered właściwości.|
|Nazwa *|Nazwa kolejki/tematu|Nazwa kolejki lub temat.|

Zaawansowane parametry są również dostępne:

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|Identyfikator komunikatu|Identyfikator wiadomości|Wartości zdefiniowane przez użytkownika Bus usługi służy do identyfikowania zduplikowanych wiadomości, jeśli włączono tę funkcję.|
|Aby|Aby|Adres, aby wysłać do.|
|Parametr from|Odpowiadanie na|Adres kolejki, aby odpowiedzieć.|
|ReplyToSessionId|Odpowiadanie na identyfikator sesji|Identyfikator sesji, aby odpowiedzieć.|
|Etykieta|Etykieta|Etykieta specyficzne dla aplikacji.|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|Data i godzina w czasie UTC, gdy wiadomość zostanie dodany do kolejki.|
|Identyfikator sesji|Identyfikator sesji|Identyfikator sesji.|
|CorrelationId|Identyfikator korelacji|Identyfikator korelacji.|
|Licznika TimeToLive|Czas wygaśnięcia|Czas trwania w znaczniki, że wiadomość jest prawidłowy. Czas trwania zaczyna się od gdy wiadomość jest wysyłana do Bus usługi.|



* Wskazuje, że właściwość jest wymagane.


#### <a name="when-a-message-is-received-in-a-queue"></a>Po odebraniu wiadomości w kolejce

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|NazwaKolejki *|Nazwa kolejki|Nazwa kolejki.|


* Wskazuje, że właściwość jest wymagane.


##### <a name="output-details"></a>Szczegóły wyników

ServiceBusMessage: Ten obiekt ma zawartości i właściwości wiadomości Bus usługi.


| Nazwa właściwości | Typ danych | Opis |
|---|---|---|
|ContentData|ciąg|Zawartość wiadomości.|
|Typ zawartości|ciąg|Typ zawartości treść wiadomości.|
|Właściwości|obiekt|Pary klucz wartość dla każdego brokered właściwości.|
|Identyfikator komunikatu|ciąg|Wartości zdefiniowane przez użytkownika Bus usługi służy do identyfikowania zduplikowanych wiadomości, jeśli włączono tę funkcję.|
|Aby|ciąg|Wyślij do adresu.|
|Parametr from|ciąg|Adres kolejki, aby odpowiedzieć.|
|ReplyToSessionId|ciąg|Identyfikator sesji, aby odpowiedzieć.|
|Etykieta|ciąg|Etykieta specyficzne dla aplikacji.|
|ScheduledEnqueueTimeUtc|ciąg|Data i godzina w czasie UTC, gdy wiadomość zostanie dodany do kolejki.|
|Identyfikator sesji|ciąg|Identyfikator sesji.|
|CorrelationId|ciąg|Identyfikator korelacji.|
|Licznika TimeToLive|ciąg|Czas trwania w znaczniki, że wiadomość jest prawidłowy. Czas trwania zaczyna się od gdy wiadomość jest wysyłana do Bus usługi.|




#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>Po odebraniu wiadomości subskrypcji tematu

|Nazwa właściwości| Nazwa wyświetlana|Opis|
| ---|---|---|
|topicName *|Nazwa tematu|Nazwa tematu.|
|subscriptionName *|Temat nazwy subskrypcji.|Nazwa subskrypcji tematu.|


* Wskazuje, że właściwość jest wymagane.


##### <a name="output-details"></a>Szczegóły wyników

ServiceBusMessage: Ten obiekt ma zawartości i właściwości wiadomości Bus usługi.


| Nazwa właściwości | Typ danych | Opis |
|---|---|---|
|ContentData|ciąg|Zawartość wiadomości.|
|Typ zawartości|ciąg|Typ zawartości treść wiadomości.|
|Właściwości|obiekt|Pary klucz wartość dla każdego brokered właściwości.|
|Identyfikator komunikatu|ciąg|Wartości zdefiniowane przez użytkownika Bus usługi służy do identyfikowania zduplikowanych wiadomości, jeśli włączono tę funkcję.|
|Aby|ciąg|Wyślij do adresu.|
|Parametr from|ciąg|Adres kolejki, aby odpowiedzieć.|
|ReplyToSessionId|ciąg|Identyfikator sesji, aby odpowiedzieć.|
|Etykieta|ciąg|Etykieta specyficzne dla aplikacji.|
|ScheduledEnqueueTimeUtc|ciąg|Data i godzina w czasie UTC, gdy wiadomość zostanie dodany do kolejki.|
|Identyfikator sesji|ciąg|Identyfikator sesji.|
|CorrelationId|ciąg|Identyfikator korelacji.|
|Licznika TimeToLive|ciąg|Czas trwania w znaczniki, że wiadomość jest prawidłowy. Czas trwania zaczyna się od gdy wiadomość jest wysyłana do Bus usługi.|



### <a name="http-responses"></a>Odpowiedzi HTTP

Poprzedniej akcji i wyzwalaczy może zwracać jedną lub więcej z poniższych kodów stanu HTTP:

|Nazwa|Opis|
|---|---|
|200|Ok|
|202|Zaakceptowane|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd.|
|domyślne|Operacja nie powiodła się.|

## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).
