<properties 
    pageTitle="Azure zagadnienia wydajności i optymalizacji wyszukiwania | Microsoft Azure" 
    description="Dostosowywanie wydajności wyszukiwania Azure i skonfigurować optymalnego skali" 
    services="search" 
    documentationCenter="" 
    authors="LiamCavanagh" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="liamca"/>

# <a name="azure-search-performance-and-optimization-considerations"></a>Azure zagadnienia wydajności i optymalizacji wyszukiwania

Doskonałe przeszukiwanie jest klucz do sukcesu dla wielu urządzeń przenośnych i aplikacji sieci web. Z nieruchomości, używane rynków samochodu do katalogi online szybkiego wyszukiwania odpowiednie wyniki mają wpływ i obsługi klienta. Ten dokument jest przeznaczony do pomocy wykrywania obsługi najważniejsze wskazówki dotyczące jak najlepiej wykorzystać wyszukiwania Azure, w szczególności dla zaawansowanych scenariuszy z zaawansowanych wymagania dotyczące skalowalność, wielojęzycznej lub niestandardowe klasyfikowania.  Ponadto ten dokument zawiera wewnętrzne i obejmuje metod, które działają wydajnie w aplikacjach rzeczywistych klientów.

## <a name="performance-and-scale-tuning-for-search-services"></a>Wydajność i dostosowywanie skali dla usług wyszukiwania

Firma Microsoft używane wyszukiwarki, takich jak Bing i Google i Wysoka wydajność, jakie oferują.  Dzięki temu podczas klientów za pomocą wyszukiwania sieci web lub aplikacji mobilnej, będzie oczekiwać podobne cechy.  Podczas optymalizowania wydajności wyszukiwania, jedną z metod zalecane jest skoncentrowanie się na opóźnienie, czyli czas kwerendy Kończenie i zwrócić wyniki.  Optymalizowanie pod kątem opóźnienie wyszukiwania jest ważne:

1. Wybierz opóźnienie docelowej (lub maksymalnej ilości czasu), żądanie typowe wyszukiwania należy wykonać, aby ukończyć.

2. Tworzenie i testowanie obciążenie pracą rzeczywistą przed usługi wyszukiwania z zestawu rzeczywistych danych do pomiaru tych stawek opóźnienie.

3. Rozpoczynanie z najmniejszą liczbę kwerend na sekundę (QPS) i Kontynuuj zwiększenie liczby wykonywane w teście, dopóki nie opóźnienie kwerend mniejsza niż opóźnienie zdefiniowanej wartości docelowej.  Jest to ważne testu ułatwiające zaplanowanie skala w miarę aplikacji użycia w.

4. Jeśli to możliwe, ponowne używanie połączeń HTTP.  Jeśli korzystasz z platformy Azure wyszukiwania .NET SDK, oznacza to, należy użyć ponownie wystąpienie lub wystąpienia [SearchIndexClient](https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchindexclient.aspx) i korzystając z interfejsu API usługi REST, można ponownie użyć pojedynczej HttpClient.
 
Podczas tworzenia tych testowanie obciążenia, istnieje kilka cech wyszukiwania Azure należy pamiętać:

1. Użytkownik może wypychanych tak wiele wyszukiwania kwerendy w tym samym czasie, że będzie przerasta zasobów dostępnych w usłudze Azure wyszukiwania.  W takim przypadku zostanie wyświetlony kody odpowiedzi HTTP 503.  Z tego powodu najlepiej rozpocząć z różnych zakresów wyszukiwania żądania Zobacz różnice w kursach opóźnienie, po dodaniu więcej żądania wyszukiwania.

2. Przekazywanie zawartości do wyszukiwania Azure będzie mieć wpływ na ogólną wydajność i opóźnienie usługi Azure wyszukiwania.  Jeśli chcesz wysłać danych, gdy użytkownicy wykonują wyszukiwania jest sporządzanie obciążenie pod uwagę testy;

