<properties
   pageTitle="Jak utworzyć NSGs w trybie klasycznym za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure"
   description="Dowiedz się, jak tworzyć i wdrażać NSGs w trybie klasycznym za pomocą interfejsu wiersza polecenia Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Jak utworzyć NSGs (klasyczny) w polecenie Azure

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]W tym artykule opisano, jak model klasyczny wdrożenia. Możesz również [utworzyć NSGs w modelu wdrożenia Menedżera zasobów](virtual-networks-create-nsg-arm-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Poniższe przykładowe polecenie Azure polecenia przewiduje środowisku prosty już utworzone w zależności od tego scenariusza powyżej. Jeśli chcesz uruchomić polecenia wyświetlaną w tym dokumencie, należy najpierw utworzyć środowisku testowym, [tworząc VNet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Jak utworzyć NSG podsieci fronton
Aby utworzyć NSG o nazwie nazwany **NSG FrontEnd** według tego scenariusza powyżej, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.

2. Uruchamianie **`azure config mode`** polecenie, aby przejść do trybu klasycznego, tak jak pokazano poniżej.

        azure config mode asm

    Oczekiwany wynik:

        info:    New mode is asm

3. Uruchamianie **`azure network nsg create`** polecenie, aby utworzyć NSG.

        azure network nsg create -l uswest -n NSG-FrontEnd

    Oczekiwany wynik:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parametry:

    - **-l (lub — lokalizacja)**. Azure region, w którym zostanie utworzone nowe NSG. W naszym scenariuszu *westus*.
    - **-n (lub — nazwa)**. Nazwa nowego NSG. W naszym scenariuszu *NSG FrontEnd*.

4. Uruchamianie **`azure network nsg rule create`** polecenie, aby utworzyć regułę, która umożliwia dostęp do portu 3389 (RDP) z Internetu.

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Oczekiwany wynik:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Parametry:

    - **(lub nazwa — nsg)**. Nazwa NSG, w której zostanie utworzona reguła. W naszym scenariuszu *NSG FrontEnd*.
    - **-n (lub — nazwa)**. Nazwa nowej reguły. W naszym scenariuszu *rdp reguły*.
    - **-c (lub — akcji)**. Poziom dostępu dla reguły (Odmów i Zezwalaj).
    - **-p (lub — protocol)**. Protocol (port Tcp, Udp, lub *) reguły.
    - **-r (lub — typ)**. Kierunek połączenia (przychodzącej lub wychodzącej).
    - **-y (lub — priorytet)**. Priorytet reguły.
    - **-f (lub — prefiksu adresu źródłowego z)**. Prefiks adresu źródła w CIDR lub przy użyciu znaczników domyślnych.
    - **+ o (lub — zakres w przypadku port źródłowy)**. Port źródłowy lub zakres portu.
    - **-e (lub — docelowy adres prefiks)**. Prefiks adresu docelowego w CIDR lub przy użyciu znaczników domyślnych.
    - **-u (lub — zakres w przypadku port docelowy)**. Port docelowy lub zakresie portów.

5. Uruchamianie **`azure network nsg rule create`** polecenie, aby utworzyć regułę, która umożliwia dostęp do portu 80 (HTTP) z Internetu.

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Oczekiwany putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Uruchamianie **`azure network nsg subnet add`** polecenie, aby połączyć NSG podsieci frontonu.

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd

    Oczekiwany wynik:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Jak utworzyć NSG podsieci wewnętrznej
Aby utworzyć NSG o nazwie nazwanych *Wewnętrznej bazy danych NSG* według tego scenariusza powyżej, wykonaj poniższe czynności.

3. Uruchamianie **`azure network nsg create`** polecenie, aby utworzyć NSG.

        azure network nsg create -l uswest -n NSG-BackEnd

    Oczekiwany wynik:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parametry:

    - **-l (lub — lokalizacja)**. Azure region, w którym zostanie utworzone nowe NSG. W naszym scenariuszu *westus*.
    - **-n (lub — nazwa)**. Nazwa nowego NSG. W naszym scenariuszu *NSG FrontEnd*.

4. Uruchamianie **`azure network nsg rule create`** polecenie, aby utworzyć regułę, która umożliwia dostęp do portu 1433 (SQL) z podsieci frontonu.

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Oczekiwany wynik:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. Uruchamianie **`azure network nsg rule create`** polecenie, aby utworzyć regułę, która powoduje zablokowanie dostęp do Internetu.

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    Oczekiwany putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Uruchamianie **`azure network nsg subnet add`** polecenie, aby połączyć NSG podsieci wewnętrznej.

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd

    Oczekiwany wynik:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK
