<properties
   pageTitle="Wdrażanie wirtualnego urządzenia w wysokiej dostępności | Microsoft Azure"
   description="Jak wdrożyć usługę urządzenia wirtualna sieć w wysokiej dostępności."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>Wdrażanie wirtualnego urządzenia w wysokiej dostępności

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

W tym artykule omówiono zestaw wskazówki dotyczące wdrażania urządzeń wirtualnych sieci (NVAs) w sposób wysokiej dostępności. Przed kontynuowaniem upewnij się, że wiesz, jak [zdefiniowane przez użytkownika przekierowuje (UDR)] [ udr-overview] i [Usługa równoważenia obciążenia] [ lb-overview] pracować w Azure.

Za pomocą różnych NVAs dostępne Azure marketplace rozszerzenie funkcji Azure w taki sam sposób, w przypadku używania urządzeń w centrum danych lokalnych. Na poniższej ilustracji pokazano wdrożeniu przykładowe [pojedynczy NVA] [ nva-scenario] jako urządzenia zapory. 

![[0]][0]

Chociaż poprzedniego wdrożenia działa, wprowadza pojedynczy punkt awarii. Jeśli wirtualną urządzenia kończy się niepowodzeniem, przepływa żaden ruch. Aby rozwiązać ten problem, należy użyć wielu NVAs. Jednak także wymagające inne ustawienia i zasoby, które ma być używany w zależności od potrzeb.

Jedną z następujących rozwiązań umożliwia wdrażanie wysokiej dostępności środowiska NVA.

|Rozwiązanie|Zalety|Zagadnienia dotyczące|
|---|---|---|
|Ingress z urządzenia wirtualnego warstwy 7|Wszystkie węzły są aktywne|Wymaga NVA, który można zakończyć połączenia za pomocą SNAT<br/>Wymaga oddzielny zestaw NVAs ruchu pochodzących z Internetu i z platformy Azure<br/>Można używać tylko dla ruchu pochodzące spoza Azure|
|Ingress-wyjściowe z urządzenia wirtualne warstwy 7|Wszystkie węzły są aktywne<br/>Radzić sobie ruch wychodzący platformy Azure |Wymaga NVA, który można zakończyć połączenia za pomocą SNAT<br/>Wymaga oddzielny zestaw NVAs ruchu pochodzących z Internetu i z platformy Azure|
|Przełączanie PIP UDR|Jeden zestaw NVAs dla całego ruchu<br/>Można obsługiwać cały ruch (brak limitu na reguły portów)|Aktywne pasywne<br/>Wymaga awaryjnego procesu|

## <a name="ingress-with-layer-7-virtual-appliances"></a>Ingress z urządzenia wirtualne warstwy 7
Zestaw NVAs za równoważenia obciążenia Azure służy do zapewnienia połączeń Azure obciążenia za małego zestawu porty po stronie serwera (na przykład HTTP i HTTPS). Poniższej ilustracji wyróżnienie sposób zapewnić wysoką dostępność w tym scenariuszu na poziomie NVA.

![[1]][1]

W tym scenariuszu urządzenia wirtualnego sieci używane należy zakończyć wszystkie połączenia i przekazać je do podsieci obciążenie pracą. Obciążenie pracą wirtualnych maszyn odpowiedzieć NVA, że otrzymała wniosek i przepływ ruchu bez problemów. 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>Ingress wyjściowego z urządzenia wirtualnego warstwy 7
Można rozszerzyć poprzedniego architektura i Dodawanie innego zestawu NVAs do obsługi ruchu pochodzących z Azure obsługiwanych przez NVAs, jak pokazano na poniższym rysunku:

![[2]][2]

W tym scenariuszu cały ruch pochodzących Azure jest przekierowywane do równoważenia obciążenia wewnętrzny, który rozdziela obciążenie między innego zestawu NVAs. Te NVAs kierować ruch do Internetu, przy użyciu ich poszczególnych publicznych adresów IP. 

## <a name="pip-udr-switch"></a>Przełączanie PIP UDR
Można uniknąć tworzenia wielu stosy NVA za pomocą dwóch NVAs w trybie pasywne aktywne. W tym scenariuszu możesz przełączać się publiczny adres IP (PIP) i przekierowuje zdefiniowane przez użytkownika (UDRs) po zatrzymaniu aktywny węzeł.  

![[3]][3]

W tym scenariuszu jest podobna do pojedynczej scenariusz NVA. Jedyna różnica polega na tym, że PIP i UDRs musi zostać zmieniona na przełączanie ruchu między NVAs. Te zmiany można przeprowadzić ręcznie lub można je również zautomatyzować. Aby zautomatyzować, należy wdrożyć aplikację do obu NVAs, która sprawdza kondycji aktywny węzeł. Po aktywny węzeł nie działa, aplikacja można zmienić PIP i UDRs, aby utworzyć łącze do węzła pasywne.

Możliwe stosowania tego rozwiązania jest użycie [ZooKeeper] [ zookeeper] demon na NVAs obsługę wyborach znak wiodący (podejmowania decyzji, który węzeł jest aktywny węzeł). Gdy jest znak wiodący połączeń do interfejsu API usługi REST Azure, aby usunąć PIP z uszkodzonego węzła i dołączyć go do osobą prowadzącą. Należy go też zmienić UDRs, wskaż polecenie nowy znak wiodący prywatny adres IP.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [wdrażać strefy Zdemilitaryzowanej między Azure i centrum danych lokalnych] [ dmz-on-prem] przy użyciu NVAs warstwy-7.
- Dowiedz się, jak [wdrażać strefy Zdemilitaryzowanej między Azure i Internet] [ dmz-internet] przy użyciu NVAs warstwy-7.

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "Jedną architekturę NVA"
[1]: ./media/guidance-nva-ha/l7-ingress.png "Warstwy 7 ingress"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "Warstwy 7 ingress i wyjściowego"
[3]: ./media/guidance-nva-ha/active-passive.png "Klaster pasywne aktywne"