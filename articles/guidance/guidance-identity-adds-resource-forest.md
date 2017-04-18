<properties
   pageTitle="Azure odwołanie architektura — IaaS: Tworzenie las zasobu usługi Active Directory platformy Azure | Microsoft Azure"
   description="Jak utworzyć dla tej domeny usługi Active Directory platformy Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="creating-a-active-directory-directory-services-adds-resource-forest-in-azure"></a>Tworzenie las zasobu usługi Active Directory katalogu (DODAJE) platformy Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Ten artykuł zawiera opis sposobu tworzenia domeny usługi Active Directory platformy Azure oddzielone od, ale zaufany przez domen w sieci lokalnej.

> [AZURE.NOTE] Azure ma dwa różne wdrożenia modele: [Menedżer zasobów] [ resource-manager-overview] i klasyczny. Ta architektura odwołanie używa Menedżera zasobów, które firma Microsoft zaleca się w przypadku wdrożeń nowy.

Organizacja, która jest uruchamiany Active Directory (AD) lokalnego może być las obejmujące wiele różnych domenach. Na przykład możesz utworzyć osobne domeny dla różnych działów lub suborganizations lub nowej domeny mogły zostać dodane wyniku nabycia lub łączenia się z innymi organizacjami. Można używać domeny o podanie izolacji między funkcjonalności obszarów, które muszą być przechowywane oddzielnie, prawdopodobnie ze względów bezpieczeństwa, ale możesz udostępnić informacje między domenami przez ustanowienie relacji zaufania.

Organizacji, która korzysta z różnych domenach można korzystać z Azure przy przenoszeniu co najmniej jedną z tych domen w chmurze. Możesz też organizacji może chcesz zachować wszystkie zasoby chmury logicznie różnią się od tych przechowywanych w lokalnej i przechowywanie informacji o zasobach w chmurze w własnego katalogu w domenie, również przechowywanych w chmurze.

Można zaimplementować usługi Active Directory platformy Azure na różne sposoby, jak opisano w artykułach [Rozszerzanie usługi Active Directory Azure] [ extending-ad-to-azure] i [Wdrażania usługi Azure Active Directory][implementing-aad]. W tym dokumencie opisano na jeden scenariusz określonych: Tworzenie domeny w chmurze, który jest różnią się od domen przechowywanych w lokalnej, ale których relacja zaufania z domenami lokalnego. 

Typowe zastosowanie przypadkach dla tej architektury obejmują:

- Zachowywanie odstęp zabezpieczeń dla obiektów i tożsamości przechowywanych w chmurze.

- Migrowanie poszczególnych domen ze źródeł lokalnych, w chmurze.

## <a name="architecture-diagram"></a>Diagram architektury

