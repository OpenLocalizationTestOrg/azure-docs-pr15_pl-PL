## <a name="how-to-create-a-vnet-using-a-network-config-file-from-powershell"></a>Jak utworzyć VNet za pomocą pliku konfiguracji sieci z programu PowerShell

Azure używa pliku xml do definiowania VNets wszystkich dostępnych do subskrypcji. Można pobrać ten plik i edytować go, aby zmodyfikować lub usunąć istniejących VNets i tworzyć nowych plików. W tym dokumencie możesz dowiedzieć się, jak pobrać ten plik, określane jako pliku konfiguracji (lub netcgf) sieciowego i edytowanie go, aby utworzyć nowy VNet. Sprawdź [Schemat konfiguracji Azure wirtualną sieć](https://msdn.microsoft.com/library/azure/jj157100.aspx) , aby dowiedzieć się więcej o pliku konfiguracji sieci.

Aby utworzyć VNet za pomocą pliku netcfg przy użyciu programu PowerShell, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz Azure programu PowerShell, zobacz, [jak instalowanie i konfigurowanie programu PowerShell Azure](../articles/powershell-install-configure.md) i postępuj zgodnie z instrukcjami maksymalnie end, aby zalogować się do Azure i wybierz subskrypcję.
2. Z poziomu konsoli programu PowerShell Azure należy użyć polecenia cmdlet **Get-AzureVnetConfig** o pobranie pliku konfiguracji sieci, uruchamiając poniższe polecenie. 

        Get-AzureVNetConfig -ExportToFile c:\NetworkConfig.xml

    Oczekiwany wynik:

        XMLConfiguration                                                                                                     
        ----------------                                                                                                     
        <?xml version="1.0" encoding="utf-8"?>...  

3. Otwórz plik został zapisany w kroku 2 powyżej za pomocą dowolnej aplikacji Edytor XML lub tekst, a następnie poszukaj **<VirtualNetworkSites>** elementu. Jeśli masz już utworzony sieci każda sieć będzie wyświetlana jako własnej **<VirtualNetworkSite>** elementu.
4. Aby utworzyć wirtualną sieć opisaną w tym scenariuszu, Dodaj następujący kod XML tuż poniżej **<VirtualNetworkSites>** elementu:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

9.  Zapisz plik konfiguracji sieci.
10. Z konsoli programu PowerShell Azure umożliwia przekazywanie pliku konfiguracji sieci, uruchamiając poniższe polecenie cmdlet **Set-AzureVnetConfig** . Zwróć uwagę, wynik w obszarze polecenia, powinien zostać wyświetlony **powiodło się** w obszarze **OperationStatus**. Jeśli to w przeciwnym wypadku, zaznacz plik xml pod kątem błędów.

        Set-AzureVNetConfig -ConfigurationPath c:\NetworkConfig.xml

    Oto przewidywaną powyżej polecenia:

        OperationDescription OperationId                          OperationStatus
        -------------------- -----------                          ---------------
        Set-AzureVNetConfig  49579cb9-3f49-07c3-ada2-7abd0e28c4e4 Succeeded 
    
11. Za pomocą konsoli programu PowerShell Azure umożliwia Sprawdź, czy nowej sieci została dodana, uruchamiając poniższe polecenie cmdlet **Get-AzureVnetSite** . 

        Get-AzureVNetSite -VNetName TestVNet

    Oto przewidywaną powyżej polecenia:

        AddressSpacePrefixes : {192.168.0.0/16}
        Location             : Central US
        AffinityGroup        : 
        DnsServers           : {}
        GatewayProfile       : 
        GatewaySites         : 
        Id                   : b953f47b-fad9-4075-8cfe-73ff9c98278f
        InUse                : False
        Label                : 
        Name                 : TestVNet
        State                : Created
        Subnets              : {FrontEnd, BackEnd}
        OperationDescription : Get-AzureVNetSite
        OperationId          : 3f35d533-1f38-09c0-b286-3d07cd0904d8
        OperationStatus      : Succeeded