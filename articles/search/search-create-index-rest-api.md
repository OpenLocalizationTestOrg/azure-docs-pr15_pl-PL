<properties
    pageTitle="Tworzenie indeksu wyszukiwania Azure za pomocą interfejsu API usługi REST | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Tworzenie indeksu w kodzie przy użyciu interfejsu API usługi REST Azure wyszukiwania HTTP."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-rest-api"></a>Tworzenie indeksu wyszukiwania Azure za pomocą interfejsu API usługi REST
> [AZURE.SELECTOR]
- [Omówienie](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [POZOSTAŁE](search-create-index-rest-api.md)


Ten artykuł prowadzi użytkownika przez proces tworzenia Azure wyszukiwania [indeksu](https://msdn.microsoft.com/library/azure/dn798941.aspx) przy użyciu interfejsu API usługi REST wyszukiwania Azure.

Przed wykonywaniem tego przewodnika i tworzenie indeksu, należy użyć już [utworzone usługa Azure wyszukiwania](search-create-service-portal.md).

Aby utworzyć indeks wyszukiwania Azure za pomocą interfejsu API usługi REST, pojedynczego żądania HTTP POST będzie wydać końcowy adres URL usługi Azure wyszukiwania. Do definicji indeksu będą zawarte w treści wezwania jako poprawnego zawartość JSON.


## <a name="i-identify-your-azure-search-services-admin-api-key"></a>I. Identyfikowanie klucz interfejsu api usługi Azure wyszukiwania administratora
Teraz, gdy masz obsługi administracyjnej usługi wyszukiwania Azure, może wydawać się żądania HTTP przed punktem końcowym adres URL tej usługi za pomocą interfejsu API usługi REST. Jednak *Wszystkie* interfejsu API żądania muszą zawierać klucz interfejsu api wygenerowany przez usługę wyszukiwania, których możesz obsługi administracyjnej. Masz prawidłowego klucza określa zaufania na zasadzie na żądanie między aplikacji wysłanie żądania oraz usługi, która obsługuje go.

1. Aby znaleźć klawisze interfejsu api tej usługi musisz zalogować do [Azure Portal](https://portal.azure.com/)
2. Przejdź do pozycji Karta usługi Azure wyszukiwania
3. Kliknij ikonę "Klawisze"

Twoja usługa uzyskuje *kluczach administratora* i *kwerendy*.

 - Podstawowy i pomocniczy *klawiszy administrator* udzielanie pełne prawa do wszystkich działań, łącznie z możliwością zarządzania usługą, tworzenie i usuwanie indeksy, indeksatory i źródeł danych. Istnieją dwa klucze, tak aby możesz nadal korzystać z kluczem pomocniczym, jeśli zechcesz ponownie wygenerować klucz podstawowy i odwrotnie.
 - *Klucze kwerendy* udzielić dostępu tylko do odczytu do indeksy i dokumenty, a zwykle są rozdzielane aplikacje klienckie, które problemu żądania wyszukiwania.

Na potrzeby tworzenia indeksu, można użyć jednej klucz podstawowy i pomocniczy administratora.

## <a name="ii-define-your-azure-search-index-using-well-formed-json"></a>II. Definiowanie indeksu wyszukiwania Azure za pomocą poprawnego JSON
Pojedyncze żądanie HTTP POST w usłudze zostanie utworzony indeks. Treści żądania HTTP POST będzie zawierać jeden obiekt JSON, który definiuje indeksu wyszukiwania Azure.

1. Pierwszej właściwości tego obiektu JSON to nazwa indeksu.
2. Druga właściwość tego obiektu JSON jest tablicą JSON o nazwie `fields` zawierający osobne obiektu JSON dla każdego pola w indeksie. Każdy z tych obiektów JSON zawiera wiele pary nazwa wartość dla każdego pola atrybutów, takich jak "Nazwa", "typ", itd.

Należy zachować wyszukiwania użytkownika środowiska i małych firm potrzeb pamiętać podczas projektowania indeksu podczas każdego pola, musi mieć przypisane [atrybuty pisane z wielkiej litery](https://msdn.microsoft.com/library/azure/dn798941.aspx). Zastosuj te określanie atrybutów wyszukiwania funkcji (filtrowania, faceting sortowania wyszukiwanie pełnotekstowe itp.) do pola, które. Dowolny atrybut, który nie zostanie określony domyślnie należy włączyć funkcję wyszukiwania odpowiednich, o ile nie wyłączysz specjalnie.

W naszym przykładzie o nazwie "hotele" indeksu i zdefiniowane naszych pola w następujący sposób:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Starannie wybrany atrybuty indeksu dla każdego pola, według jak firma Microsoft wydaje się, że będą używane w aplikacji. Na przykład `hotelId` jest unikatowy klucz tej osoby w wyszukiwaniu hotele prawdopodobnie nie będą wiedzieć, więc możemy Wyłącz przeszukiwania pełnego tekstu dla tego pola, ustawiając `searchable` do `false`, co pozwala zaoszczędzić miejsce w indeksie.

Należy zauważyć, że dokładnie jedno pole w indeksie typu `Edm.String` musi być wyznaczonych jako pole 'key'.

Definicja indeksu powyżej używa analizatora języków niestandardowych dla `description_fr` pól, ponieważ jest przeznaczona do przechowywania tekstu w języku francuskim. Zobacz [języka obsługuje tematu w witrynie MSDN](https://msdn.microsoft.com/library/azure/dn879793.aspx) , a także odpowiednie [Ogłoszenie w blogu](https://azure.microsoft.com/blog/language-support-in-azure-search/) uzyskać więcej informacji o programy do analizowania języka.

## <a name="iii-issue-the-http-request"></a>III. Żądanie HTTP
1. Za pomocą usługi definicja indeksu w treści wezwania, wydać żądania HTTP POST adres URL punktu końcowego usługi Azure wyszukiwania. W adresie URL, upewnij się, za pomocą nazwę usługi jako nazwa hosta i umieszczenie właściwego `api-version` jako parametru ciągu kwerendy (bieżąca wersja interfejsu API jest `2015-02-28` w momencie publikowanie tego dokumentu).
2. W nagłówku żądania określ `Content-Type` jako `application/json`. Należy także podać klucz administratora tej usługi określonego w kroku I w `api-key` nagłówka.


Konieczne będzie podanie własne usługi nazwę i interfejsu api klucza żądanie poniżej:


    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [api-key]


Na żądanie pomyślnego powinien zostać wyświetlony kod stanu 201 (utworzono). Aby uzyskać więcej informacji na temat tworzenia indeksu za pośrednictwem interfejsu API usługi REST odwiedź stronę odwołanie interfejsu API w [witrynie MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx). Aby uzyskać więcej informacji na inne kody stanu HTTP, które mogą zostać zwrócone w przypadku awarii zobacz [kody stanu HTTP (wyszukiwanie Azure)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

Po skończeniu pracy z indeksu i chcesz ją usunąć, po prostu problemów żądanie HTTP usunąć. Na przykład Oto jak chcesz usunąć indeks "hotele":

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2015-02-28
    api-key: [api-key]


## <a name="next"></a>Następny
Po utworzeniu indeksu wyszukiwania Azure, będzie gotowy do [przekazania zawartość w indeksie](search-what-is-data-import.md) , możesz rozpocząć wyszukiwanie danych.
