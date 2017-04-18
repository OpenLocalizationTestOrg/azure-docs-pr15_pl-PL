<properties
    pageTitle="Co to jest Azure wyszukiwania | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Wyszukiwanie Azure to usługa wyszukiwania zarządzanych w pełni obsługiwane chmury. Dowiedz się, w tym omówienie funkcji."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="what-is-azure-search"></a>Co to jest Azure wyszukiwania?

Azure wyszukiwania jest rozwiązaniem wyszukiwania jako usługi cloud deleguje zarządzanie infrastrukturą i server do firmy Microsoft, pozostawiając możesz za pomocą usługi gotowych do użycia, który można wypełniać danych i następnie umożliwia dodawanie wyszukiwania do usługi sieci web lub aplikacji mobilnej. Azure wyszukiwania pozwala na łatwe dodawanie zaawansowane przeszukiwanie aplikacji przy użyciu prostego [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) lub [.NET SDK](search-howto-dotnet-sdk.md) bez zarządzania infrastrukturą wyszukiwania lub staje się ekspertem w wyszukiwaniu.

## <a name="give-your-users-a-powerful-search-experience"></a>Nadaj użytkownikom wyszukiwanie zaawansowane

**Wydajnych kwerend** można określane przy użyciu [prostej kwerendy składni](https://msdn.microsoft.com/library/azure/dn798920.aspx), która oferuje operatorów logicznych, operatory wyszukiwania frazę, sufiks operatorów, pierwszeństwo operatorów. Ponadto [składnią kwerendy Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) można włączyć rozmyty wyszukiwania, wyszukiwanie odległość, zwiększenia terminów i wyrażeń regularnych. Wyszukiwanie Azure obsługuje także niestandardowe programy leksykalne do analizowania aby umożliwić aplikacji do obsługi zapytań złożone wyszukiwania przy użyciu dopasowania fonetyczny i wyrażeń regularnych.

**Obsługa języków** jest [dostępny dla innych języków 56](https://msdn.microsoft.com/library/azure/dn879793.aspx). Przy użyciu zarówno Lucene programy do analizowania i programy do analizowania firmy Microsoft (Uściślanie według lat języku naturalnym przetwarzania w pakiecie Office i Bing), Azure wyszukiwania można analizować tekst w polu wyszukiwania aplikacji inteligentną obsługę lingwistyczne specyficzne dla języka, w tym czasowników, płeć, o nieregularnym kształcie rzeczowniki mnogiej (np. "myszy" a "myszy"), program word do łączenia, dzielenie wyrazów (w przypadku języków bez spacji) i nie tylko.

**Sugestie dotyczące wyszukiwania** można włączyć dla kwerend dynamiczne i paski wyszukiwania Autouzupełniania. [Sugerowane są rzeczywiste dokumenty w indeksie](https://msdn.microsoft.com/library/azure/dn798936.aspx) jako użytkownicy wprowadź wyszukiwania częściowego wprowadzania.

**Trafienie wyróżnienia** [umożliwia](https://msdn.microsoft.com/library/azure/dn798927.aspx) użytkownikom wyświetlanie fragment tekstu w każdej wynik, który zawiera dopasowania ich zapytania. Możesz wybrać pola, które zwracana wyróżniony wstawki.

**Nawigacji aspektowej** łatwo jest dodawany do strony wyników wyszukiwania za pomocą wyszukiwania Azure. Używanie [tylko parametru jedną kwerendę](https://msdn.microsoft.com/library/azure/dn798927.aspx), wyszukiwanie Azure zwróci wszystkie informacje potrzebne do utworzenia aspektowej przeszukiwanie w interfejsie użytkownika aplikacji programu, aby umożliwić użytkownikom rozwijania i filtrować wyniki wyszukiwania (np. filtr wykazu elementy według zakresu cen lub marki).

Obsługa **przestrzenna Geo** pozwala na sposób inteligentny procesu, filtrowania i wyświetlania lokalizacje geograficzne. Wyszukiwanie Azure umożliwia użytkownikom Eksplorowanie danych oparty na odległość wyników wyszukiwania do określonej lokalizacji lub na podstawie określonego regionu geograficznego. W tym klipie wideo wyjaśniono, jak to działa: [kanału 9: wyszukiwanie Azure i dane geograficzne danych](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

**Filtry** można łatwo dołączyć nawigacji aspektowej do interfejsu użytkownika aplikacji, poprawy sformułowania kwerendy, a filtr oparty na lub deweloper zdefiniowane przez użytkownika kryteriów. Tworzenie zaawansowanych filtrów przy użyciu [składni OData](https://msdn.microsoft.com/library/azure/dn798921.aspx).

## <a name="empower-your-developers-with-an-easy-to-use-service"></a>Wzmocniła deweloperów z usługą prostych w użyciu

**Wysoce** zapewnia bardzo niezawodne przeszukiwanie usługi. Gdy skalowany poprawnie, [wyszukiwania Azure oferuje SLA 99,9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

**Pełna** jako kompleksowe rozwiązania wyszukiwania Azure wymaga naprawdę bez zarządzania infrastrukturą. Usługi mogą być łatwo dostosowane do Twoich potrzeb przez skalowania dwuwymiarowej obsługę dodatkowego miejsca do magazynowania dokumentu, wyższa ładowania zapytań lub oba.

**Integracja danych** za pomocą [indeksatory](https://msdn.microsoft.com/library/azure/dn946891.aspx) umożliwia wyszukiwanie Azure automatycznie przeszukiwania bazy danych SQL Azure, Azure DocumentDB lub [Magazyn obiektów Blob platformy Azure](search-howto-indexing-azure-blob-storage.md) synchronizowanie zawartości indeksu wyszukiwania za pomocą sklepu podstawowego.

**Dokument krakingu** jest dostępny (obecnie w podglądzie) [do odczytu i indeksu formaty plików głównych](search-howto-indexing-azure-blob-storage.md) tym Microsoft Office, a także dokumenty PDF i HTML.

**Wyszukiwanie ruch analizy** są [łatwo zbierane i analizowane](search-traffic-analytics.md) Aby odblokować wniosków z co użytkowników jest wpisywany tekst w polu wyszukiwania.

**Prosta wyników** jest główną zaletą Azure wyszukiwania. [Wyników profile](https://msdn.microsoft.com/library/azure/dn798928.aspx) służą do umożliwiają organizacji istotności modelu jako funkcja wartości w same dokumenty. Na przykład może być nowsze produkty lub rabatem produktów, które mają pojawić się wyżej w wynikach wyszukiwania. Można również tworzyć profile wyników przy użyciu tagów spersonalizowanych wyników według preferencji wyszukiwania klienta zostały śledzone i przechowywane oddzielnie.

**Sortowanie** jest oferowanych dla wielu pól za pomocą schematu indeks, a następnie włączone w czasie kwerendy z pojedynczy parametr.

**Stronicowania** i ograniczanie wyników wyszukiwania jest [proste precyzyjnie dostrajanych kontrolki](search-pagination-page-layout.md) oferująca Azure wyszukiwania na wyniki wyszukiwania.  

**Eksplorator usługi wyszukiwania** pozwala na problem kwerend dla wszystkich usługi indeksów prawo od portal Azure Twoje konto łatwe testowanie kwerendy i Uściślanie wyników profile.

## <a name="how-it-works"></a>Jak to działa

### <a name="1-provision-service"></a>1. Usługa Obsługa administracyjna
Można dokończyć usługi Azure wyszukiwania przy użyciu [Azure Portal](https://portal.azure.com/) lub [Interfejsu API zarządzania zasobu Azure](https://msdn.microsoft.com/library/azure/dn832684.aspx).

W zależności od tego, jak należy skonfigurować usługę wyszukiwania za pomocą bezpłatnej warstwa usługa, która ma być udostępniana innych subskrybentów wyszukiwania Azure, albo [opłaconej poziomu](https://azure.microsoft.com/pricing/details/search/) którego rezerwuje zasobów może być używany tylko przez usługę. Podczas inicjowania obsługi administracyjnej usługi, możesz również wybrać obszaru centrum danych, który obsługuje usługi.

W zależności od których warstwa możesz wybrać usługi, można skalować usługi dwuwymiarowej: 1) Dodawanie repliki zwiększenia możliwości obsługi ładowania zapytań intensywnie i (2) dodawanie partycje do dodania miejsca do magazynowania dla kolejnych dokumentów. Za obsługi dokumentu kwerendy i magazynowania przepustowość oddzielnie, można dostosować usługi wyszukiwania do określonych potrzeb.

### <a name="2-create-index"></a>2. Tworzenie indeksu
Przed zawartości można przekazać do usługi Azure wyszukiwania, należy najpierw zdefiniować indeksu wyszukiwania Azure. Indeks przypomina tabelę bazy danych, która zawiera dane i może zaakceptować kwerend wyszukiwania. Definiowanie schematu indeks do zamapować na strukturze dokumenty, które chcesz wyszukać, podobne do pól w bazie danych.

Schemat tych wskaźników albo można utworzyć Azure Portal lub programowo [przy użyciu zestawu SDK .NET](search-howto-dotnet-sdk.md) lub [Interfejsu API usługi REST](https://msdn.microsoft.com/library/azure/dn798941.aspx). Po zdefiniowano indeks, możesz przekazać dane do usługi Azure wyszukiwania miejsce, w którym jest następnie indeksowane.

### <a name="3-index-data"></a>3. dane indeksu
Po zdefiniowaniu pól i atrybuty indeksu, możesz przekazać zawartość do indeksu. Za pomocą modelu lub wypychania do przekazania danych do indeksu.

Model pobieraj są dostarczane za pośrednictwem indeksatory, które można skonfigurować dla na żądanie lub zaplanowane aktualizacje (zobacz [operacji indeksowania (Azure wyszukiwania usługi interfejsu API usługi REST)](https://msdn.microsoft.com/library/azure/dn946891.aspx)), co pozwala łatwo mogły zjeść tej ostatniej dane i zmiany danych z Azure DocumentDB, bazy danych SQL Azure, magazyn obiektów Blob platformy Azure lub SQL Server obsługiwany w maszyn wirtualnych Azure.

Model wypychanych jest dostępna za pośrednictwem SDK lub pozostałych interfejsy API używany do wysyłania zaktualizowane dokumenty do indeksu. Dane można push z niemal dowolnego zestawu danych w formacie JSON. Zobacz temat [Dodawanie, aktualizowanie, lub usuwanie dokumentów](https://msdn.microsoft.com/library/azure/dn798930.aspx) lub [sposobu używania .NET SDK)](search-howto-dotnet-sdk.md) wskazówki dotyczące ładowania danych.

### <a name="4-search"></a>4. wyszukiwanie
Po wypełnił indeksu wyszukiwania Azure, możesz go teraz [kwerend wyszukiwania problem](https://msdn.microsoft.com/library/azure/dn798927.aspx) z prostego żądania HTTP za pomocą interfejsu API usługi REST lub .NET SDK punktu końcowego usługi.

## <a name="try-it-now-for-free"></a>Wypróbuj teraz (bezpłatnie!)
Możesz spróbować wyszukiwania Azure już dziś! Jeśli masz już konto Azure, możesz [świadczenia usługi w bezpłatnej warstwie](search-create-service-portal.md).

Jeśli nie masz konto Azure, że możesz wypróbować bezpłatną, 60 minut sesji bez konta wymagane. Przejdź do [Spróbuj Azure aplikacji usługi](http://go.microsoft.com/fwlink/p/?LinkId=618214) i wybierz pozycję "Web App." Następnie wybierz odpowiedni szablon "ASP.NET + Azure wyszukiwania", aby rozpocząć pracę.
