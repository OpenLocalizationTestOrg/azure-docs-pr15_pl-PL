<properties 
   pageTitle="Jak przenieść wystąpienie maszyn wirtualnych lub rolę do innej podsieci"
   description="Dowiedz się, jak przenieść maszyny wirtualne i wystąpienia roli innej podsieci"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Jak przenieść wystąpienie maszyn wirtualnych lub rolę do innej podsieci

PowerShell umożliwia przechodzenie do maszyny wirtualne z jednej podsieci do drugiego w tej samej sieci wirtualnych (VNet). Rola wystąpienia mogą być przenoszone przez CSCFG do edycji, a nie przy użyciu programu PowerShell.

>[AZURE.NOTE] Ten artykuł zawiera informacje, które są względem tylko Azure wdrożeń klasyczny.

Dlaczego warto przenosić maszyny wirtualne do innej podsieci? Migracja podsieci jest przydatny, gdy starszy podsieci jest za mały i nie można rozwinąć ze względu na istniejące uruchomionego maszyny wirtualne w tej podsieci. W takim przypadku możesz utworzyć nową, większe podsieć i Migrowanie maszyn wirtualnych do nowej podsieci, a następnie po zakończeniu migracji możesz usunąć stare podsieci puste.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Jak przenieść maszyny do innej podsieci

Aby przenieść maszyny, uruchom polecenie cmdlet programu PowerShell zestawu AzureSubnet, w tym przykładzie poniżej jako szablon. W poniższym przykładzie możemy przechodzisz TestVM z jego bieżącą podsieci podsieci 2. Pamiętaj edytować w przykładzie, aby odzwierciedlała środowiska. Należy zauważyć, że po uruchomieniu polecenia cmdlet AzureVM aktualizacji w ramach procedury go nastąpi ponowne uruchomienie usługi maszyn wirtualnych w ramach procesu aktualizacji.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

Jeśli określono statyczny adres IP prywatne wewnętrznych dla swojego maszyn wirtualnych, musisz wyczyścić to ustawienie, przed przejściem maszyn wirtualnych do nowej podsieci. W takim przypadku użyj następujących opcji:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Aby przenieść wystąpienie roli do innej podsieci

Aby przenieść wystąpienie roli, edytować plik CSCFG. W poniższym przykładzie możemy są przenoszone "Role0" w wirtualnej sieci *VNETName* z jego bieżącą podsieci do *podsieci 2*. Ponieważ został już wdrożony w wystąpieniu roli, po prostu zmienisz nazwę podsieci = podsieci 2. Pamiętaj edytować w przykładzie, aby odzwierciedlała środowiska.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
