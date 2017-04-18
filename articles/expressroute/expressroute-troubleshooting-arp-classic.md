<properties
   pageTitle="ExpressRoute przewodnik rozwiązywania problemów: pobieranie ARP tabel | Microsoft Azure"
   description="Ta strona zawiera instrukcje dotyczące pobieranie ARP tabel dla obwodu ExpressRoute."
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

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>ExpressRoute przewodnik rozwiązywania problemów: pobieranie ARP tabel w modelu Klasyczny wdrażania

> [AZURE.SELECTOR]
[PowerShell — Menedżer zasobów](expressroute-troubleshooting-arp-resource-manager.md)
[programu PowerShell — klasyczny](expressroute-troubleshooting-arp-classic.md)

W tym artykule opisano kroki uzyskiwania tabel rozdzielczości ARP (Address Protocol) dla usługi Azure ExpressRoute obwodu.

>[AZURE.IMPORTANT] Ten dokument jest przeznaczony ułatwiają diagnozowanie i rozwiązywanie problemów dotyczących prosty. Nie ma być wymiana pomocy technicznej firmy Microsoft. Jeśli nie rozwiąże problemu, wykonując poniższe wskazówki, otwórz prośbę o pomoc techniczną z [platformy Microsoft Azure pomocy + pomocy technicznej](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Rozdzielczość Protocol () ARP i tabele adresów
ARP jest protokołem warstwy 2, która jest zdefiniowana w [specyfikacji RFC 826](https://tools.ietf.org/html/rfc826). ARP jest używany do mapowania adresów IP Ethernet adres (MAC).

Tabela ARP zawiera mapowanie adresu IP protokołu IPv4 i adres MAC dla określonego zaglądanie. W tabeli ARP elektrycznego ExpressRoute zaglądanie zawiera następujące informacje dla każdego interfejsu (głównego i pomocniczego):

1. Mapowanie adresu IP lokalnego routera interfejs adres MAC.
2. Mapowanie adresu IP ExpressRoute routera interfejs adres MAC.
3. Wiek mapowania

Tabele ARP może pomóc w z sprawdzanie poprawności konfiguracji warstwy 2 i rozwiązywanie problemów z konfiguracją podstawowe warstwy 2 łączności.

Oto przykład tabeli ARP:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Następujące sekcja zawiera informacje dotyczące sposobu wyświetlania tabel ARP, która ma być wyświetlana przez routery krawędź ExpressRoute.

## <a name="prerequisites-for-using-arp-tables"></a>Wymagania wstępne dotyczące korzystania z tabel ARP

Upewnij się, że masz następujące przed kontynuowaniem:

 - Prawidłowe ExpressRoute obwodu, w którym skonfigurowano zaglądanie co najmniej jeden. Obwód musi być całkowicie skonfigurowany przez dostawcę łączności. Użytkownik (lub dostawcy łączności) należy skonfigurować co najmniej jeden z peerings (Azure publicznej prywatne, Azure lub Microsoft) na ten obwód.

 - Zakresy adresów IP, które są używane do konfigurowania peerings (Azure publicznych prywatne, Azure i Microsoft). Przejrzyj przykłady przydziału adresu IP w [ExpressRoute routingu wymagania dotyczące strony](expressroute-routing.md) , aby uzyskać zrozumienia jak adresy IP są mapowane na interfejsów użytkownika aise i po stronie ExpressRoute. Możesz uzyskać informacje na temat konfiguracji peering przeglądając [ExpressRoute zaglądanie strony konfiguracji](expressroute-howto-routing-classic.md).

 - Informacje od dostawcy sieci zespołu lub łączności adresów MAC interfejsów, które są używane z tych adresów IP.

 - Najnowsze moduł programu Windows PowerShell Azure (wersja 1,50 lub nowszy).

## <a name="arp-tables-for-your-expressroute-circuit"></a>Tabele ARP dla swojego elektrycznego ExpressRoute
Ta sekcja zawiera instrukcje dotyczące wyświetlania tabel ARP dla każdego typu zaglądanie przy użyciu programu PowerShell. Przed kontynuowaniem skonfigurować zaglądanie musi się użytkownik lub dostawcy łączności. Każdy obwód występują dwie ścieżki (głównego i pomocniczego). Możesz sprawdzić tabelę ARP dla każdej ścieżki niezależnie.

### <a name="arp-tables-for-azure-private-peering"></a>Tabele ARP dla Azure zaglądanie prywatnych
Następujące polecenie cmdlet przewiduje ARP tabel Azure zaglądanie prywatne:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Oto przykładowy wynik jedną ze ścieżek:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Tabele ARP dla Azure zaglądanie publicznej:
Następujące polecenie cmdlet przewiduje ARP tabel Azure zaglądanie publicznej:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Oto przykładowy wynik jedną ze ścieżek:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Oto przykładowy wynik jedną ze ścieżek:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Tabele ARP dla zaglądanie firmy Microsoft
Następujące polecenie cmdlet przewiduje ARP tabel zaglądanie firmy Microsoft:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Poniżej przedstawiono przykładowe dane wyjściowe dla jedną ze ścieżek:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Jak skorzystać z tych informacji
Spis ARP zaglądanie może służyć do sprawdzania poprawności konfiguracji warstwy 2 i łącznością. W tej sekcji omówiono wyglądu tabele ARP w różnych scenariuszach.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>Tabela ARP, jeśli łącze jest w stanie operacyjne (oczekiwany)

 - Tabela ARP zawiera wpis stronie lokalnego przy użyciu prawidłowego adresu IP i MAC i wpis podobne stronie firmy Microsoft.
 - Ostatni oktet adresu IP lokalnego zawsze jest nieparzysta.
 - Ostatni oktet adresu IP firmy Microsoft jest zawsze liczbą parzystą.
 - Na stronie firmy Microsoft dla wszystkich trzech peerings (podstawowy i pomocniczy) pojawi się ten sam adres MAC.


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>Tabelę ARP po lokalnego lub po stronie dostawcy łączności problemów

 W tabeli ARP są wyświetlane tylko jeden wpis. Dzięki niemu mapowanie między adres MAC i adres IP, który jest używany na stronie firmy Microsoft.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Jeśli wystąpi problem tak, otwórz prośbę o pomoc techniczną u dostawcy łączności rozwiązania problemu.


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>ARP tabeli po stronie Microsoft występują problemy

 - Nie zobaczą ARP tabelą dla zaglądanie w przypadku problemów na stronie firmy Microsoft.
 -  Otwórz prośbę o pomoc techniczną z [platformy Microsoft Azure pomocy + pomocy technicznej](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Określ, czy masz problem z połączeniem warstwy 2.

## <a name="next-steps"></a>Następne kroki

 - Sprawdź poprawność konfiguracji Layer 3 dla swojego obwodu ExpressRoute:
     - Uzyskaj trasę podsumowania do określenia stanu BGP sesji.
     - Uzyskaj tabeli trasy do określenia, które prefiksy są ogłaszane w ExpressRoute.
 - Sprawdź poprawność transfer danych i zmniejszanie przeglądając bajtów.
 - Jeśli nadal występują problemy, otwórz prośbę o pomoc techniczną z [platformy Microsoft Azure pomocy + pomocy technicznej](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
