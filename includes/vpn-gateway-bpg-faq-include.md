### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>BGP jest obsługiwane na wszystkich wersji produktów bramy sieci VPN Azure?

Nie, BGP jest obsługiwana w Azure **standardowe** i **o odwróconej** VPN bramy. **Podstawowe** Jednostka SKU nie jest obsługiwane.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Czy mogę korzystać z bram Azure Policy-Based VPN BGP?

Nie, BGP jest obsługiwany w sieci VPN opartych na trasę bram tylko.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Czy mogę używać nich prywatnych (autonomicznego systemu liczby)?

Tak, można używać własnych nich publicznych lub prywatnych nich sieci lokalnej i Azure wirtualnych sieci.

#### <a name="are-there-asns-reserved-by-azure"></a>Czy istnieją nich zarezerwowana przez Azure?

Tak, nich następujące są zarezerwowane przez Azure dla peerings wewnętrzne i zewnętrzne:

- Publicznej nich: 8075 8076, 12076
- Prywatne nich: 65515, 65517, 65518, 65519, 65520

Te z nich nie można określić dla w środowisku lokalnym urządzeń sieci VPN podczas nawiązywania połączenia Azure VPN bramy.

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Czy można używać samej WPW dla sieci VPN lokalnego i Azure VNets?

Nie, należy przypisać różne nich między sieci lokalnej i VNets usługi Azure, jeśli łączysz się je razem z BGP. Azure bram sieci VPN mają domyślnie WPW 65515 przydzielone, czy BGP jest włączona dla nie dla łączność między lokalnej. Można zastąpić to domyślne przypisując różnych WPW podczas tworzenia bramy sieci VPN lub zmienić WPW po utworzeniu bramy. Należy przypisać do nich lokalnego do odpowiednich bram sieci lokalnej Azure.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Jakie prefiksów adresów będzie bram Azure VPN ogłaszanie mnie?

Brama VPN Azure będzie ogłaszanie następujące trasy do urządzenia BGP lokalnego:

- Usługi VNet prefiksów adresów
- Prefiksy adresów dla każdej lokalnej bram sieci połączenie Azure VPN bramy
- Trasy rozpoznane z innych sesji peering BGP połączenie do bramy Azure VPN, **z wyjątkiem domyślną trasę lub trasy nakłada się wszelkie prefiks VNet**.

#### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Czy można ogłaszanie trasy domyślnej (0.0.0.0/0) do bram Azure VPN?

W tym czasie.

#### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Czy można ogłaszanie dokładnie prefiksy, jako Moje prefiksy wirtualnej sieci?

Nie, reklamę prefiksy samą jak wirtualna sieć prefiksów adresów zablokowane lub filtrowane według platformie Azure. Można jednak ogłaszanie prefiks, który jest podzbiorem masz wewnątrz wirtualnej sieci. Na przykład wirtualnej sieci można użyć adresu 10.10.0.0/16 miejsca i może ogłaszanie 10.0.0.0/8.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Czy mogę korzystać z Moje połączenia VNet do VNet BGP?

Tak, można używać BGP zarówno połączenia między lokalnej i połączenia VNet do VNet.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Można można łączyć BGP innych niż BGP połączeń z dla mojej sieci VPN Azure bram

Tak, można mieszać zarówno BGP i innych niż BGP połączenia dla tej samej bramie Azure VPN.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Czy routing Azure VPN bramy pomocy technicznej BGP przewozowe?

Tak, BGP przewozowe routing jest obsługiwany, z wyjątkiem, że Azure VPN bram **nie** będzie ogłaszanie trasy domyślne innych partnerom BGP. Aby włączyć przewozowe routing przez wielu bram Azure VPN, należy włączyć BGP dla wszystkich połączeń pośredniej VNet do VNet.

### <a name="can-i-have-more-than-one-tunnels-between-azure-vpn-gateway-and-my-on-premises-network"></a>Czy można mieć więcej niż jeden tuneli między brama Azure VPN i sieci lokalnej?

Tak, można utworzyć więcej niż jeden tuneli S2S VPN między brama Azure VPN i sieci lokalnej. Pamiętaj, że wszystkie te tunele będą wliczane całkowita liczba tuneli dla bramy sieci Azure VPN. Na przykład jeśli masz dwie tunele zbędne między bramy sieci Azure VPN i jedną z sieci lokalnej ich zajmie 2 tunele poza całkowita przydziału dla bramy sieci Azure VPN (10 standardu) i 30 o odwróconej.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Czy można mieć wiele tuneli między dwoma VNets Azure z BGP?

Nie, zbędne tunele między dwoma wirtualnych sieci nie są obsługiwane.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Czy można używać BGP dla sieci VPN S2S w konfiguracji współistnienie VPN ExpressRoute-S2S

W tym czasie.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Jaki adres bramy Azure VPN używa BGP równorzędnych IP?

Brama Azure VPN będą przydzielane na jeden adres IP z zakresu GatewaySubnet zdefiniowanych dla wirtualnej sieci. Domyślnie jest to druga adres ostatniego zakresu. Na przykład jeśli usługi GatewaySubnet jest 10.12.255.0/27, począwszy od 10.12.255.0 do 10.12.255.31, adres IP równorzędnych BGP brama Azure VPN zostaną 10.12.255.30. Te informacje można znaleźć, gdy wyświetlanie informacji brama Azure VPN.

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>Jakie są wymagania dla adresów IP równorzędnych BGP na moim urządzeniu VPN?

BGP swojej lokalnej równorzędne adres **Nie może** być taka sama jak publiczny adres IP urządzenia VPN. Użyj innego adresu IP na tym urządzeniu VPN dla swój adres IP równorzędnych BGP. Może to być adres przypisany do interfejsu sprzężenia zwrotnego na tym urządzeniu. Określ ten adres w odpowiednich bramy sieci lokalnej reprezentującą lokalizacji.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>Co należy można określić jako Moje prefiksów adresów dla bramy sieci lokalnej użycie BGP?

Azure bramy sieci lokalnej określa prefiksy początkowym adresem sieci lokalnej. Z BGP, musisz przydzielić prefiks hosta (/ 32 prefiks) adresu IP równorzędnych BGP jako przestrzeni adresów dla tej sieci lokalnej. Jeśli swój adres IP równorzędnych BGP jest 10.52.255.254, należy określić "10.52.255.254/32" jako localNetworkAddressSpace bramy sieci lokalnej odpowiadającą tej sieci lokalnej. To, aby upewnić się, że brama Azure VPN ustala sesji BGP za pośrednictwem tunelem S2S VPN.

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>Co można dodać na Moje urządzenie VPN lokalnego dla sesji peering BGP?

Na urządzeniu VPN wskazująca tunelem IPsec S2S VPN, należy dodać trasy hosta adresu IP równorzędnych BGP Azure. Na przykład IP równorzędnych VPN Azure jest "10.12.255.30", należy dodać trasy hosta dla "10.12.255.30" z interfejsem następny skok pasujące interfejsu tunelem IPsec na urządzeniu VPN.
