Factory danych to usługa wielu dzierżawy, która ma następujące ograniczenie domyślne w miejscu, aby upewnić się, że subskrypcje klienta są chronione przed obciążenia innych osób. Wiele limity można łatwo wzrosnąć dla subskrypcji maksymalnie maksymalnej, kontaktując się z pomocy technicznej. 

**Zasób** | **Domyślny Limit** | **Limit maksymalny**
-------- | ------------- | -------------
fabryki danych w subskrypcji usługi Azure | 50 | [Kontakt z pomocą techniczną](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
procesy w obrębie factory danych | 2500 | [Kontakt z pomocą techniczną](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
zestawy danych w ramach fabrycznych danych | 5000 | [Kontakt z pomocą techniczną](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
równoczesne wycinków na zestawu danych | 10 | 10
bajtów dla każdego obiektu dla obiektów planowana <sup>1</sup> | 200 KB | 2000 KB
bajtów dla każdego obiektu dla zestawu danych i obiektów usługi połączone <sup>1</sup> | 100 KB | 2000 KB
Usługa HDInsight rdzenie klaster na żądanie w ramach subskrypcji <sup>2</sup> | 48 | [Kontakt z pomocą techniczną](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Jednostki przepływu danych <sup>3</sup> w chmurze | 8 | [Kontakt z pomocą techniczną](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
Liczba dla systemem aktywności planowana prób | 1000. | MaxInt (32-bitowy)

<sup>1</sup> potok, zestawu danych i obiektów usługi połączone reprezentuje logiczną grupę z pracą. Limity dotyczące tych obiektów nie dotyczą ilości danych, można przenieść i procesu z usługą Azure danych Factory. Factory danych służy do skalowanie do obsługi petabytes danych.

<sup>2</sup> rdzenie HDInsight na żądanie są przydzielane wylogowywanie się z subskrypcji, która zawiera fabrycznych danych. W wyniku powyżej limit wynosi Factory danych wymuszone core termin rdzenie HDInsight na żądanie i różni się od limitu core skojarzonego z subskrypcją usługi Azure.

<sup>3</sup> jednostki przepływu danych w chmurze (DMU) jest używana w operacji kopii w chmurze w chmurze. Jest miarą, która reprezentuje możliwości jednostką w fabrycznych danych dodatku (kombinacja Procesora, pamięci i alokacji zasobów sieci). Dzięki zastosowaniu więcej DMUs w niektórych scenariuszach dotyczących można uzyskać wyższej przepustowości Kopiuj. Zobacz sekcję [jednostki przepływu danych w chmurze](../../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units) na stronie Szczegóły.

**Zasób** | **Domyślny limit dolnym** | **Minimalne ograniczenie**
-------- | ------------------- | -------------
Planowanie interwału | 15 minut | 15 minut
Interwał między próbami ponów próbę | 1 sekunda | 1 sekunda
Spróbuj ponownie wartość limitu czasu | 1 sekunda | 1 sekunda


### <a name="web-service-call-limits"></a>Limity połączeń usługi sieci Web

Azure Menedżera zasobów zawiera limity dotyczące interfejsu API. Możesz nawiązywać połączenia interfejsu API stawki w [limity Azure API Menedżera zasobów](../azure-subscription-service-limits.md#resource-group-limits). 


