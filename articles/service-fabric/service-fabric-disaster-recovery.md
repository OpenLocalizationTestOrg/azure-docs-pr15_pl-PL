<properties
   pageTitle="Azure awarii usługi tkaninie | Microsoft Azure"
   description="Azure tkaninie usługa oferuje możliwości należy postępować z wszystkich typów awarii. W tym artykule opisano typy awarii, które mogą wystąpić i jak postępować z nimi."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="seanmck"/>

# <a name="disaster-recovery-in-azure-service-fabric"></a>Odzyskiwanie na tkaninie usługi Azure

Kluczową przedstawiania aplikacji chmury wysokiej dostępności jest zapewnienie, że mogą wtedy pozostać różne rodzaje błędów, łącznie z tymi, które są poza pilota. W tym artykule opisano fizyczny układ klaster tkaninie usługi Azure w kontekście potencjalnych awarii i zawiera wskazówki dotyczące jak radzić sobie z takich awarii ograniczyć lub wyeliminować ryzyko utraty przestoje lub danych.

## <a name="physical-layout-of-service-fabric-clusters-in-azure"></a>Fizyczny układ tkaninie usługi klastrów platformy Azure

Aby zrozumieć zagrożenie powodowane przez różne typy błędów, warto wiedzieć, jak klastrów są fizycznie ułożone w Azure.

Po utworzeniu klastrze tkaninie usługi platformy Azure należy wybrać region, w którym hostowana. Następnie Azure infrastruktury Inicjuje obsługę administracyjną zasoby dla tego klaster w obszarze głównie liczbę wirtualnych maszyn wymagane. Przyjrzyjmy się lepiej jak i gdzie te maszyny wirtualne zainicjowano obsługę administracyjną.

### <a name="fault-domains"></a>Błąd domen

Domyślnie maszyny wirtualne w klastrze są równomiernie rozmieszczony logicznych grupach nazywanych domenami błędów (FDs), które segmentu maszyn oparte na potencjalne błędy sprzętu hosta. W szczególności jeśli dwóch maszyny wirtualne znajdują się w dwóch różnych FDs, może być upewnić się, że nie mają sam przełącznik źródła lub sieci power. W wyniku sieci lokalnej lub uszkodzenia power jeden maszyn wirtualnych nie wpływa na inne, umożliwiając tkaninie usługi Obciążenie pracy nie odpowiadać komputera w klastrze.

Można wyświetlać wizualizację układu klaster w domenach błędów za pomocą mapy klaster w [Eksploratorze tkaninie usługi](service-fabric-visualizing-your-cluster.md):

![Węzły rozciągnąć domen błędów w Eksploratorze tkaninie usługi][sfx-cluster-map]

>[AZURE.NOTE] Inne osi na mapie klaster zawiera uaktualnienia domen, które logicznie zgrupować węzłów na podstawie informacji o planowanej konserwacji działania. Usługa tkaninie klastrów platformy Azure zawsze rozłożenia pięć domen uaktualnienia.

### <a name="geographic-distribution"></a>Rozmieszczenie geograficzne

Obecnie są [26 Azure regionów świata][azure-regions], z kilkoma więcej ogłoszenia. Poszczególne region może zawierać co najmniej jeden centrach danych fizycznie w zależności od żądanie i dostępność w odpowiednich miejscach, między innymi czynnikami. Zauważ, że nawet w regiony, które zawierają wiele centrach danych fizycznie, jest gwarantowana, że maszyny wirtualne usługi Klaster zostaną równomiernie rozmieszczone w tych miejscach. W rzeczywistości obecnie wszystkie maszyny wirtualne dla danego klaster zainicjowano obsługę administracyjną w ramach jednej witryny fizycznym.

## <a name="dealing-with-failures"></a>Sposób postępowania z błędów

Istnieje kilka typów błędów, które mogą mieć wpływ klaster, każda z własną łagodzenia. Firma Microsoft będzie wyglądać ich w celu prawdopodobieństwo wystąpienia.

### <a name="individual-machine-failures"></a>Błędy poszczególnych komputera

Jak wspomniano, komputer poszczególnych błędów, maszyn wirtualnych albo w sprzęt i oprogramowanie, w którym w danej domenie błędów stanowić ryzyko samodzielnie. Usługa będzie zwykle wykryć awarię w ciągu kilku sekund i odpowiadanie odpowiednio na podstawie stanu klaster. Na przykład jeśli węzeł został hostingu podstawowego replik partycją nowy podstawowy jest wybierany z częściowych pomocniczą. Jeśli Azure powoduje komputera nie powiodło się wykonywanie kopii zapasowej, go będą automatycznie ponownie dołączyć klaster i ponownie zastosować udziału obciążenie pracą.

### <a name="multiple-concurrent-machine-failures"></a>Wiele błędów równoczesne komputera

Podczas domen błędów znacznie zmniejszyć ryzyko błędy równoczesne komputera, że zawsze będzie możliwości wielu błędy losowe może spowodować przerwanie kilka maszyn w klastrze jednocześnie.

Ogólnie jak większość węzłów pozostają dostępne, klaster będzie działać, chociaż w niższej zdolności stanowe repliki uzyskiwanie spakowany do mniejszy zestaw maszyn i mniejszej liczby wystąpień stateless są dostępne rozsunąć ładowania.

#### <a name="quorum-loss"></a>Utrata kworum

