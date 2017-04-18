<properties
   pageTitle="Tworzenie klastrów tkaninie usługi Azure na serwerze systemu Windows i Linux oraz | Microsoft Azure"
   description="Klastrów tkaninie usługi w systemie Windows Server i Linux, co oznacza możesz wdrożyć i dowolne miejsce aplikacji tkaninie usługi hosta można uruchamiać serwera systemu Windows i Linux oraz."
   services="service-fabric"
   documentationCenter=".net"
   authors="Chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="chackdan"/>

# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Tworzenie klastrów tkaninie usługi na serwerze systemu Windows i Linux oraz

Azure tkaninie usługi pozwala na tworzenie klastrów tkaninie usługi w dowolnym maszyny wirtualne lub na komputerach z systemem Windows Server lub Linux. Oznacza to, będą mogli wdrażanie i uruchamianie aplikacji usługi tkaninie w dowolnym środowisku, w którym masz zestawu Server systemu Windows i Linux oraz połączonych komputerach, jest to lokalny program Microsoft Azure lub przy użyciu dowolnego dostawcy chmury.

##<a name="create-service-fabric-clusters-on-azure"></a>Tworzenie klastrów tkaninie usługi Azure

Tworzenie klastrze Azure odbywa się albo za pomocą szablonu modelu zasobów lub Azure portal. Aby uzyskać więcej informacji, przeczytaj [Utwórz klaster tkaninie usługi przy użyciu szablonu Menedżera zasobów](service-fabric-cluster-creation-via-arm.md) lub [utworzyć klaster tkaninie usługi z portalu Azure](service-fabric-cluster-creation-via-portal.md) .

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Obsługiwane systemy operacyjne dla klastrów Azure

Widoczny jest tworzenie klastrów na tych systemem operacyjnym maszyny wirtualne:

* Windows Server 2012 R2
* Windows Server 2016 (po została ogłoszona jako ogólnodostępne)
* Linux Ubuntu 16.04 (w publicznej wersja preview) 


##<a name="create-service-fabric-standalone-clusters-on-premise-or-with-any-cloud-provider"></a>Tworzenie klastrów autonomicznej usługi tkaninie lokalnego lub dowolnego dostawcy chmury

Usługa tkaninie zawiera pakiet instalacyjny umożliwiają tworzenie autonomicznego tkaninie usługi klastrów lokalnego lub dowolnego dostawcy chmury

Aby uzyskać więcej informacji o konfigurowaniu autonomicznej usługi tkaninie klastrów w systemie Windows Server przeczytaj [tworzenia klaster tkaninie usługi dla systemu Windows Server](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Dowolny w chmurze wdrożeń a wdrożenia lokalne
Proces tworzenia klastrze tkaninie usługi lokalnej jest podobna do proces tworzenia klastrze na dowolny chmury dowolnego z zestawem maszyny wirtualne. Początkowe kroki do obsługi administracyjnej maszyny wirtualne podlegają dostawcą chmury lub lokalnego środowiska, którego używasz. Po umieszczeniu zestawu maszyny wirtualne z łączność sieciowa włączone między nimi, następnie czynności, aby skonfigurować pakiet tkaninie usługi ustawienia klaster uruchamianie tworzenia klaster i edytować skrypty zarządzania są identyczne. Dzięki temu usługi wiedzy i doświadczenia w pracy i zarządzania klastrów tkaninie usługi są można przenosić po wybraniu kierowania nowego środowiska hostingu.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Korzyści wynikające z tworzenia klastrów tkaninie usługi autonomicznego
* Mogą wybrać dowolnego dostawcy chmury do obsługi klaster.
* Tkaninie usługi aplikacji, raz, może działać w wielu środowiskach hostingu z minimalnymi nie zmian.
* Informacje dotyczące tworzenia aplikacji usługi tkaninie są przenoszone z jednego środowiska hostingu do drugiego.
* Środowisko operacyjne uruchomiony i zarządzania tkaninie usługi klastrów posiada nad z jednego środowiska do innego.
* Dostęp do szerokiego klienta jest bez ograniczeń, udostępniając ze środowiskiem.
* Dodatkowe warstwy niezawodności i ochrony przed szerokie dostawie istnieje, ponieważ można przenieść usług innego środowiska wdrażania Jeśli dostawca Centrum lub w chmurze danych ma na kolor czarny.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Obsługiwane systemy operacyjne dla klastrów autonomicznego
Widoczny jest tworzenie klastrów na maszyny wirtualne i komputerach z tych systemów operacyjnych:

* Windows Server 2012 R2
* Windows Server 2016 (po została ogłoszona jako ogólnodostępne)
* Linux (już wkrótce)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Zalety klastrów tkaninie usługi Azure nad autonomicznego klastrów tkaninie usługi utworzone w lokalnym

Uruchamianie klastrów tkaninie usługi Azure zawiera opcja zalet w lokalnym, więc jeśli nie masz specyficznych wymagań, dla której jest uruchomiony klastrów, a następnie Sugerujemy uruchomić je Azure. Azure firma Microsoft udostępnia integracji z innymi funkcjami Azure i usług, które powoduje, że operacje i zarządzania klaster łatwiejsze i bardziej niezawodne.

* **Azure portal:** Azure portal ułatwia tworzenie i zarządzanie nimi klastrów.

* **Azure Menedżera zasobów:** Używanie Menedżera zasobów Azure umożliwia łatwe zarządzanie wszystkich zasobów używanych przez klaster jako całość i ułatwia śledzenie kosztów i rozliczenia.
* **Klaster tkaninie usługi Azure zasób** Klaster tkaninie usługi jest zasób ARM, więc można modelować go tak samo jak inne zasoby ARM platformy Azure.
* **Integracja z infrastruktury Azure** Usługa tkaninie współrzędne podstawowej infrastruktury Azure dla systemu operacyjnego, sieci i inne typy uaktualnienia zwiększyć dostępność i niezawodność aplikacji.  
* **Diagnostyki:** Azure, firma Microsoft udostępnia integracji Azure narzędzia diagnostyczne i analizy dziennika.
* **Automatyczne skalowanie:** W przypadku klastrów Azure firma Microsoft udostępnia wbudowanych funkcji Automatyczne skalowanie z powodu zestawów skali maszyn wirtualnych. W lokalnym i inne środowiska chmury musisz utworzyć własne automatyczne skalowanie się funkcja lub Skala ręcznie za pomocą interfejsów API, które udostępnia tkaninie usługi skalowania klastrów.

## <a name="next-steps"></a>Następne kroki
Utworzyć klaster na komputerach z systemem Windows Server lub maszyny wirtualne: [Tworzenie klaster tkaninie usługi dla systemu Windows Server](service-fabric-cluster-creation-for-windows-server.md)

Utwórz klaster na komputerach z systemem Linux lub maszyny wirtualne: [Tkaninie usługi w systemie Linux](service-fabric-linux-overview.md)
