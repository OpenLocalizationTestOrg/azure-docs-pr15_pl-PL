<properties
    pageTitle="Resetowanie hasła lub konfigurację pulpitu zdalnego na maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Dowiedz się, jak zresetować hasło konta lub usług pulpitu zdalnego na maszyn wirtualnych systemu Windows za pomocą Azure portal lub Azure programu PowerShell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Jak zresetować hasło logowania w maszyn wirtualnych systemu Windows lub usługi pulpitu zdalnego

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Jeśli nie możesz połączyć maszyn wirtualnych systemu Windows (maszyn wirtualnych), można zresetować hasło administratora lokalnego lub resetowanie Konfiguracja usługi pulpitu zdalnego. Można użyć Azure portal albo rozszerzenia dostępu do maszyn wirtualnych w programie PowerShell Azure o zresetowanie hasła. Jeśli korzystasz z programu PowerShell, upewnij się, masz najnowsze modułu programu PowerShell zainstalowany na Twoim komputerze pracy i po zalogowaniu się do subskrypcji usługi Azure. Aby uzyskać szczegółowe instrukcje przeczytaj, [jak zainstalować i skonfigurować Azure programu PowerShell](../powershell-install-configure.md).

> [AZURE.TIP] Można sprawdzić wersję programu PowerShell zainstalowanym przy użyciu`Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version`

## <a name="windows-vms-in-resource-manager-deployment-model"></a>Maszyny wirtualne systemu Windows w modelu wdrożenia Menedżera zasobów

### <a name="azure-portal"></a>Azure Portal
Wybierz pozycję usługi maszyn wirtualnych, klikając przycisk **Przeglądaj** > **maszyn wirtualnych** > *komputera wirtualnych* > **wszystkie ustawienia** > **Resetowanie hasła**. Karta resetowania hasła jest wyświetlany:

