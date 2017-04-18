<properties
   pageTitle="Rozpowszechnianie danych globalnie DocumentDB | Microsoft Azure"
   description="Informacje na temat odzyskiwania geo replikacji, pracy awaryjnej i danych planety skali przy użyciu globalnej baz danych z DocumentDB Azure, w pełni zarządzane usługi bazy danych NoSQL."
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="kipandya"/>
   
   
# <a name="distribute-data-globally-with-documentdb"></a>Rozpowszechnianie danych globalnie za pomocą DocumentDB

> [AZURE.NOTE] Globalne rozkład DocumentDB baz danych jest zazwyczaj dostępny i automatycznie włączone dla wszystkich nowo utworzonych kont DocumentDB. Pracujemy nad Włącz globalnej rozkład na wszystkich istniejących kont, ale w międzyczasie ma być globalnym rozkładu włączone dla swojego konta, należy [kontakt z pomocą techniczną](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) i firma Microsoft będzie ją włączyć można teraz.

Azure DocumentDB jest przeznaczony do potrzeb aplikacji IoT składający się z miliony globalnie rozłożone urządzeń i aplikacji skali internet, które dostarczają środowiska wysoce odpowiada użytkownikom całym świecie. Poniższe systemy baz danych buźkę największym wyzwaniem osiągnięcia krótki czas oczekiwania dostęp do danych aplikacji z wielu regionach geograficznych gwarancje spójności i dostępność przejrzyste danych. Jako globalnie Rozproszony system bazy danych DocumentDB upraszcza globalnej rozkład danych oferując konta w pełni zarządzane, w przypadku bazy danych, które zapewniają wyczyść kompromisów między spójności, dostępności i wydajności, wszystkie odpowiednie gwarancje. Konta DocumentDB bazy danych są dostępne z wysokiej dostępności, jedna cyfra ms opóźnienia, wielu [poziomów przejrzyste spójności] [consistency], przejrzystego awaryjnego regionalne z wielu interfejsów API i możliwość skalowania elastically przepustowość i miejsca do magazynowania na całym świecie. 

  
## <a name="configuring-multi-region-accounts"></a>Konfigurowanie kont wielu regionu

Konfigurowanie konta DocumentDB przeskalować na całym świecie można wykonywać w ciągu mniej niż minuty przez [Azure portal](documentdb-portal-global-replication.md). To wszystko, co należy zrobić, wybierz poziom prawo spójności między kilka poziomów obsługiwane spójności przejrzyste, a skojarzyć dowolną liczbę regionów Azure za pomocą konta bazy danych. Poziomy spójności DocumentDB zapewniają wyczyść kompromisów między gwarancji określonych spójności i wydajności. 

![Dobrze DocumentDB oferuje wiele określona modeli spójności (obniżone) do wyboru][1]

Dobrze DocumentDB oferuje wiele określona modeli spójności (obniżone) do wyboru.

Wybieranie poziomu spójności prawo zależy od danych spójności zagwarantować potrzebami aplikacji. DocumentDB automatycznie replikuje dane w wszystkich regionów określonych i gwarantuje spójność, wybranej dla Twojego konta bazy danych. 


## <a name="using-multi-region-failover"></a>Przy użyciu wielu region pracy awaryjnej 

Azure DocumentDB jest w stanie przezroczysty pracy awaryjnej konta bazy danych w wielu regionów Azure — nowe [wielu interfejsów API] [ developingwithmultipleregions] gwarancji, że aplikacji nadal korzystać logiczne punktu końcowego i nieprzerwanie przy tym przełączeniu. Pracy awaryjnej jest kontrolowane przez użytkownika, zapewniające możliwość rehome bazy danych kont w przypadku dowolnej z zakresu warunków możliwe niepowodzenie występuje, łącznie z aplikacji, infrastruktura, usługa lub regionalnych błędy (rzeczywistą lub symulowany). W przypadku awarii regionalne DocumentDB usługę przezroczysty powiedzie się na koncie bazy danych, a aplikacja będzie nadal występował uzyskać dostęp do danych bez utraty dostępności. Podczas DocumentDB oferuje [99,99% dostępności zwiększany][sla], możesz przetestować aplikacji end, aby właściwości punktu końcowego dostępności obu [programowy] imitację uszkodzenia regionalne[ arm] oraz jak i Azure Portal.