Jeśli większość replik dla usługi stanowe partition przejść w dół, niej wprowadza Państwie znana pod nazwą "kworum strata". W tym momencie tkaninie usługi zatrzymuje umożliwiające zapis do tego partition, aby upewnić się, że stanu pozostaje spójne i niezawodne. Firma Microsoft w celu wybór Zaakceptuj w okresie niedostępności, aby upewnić się, że klienci nie poinformowany, że dane o jego został zapisany, gdy w rzeczywistości nie. Należy zauważyć, że wybrano w jednocześnie odczytuje z pomocniczej repliki w tej usłudze stanowe, można wykonać te operacje znajduje się w tym trybie odczytu. Partycją pozostaje utratę kworum do momentu wystarczającej liczby replik wróci lub administrator klastrów wymusza systemu przejść za pomocą [Interfejsu API napraw ServiceFabricPartition][repair-partition-ps].

>[AZURE.WARNING] Wykonywanie akcji naprawy podczas podstawowego replice nie działa spowoduje utratę danych.

Usługi systemowe także mogą występować utratę kworum efektownych są specyficzne dla danej usługi. Na przykład utrata kworum w usłudze nazw będzie miało wpływ rozpoznawanie nazw, dlatego utracie kworum w usłudze Menedżer pracy awaryjnej blokuje tworzenia nowej usługi i praca awaryjna. Zauważ, że w przeciwieństwie do własnej usługi próby naprawienia usługi systemowe jest *niezalecane* . Zamiast tego zaleca się po prostu poczekaj na dół repliki zwracana.

#### <a name="minimizing-the-risk-of-quorum-loss"></a>Minimalizowanie ryzyka utraty kworum

Ryzyko utraty kworum można zminimalizować, zwiększając rozmiar zestawu replice docelowej tej usługi. Warto traktować liczba replik należy pod względem liczby węzłów niedostępne, który będzie odporna na po pozostając dostępne dla zapisu, pamiętając pamiętać tej aplikacji lub uaktualnienia klaster pozwalające węzły tymczasowo niedostępny, oprócz awarie sprzętu.

Rozważmy następujące przykłady przy założeniu skonfigurowano usługi mają MinReplicaSetSize trzech od najmniejszej liczby zalecane dla usług produkcji. Z TargetReplicaSetSize trzech (jeden podstawowy i dwóch pomocnicze), błąd sprzętowy podczas uaktualniania (dwie repliki w dół) spowoduje utratę kworum a usługą będzie tylko do odczytu. Także mając pięć repliki, może być wytrzymać dwóch błędy podczas uaktualniania (trzy replik w dół), jak pozostałe dwie repliki nadal można tworzyć kworum w zestawie minimalne.

### <a name="data-center-outages-or-destruction"></a>Dostawie centrum danych lub zniszczenia

Czasami centrach danych fizycznie może być tymczasowo niedostępne z powodu utrata łączności power lub sieci. W takich przypadkach do klastrów tkaninie usług i aplikacji również będą niedostępne, ale dane zostaną zachowane. W przypadku klastrów działa Azure umożliwia wyświetlanie aktualizacji na dostawie na [stronie Stan Azure][azure-status-dashboard].

W przypadku bardzo mało prawdopodobne, że centrum danych fizycznie jest niszczony dowolnego klastrów tkaninie usługi hostowanej zostaną utracone, wraz z ich stanu.

Do ochrony przed taką możliwość, jest bardzo ważne aby okresowo [Kopia zapasowa swój stan](service-fabric-reliable-services-backup-restore.md) do sklepu zbędne geo i upewnij się, że jest sprawdzana poprawność możliwość go przywrócić. Jak często wykonywanie kopii zapasowej będą zależne od usługi odzyskiwania punktów cel (RPO). Nawet jeśli użytkownik nie pełni wprowadziły kopia zapasowa i przywracanie jeszcze, wykonała obsługi dla `OnDataLoss` zdarzenia tak, aby można rejestrować wystąpieniu jako następujący:

```c#
protected virtual Task<bool> OnDataLoss(CancellationToken cancellationToken)
{
  ServiceEventSource.Current.ServiceMessage(this, "OnDataLoss event received.");
  return Task.FromResult(true);
}
```


### <a name="software-failures-and-other-sources-of-data-loss"></a>Błędy oprogramowania i innych źródeł utratą danych

Przyczyna utraty danych wady kod usług, ludzi ewentualne błędy i naruszenia bezpieczeństwa są najczęściej niż błędy centrum danych szerokie. Jednak we wszystkich przypadkach strategii odzyskiwania jest taki sam: sporządzanie regularnego wykonywania kopii zapasowych wszystkich usług stanowe i wykonywać możliwości przywrócenia tego stanu.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak symulacji różnych awarii przy użyciu [struktury pola](service-fabric-testability-overview.md)
- W tym artykule innych odzyskiwania i wysokiej dostępności zasobów. Microsoft opublikowała dużej ilości wskazówki dotyczące tych tematów. Gdy niektóre z tych dokumentów odwołać się do określonych technik do użytku w innych programach, zawierają wiele ogólne najważniejsze wskazówki, które można stosować w kontekście tkaninie usługi także:
 - [Dostępność — Lista kontrolna](../best-practices-availability-checklist.md)
 - [Wykonywanie rozwijania odzyskiwania po awarii](../sql-database/sql-database-disaster-recovery-drills.md)
 - [Odzyskiwanie i wysoką dostępność dla aplikacji Azure][dr-ha-guide]


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