![Strony do resetowania hasła](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Wprowadź nazwę użytkownika i nowe hasło, a następnie kliknij przycisk **Zapisz**. Spróbuj ponownie połączyć się z maszyn wirtualnych.

### <a name="vmaccess-extension-and-powershell"></a>Rozszerzenie VMAccess i programu PowerShell

Upewnij się, masz Azure PowerShell 1.0 lub nowszej i jest zarejestrowany w programie przy użyciu konta `Login-AzureRmAccount` polecenia cmdlet.

#### <a name="reset-the-local-administrator-account-password"></a>**Resetowanie hasła do konta administratora lokalnego**

Za pomocą polecenia programu PowerShell [AzureRmVMAccessExtension zestawu](https://msdn.microsoft.com/library/mt619447.aspx) można zresetować nazwy użytkownika i hasła administratora.

Utwórz lokalny administrator poświadczenia konta przy użyciu następującego polecenia:

    $cred=Get-Credential

Wpisz inną nazwę niż bieżącego konta, następujące polecenie rozszerzenia VMAccess zmienia nazwę lokalnego konta administratora, przypisuje hasło do tego konta, a problemy zdarzenia wylogowania pulpitu zdalnego. Po wyłączeniu lokalnego konta administratora z rozszerzeniem VMAccess umożliwia.

Rozszerzenie programu access maszyn wirtualnych umożliwia ustawienie nowe poświadczenia w następujący sposób:

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
        -Location WestUS -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Zamienianie `myRG`, `myVM`, `myVMAccess`oraz lokalizację z wartości odpowiednich konfigurację.


#### <a name="reset-the-remote-desktop-service-configuration"></a>**Resetowanie konfiguracji usługi pulpitu zdalnego**

Możesz przywrócić dostępu zdalnego do maszyn wirtualnych przy użyciu [Zestawu AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) lub [Zestawu AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx)w następujący sposób. (Zamień `myRG`, `myVM`, `myVMAccess` i lokalizacji własnego wartościami.)

    Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
        -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Lub:<br>

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Oba polecenia Dodaj nazwanych agenta dostępu do maszyn wirtualnych maszyn wirtualnych. W dowolnym momencie maszyny może zawierać tylko jeden pełnomocnik dostępu maszyn wirtualnych. Aby ustawić właściwości agenta dostępu maszyn wirtualnych pomyślnie, Usuń agenta dostępu do wcześniej ustawione przy użyciu `Remove-AzureRmVMAccessExtension` lub `Remove-AzureRmVMExtension`. Rozpoczynając od Azure programu PowerShell wersji 1.2.2, można uniknąć tego kroku podczas korzystania z `Set-AzureRmVMExtension` z `-ForceRerun` opcji. Podczas korzystania z `-ForceRerun`, upewnij się, że za pomocą tej samej nazwy dla maszyn wirtualnych agenta dostępu do określonych poprzedniego polecenia.

Jeśli nadal nie możesz połączyć zdalnie na komputerze wirtualnych, zobacz więcej czynności do wykonania na [połączeń Rozwiązywanie problemów z pulpitu zdalnego do maszyny wirtualnej systemu Windows Azure](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="windows-vms-in-the-classic-deployment-model"></a>Maszyny wirtualne systemu Windows w modelu Klasyczny wdrażania

### <a name="azure-portal"></a>Azure portal

Dla maszyn wirtualnych utworzony przy użyciu modelu Klasyczny wdrożenia, [Azure portal](https://portal.azure.com) Umożliwia resetowanie usługi pulpitu zdalnego. Kliknij pozycję: **Przeglądanie** > **maszyn wirtualnych (klasyczny)** > *komputera wirtualnych* > **Resetowanie zdalnego...**. Zostanie wyświetlona strona następujące.

![Resetowanie strony konfiguracji RDP](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

Możesz też spróbować Resetowanie nazwę i hasło lokalnego konta administratora. Kliknij pozycję: **Przeglądanie** > **maszyn wirtualnych (klasyczny)** > *komputera wirtualnych* > **wszystkie ustawienia** > **Resetowanie hasła**. Zostanie wyświetlona strona następujące.

![Strony do resetowania hasła](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Po wprowadzeniu nową nazwę użytkownika i hasło, kliknij przycisk **Zapisz**.

### <a name="vmaccess-extension-and-powershell"></a>Rozszerzenie VMAccess i programu PowerShell

Upewnij się, że Agent maszyn wirtualnych jest zainstalowany na komputerze wirtualnych. Rozszerzenie VMAccess nie musi być zainstalowany zanim będzie można używać, dopóki Agent maszyn wirtualnych jest dostępna. Upewnij się, że Agent maszyn wirtualnych jest już zainstalowany przy użyciu następującego polecenia. (Zamiast "myCloudService" i "myVM" według nazwy usługi w chmurze i usługi Głosowa, odpowiednio. Te nazwy można dowiedzieć się, uruchamiając `Get-AzureVM` bez parametrów.)

    $vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
    write-host $vm.VM.ProvisionGuestAgent

Jeśli polecenie **zapisu hosta** Wyświetla **wartość PRAWDA**, Agent maszyn wirtualnych jest zainstalowany. Jeśli zostanie wyświetlony **FAŁSZ**, zobacz instrukcje i łącza do pobrania [agenta maszyn wirtualnych i rozszerzenia — część 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure we wpisie w blogu.

Jeśli utworzono maszyny wirtualnej za pomocą portalu, sprawdź, czy `$vm.GetInstance().ProvisionGuestAgent` zwraca **wartość True**. W przeciwnym razie możesz go ustawić za pomocą tego polecenia:

    $vm.GetInstance().ProvisionGuestAgent = $true

To polecenie uniemożliwia następujący komunikat o błędzie, jeśli są uruchomione polecenie **Ustaw AzureVMExtension** w następnych kroków: "Agenta gościa świadczenia musi być włączona na obiekcie maszyn wirtualnych przed ustawieniem rozszerzenie programu Access IaaS maszyn wirtualnych."

#### <a name="reset-the-local-administrator-account-password"></a>**Resetowanie hasła do konta administratora lokalnego**

Tworzenie poświadczeń logowania przy użyciu bieżącej nazwy konta administratora lokalnego i nowe hasło, a następnie uruchom `Set-AzureVMAccessExtension` w następujący sposób.

    $cred=Get-Credential
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Wpisz inną nazwę niż bieżącego konta, z rozszerzeniem VMAccess zmienia nazwę lokalnego konta administratora, przypisuje hasło do tego konta, a problemy wyrejestrowywania pulpitu zdalnego. Po wyłączeniu lokalnego konta administratora z rozszerzeniem VMAccess umożliwia.

Te polecenia również zresetowanie konfiguracji usługi pulpitu zdalnego.

#### <a name="reset-the-remote-desktop-service-configuration"></a>**Resetowanie konfiguracji usługi pulpitu zdalnego**

Aby zresetować konfigurację usługi pulpitu zdalnego, uruchom następujące polecenie:

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

Rozszerzenie VMAccess uruchamia dwa polecenia na komputerze wirtualnej:

- `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

To polecenie włącza wbudowanej grupy Zapory systemu Windows, która zezwala na ruch przychodzący pulpitu zdalnego, która korzysta z portu TCP 3389.

- `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

To polecenie ustawia fDenyTSConnections wartości rejestru 0, umożliwiając połączeń pulpitu zdalnego.


## <a name="next-steps"></a>Następne kroki

Jeśli nie odpowiada Azure maszyn wirtualnych rozszerzenie programu access i nie można zresetować hasło, możesz [zresetować hasło lokalnego systemu Windows w trybie offline](virtual-machines-windows-reset-local-password-without-agent.md). Ta metoda polega na bardziej zaawansowane i wymaga nawiązania połączenia wirtualnej dysk twardy problematyczny maszyn wirtualnych innego maszyn wirtualnych. Wykonaj kroki opisane w tym artykule najpierw, a następnie spróbuj tylko metody resetowania hasła offline ostatecznym.

[Azure rozszerzenia maszyn wirtualnych i funkcje](virtual-machines-windows-extensions-features.md)

[Nawiązywanie połączenia z Azure maszyn wirtualnych RDP lub SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Rozwiązywanie problemów z połączeniami pulpitu zdalnego do maszyny wirtualnej systemu Windows Azure](virtual-machines-windows-troubleshoot-rdp-connection.md)
