<properties
pageTitle="RSS | Microsoft Azure"
description="Tworzenie aplikacji logika z usługą Azure aplikacji. Łącznik RSS umożliwia użytkownikom publikowanie i pobieranie elementów kanału informacyjnego. Umożliwia także użytkownikom wyzwalanie operacje po opublikowaniu nowego elementu w źródle danych."
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
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-rss-connector"></a>Wprowadzenie do łącznika RSS
RSS to popularny w Internecie format zespalania używane do publikowania często zaktualizowana zawartość — na przykład wpisy w blogu i nagłówki wiadomości.  Wielu wydawców zawartości zapewniają źródła danych RSS, aby zezwolić użytkownikom na jego subskrypcji.  Za pomocą łącznika RSS na pobieranie podawania przepływu informacji i wyzwalacz nowe elementy są publikowane w źródła danych RSS.

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych. 

Można rozpocząć pracę, tworząc aplikację logiczny teraz, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje

Łącznik RSS może służyć jako akcji; ma trigger(s). Wszystkie łączniki obsługuje danych w formatach XML i JSON. 

 Łącznik RSS występują następujące akcje i/lub trigger(s) dostępne:

### <a name="rss-actions"></a>Akcje danych RSS
Możesz wykonać następujące akcje:

|Akcja|Opis|
|--- | ---|
|[ListFeedItems](connectors-create-api-rss.md#listfeeditems)|Pobierz wszystkie elementy danych RSS.|
### <a name="rss-triggers"></a>Wyzwalacze RSS
Można wykrywać te zdarzenia:

|Wyzwalacza | Opis|
|--- | ---|
|Po opublikowaniu nowego elementu kanału informacyjnego|Nowe źródło danych jest opublikowany uaktywnia przepływu pracy|


## <a name="create-a-connection-to-rss"></a>Tworzenie połączenia funkcji RSS

>[AZURE.INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]

>[AZURE.TIP] To połączenie jest używane w innych aplikacjach logika.

## <a name="reference-for-rss"></a>Odwołania do danych RSS
Dotyczy wersji: 1.0

## <a name="onnewfeed"></a>OnNewFeed
Po opublikowaniu nowego elementu podawania: nowe źródło danych jest opublikowany uaktywnia przepływu pracy 

```GET: /OnNewFeed``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|feedUrl|ciąg|tak|kwerendy|Brak|Adres url źródła danych|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|202|Zaakceptowane|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="listfeeditems"></a>ListFeedItems
Lista wszystkich danych RSS elementy.: pobieranie wszystkich elementów danych RSS. 

```GET: /ListFeedItems``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|feedUrl|ciąg|tak|kwerendy|Brak|Adres url źródła danych|

#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|202|Zaakceptowane|
|400|Nieprawidłowe żądanie|
|401|Brak autoryzacji|
|403|Dostęp zabroniony|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|domyślne|Operacja nie powiodła się.|


## <a name="object-definitions"></a>Definicje obiektów 

### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse [FeedItem]


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|wartość|Tablica|Brak |



### <a name="feeditem"></a>FeedItem


| Nazwa właściwości | Typ danych | Wymagane |
|---|---|---|
|Identyfikator|ciąg|Tak |
|Tytuł|ciąg|Tak |
|zawartość|ciąg|Tak |
|łącza|Tablica|Brak |
|updatedOn|ciąg|Brak |


## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji logiki](../app-service-logic/app-service-logic-create-a-logic-app.md)