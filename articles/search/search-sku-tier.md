<properties
    pageTitle="Wybierz numer Magazynowy lub ceny warstwa Azure wyszukiwania | Microsoft Azure"
    description="Azure wyszukiwania mogą być obsługi administracyjnej w wersji tych produktów: bezpłatna, podstawowe i standardowy, gdzie standardowe jest dostępna w różnych konfiguracji zasobów i poziomów wydajności."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="10/24/2016"
    ms.author="heidist"/>

# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Wybierz numer Magazynowy lub ceny warstwa Azure wyszukiwania

W polu Wyszukaj Azure [zainicjowano obsługę administracyjną usługi](search-create-service-portal.md) określonego poziomu cen lub SKU. Opcje obejmują **wolny**, **podstawowe**lub **Standardowy**, gdzie **Standardowy** jest dostępna w wielu konfiguracji i możliwości. 

Celem tego artykułu jest ułatwiające wybór warstwa. Jeśli warstwa zdolności okaże się zbyt niska, będzie konieczne obsługi administracyjnej nowej usługi w wyższego poziomu, a następnie ponownie załaduj do indeksy. Nie ma możliwości uaktualnienia w miejscu w tej samej usługi z jednej wersji do drugiej. 

> [AZURE.NOTE] Po wybraniu warstwy i [świadczenia usługi wyszukiwania](search-create-service-portal.md), można zwiększyć replice i partycją zlicza w usłudze. Aby uzyskać instrukcje zobacz [poziomy zasobów skali kwerendy i indeksowania obciążenia](search-capacity-planning.md).

## <a name="how-to-approach-a-pricing-tier-decision"></a>Jak podejście cennik decyzja warstwy

W polu Wyszukaj Azure warstwie określa wydajność, nie dostępność funkcji. Ogólnie rzecz biorąc funkcje są dostępne na każdej warstwie, łącznie z funkcji podglądu. Jedynym wyjątkiem jest nie obsługuje indeksatorów w S3 HD.

> [AZURE.TIP] Zaleca się zawsze obsługi administracyjnej usługi **wolny** (jeden na subskrypcję z nie wygaśnięcia), aby jej łatwo dostępne dla projektów uproszczonej. Za pomocą usługi **wolny** do testowania i oceny; Tworzenie drugiej usługi fakturowanego na warstwie **podstawowe** lub **Standardowy** do produkcji lub większe obciążenia test.

Wydajność i koszty uruchomiona usługa go ręcznie w kształcie dłoni. Informacje w tym artykule mogą pomóc określić, które SKU zapewnia odpowiedni stosunek, ale dla każdego ze go za przydatny, potrzebujesz co najmniej drukowania szacuje od następujących czynników:

- Liczba i rozmiar indeksów, który ma zostać utworzona
- Liczby i rozmiaru dokumentów, aby przekazać
- Niektóre ogólny obraz tego zapytania obrót, za pomocą kwerendy na drugim (QPS)

Liczba i rozmiar są ważne, ponieważ osiągnięto maksymalną za pośrednictwem stały limit liczby indeksów lub dokumenty w usłudze lub zasobów (miejsca do magazynowania lub podrzędnych) używane przez usługę. Rzeczywisty limit tej usługi jest zależności jest używany na pierwszym: zasoby lub obiekty.

Z prognozy w kształcie dłoni następujące czynności należy uprościć proces:

- **Krok 1** Przejrzyj opisy SKU poniżej, aby poznać dostępne opcje.
- **Krok 2** Odpowiedz na pytania poniżej, aby zostać obciążony wstępny decyzji.
- **Krok 3** Finalizowanie swoją decyzję przeglądania słabo ograniczenia dotyczące magazynowania i cennik.

## <a name="sku-descriptions"></a>Opisy wersji

Poniższa tabela zawiera opisy poszczególnych poziomów. 

