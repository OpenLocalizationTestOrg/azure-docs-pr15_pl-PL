<properties
pageTitle="Używanie łącznika wideo w usłudze Office 365 w aplikacji logika | Microsoft Azure"
description="Wprowadzenie do korzystania z łącznika wideo w usłudze Office 365 w logiczny aplikacji usługi aplikacji Microsoft Azure"
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

# <a name="get-started-with-the-office365-video-connector"></a>Wprowadzenie do łącznika wideo usługi Office 365
Podłącz do usługi Office 365 wideo, aby uzyskać informacje na temat usługi Office 365 wideo, uzyskać listę klipów wideo i innych. Łącznik wideo w usłudze Office 365 mogą być używane z:

- Aplikacje warunków logicznych 

>[AZURE.NOTE] Tą wersją artykułu dotyczy wersji schematu 2015-08-01-podgląd aplikacji logicznych. Ten łącznik nie jest obsługiwana w żadnej poprzedniej wersji schematu.

Wideo w usłudze Office 365 możesz wykonać następujące czynności:

- Tworzenie usługi przepływu biznesowych na podstawie danych, które otrzymujesz z wideo w usłudze Office 365. 
- Używanie akcji, które sprawdzić stan portalu wideo, Pobierz listę wszystkich wideo w kanale i innych elementów. Te akcje odpowiedzi, a następnie wprowadź dane wyjściowe dostępne dla innych akcji. Na przykład można wyszukiwać klipy wideo w usłudze Office 365 za pomocą łączników wyszukiwania usługi Bing, a następnie użyj łącznika wideo w usłudze Office 365, aby uzyskać informacje na temat tego klipu wideo. Jeśli klip wideo nie spełnia wymagań, możesz publikować w tym klipie wideo w serwisie Facebook. 

