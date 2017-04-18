<properties
   pageTitle="Implementacji hybrydowych architektury sieci przy użyciu sieci VPN Azure i lokalnych | Microsoft Azure"
   description="Jak wdrażać architektura bezpiecznej sieci witryny do witryny, wyświetlanego Azure wirtualnej sieci i sieci lokalnej połączone za pomocą sieci VPN."
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>Implementacji hybrydowych architektury sieci przy użyciu Azure i lokalnych VPN

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

W tym artykule omówiono zestaw wskazówki dotyczące rozszerzanie sieci lokalnej na Azure za pomocą witryny do witryny wirtualną sieć prywatną (VPN). Ruch przepływa między sieci lokalnej i Azure wirtualnych sieci (VNet) za pośrednictwem sieci VPN IPSec tunelem. Ta architektura nadaje się do aplikacji hybrydowych o następujących właściwościach:

- Części aplikacji uruchamiane w lokalnym, natomiast inne osoby są uruchamiane w Azure.

- Ruch między sprzętu lokalnego i chmury jest prawdopodobnie light lub jest handlu nieco rozszerzonego opóźnienie elastyczność i możliwości przetwarzania w chmurze.

- Rozszerzone sieci stanowi system zamknięty. Istnieje bezpośredniej ścieżki z Internetu do Azure VNet.

- Użytkownicy łączyć się sieci lokalnej, aby korzystać z usług obsługiwany w Azure. Mostka między sieci lokalnej i services działający w Azure jest niewidoczne dla użytkowników.

Przykłady scenariuszy, które przebiegają wzdłuż tego profilu:

- Aplikacje pakietu LOB używane w organizacji, gdzie została zmigrowana część funkcji w chmurze.

- Obciążenia rozwoju i testowym.

> [AZURE.NOTE] Azure występują dwa modele rozmieszczania: [Menedżer zasobów Azure] [ resource-manager-overview] i klasycznego. Ten plan używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

## <a name="architecture-diagram"></a>Diagram architektury

Poniższy diagram wyróżnia składniki w tej architektury:

> Dokument programu Visio, który zawiera ten diagram architektury jest dostępna do pobrania w [Centrum pobierania firmy Microsoft][visio-download]. Ten diagram znajduje się w "sieci hybrydowych - VPN" strony.

![[0]][0]

- **Sieci lokalnej.** Jest to sieć komputerów i urządzeń połączonych za pośrednictwem sieci lokalnej prywatne działa w obrębie organizacji.

- **Urządzenia sieci VPN.** Jest to urządzenie lub usługa, która zapewnia zewnętrznych łączność z siecią lokalnego. Urządzenia sieci VPN może być urządzenie, lub może być oprogramowanie, takie jak Routing i zdalny dostęp Service () w systemie Windows Server 2012.

> [AZURE.NOTE] Aby uzyskać listę obsługiwanych urządzeń sieci VPN i uzyskać informacje o konfigurowaniu wybranego urządzenia VPN do łączenia się z bramą VPN Azure, zapoznaj się z instrukcjami dotyczącymi odpowiedniego urządzenia z [listy urządzeń VPN obsługiwanych przez Azure][vpn-appliance].

- **Aplikacja N warstwowych chmury.** Jest to aplikacja, obsługiwany w Azure. Może zawierać wiele poziomów, z wiele podsieci połączonych za pomocą urządzenia do równoważenia obciążenia Azure. Ruch w każdej podsieci mogą podlegać reguły zdefiniowane przy użyciu [Grup zabezpieczeń sieci Azure (NSGs)][azure-network-security-group]. Aby uzyskać więcej informacji, zobacz [Wprowadzenie do zabezpieczeń platformy Microsoft Azure][getting-started-with-azure-security].

> [AZURE.NOTE] Ten artykuł zawiera opis aplikacji chmury jako całość. Uruchomione [Architektura N warstwowych Azure] [ implementing-a-multi-tier-architecture-on-Azure] Aby uzyskać szczegółowe informacje.