Warstwy|Podstawowe scenariusze
----|-----------------
**Bezpłatne**|Usługa udostępniona, bezpłatnie, używane do obliczeń, dochodzenia lub małe obciążenia. Ponieważ jest on udostępniany innym użytkownikom, przepustowość kwerendy i indeksowania zależy kto jeszcze korzysta z usługi. Wydajność jest mała (50 MB lub indeksy 3 z 10 000 w górę dokumentów każdego).
**Podstawowe**|Małe produkcji obciążenia na sprzęcie. Wysokiej dostępności. Wydajność zależy od repliki 3 i partycją 1 (2 GB).
**S1**|Standardowy 1 obsługuje elastyczne kombinacje replik (12) na potrzeby średnich produkcji obciążenia na sprzęcie i partycje (12). Możesz przydzielić partycje i replikami kombinacji obsługiwanych przez maksymalna liczba jednostek 36 fakturowanego wyszukiwania. Na tym poziomie partycje są 25 GB i QPS jest około 15 kwerend na sekundę.
**S2**|Standardowy 2 uruchamia większe obciążenia produkcji przy użyciu tej samej jednostki 36 wyszukiwania jako S1, ale partycje o rozmiarze większym i repliki. Na tym poziomie partycje są 100 GB i QPS jest około 60 kwerend na sekundę.
**S3**|Standardowy 3 działa proporcjonalnie większych obciążenia produkcji w systemach wyższą zakończenia w konfiguracji do 12 partycje lub 12 podrzędnych jednostek w obszarze 36 wyszukiwania. Na tym poziomie partycje są 200 GB i QPS jest więcej niż 60 kwerend na sekundę. 
**S3 HD**|Standardowy 3 dużej gęstości jest przeznaczony dla dużej liczby mniejszych indeksy. Możesz mieć maksymalnie 3 partycje na 200 GB. QPS jest więcej niż 60 kwerend na sekundę. 

> [AZURE.NOTE] Maksymalne szybkości w trybie replice i partycją są wystawiona się jako jednostki wyszukiwania (36 jednostka maksymalna dla danej usługi), które są nakładane dolną granicę skutecznych niż maksimum oznacza wartość nominalnej. Na przykład, aby użyć maksymalnie 12 repliki, może mieć co najwyżej 3 partycje (12 * 3 = 36 jednostki). Podobnie Aby użyć maksymalna partycje, Zmniejsz repliki 3. Zobacz [poziomy zasobów skali kwerendy i indeksowania obciążenia w wyszukiwaniu Azure](search-capacity-planning.md) dla wykresu na kombinacji dozwolony.

## <a name="review-limits-per-tier"></a>Przejrzyj ograniczenia na warstwie

Poniższy wykres jest podzbiorem limitów z [Limity dotyczące usługi Azure wyszukiwania](search-limits-quotas-capacity.md). Wyświetla listę czynników najprawdopodobniej wpływu decyzji SKU. Ten wykres może dotyczyć podczas przeglądania poniższe pytania.

Zasób|Bezpłatne|Podstawowe|S1|S2|S3 |S3 HD
---|---|---|---|----|---|----
Umowa dotycząca poziomu usług (SLA)|Nr <sup>1</sup> |Tak |Tak  |Tak |Tak  |Tak 
Limity indeksu|3|5|50|200|200|1000 <sup>2</sup>
Limity dokumentu|10 000 łącznie|1 mln dla danej usługi|15 milionów na partition |60 milionów na partition|120 milionów na partition |1 miliona indeksu
Maksymalna liczba partycje|N/D! |1 |12  |12 |12|3 <sup>2</sup>
Rozmiar partition|Suma 50 MB|2 GB na usługi|25 GB na partition |100 GB na partition (maksymalnie 1,2 TB dla danej usługi)|200 GB na partition (maksymalnie 2,4 TB dla danej usługi)|200 GB (maksymalnie 600 GB dla danej usługi)
Maksymalna liczba replik|N/D! |3 |12 |12 |12|12
Kwerend na sekundę|N/D!|~ 3 na replice|~ 15 na replice|~ 60 na replice|> 60 na replice|> 60 na replice

<sup>1</sup> bezpłatnego i wersji Preview produktu nie są dostarczane z poziomu. Systemy są wymuszane, gdy SKU staje się ogólnodostępne.

<sup>2</sup> S3 kopie i S3 HD są tworzone przez infrastrukturę identyczne dużej pojemności, ale każdy z nich osiągnie limit maksymalny na różne sposoby. S3 kieruje mniejszej liczby bardzo duże indeksy. Jako takie, jego maksymalna jest związany z zasobów (2,4 TB dla każdej usługi). S3 HD jest przeznaczony dla dużej liczby bardzo mały indeksy. W 1000 indeksy S3 HD osiągnie limit w postaci indeksu ograniczeń. Jeśli jesteś klientem S3 HD, która wymaga więcej niż 1000 indeksy, skontaktuj się Support firmy Microsoft informacje na temat kontynuować.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Wyeliminować wersji produktów, które nie spełnia wymagań 

