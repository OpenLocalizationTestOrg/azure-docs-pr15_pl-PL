<properties
    pageTitle="Azure Active Directory Domain Services: Przewodnik po administracji | Microsoft Azure"
    description="Dołączanie maszyny wirtualnej systemu Windows z domeną zarządzane przy użyciu programu PowerShell Azure i model klasyczny wdrożenia."
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="maheshu"/>


# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a>Dołączanie do maszyny wirtualnej Windows Server domenie zarządzanych za pomocą programu PowerShell

> [AZURE.SELECTOR]
- [Portal Azure klasyczny - systemu Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell — systemu Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

> [AZURE.IMPORTANT] Azure występują dwa modele rozmieszczania służące do tworzenia i pracy z zasobami: [Menedżer zasobów i klasyczny](../resource-manager-deployment-model.md). W tym artykule opisano, jak przy użyciu modelu Klasyczny wdrożenia. Azure usług domenowych AD nie obsługuje obecnie modelu Menedżera zasobów.

Te kroki pokazano, jak dostosować zestawu polecenia programu PowerShell Azure, które tworzą i wstępnie skonfigurować maszyny wirtualnej systemu Windows Azure za pomocą metody bloku konstrukcyjnego. Poniższe czynności ułatwiają tworzenie maszyny wirtualnej systemu Windows Azure i dołączyć go do Azure AD domenie usług domenowych zarządzane.

Te kroki należy wykonać podejście wypełniania-w--pustych komórek do tworzenia zestawów polecenia programu PowerShell Azure. Takie rozwiązanie może być przydatne, jeśli jesteś nowym użytkownikiem programu PowerShell lub chcesz się dowiedzieć, jakie wartości Określ pomyślnego konfiguracji. Zaawansowanych użytkowników programu PowerShell poleceń, można zastąpić wartości dla zmiennych (wiersze rozpoczynające się od "$").

Jeśli nie zrobiono tego wcześniej, skorzystaj z instrukcji w [sposób instalowania i konfigurowania środowiska PowerShell Azure](../powershell-install-configure.md) instalowania programu PowerShell Azure na komputerze lokalnym. Następnie otwórz wiersz polecenia programu Windows PowerShell.

## <a name="step-1-add-your-account"></a>Krok 1: Dodawanie konta

1. W wierszu polecenia programu PowerShell wpisz **AzureAccount Dodaj** , a następnie naciśnij klawisz **Enter**.
2. Wpisz adres e-mail skojarzonego z subskrypcją usługi Azure i kliknij przycisk **Kontynuuj**.
3. Wpisz hasło do konta.
4. Kliknij przycisk **Zaloguj**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Krok 2: Ustawianie subskrypcję i konta miejsca do magazynowania

Ustawianie Azure subskrypcji i konta miejsca do magazynowania, uruchamiając następujące polecenia w wierszu polecenia środowiska Windows PowerShell. Zamień wszystko w obrębie ofert, łącznie z < i > znaków, nazwami poprawne.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Nazwa poprawne subskrypcji możesz przejść z właściwości SubscriptionName dane wyjściowe polecenia **Get-AzureSubscription** . Po uruchomieniu polecenia **Wybierz AzureSubscription** , można uzyskać nazwę konta magazynu poprawne z właściwości Etykieta danych wyjściowych polecenia **Get-AzureStorageAccount** .


## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a>Krok 3: Przewodnik krok po kroku — obsługi administracyjnej maszyny wirtualnej i dołączyć go do domeny zarządzanych
Oto polecenia programu Azure PowerShell odpowiednich ustawiona na tworzenie tej maszyny wirtualnej przy użyciu pustych wierszy między każdego bloku w celu zwiększenia czytelności.

Określanie informacji o komputerze wirtualnych systemu Windows ma zostać zastrzeżona.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

W przypadku wartości InstanceSize D-DS- lub seria G maszyn wirtualnych, zobacz [maszyn wirtualnych i rozmiarów usługi Cloud Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Wprowadź informacje dotyczące zarządzanych domeny.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Określ nazwę usługi w chmurze.

    $svcname="Contoso100-test"

Określ nazwę wirtualnej sieci, do którego powinny zostać połączone maszyn wirtualnych. Upewnij się, że domena zarządzanych AAD DS jest dostępne w tym wirtualnej sieci.

    $vnetname="MyPreviewVnet"

Zaznacz obraz maszyn wirtualnych może być używany do zapewniania obsługi maszyn wirtualnych.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Konfigurowanie maszyn wirtualnych - nazwa maszyn wirtualnych zestawu, wystąpienie, rozmiar i obrazów.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Uzyskaj poświadczenia administratora lokalnego dla maszyn wirtualnych. Wybierz hasło silne administratora lokalnego.

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

Uzyskaj poświadczenia konta użytkownika należące do grupy "Administratorzy AAD kontrolera domeny" do połączenia maszyn wirtualnych zarządzanych domeny. Nie określisz nazwę domeny — na przykład w naszym przykładzie, "Robert" jest określona jako nazwę użytkownika.

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

Konfigurowanie maszyn wirtualnych — Określanie wymogu dołączania do domeny i wymagane poświadczenia.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Ustaw podsieci dla maszyn wirtualnych.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Opcjonalnie: Wskaż adres IP domeny. Jeśli ustawisz adresy IP domeny zarządzanych usług domenowych AD Azure do serwerów DNS dla wirtualnej sieci, ten krok nie jest wymagany.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Teraz zapewnienie maszyn wirtualnych systemu Windows domeny.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a>Skrypt do obsługi administracyjnej maszyn wirtualnych systemu Windows i automatycznie dołączyć go do AAD domenie usług domenowych zarządzanych
Tworzy wartość polecenia programu PowerShell maszyny wirtualnej na serwerze programu LOB, który:

- Używa obrazu systemu Windows Server 2012 R2 w centrum danych.
- Jest bardzo mały maszyn wirtualnych.
- Zawiera nazwę firmy contoso — test.
- Jest automatycznie domeny dołączony do domeny zarządzanych contoso100.
- Zostanie dodany do tej samej sieci wirtualnej zarządzanych domeny.

Poniżej przedstawiono pełną przykładowy skrypt do tworzenia maszyny wirtualnej systemu Windows i automatycznie dołączyć go do domeny zarządzanych usług domenowych AD Azure.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a>Zawartość pokrewna
- [Azure usługach domenowych AD - przewodnik wprowadzenie](./active-directory-ds-getting-started.md)

- [Administrowanie Azure AD domenie usług domenowych zarządzanych](./active-directory-ds-admin-guide-administer-domain.md)
