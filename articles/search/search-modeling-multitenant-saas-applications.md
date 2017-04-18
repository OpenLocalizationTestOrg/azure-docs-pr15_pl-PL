<properties
    pageTitle="Modelowanie Multitenancy w wyszukiwaniu Azure | Microsoft Azure | Usługa wyszukiwania hostowanej chmury"
    description="Informacje na temat typowych wzorców projektu dla multitenant aplikacji władz akredytacji bezpieczeństwa podczas korzystania z wyszukiwania Azure."
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
    ms.date="10/26/2016"
    ms.author="ashmaka"/>

# <a name="design-patterns-for-multitenant-saas-applications-and-azure-search"></a>Projektowanie wzorców multitenant aplikacji władz akredytacji bezpieczeństwa i wyszukiwania Azure

Multitenant aplikacji jest taki, który zawiera tę samą usług i funkcji do dowolnej liczby dzierżaw, którzy nie widać lub udostępnianie danych innym dzierżawy. Strategie izolacji dzierżawy multitenant aplikacji utworzonych za pomocą wyszukiwania Azure omówione w tym dokumencie.

## <a name="azure-search-concepts"></a>Azure pojęcia wyszukiwania
Jako rozwiązania wyszukiwania jako usługi Azure wyszukiwania pozwala na dodawanie sformatowanego wyszukiwania środowiska aplikacji bez zarządzania infrastrukturą dowolnego lub staje się ekspertem w wyszukiwaniu. Dane są przekazane do usługi, a następnie zapisywane w chmurze. Za pomocą prostych żądań API wyszukiwania Azure, dane następnie można modyfikować i przeszukiwana. Omówienie usługi można znaleźć w [tym artykule](http://aka.ms/whatisazsearch). Przed omówieniem wzorców projektu, jest zrozumieć niektóre pojęcia w wyszukiwaniu Azure.

### <a name="search-services-indexes-fields-and-documents"></a>Usług wyszukiwania, indeksy pól i dokumentów
Podczas korzystania z wyszukiwania Azure jedną subskrybuje _usługi wyszukiwania_. Jak danych jest przekazane do wyszukiwania Azure, znajduje się w _indeksu_ w ramach usługi wyszukiwania. Może być liczbą indeksów w ramach jednej usługi. Aby użyć znanych koncepcji baz danych, Usługa wyszukiwania można przyrównać do bazy danych podczas indeksy w ramach usługi można przyrównać do tabel w bazie danych.

Każdy indeks w ramach usługi wyszukiwania ma własny schemat, który jest zdefiniowany przez kilka możliwości dostosowywania _pola_. Dane są dodawane do indeksu wyszukiwania Azure w formularzu poszczególnych _dokumentów_. Każdy dokument musi być przekazane do określonego indeksu i należy dopasować schematu tego indeksu. Podczas wyszukiwania danych za pomocą wyszukiwania Azure kwerend wyszukiwania pełnotekstowym są wydawane dla określonego indeksu.  Aby porównać te pojęcia do tych bazy danych, pola można przyrównać do kolumn w tabeli i dokumenty można przyrównać do obszaru wiersze.

### <a name="scalability"></a>Skalowalność
Wszelkie usługi Azure wyszukiwania w standardowe [ceny warstwa](https://azure.microsoft.com/pricing/details/search/) można skalować dwuwymiarowej: przechowywanie i dostępności.
* Aby zwiększyć przestrzeni dyskowej usługi wyszukiwania można dodawać _partycje_ .
* _Repliki_ można dodać do usługi, aby zwiększyć przepustowość żądań obsługujące usługi wyszukiwania.

Dodawanie i usuwanie partycje i repliki u umożliwi możliwości usługi wyszukiwania powiększanie z ilości danych i ruch przez aplikację. Aby usługa wyszukiwania osiągnąć odczytu [Umowa dotycząca poziomu usług](https://azure.microsoft.com/support/legal/sla/search/v1_0/)wymaga dwóch replikach. Aby usługi, aby osiągnąć odczytu i zapisu [Umowa dotycząca poziomu usług](https://azure.microsoft.com/support/legal/sla/search/v1_0/)wymaga trzech repliki.


### <a name="service-and-index-limits-in-azure-search"></a>Limity dotyczące usługi i indeks wyszukiwania Azure
Istnieje kilka różnych [poziomów ceny](https://azure.microsoft.com/pricing/details/search/) w wyszukiwaniu Azure, każda poziomów zawiera inne [limity i przydziałów](search-limits-quotas-capacity.md). Niektóre z tych ograniczeń są na poziomie usługi, niektóre są na poziomie indeks, a niektóre są na poziomie partycją.


|                                  | Podstawowe     | Standard1   | Standard2   | Standard3   | Standard3 HD  |
|----------------------------------|-----------|-------------|-------------|-------------|---------------|
| Maksymalna liczba replik dla danej usługi     | 3         | 12          | 12          | 12          | 12            |
| Maksymalna liczba partycje dla danej usługi   | 1         | 12          | 12          | 12          | 1             |
| Wyszukiwanie maksymalna liczba jednostek (repliki * partycje) dla danej usługi | 3         | 36          | 36          | 36          | 36 (maksymalna partycje 3)            |
| Maksymalna liczba dokumentów na usługi    | 1 mln | milionów 180 | 720 milionów | miliardów 1.4 | 600 mln   |
| Maksymalna liczba miejsca do magazynowania dla danej usługi      | 2 GB      | 300 GB      | 1.2 TB      | 2,4 TB      | 600 GB        |
| Maksymalna liczba dokumentów na Partition  | 1 mln | 15 milionów  | 60 milionów  | 120 milionów | 200 milionów   |
| Maksymalna liczba miejsce w magazynie na Partition    | 2 GB      | 25 GB       | 100 GB      | 200 GB      | 200 GB        |
| Maksymalna liczba indeksów na usługi      | 5         | 50          | 200         | 200         | 3000 (maksymalnie 1000 indeksy i partycją)          |


#### <a name="s3-high-density"></a>S3 Duża gęstość "
W warstwie cennik S3 Azure wyszukiwania opcja znajduje się w trybie High gęstości (HD) zaprojektowane specjalnie dla multitenant scenariuszy. W większości przypadków jest niezbędne do obsługi dużej liczby mniejsze dzierżaw w obszarze jednej usługi do osiągnięcia korzyści efektywności prostoty i koszt.

S3 HD umożliwia wielu indeksy mała, aby zapewniać w obszarze Zarządzanie usługi wyszukiwania pojedynczego przez trading możliwość skalowania indeksów przy użyciu partycje dla funkcji do obsługi więcej indeksów w jednej usługi.

Praktyce usługi S3 może mieć między 1 a 200 indeksy, które razem może udostępniać dokumenty do 1.4 miliardów. HD S3 z drugiej strony umożliwia pojedyncze indeksy, aby przejść tylko 1 milion dokumentów, ale może obsługiwać do 1000 indeksów na partition (maksymalnie 3000 dla danej usługi) dokumentu całkowita liczba 200 milionów na partition (maksymalnie 600 mln dla danej usługi).



## <a name="considerations-for-multitenant-applications"></a>Zagadnienia związane z multitenant aplikacji
Aplikacje multitenant musi skuteczne rozpowszechnianie zasobów między dzierżawami przy zachowaniu poziom prywatności między różnymi dzierżaw. Istnieje kilka zagadnienia podczas projektowania architektury aplikacji pakietu:

* _Dzierżawa izolacji:_ Deweloperzy aplikacji muszą podjęcie właściwych środków, aby upewnić się, że nie dzierżaw autoryzacji lub niechciane dostęp do danych innych dzierżaw. Poza perspektywy prywatności danych dzierżawy izolacji strategii wymagają efektywne zarządzanie udostępnionych oraz ochrony przed zakłócenia sąsiadów.
* _Koszt zasobu w chmurze:_ Podobnie jak w przypadku dowolnej innej aplikacji, oprogramowanie musi być koszt konkurencyjności jako część aplikacji multitenant.
* _Ułatwienia operacji:_ Podczas tworzenia architekturę multitenant, ich wpływu na operacje i złożoność aplikacji jest ważne. Wyszukiwanie Azure ma [SLA 99,9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).
* _Za globalnej:_ Multitenant aplikacji może być konieczne skuteczne służyć dzierżaw, które są rozmieszczone na całym świecie.
* _Skalowalność:_ Deweloperów aplikacji należy rozważyć sposób Uzgodnienie między utrzymanie wystarczająco niski poziom złożoności aplikacji i projektowanie aplikacji przeskalować liczbę dzierżaw i rozmiar danych i obciążenie pracą dzierżaw.

Wyszukiwanie Azure oferuje kilka ograniczenia, które mogą być używane w celu wyodrębnienia danych i obciążenie pracą dzierżaw.

## <a name="modeling-multitenancy-with-azure-search"></a>Modelowanie multitenancy za pomocą wyszukiwania Azure
W przypadku scenariusza multitenant Deweloper aplikacji korzysta z co najmniej jednej usługi wyszukiwania i dzielenie ich dzierżaw między usługami i indeksy. Azure wyszukiwania zawiera kilka typowych wzorców podczas modelowania multitenant scenariusza:

1. _Indeks na dzierżawcę:_ Każdy dzierżawy ma własny indeks w usłudze wyszukiwania, która ma być udostępniana innym dzierżaw.
1. _Usługi na dzierżawcę:_ Każdego dzierżawy ma własny dedykowane usługi Azure wyszukiwania, oferowanie najwyższego poziomu danych i obciążenie pracą odstęp.
1. _Mieszanych:_ Dzierżaw większych, bardziej aktywne są przypisywane dedykowane usług podczas mniejszych dzierżaw są przypisane poszczególne indeksy poziomu usług udostępnionych.

## <a name="1-index-per-tenant"></a>1. indeksu na dzierżawcę
![Wizerunku modelu indeksu dla dzierżawy](./media/search-modeling-multitenant-saas-applications/azure-search-index-per-tenant.png)

W modelu indeksu dla dzierżawy kilka dzierżaw zajmować pojedynczą usługą Azure wyszukiwania, w których każdy dzierżawy nie własne indeksu.

Dzierżaw osiągnąć izolacji danych, ponieważ wszystkie wyszukiwania żądania i operacje dokumentu są wydawane na poziomie indeksu wyszukiwania Azure. W warstwie aplikacji ma świadomość potrzebna w celu skierowania ruchu dzierżaw różnych indeksów pisane z wielkiej litery podczas także zarządzanie zasobami na poziomie usługi wszystkich dzierżaw.

Kluczowym atrybutem model indeksu dla dzierżawy jest możliwość przez dewelopera aplikacji oversubscribe wydajność usługi wyszukiwania między dzierżawami aplikacji. Jeśli dzierżaw nierówne dystrybucji obciążenia, optymalnego połączenia dzierżaw może zostać rozłożone na indeksy usługi wyszukiwania, aby zezwalały na liczbę dzierżaw wysoce aktywne, zasobów podczas jednocześnie obsługujące długie końcu mniej aktywnych dzierżaw. Rozwiązanie jest Brak modelu sobie z sytuacjami, w której każdy dzierżawy jednocześnie jest wysoce aktywna.

Model indeksu dla dzierżawy stanowi podstawę dla modelu kosztu zmiennego miejsce, w którym jest kupiony zaplanowania całej usługi Azure wyszukiwania, a następnie uzupełnić z dzierżawami. Dzięki temu nieużywane zdolności w celu oznaczenia prób i bezpłatnego konta.

W przypadku aplikacji z globalnej za modelu indeksu dla dzierżawy może nie być najbardziej skuteczne. Jeśli aplikacja dzierżaw są rozpowszechniane na całym świecie, oddzielna usługa może być konieczne dla każdego regionu, który zduplikować kosztów w każdej z nich.

Wyszukiwanie Azure umożliwia skali poszczególnych indeksy i łączna liczba indeksów powiększanie. Jeśli odpowiedni cennik warstwa jest wybierany, partycje i repliki można dodać do całej usługę po poszczególnych indeksu w usłudze powiększa się zbyt duża w odniesieniu do miejsca do magazynowania lub ruchu.

Jeśli łączna liczba indeksów rozwoju zbyt duża dla jednej usługi innej usługi musi być tak, aby zezwalały na nowych dzierżaw. Jeśli masz indeksy mają zostać przeniesione między usługami wyszukiwania podczas dodawania nowych usług, dane z indeksu ma można ręcznie skopiować z jednego indeksu do innej Azure wyszukiwania nie zezwala na indeksu mają zostać przeniesione.


## <a name="2-service-per-tenant"></a>2. Usługa na dzierżawcę
![Wizerunku modelu usługi na dzierżawcę](./media/search-modeling-multitenant-saas-applications/azure-search-service-per-tenant.png)

W architekturze usługi dla dzierżawy każdego dzierżawy ma własny usługi wyszukiwania.

W tym modelu aplikacja uzyskuje maksymalną stopnia izolacji dla jego dzierżaw. Każdej usługi ma zarezerwowane miejsca do magazynowania i przepustowość obsługi żądania wyszukiwania, a także osobne klucze interfejsu API.

Dla aplikacji, której każdego dzierżawy jest za duży lub Obciążenie pracą jest nieco zmienności dzierżawy dzierżawy model usługi dla dzierżawy jest rzeczywistą możliwość wyboru zasobów nie są udostępnione przez różne dzierżaw obciążenia.

Usługa na model dzierżawy również oferuje zaletą modelu przewidywalne, stały koszt. Dopóki zostanie dzierżawy wypełnij go, jednak koszt na na dzierżawy jest większa niż model indeksu dla dzierżawy jest nie zaplanowania inwestycji w usłudze całego wyszukiwania.

Model usługi dla dzierżawy jest wydajność wybór dla aplikacji z globalnej za. Z dzierżawami dystrybucja geograficznie łatwiej usługę każdego dzierżawy w odpowiedni region.

Wyzwania w skalowania tego wzorca pojawiają się po poszczególnych dzierżaw pokonać usługi. Azure wyszukiwania nie obsługuje obecnie uaktualniania warstwie cennik usługi wyszukiwania, aby wszystkie dane musi być ręcznie skopiowane do nowej usługi.

## <a name="3-mixing-both-models"></a>3. mieszanie obu modeli
Inny wzór modelowania multitenancy jest mieszanie strategii zarówno indeksu dla dzierżawy, jak i usługi dla dzierżawy.

Przez mieszanie dwa wzorce, dzierżaw największa aplikacji może być dedykowane usług podczas długich końcu dzierżaw mniej aktywnego, mniejszych może być indeksy w usług udostępnionych. Ten model gwarantuje, że największa dzierżaw spójne wysokiej wydajności z usługi podczas ochrona dzierżaw mniejszy od dowolnego zakłócenia sąsiadów.

Jednak wykonania tej strategii korzysta prognozowania w prognozowaniu, które dzierżaw wymaga usługi dedykowane a indeksu w usług udostępnionych. Zwiększa złożoność aplikacji z potrzebą zarządzania obu tych multitenancy modeli.

## <a name="achieving-even-finer-granularity"></a>Osiągnięcia jeszcze bardziej szczegółowy
Powyższe wzorców projektu modelu multitenant scenariuszy w wyszukiwaniu Azure przyjęto założenie, jednolitego zakresu, gdzie każdy dzierżawy będzie całego wystąpienie aplikacji. Aplikacje można jednak czasami obsługują wiele mniejszych zakresach.

Jeśli modele usługi dla dzierżawy i indeksu dla dzierżawy nie są wystarczająco małych zakresów, jest możliwe modelowanie indeksu w celu uzyskania jeszcze dokładniejszą szczegółowości.

Aby indeks zachowywać się inaczej dla punktów końcowych innego klienta, można dodawać pola do indeksu, który wyznacza konkretną wartość dla każdego klienta możliwe. Za każdym razem klient nawiąże połączenie z wyszukiwania Azure lub modyfikowanie indeksu, kod z aplikacją kliencką określa odpowiednią wartość dla tego pola za pomocą możliwości [filtrowania](https://msdn.microsoft.com/library/azure/dn798921.aspx) wyszukiwania Azure w czasie kwerendy.

Ta metoda pozwala uzyskać funkcjonalność osobne konta użytkowników, osobne poziomy uprawnień i nawet całkowicie oddzielić aplikacji.

## <a name="next-steps"></a>Następne kroki
Wyszukiwanie Azure jest ciekawe wybór dla wielu aplikacji, [Dowiedz się więcej o zaawansowanych funkcji usługi](http://aka.ms/whatisazsearch). Podczas obliczania różnych wzorców projektu w zastosowaniach multitenant, należy rozważyć [różne warstwy cennik](https://azure.microsoft.com/pricing/details/search/) i odpowiednich [limity dotyczące usługi](search-limits-quotas-capacity.md) najlepsze dostosować wyszukiwanie Azure, aby dopasować aplikacji i architektury wszystkich rozmiarów do.

Wszelkie pytania dotyczące wyszukiwania Azure i scenariusze multitenant może prowadzić do azuresearch_contact@microsoft.com.
