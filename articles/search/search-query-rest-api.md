<properties
    pageTitle="Kwerenda indeksu wyszukiwania Azure za pomocą interfejsu API usługi REST | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Konstruowanie kwerendy wyszukiwania w Azure wyszukiwanie i używanie parametrów wyszukiwania do filtrowania i sortowania wyników wyszukiwania."
    services="search"
    documentationCenter=""
    manager="jhubbard"
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Kwerenda indeksu wyszukiwania Azure za pomocą interfejsu API usługi REST
> [AZURE.SELECTOR]
- [Omówienie](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [POZOSTAŁE](search-query-rest-api.md)

W tym artykule zostanie wyświetlona jak wyszukiwać indeksu przy użyciu [Interfejsu API usługi REST wyszukiwania Azure](https://msdn.microsoft.com/library/azure/dn798935.aspx).

Przed rozpoczęciem tego instruktażu, czy masz już [utworzone indeksu wyszukiwania Azure](search-what-is-an-index.md) i [wypełnione go z danymi](search-what-is-data-import.md).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>I. Identyfikowanie klucz interfejsu api usługi Azure wyszukiwania kwerendy
Kluczowego składnika każdej operacji wyszukiwania przed interfejsu API usługi REST wyszukiwania Azure jest *klucz interfejsu api* , który został wygenerowany przez usługę, którą możesz obsługi administracyjnej. Masz prawidłowego klucza określa zaufania na zasadzie na żądanie między aplikacji wysłanie żądania oraz usługi, która obsługuje go.

1. Aby znaleźć klawisze interfejsu api tej usługi musisz zalogować do [Azure Portal](https://portal.azure.com/)
2. Przejdź do pozycji Karta usługi Azure wyszukiwania
3. Kliknij ikonę "Klawisze"

Twoja usługa uzyskuje *kluczach administratora* i *kwerendy*.

 - Podstawowy i pomocniczy *klawiszy administrator* udzielanie pełne prawa do wszystkich działań, łącznie z możliwością zarządzania usługą, tworzenie i usuwanie indeksy, indeksatory i źródeł danych. Istnieją dwa klucze, tak aby możesz nadal korzystać z kluczem pomocniczym, jeśli zechcesz ponownie wygenerować klucz podstawowy i odwrotnie.
 - *Klucze kwerendy* udzielić dostępu tylko do odczytu do indeksy i dokumenty, a zwykle są rozdzielane aplikacje klienckie, które problemu żądania wyszukiwania.

Na potrzeby kwerendy indeksu można użyć jednego z kluczy kwerendy. Klucze Administrator można także w przypadku kwerend, ale należy używać klucza zapytania w kodzie aplikacji, ponieważ wynika to lepiej [zasada przyznawania jak najmniejszych uprawnień](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="ii-formulate-your-query"></a>II. Sformułowania zapytania
Istnieją dwa sposoby [wyszukiwania indeksu przy użyciu interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn798927.aspx). Jednym ze sposobów jest żądanie HTTP POST miejsce, w którym zostaną określone parametry kwerendy w obiekcie JSON w treści wezwania. Innym sposobem jest żądanie HTTP GET miejsce, w którym parametry kwerendy zostanie określone w adresie URL żądania. Należy zauważyć, że WPIS ma więcej [złagodzone limity](https://msdn.microsoft.com/library/azure/dn798927.aspx) rozmiaru parametry kwerendy niż pobieranie. Z tego powodu zalecamy przy użyciu WPIS, jeśli nie masz specyfiki miejsce, w którym przy użyciu GET będzie wygodniejszy.

WPIS i uzyskiwanie należy podać swoją *nazwę usługi*, *Nazwa indeksu*i odpowiedniej *wersji interfejsu API* (bieżąca wersja interfejsu API jest `2015-02-28` w momencie publikowanie tego dokumentu) w adresie URL żądania. POBIERANIE, *ciągu kwerendy* na końcu adresu URL będzie miejsce, w którym podane parametry kwerendy. Poniżej podano format adresu URL:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

Format wpisu jest takie same, ale tylko interfejsu api wersji w parametrów ciągu kwerendy.



#### <a name="example-queries"></a>Przykład kwerendy

Poniżej przedstawiono kilka przykładowych kwerend na indeks o nazwie "hotele". Te kwerendy są wyświetlane w formacie zarówno GET i POST.

Przeszukiwać cały indeks terminu "budżet" i zwrócone tylko `hotelName` pola:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Stosowanie filtru do indeksu, aby znaleźć tańsze niż 150 zł na nocnej hotele i powrócić `hotelId` i `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Przeszukiwać cały indeks kolejności według określonego pola (`lastRenovationDate`) w kolejności malejącej, należy wykonać dwa wyników i Pokaż tylko `hotelName` i `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. Przesyłanie żądania HTTP
Teraz, gdy masz opracowany kwerendy jako część adresu URL żądania HTTP (w przypadku GET) lub treści (w przypadku wpisu), można zdefiniować nagłówków żądania i przesłać zapytania.

#### <a name="request-and-request-headers"></a>Żądanie i nagłówków żądania
Należy zdefiniować dwa nagłówków żądania GET lub trzy wpisu:
1. `api-key` Nagłówka należy ustawić klucz kwerendy został znaleziony w kroku I powyżej. Należy zauważyć, że umożliwia także klucz Administrator jako `api-key` nagłówka, ale zaleca się używanie klawisza kwerendy, jak wyłącznie udziela dostęp tylko do odczytu do indeksy i dokumentów.
2. `Accept` Nagłówka musi być ustawiona na `application/json`.
3. Tylko wpisu `Content-Type` nagłówka należy również skonfigurować `application/json`.

Poniżej podano w indeksie "hotele" przy użyciu Azure wyszukiwania interfejsu API usługi REST, przy użyciu prostej kwerendy wyszukiwania terminów "motel" program żądanie HTTP GET:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Oto przykład tę samą kwerendę, tym razem przy użyciu protokołu HTTP WPIS:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Żądanie kwerendy pomyślnego spowoduje kodu stanu `200 OK` i wyniki wyszukiwania są zwracane jako JSON w treści odpowiedzi. Oto z wynikami dla powyższych kwerendy wygląda, przy założeniu indeksu "hotele" zostanie wypełniona przykładowych danych podczas [Importowania danych do wyszukiwania Azure za pomocą interfejsu API usługi REST](search-import-data-rest-api.md) (należy zauważyć, że JSON został sformatowany dla jasności).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Aby dowiedzieć się więcej, odwiedź stronę w sekcji "Odpowiedź" [Przeszukaj dokumenty](https://msdn.microsoft.com/library/azure/dn798927.aspx). Aby uzyskać więcej informacji na inne kody stanu HTTP, które mogą zostać zwrócone w przypadku awarii zobacz [kody stanu HTTP (wyszukiwanie Azure)](https://msdn.microsoft.com/library/azure/dn798925.aspx).