Poniższy diagram wyróżnienie ważne składniki w tej architektury (*kliknij, aby powiększyć*). Aby uzyskać więcej informacji o elementach wyszarzona, przeczytaj [implementacji architektury sieci bezpiecznego hybrydowych platformy Azure] [ implementing-a-secure-hybrid-network-architecture] i [implementowania architektury sieci bezpiecznego hybrydowego z dostępem do Internetu w Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Sieci lokalnej.** Sieci lokalnej zawiera własnej AD las i domeny.

- **Serwery AD.** Są to kontrolery implementacji usługi katalogowej (AD DS) uruchomione maszyny wirtualne w chmurze. Te serwery obsługują las zawierający jedną lub więcej domen, oddzielone od tych znajduje lokalnego.

- **Relacja zaufania jednokierunkowe.** W przykładzie na diagramie pokazano jednokierunkowe zaufanie z domeny w chmurze do domeny lokalnej. Tej relacji umożliwia użytkownikom lokalnego dostępu do zasobów w domenie w chmurze, ale nie na odwrót. Jest to typowe wielkość liter. Jednak można utworzyć dwukierunkowe zaufanie, jeśli użytkownicy chmury również muszą mieć dostęp do zasobów lokalnych.

- **Podsieć Active Directory.** Serwer usług AD DS są obsługiwane w osobnej podsieci. Reguły NSG Chroń serwer usług AD DS i umożliwiają Zapora przeciw ruchu z nieznanych źródeł.

- **Podsieci poziomu sieci Web**, **podsieci warstwa firm**i **danych poziomu podsieci**. Te podsieci obsługi serwerów i składniki, których uruchamianie aplikacji w chmurze. Aby uzyskać więcej informacji, zobacz [Uruchamianie maszyny wirtualne dla architektury N warstwowych Azure][running-VMs-for-an-N-tier-architecture-on-Azure]. Zasoby i maszyny wirtualne w danej podsieci znajdują się w domenie chmury.

- **Azure bramy**. Brama Azure umożliwia połączenie między sieci lokalnej i Azure VNet. Może to być [połączenie VPN] [ azure-vpn-gateway] lub [Azure ExpressRoute][azure-expressroute]. Aby uzyskać więcej informacji, zobacz [implementacji architektury sieci bezpiecznego hybrydowych platformy Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Zalecenia

Ta sekcja zawiera listę rekomendacje oparte na istotne elementy, które są wymagane do wykonania podstawowej architektury. Zalecenia te obejmują:

- DODAJE ustawienia, a

- Ufaj konfiguracji relacji.

Może być dodatkowe lub różne wymagania niż opisane w tym miejscu. Elementy w tej sekcji można użyć jako punkt początkowy dla ustalając jak dostosować architektura własny system.

### <a name="adds-recommendations"></a>DODAJE zalecenia

Określone zalecenia dotyczące implementacji usługi Active Directory w chmurze, można znaleźć w dokumencie [Rozszerzanie usługi Active Directory Azure][extending-ad-to-azure]. Artykuł [wskazówek dotyczących wdrażania systemu Windows Server usługi Active Directory na maszyn wirtualnych Azure] [ ad-azure-guidelines] zawiera dodatkowe informacje szczegółowe.

### <a name="trust-recommendations"></a>Ufaj zalecenia

Domeny lokalnej są zawarte w różnych las z domenami w chmurze. Aby włączyć uwierzytelnianie użytkowników lokalnych w chmurze, domenami w chmurze musi ufać domenie logowania w las lokalnego. Analogicznie jeśli chmury udostępnia domena logowania dla użytkowników zewnętrznych, może być konieczne dla las lokalnego zaufać domenie chmury.

Możesz ustanowić zaufania na poziomie las, [tworząc relacje zaufania las][creating-forest-trusts], lub na poziomie domeny, [tworząc zaufania zewnętrzne][creating-external-trusts]. Zaufanie poziomu las tworzy relację między wszystkimi domenami dwoma lasami. Poziom zaufania domeny zewnętrznej tworzy tylko relację między dwoma określonych domen. Należy utworzyć tylko domeny zewnętrznej poziomu zaufania między domenami w różnych lasach.

Relacje zaufania mogą być jednokierunkowe (jednokierunkowe) lub dwukierunkowym (dwukierunkowe):

- Zaufanie jednokierunkowe umożliwia użytkownikom w jednej domeny lub las (nazywanego *przychodzące* domeny lub las) na dostęp do zasobów przechowywanych w innym ( *wychodzących* domeny lub las). 

- Dwukierunkowe zaufanie umożliwia użytkownikom w domenie lub las uzyskać dostęp do zasobów przechowywanych w innych.

W poniższej tabeli podsumowano konfiguracji zaufania w niektórych scenariuszach dotyczących proste:

| Scenariusz | W lokalnym zaufania | Zaufania chmury |
|----------|-------------------|-------------|
| W lokalnym użytkownicy muszą mieć dostęp do zasobów w chmurze, ale nie odwrotnie | Jednokierunkowe, przychodzące | Jednokierunkowe, wychodzące |
| Użytkownicy w chmurze potrzebują dostępu do zasobów lokalnych, ale nie na odwrót | Jednokierunkowe, wychodzące | Jednokierunkowe, przychodzące |
| Użytkownicy w chmurze i lokalnych wymaga dostępu do zasobów przechowywanych w chmurze i lokalnych | Dwukierunkowa, przychodzące i wychodzące | Dwukierunkowa, przychodzące i wychodzące |

## <a name="security-considerations"></a>Zagadnienia dotyczące zabezpieczeń

Las poziomu zaufania są przechodnie. Jeśli ustanowić zaufanie poziomu las między las lokalnego i las w chmurze, to zaufanie jest używane w innych nowych domen utworzonych w obu las. Jeśli korzystasz z domen zapewniania podziału ze względów bezpieczeństwa, rozważ utworzenie zaufania tylko na poziomie domeny. Poziom zaufania domen są nieprzechodnie.

Dla określonych AD zagadnienia dotyczące zabezpieczeń, zobacz sekcję *Zagadnienia dotyczące zabezpieczeń* w [Rozszerzanie usługi Active Directory Azure][extending-ad-to-azure].

## <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Wdrożenie co najmniej dwa kontrolery domen dla każdej domeny. Dzięki temu automatyczne replikacji między serwerami. Tworzenie dostępności ustawieniem maszyny wirtualne, działające jako serwery AD obsługują każdej domeny. Upewnij się, że są co najmniej dwa serwery w zestawie. 

Ponadto należy rozważyć wyznaczenie jednego lub kilku serwerów w każdej domenie jako [Wzorce operacji] [ standby-operations-masters] w przypadku łączności z serwerem uatrakcyjniając rolę FSMO kończy się niepowodzeniem.

## <a name="scalability-considerations"></a>Zagadnienia dotyczące skalowalność

AD jest automatycznie skalowalna w przypadku sterowników domeny, które są częścią tej samej domeny. Żądania są rozmieszczane wszystkich kontrolerach w domenie. Możesz dodać innego kontrolera domeny, a następnie automatycznie synchronizowane z domeną. Nieskonfigurowane równoważenia obciążenia osobnych, aby skierować ruch do kontrolerów w tej domenie. Upewnij się, że wszystkie kontrolery wystarczających zasobów pamięci i miejsca do magazynowania do obsługi bazy danych domeny. Wprowadź wszystkie kontrolera domeny maszyny wirtualne ten sam rozmiar.

## <a name="management-and-monitoring-considerations"></a>Zarządzanie i monitorowanie zagadnienia

Uzyskać informacji na temat zarządzania i monitorowania zagadnienia, zobacz odpowiedniej sekcji [Rozszerzanie usługi Active Directory Azure][extending-ad-to-azure]. 

Aby uzyskać dodatkowe informacje, zobacz [Monitorowanie Active Directory][monitoring_ad]. Możesz zainstalować narzędzi, takich jak [Systemy Centrum] [ microsoft_systems_center] na serwerze monitorowania w podsieci zarządzania, aby ułatwić wykonywanie następujących zadań. 

## <a name="solution-components"></a>Składniki rozwiązania

Przykładowy skrypt rozwiązanie, [Rozmieszczanie ReferenceArchitecture.ps1][solution-script], jest dostępny, której można zaimplementować architektura znajdujący się zalecenia opisane w tym artykule. Ten skrypt wykorzystuje szablony Azure Menedżera zasobów. Szablony są dostępne jako zestaw podstawowych bloków konstrukcyjnych, z których każdy będzie wykonuje określonych akcji, takich jak tworzenie VNet lub konfigurowanie NSG. Przeznaczenie skrypt należy dodać akompaniament rozmieszczania szablonu.

Szablony są sparametryzowane z parametrami przechowywanych w oddzielnym pliku JSON. Możesz zmienić parametrów w tych plików, aby skonfigurować wdrożenie do własnych wymagań. Nie trzeba zmienić szablony się. Nie można zmienić schematy obiektów w plikach parametru.

Podczas edytowania szablonów tworzyć obiektów, które należy wykonać konwencje nazewnictwa opisanego w [Zalecane konwencje nazewnictwa dla zasobów Azure][naming-conventions].

Rozwiązanie przykładowe tworzy i konfiguruje środowiska usługi w chmurze, który wykonuje domenę o nazwie *treyresearch.com*. Tego środowiska obejmuje Brama VPN podsieci i serwery, strefy Zdemilitaryzowanej warstwa, warstwa firm i składniki warstwy danych programu access, DODAJE i zarządzania warstwy. Przykładowe rozwiązanie zawiera również opcjonalnym dotyczący tworzenia w środowisku lokalnym symulowane przy użyciu własnej domeny *contoso.com*. Rozwiązanie zawiera skrypty ustanowienia relacji zaufania tych domen, która umożliwia użytkownikom lokalnego dostępu do obiektów w domenie *treyresearch.com* w chmurze.

W poniższych sekcjach opisano elementy w lokalnej i w chmurze konfiguracji.

### <a name="on-premises-components"></a>Składniki lokalnego

>[AZURE.NOTE] Składniki są głównym celem architektura opisanych w tym dokumencie i znajdują się po prostu, aby mieć możliwość testowania środowiska chmury bezpiecznie, a nie przy użyciu środowisku produkcyjnym rzeczywistą. Z tego powodu w tej sekcji przedstawiono tylko pliki parametru klucza. Można modyfikować ustawienia, takie jak adresy IP lub rozmiarów maszyny wirtualne, ale zaleca się pozostawienie wiele innych parametrów bez zmian.

Las AD domeny *contoso.com* składa się z tego środowiska. Domena zawiera dwa serwery DODAJE z adresami IP 192.168.0.4 i 192.168.0.5. Te dwa serwery również uruchomić usługę DNS. Lokalnego konta administratora na obu maszyny wirtualne jest nazywany `testuser` przy użyciu hasła `AweS0me@PW`. Ponadto konfiguracji konfiguruje bramą VPN do łączenia się z VNet w chmurze. Konfiguracja można modyfikować, edytując następujące pliki JSON znajduje się w [**parametrów onpremise**] [ on-premises-folder] folder:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Ten plik definiuje przestrzeń adresów sieciowych w środowisku lokalnym.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Ten plik zawiera konfigurację maszyny wirtualne lokalnej usługi DODAJE obsługi. Domyślnie są tworzone dwa *standardowe-DS3-wersji 2* maszyny wirtualne.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** i ** [connection.parameters.json][on-premises-connection-parameters]**. Te pliki przytrzymaj ustawień połączenia VPN do bramy Azure VPN w chmurze, w tym klucz może być używany do ochrony ruchu przechodzenie bramy.

Pozostałe pliki w folderze zawierają informacje o konfiguracji użyte do utworzenia domeny lokalnego (*contoso.com*), za pomocą tej infrastruktury. Możesz używać ich do zainstalowania DODAJE, ustawienia DNS i utworzyć las lokalnego.

Rozwiązanie używa też poniższy skrypt o nazwie [przychodzące trust.ps1][incoming-trust], które są uruchamiane na komputerze w domenie lokalnej:

```Powershell
# Run the following powershell script in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)

$TrustedDomainName = "contoso.com"
#$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

$TrustingDomainName = "treyresearch.com"
$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustingDomainDnsIpAddresses
$ForwardDomainName = $TrustingDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Ten skrypt dodaje adresów IP serwerów usług AD DS w chmurze (zobacz następną sekcję) do lokalnej usługi DNS, a następnie używa [netdom] [ netdom] polecenie, aby utworzyć przychodzące zaufanie jednokierunkowe z domeny w chmurze (*treyresearch.com*).

### <a name="cloud-components"></a>Składniki chmury

Składniki formularza podstawową tej architektury. One konfiguracji infrastruktury dla domeny *treyresearch.com* i tworzyć relacje zaufania w lokalnej domenie *contoso.com* . [**Parametry azure**] [ azure-folder] folder zawiera następujące pliki parametru dotyczące konfigurowania tych składników:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Ten plik definiuje strukturę VNet maszyny wirtualne i innych składników w chmurze. Zawiera ustawień, takich jak nazwa, przestrzeni adresów, podsieci i adresy wszelkie wymagane serwery DNS. Adresy DNS wyświetlane w tym przykładzie Kompendium adresy IP lokalne serwery DNS, a także domyślnego serwera Azure DNS. Modyfikowanie te adresy, aby odwołać ustawień DNS, jeśli nie używasz przykładowe lokalnego środowiska:
    
    ```json
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg",
        "addressPrefixes": [
          "10.0.0.0/16"
        ],
        "subnets": [
          {
            "name": "dmz-private-in",
            "addressPrefix": "10.0.0.0/27"
          },
          {
            "name": "dmz-private-out",
            "addressPrefix": "10.0.0.32/27"
          },
          {
            "name": "dmz-public-in",
            "addressPrefix": "10.0.0.64/27"
          },
          {
            "name": "dmz-public-out",
            "addressPrefix": "10.0.0.96/27"
          },
          {
            "name": "mgmt",
            "addressPrefix": "10.0.0.128/25"
          },
          {
            "name": "GatewaySubnet",
            "addressPrefix": "10.0.255.224/27"
          },
          {
            "name": "web",
            "addressPrefix": "10.0.1.0/24"
          },
          {
            "name": "biz",
            "addressPrefix": "10.0.2.0/24"
          },
          {
            "name": "data",
            "addressPrefix": "10.0.3.0/24"
          },
          {
            "name": "adds",
            "addressPrefix": "10.0.4.0/27"
          }
        ],
        "dnsServers": [
          "10.0.4.4",
          "10.0.4.5",
          "168.63.129.16"
        ]
      }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Ten plik umożliwia skonfigurowanie maszyny wirtualne, uruchomiony DODAJE w chmurze. Konfiguracja składa się z dwóch maszyny wirtualne. Zmienianie nazwy użytkownika Administrator i hasła w `virtualMachineSettings` sekcję, a opcjonalnie można zmodyfikować rozmiar pamięci Wirtualnej zgodnie z wymaganiami dotyczącymi domeny:

    Aby uzyskać więcej informacji, zobacz [Rozszerzanie usługi Active Directory Azure][extending-ad-to-azure].
    
    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-ad",
        "computerNamePrefix": "aad",
        "size": "Standard_DS3_v2",
        "osType": "Windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "adds",
            "privateIPAllocationMethod": "Static",
            "startingIPAddress": "10.0.4.4",
            "enableIPForwarding": false,
            "dnsServers": [
            ],
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 127,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [
        ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-as"
        }
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ```

- ** [Dodawanie dodaje domeny controller.parameters.json][add-adds-domain-controller-parameters]**. Ten plik zawiera ustawienia dotyczące tworzenia domeny *treyresearch.com* obejmujące serwery DODAJE. Używa rozszerzeń niestandardowych ustanawiania domeny i Dodaj serwery DODAJE do niego. Chyba że możesz utworzyć dodatkowe serwery DODAJE (w tym przypadku należy dodać je do `vms` tablica), zmienianie ich nazw w domyślnej lub chcesz utworzyć domenę pod inną nazwą, nie trzeba modyfikować ten plik.

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Ten plik zawiera ustawienia służące do tworzenia bramy Azure VPN w chmurze używane do łączenia się z siecią w lokalnej. Należy zmodyfikować `sharedKey` wartość w `connectionsSettings` sekcji z urządzenia VPN lokalnego. Aby uzyskać więcej informacji, zobacz [implementacji hybrydowych architektury sieci przy użyciu Azure i lokalnych VPN][hybrid-azure-on-prem-vpn].

- ** [strefy zdemilitaryzowanej private.parameters.json] [ dmz-private-parameters] ** i ** [strefy zdemilitaryzowanej public.parameters.json ] [ dmz-public-parameters] **. Te pliki Konfigurowanie przychodzącej (publiczna) i ruchu wychodzącego strony (prywatnych) maszyny wirtualne, które składają się strefy Zdemilitaryzowanej, ochrona serwerów w chmurze. Aby uzyskać więcej informacji na temat tych elementów i ich konfiguracji, zobacz [implementacji strefy Zdemilitaryzowanej między Azure i Internet][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer web.parameters.json][loadBalancer-web-parameters]**, ** [loadBalancer biz.parameters.json][loadBalancer-biz-parameters]**, i ** [loadBalancer data.parameters.json][loadBalancer-data-parameters]**. Te pliki parametry zawierają specyfikacje maszyn wirtualnych dla poziomów dostępu do sieci web, firm i danych, a następnie skonfiguruj urządzenia do równoważenia obciążenia dla każdego poziomu. Są to maszyny wirtualne, które obsługi aplikacji sieci web i baz danych i wykonać obciążenia firm dla organizacji. Maszyny wirtualne w warstwie sieci web są dodawane do domeny w chmurze przy użyciu ustawienia określone w ** [sieci web — maszyn wirtualnych domeny join.parameters.json] [ web-vm-domain-join-parameters] ** pliku.

    Każdy plik zawiera dwa zestawy parametrów konfiguracji. `virtualMachineSettings` Sekcja definiuje maszyny wirtualne obsługujące usługi ADFS w chmurze. Domyślnie skrypt tworzy dwóch z tych maszyny wirtualne w ten sam zestaw dostępności. Następujące fragmenty Pokaż odpowiednich części pliku *loadBalancer web.parameters.json* :

    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-web",
        "computerNamePrefix": "web",
        "size": "Standard_DS1_v2",
        "osType": "windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "web",
            "privateIPAllocationMethod": "Dynamic",
            "isPrimary": "true",
            "enableIPForwarding": false,
            "dnsServers": [ ]
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [ ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-web-vm-as"
        }
      }
    },
    ...
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ````
    Można modyfikować rozmiary i liczby maszyny wirtualne w każdej warstwie, zgodnie z własnymi potrzebami.

    `loadBalancerSettings` Sekcja zawiera opis usługi równoważenia obciążenia dla tych maszyny wirtualne. Usługi równoważenia obciążenia przekazuje ruch, który pojawia się na porcie 80 (HTTP) i na porcie 443 (HTTPS) do jednego lub drugiego maszyny wirtualne. 

    >[AZURE.NOTE] Dla reguły portu 80 używa połączenia TCP zamiast HTTP. Jest to spowodowane instalacji usług IIS w warstwie sieci web jest skonfigurowana do obsługi uwierzytelniania systemu Windows tylko. Uwierzytelnianie anonimowe jest wyłączona. Próby *ping* portu 80 za pośrednictwem połączenia HTTP kończy się niepowodzeniem z błędem (nieautoryzowanego) 401 konieczne tylko przy użyciu połączenia protokołu TCP wykryje, czy port jest aktywny:

    ```json
    "loadBalancerSettings": {
      "value": {
        "name": "ra-adtrust-web-lb",
        "frontendIPConfigurations": [
          {
            "name": "ra-adtrust-web-lb-fe",
            "loadBalancerType": "internal",
            "internalLoadBalancerSettings": {
              "privateIPAddress": "10.0.1.254",
              "subnetName": "web"
            }
          }
        ],
        "backendPools": [
          {
            "name": "ra-adtrust-web-lb-bep",
            "nicIndex": 0
          }
        ],
        "loadBalancingRules": [
          {
            "name": "http-rule",
            "frontendPort": 80,
            "backendPort": 80,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "http-probe",
            "enableFloatingIP": false
          },
          {
            "name": "https-rule",
            "frontendPort": 443,
            "backendPort": 443,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "https-probe",
            "enableFloatingIP": false
          }
        ],
        "probes": [
          {
            "name": "http-probe",
            "port": 80,
            "protocol": "Tcp",
            "requestPath": null
          },
          {
            "name": "https-probe",
            "port": 443,
            "protocol": "Tcp",
            "requestPath": null
          }
        ],
        "inboundNatRules": [ ]
      }
    }
    ```

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Ten plik zawiera konfigurację skoku pola i zarządzania maszyny wirtualne. Tylko istnieje możliwość uzyskania logowania i dostęp administracyjny do maszyny wirtualne w sieci web, firm i poziomów danych w polu skok. Domyślnie skrypt tworzy pojedynczy *Standard_DS1_v2* maszyn wirtualnych, ale można modyfikować ten plik, aby utworzyć maszyny wirtualne zwiększenie lub dodatkowe, jeśli obciążenie pracą zarządzania prawdopodobnie znacząca.

Konfiguracja używa też [wychodzącej trust.ps1] [ outgoing-trust] skrypt pokazane poniżej, aby utworzyć jednokierunkowe zaufanie wychodzące w domenie *contoso.com* :

```powershell
# prerequiste: 
# You need to first run incoming-trust.ps1 in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)
# Then,
# Run the following powershell script in ra-adtrust-ad-vm1 (ip 10.0.4.4)

$TrustedDomainName = "contoso.com"
$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

#$TrustingDomainName = "treyresearch.com"
#$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustedDomainDnsIpAddresses
$ForwardDomainName = $TrustedDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

#netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Ten skrypt jest podobna do skryptu *przychodzący trust.ps1* opisanych wcześniej. Dodaje adresów IP w lokalnej serwer usług AD DS do lokalnej usługi DNS, a następnie używa [netdom] [ netdom] polecenie, aby utworzyć wychodzących relacji zaufania.

## <a name="solution-deployment"></a>Wdrożenie rozwiązania

Rozwiązanie zakłada następujące wymagania:

- Masz istniejącej subskrypcji Azure, w którym można tworzyć grup zasobów.

- Zostały pobrane i zainstalowane najnowszej kompilacji programu Azure Powershell. Zobacz [tutaj] [ azure-powershell-download] instrukcje.

Aby uruchomić skrypt, który wdraża rozwiązanie:

1. Przenieś do folderu wygodny na komputerze lokalnym i Utwórz następujące podfoldery:

    - Skryptów

    - Skryptów i parametrów

    - Onpremise skryptów i parametrów

    - Azure skryptów i parametrów

2. Pobierz [Rozmieszczanie ReferenceArchitecture.ps1] [ solution-script] pliku do folderu skryptów

3. Pobierz zawartość [parametrów onpremise] [ on-premises-folder] folderu skryptów i parametrów/Onpremise:

4. Pobierz zawartość [parametrów azure] [ azure-folder] folderu skryptów i parametrów-Azure.

5. Edytowanie pliku ReferenceArchitecture.ps1 rozmieszczanie w folderze skryptów, a następnie Zmień następujące linii, aby określić grupy zasobów, które powinny zostać utworzona lub służy do przechowywania zasobów utworzone przez skrypt:

    ```powershell
    # Azure Onpremise Deployments
    $onpremiseNetworkResourceGroupName = "ra-adtrust-onpremise-rg"

    # Azure ADDS Deployments
    $azureNetworkResourceGroupName = "ra-adtrust-network-rg"
    $workloadResourceGroupName = "ra-adtrust-workload-rg"
    $securityResourceGroupName = "ra-adtrust-security-rg"
    $addsResourceGroupName = "ra-adtrust-adds-rg"
    ```

6. Możesz edytować plików parametr w folderach skryptów i parametrów-Azure i skryptów i parametrów-Onpremise. Aktualizowanie odwołań grupa zasobów w tych plików, aby dopasować nazwy grup zasobów przydzielonych do zmiennych w pliku ReferenceArchitecture.ps1 rozmieszczanie. Poniższa tabela przedstawia pliki, które parametru odwołanie grupę zasobów. *Pomoc zdalna adfs-obciążenie pracą rg*, *Pomoc zdalna adfs zabezpieczenia rg* *Pomoc zdalna adfs dodaje rg*, *Pomoc zdalna adfs-adfs-rg*i *Pomoc zdalna adfs-proxy-rg* grup zasobów są używane tylko w skrypt programu PowerShell i nie występują w plikach parametru.

  	|Grupa zasobów|Parametr plików|
  	|--------------|--------------|
  	|Pomoc zdalna adtrust-onpremise-rg|parameters\onpremise\connection.parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\onpremise\virtualNetwork.parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON
  	|Pomoc zdalna adtrust sieć rg|parameters\onpremise\connection.parameters.JSON<br />parameters\azure\dmz-Private.parameters.JSON<br />parameters\azure\dmz-public.parameters.JSON<br />parameters\azure\loadBalancer-Biz.parameters.JSON<br />parameters\azure\loadBalancer-Data.parameters.JSON<br />parameters\azure\loadBalancer-Web.parameters.JSON<br />parameters\azure\virtualMachines-ADDS.parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.parameters.JSON<br />parameters\azure\virtualNetwork-ADDS-DNS.parameters.JSON<br />parameters\azure\virtualNetwork.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON (*dwa wystąpienia*)

    Ponadto konfiguracji dla lokalnego i w chmurze składniki, zgodnie z opisem w [Składniki rozwiązania] [ solution-components] sekcji.

7. Otwórz okno programu PowerShell Azure, przejdź do folderu skryptów i uruchom następujące polecenie:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Zamienianie `<subscription id>` ze swojego identyfikatora Azure subskrypcji.

    Aby uzyskać `<location>`, określ Azure regionu, takich jak `eastus` lub `westus`.

    `<mode>` Parametr może mieć jedną z następujących wartości:

    - `Onpremise`, aby utworzyć symulowany lokalnego środowiska.

    - `Infrastructure`, tworzyć infrastruktury VNet i przejść pola w chmurze.

    - `CreateVpn`, aby utworzyć bramę Azure wirtualnej sieci i połączyć go do sieci lokalnej.

    - `AzureADDS`, sporządzenie maszyny wirtualne, działające jako serwery DODAJE wdrożenia usługi Active Directory, aby te maszyny wirtualne i tworzenia domeny w chmurze.

    - `WebTier`, który tworzy sieci web poziomu maszyny wirtualne i Usługa równoważenia obciążenia.

    - `Prepare`, który wykonuje wszystkie poprzednie zadania. **Jeśli tworzysz całkowicie nowych wdrożenia i nie mam istniejącej infrastruktury lokalnej jest zalecana opcja.**

    - `Workload`Aby utworzyć warstwie maszyny wirtualne firm i danych i urządzenia do równoważenia obciążenia. Te maszyny wirtualne nie są częścią `Prepare` opcji.

    >[AZURE.NOTE] Jeśli korzystasz z `Prepare` opcji skrypt trwa kilka godzin do wykonania.

8.  Jeśli używasz konfiguracji lokalnego przykładowe:

    1. Nawiązywanie połączenia w polu skok (*Pomoc zdalna adtrust zarządzania vm1* w grupie zasobów *Pomoc zdalna adtrust zabezpieczeń rg* ). Zaloguj się jako *testuser* przy użyciu hasła *AweS0me@PW*.

    2.  W oknie dialogowym skoku otworzyć sesję RDP na pierwszym maszyn wirtualnych w domenie *contoso.com* (domena lokalnego). Ten maszyn wirtualnych ma adres IP 192.168.0.4. Nazwa użytkownika jest *contoso\testuser* przy użyciu hasła *AweS0me@PW*.

    3. Pobierz [przychodzące trust.ps1] [ incoming-trust] skrypt i uruchom go, aby utworzyć zaufanie przychodzące z domeny *treyresearch.com* .

9. Jeśli korzystasz z infrastruktury lokalnego:

    1. Pobierz [przychodzące trust.ps1] [ incoming-trust] skrypt.

    2. Edytowanie skryptu i Zamień wartość `$TrustedDomainName` zmienną o nazwie własnej domeny.

    3. Uruchom skrypt.

10. W polu skok Podłącz do pierwszej maszyn wirtualnych w *treyresearch.com* domain (domena w chmurze). Ten maszyn wirtualnych ma adres IP 10.0.4.4. Nazwa użytkownika jest *treyresearch\testuser* przy użyciu hasła *AweS0me@PW*.

11. Pobierz [wychodzących trust.ps1] [ outgoing-trust] skrypt i uruchom go, aby utworzyć zaufanie przychodzące z domeny *treyresearch.com* . Jeśli używasz komputery lokalnych, należy najpierw edytować skrypt. Ustawianie `$TrustedDomainName` zmiennej do nazwy domeny lokalnej i określić adresy IP serwerów usług AD DS dla tej domeny w `$TrustedDomainDnsIpAddresses` zmiennej.

12. Na komputerze lokalnym, wykonaj czynności opisane w artykule [Weryfikowanie zaufania] [ verify-a-trust] do określenia, czy relacja zaufania został poprawnie skonfigurowany między domen *contoso.com* i *treyresearch.com* . Może być konieczne Poczekaj kilka minut po wykonaniu powyższych czynności, zanim będzie sprawdzana poprawność zaufania.

Pozostałe opcjonalne pokazano, jak ustalić, czy zaufania domeny działa zgodnie z oczekiwaniami. Poniższe czynności wymagają, masz dostęp do komputera przy użyciu programu Visual Studio zainstalowany.

1.  W oknie Azure programu PowerShell, uruchom następujące polecenie w celu utworzenia warstwie sieci web, jeśli nie skonfigurowano go wcześniej (za pomocą `Prepare` opcja):

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> WebTier
    ```

    To polecenie tworzy warstwa i dodaje maszyny wirtualne do domeny *treyresearch.com* .

2. Za pomocą programu Visual Studio na komputerze programowania, tworzenie aplikacji sieci Web programu ASP.NET o nazwie *TreyResearchWebApp*. Za pomocą programu .NET Framework 4.5.2.

3. Wybierz szablon *MVC* i zmienić uwierzytelnianie uwierzytelniania *Systemu Windows*. Nie należy tworzyć aplikacji usługi w chmurze.

3. Tworzenie i uruchamianie aplikacji, aby przetestować uwierzytelniania. Upewnij się, że nazwę bieżącego użytkownika systemu Windows jest wyświetlany na pasku menu w górnej części strony, w kierunku po prawej stronie.

4. Zamknij program Internet Explorer.

5. W oknie *Eksploratora rozwiązanie* kliknij prawym przyciskiem myszy projektu TreyResearchWebApp, kliknij pozycję *Publikuj*.

6. W oknie *Publikowanie sieci Web* kliknij pozycję *niestandardowe*. Tworzenie niestandardowego profilu o nazwie *TreyResearchWebApp*.

7. Na stronie *połączenie* *metody Publikuj* ustawić system *Plików* i określ folder o nazwie *TreyResearchWebApp*, znajduje się w odpowiednim miejscu na komputerze dewelopera.

8. Na stronie *Ustawienia* ustawienie *konfiguracji* do *wersji*.

9. Na stronie *podglądu* kliknij pozycję *Publikuj*.

10. Nawiązywanie połączenia z każdego maszyn wirtualnych w warstwie sieci web z kolei (za pośrednictwem w polu skok) i wykonywać następujące zadania. Adresy IP sieci web warstwa maszyny wirtualne są 10.0.1.4 i 10.0.1.5. Nazwa użytkownika dla obu maszyny wirtualne jest *treyresearch\testuser* przy użyciu hasła *AweS0me@PW*:

    1. Skopiuj *TreyResearchWebApp* folder i jego zawartość z komputera programowania do folderu *C:\inetpub* .

    2. Przy użyciu konsoli menedżera informacji usług internetowych, przejdź do *witryny sieci Web Sites\Default* na tym komputerze.

    3. W okienku *Akcje* kliknij pozycję *Ustawienia podstawowe*i zmień ścieżki fizycznej witryny sieci web na *%SystemDrive%\inetpub\TreyResearchWebApp*.

    4. W okienku *Widoku funkcji* kliknij dwukrotnie *uwierzytelniania*. Sprawdź, czy jest włączone *Uwierzytelnianie systemu Windows* i *Uwierzytelnianie anonimowe* jest wyłączone.

11. na komputerze lokalnym Otwórz program Internet Explorer i przejdź do witryny sieci web u http://10.0.1.254 (jest to adres równoważenia obciążenia poziomu sieci web).

12. W oknie dialogowym *Zabezpieczenia systemu Windows* wprowadź poświadczenia użytkownika w lokalnej domenie. Określ nazwę użytkownika, który nie istnieje w domenie *treyresearch* . Jeśli używasz symulowany lokalnego środowiska najpierw Utwórz użytkownika w domenie *firmy contoso* i określ poświadczenia użytkownika.

13. Gdy pojawi się Strona główna, upewnij się, że poprawnej domeny i nazwę użytkownika mają być wyświetlane na pasku menu w górnej części strony, w kierunku po prawej stronie.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, najważniejsze wskazówki dotyczące [rozszerzenia domeny lokalnej DODAJE do Azure][adds-extend-domain]

- Dowiedz się, najważniejsze wskazówki dotyczące [tworzenia infrastrukturę ADFS] [ adfs] platformy Azure.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[implementing-aad]: ./guidance-identity-aad.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[running-VMs-for-an-N-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[naming-conventions]: ./guidance-naming-conventions.md
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[netdom]: https://technet.microsoft.com/library/cc835085.aspx
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://github.com/mspnp/reference-architectures/blob/master/guidance-identity-adds-trust/parameters/onpremise/connection.parameters.json
[incoming-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/incoming-trust.ps1
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/outgoing-trust.ps1
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-adds.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/add-adds-domain-controller.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-public.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-mgmt.parameters.json
[web-vm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/web-vm-domain-join.parameters.json
[solution-components]: [#solution_components]
[0]: ./media/guidance-identity-aad-resource-forest/figure1.png "Zabezpieczanie hybrydowych architektury sieci przy użyciu różnych domenach AD"