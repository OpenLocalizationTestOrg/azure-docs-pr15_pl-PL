<properties
pageTitle=" Używanie łącznika zapasu czasu w aplikacji logika | Microsoft Azure"
description="Wprowadzenie do korzystania z łącznika zapas czasu w aplikacji Microsoft Azure aplikacji usługi logiczny"
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

# <a name="get-started-with-the-slack-connector"></a>Wprowadzenie do łącznika zapasu czasu

Zapas czasu to narzędzie komunikacji zespołu, która łączy wszystkie zespołu komunikacji w jednym umieścić, od razu można wyszukiwać i dostępne miejsce, w którym możesz przejść.

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych.

Za pomocą łączników zapasu czasu można:

* Umożliwia tworzenie aplikacji warunków logicznych

Aby dodać operację w aplikacjach logiczny, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Załóżmy porozmawiać o wyzwalacze i akcje

Łącznik zapasu czasu może być używany jako akcji; Istnieją wyzwalacze nie. Wszystkie łączniki obsługuje danych w formatach XML i JSON. 

 Łącznik zapasu czasu występują następujące akcje i/lub trigger(s) dostępne:

### <a name="slack-actions"></a>Akcje zapasu czasu
Możesz wykonać następujące akcje:

|Akcja|Opis|
|--- | ---|
|Funkcji postMessage|Publikowanie wiadomości do określonego kanału.|
## <a name="create-a-connection-to-slack"></a>Tworzenie połączenia z zapas czasu
Aby użyć łącznik zapasu czasu, należy najpierw utworzyć **połączenie** , a następnie odpowiednie szczegóły dotyczące tych właściwości: 

|Właściwość| Wymagane|Opis|
| ---|---|---|
|Tokenu|Tak|Poświadczenia zapasu czasu|

Wykonaj poniższe czynności, aby zalogować się do zapas czasu i ukończyć konfigurację zapasu czasu **połączenia** w logiczny aplikacji:

1. Wybierz pozycję **cyklu**
2. Wybierz **Częstotliwość** i wprowadź **interwału**
3. Wybierz pozycję **Dodaj akcję**  
![Konfigurowanie zapas czasu][1]  
4. W polu wyszukiwania wprowadź zapas czasu i poczekaj, aż wyszukiwanie, aby zwracało wszystkie pozycje z zapasu czasu w nazwie
5. Wybierz pozycję **Zapas czasu - ogłoszenie**
6. Wybierz pozycję **Zaloguj się do zapas czasu**:  
![Konfigurowanie zapas czasu][2]
7. Podaj poświadczenia zapasu czasu, aby zalogować się do zezwolić aplikacji    
![Konfigurowanie zapas czasu][3]  
8. Nastąpi przekierowanie do dziennika Twojej organizacji na stronie. **Autoryzuj** Zapas czasu na interakcję z aplikacji logiczny:      
![Konfigurowanie zapas czasu][5] 
9. Po zakończeniu zezwolenie nastąpi przekierowanie do aplikacji logiki do jego wykonaniem przez skonfigurowanie sekcji **Zapas czasu — pobranie wszystkich wiadomości** . Dodawanie innych wyzwalacze i akcje, które są potrzebne.  
![Konfigurowanie zapas czasu][6]
10. Zapisz swoją pracę, wybierając pozycję **Zapisz** na pasku menu powyżej.


>[AZURE.TIP] To połączenie jest używane w innych aplikacjach logicznych.

## <a name="slack-rest-api-reference"></a>Zapas czasu odwołanie interfejsu API usługi REST
#### <a name="this-documentation-is-for-version-10"></a>Niniejsza dokumentacja jest wersji: 1.0


### <a name="post-a-message-to-a-specified-channel"></a>Publikowanie wiadomości do określonego kanału.
**```POST: /chat.postMessage```** 



| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|kanał|ciąg|tak|kwerendy|Brak|Kanału, grupy prywatnej lub kanału za pomocą wiadomości Błyskawicznych do wysłania wiadomości. Nazwa (ex: #general) lub zakodowany.|
|tekst|ciąg|tak|kwerendy|Brak|Tekst wysyłanej wiadomości. Aby uzyskać opcji formatowania, zobacz https://api.slack.com/docs/formatting.|
|Nazwa użytkownika|ciąg|Brak|kwerendy|Brak|Nazwa robot|
|as_user|wartość logiczna|Brak|kwerendy|Brak|Przekazać wartość PRAWDA, aby opublikować wiadomość jako uwierzytelniony użytkownik, a nie jako Robot sieci|
|analizy|ciąg|Brak|kwerendy|Brak|Zmienianie sposobu wiadomości są traktowane. Aby uzyskać szczegółowe informacje Zobacz https://api.slack.com/docs/formatting.|
|link_names|Liczba całkowita|Brak|kwerendy|Brak|Znajdowanie i łączenie kanału nazw i nazw użytkowników.|
|unfurl_links|wartość logiczna|Brak|kwerendy|Brak|Przekazać wartość PRAWDA, aby włączyć unfurling przede wszystkim opartych na tekście zawartości.|
|unfurl_media|wartość logiczna|Brak|kwerendy|Brak|Przekazać wartość FAŁSZ, aby wyłączyć unfurling z zawartości multimedialnej.|
|icon_url|ciąg|Brak|kwerendy|Brak|Adres URL obrazu, który użyć jako ikony dla tej wiadomości|
|icon_emoji|ciąg|Brak|kwerendy|Brak|Emoji ma zostać użyte jako ikona dla tej wiadomości|


### <a name="here-are-the-possible-responses"></a>Poniżej przedstawiono możliwe odpowiedzi:

|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|408|Limit czasu żądania|
|429|Zbyt wiele żądań|
|500|Wewnętrzny błąd serwera. Wystąpił nieznany błąd|
|503|Zapas czasu usługa jest niedostępna|
|504|Limit czasu bramy|
|domyślne|Operacja nie powiodła się.|
------



## <a name="object-definitions"></a>Definicje obiektu: 

 **Komunikat**: usługi Yammer wiadomości

Wymagane właściwości dla wiadomości:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|Identyfikator|Liczba całkowita|
|content_excerpt|ciąg|
|sender_id|Liczba całkowita|
|replied_to_id|Liczba całkowita|
|created_at|ciąg|
|network_id|Liczba całkowita|
|message_type|ciąg|
|sender_type|ciąg|
|adres URL|ciąg|
|web_url|ciąg|
|group_id|Liczba całkowita|
|Treść|nie zdefiniowano|
|thread_id|Liczba całkowita|
|direct_message|wartość logiczna|
|client_type|ciąg|
|client_url|ciąg|
|język|ciąg|
|notified_user_ids|Tablica|
|prywatność|ciąg|
|liked_by|nie zdefiniowano|
|system_message|wartość logiczna|



 **PostOperationRequest**: reprezentuje żądanie post usługi Yammer łącznika opublikować w usłudze yammer

Wymagane właściwości PostOperationRequest:

Treść

**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|Treść|ciąg|
|group_id|Liczba całkowita|
|replied_to_id|Liczba całkowita|
|direct_to_id|Liczba całkowita|
|Emisja|wartość logiczna|
|temat1|ciąg|
|temat2|ciąg|
|topic3|ciąg|
|topic4|ciąg|
|topic5|ciąg|
|topic6|ciąg|
|topic7|ciąg|
|topic8|ciąg|
|topic9|ciąg|
|topic10|ciąg|
|topic11|ciąg|
|topic12|ciąg|
|topic13|ciąg|
|topic14|ciąg|
|topic15|ciąg|
|topic16|ciąg|
|topic17|ciąg|
|topic18|ciąg|
|topic19|ciąg|
|topic20|ciąg|



 **MessageList**: listy wiadomości

Wymagane właściwości MessageList:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|wiadomości|Tablica|



 **Element MessageBody**: treść wiadomości

Wymagane właściwości Element MessageBody:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|przeanalizować|ciąg|
|zwykły|ciąg|
|sformatowany|ciąg|



 **LikedBy**: łączone według

Wymagane właściwości LikedBy:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|Liczba|Liczba całkowita|
|nazwy|Tablica|



 **YammmerEntity**: łączone według

Wymagane właściwości YammmerEntity:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|Typ|ciąg|
|Identyfikator|Liczba całkowita|
|full_name|ciąg|


## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji warunków logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md)
## <a name="object-definitions"></a>Definicje obiektu: 

 **WebResultModel**: wyniki wyszukiwania w sieci web Bing

Wymagane właściwości WebResultModel:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|Tytuł|ciąg|
|Opis|ciąg|
|DisplayUrl|ciąg|
|Identyfikator|ciąg|
|FullUrl|ciąg|



 **VideoResultModel**: wyniki wyszukiwania wideo Bing

Wymagane właściwości VideoResultModel:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|Tytuł|ciąg|
|DisplayUrl|ciąg|
|Identyfikator|ciąg|
|MediaUrl|ciąg|
|Środowisko uruchomieniowe|Liczba całkowita|
|Miniatura|nie zdefiniowano|



 **ThumbnailModel**: Miniatura właściwości elementu MMS

Wymagane właściwości ThumbnailModel:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|MediaUrl|ciąg|
|Typ zawartości|ciąg|
|Szerokość|Liczba całkowita|
|Wysokość|Liczba całkowita|
|FileSize|Liczba całkowita|



 **ImageResultModel**: wyniki wyszukiwania obrazów Bing

Wymagane właściwości ImageResultModel:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|Tytuł|ciąg|
|DisplayUrl|ciąg|
|Identyfikator|ciąg|
|MediaUrl|ciąg|
|SourceUrl|ciąg|
|Miniatura|nie zdefiniowano|



 **NewsResultModel**: wyniki wyszukiwania usługi Bing wiadomości

Wymagane właściwości NewsResultModel:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|Tytuł|ciąg|
|Opis|ciąg|
|DisplayUrl|ciąg|
|Identyfikator|ciąg|
|Źródła|ciąg|
|Data|ciąg|



 **SpellResultModel**: Wyniki sugestii pisowni Bing

Wymagane właściwości SpellResultModel:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|Identyfikator|ciąg|
|Wartość|ciąg|



 **RelatedSearchResultModel**: Bing związane z wyników wyszukiwania

Wymagane właściwości RelatedSearchResultModel:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|Tytuł|ciąg|
|Identyfikator|ciąg|
|BingUrl|ciąg|



 **CompositeSearchResultModel**: wyniki wyszukiwania złożone Bing

Wymagane właściwości CompositeSearchResultModel:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|WebResultsTotal|Liczba całkowita|
|ImageResultsTotal|Liczba całkowita|
|VideoResultsTotal|Liczba całkowita|
|NewsResultsTotal|Liczba całkowita|
|SpellSuggestionsTotal|Liczba całkowita|
|WebResults|Tablica|
|ImageResults|Tablica|
|VideoResults|Tablica|
|NewsResults|Tablica|
|SpellSuggestionResults|Tablica|
|RelatedSearchResults|Tablica|


## <a name="object-definitions"></a>Definicje obiektu: 

 **PostOperationResponse**: reprezentuje odpowiedzi operację łącznika zapas czasu wpisy zapas czasu

Wymagane właściwości PostOperationResponse:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|Ok|wartość logiczna|
|kanał|ciąg|
|TS|ciąg|
|Komunikat|nie zdefiniowano|
|Błąd|ciąg|



 **MessageItem**: wiadomości kanału.

Wymagane właściwości MessageItem:


Brak właściwości są wymagane. 


**Wszystkie właściwości**: 


| Nazwa | Typ danych |
|---|---|
|tekst|ciąg|
|Identyfikator|ciąg|
|użytkownika|ciąg|
|utworzone|Liczba całkowita|
|usunięte is_user|wartość logiczna|


## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji warunków logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
