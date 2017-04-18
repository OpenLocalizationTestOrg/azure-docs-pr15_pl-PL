## <a name="vpn-gateway"></a>Brama VPN 
Zasób bramy sieci VPN umożliwia tworzenie bezpiecznego połączenia między ich lokalnym centrum danych i Azure. Zasób bramy sieci VPN można skonfigurować na trzy sposoby:
 
- **Wskaż witrynę** — bezpieczny dostęp do zasobów Azure obsługiwany w VNET za pomocą klienta VPN z dowolnego komputera. 
- **Połączenia z serwisem wielu** — bezpiecznie łączyć z centrami danych z lokalnego do zasobów działa w VNET. 
- **VNET do VNET** — można zapewnić bezpieczne połączenie VNETS Azure w ramach tego samego regionu, lub regionów do utworzenia obciążenie pracą z geo nadmiarowości.

Właściwości klucza bramy sieci VPN obejmują:
 
- **Typ bramy** - dynamicznie kierowane lub statycznego routingu bramy. 
- **Prefiks puli adres klienta VPN** — adresy IP ma być przypisane do klientów łączących się w punkcie konfiguracji witryny.