Następujące pytania mogą ułatwić obciążony prawo decyzja SKU dla z pracą.

1. Czy masz wymagania **Dotyczącą Umowa dotycząca poziomu usług (SLA)** ? Zawęź decyzji SKU standardowe podstawowe lub innych niż Podgląd.
2. **Ile indeksy** czy wymagać? Jedną z najważniejszych zmiennych factoring do decyzji SKU jest liczba indeksów obsługiwanych przez każdego SKU. Obsługa indeks jest poziomie znacznie różnią się w dolnym poziomów cennik. Wymagania dotyczące liczba indeksów może być podstawowy wyznacznik decyzji SKU.
3. **Ile dokumenty** będą ładowane do każdego indeks? Liczby i rozmiaru dokumentów określi rozmiar indeksu. Zakładając, że można oszacować przewidywane rozmiar indeksu, możesz porównać tego numeru dla rozmiaru partycją na SKU rozszerzonego przez liczbę partycje wymaganych do przechowania indeksu tego rozmiaru. 
4. **Co to jest ładowania zapytań oczekiwanych**? Po rozumie wymagania dotyczące miejsca do magazynowania, warto rozważyć obciążenia kwerendy. S2 i obu SKU S3 oferują u odpowiednik przepustowość, ale wymagania SLA będzie wykluczenia dowolnego podglądu wersji produktu. 
5. Planując warstwie S2 lub S3, określ, czy są potrzebne [indeksatory](search-indexer-overview.md). Indeksatory nie są jeszcze dostępne dla poziomu S3 HD. Alternatywny rozwiązaniem jest używany model wypychanych aktualizacji indeksu, gdzie wpisać kod aplikacji, aby przekazać zestawu danych do indeksu.

Większość klientów można reguły określonej wersji lub na podstawie ich odpowiedzi na te pytania. Jeśli nadal nie masz pewności przejść z wersji produktu, możesz Publikuj pytania na forach MSDN lub zdarzeń StackOverflow lub kontakt z pomocą techniczną Azure w sprawie dodatkowych wskazówek.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Decyzja sprawdzania poprawności: używaną WERSJĘ oferuje wystarczającą ilość miejsca i QPS?

Jako ostatni krok odwiedzaj [ceny strony](https://azure.microsoft.com/pricing/details/search/) i [sekcje na usługi i na indeks usługi limity](search-limits-quotas-capacity.md) dokładnie oszacowania limitów subskrypcji i usługi. 

Jeśli wymagania ceny lub miejsca do magazynowania znajdują się poza zakresem, warto refactor obciążenia między wiele mniejszych usług (na przykład). Na poziomie bardziej szczegółowego można ponownie zaprojektować indeksów może być mniejszy lub używanie filtrów w celu skuteczność kwerendy.

> [AZURE.NOTE] Wymagania dotyczące magazynu może być nadmiernie nadmuchany, gdy dokumenty będą zawierać danych zewnętrznych. Najlepiej, jeśli dokumenty będą zawierać tylko danych z możliwością lub metadanych. Dane binarne nie można wyszukiwać i powinny być przechowywane oddzielnie (prawdopodobnie w Azure magazynowania tabeli lub obiektów blob) z polem w indeksie, aby wstrzymać odwołanie adresu URL do danych zewnętrznych. Maksymalny rozmiar pojedynczy dokument jest 16 MB (lub mniej jeżeli są zbiorcze przekazywanie wielu dokumentów w jedno żądanie). Aby uzyskać więcej informacji, zobacz [limity dotyczące usługi Azure wyszukiwania](search-limits-quotas-capacity.md) .

## <a name="next-step"></a>Następny krok

Znając co prawo dopasowanie SKU nadal z następujących czynności:

- [Tworzenie usługi wyszukiwania w portalu](search-create-service-portal.md)
- [Zmienianie przydziału partycje i repliki przeskalować usługi](search-capacity-planning.md)

