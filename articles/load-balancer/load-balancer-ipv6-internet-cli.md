<properties
    pageTitle="Tworzenie internetowej przeciwległych równoważenia obciążenia, z protokołu IPv6 w Azure Menedżera zasobów za pomocą interfejsu wiersza polecenia Azure | Microsoft Azure"
    description="Dowiedz się, jak utworzyć internetową przeciwległych równoważenia obciążenia, z protokołu IPv6 w Azure Menedżera zasobów za pomocą interfejsu wiersza polecenia Azure"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="Protokół IPv6, równoważenia obciążenia azure, podwójne stos, publiczny adres ip, natywny protokół ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a>Tworzenie internetowej przeciwległych równoważenia obciążenia, z protokołu IPv6 w Azure Menedżera zasobów za pomocą interfejsu wiersza polecenia Azure

> [AZURE.SELECTOR]
- [Programu PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Polecenie Azure](./load-balancer-ipv6-internet-cli.md)
- [Szablon](./load-balancer-ipv6-internet-template.md)

Usługi równoważenia obciążenia Azure jest równoważenia obciążenia warstwy-4 (port TCP, UDP). Usługi równoważenia obciążenia zapewnia wysoką dostępność przekazując ruch przychodzący między wystąpień prawidłowy usługi w chmurze usług lub maszyn wirtualnych w zestawie równoważenia obciążenia. Azure równoważenia obciążenia można także przedstawić tych usług na wiele portów i/lub wiele adresów IP.

## <a name="example-deployment-scenario"></a>Przykładowy scenariusz wdrażania

Na poniższym diagramie przedstawiono rozwiązania do równoważenia obciążenia wdrażania przy użyciu szablonu przykład opisane w tym artykule.

