<properties 
   pageTitle="Rozwiązywanie problemów z grupami zabezpieczeń sieci - programu PowerShell | Microsoft Azure"
   description="Dowiedz się, jak rozwiązywać problemy z grupami zabezpieczeń sieci w modelu wdrożenia Menedżera zasobów Azure przy użyciu programu PowerShell Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Rozwiązywanie problemów z grupami zabezpieczeń sieci przy użyciu programu PowerShell Azure

> [AZURE.SELECTOR]
- [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
- [Programu PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Jeśli skonfigurowano grup zabezpieczeń sieci (NSGs) na komputerze wirtualnych (maszyn wirtualnych) i występują problemy z łącznością maszyn wirtualnych, w tym artykule omówiono możliwości diagnostyki NSGs w celu dalszego.

NSGs umożliwiają kontrolowanie typów ruchu tego przepływu i usługi wirtualnych maszyn. NSGs mogą być stosowane do podsieci w wirtualnej sieci Azure (VNet) i/lub interfejsów sieciowych (NIC). Skuteczne reguły stosowane do NIC są zagregowane reguły, które znajdują się w NSGs zastosowane do NIC i podsieć, z którą jest połączony. Reguły w tych NSGs może czasami powodują konflikt i wpływają na łączność sieciowa maszyn wirtualnych.  

Można wyświetlić wszystkie reguły skutecznych zabezpieczeń z usługi NSGs stosowana w swojej maszyn wirtualnych nic. W tym artykule pokazano, jak rozwiązywać problemy z łącznością maszyn wirtualnych za pomocą tych reguł w modelu wdrożenia Azure Menedżera zasobów. Jeśli nie znasz VNet i NSG pojęcia, przeczytaj artykuły omówienie [wirtualnych sieci](virtual-networks-overview.md) i [grupy zabezpieczeń sieci](virtual-networks-nsg.md) .

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Rozwiązywanie problemów z ruch maszyn wirtualnych za pomocą skuteczne reguły zabezpieczeń

Scenariusz, w którym występuje jest przykładem typowych problem z połączeniem:

Maszyny o nazwie *VM1* jest częścią podsieć o nazwie *podsieć1* w VNet o nazwie *WestUS VNet1*. Próba nawiązania połączenia maszyn wirtualnych, przy użyciu RDP przez TCP port 3389 zakończy się niepowodzeniem. NSGs są stosowane na zarówno NIC *VM1 NIC1* , jak i podsieć *podsieć1*. Ruch na TCP port 3389 jest dozwolony w NSG skojarzone z sieciowej *VM1 NIC1*, jednak TCP ping do VM1 na porcie 3389 kończy się niepowodzeniem.

Gdy w tym przykładzie użyto TCP port 3389, następujące czynności może służyć do określenia błędy połączeń przychodzących i wychodzących przez dowolnego portu.

## <a name="detailed-troubleshooting-steps"></a>Szczegółowe kroki rozwiązywania problemów
Wykonaj poniższe czynności, aby Rozwiązywanie problemów z NSGs dla maszyny:

1. Rozpoczynanie sesji programu PowerShell Azure i logowania Azure. Jeśli nie znasz program przy użyciu programu PowerShell Azure, przeczytaj artykuł [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md) .

2. Wpisz następujące polecenie, aby zwrócić wszystkie reguły NSG zastosowane do NIC o nazwie *VM1 NIC1* w grupie zasobów *RG1*:

        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Jeśli nie znasz nazwy NIC, wpisz następujące polecenie, aby pobrać nazwy wszystkich nic w grupie zasobów: 
    
    >`Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`

    Przykładowe dane wyjściowe skuteczne reguły zwracane dla *VM1 NIC1* NIC brzmi:

        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
        
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]

    Zwróć uwagę następujące informacje w wyniku kwerendy:

    - Istnieją dwie sekcje **NetworkSecurityGroup** : jeden jest skojarzony z podsieci (*podsieć1*) i jedną jest skojarzony z NIC (*VM1 NIC1*). W tym przykładzie zastosowano NSG do wszystkich.
    - **Skojarzenia** pokazuje zasobów (podsieci lub NIC) danego NSG jest skojarzony. W przypadku zasobu NSG Anulowano przeniesiony skojarzenie bezpośrednio przed uruchomieniem tego polecenia, może być konieczne Poczekaj kilka sekund zmiany, aby odzwierciedlała w wynikach polecenia. 
    - Nazwy reguły, które są poprzedzone z *defaultSecurityRules*: gdy NSG jest tworzony, kilka domyślne zabezpieczeń reguły są tworzone w nim. Nie można usunąć domyślne reguły, ale może zostać zastąpiona z regułami wyższy priorytet.
     Przeczytaj artykuł [Omówienie NSG](virtual-networks-nsg.md#default-rules) , aby dowiedzieć się więcej o NSG domyślne reguły zabezpieczeń.
    - **ExpandedAddressPrefix** rozwinie prefiksów adresów dla znaczników domyślnych NSG. Znaczniki odpowiadają wielu prefiksów adresów. Rozszerzenia znaczniki mogą być przydatne podczas rozwiązywania problemów z łącznością maszyn wirtualnych z określonych adresów. Na przykład z VNET zaglądanie, znacznik VIRTUAL_NETWORK zostanie wyświetlona peered prefiksy VNet w poprzednich danych wyjściowych.

        >[AZURE.NOTE] Polecenie tylko pokazuje skuteczne reguły jeśli NSG jest skojarzony z podsieci, NIC lub oba. Maszyny może być kart z różnych NSGs zastosowane. Podczas rozwiązywania problemów, uruchom polecenie dla każdej karty sieciowej.
        
3. Aby ułatwić filtrowanie większej liczby reguł NSG, wprowadź następujące polecenia, aby dodatkowo Rozwiązywanie problemów z: 

        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView

    Filtr dla ruchu RDP (TCP port 3389), zostanie zastosowany do widoku siatki, jak pokazano na poniższej ilustracji:

    ![Lista reguł](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
    
4. Jak widać w widoku siatki, istnieją zarówno umożliwiają tworzenie i odmówić reguły RDP. Wynik z kroku 2 wskazuje, że reguły *DenyRDP* w NSG stosowane do danej podsieci. Dla reguły przychodzące NSGs stosowane do danej podsieci są przetwarzane jako pierwsze. Jeśli zostanie znaleziony pasujący element, nie jest wykonywane NSG zastosowane do interfejsu sieciowego. W tym przypadku reguły *DenyRDP* z podsieci blokuje RDP do maszyn wirtualnych (**VM1**).

    >[AZURE.NOTE] Maszyny może być dołączone do niego kart. Każdy może być połączony z innej podsieci. Ponieważ polecenia w poprzednich krokach są wykonywane NIC, należy upewnij się, że określono NIC występują braku łączności. Jeśli nie masz pewności, będzie można uruchamiać polecenia przed każdym NIC dołączone do maszyn wirtualnych.

5. Aby RDP do VM1 Zmień regułę *Odmówić RDP (3389)* aby *Umożliwić RDP(3389)* w **NSG podsieć1** NSG. Upewnij się, że TCP port 3389 jest otwarty, Otwieranie połączenia RDP maszyn wirtualnych lub za pomocą narzędzia PsPing. Więcej informacji o PsPing, czytając [PsPing pobieranie strony](https://technet.microsoft.com/sysinternals/psping.aspx)

    Możesz lub usunąć reguły z NSG, korzystając z informacji w wyniku kwerendy z następujące polecenie:

        Get-Help *-AzureRmNetworkSecurityRuleConfig
        

## <a name="considerations"></a>Zagadnienia dotyczące

Podczas rozwiązywania problemów z łącznością, rozważ następujące wskazówki:

- Domyślne reguły NSG zostaną zablokować dostęp ruchu przychodzącego z Internetu i tylko zezwolić VNet ruch przychodzący. Zasady powinny zostać jawnie dodane umożliwiają dostęp ruchu przychodzącego z Internetu, zależnie od potrzeb.
- Jeśli istnieją żadne reguły zabezpieczeń NSG powodujące łączność sieciowa maszyn wirtualnych kończy się niepowodzeniem, ten problem może być:
    - Oprogramowaniem zapory uruchomionym w systemie operacyjnym maszyn wirtualnych
    - Trasy skonfigurowane dla urządzeń wirtualna lub lokalnego. Ruch internetowy mogą być przekierowywane do lokalnego za pośrednictwem wymuszone tunelowania. Połączenie RDP-SSH z Internetu do swojego maszyn wirtualnych może nie działać z tych ustawień, w zależności od tego, jak sprzętu sieciowego lokalnego obsługuje ruch. Przeczytaj artykuł [Przekierowuje rozwiązywania problemów](virtual-network-routes-troubleshoot-powershell.md) , aby dowiedzieć się, jak diagnozowanie problemów rozsyłania, które mogą utrudnić przepływu ruchu i maszyn wirtualnych. 
- Jeśli masz peered VNets, domyślnie, znacznik VIRTUAL_NETWORK automatycznie zostaną rozszerzone mają zawierać prefiksy dla peered VNets. Tych prefiksów można wyświetlać na liście **ExpandedAddressPrefix** rozwiązywać problemy związane z VNet zaglądanie łączności. 
- Reguły skutecznych zabezpieczeń są wyświetlane tylko jeśli jest NSG skojarzone z maszyn NIC i podsieci. 
- Jeśli istnieją nie NSGs skojarzone z karty Sieciowej lub podsieci i masz publiczny adres IP przypisanej do swojego maszyn wirtualnych, wszystkie porty będzie otwarty ruch przychodzący i wychodzący dostęp. Jeśli maszyn wirtualnych ma publiczny adres IP, zdecydowanie zalecane jest stosowanie NSGs do karty Sieciowej lub podsieć.  