3. Nie wszystkie kwerendy wyszukiwania będą działać na tym samym poziomie wydajności.  Na przykład dokument sugestie wyszukiwania lub wyszukać zwykle działa szybciej niż kwerenda z znaczącej liczby aspekty i filtry.  Warto wykonać różne kwerend, które powinny być widoczne pod uwagę podczas tworzenia testy.  

4. Odmiany żądania wyszukiwania jest ważne, ponieważ jeśli ciągłe wykonać takie same żądania wyszukiwania, pamięci podręcznej danych rozpocznie się wprowadzić wydajności lepsze, nie może za pomocą kwerendy więcej różnych ustawiana.

> [AZURE.NOTE] [Testowanie programu Visual Studio ładowania](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) dobrze umożliwia wykonywanie testów wydajności usługi jako umożliwia wykonywanie żądania HTTP, potrzebne do wykonywania kwerend dla wyszukiwania Azure i umożliwia parallelization żądania.

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Skalowanie Azure wyszukiwanie stawki wysoka kwerendy i ograniczenie żądania

Kiedy otrzymujesz zbyt wiele żądań ograniczonej lub przekracza ceny opóźnienie docelowej z ładowania zapytań lepszą, można wyszukać zmniejszanie stawki opóźnienie w jeden z dwóch sposobów:

1. **Zwiększanie repliki:**  Replice przypomina kopię danych umożliwiające wyszukiwanie Azure, aby załadować żądania saldo do wielu kopii.  Wszystkie równoważenia obciążenia i replikacji danych między replikami zarządza Azure wyszukiwania można zmienić liczbę repliki przydzielonego usługi w dowolnym momencie.  Można przydzielić do 12 repliki w usłudze wyszukiwanie standardowe i 3 replikami usługi podstawowego wyszukiwania.  [Azure Portal](search-create-service-portal.md) lub za pomocą [interfejsu API Menedżera Azure wyszukiwania](search-get-started-management-api.md)można dostosować repliki.

