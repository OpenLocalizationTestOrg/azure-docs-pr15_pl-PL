<properties
  pageTitle="Łączenie usługi w chmurze do kontrolera domeny niestandardowej | Microsoft Azure"
  description="Dowiedz się, jak nawiązać z własną domeną AD przy użyciu programu PowerShell i rozszerzenia domeny AD role usługi sieci web i pracownika"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Role usługi Cloud Azure nawiązywanie połączenia z niestandardowy kontrolera domeny AD obsługiwane platformy Azure

Firma Microsoft będzie zdefiniowania wirtualnej sieci (VNet) platformy Azure. Firma Microsoft spowoduje dodanie Active Directory kontrolera domeny (obsługiwanych na maszyn wirtualnych Azure) do VNet. Następnie firma Microsoft będzie Dodawanie istniejących ról usługi w chmurze do wstępnie zaprojektowanych VNet i połączyć je później do kontrolera domeny.

Początek, kilka rzeczy, które należy pamiętać:

1.  Samouczku programu PowerShell, więc upewnij się, że masz zainstalowany PowerShell Azure i gotowa do wysłania. Aby uzyskać pomoc dotyczącą konfigurowania Azure programu PowerShell, zobacz, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

2.  Wystąpienia programu kontrolera domeny AD i roli sieci Web i pracownik muszą być w VNet.

Wykonaj ten przewodnik krok po kroku, a Jeśli napotkasz jakiekolwiek problemy z nami specjalny komentarz poniżej. Ktoś będzie wrócić do Ciebie (tak, możemy czytanie komentarzy).

3. Sieci, do której odwołuje się do chmury usługi <mark>musi być</mark> **Klasyczny wirtualnej sieci**.

## <a name="create-a-virtual-network"></a>Tworzenie wirtualnych sieci

Możesz utworzyć wirtualną sieć platformy Azure za pomocą portal Azure klasyczny lub programu PowerShell. Ten samouczek użyjemy programu PowerShell. Aby utworzyć wirtualną sieć za pomocą portalu klasyczny Azure, zobacz [Tworzenie wirtualnych sieci](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej

Po zakończeniu konfigurowania wirtualną sieć potrzebne do utworzenia kontrolera domeny AD. Ten samouczek możemy zostaną określone kontrolera domeny AD na maszyn wirtualnych Azure.

W tym celu należy utworzyć maszyny wirtualnej przy użyciu programu PowerShell za pomocą poleceń poniżej:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Promowanie komputera wirtualnych kontrolera domeny
Aby skonfigurować maszyny wirtualnej jako kontrolera domeny AD, będzie konieczne logowanie się do maszyn wirtualnych i skonfiguruj go.

W celu zalogowania się do maszyn wirtualnych można pobrać pliku RDP przy użyciu programu PowerShell, użyj poleceń znajdujących się poniżej.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Po zalogowaniu się do maszyn wirtualnych, wykonując przewodnik krok po kroku dotyczące [konfiguracji klienta kontrolera domeny AD](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx)Konfiguracja komputera wirtualnych jako kontroler domeny AD.

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Dodawanie usługi w chmurze do wirtualnej sieci

Następnie należy dodać wdrożenia usługi cloud do VNet została właśnie utworzona. W tym celu należy zmodyfikować z cscfg usługi cloud dodając w odpowiednich sekcjach do swojego cscfg przy użyciu programu Visual Studio lub dowolnego edytora.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Następnie tworzyć projekty usługi cloud i Wdroż Azure. Aby uzyskać pomoc dotyczącą wdrażania pakietu chmury usługi Azure, zobacz [jak tworzenie i wdrażanie usługi w chmurze](cloud-services-how-to-create-deploy.md#deploy)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Łączenie role usługi sieci web i pracownika do domeny

Po wdrożeniu projektu chmury usługi Azure nawiązać połączenie z wystąpienia roli niestandardowej domeny AD przy użyciu rozszerzenia AD domeny. Aby dodać rozszerzenie hasło do istniejącego wdrożenia usługi cloud i dołączanie do domeny niestandardowej, wykonaj następujące polecenia w programie PowerShell:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

I to wszystko.

Cloud services teraz powinny zostać połączone z kontrolera domeny niestandardowej. Jeśli chcesz dowiedzieć się więcej o różne opcje dotyczące sposobu konfigurowania domeny AD rozszerzenia, użyj pomocy programu PowerShell, tak jak pokazano poniżej.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
