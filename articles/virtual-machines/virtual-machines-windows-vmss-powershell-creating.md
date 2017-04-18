<properties
    pageTitle="Tworzenie zestawów skali maszyn wirtualnych przy użyciu poleceń cmdlet środowiska PowerShell | Microsoft Azure"
    description="Wprowadzenie do tworzenia i zarządzania nimi z pierwszych zestawów skali maszyn wirtualnych Azure, na przy użyciu poleceń cmdlet środowiska PowerShell Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="danielsollondon"/>

# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a>Tworzenie zestawów skali maszyn wirtualnych przy użyciu poleceń cmdlet programu PowerShell

Przykład sposobu tworzenia Set(VMSS) skali maszyn wirtualnych, tworzy VMSS 3 węzłów ze wszystkimi skojarzone sieci i miejsca do magazynowania.

## <a name="first-steps"></a>Pierwsze kroki
Upewnij się, masz najnowsze modułu programu PowerShell usługi Azure, zainstalowany, to będzie zawierać apletów poleceń programu PowerShell, aby zachować i tworzenie VMSS.
Przejdź do polecenia narzędzia wiersza polecenia [tutaj](http://aka.ms/webpi-azps) najnowsze dostępne Azure modułów.

Aby znaleźć VMSS związane z apletów poleceń, należy użyć ciągu wyszukiwania \*VMSS\*.

## <a name="creating-a-vmss"></a>Tworzenie VMSS

##### <a name="create-resource-group"></a>Tworzenie grupy zasobów

```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

##### <a name="create-storage-account"></a>Tworzenie konta miejsca do magazynowania

Ustaw typ konta magazynu / Nazwa.

```
$stoname = 'sto' + $rgname;
$stotype = 'Standard_LRS';
 New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname -Location $loc -SkuName $stotype -Kind "Storage";

$stoaccount = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname;
```

#### <a name="create-networking-vnet--subnet"></a>Tworzenie sieci (VNET-podsieci)

##### <a name="subnet-specification"></a>Specyfikacja podsieci

```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

##### <a name="vnet-specification"></a>Specyfikacja VNET

```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

#In this case below we assume the new subnet is the only one, note difference if you have one already or have adjusted this code to more than one subnet.
$subnetId = $vnet.Subnets[0].Id;
```

##### <a name="create-public-ip-resource-to-allow-external-access"></a>Tworzenie zasobu publiczny adres IP, aby umożliwić dostęp zewnętrzny

To będzie związana z modułem równoważenia obciążenia.

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

##### <a name="create-and-configure-load-balancer"></a>Tworzenie i konfigurowanie usługi równoważenia obciążenia

```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

#Bind Public IP to Load Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

##### <a name="configure-load-balancer"></a>Konfigurowanie usługi równoważenia obciążenia
Tworzenie konfiguracji puli adres wewnętrznej bazy danych, to będzie udostępniany przez sieciowe maszyny wirtualne w VMSS.

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

Ustawianie portu sondy strategii obciążenia, zmień odpowiednie ustawienia aplikacji.

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

Tworzenie reguł translatora adresów Sieciowych do bezpośredniej łączności routingu (równoważenia obciążenia) do maszyny wirtualne w VMSS za pośrednictwem usługi równoważenia obciążenia, notatkę, którą to zaprezentowano przy użyciu RDP, to tylko dla pokaz i wewnętrznych metody VNET powinny być stosowane do RDP'ing na tych serwerach.

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

Utwórz Załaduj strategie regułę, w tym przykładzie, że pokazuje Załaduj równoważenia port 80 żądania, używając ustawień z powyższych czynności spowoduje.

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

Tworzenie usługi równoważenia obciążenia z konfiguracją.

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

Sprawdź ustawienia kg, sprawdź podawać portu równoważenia obciążenia, notatkę, nie zostaną wyświetlone reguły przychodzące translatora adresów Sieciowych do momentu są tworzone w VMSS maszyn.

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-vmss"></a>Konfigurowanie i Utwórz VMSS

Notatki, w tym przykładzie infrastruktury przedstawiono sposób konfigurowania rozpowszechnianie i skalowanie przez VMSS ruchu w sieci web, ale obrazy maszyny wirtualne określony w tym miejscu ma wszystkie zainstalowane usługi sieci web.

```
#specify VMSS Name
$vmssName = 'vmss' + $rgname;

##specify VMSS specific details
$adminUsername = 'azadmin';
$adminPassword = "Password1234!";

$PublisherName = 'MicrosoftWindowsServer'
$Offer         = 'WindowsServer'
$Sku          = '2012-R2-Datacenter'
$Version       = 'latest'
$vmNamePrefix = 'winvmss'

$vhdContainer = "https://" + $stoname + ".blob.core.windows.net/" + $vmssName;

###add an extension
$extname = 'BGInfo';
$publisher = 'Microsoft.Compute';
$exttype = 'BGInfo';
$extver = '2.1';
```

Powiązać NIC równoważenia obciążenia i podsieci

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

Tworzenie konfiguracji VMSS

```
#Specify number of nodes
$numberofnodes = 3

$vmss = New-AzureRmVmssConfig -Location $loc -SkuCapacity $numberofnodes -SkuName 'Standard_A2' -UpgradePolicyMode 'automatic' `
  	| Add-AzureRmVmssNetworkInterfaceConfiguration -Name $subnetName -Primary $true -IPConfiguration $ipCfg `
  	| Set-AzureRmVmssOSProfile -ComputerNamePrefix $vmNamePrefix -AdminUsername $adminUsername -AdminPassword $adminPassword `
  	| Set-AzureRmVmssStorageProfile -Name 'test' -OsDiskCreateOption 'FromImage' -OsDiskCaching 'None' `
    -ImageReferenceOffer $Offer -ImageReferenceSku $Sku -ImageReferenceVersion $Version `
    -ImageReferencePublisher $PublisherName -VhdContainer $vhdContainer `
  	| Add-AzureRmVmssExtension -Name $extname -Publisher $publisher -Type $exttype -TypeHandlerVersion $extver -AutoUpgradeMinorVersion $true
```

Tworzenie konfiguracji VMSS

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

Po utworzeniu VMSS. Można sprawdzić, nawiązywanie połączenia z poszczególnych maszyn wirtualnych za pomocą RDP w tym przykładzie:

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