- **[Wirtualna sieć (VNet)][azure-virtual-network].** Aplikacja chmury i składniki Brama VPN Azure znajdują się w tym samym VNet.

- **[Brama Azure VPN][azure-vpn-gateway].** Usługa bramy sieci VPN umożliwia łączenie VNet do sieci lokalnej za pomocą urządzenia VPN. Aby uzyskać więcej informacji, zobacz [Łączenie sieci lokalnej wirtualną sieć Microsoft Azure][connect-to-an-Azure-vnet]. Brama VPN obejmuje następujące elementy:

  - **Brama wirtualnej sieci.** Jest to zasób, który zawiera wirtualnego urządzenia VPN dla VNet. Odpowiada za kierowanie ruchu z sieci lokalnej do VNet.

  - **Brama sieci lokalnej.** Jest to analizę urządzenia VPN lokalnego. Ruch sieciowy z poziomu aplikacji chmury do sieci lokalnej odbywa się za pośrednictwem tej bramy.

  - **Połączenie.** Połączenie ma właściwości, które określają typu połączenia (IPSec) i klawisz udostępnione urządzenia VPN lokalnego do szyfrowania ruchu.

  - **Podsieć bramy.** Brama wirtualnej sieci jest przechowywana w osobnej podsieci, którym podlega różne wymagania, zgodnie z opisem w poniższej sekcji zalecenia.

- **Usługi równoważenia obciążenia wewnętrzny.** Ruch sieciowy z bramy sieci VPN jest przekierowywane do aplikacji chmurze za pomocą usługi równoważenia obciążenia wewnętrzny. Usługi równoważenia obciążenia znajduje się w podsieci zewnętrznej aplikacji.

## <a name="recommendations"></a>Zalecenia

Azure oferuje wiele różnych zasobów i typów zasobów, aby ta architektura odwołanie może być obsługi administracyjnej wiele różnych sposobów. Utworzono szablon Menedżera zasobów Azure zainstalować architektura odwołania, znajdujący się tych zaleceń. Jeśli chcesz utworzyć własny architektura odwołania można śledzić tych zaleceń, chyba że masz określonych wymóg, aby zalecenie nie mieści się.

### <a name="vnet-and-gateway-subnet"></a>Podsieć VNet i bramy

Tworzenie VNet Azure do przechowywania elementów w chmurze. Obszar adresów Azure VNet musi być mała, aby zezwalały na adresy używane za pośrednictwem SMS i podsieci w VNet. Upewnij się, że VNet przestrzeni adresów nie ma wystarczających miejsca na rozwój, jeśli dodatkowe maszyny wirtualne mogą być potrzebne w przyszłości. Obszar adresów VNet nie musi zachodzić w sieci lokalnej. Na przykład na powyższym diagramie używa 10.20.0.0/16 miejsca na adres VNet.

Tworzenie podsieci o nazwie _GatewaySubnet_z zakresu adresów /27. Danej podsieci jest wymagane przez bramę wirtualnej sieci i przydzielanie 32 adresów do danej podsieci pomoże w zapobieganiu został uruchomiony sprzętu ograniczenia rozmiaru możliwe bramy w przyszłości. Unikaj umieszczania danej podsieci w środku przestrzeni adresów. Zalecane jest obszaru adresów podsieci bramy w górnej części obszaru adresów VNet. W poniższym przykładzie na diagramie użyto 10.20.255.224/27.  Krótkiej procedury, aby obliczyć CIDR jest następująca:

1. Ustaw zmienną bitów w obszarze adres VNet 1, aż do bitów są używane przez podsieci bramy, a następnie ustaw pozostałe bitów 0.

2. Konwertowanie bitów otrzymane w postaci dziesiętnej i przedstawić go jako przestrzeni adresów z wartością długość prefiksu rozmiar podsieci bramy.

