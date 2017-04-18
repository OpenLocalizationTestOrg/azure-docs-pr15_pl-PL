<properties 
    pageTitle="Odwołanie do zasad zarządzania API platformy Azure" 
    description="Informacje na temat zasad można konfigurować interfejsu API zarządzania." 
    services="api-management" 
    documentationCenter="" 
    authors="vladvino" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="apimpm"/>

# <a name="azure-api-management-policy-reference"></a>Odwołanie do zasad zarządzania API platformy Azure

Ta sekcja zawiera indeksu dla zasad [odwołanie do zasad zarządzania interfejsu API][]. Aby uzyskać informacje dotyczące dodawania i konfigurowanie zasad zobacz [zasady zarządzania interfejsu API w][].

Zasady wyrażeń może służyć jako wartości atrybutu lub tekstu w każdym z zasad zarządzania interfejsu API, chyba że zasady określi inaczej. Pewne zasady, takie jak zasady [Przepływ sterowania][] i [Ustaw zmienną][] są oparte na wyrażeniach zasad. Aby uzyskać więcej informacji zobacz [Zaawansowane zasady][] i [wyrażeń zasad][]

## <a name="policy-reference-index"></a>Zasady odwołanie indeksu

-   [Zasady ograniczenia dostępu][]
    -   [Nagłówek HTTP sprawdzanie][] — wymusza istnienia i/lub wartość nagłówka HTTP.
    -   [Ogranicz szybkość połączenia przez subskrypcji][] - użycie interfejsu API zapobiega wzrósł, ograniczając szybkość połączenia, w oparciu o na subskrypcje.
    -   [Ogranicz szybkość połączenia przez klucz](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) — użycie interfejsu API zapobiega wzrósł, ograniczając szybkość połączenia, na podstawie jednego klucza.
    -   [Ogranicz rozmówcy adresy IP][] - filtry (umożliwia-powoduje zablokowanie) połączenia z określonych adresów IP i/lub zakresy adresów.
    -   [Ustawianie przydział użycia przez subskrypcję][] — umożliwia wymuszanie odnawialnych lub długotrwałe połączenia głośności i/lub przepustowości przydziału na podstawie subskrypcji na.
    -   [Ustawianie przydział użycia klawisza](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) — umożliwia wymuszanie odnawialnych lub długotrwałe połączenia głośności i/lub przepustowości przydziału na podstawie klucza na.
    -   [Sprawdź poprawność JWT][] - wymusza istnienia i ważności JWT wyodrębnionych z określonego nagłówka HTTP lub parametr kwerendy.