![Scenariusz równoważenia obciążenia](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

W tym scenariuszu utworzysz Azure następujące zasoby:

- dwa wirtualnych maszyn
- wirtualna sieciowej dla każdego maszyn wirtualnych z adresami IPv4 i IPv6 przypisane
- usługi równoważenia obciążenia z Internetu przy użyciu protokołu IPv4 i IPv6 publiczny adres IP
- Dostępność grupy zgodnej zawiera dwa maszyny wirtualne
- dwa ładowanie równoważenia reguły do zamapować publicznej służącą do prywatnych punktów końcowych

## <a name="deploying-the-solution-using-the-azure-cli"></a>Wdrożenie rozwiązania przy użyciu interfejsu wiersza polecenia Azure

Poniższe kroki pokazują sposób tworzenia internetowej przeciwległych równoważenia obciążenia Menedżera zasobów Azure za pomocą interfejsu wiersza polecenia. Przy użyciu Menedżera zasobów Azure każdego zasobu jest utworzony i skonfigurowany pojedynczo, a następnie umieść ze sobą, aby utworzyć zasób.

Aby wdrożyć usługi równoważenia obciążenia, możesz utworzyć i skonfigurować następujących obiektów:

- Zewnętrzną Konfiguracja IP — zawiera publicznych adresów IP dla ruchu sieci przychodzącego.
- Pula adresów wewnętrznej - zawiera interfejsów sieciowych (NIC) dla maszyn wirtualnych otrzymywać ruch sieciowy z usługi równoważenia obciążenia.
- Zasady równoważenia obciążenia - zawiera reguły mapowania portem publicznej usługi równoważenia obciążenia do portu w puli adresów wewnętrznej.
- Translatora adresów Sieciowych reguły przychodzące — zawiera reguły mapowanie portem publicznej usługi równoważenia obciążenia na port dla określonej maszyny wirtualnej w puli adresów wewnętrznej.
- Sondy — zawiera sondy kondycji umożliwia sprawdzanie dostępności wystąpień maszyn wirtualnych w puli adresów wewnętrznej.

Aby uzyskać więcej informacji zobacz [Azure Menedżera zasobów pomocy technicznej dotyczącej usługi równoważenia obciążenia](load-balancer-arm.md).

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a>Konfigurowanie środowiska usługi interfejsu wiersza polecenia za pomocą Menedżera zasobów Azure

Na przykład możemy działają narzędzia interfejsu wiersza polecenia w oknie polecenia programu PowerShell. Nie użyto polecenia cmdlet programu PowerShell Azure, ale firma Microsoft korzysta z funkcji skryptów programu PowerShell w celu zwiększenia czytelności i ponownego użycia.

1. Jeśli po raz pierwszy używasz polecenie Azure, zobacz [Instalowanie i konfigurowanie polecenie Azure](../../articles/xplat-cli-install.md) i postępuj zgodnie z instrukcjami do miejsca, w którym wybierz swoje konto Azure i subskrypcji.

2. Uruchom polecenie **Konfiguracja azure tryb** , aby przełączyć się do trybu Menedżera zasobów.

        azure config mode arm

    Oczekiwany wynik:

        info:    New mode is arm

3. Zaloguj się do Azure i uzyskiwanie listy subskrypcji.

        azure login

    Wprowadź swoje poświadczenia Azure po wyświetleniu monitu.

        azure account list

    Wybierz subskrypcję, którą chcesz użyć. Zanotuj identyfikator subskrypcji w następnym kroku.

4. Ustawianie zmiennych programu PowerShell do użytku z poleceń interfejsu wiersza polecenia.

        ```
        $subscriptionid = "########-####-####-####-############"  # enter subscription id
        $location = "southcentralus"
        $rgName = "pscontosorg1southctrlus09152016"
        $vnetName = "contosoIPv4Vnet"
        $vnetPrefix = "10.0.0.0/16"
        $subnet1Name = "clicontosoIPv4Subnet1"
        $subnet1Prefix = "10.0.0.0/24"
        $subnet2Name = "clicontosoIPv4Subnet2"
        $subnet2Prefix = "10.0.1.0/24"
        $dnsLabel = "contoso09152016"
        $lbName = "myIPv4IPv6Lb"
        ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Tworzenie grupy zasobów, równoważenia obciążenia wirtualnej sieci i podsieci

1. Tworzenie grupy zasobów

        azure group create $rgName $location

2. Tworzenie równoważenia obciążenia

        $lb = azure network lb create --resource-group $rgname --location $location --name $lbName

3. Tworzenie wirtualnych sieci (VNet).

        $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix

    Tworzenie dwóch podsieci, w tym VNet.

        $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
        $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a>Tworzenie publicznych adresów IP dla puli zewnętrzną

1. Ustawianie zmiennych programu PowerShell

        $publicIpv4Name = "myIPv4Vip"
        $publicIpv6Name = "myIPv6Vip"

2. Tworzenie puli IP zewnętrzną publiczny adres IP.

        $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
        $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel

    >[AZURE.IMPORTANT]Usługi równoważenia obciążenia jako nazwy FQDN jest używana etykieta domeny publiczny adres IP. To zmiana z wdrożenia klasyczny korzysta z usług w chmurze nazwę równoważenia obciążenia FQDN.
    >W tym przykładzie nazwy FQDN jest *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Tworzenie puli frontonu i wewnętrznej

W tym przykładzie tworzy puli IP zewnętrzną otrzymuje przychodzący ruch sieciowy na równoważenia obciążenia i puli IP wewnętrznej, gdzie puli zewnętrzną wysyła ruch sieciowy równoważenia obciążenia.

1. Ustawianie zmiennych programu PowerShell

        $frontendV4Name = "FrontendVipIPv4"
        $frontendV6Name = "FrontendVipIPv6"
        $backendAddressPoolV4Name = "BackendPoolIPv4"
        $backendAddressPoolV6Name = "BackendPoolIPv6"

2. Tworzenie puli adresów IP zewnętrzną kojarzenie publiczny adres IP utworzony w poprzednim kroku i równoważenia obciążenia.

        $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
        $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
        $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
        $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName

## <a name="create-the-probe-nat-rules-and-lb-rules"></a>Tworzenie sondy, reguły translatora adresów Sieciowych i kg reguł

W tym przykładzie tworzy następujące elementy:

- reguły sondy w celu wyszukania łączności z programem TCP port 80
- Aby przetłumaczyć cały ruch przychodzący na porcie 3389 do portu 3389 RDP<sup>1</sup> reguły translatora adresów Sieciowych
- Aby przetłumaczyć cały ruch przychodzący na porcie 3391 do portu 3389 RDP<sup>1</sup> reguły translatora adresów Sieciowych
- Reguła równoważenia obciążenia do saldo cały ruch przychodzący na porcie 80 do portu 80 na adresy w puli wewnętrznej.

<sup>1</sup> translatora adresów Sieciowych reguły są skojarzone z wystąpieniem określonego maszyn wirtualnych za usługi równoważenia obciążenia. Ruch sieciowy nadchodzi na porcie 3389 są wysyłane do określonych maszyn wirtualnych i portu skojarzone z regułą translatora adresów Sieciowych. Dla reguły translatora adresów Sieciowych, musisz określić protocol (UDP lub TCP). Nie można przypisać obu protokołów do tego samego portu.

1. Ustawianie zmiennych programu PowerShell

        $probeV4V6Name = "ProbeForIPv4AndIPv6"
        $natRule1V4Name = "NatRule-For-Rdp-VM1"
        $natRule2V4Name = "NatRule-For-Rdp-VM2"
        $lbRule1V4Name = "LBRuleForIPv4-Port80"
        $lbRule1V6Name = "LBRuleForIPv6-Port80"

2. Tworzenie sondy

    Poniższy przykład tworzy sondy TCP sprawdzające połączenia wewnętrznej port TCP 80 co 15 sekund. Jego oznaczy zasobów wewnętrznej niedostępne po dwóch następujących po sobie błędów.

        $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName

3. Tworzenie reguły przychodzące translatora adresów Sieciowych, które zezwalają na połączenia RDP do zasobów wewnętrznej

        $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
        $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName

4. Tworzenie reguł, które wysyłać ruch do różnych portów wewnętrznej zależności, na które fronton odebrano żądania usługi równoważenia obciążenia

        $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
        $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName

5. Sprawdź ustawienia

        azure network lb show --resource-group $rgName --name $lbName

    Oczekiwany wynik:

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show


## <a name="create-nics"></a>Tworzenie nic

Tworzenie kart sieciowych i kojarzenie ich reguły, zasady równoważenia obciążenia i sondy translatora adresów Sieciowych.

1. Ustawianie zmiennych programu PowerShell

        $nic1Name = "myIPv4IPv6Nic1"
        $nic2Name = "myIPv4IPv6Nic2"
        $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
        $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
        $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
        $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
        $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
        $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"

2. Tworzenie NIC dla każdej wewnętrznej i Dodawanie konfiguracji protokołu IPv6.

        $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

        $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
        $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a>Tworzenie zasobów maszyn wirtualnych wewnętrznej i dołączanie każdego NIC

Aby utworzyć maszyny wirtualne, musisz mieć konto miejsca do magazynowania. W przypadku równoważenia obciążenia maszyny wirtualne muszą być członkami zestawu dostępności. Aby uzyskać więcej informacji na temat tworzenia maszyny wirtualne Zobacz [Tworzenie maszyn wirtualnych Azure przy użyciu programu PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md)

1. Ustawianie zmiennych programu PowerShell

        $storageAccountName = "ps08092016v6sa0"
        $availabilitySetName = "myIPv4IPv6AvailabilitySet"
        $vm1Name = "myIPv4IPv6VM1"
        $vm2Name = "myIPv4IPv6VM2"
        $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
        $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
        $disk1Name = "WindowsVMosDisk1"
        $disk2Name = "WindowsVMosDisk2"
        $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
        $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
        $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
        $vmUserName = "vmUser"
        $mySecurePassword = "PlainTextPassword*1"

    >[AZURE.WARNING] W tym przykładzie użyto nazwy użytkownika i hasła dla maszyny wirtualne w postaci zwykłego tekstu. Odpowiednie należy zachować ostrożność przy użyciu poświadczeń w Wyczyść. Aby zabezpieczyć metody obsługi poświadczeń w programie PowerShell Zobacz polecenia cmdlet [Get-poświadczeń](https://technet.microsoft.com/library/hh849815.aspx) .

2. Tworzenie zestawu konta i dostępność miejsca do magazynowania

    Po utworzeniu maszyny wirtualne można użyć istniejącego konta miejsca do magazynowania. Następujące polecenie tworzy nowe konto miejsca do magazynowania.

        $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"

    Następnie utwórz zestaw dostępności.

        $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location

3. Tworzenie maszyn wirtualnych ze skojarzonymi nic

        $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

        $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn  --storage-account-name $storageAccountName --disable-bginfo-extension

## <a name="next-steps"></a>Następne kroki

[Przed rozpoczęciem konfigurowania równoważenia obciążenia wewnętrznych](load-balancer-get-started-ilb-arm-cli.md)

[Konfigurowanie trybu rozkładu równoważenia obciążenia](load-balancer-distribution-mode.md)

[Konfigurowanie ustawienia przekroczenia limitu czasu bezczynności protokołu TCP dla usługi równoważenia obciążenia](load-balancer-tcp-idle-timeout.md)
