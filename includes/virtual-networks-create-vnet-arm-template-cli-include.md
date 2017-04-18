## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Wdrażanie szablonu ARM za pomocą interfejsu wiersza polecenia Azure

Aby wdrożyć szablon ARM pobranego przy użyciu interfejsu wiersza polecenia Azure, wykonaj poniższe czynności.

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../articles/xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.
2. Uruchamianie **`azure config mode`** polecenie, aby przełączyć się do trybu Menedżera zasobów, jak pokazano poniżej.

        azure config mode arm

    Oto przewidywaną powyżej polecenia:

        info:    New mode is arm

3. W razie potrzeby uruchom **`azure group create`** Tworzenie nowej grupy zasobów, jak pokazano poniżej. Zwróć uwagę, dane wyjściowe polecenia. Lista wyświetlana po wynik wyjaśniono parametry używane. Aby uzyskać więcej informacji dotyczących grup zasobów odwiedź stronę [Azure Omówienie Menedżera zasobów](../articles/resource-group-overview.md).

        azure group create -n TestRG -l centralus

    Oto przewidywaną powyżej polecenia:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (lub — nazwa)**. Nazwa nowej grupy zasobów. W naszym scenariuszu *TestRG*.
    - **-l (lub — lokalizacja)**. Region Azure miejsce, w którym można utworzyć nową grupę zasobów. W naszym scenariuszu *centralus*.

4. Uruchamianie **`azure group deployment create`** polecenia cmdlet wdrażania nowych VNet przy użyciu szablonu i parametr pliki, możesz pobrać i modyfikować powyżej. Lista wyświetlana po wynik wyjaśniono parametry używane.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Oto oczekiwany wynik polecenia powyżej:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g (lub — grupa zasobów)**. Nazwa grupy zasobów nowych VNet zostanie utworzony w.
    - **-f (lub — plik szablonu)**. Ścieżka do pliku szablonu ARM.
    - **-e (lub — pliku parametrów)**. Ścieżka do pliku parametrów ARM.

5. Uruchamianie **`azure network vnet show`** polecenie, aby wyświetlić właściwości nowego vnet, tak jak pokazano poniżej.

        azure network vnet show -g TestRG -n TestVNet

    Oto przewidywaną powyżej polecenia:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
