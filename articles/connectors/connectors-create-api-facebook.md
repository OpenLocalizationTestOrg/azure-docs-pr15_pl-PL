<properties
    pageTitle="Dodawanie łącznika serwisu Facebook w aplikacji logika | Microsoft Azure"
    description="Omówienie łącznika serwisu Facebook z parametrami interfejsu API usługi REST"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-facebook-connector"></a>Wprowadzenie do łącznika serwisu Facebook
Łączenie z serwisem Facebook i publikowanie dla osi czasu, a otrzymasz strony kanału i nie tylko. 

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych.


Z serwisem Facebook możesz wykonać następujące czynności:

- Tworzenie usługi przepływu biznesowych na podstawie danych, które otrzymujesz z serwisu Facebook. 
- Użyj wyzwalacza po odebraniu nowego wpisu.
- Używanie akcji, które publikują do osi czasu, Uzyskaj strony kanału i nie tylko. Te akcje odpowiedzi, a następnie wprowadź dane wyjściowe dostępne dla innych akcji. Na przykład gdy na osi czasu znajduje się nowy wpis, możesz wykonać ten wpis i przekazać je do swojego kanału Twitter. 



Aby dodać operację w aplikacjach logiczny, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje
Łącznik Facebook obejmuje następujące wyzwalacza i akcje. 

| Wyzwalaczy | Akcje|
| --- | --- |
| <ul><li>Po nowy wpis na osi czasu</li></ul> |<ul><li>Uzyskiwanie kanału informacyjnego z osi czasu</li><li>Publikowanie dla osi czasu</li><li>Po nowy wpis na osi czasu</li><li>Strona kanału</li><li>Uzyskiwanie użytkownika osi czasu</li><li>Publikowanie do strony</li></ul>

Wszystkie łączniki obsługuje danych w formatach XML i JSON.

## <a name="create-a-connection-to-facebook"></a>Tworzenie połączenia z serwisem Facebook
Po dodaniu tego łącznika do aplikacji logiki musi zezwolić logiki aplikacje, aby nawiązać połączenie z serwisem Facebook.

1. Zaloguj się do konta w serwisie Facebook
2. Wybierz pozycję **Autoryzuj**i zezwolić aplikacji logiki do łączenia i za pomocą konta w serwisie Facebook. 

>[AZURE.INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]

>[AZURE.TIP] Za pomocą tego samego połączenie z serwisem Facebook w innych aplikacjach logicznych.

## <a name="swagger-rest-api-reference"></a>Swagger odwołanie interfejsu API usługi REST
Dotyczy wersji: 1.0.

### <a name="get-feed-from-my-timeline"></a>Uzyskiwanie strumieniowego z osi czasu
Otrzymuje źródła danych z osi czasu zalogowany użytkownik.  
```GET: /me/feed```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|pola|ciąg|Brak|kwerendy|Brak |Określ pola, które mają być zwracane. Przykład (identyfikator, nazwa obrazu).|
|limit|Liczba całkowita|Brak|kwerendy| Brak|Maksymalna liczba wpisów, które mają być pobierane|
|z|ciąg|Brak|kwerendy| Brak|Ograniczanie listy wpisów do tylko te z lokalizacją dołączony.|
|Filtr|ciąg|Brak|kwerendy| Brak|Pobieranie tylko wpisy zgodne filtru określonego strumienia.|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


### <a name="post-to-my-timeline"></a>Publikowanie dla osi czasu
Wpis komunikatu o stanie do osi czasu zalogowany użytkownik.  
```POST: /me/feed```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|wpis|ciąg |tak|Treść|Brak |Nowa wiadomość, która zostanie umieszczona|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


### <a name="when-there-is-a-new-post-on-my-timeline"></a>Po nowy wpis na osi czasu
Uaktywnia nowy przepływ jest nowy wpis na osi czasu zalogowany użytkownik.  
```GET: /trigger/me/feed```

Nie ma żadnych parametrów. 

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


### <a name="get-page-feed"></a>Strona kanału
Pobieranie wpisy z kanału informacyjnego określoną stronę.  
```GET: /{pageId}/feed```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator pageId|ciąg|tak|Ścieżka| Brak|Identyfikator strony, z której wpisy znajduje się mają być pobierane.|
|limit|Liczba całkowita|Brak|kwerendy| Brak|Maksymalna liczba wpisów, które mają być pobierane|
|include_hidden|wartość logiczna|Brak|kwerendy|Brak |Czy chcesz uwzględnić wszystkie wpisy, które zostały ukryte przez stronę|
|pola|ciąg|Brak|kwerendy|Brak |Określ pola, które mają być zwracane. Przykład (identyfikator, nazwa obrazu).|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


