## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Jak utworzyć klasyczny VNet, za pomocą interfejsu wiersza polecenia Azure

Polecenie Azure umożliwia zarządzanie zasobami Azure z wiersza polecenia z dowolnym komputerze z systemem Windows, Linux lub OSX. Aby utworzyć VNet za pomocą interfejsu wiersza polecenia Azure, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../articles/xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.
2. Uruchom polecenie **Utwórz vnet azure sieci** , aby utworzyć VNet i podsieci, tak jak pokazano poniżej. Lista wyświetlana po wynik wyjaśniono parametry używane.

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    Oczekiwany wynik:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **— vnet**. Nazwa VNet ma zostać utworzony. W naszym scenariuszu *TestVNet*
    - **-e (lub — przestrzeni adresów)**. Przestrzeń adresów VNet. W naszym scenariuszu *192.168.0.0*
    - **-i (lub - cidr)**. Maska sieci w formacie CIDR. W naszym scenariuszu *16*.
    - **- n (lub--nazwa podsieci**). Nazwa pierwszą podsieć. W naszym scenariuszu dla *serwera sieci Web*.
    - **-p (lub — podsieci start ip)**. Początkowy adres IP dla podsieci lub przestrzeni adresów podsieci. W naszym scenariuszu *192.168.1.0*.
    - **-r (lub — cidr podsieci)**. Maska sieci w formacie CIDR podsieci. W naszym scenariuszu *24*.
    - **-l (lub — lokalizacja)**. Azure region, w której zostanie utworzony VNet. W naszym scenariuszu *Centralnej USA*.

3. Uruchom polecenie **Utwórz azure vnet podsieci** , aby utworzyć podsieć, tak jak pokazano poniżej. Lista wyświetlana po wynik wyjaśniono parametry używane.

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    Oto przewidywaną powyżej polecenia:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (lub nazwa — vnet**. Nazwa VNet, w której zostanie utworzony podsieci. W naszym scenariuszu *TestVNet*.
    - **-n (lub — nazwa)**. Nazwa nowej podsieci. W naszym scenariuszu *wewnętrznej bazy danych*.
    - **(lub — prefiksu adresu)**. Blok CIDR podsieci. Cztery naszych scenariusza, *192.168.2.0/24*.

4. Uruchom polecenie **Pokaż vnet azure sieci** , aby wyświetlić właściwości nowego vnet, tak jak pokazano poniżej.

            azure network vnet show

    Oto przewidywaną powyżej polecenia:

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