Aby dodać operację w aplikacjach logiki, zobacz [Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Wyzwalacze i akcje

Łącznik wideo w usłudze Office 365 ma następujące akcje dostępne. Istnieją wyzwalacze nie.

| Wyzwalaczy | Akcje|
| --- | --- |
| Brak | <ul><li>Sprawdzenie stanu portalu wideo</li><li>Pobierz wszystkie kanały widoczny</li><li>Uzyskiwanie adresu url odtwarzanie manifestu usługi multimediów Azure klipu wideo</li><li>Uzyskiwanie token okaziciela, aby uzyskać dostęp do odszyfrowywanie klipu wideo</li><li>Pobiera informacje o określonej usługi Office 365 wideo</li><li>Wyświetla listę wszystkich obecnych w kanale wideo usługi Office 365</li></ul>

Wszystkie łączniki obsługuje danych w formatach XML i JSON. 

## <a name="create-a-connection-to-office365-video-connector"></a>Tworzenie połączenia z łącznika wideo usługi Office 365
Po dodaniu tego łącznika do aplikacji logika należy Zaloguj się do swojego konta wideo w usłudze Office 365 i zezwolić aplikacji logicznych w celu nawiązywania połączenia z kontem.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Po utworzeniu połączenia wprowadź właściwości wideo usługi Office 365, takie jak nazwa dzierżawy lub kanał identyfikatora. **Odwołanie interfejsu API usługi REST** w tym temacie opisano następujące właściwości.

>[AZURE.TIP] Za pomocą tego samego połączenia wideo w usłudze Office 365 w innych aplikacjach logicznych.

## <a name="swagger-rest-api-reference"></a>Swagger odwołanie interfejsu API usługi REST
Dotyczy wersji: 1.0.

### <a name="checks-video-portal-status"></a>Sprawdzenie stanu portalu wideo 
Sprawdza, czy wideo portalu stan, aby zobaczyć, czy są włączone usługi wideo.  
```GET: /{tenant}/IsEnabled``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|dzierżawy|ciąg|tak|Ścieżka|Brak|Nazwa dzierżawy dla katalogu użytkownika jest częścią|


#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|



### <a name="get-all-viewable-channels"></a>Uzyskiwanie widoczny kanałów 
Pobiera wszystkie kanały, które użytkownik ma dostęp do wyświetlania.  
```GET: /{tenant}/Channels``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|dzierżawy|ciąg|tak|Ścieżka|Brak|Nazwa dzierżawy dla katalogu użytkownika jest częścią|


#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Wyświetla listę wszystkich obecnych w kanale wideo usługi Office 365 
Wyświetla listę wszystkich obecnych w kanale wideo usługi Office 365.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|dzierżawy|ciąg|tak|Ścieżka|Brak|Nazwa dzierżawy dla katalogu użytkownika jest częścią|
|channelId|ciąg|tak|Ścieżka|Brak|Identyfikator kanału, z której klipów wideo potrzebne do pobrania|


#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|




### <a name="gets-information-about-a-particular-office365-video"></a>Pobiera informacje o określonej usługi Office 365 wideo 
Pobiera informacje o określonej usługi Office 365 wideo.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|dzierżawy|ciąg|tak|Ścieżka|Brak|Nazwa dzierżawy dla katalogu użytkownika jest częścią|
|channelId|ciąg|tak|Ścieżka|Brak|Identyfikator kanału|
|videoId|ciąg|tak|Ścieżka|Brak|Identyfikator wideo|


#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Uzyskiwanie adresu url odtwarzanie manifestu usługi multimediów Azure klipu wideo 
Uzyskiwanie adresu url odtwarzanie manifestu usługi multimediów Azure klipu wideo.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|dzierżawy|ciąg|tak|Ścieżka|Brak|Nazwa dzierżawy dla katalogu użytkownika jest częścią|
|channelId|ciąg|tak|Ścieżka|Brak|Identyfikator kanału|
|videoId|ciąg|tak|Ścieżka|Brak|Identyfikator wideo|
|streamingFormatType|ciąg|tak|kwerendy|Brak|Przesyłanie strumieniowe typ formatu. 1 - gładkie Streaming lub MPEG-kreska. 0 - HLS strumieniowego przesyłania|


#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>Uzyskiwanie token okaziciela, aby uzyskać dostęp do odszyfrowywanie klipu wideo 
Uzyskaj token okaziciela, aby uzyskać dostęp do odszyfrowywanie wideo.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Nazwa| Typ danych|Wymagane|Znajduje się w|Wartość domyślna|Opis|
| ---|---|---|---|---|---|
|dzierżawy|ciąg|tak|Ścieżka|Brak|Nazwa dzierżawy dla katalogu użytkownika jest częścią|
|channelId|ciąg|tak|Ścieżka|Brak|Identyfikator kanału|
|videoId|ciąg|tak|Ścieżka|Brak|Identyfikator wideo|


#### <a name="response"></a>Odpowiedź

|Nazwa|Opis|
|---|---|
|200|Operacja zakończyła się pomyślnie|
|400|BadRequest|
|401|Brak autoryzacji|
|404|Nie można odnaleźć|
|500|Wewnętrzny błąd serwera|
|domyślne|Operacja nie powiodła się.|


## <a name="object-definitions"></a>Definicje obiektów

#### <a name="channel-channel-class"></a>Kanał: Kanał klasy

| Nazwa | Typ danych | Wymagane|
|---|---|---|
|Identyfikator|ciąg|Brak|
|Tytuł|ciąg|Brak|
|Opis|ciąg|Brak|


#### <a name="video"></a>Klip wideo 

| Nazwa | Typ danych |Wymagane|
|---|---|---|
|Identyfikator|ciąg|Brak|
|Tytuł|ciąg|Brak|
|Opis|ciąg|Brak|
|CreationDate|ciąg|Brak|
|Właściciel|ciąg|Brak|
|ThumbnailUrl|ciąg|Brak|
|VideoUrl|ciąg|Brak|
|VideoDuration|Liczba całkowita|Brak|
|VideoProcessingStatus|Liczba całkowita|Brak|
|ViewCount|Liczba całkowita|Brak|


## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji logicznych](../app-service-logic/app-service-logic-create-a-logic-app.md).
