<properties
pageTitle="Dodawanie łącznika usługi Yammer w aplikacji logika | Microsoft Azure"
description="Omówienie łącznik usługi Yammer z parametrami interfejsu API usługi REST"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-yammer-connector"></a>Wprowadzenie do łącznika usługi Yammer

Połącz się do usługi Yammer do konwersacji programu access w sieci przedsiębiorstwa.

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych.

Za pomocą usługi Yammer można:

- Tworzenie usługi przepływu biznesowych na podstawie danych, które otrzymujesz w usłudze Yammer. 
- Użyj uaktywnia dla jest nowa wiadomość w grupie lub kanał obserwowane.
- Publikowanie wiadomości i Pobierz wszystkie wiadomości i innych elementów za pomocą akcji. Te akcje odpowiedzi, a następnie wprowadź dane wyjściowe dostępne dla innych akcji. Na przykład gdy pojawi się nowa wiadomość, możesz wysłać wiadomości e-mail za pomocą usługi Office 365.

Aby dodać operację w aplikacjach logiki, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje
Usługi Yammer zawiera następujące wyzwalacze i akcje. 

Wyzwalacza | Akcje
--- | ---
<ul><li>Jeśli istnieje nową wiadomość w grupie</li><li>Jeśli istnieje nowej wiadomości w moim następujące kanału</li></ul>| <ul><li>Pobierz wszystkie wiadomości</li><li>Pobiera wiadomości w grupie</li><li>Pobieranie wiadomości z moim następujące kanału</li><li>Ogłoszenie</li><li>Jeśli istnieje nową wiadomość w grupie</li><li>Jeśli istnieje nowej wiadomości w moim następujące kanału</li></ul>

Wszystkie łączniki obsługuje danych w formatach XML i JSON. 

## <a name="create-a-connection-to-yammer"></a>Tworzenie połączenia z usługi Yammer
Aby użyć łącznik usługi Yammer, należy najpierw utworzyć **połączenie** , a następnie odpowiednie szczegóły dotyczące tych właściwości: 

|Właściwość| Wymagane|Opis|
| ---|---|---|
|Tokenu|Tak|Poświadczenia usługi Yammer|

>[AZURE.INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]


>[AZURE.TIP] To połączenie jest używane w innych aplikacjach logika.

## <a name="yammer-rest-api-reference"></a>Usługi Yammer odwołanie interfejsu API usługi REST
Niniejsza dokumentacja jest wersji: 1.0


### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>Pobierz wszystkie wiadomości publicznej w sieci Yammer zalogowany użytkownik
Odpowiada "Wszystkie" konwersacje za pomocą interfejsu sieci web usługi Yammer.  
```GET: /messages.json```

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|older_then|Liczba całkowita|Brak|kwerendy|Brak|Zwraca wiadomości starsze niż określony jako ciąg numeryczny identyfikator wiadomości. To jest przydatne w przypadku podział na strony wiadomości. Na przykład jeśli aktualnie wyświetlaną 20 wiadomości i najstarszego jest liczbą 2912, można dołączyć "? older_than = 2912″ na żądanie, aby otrzymywać wiadomości 20 wcześniejszych niż, jest wyświetlany.|
|newer_then|Liczba całkowita|Brak|kwerendy|Brak|Zwraca komunikaty nowszych niż określony jako ciąg numeryczny identyfikator wiadomości. To użyto sondowania dla nowych wiadomości. Jeśli wyszukujesz wiadomości i najnowszą wiadomość zwrócony 3516, pozwalające żądania z parametrem "? newer_than = 3516″, aby mieć pewność, że nie kopie wiadomości już na stronie.|
|limit|Liczba całkowita|Brak|kwerendy|Brak|Zwraca określoną liczbę wiadomości.|
|strony|Liczba całkowita|Brak|kwerendy|Brak|Uzyskaj określonej strony. Jeśli danych jest większa niż limit, aby uzyskać dostęp do stron sieci Web można użyć w tym polu|


### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|408|Limit czasu żądania|
|429|Zbyt wiele żądań|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|503|Usługa Yammer jest niedostępna|
|504|Limit czasu bramy|
|domyślne|Operacja nie powiodła się.|


### <a name="post-a-message-to-a-group-or-all-company-feed"></a>Publikowanie wiadomości do grupy lub strumieniowego wszystkie firmy
Jeśli podano identyfikator grupy wiadomości będzie zostanie umieszczona w określonej grupie jeszcze będzie zostanie umieszczona w wszystkich kanału informacyjnego firmy.    
```POST: /messages.json``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|dane wejściowe| |tak|Treść|Brak|Opublikuj wiadomość z żądaniem|


### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|201|Utworzone|



## <a name="object-definitions"></a>Definicje obiektów

#### <a name="message-yammer-message"></a>Komunikat o błędzie: Wiadomości usługi Yammer

| Nazwa | Typ danych | Wymagane |
|---|---| --- | 
|Identyfikator|Liczba całkowita|Brak|
|content_excerpt|ciąg|Brak|
|sender_id|Liczba całkowita|Brak|
|replied_to_id|Liczba całkowita|Brak|
|created_at|ciąg|Brak|
|network_id|Liczba całkowita|Brak|
|message_type|ciąg|Brak|
|sender_type|ciąg|Brak|
|adres URL|ciąg|Brak|
|web_url|ciąg|Brak|
|group_id|Liczba całkowita|Brak|
|Treść|nie zdefiniowano|Brak|
|thread_id|Liczba całkowita|Brak|
|direct_message|wartość logiczna|Brak|
|client_type|ciąg|Brak|
|client_url|ciąg|Brak|
|język|ciąg|Brak|
|notified_user_ids|Tablica|Brak|
|prywatność|ciąg|Brak|
|liked_by|nie zdefiniowano|Brak|
|system_message|wartość logiczna|Brak|

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest: Żądanie post usługi Yammer łącznika opublikować w usłudze yammer odpowiada

| Nazwa | Typ danych | Wymagane |
|---|---| --- | 
|Treść|ciąg|tak|
|group_id|Liczba całkowita|Brak|
|replied_to_id|Liczba całkowita|Brak|
|direct_to_id|Liczba całkowita|Brak|
|Emisja|wartość logiczna|Brak|
|temat1|ciąg|Brak|
|temat2|ciąg|Brak|
|topic3|ciąg|Brak|
|topic4|ciąg|Brak|
|topic5|ciąg|Brak|
|topic6|ciąg|Brak|
|topic7|ciąg|Brak|
|topic8|ciąg|Brak|
|topic9|ciąg|Brak|
|topic10|ciąg|Brak|
|topic11|ciąg|Brak|
|topic12|ciąg|Brak|
|topic13|ciąg|Brak|
|topic14|ciąg|Brak|
|topic15|ciąg|Brak|
|topic16|ciąg|Brak|
|topic17|ciąg|Brak|
|topic18|ciąg|Brak|
|topic19|ciąg|Brak|
|topic20|ciąg|Brak|

#### <a name="messagelist-list-of-messages"></a>MessageList: Listy wiadomości

| Nazwa | Typ danych | Wymagane |
|---|---| --- | 
|wiadomości|Tablica|Brak|


#### <a name="messagebody-message-body"></a>Element MessageBody: Treść wiadomości

| Nazwa | Typ danych | Wymagane |
|---|---| --- | 
|przeanalizować|ciąg|Brak|
|zwykły|ciąg|Brak|
|sformatowany|ciąg|Brak|

#### <a name="likedby-liked-by"></a>LikedBy: Łączone według

| Nazwa | Typ danych | Wymagane |
|---|---| --- | 
|Liczba|Liczba całkowita|Brak|
|nazwy|Tablica|Brak|

#### <a name="yammmerentity-liked-by"></a>YammmerEntity: Łączone według

| Nazwa | Typ danych | Wymagane |
|---|---| --- | 
|Typ|ciąg|Brak|
|Identyfikator|Liczba całkowita|Brak|
|full_name|ciąg|Brak|


## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png