## <a name="scaling-across-the-planet"></a>Skalowanie na świecie
DocumentDB umożliwia niezależne obsługi administracyjnej przepustowość i używanie miejsca do magazynowania dla każdego zbioru DocumentDB w dowolnym skali globalnie przez wszystkich regionów skojarzonego z kontem bazy danych. Kolekcja DocumentDB jest automatycznie distributed globalnie i zarządzane we wszystkich regionach skojarzonego z kontem bazy danych. Kolekcje w ramach konta bazy danych może zostać rozłożone na dowolny Azure regionów, w których [Usługa DocumentDB jest dostępna][serviceregions]. 

Przepustowość zakupionych i miejsca do magazynowania zużyte dla każdego zbioru DocumentDB jest automatycznie obsługi administracyjnej przez wszystkich regionów równomiernie. Dzięki aplikacjom bezproblemowo skalowanie przez kuli ziemskiej [opłaty tylko w przypadku przepustowość i przestrzeni dyskowej, używanym w każdej godziny][pricing]. Na przykład jeśli masz obsługi administracyjnej RUs 2 milionów kolekcji DocumentDB, następnie każdego z regionów skojarzonego z kontem bazy danych otrzymuje RUs 2 milionów dla tej kolekcji. Poniżej przedstawiono to.

![Skalowanie przepustowości dla zbioru DocumentDB na cztery obszary][2]

DocumentDB gwarantuje < 10 ms i < 15 ms zapis opóźnienia w P99. Żądania odczytu nigdy nie obejmują obramowanie centrum danych zagwarantowanie [wymagania dotyczące zgodności, który wybrano][consistency]. Zapisy są zawsze kworum Projekt zatwierdzony lokalnie przed uznanych do klientów. Każde konto bazy danych jest konfigurowana zapisu region Priority (priorytet). Region oznaczone najwyższy priorytet będą działać jako bieżący obszar zapisu dla konta. Wszystkie SDK będzie przezroczysty rozesłać zapisy konta bazy danych do bieżącego obszaru zapisu. 

Na koniec ponieważ DocumentDB jest całkowicie [niezależne od schematu] [ vldb] — nie musisz się martwić, zarządzanie aktualizowanie schematy lub indeksów pomocniczych w wielu centrach danych. [Zapytania SQL] [ sqlqueries] kontynuować pracę podczas twojej aplikacji i modele danych w dalszym ciągu są obsługiwane. 


## <a name="enabling-global-distribution"></a>Włączanie globalnej rozkładu 

Możesz zdecydować dane lokalnie lub globalnie distributed albo kojarząc jednego lub kilku regionów Azure z DocumentDB bazy danych konta. Można dodać lub usunąć regionów do swojego konta bazy danych w dowolnym momencie. Aby włączyć globalnego rozkładu za pomocą portalu, zobacz [jak przeprowadzić DocumentDB replikacji globalnej bazy danych za pomocą portalu Azure](documentdb-portal-global-replication.md). Aby włączyć globalnego rozkładu programowo, zobacz [rozwijających się z kontami DocumentDB wielu region](documentdb-developing-with-multiple-regions.md).

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat dystrybucji danych globalnie z DocumentDB w następujących artykułach:

* [Obsługa administracyjna przepustowość i miejsca do magazynowania dla zbioru] [throughputandstorage]
* [Wielu API za pośrednictwem pozostałych. .NET, języka Java, Python i SDK węzeł] [developingwithmultipleregions]
* [Poziomy spójności w DocumentDB] [consistency]
* [Dostępność poziomu] [sla]
* [Zarządzanie kontami bazy danych] [manageaccount]

[1]: ./media/documentdb-distribute-data-globally/consistency-tradeoffs.png
[2]: ./media/documentdb-distribute-data-globally/collection-regions.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[pcolls]: documentdb-partition-data.md
[consistency]: documentdb-consistency-levels.md
[consistencytradeooffs]: ./documentdb-consistency-levels/#consistency-levels-and-tradeoffs
[developingwithmultipleregions]: documentdb-developing-with-multiple-regions.md
[createaccount]: documentdb-create-account.md
[manageaccount]: documentdb-manage-account.md
[manageaccount-consistency]: documentdb-manage-account.md#consistency
[throughputandstorage]: documentdb-manage.md
[arm]: documentdb-automation-resource-manager-cli.md
[regions]: https://azure.microsoft.com/regions/
[serviceregions]: https://azure.microsoft.com/en-us/regions/#services 
[pricing]: https://azure.microsoft.com/pricing/details/documentdb/
[sla]: https://azure.microsoft.com/support/legal/sla/documentdb/ 
[vldb]: http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf
[sqlqueries]: documentdb-sql-query.md