### <a name="get-user-timeline"></a>Uzyskiwanie użytkownika osi czasu
Uzyskaj wpisy z osi czasu użytkownika.  
```GET: /{userId}/feed```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Nazwa użytkownika|ciąg|tak|Ścieżka|Brak |Identyfikator użytkownika, którego osi czasu muszą być pobierane.|
|limit|Liczba całkowita|Brak|kwerendy|Brak |Maksymalna liczba wpisów, które mają być pobierane|
|z|ciąg|Brak|kwerendy|Brak |Ograniczanie listy wpisów do tylko te z lokalizacją dołączony.|
|Filtr|ciąg|Brak|kwerendy| Brak|Pobieranie tylko wpisy zgodne filtru określonego strumienia.|
|pola|ciąg|Brak|kwerendy| Brak|Określ pola, które mają być zwracane. Przykład (identyfikator, nazwa obrazu).|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


### <a name="post-to-page"></a>Publikowanie do strony
Publikowanie wiadomości do strony serwisu Facebook jako zalogowany użytkownik.  
```POST: /{pageId}/feed```

| Nazwa|Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|Identyfikator pageId|ciąg|tak|Ścieżka|Brak |Identyfikator strony, aby opublikować.|
|wpis|wiele |tak|Treść|Brak |Nowa wiadomość, która zostanie umieszczona.|

#### <a name="response"></a>Odpowiedź
|Nazwa|Opis|
|---|---|
|200|Ok|
|400|Nieprawidłowe żądanie|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="object-definitions"></a>Definicje obiektów

#### <a name="getfeedresponse"></a>GetFeedResponse

|Nazwa właściwości | Typ danych | Wymagane|
|---|---|---|
|dane|Tablica|Brak|

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|dane|Tablica|Brak|

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem: Pojedynczy wpis w profilu użytkownika kanału
Profil może być użytkownika, strony, aplikacji lub grupy. 

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Identyfikator|ciąg|Brak|
|admin_creator|Tablica|Brak|
|Podpis|ciąg|Brak|
|created_time|ciąg|Brak|
|Opis|ciąg|Brak|
|feed_targeting|nie zdefiniowano|Brak|
|z|nie zdefiniowano|Brak|
|Ikona|ciąg|Brak|
|is_hidden|wartość logiczna|Brak|
|is_published|wartość logiczna|Brak|
|łącze|ciąg|Brak|
|Komunikat|ciąg|Brak|
|Nazwa|ciąg|Brak|
|OBJECT_ID|ciąg|Brak|
|Obraz|ciąg|Brak|
|Umieść|nie zdefiniowano|Brak|
|prywatność|nie zdefiniowano|Brak|
|właściwości|Tablica|Brak|
|źródła|ciąg|Brak|
|status_type|ciąg|Brak|
|Artykuł|ciąg|Brak|
|Określanie wartości docelowej|nie zdefiniowano|Brak|
|Aby|Tablica|Brak|
|Typ|ciąg|Brak|
|updated_time|ciąg|Brak|
|with_tags|nie zdefiniowano|Brak|

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem: Pojedynczy wpis w profilu użytkownika kanału
Profil może być użytkownika, strony, aplikacji lub grupy.

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Identyfikator|ciąg|Brak|
|created_time|ciąg|Brak|
|z|nie zdefiniowano|Brak|
|Komunikat|ciąg|Brak|
|Typ|ciąg|Brak|

#### <a name="adminitem"></a>AdminItem

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Identyfikator|ciąg|Brak|
|łącze|ciąg|Brak|

#### <a name="propertyitem"></a>PropertyItem

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Nazwa|ciąg|Brak|
|tekst|ciąg|Brak|
|Odwołanie|ciąg|Brak|

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Komunikat|ciąg|tak|
|łącze|ciąg|Brak|
|Obraz|ciąg|Brak|
|Nazwa|ciąg|Brak|
|Podpis|ciąg|Brak|
|Opis|ciąg|Brak|
|Umieść|ciąg|Brak|
|znaczniki|ciąg|Brak|
|prywatność|nie zdefiniowano|Brak|
|object_attachment|ciąg|Brak|

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Komunikat|ciąg|tak|
|łącze|ciąg|Brak|
|Obraz|ciąg|Brak|
|Nazwa|ciąg|Brak|
|Podpis|ciąg|Brak|
|Opis|ciąg|Brak|
|Akcje|Tablica|Brak|
|Umieść|ciąg|Brak|
|znaczniki|ciąg|Brak|
|object_attachment|ciąg|Brak|
|Określanie wartości docelowej|nie zdefiniowano|Brak|
|feed_targeting|nie zdefiniowano|Brak|
|opublikowany|wartość logiczna|Brak|
|scheduled_publish_time|ciąg|Brak|
|backdated_time|ciąg|Brak|
|backdated_time_granularity|ciąg|Brak|
|child_attachments|Tablica|Brak|
|multi_share_end_card|wartość logiczna|Brak|