-   [Zasady zaawansowane][]
    -   [Przepływ sterowania][] — dotyczy warunkowo instrukcji zasad na podstawie wyników oceny [wyrażeń][]logicznych.
    -   [Przesyłanie dalej wezwania][] — przesyła żądanie usługi wewnętrznej bazy danych.
    -   [Dziennik zdarzeń koncentratora][] - wysyła wiadomości w określonym formacie do elementu docelowego wiadomości zdefiniowane przez osobę [rejestratora](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) .
    -   [Ponów próbę](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - ponowne próby wykonywanie instrukcji zasad zamkniętych Jeśli i do momentu spełnienia warunku. Wykonanie Powtórz w określonych odstępach czasu, a także do określonej liczba prób.
    -   [Odpowiedź zwrócona](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - przerwanie potok realizacji i zwraca określoną odpowiedź bezpośrednio do rozmówcy.
    -   [Wysyłanie wezwania na jednym ze sposobów](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - wysyła żądanie do określonego adresu URL bez oczekiwanie na odpowiedź.
    -   [Wysyłanie wezwania](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) - wysyła żądanie do określonego adresu URL.
    -   [Metoda żądania set](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - pozwala zmienić metodę HTTP na żądanie.
    -   [Ustaw stan](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) — zmienia kod stanu HTTP do określonej wartości.
    -   [Ustaw zmienną][] - utrzymują wartości w zmiennej nazwanych [kontekstu][] dostępu później.
    -   [Śledzenie](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) — dodaje ciąg w wyniku [Inspektor interfejsu API](../api-management/api-management-howto-api-inspector.md) .
    -   [Poczekaj](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - oczekuje załączony Wyślij żądanie, Pobierz wartość z pamięci podręcznej lub zasady przepływu sterowania do wykonania przed kontynuowaniem.
-   [Zasady uwierzytelniania][]
    -   [Uwierzytelniony przez Basic][] — uwierzytelniania za pomocą usługi wewnętrznej bazy danych przy użyciu uwierzytelniania podstawowego.
    -   [Uwierzytelniania z certyfikatem klienta][] - uwierzytelniania za pomocą usługi wewnętrznej bazy danych przy użyciu certyfikatów klienta.
-   [Zasady przechowywania][] 
    -   [Pobieranie z pamięci podręcznej][] - wykonywanie pamięci podręcznej wyszukiwania i zwracana nieprawidłowa odpowiedź buforowana, gdy są dostępne.
    -   [Magazyn pamięci podręcznej][] - odpowiedzi pamięci podręcznej według określonej pamięci podręcznej konfiguracji kontroli.
    -   [Uzyskaj wartość z pamięci podręcznej](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) — Pobierz elementu pamięci podręcznej przez klucz.
    -   [Wartość magazynu w pamięci podręcznej](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - sklepu element w pamięci podręcznej przez klucza.
    -   [Usuwanie wartości z pamięci podręcznej](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - Usuń element w pamięci podręcznej przez klucza.
-   [Krzyżowe zasady domeny][] 
    -   [Zezwalanie na połączenia między domenami][] — sprawia, że API jest dostępna z klientów programu Adobe Flash i Microsoft Silverlight przeglądarki.
    -   [CORS][] — powoduje dodanie zasobów między origin udostępniania pomocy technicznej (CORS), aby operacji lub interfejsu API, aby zezwolić połączeń między domenami z przeglądarki klientów.
    -   [JSONP][] - dodaje JSON z obsługą operacji dopełnienie (JSONP) lub interfejs API, aby zezwolić połączeń między domenami z klientów zgodny z przeglądarką JavaScript.
-   [Transformacja zasad][] 
    -   [Konwertowanie JSON do formatu XML][] — konwertuje żądania lub odpowiedzi treści z JSON do formatu XML.
    -   [Konwertowanie XML do formatu JSON][] - konwertuje żądania lub odpowiedzi treści z pliku XML do formatu JSON.
    -   [Znajdowanie i zamienianie ciąg w treści][] — umożliwia znalezienie wyrazów ciąg żądania lub odpowiedzi i zamienia ciąg różnych.
    -   [Adresy URL maski zawartości][] — ponownie zapisuje (maski) łączy w odpowiedzi podstawowy tak, aby wskazywały do odpowiedniej łącza, za pomocą bramy.
    -   [Skonfiguruj usługę wewnętrznej bazy danych][] — zmienia obsługi wewnętrznej bazy danych dla przychodzącego żądania.
    -   [Ustawianie treści][] - ustawia treść wiadomości przychodzące i wychodzące żądania.
    -   [Nagłówek HTTP Ustawianie][] — przypisuje wartość istniejącą odpowiedź i/lub nagłówka żądania lub dodaje nowy nagłówek odpowiedzi i/lub wezwanie.
    -   [Ustaw parametr ciągu kwerendy][] — powoduje dodanie, zastępuje wartości lub usuwania żądanie parametr ciągu kwerendy.
    -   [Edycji adresu URL][] — konwertuje adresu URL żądania z postaci publicznej na formularzu poprawnie przez usługę sieci web.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o wyrażeniach zasad Zobacz poniższym klipie wideo.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Zasady ograniczenia dostępu]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Zaznacz nagłówek HTTP]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Ogranicz szybkość połączenia przez subskrypcji]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Ograniczanie rozmówcy adresy IP]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Przydział użycia zestawu przez subskrypcji]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Sprawdź poprawność JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Zasady zaawansowane]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Przepływ sterowania]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Ustaw zmienną]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[wyrażenia]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[kontekst]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Przesyłanie dalej wezwania]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Dziennik zdarzeń Centrum]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Zasady uwierzytelniania]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Typ poświadczeń uwierzytelniania Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Typ poświadczeń uwierzytelniania certyfikat klienta]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Zasady przechowywania]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Pobieranie z pamięci podręcznej]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Przechowywane w pamięci podręcznej]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Krzyżowe zasady domeny]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Zezwalanie na połączenia między domenami]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformacja zasad]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Konwertowanie JSON XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Konwertowanie XML JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Znajdowanie i zamienianie ciąg w treści]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Adresy URL maski w zawartości]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Ustawianie wewnętrznej bazy danych usługi]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Ustawianie treści]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Ustawianie nagłówka HTTP]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Ustaw parametr ciągu kwerendy]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Zmiana adresu URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Zasady zarządzania interfejsu API w]: api-management-howto-policies.md
[Odwołanie do zasad zarządzania interfejsu API]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Zasady wyrażeń]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
