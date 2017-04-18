<properties 
    pageTitle="Omówienie interfejsu API usługi REST usługi multimediów | Microsoft Azure" 
    description="Omówienie interfejsu API usługi REST usługi multimediów" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako"/>


# <a name="media-services-rest-api-overview"></a>Omówienie interfejsu API usługi REST usługi multimediów 

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Usługi multimediów Azure Microsoft to usługa, która akceptuje żądania HTTP oparte na OData i można odpowiedź w pełne JSON lub atom + pub. Ponieważ spełnia usługi multimediów Azure wskazówek, istnieje zestaw wymagane nagłówki HTTP, które każdego klienta musi zostać użyte podczas łączenia się z usługami multimedia, a także zestaw opcjonalne nagłówków, które mogą być używane. W poniższych sekcjach opisano nagłówki i zleceń HTTP można używać podczas tworzenia żądania i otrzymujesz odpowiedzi od usługi multimediów.

##<a name="considerations"></a>Zagadnienia dotyczące 

Następujące kwestie podczas korzystania z pozostałych.

- Podczas wysyłania kwerend jednostki, istnieje ograniczenie 1000 jednostek zwracane w tym samym czasie, ponieważ publicznej pozostałych wersji 2 ogranicza wyniki kwerendy do 1000 wyników. Należy użyć **Pomiń** i **wykonać** (.NET) / **góry** (REST) jako opisanych w [tym przykładzie .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) i [w tym przykładzie interfejsu API usługi REST](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 

- Po użyciu JSON i określając Aby użyć słowa kluczowego **__metadata** w wezwaniu na (na przykład, aby odwołuje się do obiektu połączonego) należy ustawić nagłówka **Accept** do [formatu JSON pełne](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (Zobacz przykład poniżej). OData nie rozpoznaje właściwości **__metadata** w wezwaniu, chyba że zostanie ustawiony na pełne.  

        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
        
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 
        

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standardowych nagłówków żądania HTTP obsługiwane przez usługi multimediów

Dla każdego połączenia, wprowadzone do usługi multimediów ustawiono wymagane nagłówki, które należy uwzględnić w wezwaniu na, a także zestaw nagłówki opcjonalne może chcesz uwzględnić. W poniższej tabeli wymieniono wymagane nagłówki:


Nagłówek|Typ|Wartość
---|---|---
Autoryzacja|Okaziciela|Okaziciela jest mechanizm tylko autoryzacji zaakceptowane. Wartość musi również zawierać token dostępu dostarczony przez ACS.
x ms wersji|Dziesiętna|2.11
DataServiceVersion|Dziesiętna|3.0
MaxDataServiceVersion|Dziesiętna|3.0



>[AZURE.NOTE] Ponieważ usługi multimediów używa OData udostępniania źródłowych repozytorium metadanych zawartości za pośrednictwem interfejsów API pozostałych, nagłówków DataServiceVersion i MaxDataServiceVersion powinien zostać uwzględniony w przypadku każdego wezwania; Jednak jeśli nie są one, następnie obecnie usługi multimediów założono, że wartość DataServiceVersion używany jest 3.0.

Oto zbiór nagłówki opcjonalne:

Nagłówek|Typ|Wartość
---|---|---
Data|Data RFC 1123|Sygnatura czasowa żądania
Zaakceptuj|Typ zawartości|Żądany typ zawartości na odpowiedź, takie jak następujące:<p> -aplikacji i json; odata = pełne<p> -aplikacji i atom + xml<p> Odpowiedzi może być inny typ zawartości, na przykład pobierania obiektów blob, gdzie pomyślną odpowiedź będzie zawierać strumienia obiektów blob jako ładunek.
Zaakceptuj kodowanie|Gzip, korygowania|GZIP i DEFLATE kodowanie, gdy jest to właściwe. Uwaga: W przypadku dużych zasobów usługi multimediów może zignorować ten nagłówek i zwracana nieskompresowanych danych.
Zaakceptuj języka|"en", "es" i tak dalej.|Określa preferowany język na potrzeby odpowiedzi.
Akceptowanie znaków|Typ znaków, taki jak "UTF-8"|Domyślnie jest UTF-8.
Metoda X-HTTP|Metoda HTTP|Umożliwia klientów lub zapory, które nie obsługują protokołu HTTP metod, takich jak położenie lub Usuń, aby użyć tych metod, tunelowane za pośrednictwem połączenia GET.
Typ zawartości|Typ zawartości|Żądania treści żądania położenie lub wpisu dla typu zawartości.
Identyfikator klienta żądanie|Ciąg|Wartości zdefiniowane rozmówcy identyfikuje danego żądania. Jeśli określony, ta wartość uwzględniane w komunikacie odpowiedzi jako sposobu mapowania żądania. <p><p>**Ważne**<p>Wartości powinny być zamykane w 2096b (2k).

## <a name="standard-http-response-headers-supported-by-media-services"></a>Standardowych nagłówków odpowiedzi HTTP obsługiwane przez usługi multimediów

Poniżej przedstawiono ustawianie nagłówków, które mogą zostać zwrócone do Ciebie w zależności od zasobów, które zostały żąda i akcję, którą ma spełniać.


Nagłówek|Typ|Wartość
---|---|---
Identyfikator żądania|Ciąg|Unikatowy identyfikator dla bieżącej operacji usługi wygenerowane.
Identyfikator klienta żądanie|Ciąg|Identyfikator określony przez rozmówcy w oryginalnym wezwaniu, jeśli jest widoczny.
Data|Daty RFC 1123|Data została przetworzona wezwanie.
Typ zawartości|Zmienia się|Typ zawartości treść odpowiedzi.
Kodowanie zawartości|Zmienia się|Gzip lub korygowania, zależnie od potrzeb.


## <a name="standard-http-verbs-supported-by-media-services"></a>Standardowe zlecenia HTTP obsługiwane przez usługi multimediów

Poniżej przedstawiono pełną listę zleceń HTTP, które można używać podczas tworzenia HTTP żądania:


Czasownikowe|Opis
---|---
Pobierz|Zwraca wartość bieżącą obiektu.
WPIS|Tworzy obiekt na podstawie danych przedstawionych lub przesyła polecenia.
UMIESZCZENIE|Zastępuje obiektu lub tworzy nazwany obiekt (w stosownych przypadkach).
USUWANIE|Usuwa obiekt.
SCALANIE|Aktualizuje zmian nazwanych właściwości istniejącego obiektu.
SZEF|Zwraca metadanych obiektu na odpowiedź GET.

##<a name="limitation"></a>Ograniczenia

Podczas wysyłania kwerend jednostki, istnieje ograniczenie 1000 jednostek zwracane w tym samym czasie, ponieważ publicznej pozostałych wersji 2 ogranicza wyniki kwerendy do 1000 wyników. Należy użyć **Pomiń** i **wykonać** (.NET) / **góry** (REST) jako opisanych w [tym przykładzie .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) i [w tym przykładzie interfejsu API usługi REST](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 


## <a name="discovering-media-services-model"></a>Odkrywanie modelu usługi multimediów

Aby wprowadzić więcej odnajdowania obiektów usługi multimediów, można używać operacji $metadata. Pozwala go pobrać wszystkie typy prawidłowej jednostki, właściwości obiektu, skojarzenia, funkcje, akcje i tak dalej. W poniższym przykładzie pokazano sposób tworzenia identyfikatora URI: https://media.windows.net/API/$ metadanych.

Należy dołączyć "? version=2.x interfejsu api" na końcu URI, aby wyświetlić metadane w przeglądarce, czy nie powinno obejmować nagłówku x-ms wersja żądania.



##<a name="media-services-learning-paths"></a>Ścieżek nauki usługi multimediów

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Przekazywanie opinii

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





 
