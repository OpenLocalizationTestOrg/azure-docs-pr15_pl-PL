<properties 
    pageTitle="Wprowadzenie do poziomu Azure Redis pamięci podręcznej Premium | Microsoft Azure" 
    description="Dowiedz się, jak tworzyć i zarządzać Redis utrzymywanie, Redis klaster i VNET pomocy technicznej dotyczącej usługi wystąpień pamięci podręcznej Azure Redis warstwa Premium" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="introduction-to-the-azure-redis-cache-premium-tier"></a>Wprowadzenie do poziomu Azure Redis pamięci podręcznej Premium
Azure Redis pamięci podręcznej jest to rozłożone, zarządzanych pamięci podręcznej, który ułatwia tworzenie aplikacji dużą skalowalność i odpowiada dostarczając wielokrotnie szybki dostęp do danych. 

Nowy poziom Premium jest w warstwie gotowe przedsiębiorstwa, które zawiera wszystkie funkcje standardowego warstwy i uzyskać więcej informacji, takich jak lepszą wydajność, zwiększenie obciążenia, odzyskiwanie, Importuj/Eksportuj i ulepszone zabezpieczenia. Kontynuuj odczytu, aby dowiedzieć się więcej o dodatkowe funkcje warstwie pamięci podręcznej Premium.

## <a name="better-performance-compared-to-standard-or-basic-tier"></a>Lepszą wydajność w porównaniu z poziomu standardowy lub Basic
**Lepsze wydajności na standardowy lub podstawowe warstwy.** Pamięć podręczną w warstwie premii są rozmieszczone na sprzęt, który ma szybsze procesory i zapewnia lepszą wydajność w porównaniu do Basic lub warstwa standardowy. Pamięć podręczną warstwa Premium mają wyższej przepustowości i opóźnienia dolnym. 

**Przepustowości dla samej wielkości pamięci podręcznej się wyżej w Premium porównaniu warstwa standardowy.** Na przykład przepustowość 53 GB P4 pamięci podręcznej (Premium) jest 250K żądania na sekundę porównaniu 150 K dla C6 (Standard).

