- Wirtualnych sieci może być w regionach Azure takich samych lub różnych (lokalizacja).

- Usługi w chmurze lub punkt końcowy równoważenia obciążenia nie może obejmować wielu wirtualnych sieci, nawet jeśli są połączone.

- Łączenie wielu sieci wirtualnej Azure nie wymaga bram VPN lokalnego, chyba że jest wymagana łączność między lokalnej.

- VNet-VNet obsługuje łączenia wirtualnej sieci. Nie obsługuje łączenia maszyn wirtualnych lub nie w wirtualnej sieci usługi w chmurze.

- VNet do VNet wymaga bram Azure VPN z RouteBased (wcześniej nazywane Routing dynamiczny) rodzaje sieci VPN. 

- Połączenia wirtualnej sieci można używać jednocześnie z wielu witryn sieci VPN z maksymalnie 10 (domyślnego, standardowego bramy) lub 30 (wysokiej wydajności bramy) VPN tuneluje wirtualnej sieci VPN bramy nawiązywanie połączeń z innymi wirtualnych sieci lokalnej witryny.

- Nie musisz zachodzi obszary adresów wirtualnych sieci i lokalnych witryn sieci lokalnej. Nakładające się spacje adres spowoduje, że VNet do VNet połączenia kończy się niepowodzeniem.

- Zbędne tunele między dwoma wirtualnych sieci nie są obsługiwane.

- Wszystkie tuneli VPN wirtualnej sieci udostępnianie dostępna przepustowość brama Azure VPN i tym samym przestoje Brama VPN SLA platformy Azure.

- Ruch VNet do VNet przesyłania w witrynie Microsoft Network nie Internet.

- Ruch VNet do VNet w ramach tego samego regionu jest bezpłatna dla obu kierunków; krzyżowe region VNet do VNet wyjściowym ruch jest pobierana z szybkości przesyłania danych między VNet wychodzącego według regionów źródła. Zajrzyj do [ceny strony](https://azure.microsoft.com/pricing/details/vpn-gateway/) Aby uzyskać szczegółowe informacje.