#### <a name="postfeedresponse"></a>PostFeedResponse

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Identyfikator|ciąg|Brak|

#### <a name="profilecollection"></a>ProfileCollection

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|dane|Tablica|Brak|

#### <a name="useritem"></a>UserItem

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Identyfikator|ciąg|Brak|
|Imię|ciąg|Brak|
|nazwisko|ciąg|Brak|
|Nazwa|ciąg|Brak|
|płeć|ciąg|Brak|
|informacje o|ciąg|Brak|

#### <a name="actionitem"></a>ActionItem

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Nazwa|ciąg|Brak|
|łącze|ciąg|Brak|

#### <a name="targetitem"></a>TargetItem

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|kraje|Tablica|Brak|
|Ustawienia regionalne|Tablica|Brak|
|obszary|Tablica|Brak|
|miasta|Tablica|Brak|

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem: Kierowanie dla tego wpisu kanału obiekt, który steruje wiadomości
Każda osoba w tym grupom zobaczyć ten wpis w większości przypadków, inne są mniejsze. Dotyczy tylko do stron.

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|kraje|Tablica|Brak|
|obszary|Tablica|Brak|
|miasta|Tablica|Brak|
|age_min|Liczba całkowita|Brak|
|age_max|Liczba całkowita|Brak|
|określoną płeć|Tablica|Brak|
|relationship_statuses|Tablica|Brak|
|interested_in|Tablica|Brak|
|college_years|Tablica|Brak|
|zainteresowania|Tablica|Brak|
|relevant_until|Liczba całkowita|Brak|
|education_statuses|Tablica|Brak|
|Ustawienia regionalne|Tablica|Brak|

#### <a name="placeitem"></a>PlaceItem

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Identyfikator|ciąg|Brak|
|Nazwa|ciąg|Brak|
|overall_rating|Liczba|Brak|
|Lokalizacja|nie zdefiniowano|Brak|

#### <a name="locationitem"></a>LocationItem

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Miasto|ciąg|Brak|
|kraj|ciąg|Brak|
|szerokości|Liczba|Brak|
|located_in|ciąg|Brak|
|długość geograficzna|Liczba|Brak|
|Nazwa|ciąg|Brak|
|region|ciąg|Brak|
|Województwo|ciąg|Brak|
|Ulica|ciąg|Brak|
|ZIP|ciąg|Brak|

#### <a name="privacyitem"></a>PrivacyItem

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Opis|ciąg|Brak|
|wartość|ciąg|tak|
|Zezwalaj na|ciąg|Brak|
|odmawianie|ciąg|Brak|
|znajomi|ciąg|Brak|

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|łącze|ciąg|Brak|
|Obraz|ciąg|Brak|
|image_hash|ciąg|Brak|
|Nazwa|ciąg|Brak|
|Opis|ciąg|Brak|

#### <a name="postphotorequest"></a>PostPhotoRequest

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|adres URL|ciąg|tak|
|Podpis|ciąg|Brak|

#### <a name="postphotoresponse"></a>PostPhotoResponse

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Identyfikator|ciąg|tak|
|post_id|ciąg|tak|

#### <a name="postvideorequest"></a>PostVideoRequest

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|videoData|ciąg|tak|
|Opis|ciąg|tak|
|Tytuł|ciąg|tak|
|uploadedVideoName|ciąg|Brak|

#### <a name="getphotoresponse"></a>GetPhotoResponse

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|dane|nie zdefiniowano|tak|

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|adres URL|ciąg|tak|
|is_silhouette|wartość logiczna|tak|
|wysokość|ciąg|Brak|
|Szerokość|ciąg|Brak|

#### <a name="geteventresponse"></a>GetEventResponse

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|dane|Tablica|tak|

#### <a name="geteventresponseitem"></a>GetEventResponseItem

|Nazwa właściwości | Typ danych |Wymagane|
|---|---|---|
|Identyfikator|ciąg|tak|
|Nazwa|ciąg|tak|
|godzina_rozpoczęcia|ciąg|Brak|
|end_time|ciąg|Brak|
|Strefa czasowa|ciąg|Brak|
|Lokalizacja|ciąg|Brak|
|Opis|ciąg|Brak|
|ticket_uri|ciąg|Brak|
|rsvp_status|ciąg|tak|


## <a name="next-steps"></a>Następne kroki

[Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).