Aby uzyskać więcej informacji na temat rozmiaru przepustowość i przepustowość z pamięci podręcznej premium, zobacz [Azure Redis pamięci podręcznej często zadawane pytania](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis utrzymywanie danych
Poziom Premium pozwala zachować dane pamięci podręcznej na koncie magazyn Azure. W pamięci podręcznej Basic i standardowe wszystkie dane są przechowywane tylko w pamięci. W przypadku podstawowej infrastruktury problemów może być utracie danych. Zaleca się za pomocą funkcji utrzymywanie danych Redis w warstwie Premium zwiększanie ochrony przed utratą danych. Azure Redis pamięci podręcznej oferuje RDB i AOF (już wkrótce), opcje w [Redis trwałe](http://redis.io/topics/persistence). 

Aby uzyskać instrukcje dotyczące konfigurowania utrzymywanie zobacz [jak skonfigurować pozostawanie na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-persistence.md).

##<a name="redis-cluster"></a>Redis klaster
Jeśli chcesz utworzyć większa niż 53 GB pamięci podręcznej lub chcesz shard danych w wielu węzłach Redis umożliwia Redis klaster, który jest dostępny w warstwie Premium. Każdy węzeł składa się z głównego replice para pamięci podręcznej zarządzane przez Azure wysokiej dostępności. 

**Klaster redis umożliwia maksymalną skalę i przepustowość.** Przepustowość zwiększa liniowo zwiększenie liczby odłamki (węzłów) w klastrze. Np. Jeśli tworzysz klastrze P4 odłamki 10, a następnie dostępna przepustowość jest 250K * 10 = 2,5 miliona żądania na sekundę. Zobacz [Azure Redis pamięci podręcznej często zadawane pytania](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) , aby uzyskać więcej informacji o rozmiarze, przepustowość i przepustowości z pamięci podręcznej premium.

Aby rozpocząć pracę z klastrów, zobacz [jak skonfigurować klaster na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-clustering.md).

##<a name="enhanced-security-and-isolation"></a>Ulepszone zabezpieczenia i izolacji

Utworzony w warstwie Basic lub standardowe pamięci podręcznej są dostępne na publicznego Internetu. Dostęp do pamięci podręcznej jest ograniczony, oparte na klawisz dostępu. Z poziomu Premium można dodatkowo sprawdź, czy tylko klientów w określonej sieci może dostęp pamięci podręcznej. Można wdrażać Redis pamięci podręcznej w [Azure wirtualnej sieci (VNet)](https://azure.microsoft.com/services/virtual-network/). Może używać wszystkich funkcji VNet, takich jak podsieci, zasad kontroli dostępu, i inne funkcje dodatkowe ograniczenia dostępu do Redis.

Aby uzyskać więcej informacji zobacz [jak skonfigurować wirtualną sieć obsługę bufor Redis Azure Premium](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Importuj/Eksportuj

Import/Eksport jest operacji zarządzania danych Azure Redis w pamięci podręcznej, dzięki czemu można zaimportować dane do pamięci podręcznej Azure Redis lub eksportowania danych z pamięci podręcznej Azure Redis przez importowania i eksportowania migawki Redis pamięci podręcznej bazy danych (RDB) z pamięci podręcznej premium do obiektów blob strony przy użyciu konta z miejsca do magazynowania Azure. Dzięki temu można przeprowadzić migrację między różnych wystąpieniach Azure Redis w pamięci podręcznej i wypełniać w pamięci podręcznej danych przed użyciem.

Importowanie można przenosić Redis zgodne RDB plików z dowolnego Redis serwerem w chmurze lub środowisko, w tym Redis uruchomionych Linux, Windows lub dowolnego dostawcy chmurze, takich jak Amazon usług sieci Web i innych osób. Importowanie danych jest łatwym sposobem utworzenia pamięci podręcznej z danymi wstępnie wypełnione. Podczas procesu importowania Azure Redis w pamięci podręcznej ładuje pliki RDB z magazynu Azure do pamięci, a następnie wstawia nazwy klawiszy w pamięci podręcznej.

Eksportowanie umożliwia eksportowanie danych przechowywanych w pamięci podręcznej Redis Azure do Redis zgodne pliki RDB. Ta funkcja umożliwia przenoszenie danych z jednego wystąpienia pamięci podręcznej Azure Redis do drugiego lub na inny serwer Redis. Podczas procesu eksportowania jest tworzony plik tymczasowy na maszyn wirtualnych obsługującego wystąpienie serwera Azure Redis w pamięci podręcznej, a plik jest przekazywany do konta wyznaczonego miejsca do magazynowania. Po zakończeniu operacji eksportowania ze stanem sukcesu lub niepowodzenia tymczasowy plik zostanie usunięty.

Aby uzyskać więcej informacji zobacz [jak zaimportować dane i eksportowanie danych z pamięci podręcznej Azure Redis](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Uruchom ponownie komputer

Poziom premium umożliwia Uruchom ponownie jeden lub więcej węzłów z pamięci podręcznej na żądanie. Pozwala na testować aplikację elastyczność w przypadku awarii. Można ponownie uruchomić następujące węzły.

-   Główny węzeł pamięci podręcznej
-   Węzeł podrzędny z pamięci podręcznej
-   Zarówno i podrzędna węzły pamięci podręcznej
-   Korzystając z pamięci podręcznej premium z klaster, można uruchamiać ponownie wzorca, podrzędny lub oba węzły dla poszczególnych odłamki w pamięci podręcznej

Aby uzyskać więcej informacji zobacz [Uruchom ponownie](cache-administration.md#reboot) i [Uruchom ponownie — często zadawane pytania](cache-administration.md#reboot-faq).

## <a name="schedule-updates"></a>Planowanie aktualizacji

Funkcja zaplanowane aktualizacje umożliwia oznaczania okna konserwacji dla pamięci podręcznej. Po określeniu okna konserwacji aktualizacje serwera Redis zostały wprowadzone w tym oknie. Aby wyznaczyć oknem konserwacji, zaznacz odpowiednie dni i określanie zachowania okno Uruchom godzina w przypadku każdego dnia. Należy zauważyć, że podczas okna konserwacji w czasie UTC. 

Aby uzyskać więcej informacji zobacz [Planowanie aktualizacji](cache-administration.md#schedule-updates) i [Zaplanuj aktualizacje: często zadawane pytania](cache-administration.md#schedule-updates-faq).

>[AZURE.NOTE] Tylko Redis server, które aktualizacje zostały wprowadzone w oknie zaplanowanych. Okno konserwacji nie ma zastosowania do Azure aktualizacje lub aktualizacje do maszyn wirtualnych systemu operacyjnego.

## <a name="to-scale-to-the-premium-tier"></a>Aby przeskalować w warstwie premium

Przeskalować do poziomu premium, po prostu wybierz jeden z poziomów premium w karta **Zmienianie ceny warstwy** . Można również skalować pamięć podręczną w warstwie premium przy użyciu programu PowerShell i interfejsu wiersza polecenia. Aby uzyskać instrukcje krok po kroku zobacz [jak skala Azure Redis w pamięci podręcznej](cache-how-to-scale.md) i [Jak zautomatyzować operacji skalowania](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Następne kroki

Tworzenie pamięci podręcznej i Poznaj nowe funkcje warstwa premium.

-   [Jak skonfigurować pozostawanie na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-persistence.md)
-   [Jak skonfigurować wirtualną sieć obsługę bufor Redis Azure Premium](cache-how-to-premium-vnet.md)
-   [Jak skonfigurować klaster na Premium Azure Redis potrzeby pamięci podręcznej](cache-how-to-premium-clustering.md)
-   [Jak importowanie danych do i Eksportuj dane z pamięci podręcznej Azure Redis](cache-how-to-import-export-data.md)
-   [Sposobu administrowania Azure Redis w pamięci podręcznej](cache-administration.md)
  