Na przykład dla VNet z zakres adresów IP 10.20.0.0/16, stosując kroku #1 staje się 10.20.0b11111111.0b11100000.  Konwertowanie który na liczbę dziesiętną i wyraża go jako przestrzeni adresów daje 10.20.255.224/27

> [AZURE.WARNING] Nie należy wdrażać innych maszyn wirtualnych lub wystąpienia roli do podsieci bramy. Ponadto nie przydziela NSG danej podsieci, jak spowoduje bramy przestanie działać.

### <a name="virtual-network-gateway"></a>Brama wirtualnej sieci

Przydzielanie publiczny adres IP bramy wirtualnej sieci.

Tworzenie bramy wirtualną sieć w podsieci bramy i przypisz mu nowo przydzielone publiczny adres IP. Używany typ bramy, który najlepiej pasuje do wymagań i które jest włączona przez urządzenia VPN:

Tworzenie [bramy na podstawie zasad] [ policy-based-routing] Jeśli musisz ściśle kontrolować sposób rozsyłania żądania. na podstawie zasad kryteriów takich jak prefiksów adresów. Bram na podstawie zasad używania routingu statycznego i działają tylko w przypadku połączeń witryny do witryny.

Tworzenie [bramy oparte na trasę] [ route-based-routing] w przypadku połączenia z siecią lokalnego przy użyciu RRAS obsługuje wiele witryn lub regionu krzyżowe połączeń lub implementowania połączeń VNet do VNet (łącznie marszrut przechodzące przez wielu VNets). Oparte na rozsyłania bramy za pomocą routing dynamiczny bezpośredni ruchu między sieciami. Ich przeszkadzają błędów w lepiej niż trasy statyczne ścieżki sieciowej, ponieważ próbują trasy alternatywne. Oparte na rozsyłania bram można również zmniejszyć ogólnych zarządzania, ponieważ marszrut nie może być konieczne mają być aktualizowane ręcznie po zmienianie adresów sieciowych.

Aby uzyskać listę obsługiwanych urządzeń sieci VPN, zobacz [temat VPN urządzenia połączeń Brama VPN witryny do witryny][vpn-appliances].

> [AZURE.NOTE] Po utworzeniu bramy nie można zmienić między typami bramy bez usuwanie i ponowne tworzenie bramy.

Wybierz pozycję Azure jednostka SKU bramy sieci VPN, który najlepiej pasuje do wymagań przepustowości. Azure Brama VPN jest dostępna w trzech SKU pokazano w poniższej tabeli. Każdy SKU zapewnia różne przepustowości, [są nakładane opłaty] [ azure-gateway-charges] oparte na ilość czasu, który jest zainicjowany bramy i jest dostępny.

| JEDNOSTKA SKU | Przepustowość sieci VPN | Tunele Max IPSec|
|-----|----------------|------------------|
| Podstawowe | 100 MB/s | 10 |
| Standardowe | 100 MB/s | 10 |
| Wysoka wydajność | 200 MB/s | 30 |

> [AZURE.NOTE] Podstawowa jednostka SKU nie jest zgodny z Azure ExpressRoute. Użytkownik może [zmienić używaną WERSJĘ] [ changing-SKUs] po utworzeniu bramy.

Tworzenie reguł rozsyłania podsieci bramy bezpośredni ruchu danych przychodzącego aplikacji z bramy do równoważenia obciążenia wewnętrznych zamiast zezwalanie żądania w celu przekazania bezpośrednio do maszyny wirtualne implementujących aplikacji.

### <a name="on-premises-network-connection"></a>Lokalne połączenia sieciowego

Tworzenie bramy sieci lokalnej. Określ publiczny adres IP urządzenia VPN lokalnego i przestrzeni adresów sieci lokalnej. Należy zauważyć, że urządzenie VPN lokalnego muszą mieć publiczny adres IP, które mogą uzyskiwać dostęp do sieci lokalnej bramy w Brama VPN Azure. Urządzenia VPN nie może znajdować się za urządzeniem NAT.

