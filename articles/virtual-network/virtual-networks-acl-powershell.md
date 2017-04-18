<properties
   pageTitle="Jak zarządzać listy kontroli dostępu (ACL) dla punktów końcowych przy użyciu programu PowerShell"
   description="Dowiedz się, jak zarządzać ACL przy użyciu programu PowerShell"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-manage-access-control-lists-acls-for-endpoints-by-using-powershell"></a>Jak zarządzać listy kontroli dostępu (ACL) dla punktów końcowych przy użyciu programu PowerShell

Można tworzyć i zarządzać sieci listy kontroli dostępu (ACL) dla punktów końcowych przy użyciu programu PowerShell Azure lub w portalu zarządzania. W tym temacie znajdziesz procedury ACL typowych zadań, które można wykonać przy użyciu programu PowerShell. Aby uzyskać listę poleceń cmdlet środowiska PowerShell Azure zobacz [Polecenia cmdlet zarządzania Azure](http://go.microsoft.com/fwlink/?LinkId=317721). Aby uzyskać więcej informacji na temat list ACL, zobacz [Co to jest sieć listy kontroli dostępu (ACL)?](virtual-networks-acl.md). Jeśli chcesz zarządzać do list ACL za pomocą portalu zarządzania, zobacz [jak się punkty końcowe maszyn wirtualnych](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Zarządzanie ACL sieci przy użyciu programu PowerShell Azure

Polecenia cmdlet programu PowerShell Azure umożliwia tworzenie, usuwanie i konfigurowanie (zestaw) sieci listy kontroli dostępu (ACL). Kilka przykładów niektóre metody można skonfigurować przy użyciu programu PowerShell ACL zostały włączone.

Aby pobrać listę wszystkich poleceń cmdlet programu PowerShell ACL, możesz użyć jedną z następujących czynności:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Tworzenie ACL sieci przy użyciu reguł, które umożliwiają dostęp z podsieci zdalnej

W poniższym przykładzie pokazano sposób, aby utworzyć nową listę ACL, który zawiera reguły. Ten list ACL następnie zostanie zastosowany do punktu końcowego maszyn wirtualnych. Reguły ACL w poniższym przykładzie umożliwi dostęp z podsieci zdalnej. Aby utworzyć nową listę ACL sieci z regułami o zezwolenie dla podsieci zdalnej, otwórz Azure środowiska PowerShell ISE. Skopiuj i wklej skrypt poniżej, konfigurowanie skrypt za pomocą własne wartości, a następnie uruchom skrypt.

1. Utwórz nowy obiekt ACL sieci.

        $acl1 = New-AzureAclConfig

1. Ustaw regułę, która umożliwia dostęp z podsieci zdalnej. W poniższym przykładzie można ustawić reguły *100* (który mają pierwszeństwo przed reguły 200 lub nowszy) umożliwiają dostęp *10.0.0.0/8* podsieci zdalnej do punktu końcowego maszyn wirtualnych. Zamień wartości wymagań konfiguracji. Ma zostać zamieniona nazwa "SharePoint ACL konfiguracji" z przyjazną nazwę, którą chcesz nawiązać połączenie tej reguły.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"

1. Dodatkowe reguły Powtórz polecenia cmdlet, zamieniając wartości wymagań konfiguracji. Pamiętaj zmienić regułę numer zamówienia, aby odzwierciedlała kolejności, w jakiej mają zasady, które mają być stosowane. Mniejszą wartość reguła ma pierwszeństwo przed większej liczby.

        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"

1. Następnie można utworzyć nowy punkt końcowy (Dodaj) lub ustawić listy ACL dla istniejący punkt końcowy (zestaw). W tym przykładzie było dodać nowy punkt końcowy maszyn wirtualnych o nazwie "web" i aktualizowanie punkt końcowy maszyn wirtualnych z ustawieniami ACL.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM

1. Następnie łączenie poleceń cmdlet i uruchom skrypt. W tym przykładzie Scalonej poleceń cmdlet będzie miała następującą postać:

        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Usunąć regułę ACL sieci, który umożliwia dostęp z podsieci zdalnej

W poniższym przykładzie pokazano sposób, aby usunąć regułę ACL sieci.  Aby usunąć regułę ACL sieci z regułami o zezwolenie dla podsieci zdalnej, otwórz Azure środowiska PowerShell ISE. Skopiuj i wklej skrypt poniżej, konfigurowanie skrypt za pomocą własne wartości, a następnie uruchom skrypt.

1. Pierwszym krokiem jest pobieranie obiektu ACL sieci do punktu końcowego maszyn wirtualnych. Zostanie następnie usunąć regułę ACL. W tym przypadku zostaje usunięta go przez identyfikator reguły. Identyfikator reguły 0 zostanie usunięty tylko z tej listy. Nie powoduje usunięcia obiektu ACL od punktu końcowego maszyn wirtualnych.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1

1. Następnie należy obiekt ACL sieci zastosowany do punktu końcowego maszyn wirtualnych i aktualizowanie maszyny wirtualnej.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Usuwanie punktu końcowego maszyn wirtualnych ACL sieci

W niektórych scenariuszach można usunąć obiektu ACL sieci z punkt końcowy maszyn wirtualnych. Aby to zrobić, otwórz Azure środowiska PowerShell ISE. Skopiuj i wklej skrypt poniżej, konfigurowanie skrypt za pomocą własne wartości, a następnie uruchom skrypt.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Następne kroki

[Co to jest sieć listy kontroli dostępu (ACL)?](virtual-networks-acl.md)
