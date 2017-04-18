<properties 
   pageTitle="Przewodnik Rozwiązywanie problemów z ExpressRoute - pobieranie ARP tabel | Microsoft Azure"
   description="Ta strona zawiera instrukcje dotyczące uzyskiwania ARP tabel dla obwodu ExpressRoute"
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="ganesr"/>

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>Rozwiązywanie problemów z ExpressRoute przewodniku — wprowadzenie ARP tabel w modelu wdrożenia Menedżera zasobów

> [AZURE.SELECTOR]
[PowerShell — Menedżer zasobów](expressroute-troubleshooting-arp-resource-manager.md)
[programu PowerShell — klasyczny](expressroute-troubleshooting-arp-classic.md)

W tym artykule opisano czynności, aby dowiedzieć się, tabele ARP dla swojego elektrycznego ExpressRoute. 

>[AZURE.IMPORTANT] Ten dokument jest przeznaczony ułatwiają diagnozowanie i rozwiązywanie problemów dotyczących prosty. Nie ma być wymiana pomocy technicznej firmy Microsoft. Jeśli nie możesz rozwiązać problemu przy użyciu wskazówki opisane poniżej, musisz otworzyć bilet pomocy technicznej z [pomocy technicznej firmy Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Rozdzielczość Protocol () ARP i tabele adresów
Rozdzielczość ARP (Address Protocol) jest protokołem warstwy 2 zdefiniowane w [specyfikacji RFC 826](https://tools.ietf.org/html/rfc826). ARP jest używany do zamapować Ethernet adres (MAC) z adresem ip.

Tabela ARP zawiera mapowanie adresu IP protokołu ipv4 i adres MAC dla określonego zaglądanie. W tabeli ARP elektrycznego ExpressRoute zaglądanie zawiera następujące informacje dla każdego interfejsu (głównego i pomocniczego)

1. Mapowania adresów ip interfejsu routera lokalnej na adres MAC
2. Mapowania adresów ip interfejsu routera ExpressRoute adres MAC.
3. Wiek mapowania

Tabele ARP może pomóc sprawdzić poprawność konfiguracji warstwy 2 i rozwiązywanie problemów z podstawowe warstwy 2 problemy z łącznością. 

Przykładowa tabela ARP: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Następną sekcję zawiera informacje dotyczące sposobu wyświetlania tabel ARP widoczne routerom ExpressRoute krawędzi. 

## <a name="prerequisites-for-learning-arp-tables"></a>Wymagania wstępne dotyczące nauki tabele ARP

Zapewnienie, że mają następujące przed postępu w dalszej

 - Skonfigurowano zaglądanie co najmniej jeden obwód ExpressRoute prawidłowe. Obwód musi być całkowicie skonfigurowany przez dostawcę łączności. Użytkownik (lub dostawcy łączności) należy skonfigurować co najmniej jeden z peerings (Azure publicznych prywatne, Azure i Microsoft) w tym elektrycznego.
 - Zakresy adresów IP dla peerings (Azure publicznych prywatne, Azure i Microsoft). Przejrzyj przykłady przydziału adresu ip w [ExpressRoute routingu wymagania dotyczące strony](expressroute-routing.md) , aby uzyskać zrozumienia jak adresy ip są mapowane na interfejsów na Twojej stronie i na stronie ExpressRoute. Dowiedz się o peering konfiguracji, przeglądając [strony konfiguracji zaglądanie ExpressRoute](expressroute-howto-routing-arm.md).
 - Informacje z zespołem sieci / dostawcy łączności na adresy MAC interfejsów używane z tych adresów IP.
 - Musi mieć najnowszą modułu programu PowerShell Azure (wersja 1,50 lub nowszy).

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Pobieranie ARP tabel dla swojego obwodu ExpressRoute
Ta sekcja zawiera instrukcje dotyczące sposobu wyświetlania tabel ARP na zaglądanie przy użyciu programu PowerShell. Możesz lub dostawcy łączności należy skonfigurować zaglądanie przed przebiegają dalej. Każdy obwód występują dwie ścieżki (głównego i pomocniczego). Możesz sprawdzić tabelę ARP dla każdej ścieżki niezależnie.

### <a name="arp-tables-for-azure-private-peering"></a>Tabele ARP dla Azure zaglądanie prywatnych
Następujące polecenie cmdlet przewiduje ARP tabel Azure zaglądanie prywatnych

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Przykładowe dane wyjściowe przedstawiono poniżej jedną ze ścieżek

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Tabele ARP dla Azure zaglądanie publicznej
Następujące polecenie cmdlet przewiduje ARP tabel Azure zaglądanie publicznej

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Przykładowe dane wyjściowe przedstawiono poniżej jedną ze ścieżek

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Tabele ARP dla zaglądanie firmy Microsoft
Następujące polecenie cmdlet przewiduje ARP tabel zaglądanie firmy Microsoft

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Przykładowe dane wyjściowe przedstawiono poniżej jedną ze ścieżek

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Jak skorzystać z tych informacji
Spis ARP zaglądanie może służyć do określenia sprawdzić poprawność konfiguracji warstwy 2 i łącznością. W tej sekcji omówiono wyglądu tabele ARP w różnych scenariuszach.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>Tabelę ARP, gdy łącze jest w stanie operacyjne (oczekiwany stan)

 - Tabelę ARP będzie mieć wpis dla siebie lokalnych z prawidłowy adres IP i adres MAC i podobne wpis na stronie firmy Microsoft. 
 - Ostatni oktet adresu ip lokalnego zawsze jest nieparzysta.
 - Ostatni oktet adresu ip firmy Microsoft jest zawsze liczbą parzystą.
 - Taki sam adres MAC. pojawią się na stronie firmy Microsoft dla wszystkich peerings 3 (podstawowy / pomocniczej). 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>Tabela ARP, kiedy lokalnego / łączności dostawcy stronie występują problemy

 - W tabeli ARP pojawi się tylko jeden wpis. To zostanie wyświetlona mapowanie między adresu MAC oraz adres IP używany na stronie firmy Microsoft. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Otwórz prośbę o pomoc techniczną u dostawcy łączności debugowania tych problemów. 


### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP tabeli po stronie Microsoft występują problemy

 - Nie zobaczą ARP tabelą dla zaglądanie w przypadku problemów na stronie firmy Microsoft. 
 -  Otwórz bilet pomocy technicznej z [pomocy technicznej firmy Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Określ, czy masz problem z połączeniem warstwy 2. 

## <a name="next-steps"></a>Następne kroki

 - Sprawdź poprawność konfiguracji Layer 3 dla swojego obwodu ExpressRoute
     - Uzyskiwanie rozsyłania podsumowania do określenia stanu BGP sesji 
     - Uzyskiwanie rozsyłania tabeli, aby określić, które prefiksy są ogłaszane w ExpressRoute
 - Sprawdź poprawność transfer danych, przeglądania bajtów / się
 - Jeśli nadal występują problemy, otwórz bilet pomocy technicznej z [pomocy technicznej firmy Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