Utwórz połączenie z witryny do witryny dla bramy wirtualnej sieci i bramy sieci lokalnej. Wybierz typ połączenia między witryny (IPSec), a następnie określ klucz udostępniony. Szyfrowanie witryny do witryny z bramą VPN Azure jest oparte na protokół IPSec używania kluczy wstępnych uwierzytelniania. Podczas tworzenia bramy sieci VPN Azure określić klucz. Musisz skonfigurować urządzenia VPN z lokalnego z tym samym kluczem. Inne mechanizmy uwierzytelniania nie są obecnie obsługiwane.

Upewnij się, że w lokalnej infrastrukturze routingu jest skonfigurowany do przesyłania żądań przeznaczone dla adresów VNet Azure na urządzeniu VPN.

Otwórz dowolne porty wymagane przez aplikację cloud w sieci lokalnej.

Testuj połączenie, aby sprawdzić, czy:

- Urządzenia VPN lokalnego poprawnie kieruje ruch do aplikacji chmurze za pomocą bramy sieci VPN Azure.

- VNet poprawnie kieruje ruch do sieci lokalnej.

- Zabronione ruch w obu kierunków jest blokowany poprawnie.

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

Ograniczone skalowalność pionowej można uzyskać, przenosząc z Basic lub standardowej wersji produktu bramy sieci VPN do wysoka SKU VPN wydajności.

Dla VNets z oczekiwaniami dużej liczby ruch przez sieć VPN rozważ rozpowszechniania różnych obciążenie pracą w osobnym VNets mniejszy i konfigurowanie bramy sieci VPN dla każdego z nich.

VNet można podzielić poziomo lub pionowo. Aby podzielić poziomo, przenoszenie niektórych przypadkach maszyn wirtualnych z każdej warstwy na podsieci nowych VNet. Wynik to, że każdy VNet ma taką samą strukturę i funkcje. Aby podzielić w pionie, na wprowadzanie zmian w projekcie każdej warstwy, aby podzielić funkcji na różne obszary logiczne (takie jak obsługa zamówienia, faktury, zarządzanie kontami klienta i tak dalej). Każdy z obszarów funkcjonalnych można umieścić w osobnym VNet.

Replikacji kontrolera domeny lokalnej usługi Active Directory w VNet oraz ich realizacji DNS w VNet, może pomóc w celu zmniejszenia niektóre dane związane z zabezpieczeniami i administracyjnych wynikających z lokalnego w chmurze.

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Jeśli potrzebujesz upewnić się, że sieci lokalnej pozostaje dostępny do bramy sieci VPN Azure, zaimplementować klastrze pracy awaryjnej bramy sieci VPN lokalnego.

Jeśli Twoja organizacja ma wiele witryn lokalnego, tworzenie [połączenia wielu witryny] [ vpn-gateway-multi-site] do co najmniej jeden VNets Azure. Ta metoda wymaga routing dynamiczny (w oparciu o rozsyłania), więc upewnić się, że Brama VPN lokalnego obsługuje tę funkcję.

Zobacz [Umowa dotycząca poziomu usług dla bramy sieci VPN] [ sla-for-vpn-gateway] uzyskać informacje na temat SLA bramy sieci VPN.

## <a name="manageability-considerations"></a>Zagadnienia dotyczące zarządzania

Monitorowanie informacji diagnostycznych z urządzenia VPN lokalnego. Ten proces zależy od funkcji dostępnych na urządzeniu VPN. Na przykład, jeśli korzystasz z usługi Routing i dostęp zdalny w systemie Windows Server 2012, należy włączyć [Rejestrowanie RRAS][rras-logging].

