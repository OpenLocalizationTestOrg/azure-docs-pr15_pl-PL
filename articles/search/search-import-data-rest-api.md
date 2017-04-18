<properties
    pageTitle="Przekazywanie danych w wyszukiwaniu Azure za pomocą interfejsu API usługi REST | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Dowiedz się, jak przekazać danych do indeksu wyszukiwania Azure za pomocą interfejsu API usługi REST."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search-using-the-rest-api"></a>Przekazywanie danych do wyszukiwania Azure za pomocą interfejsu API usługi REST
> [AZURE.SELECTOR]
- [Omówienie](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [POZOSTAŁE](search-import-data-rest-api.md)

W tym artykule procedurach pokazano, jak za pomocą [Interfejsu API usługi REST wyszukiwania Azure](https://msdn.microsoft.com/library/azure/dn798935.aspx) importowanie danych do indeksu wyszukiwania Azure.

Przed rozpoczęciem tego instruktażu, czy masz już [utworzony indeks wyszukiwania Azure](search-what-is-an-index.md).

Aby przekazać dokumenty do indeksu przy użyciu interfejsu API usługi REST, żądania HTTP POST będzie wydać końcowy adres URL z indeksu. Treści żądania HTTP jest obiektem JSON zawierającej dokumenty, które można dodać, modyfikować lub usunąć.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>I. Identyfikowanie klucz interfejsu api usługi Azure wyszukiwania administratora
Podczas wydawania żądania HTTP do usługi przy użyciu interfejsu API usługi REST, *każdego* interfejsu API żądania muszą zawierać klucz interfejsu api wygenerowany przez usługę wyszukiwania, których możesz obsługi administracyjnej. Masz prawidłowego klucza określa zaufania na zasadzie na żądanie między aplikacji wysłanie żądania oraz usługi, która obsługuje go.

1. Aby znaleźć klawisze interfejsu api tej usługi musisz zalogować do [Azure Portal](https://portal.azure.com/)
2. Przejdź do pozycji Karta usługi Azure wyszukiwania
3. Kliknij ikonę "Klawisze"

Twoja usługa uzyskuje *kluczach administratora* i *kwerendy*.

  - Podstawowy i pomocniczy *klawiszy administrator* udzielanie pełne prawa do wszystkich działań, łącznie z możliwością zarządzania usługą, tworzenie i usuwanie indeksy, indeksatory i źródeł danych. Istnieją dwa klucze, tak aby możesz nadal korzystać z kluczem pomocniczym, jeśli zechcesz ponownie wygenerować klucz podstawowy i odwrotnie.
  - *Klucze kwerendy* udzielić dostępu tylko do odczytu do indeksy i dokumenty, a zwykle są rozdzielane aplikacje klienckie, które problemu żądania wyszukiwania.

Na potrzeby importowania danych do indeksu, można użyć jednej klucz podstawowy i pomocniczy administratora.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Zdecyduj, które indeksowania akcję do użycia
Korzystając z interfejsu API usługi REST, będzie wydawać żądania HTTP POST z JSON żądanie organów do indeksu wyszukiwania Azure adres URL punktu końcowego. Obiekt JSON w swojej żądania HTTP będzie zawierać pojedynczą tablicę JSON o nazwie "wartość" zawierającej obiekty JSON reprezentujące dokumenty, które chcesz dodać do indeksu, aktualizacji lub usunięcia.

Każdy obiekt JSON w tablicy "wartość" oznacza dokumentu, który może być indeksowane. Każdy z tych obiektów zawiera klucz dokumentu i określić żądaną akcję indeksowania (przekazywania, korespondencji seryjnej, Usuń itp.). W zależności od tego, które z poniżej akcji wybierzesz, tylko dla niektórych pól musi być uwzględniony w każdym dokumencie:

@search.action | Opis | Potrzebne pola dla każdego dokumentu | Notatki
--- | --- | --- | ---
`upload` | `upload` Akcji jest podobne do "upsert", miejsce, w którym wstawiony, jeśli jest nowe i zaktualizowane i zastąpić Jeśli istnieje dokument. | klawisz plus inne pola, które chcesz zdefiniować | W przypadku aktualizowania i zamienić istniejący dokument, dowolnego pola, które nie jest określone w wezwaniu będzie jego wartości pola `null`. Dzieje się tak, nawet wtedy, gdy pole wcześniej została ustawiona na wartość inną niż null.
`merge` | Aktualizuje istniejącego dokumentu określone pola. Jeśli dokument nie istnieje w indeksie, korespondencji seryjnej nie powiedzie się. | klawisz plus inne pola, które chcesz zdefiniować | Wszystkie pola wybrane w podczas tworzenia korespondencji seryjnej zastąpią istniejące pole w dokumencie. Ta opcja uwzględnia pól typu `Collection(Edm.String)`. Na przykład, jeśli dokument zawiera pole `tags` z wartością `["budget"]` i wykonać podczas tworzenia korespondencji seryjnej z wartością `["economy", "pool"]` dla `tags`, końcowej `tags` pole będzie `["economy", "pool"]`. Nie będzie `["budget", "economy", "pool"]`.
`mergeOrUpload` | Ta akcja zachowuje się jak `merge` Jeśli dokumentu zawierającego dany klucz już istnieje w indeksie. Jeśli dokument nie istnieje, zachowuje się jak `upload` z nowym dokumentem. | klawisz plus inne pola, które chcesz zdefiniować | -
`delete` | Usuwa określonego dokumentu z indeksu. | tylko klucz | Wszystkie pola określ inne niż pole klucza będą ignorowane. Aby usunąć pojedyncze pole z dokumentu, należy użyć `merge` zamiast tego i po prostu ustawić pole jawnie na wartość null.

## <a name="iii-construct-your-http-request-and-request-body"></a>III. Konstruowanie żądania HTTP i treści żądania
Teraz, gdy zostaną zebrane wartości pól niezbędne dla czynności indeks, możesz przystąpić do utworzenia żądania HTTP i treści żądania JSON do zaimportowania danych.

#### <a name="request-and-request-headers"></a>Żądanie i nagłówków żądania
W adresie URL, należy podać nazwę usługi, nazwa indeksu ("hotele" w tym przypadku), a także odpowiedniej wersji interfejsu API (bieżąca wersja interfejsu API jest `2015-02-28` w momencie publikowania tego dokumentu). Należy zdefiniować `Content-Type` i `api-key` żądanie nagłówków. Dla niego użyj jednej z klawiszami administratora tej usługi.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Treść żądania

```JSON
{
    "value": [
        {
            "@search.action": "upload",
            "hotelId": "1",
            "baseRate": 199.0,
            "description": "Best hotel in town",
            "description_fr": "Meilleur hôtel en ville",
            "hotelName": "Fancy Stay",
            "category": "Luxury",
            "tags": ["pool", "view", "wifi", "concierge"],
            "parkingIncluded": false,
            "smokingAllowed": false,
            "lastRenovationDate": "2010-06-27T00:00:00Z",
            "rating": 5,
            "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
            "@search.action": "upload",
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags": ["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

W tym przypadku użyto `upload`, `mergeOrUpload`, i `delete` jako działania wyszukiwania.

Przyjęto założenie, że ten przykład "hotele" indeks jest już wypełniona liczbę dokumentów. Należy zauważyć, jak firma Microsoft nie musieli określić wszystkie pola możliwe dokumentu podczas korzystania z `mergeOrUpload` i jak możemy tylko określony klucz dokumentu (`hotelId`) podczas korzystania z `delete`.

Należy również zauważyć, że tylko może zawierać najwyżej 1000 dokumenty (lub 16 MB) w pojedynczej żądaniu indeksowania.

## <a name="iv-understand-your-http-response-code"></a>IV. Opis kodu odpowiedzi HTTP
#### <a name="200"></a>200
Po przesłaniu żądania pomyślnego indeksowania otrzymasz odpowiedź HTTP z kodem stanu `200 OK`. Treści JSON odpowiedzi HTTP będzie następujący:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Kod stanu `207` jest zwracana, gdy co najmniej jeden element nie został pomyślnie zindeksowane. Treści JSON odpowiedzi HTTP będzie zawierać informacje o dokumentach powiodła się.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [AZURE.NOTE] To często oznacza to, że obciążenie usługi wyszukiwania jest osiągnięcie punktu miejsce, w którym indeksowania żądania rozpocznie zwraca `503` odpowiedzi. W tym przypadku zdecydowanie zaleca który kod klienta ponownie wyłączyć i oczekiwania ponawianie próby. Da systemu trochę czasu, aby odzyskać, zwiększyć szanse, że przyszłe żądania powiedzie. Szybko ponawianie próby wezwaniach tylko przedłużenie sytuacji.

#### <a name="429"></a>429
Kod stanu `429` zostaną zwrócone przekroczenia przydziału na Liczba dokumentów na indeks.

#### <a name="503"></a>503
Kod stanu `503` zostaną zwrócone, jeśli żaden z elementów w wezwaniu zostały pomyślnie zindeksowane. Ten błąd oznacza, że system jest obciążony i w tej chwili nie może przetworzyć żądania.

> [AZURE.NOTE] W tym przypadku zdecydowanie zaleca który kod klienta ponownie wyłączyć i oczekiwania ponawianie próby. Da systemu trochę czasu, aby odzyskać, zwiększyć szanse, że przyszłe żądania powiedzie. Szybko ponawianie próby wezwaniach tylko przedłużenie sytuacji.

Aby uzyskać więcej informacji o akcje dokumentu i odpowiedzi sukcesu i błędu zobacz [Dodawanie, aktualizowanie lub usuwanie dokumentów](https://msdn.microsoft.com/library/azure/dn798930.aspx). Aby uzyskać więcej informacji na inne kody stanu HTTP, które mogą zostać zwrócone w przypadku awarii zobacz [kody stanu HTTP (wyszukiwanie Azure)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

## <a name="next"></a>Następny
Po wypełnieniu indeksu wyszukiwania Azure, można rozpocząć wydawania zapytania, aby wyszukać dokumenty. Aby uzyskać szczegółowe informacje, zobacz [Kwerendy i indeksu wyszukiwania Azure](search-query-overview.md) .