2. **Zwiększanie poziomu wyszukiwania:**  Azure wyszukiwania zawiera [liczbę poziomów](https://azure.microsoft.com/pricing/details/search/) z każdego z tych poziomów oferuje różne poziomy wydajności.  W niektórych przypadkach może być tak dużo kwerend warstwie, które znajdują się na nie może dostarczyć stawek wystarczająco krótki czas oczekiwania, nawet wtedy, gdy repliki są wyczerpane.  W tym przypadku warto rozważyć używanie jednej z wyższych warstw wyszukiwania takich jak warstwie Azure wyszukiwania S3, który jest odpowiedni dla scenariuszy z dużej liczby dokumentów i obciążenia kwerendy bardzo wysoki.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Skalowanie Azure wyszukiwanie działa wolno poszczególnych kwerend

Innym powodem Dlaczego stawki opóźnienie mogą być powolne pochodzi z pojedynczą kwerendę trwa zbyt długo.  W tym przypadku Dodawanie repliki nie spowoduje zwiększenie stawki opóźnienie.  W tym przypadku dostępne są dwie opcje:

1. **Zwiększanie partycje** Partycją jest mechanizm dzielenia danych przez dodatkowe zasoby.  Z tego powodu po dodaniu partycją drugiego danych pobiera podzielić na dwa.  Trzecia partition dzieli indeksu na trzy itp.  To także ma efekt, że w niektórych przypadkach powolne kwerendy działa szybciej z powodu parallelization obliczeń.  Istnieje kilka przykładów miejsce, w którym mają być widoczne ten parallelization szczególnie dobrze przetwarzania kwerend, które mają niski selektywność kwerendy.  Składa się z zapytań pasujących wiele dokumentów lub po faceting należy podać liczniki nad dużej liczby dokumentów.  Ponieważ istnieje wiele obliczeń potrzebne do wyniku trafności dokumentów lub Zliczanie liczby dokumentów, dodając dodatkowe partycje mogą pomóc określić dodatkowe obliczenia.  

   Może to być maksymalnie 12 partycje w usłudze wyszukiwanie standardowe i partycją 1 w usłudze podstawowego wyszukiwania.  [Azure Portal](search-create-service-portal.md) lub za pomocą [interfejsu API Menedżera Azure wyszukiwania](search-get-started-management-api.md)można dostosować partycje.

2. **Ograniczyć pola wysoka kardynalności:** Pole wysoka kardynalności składa się z facetable lub podatne pole, które ma znaczną liczbę unikatowych wartości, a w wyniku zajmuje dużo zasobów, aby obliczyć wyników na.   Na przykład ustawienie pola Identyfikator produktu lub opis jako facetable i podatne spowodowałby dla kardynalności wysoka ponieważ większość wartości z dokumentu do dokumentu są unikatowe. Ile to możliwe, należy ograniczyć liczbę pól kardynalności wysoki.

3. **Zwiększanie poziomu wyszukiwania:**  Przenoszenie do wyższego poziomu Azure wyszukiwania można zwiększyć wydajność powolne kwerend w inny sposób.  Każdy wyższego poziomu udostępnia również szybszy Procesor i więcej pamięci, który może być dodatni wpływ na wydajność kwerendy.

## <a name="scaling-for-availability"></a>Skalowanie dostępności

Replik nie tylko zmniejszyć opóźnienie kwerend, ale również umożliwia wysokiej dostępności.  Z jednym replice możesz się spodziewać okresowe przestoje ze względu na serwerze jest uruchamiany ponownie po aktualizacji oprogramowania lub innych zdarzeń konserwacji, które wystąpi.  W wyniku jest należy rozważyć, czy aplikacja wymaga szybkiej wyszukiwań (zapytanie) oraz zapisu (zdarzenia indeksowania).  Wyszukiwanie Azure oferuje opcje Umowa dotycząca poziomu usług na z ofertą płatnej wyszukiwania z następującymi atrybutami:

- 2 repliki wysokiej dostępności obciążenia tylko do odczytu (zapytanie)
- 3 lub większej liczbie replik wysokiej dostępności obciążenia odczytu i zapisu (kwerendy i indeksowania)

Aby uzyskać więcej informacji na temat odwiedź stronę [Azure umowy o poziomie usług wyszukiwania](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Ponieważ repliki są kopie danych, o wielu replikach umożliwia wyszukiwanie Azure do komputera w czasie, a jednocześnie kwerend kontynuować na innych replik ponownego uruchamiania i konserwacji wobec jednej replice.  Z tego powodu również należy rozważyć, jak to przestoje mogą mieć wpływ na zapytania, które mają teraz na jednym mniej kopię danych.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Skalowanie geo distributed obciążenia i podaj geo nadmiarowości

Geo distributed obciążenia znajdziesz, że użytkownicy znajduje się daleko od centrum danych, w którym usługi Azure wyszukiwania jest hostowana opóźnienie większe.  Z tego powodu często jest dostęp do wielu usług wyszukiwania w regionach, które znajdują się w zbliża do tych użytkowników.  Wyszukiwanie Azure obecnie zapewnia zautomatyzowana metoda replikacji geo indeksów wyszukiwania Azure między różnymi regionami, ale istnieje kilka technik, których można używać mogących powodować ten proces prosty zaimplementowania i zarządzać nimi. Te zostały opisane w następnej sekcji kilka.

Celem zbiór usług wyszukiwania geo distributed ma mieć co najmniej dwa indeksy dostępne w dwóch lub więcej regionów miejsce, w którym użytkownik będą kierowane do usługa Azure wyszukiwania, która zapewnia opóźnienie najniższych, w tym przykładzie:

   ![Karta między usługami według regionów][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Synchronizacja danych w wielu usługach Azure wyszukiwania

Dostępne są dwie opcje zachowywania rozłożone wyszukiwania usługi synchronizacji które składają się z albo za pomocą [Indeksowanie wyszukiwania Azure](search-indexer-overview.md) lub Push API (nazywane także [Interfejsu API usługi REST wyszukiwania Azure](https://msdn.microsoft.com/library/dn798935.aspx)).  

### <a name="azure-search-indexers"></a>Indeksatory Azure wyszukiwania

Jeśli używasz indeksowanie wyszukiwania Azure już importujesz zmian w danych z centralnego magazynu danych, takich jak bazy danych SQL Azure lub DocumentDB. Po utworzeniu nowego wyszukiwania usługi, możesz po prostu również tworzyć nowe indeksowanie wyszukiwania Azure w tej usłudze, która kieruje do tego samego magazynu danych. W ten sposób po wprowadzenia nowych zmian do magazynu danych one będą następnie indeksowane przez różne indeksatory.  

Oto przykład jak będzie wyglądać danej architektury.

   ![Jedno źródło danych z rozkładem indeksatora i usługi kombinacji][2]


### <a name="push-api"></a>Interfejs API push 
Jeśli korzystasz z API Push Azure wyszukiwania w celu [aktualizacji zawartości w indeksie wyszukiwania Azure](https://msdn.microsoft.com/library/dn798930.aspx), możesz zachować różnych usług wyszukiwania synchronizacji przez naciśnięcie zmiany do wszystkich usług wyszukiwania, gdy jest wymagana aktualizacja.  Spowoduje to należy koniecznie obsługiwać przypadkach, gdy jest aktualizacja usługi jedno wyszukiwanie zakończy się niepowodzeniem i co najmniej jedna aktualizacja powiodła się.

## <a name="leveraging-azure-traffic-manager"></a>Używanie Azure Menedżer ruchu

[Menedżer ruchu Azure](../traffic-manager/traffic-manager-overview.md) pozwala na rozsyłanie żądań do wielu znajduje się geo witryn sieci Web archiwizowana przez wiele usług wyszukiwania Azure.  Jedną z zalet Menedżer ruchu jest, aby można go sondy wyszukiwania Azure upewnij się, że jest dostępna i kierowanie użytkowników do usługi wyszukiwania alternatywnych w przypadku przestoje.  Ponadto jeśli są routingu żądania wyszukiwania do witryny sieci Web Azure, Menedżer ruchu Azure umożliwia ładowania saldo przypadku witryny sieci Web w górę, ale nie Azure wyszukiwania.  Oto przykład jakie architektury, która korzysta Menedżer ruchu.

   ![Krzyżowe — karta usług przez region z centralnej Menedżer ruchu][3]

## <a name="monitoring-performance"></a>Monitorowanie wydajności

Wyszukiwanie Azure umożliwia analizowanie i monitorowanie wydajności usługi za pośrednictwem [Wyszukiwania analizy ruchu (STA)](search-traffic-analytics.md). Za pośrednictwem STA można opcjonalnie rejestrować operacji wyszukiwania poszczególnych, a także zagregowane metryki konto Azure miejsca do magazynowania, które może on być przetwarzania analizy lub zwizualizować w usłudze Power BI.  Metryki STA można przeglądać statystyki dotyczące wydajności, takich jak średnia liczba wystąpień czasy odpowiedzi kwerendy lub kwerend.  Ponadto rejestrowanie operacji umożliwia przechodzenie do szczegółów operacji wyszukiwania określonych szczegółów.

STA jest przydatne narzędzie zrozumienie stawki opóźnienie z tej perspektywy Azure wyszukiwania.  Ponieważ wskaźniki kwerendy, rejestrowane są oparte na czas kwerendy w pełni zostanie przetworzony w wyszukiwaniu Azure (od momentu, gdy jest wymagane, gdy jest wysyłany), jest możliwe do służy do określenia, czy problemy opóźnienie problemy z ze strony usługi Azure wyszukiwania lub poza usługi, takie jak od czasu oczekiwania w sieci.  

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat cennik limity warstwy i usług dla każdego z nich, zobacz [limity dotyczące usługi Azure wyszukiwania](search-limits-quotas-capacity.md).

Odwiedź stronę [planowania pojemności](search-capacity-planning.md) , aby dowiedzieć się więcej o kombinacji partycją i replice.

Aby uzyskać więcej Przechodzenie do szczegółów na wydajność i niektórych pokazy jak wdrażać optymalizacje omawiane w tym artykule klip:

> [AZURE.VIDEO azurecon-2015-azure-search-best-practices-for-web-and-mobile-applications]

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png