[Diagnostyka Brama VPN Azure] za pomocą[ gateway-diagnostic-logs] do przechwytywania informacji na temat problemów z łącznością. Dzienniki te może służyć do śledzenia informacji, takich jak źródła i miejsc docelowych połączenia żądania, który protokół użyto i jak ustanowienia połączenia (lub dlaczego nie można).

Monitorowanie dzienniki operacyjne bramy sieci VPN Azure za pomocą dzienników inspekcji dostępne w portalu Azure. Oddzielne dzienniki są dostępne dla bramy sieci lokalnej, brama Azure sieci i połączenia. Te informacje mogą służyć do śledzenia zmian bramy i może być przydatne, jeśli wcześniej działa bramy przestanie działać jakiegoś powodu.

![[2]][2]

Monitorowanie łączności i śledzić zdarzenia błędów łączności. Można użyć monitorowania pakietu, takich jak [Nagios] [ nagios] do przechwytywania i raportowania te informacje.

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Generowanie różnych udostępnionego klucza dla każdej bramy sieci VPN. Pomoc oprzeć atakami siłowymi przy użyciu silnych klucza udostępnionego.

> [AZURE.NOTE] Obecnie nie umożliwia Azure klucza magazynu kluczy wstępnych Azure VPN bramy.

Upewnić, że urządzenie VPN lokalnego używa metody szyfrowania, która jest [zgodna z bramą VPN Azure][vpn-appliance-ipsec]. W przypadku na podstawie zasad routingu Brama VPN Azure obsługuje algorytmy AES256, AES128 i 3DES. Oparte na rozsyłania bram obsługuje AES256 i 3DES.

W przypadku urządzenia VPN lokalnego w sieci obwód kształtu, który ma zaporę między obwód kształtu Sieć i Internet, może być konieczne skonfiguruje [reguły zapory dodatkowe] [ additional-firewall-rules] umożliwia połączenie VPN witryny do witryny.

Jeśli dane są wysyłane w aplikacji w VNet w Internecie, należy rozważyć, czy [implementacji wymuszonego tunneling] [ forced-tunneling] do kierowania cały ruch powiązanych z Internetem za pośrednictwem sieci lokalnej. Ta metoda umożliwia inspekcji wychodzących wnioski aplikacji z infrastruktury lokalnego.

> [AZURE.NOTE] Wymuszone tunneling może mieć wpływ na łączność z usług Azure (na przykład usługa miejsca do magazynowania) i Menedżera licencji systemu Windows.

## <a name="troubleshooting-considerations"></a>Zagadnienia dotyczące rozwiązywania problemów

> [AZURE.NOTE] Odwiedź routingu i zdalnego dostępu do blogu, aby uzyskać ogólne informacje w [rozwiązywania typowych problemów związanych z sieci VPN][troubleshooting-vpn-errors].

Jeśli ruch nie może się przechodzenie przez połączenie VPN, wykonaj następujące czynności:

### <a name="on-premises-vpn-appliance"></a>Lokalne połączenia VPN urządzenia:

**Urządzenia VPN lokalnego działa poprawnie?**

Zaznacz dowolną dzienniki generowane przez urządzenia VPN błędów i błędów. Lokalizacja tych informacji zależy od urządzenia. Na przykład jeśli korzystasz z systemu Windows Server 2012 RRAS, umożliwia następującego polecenia programu PowerShell wyświetlić informacje o błędzie zdarzenia usługi RRAS:

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

Właściwość _wiadomości_ każdego wpisu zawiera opis błędu. Przykłady typowych są:

- Nie można połączyć, prawdopodobnie ze względu na niepoprawny adres IP określonej bramy sieci VPN Azure w konfiguracji interfejsu sieci RRAS VPN:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- Nieprawidłowy klucz udostępniony jest określony w konfiguracji interfejsu sieci RRAS VPN:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

Można również uzyskać dziennika zdarzeń informacji na temat próby połączenia za pośrednictwem usługi RRAS przy użyciu następującego polecenia programu PowerShell:

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

