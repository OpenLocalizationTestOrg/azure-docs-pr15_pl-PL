<properties
    pageTitle="Jak używać Fiddler ocenianie i przetestować interfejsy API pozostałych wyszukiwania Azure | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Używanie Fiddler dla zwolnienia kod metodę sprawdzania dostępności Azure wyszukiwania i testujesz interfejsy API pozostałych."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Ocenianie i przetestować interfejsy API pozostałych wyszukiwania Azure za pomocą Fiddler
> [AZURE.SELECTOR]
- [Omówienie](search-query-overview.md)
- [Eksplorator usługi wyszukiwania](search-explorer.md)
- [Fiddler](search-fiddler.md)
- [.NET](search-query-dotnet.md)
- [POZOSTAŁE](search-query-rest-api.md)

W tym artykule wyjaśniono, jak za pomocą Fiddler, dostępny do [pobrania bezpłatnie z Telerik](http://www.telerik.com/fiddler), żądania HTTP problem i Wyświetl odpowiedzi przy użyciu interfejsu API usługi REST wyszukiwania Azure, bez konieczności pisania kodu. Azure wyszukiwania jest w pełni zarządzanymi, hostowana usługa w chmurze wyszukiwania na Microsoft Azure łatwo programowalnej za pośrednictwem .NET oraz interfejsy API pozostałych. Usługa wyszukiwania Azure interfejsy API pozostałych opisano w [witrynie MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

W następującej procedurze będzie utworzyć indeks, przekazywanie dokumentów, kwerendy indeks i następnie kwerendy systemu informacji o usłudze.

Aby wykonać te kroki, konieczne będzie usługa Azure wyszukiwania i `api-key`. Aby uzyskać instrukcje, jak rozpocząć pracę, zobacz [Tworzenie usługi Azure wyszukiwania w portalu](search-create-service-portal.md) .

## <a name="create-an-index"></a>Tworzenie indeksu

1. Rozpocznij Fiddler. W menu **plik** wyłączyć **Rejestrowanie ruch** do Ukryj zbędne działania HTTP, który nie ma wpływu na bieżące zadanie.

3. Na karcie **Kompozytor** będzie sformułowania żądanie wygląda następujące zrzut ekranu.

    ![][1]

2. Wybierz pozycję **Umieść**.

3. Wprowadź adres URL, która określa adres URL usługi, atrybuty żądania i wersja api. Kilka wskazówek należy pamiętać:
   + Używanie HTTPS jako prefiks.
   + Atrybut żądania jest "/ indeksy/hotele". To informuje wyszukiwania, aby utworzyć indeks o nazwie "hotele".
   + Wersja API jest małe litery, określony jako "? wersja api = 2015-02-28". Wersje interfejsu API są ważne, ponieważ wyszukiwania Azure wdraża aktualizacje regularnie. Sporadycznie aktualizacji usługi może spowodować zmianę podziału API. Z tego powodu wyszukiwanie Azure wymaga wersję interfejsu api dla każdego żądania, aby móc pełną kontrolę nad która z nich jest używana.

    Pełny adres URL powinna wyglądać podobnie do następującego przykładu.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Określ nagłówek, zamieniając host i klucz interfejsu api wartości, które są prawidłowe dla tej usługi.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  W treści wezwania Wklej w polach, które składają się definicja indeksu.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Kliknij przycisk **Wykonaj**.

W ciągu kilku sekund powinna być widoczna odpowiedź HTTP 201 na liście sesji wskazuje, że został utworzony pomyślnie.

Jeśli zostanie wyświetlony HTTP 504, sprawdź, czy adres URL określa HTTPS. Jeśli zostanie wyświetlony HTTP 400 lub 404, sprawdź treści żądania, aby sprawdzić, czy nie wystąpiły, kopiowanie i wklejanie błędy. HTTP 403 zwykle oznacza problem z klucz interfejsu api (nieprawidłowy klucz lub składni problem z jak określono klucz interfejsu api).

## <a name="load-documents"></a>Ładowanie dokumentów

Na karcie **Kompozytor** Twoją prośbę o publikować dokumenty będą wyglądać następująco. Treści wezwania zawiera dane wyszukiwania dla hotele 4.

   ![][2]

1. Wybierz przycisk **OPUBLIKUJ**.

2.  Wprowadź adres URL, które rozpoczynają się od HTTPS, a po nim adres URL usługi następuje "/indexes/ < nazwa_indeksu >/dokumenty/indeks? wersja api = 2015-02-28". Pełny adres URL powinna wyglądać podobnie do następującego przykładu.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Nagłówek powinna być taka sama jak przed. Pamiętaj, że zastępowana host i klucz interfejsu api z wartościami, które są ważne tej usługi.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Żądanie treść zawiera cztery dokumenty, które mają zostać dodane do indeksu hotele.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
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
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Kliknij przycisk **Wykonaj**.

W ciągu kilku sekund powinna być widoczna odpowiedź HTTP 200 na liście sesji. Oznacza to, że pomyślnie utworzono dokumenty. Jeśli zostanie wyświetlony 207, co najmniej jeden dokument nie można przekazać. Jeśli zostanie wyświetlony 404, masz błąd składni w nagłówku lub w treści wezwania.

## <a name="query-the-index"></a>Kwerendy indeksu

Teraz, gdy są ładowane indeksu i dokumentów, może wydawać się kwerendy z nimi.  Na karcie **Kompozytor** polecenie **GET** , która korzysta z usługi będzie wyglądać podobnie do następujących zrzut ekranu.

   ![][3]

1.  Wybierz pozycję **Pobierz**.

2.  Wprowadź adres URL, które rozpoczynają się od HTTPS, a po nim adres URL usługi następuje "/indexes/ < nazwa_indeksu >-dokumenty?", a następnie parametry kwerendy. Jako przykład za pomocą następujący adres URL, zamieniając Przykładowa nazwa hosta prawidłowa tej usługi.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Ta kwerenda wyszukuje termin "motel" i pobiera kategorii aspekt dla oceny.

3.  Nagłówek powinna być taka sama jak przed. Pamiętaj, że zastępowana host i klucz interfejsu api z wartościami, które są ważne tej usługi.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

Kod odpowiedzi powinny być 200, a wynik odpowiedzi powinna wyglądać podobnie do następujących zrzut ekranu.

   ![][4]

Poniższe zapytanie przykład pochodzi z [operacji indeksowania wyszukiwania (API wyszukiwania Azure)](http://msdn.microsoft.com/library/dn798927.aspx) w witrynie MSDN. Wiele przykładowych kwerend w tym temacie może zawierać spacji, które nie są dozwolone w Fiddler. Zamień każdego miejsca przy użyciu znak + a przed wklejania w kwerendzie ciąg przed podjęciem kwerendę w Fiddler.

**Przed spacje są zastępowane:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Po spacje są zastępowane +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>Kwerenda systemu

Można również sprawdzać system zlicza dokumentu i zużycie miejsca do magazynowania. Na karcie **Kompozytor** żądania będzie wyglądać podobnie do następującej i odpowiedź zwraca zestawienia liczby dokumentów i miejsca.

 ![][5]

1.  Wybierz pozycję **Pobierz**.

2.  Wprowadź adres URL, który zawiera adres URL usługi, a po nim "/ indeksy i hotele i statystykę? wersja api = 2015-02-28":

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Określ nagłówek, zamieniając host i klucz interfejsu api wartości, które są prawidłowe dla tej usługi.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Treść żądania pozostaw puste.

5.  Kliknij przycisk **Wykonaj**. Powinien zostać wyświetlony kod stanu HTTP 200 na liście sesji. Wybierz pozycję opublikowany dla polecenia.

6.  Kliknij kartę **moduły inspektorów** , kliknij kartę **nagłówki** , a następnie wybierz odpowiedni format JSON. Powinien zostać wyświetlony rozmiar miejsca do magazynowania i liczba dokumentu (w KB).

## <a name="next-steps"></a>Następne kroki

Zobacz [Zarządzanie usługą wyszukiwania Azure](search-manage.md) dla bez użycia kodu dojechać do korzystania z wyszukiwania Azure i zarządzania nimi.


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
