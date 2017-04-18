<properties
    pageTitle="Nie można RDP do Azure maszyn wirtualnych | Microsoft Azure"
    description="Rozwiązywanie problemów w przypadku nie możesz połączyć się z maszyny wirtualnej systemu Windows Azure za pomocą pulpitu zdalnego"
    keywords="Błąd pulpitu zdalnego, błąd Podłączanie pulpitu zdalnego, nie może nawiązać połączenia Głosowa, rozwiązywanie problemów pulpitu zdalnego"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/26/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Rozwiązywanie problemów z połączeniami pulpitu zdalnego Azure maszyn wirtualnych

Połączenia protokołu RDP (Remote Desktop) z systemem Windows Azure maszyn wirtualnych (maszyn wirtualnych) może się nie powieść z różnych powodów, pozostawiając użytkownik nie może uzyskać dostępu do maszyn wirtualnych. Ten problem może być za pomocą usługi pulpitu zdalnego na maszyn wirtualnych, połączenie sieciowe lub klienta pulpitu zdalnego na komputerze hosta. Ten artykuł prowadzi użytkownika przez niektóre z najczęściej używanych metod rozwiązywać problemy z połączeniem RDP. 

Jeśli potrzebujesz dodatkowej pomocy w dowolnym momencie, w tym artykule, można skontaktować się Azure ekspertów na [MSDN Azure i fora przepełnienie stosu](https://azure.microsoft.com/support/forums/). Można także pliku Azure pomocy technicznej zdarzenia. Przejdź do [witryny pomocy technicznej Azure](https://azure.microsoft.com/support/options/) i wybierz pozycję **Uzyskiwanie pomocy technicznej**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Szybkie kroki rozwiązywania problemów
Po wykonaniu każdej czynności rozwiązywania problemów spróbuj ponownie połączyć się maszyn wirtualnych:

1. Resetowanie konfiguracji pulpitu zdalnego.
2. Zaznacz grupę zabezpieczeń sieci zasady / Cloud Services punktów końcowych.
3. Przejrzyj dzienniki konsoli maszyn wirtualnych.
4. Sprawdzanie kondycji zasobu maszyn wirtualnych.
5. Zresetuj hasło maszyn wirtualnych.
6. Uruchom ponownie usługi maszyn wirtualnych.
7. Ponownie wdróż do maszyn wirtualnych.

Jeśli potrzebujesz bardziej szczegółowe kroki i objaśnienia wrócić do czytania.

> [AZURE.TIP] Jeśli przycisk **Połącz** z maszyn wirtualnych jest wyszarzona, w portalu i nie nawiązano Azure za pośrednictwem połączenia [VPN witryny do witryny](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) lub [Rozsyłania Express](../expressroute/expressroute-introduction.md) , musisz utworzyć i przypisać do maszyn wirtualnych publiczny adres IP, zanim będzie można używać RDP. Więcej informacji o [publicznych adresów IP w Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-to-troubleshoot-rdp-issues"></a>Sposoby rozwiązywania problemów RDP
Możesz rozwiązać maszyny wirtualne utworzony przy użyciu modelu wdrożenia Menedżera zasobów, stosując jedną z następujących metod:

- [Azure portal](#using-the-azure-portal) - doskonałe, jeśli chcesz szybko resetowanie poświadczeń RDP konfiguracji lub użytkownika, a nie masz zainstalowane narzędzia Azure.
- [Azure programu PowerShell](#using-azure-powershell) — Jeśli masz doświadczenia w wierszu polecenia programu PowerShell szybko resetowanie poświadczeń konfiguracji lub użytkownika RDP przy użyciu poleceń cmdlet programu PowerShell Azure.

Można również instrukcje dotyczące rozwiązywania problemów z maszyny wirtualne utworzony przy użyciu [modelu wdrożenia klasyczny](#troubleshoot-vms-created-using-the-classic-deployment-model).


<a id="fix-common-remote-desktop-errors"></a>
## <a name="troubleshoot-using-the-azure-portal"></a>Rozwiązywanie problemów z używaniem Azure portal
Po wykonaniu każdej czynności rozwiązywania problemów spróbuj ponownie połączyć się z maszyn wirtualnych. Jeśli nadal nie możesz połączyć, przejdź do następnego kroku.

1. **Resetowanie połączenia RDP**. W tym kroku rozwiązywania problemów Resetuje konfigurację RDP podczas połączenia zdalnego są wyłączone lub reguły zapory systemu Windows blokują RDP, na przykład.

    Wybierz swojego maszyn wirtualnych w portalu Azure. Przewiń w dół w okienku ustawienia w sekcji **Obsługa + Rozwiązywanie problemów** u dołu listy. Kliknij przycisk **Resetuj hasło** . Ustawianie **trybu** zresetować **tylko konfiguracja** , a następnie kliknij przycisk **Aktualizuj** :

    ![Zresetuj konfigurację RDP w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-rdp.png)

2. **Sprawdź grupy zabezpieczeń sieci reguły**. W tym kroku rozwiązywania problemów sprawdza, czy masz regułę w grupie zabezpieczeń sieci, aby zezwolić na ruch RDP. Domyślny port RDP to TCP port 3389. Reguły w celu zezwolenia na ruch RDP może nie zostać utworzony automatycznie podczas tworzenia usługi maszyn wirtualnych.

    Wybierz swojego maszyn wirtualnych w portalu Azure. Kliknij pozycję **interfejsy** z okienka ustawienia.

    ![Wyświetlanie interfejsów sieciowych maszyn wirtualnych w Azure portal](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-network-interfaces.png)

    Wybierz usługi sieciowej z listy (jest zazwyczaj tylko jeden):

    ![Wybierz pozycję sieciowej w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-interface.png)

    Wybierz **grupę zabezpieczeń sieci** do wyświetlania grupy zabezpieczeń sieci skojarzony z sieciowej:

    ![Zaznacz grupę zabezpieczeń sieci w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-nsg.png)

    Upewnij się, że istnieje regułę ruchu przychodzącego, która zezwala na ruch RDP na porcie TCP 3389. W poniższym przykładzie pokazano regułę prawidłowych zabezpieczeń, która zezwala na ruch RDP. Widać `Service` i `Action` zostały skonfigurowane poprawnie:

    ![Sprawdź reguły RDP NSG w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/verify-nsg-rules.png)

    Jeśli nie masz regułę, która umożliwia ruch RDP, [Utwórz regułę grupy zabezpieczeń sieci](virtual-machines-windows-nsg-quickstart-portal.md). Zezwalaj na TCP port 3389.

3. **Narzędzia diagnostyczne uruchamiania maszyn wirtualnych Recenzja**. W tym kroku rozwiązywania problemów przegląda dzienniki konsoli maszyn wirtualnych w celu określenia, jeśli maszyn wirtualnych jest zgłoszenie problemu. Nie wszystkie maszyny wirtualne ma włączone, Diagnostyka uruchamiania, więc tego kroku rozwiązywania problemów mogą być opcjonalne.
    
    Określonych kroków rozwiązywania problemu wykraczają poza zakres tego artykułu, ale mogą wskazywać większą problemu, który ma wpływ na łączność RDP. Aby uzyskać więcej informacji na temat przeglądania konsoli Dzienniki i maszyn wirtualnych zrzut ekranu zobacz [Informacje diagnostyczne dla maszyny wirtualne uruchamiania](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **Sprawdzanie kondycji zasobu maszyn wirtualnych**. W tym kroku rozwiązywania problemów sprawdza, czy nie są znane problemy z platformy Azure, które mogą mieć wpływ na łączności z programem maszyn wirtualnych.

    Wybierz swojego maszyn wirtualnych w portalu Azure. Przewiń w dół w okienku ustawienia w sekcji **Obsługa + Rozwiązywanie problemów** u dołu listy. Kliknij przycisk **kondycji zasobu** . Raporty prawidłowy maszyn wirtualnych jako **dostępne**:

    ![Sprawdzanie kondycji zasobu maszyn wirtualnych w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/check-resource-health.png)

5. **Resetowanie poświadczeń użytkownika**. W tym kroku rozwiązywania problemów pozwala zresetować hasło lokalnego konta administratora podczas pewności lub pamiętasz poświadczeń.

    Wybierz swojego maszyn wirtualnych w portalu Azure. Przewiń w dół w okienku ustawienia w sekcji **Obsługa + Rozwiązywanie problemów** u dołu listy. Kliknij przycisk **Resetuj hasło** . Upewnij się, że **Tryb** jest ustawiony na **zresetować hasło** , a następnie wprowadź nazwę użytkownika i nowe hasło. Na koniec kliknij przycisk **Aktualizuj** :

    ![Resetowanie poświadczeń użytkownika w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-password.png)

6. **Ponowne uruchamianie usługi maszyn wirtualnych**. W tym kroku rozwiązywania problemów można rozwiązać wszelkie źródłowych problemów, jakie występują maszyn wirtualnych się.

    Wybierz swojego maszyn wirtualnych w portalu Azure, a następnie kliknij kartę **Przegląd** . Kliknij przycisk **Uruchom ponownie** :

    ![Ponowne uruchamianie maszyn wirtualnych w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/restart-vm.png)

7. **Ponownie wdróż do maszyn wirtualnych**. W tym kroku rozwiązywania problemów redeploys z maszyn wirtualnych do innego hosta w Azure, aby rozwiązać problemy z sieci lub dowolny podstawowej platformy.

    Wybierz swojego maszyn wirtualnych w portalu Azure. Przewiń w dół w okienku ustawienia w sekcji **Obsługa + Rozwiązywanie problemów** u dołu listy. Kliknij przycisk **ponownie wdróż** , a następnie kliknij pozycję **Rozmieść ponownie**:

    ![Ponownie wdróż maszyn wirtualnych w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/redeploy-vm.png)

    Po zakończeniu tej operacji tymczasowych dysku dane zostaną utracone i aktualizowania dynamiczne adresy IP, które są skojarzone z maszyn wirtualnych.

Jeśli są wciąż występują problemy z RDP, możesz [otworzyć prośbę o pomoc techniczną](https://azure.microsoft.com/support/options/) lub więcej [bardziej szczegółowych pomysłów dotyczących rozwiązywania problemów RDP i czynności](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-using-azure-powershell"></a>Rozwiązywanie problemów z używaniem Azure programu PowerShell
Jeśli jeszcze tego nie zrobiono, [Zainstaluj i skonfiguruj najnowszą Azure programu PowerShell](../powershell-install-configure.md).

W poniższych przykładach użyto zmiennych `myResourceGroup`, `myVM`, i `myVMAccessExtension`. Zamień te nazwy i lokalizacji własne wartości.

> [AZURE.NOTE] Za pomocą polecenia cmdlet [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) programu PowerShell resetowanie poświadczeń użytkownika i konfiguracji RDP. W poniższych przykładach `myVMAccessExtension` jest nazwą użytkownika w ramach procesu. Jeśli już masz doświadczenia w VMAccessAgent, nazwę istniejącego rozszerzenia można uzyskać za pomocą `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` Aby sprawdzić właściwości maszyn wirtualnych. Aby wyświetlić nazwy, poszukaj w sekcji "Rozszerzenia" danych wyjściowych.

Po wykonaniu każdej czynności rozwiązywania problemów spróbuj ponownie połączyć się z maszyn wirtualnych. Jeśli nadal nie możesz połączyć, przejdź do następnego kroku.

1. **Resetowanie połączenia RDP**. W tym kroku rozwiązywania problemów Resetuje konfigurację RDP podczas połączenia zdalnego są wyłączone lub reguły zapory systemu Windows blokują RDP, na przykład.

    Polecenie Obserwuj resetuje połączenie RDP na maszyny o nazwie `myVM` w `WestUS` lokalizacji i w grupie zasobów o nazwie `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```

2. **Sprawdź grupy zabezpieczeń sieci reguły**. W tym kroku rozwiązywania problemów sprawdza, czy masz regułę w grupie zabezpieczeń sieci, aby zezwolić na ruch RDP. Domyślny port RDP to TCP port 3389. Reguły w celu zezwolenia na ruch RDP może nie zostać utworzony automatycznie podczas tworzenia usługi maszyn wirtualnych.

    Najpierw przypisać wszystkie dane konfiguracji grupy zabezpieczeń sieci do `$rules` zmiennej. Poniższy przykład uzyskuje informacje dotyczące grupy zabezpieczeń sieci o nazwie `myNetworkSecurityGroup` w grupie zasobów o nazwie `myResourceGroup`:

    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```

    Teraz wyświetlić reguł, które są skonfigurowane dla tej grupy zabezpieczeń sieci. Sprawdź, czy reguły istnieje umożliwia port TCP 3389 połączeń przychodzących w następujący sposób:

    ```powershell
    $rules.SecurityRules
    ```

    W poniższym przykładzie pokazano regułę prawidłowych zabezpieczeń, która zezwala na ruch RDP. Widać `Protocol`, `DestinationPortRange`, `Access`, i `Direction` zostały skonfigurowane poprawnie:

    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```

    Jeśli nie masz regułę, która umożliwia ruch RDP, [Utwórz regułę grupy zabezpieczeń sieci](virtual-machines-windows-nsg-quickstart-powershell.md). Zezwalaj na TCP port 3389.

3. **Resetowanie poświadczeń użytkownika**. W tym kroku rozwiązywania problemów pozwala zresetować hasło lokalnego konta administratora przez użytkownika podczas pewności lub pamiętasz poświadczeń.

    Najpierw określ nazwę użytkownika i hasło nowego przypisując poświadczenia, aby `$cred` zmiennej w następujący sposób:

    ```powershell
    $cred=Get-Credential
    ```

    Teraz zaktualizować poświadczenia na swojej maszyn wirtualnych. Poniższy przykład aktualizuje poświadczenia na maszyny o nazwie `myVM` w `WestUS` lokalizacji i w grupie zasobów o nazwie `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```

4. **Ponowne uruchamianie usługi maszyn wirtualnych**. W tym kroku rozwiązywania problemów można rozwiązać wszelkie źródłowych problemów, jakie występują maszyn wirtualnych się.

    Poniższy przykład uruchomieniu maszyn wirtualnych, o nazwie `myVM` w grupie zasobów o nazwie `myResourceGroup`:

    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```

5. **Ponownie wdróż do maszyn wirtualnych**. W tym kroku rozwiązywania problemów redeploys z maszyn wirtualnych do innego hosta w Azure, aby rozwiązać problemy z sieci lub dowolny podstawowej platformy.

    Poniższy przykład redeploys maszyn wirtualnych, o nazwie `myVM` w `WestUS` lokalizacji i w grupie zasobów o nazwie `myResourceGroup`:

    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Jeśli są wciąż występują problemy z RDP, możesz [otworzyć prośbę o pomoc techniczną](https://azure.microsoft.com/support/options/) lub więcej [bardziej szczegółowych pomysłów dotyczących rozwiązywania problemów RDP i czynności](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Rozwiązywanie problemów z maszyny wirtualne utworzony przy użyciu modelu wdrożenia klasyczny

Po wykonaniu każdej czynności rozwiązywania problemów spróbuj ponownie połączyć się maszyn wirtualnych.

1. **Resetowanie połączenia RDP**. W tym kroku rozwiązywania problemów Resetuje konfigurację RDP podczas połączenia zdalnego są wyłączone lub reguły zapory systemu Windows blokują RDP, na przykład.

    Wybierz swojego maszyn wirtualnych w portalu Azure. Kliknij przycisk **... Więcej** przycisk, a następnie kliknij przycisk **Resetuj dostępu zdalnego**:

    ![Zresetuj konfigurację RDP w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-rdp.png)

2.  **Punkty końcowe Sprawdź usług w chmurze**. W tym kroku rozwiązywania problemów sprawdza mają punkty końcowe w chmurze usług do zezwolenia na ruch RDP. Domyślny port RDP to TCP port 3389. Reguły w celu zezwolenia na ruch RDP może nie zostać utworzony automatycznie podczas tworzenia usługi maszyn wirtualnych.

    Wybierz swojego maszyn wirtualnych w portalu Azure. Kliknij przycisk **punkty końcowe** , aby wyświetlić punkty końcowe obecnie skonfigurowane dla swojego maszyn wirtualnych. Upewnij się, że punkty końcowe istnieje umożliwiająca ruch RDP na porcie TCP 3389.
    
    W poniższym przykładzie pokazano prawidłowe punkty końcowe pozwalające RDP ruchu:

    ![Weryfikowanie usług w chmurze punkty końcowe w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)

    Jeśli nie masz punktu końcowego, która umożliwia ruch RDP, [utworzyć punkt końcowy usług w chmurze](virtual-machines-windows-classic-setup-endpoints.md). Port TCP Zezwól na prywatne portu 3389.

3. **Narzędzia diagnostyczne uruchamiania maszyn wirtualnych Recenzja**. W tym kroku rozwiązywania problemów przegląda dzienniki konsoli maszyn wirtualnych w celu określenia, jeśli maszyn wirtualnych jest zgłoszenie problemu. Nie wszystkie maszyny wirtualne ma włączone, Diagnostyka uruchamiania, więc tego kroku rozwiązywania problemów mogą być opcjonalne.
    
    Określonych kroków rozwiązywania problemu wykraczają poza zakres tego artykułu, ale mogą wskazywać większą problemu, który ma wpływ na łączność RDP. Aby uzyskać więcej informacji na temat przeglądania konsoli Dzienniki i maszyn wirtualnych zrzut ekranu zobacz [Informacje diagnostyczne dla maszyny wirtualne uruchamiania](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **Sprawdzanie kondycji zasobu maszyn wirtualnych**. W tym kroku rozwiązywania problemów sprawdza, czy nie są znane problemy z platformy Azure, które mogą mieć wpływ na łączności z programem maszyn wirtualnych.

    Wybierz swojego maszyn wirtualnych w portalu Azure. Przewiń w dół w okienku ustawienia w sekcji **Obsługa + Rozwiązywanie problemów** u dołu listy. Kliknij przycisk **Kondycji zasobu** . Raporty prawidłowy maszyn wirtualnych jako **dostępne**:

    ![Sprawdzanie kondycji zasobu maszyn wirtualnych w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-check-resource-health.png)

5. **Resetowanie poświadczeń użytkownika**. W tym kroku rozwiązywania problemów pozwala zresetować hasło lokalnego konta administratora przez użytkownika podczas pewności lub pamiętasz poświadczeń.

    Wybierz swojego maszyn wirtualnych w portalu Azure. Przewiń w dół w okienku ustawienia w sekcji **Obsługa + Rozwiązywanie problemów** u dołu listy. Kliknij przycisk **Resetuj hasło** . Wprowadź nazwę użytkownika i nowe hasło. Na koniec kliknij przycisk **Zapisz** :

    ![Resetowanie poświadczeń użytkownika w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-password.png)

6. **Ponowne uruchamianie usługi maszyn wirtualnych**. W tym kroku rozwiązywania problemów można rozwiązać wszelkie źródłowych problemów, jakie występują maszyn wirtualnych się.

    Wybierz swojego maszyn wirtualnych w portalu Azure, a następnie kliknij kartę **Przegląd** . Kliknij przycisk **Uruchom ponownie** :

    ![Ponowne uruchamianie maszyn wirtualnych w portalu Azure](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-restart-vm.png)
    
Jeśli są wciąż występują problemy z RDP, możesz [otworzyć prośbę o pomoc techniczną](https://azure.microsoft.com/support/options/) lub więcej [bardziej szczegółowych pomysłów dotyczących rozwiązywania problemów RDP i czynności](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-specific-rdp-errors"></a>Rozwiązywanie problemów z błędami określonych RDP
Komunikat o błędzie mogą wystąpić podczas próby połączenia z maszyn wirtualnych za pośrednictwem RDP. Poniżej przedstawiono najczęstsze komunikaty o błędach:

- [Sesja zdalna uległa rozłączeniu, ponieważ do dyspozycji licencji serwery licencji zdalnego pulpitu są](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdplicense).
- [Pulpit zdalny nie może znaleźć komputer "Nazwa"](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpname).
- Wystąpił błąd [uwierzytelniania. Nie można skontaktować się z lokalnego serwera zabezpieczeń](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpauth).
- [Błąd zabezpieczeń systemu Windows: poświadczenia nie działa](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#wincred).
- [Ten komputer nie może połączyć się z komputerem zdalnym](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpconnect).


## <a name="additional-resources"></a>Dodatkowe zasoby
Jeśli żadna z tych błędów wystąpił i nadal nie możesz połączyć się maszyn wirtualnych za pomocą pulpitu zdalnego, przeczytaj szczegółowy [Podręcznik dla pulpitu zdalnego rozwiązywania problemów](virtual-machines-windows-detailed-troubleshoot-rdp.md).

- [Azure pakiet Diagnostyka IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Aby uzyskać czynności na dostęp do aplikacji uruchomionych maszyny rozwiązywania problemów, zobacz [Rozwiązywanie problemów z dostępem do uruchamiania aplikacji na maszyn wirtualnych Azure](virtual-machines-linux-troubleshoot-app-connection.md).
- Jeśli występują problemy dotyczące nawiązywania połączenia z maszyny Linux platformy Azure za pomocą Secure Shell (SSH), zobacz [Rozwiązywanie problemów z SSH połączenia maszyny Linux platformy Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).