W przypadku braku połączenia ten dziennik będzie zawierać błędy, które wyglądają podobnie do następującej:

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**Jest poprawnie routingu ruchu urządzenia sieci VPN przez bramę VPN Azure?**

Użyj narzędzia, takie jak [PsPing] [ psping] do Sprawdź połączenie oraz routing przez bramy sieci VPN. Na przykład, aby przetestować łączność z komputera lokalnego do serwera sieci web znajdujące się w VNet, uruchom następujące polecenie (Zamień `<<web-server-address>>` z adresem serwera sieci web):

```
PsPing -t <<web-server-address>>:80
```

Jeśli na komputerze lokalnym może skierować ruch do serwera sieci web, zobacz dane wyjściowe podobne do następujących czynności:

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

Jeśli na komputerze lokalnym nie można komunikować się z określoną lokalizacją docelową, zostaną wyświetlone komunikaty tak jak poniżej:

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**Zapora w lokalnej zezwala VPN ruchu? Zostały otwarte porty?**

Sprawdź używanego urządzenia VPN lokalnego metody szyfrowania, która jest [zgodna z bramą VPN Azure][vpn-appliance].

W przypadku na podstawie zasad routingu Brama VPN Azure obsługuje algorytmy AES256, AES128 i 3DES. Oparte na rozsyłania bram obsługuje AES256 i 3DES.

### <a name="azure-vnet-gateway"></a>Azure brama VNet:

Sprawdź [Dzienniki diagnostyczne Brama VPN Azure] [ gateway-diagnostic-logs] dla potencjalnych problemów.

**Czy Brama VPN Azure i lokalnych urządzenia VPN skonfigurowanym kluczem uwierzytelnianie za pomocą udostępnionego?**

Można wyświetlić klucz przechowywane przez bramę VPN Azure za pomocą następującego polecenia Azure polecenie:

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

Polecenie odpowiednie dla urządzenia VPN lokalnego umożliwia wyświetlanie klucz skonfigurowane dla tego urządzenia.

Sprawdź, czy podsieci _GatewaySubnet_ posiadająca Brama VPN Azure nie jest skojarzony z NSG.

Możesz wyświetlać ich szczegóły podsieci przy użyciu następującego polecenia Azure polecenie:

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

Upewnij się, nie ma pola danych o nazwie _Identyfikator grupy zabezpieczeń sieci_. W poniższym przykładzie pokazano wyniki na przykład _GatewaySubnet_ ma przypisany NSG (_VPN brama grupy_). Może to powodować zapobiec bramy z działa poprawnie w przypadku reguł zdefiniowanych dla tego NSG:

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**Maszyn wirtualnych w Azure VNet są skonfigurowane do zezwolenia na ruch odbierane z zewnątrz VNet? Sprawdzanie reguł NSG skojarzone z podsieci zawierające te maszyn wirtualnych.**

Można wyświetlić wszystkie reguły NSG przy użyciu następującego polecenia Azure polecenie:

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**Brama VPN Azure utraceniu połączenia?**

Za pomocą następującego polecenia programu PowerShell Azure sprawdzić bieżący stan połączenia Azure VPN. `<<connection-name>>` Parametr jest nazwą połączenie Azure VPN, która łączy Brama wirtualnej sieci i bramy lokalnej.

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

Następujące wstawki wyróżnić wyniki generowane jeśli bramy jest połączony (przykład pierwszego) i odłączona (drugi przykład):

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>Konfiguracja hosta maszyn wirtualnych, wykorzystania przepustowości sieci i wydajność aplikacji:

Upewnij się, że Zapora systemu operacyjnego gościa uruchomionych maszyny wirtualne Azure w podsieci są poprawnie skonfigurowane umożliwiające dozwolonych ruchu z zakresów adresów IP lokalnego.

**Jest dostępna do bramy sieci VPN Azure wielkość ruchu zbliżony limit przepustowości?**

Narzędzia zależy od funkcji dostępnych dla urządzenia VPN z lokalnego. Na przykład jeśli korzystasz z systemu Windows Server 2012 RRAS, umożliwia monitorowanie wydajności śledzenia liczby dane są otrzymane i przesyłane przez połączenie VPN; przy użyciu obiektu _RAS sumy_ , wybierz liczniki _Bajty odebrane/s_ i _Bajty przesłane/s_ :

![[3]][3]

Należy porównać wyniki z przepustowości sieci VPN bramy (100 MB/s Basic i standardowej wersji produktu i 200 MB/s dla wysoka SKU wydajności):

![[4]][4]

**Dowolny maszyn wirtualnych w Azure VNet działają za wolno? Są, które były nadmiernie, są wystarczająco dużo ich do obsługi obciążenia są wszystkie równoważenia obciążenia poprawnie skonfigurowane?**

W tym celu [Przechwytywanie i analizowania informacji diagnostycznych][azure-vm-diagnostics]. Można sprawdzić wyniki za pomocą portalu Azure, ale wiele narzędzia innych firm są również dostępne także udostępnia szczegółowe spostrzeżeń dane dotyczące wydajności.

**Jest w chmurze zasobów za pomocą aplikacji, co skuteczne?**

Kod aplikacji instrument uruchomionych dla każdego maszyn wirtualnych w celu określenia, czy aplikacje wykonywania najlepsze wykorzystanie zasobów. Można użyć narzędzi, takich jak [Wniosków aplikacji] [ application-insights] ułatwiające wykonywanie następujących zadań.

## <a name="solution-deployment"></a>Wdrożenie rozwiązania

Jeśli masz już skonfigurowane z urządzenia VPN istniejącej infrastruktury lokalnego, można wdrożyć architektura odwołania, wykonując następujące czynności:

1. Kliknij prawym przyciskiem myszy przycisk poniżej i wybierz pozycję "Otwórz łącze w nowej karcie" lub "Otwórz łącze w nowym oknie":  
[![Wdrażanie Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. Czekaj na łącze, aby otworzyć w portalu Azure, a następnie wykonaj następujące czynności: 
    - Nazwa **grupy zasobów** jest już zdefiniowana w pliku parametrów, więc wybierz polecenie **Utwórz nowy** i wprowadź `ra-hybrid-vpn-rg` w polu tekstowym.
    - Wybierz region w polu rozwijanym **lokalizacji** .
    - Nie należy edytować **Uri głównego szablonu** lub pól tekstowych **Uri głównego parametru** .
    - Przejrzyj warunki umowy, a następnie kliknij pole wyboru **zgodę na warunki umowy podanych powyżej** .
    - Kliknij przycisk **Kup** .

3. Poczekaj, aż wdrożenia do wykonania.

## <a name="next-steps"></a>Następne kroki

- [Instalowanie dodatkowych kontrolerów domeny dla domeny usługi Active Directory w lokalnej przy użyciu maszyny wirtualne w VNet Azure][installing-ad].

- [Wskazówki dotyczące wdrażania systemu Windows Server usługi Active Directory na maszyny wirtualne Azure][deploying-ad].

- [Tworzenie serwera DNS w VNet][creating-dns].

- [Konfigurowanie i zarządzanie systemu DNS w VNet][configuring-dns].

- [Używanie lokalnego Stormshield sieci zabezpieczeń urządzeń w sieci VPN][stormshield].

- [Implementacji architektury sieci hybrydowego z Azure ExpressRoute][expressroute].

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "Struktura hybrydowym sieci, obejmujące w lokalnej i w chmurze infrastruktury"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "Podziału VNet, aby zwiększyć skalowalność"
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "Dzienniki inspekcji w portalu Azure"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "Monitorowanie ruchu w sieci VPN liczniki wydajności"
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "Wykres wydajności sieci VPN przykład"