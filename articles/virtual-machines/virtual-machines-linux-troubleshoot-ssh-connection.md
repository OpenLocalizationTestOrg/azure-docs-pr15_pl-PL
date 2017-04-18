<properties
    pageTitle="Rozwiązywanie problemów z nawiązywaniem połączeń SSH do maszyny | Microsoft Azure"
    description="Jak rozwiązywać problemy, takie jak "SSH połączenie nie powiodło się" lub "Odmowa połączenia SSH" dla maszyn wirtualnych Azure systemem Linux."
    keywords="SSH odmowa połączenia, ssh błąd, azure ssh, SSH połączenie nie powiodło się"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-ssh-connections-to-an-azure-linux-vm-that-fails-errors-out-or-is-refused"></a>Rozwiązywanie problemów z połączeniami SSH do maszyn wirtualnych Linux Azure, który nie powiedzie się, błędów, lub jest odrzucone
Istnieją różne powody, że wystąpiły błędy Secure Shell (SSH), SSH błędy połączenia, lub SSH jest odrzucone podczas próby nawiązania połączenia Linux maszyn wirtualnych (maszyn wirtualnych). Ten artykuł ułatwia Znajdź i rozwiąż problemy. Aby rozwiązać problemy z połączeniem, można użyć portal Azure, polecenie Azure lub rozszerzenia dostępu do maszyn wirtualnych Linux.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Jeśli potrzebujesz dodatkowej pomocy w dowolnym momencie, w tym artykule, można skontaktować się Azure ekspertów na [MSDN Azure i fora przepełnienie stosu](http://azure.microsoft.com/support/forums/). Można także pliku Azure pomocy technicznej zdarzenia. Przejdź do [witryny pomocy technicznej Azure](http://azure.microsoft.com/support/options/) i wybierz pozycję **Uzyskiwanie pomocy technicznej**. Aby dowiedzieć się, jak za pomocą Azure pomocy technicznej przeczytaj [często zadawane pytania dotyczące pomocy technicznej Microsoft Azure](http://azure.microsoft.com/support/faq/).


## <a name="quick-troubleshooting-steps"></a>Szybkie kroki rozwiązywania problemów
Po wykonaniu każdej czynności rozwiązywania problemów spróbuj ponownie połączyć się maszyn wirtualnych.

1. Zresetuj konfigurację SSH.
2. Resetowanie poświadczeń dla użytkownika.
3. Sprawdź, czy zasady [Grupy zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md) pozwalają na ruch SSH.
    - Upewnij się, że reguła grupy zabezpieczeń sieci istnieje Aby zezwolić na ruch SSH (domyślnie TCP port 22).
    - Nie można używać przekierowywanie portu / mapowanie bez używania usługi równoważenia obciążenia Azure.
4. Sprawdzanie [kondycji zasobu maszyn wirtualnych](../resource-health/resource-health-overview.md). 
    - Upewnij się, że maszyn wirtualnych raporty jako prawidłowy.
    - Jeśli masz diagnostyki uruchamiania włączone, sprawdź, czy maszyn wirtualnych nie jest raportowanie błędów uruchamiania w dzienniku.
5. Uruchom ponownie maszyn wirtualnych.
6. Ponownie wdróż maszyn wirtualnych.

Kontynuuj odczytu dla bardziej szczegółowych instrukcji rozwiązywania problemów i objaśnienia.


## <a name="available-methods-to-troubleshoot-ssh-connection-issues"></a>Dostępne metody SSH rozwiązywania problemów z połączeniem

Możesz zresetować poświadczenia lub konfiguracji SSH przy użyciu jednej z następujących metod:

- [Azure portal](#using-the-azure-portal) - doskonałe, jeśli chcesz szybko Resetowanie konfiguracji SSH lub klawisz SSH, jeśli nie masz zainstalowane narzędzia Azure.
- [Polecenie azure polecenia](#using-the-azure-cli) - Jeśli znajdujesz się w wierszu polecenia szybko Resetowanie konfiguracji SSH lub poświadczeń.
- [Rozszerzenie azure VMAccessForLinux](#using-the-vmaccess-extension) — tworzenie i ponowne używanie plików definicji json resetowanie poświadczeń SSH konfiguracji lub użytkownika.

Po wykonaniu każdej czynności rozwiązywania problemów spróbuj ponownie połączyć się z maszyn wirtualnych. Jeśli nadal nie możesz połączyć, przejdź do następnego kroku.


## <a name="using-the-azure-portal"></a>Za pomocą portalu Azure
Azure portal umożliwia szybkie resetowanie poświadczeń SSH konfiguracji lub użytkownika bez instalowania narzędzi na komputerze lokalnym.

Wybierz swojego maszyn wirtualnych w portalu Azure. Przewiń w dół do sekcji **pomocy technicznej + rozwiązywanie problemów** i wybierz pozycję **Resetuj hasło** , jak w poniższym przykładzie:

![Resetowanie konfiguracji SSH lub poświadczeń w portalu Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/reset-credentials-using-portal.png)

### <a name="reset-the-ssh-configuration"></a>Zresetuj konfigurację SSH
Pierwszym krokiem, wybierz pozycję `Reset SSH configuration only` z menu rozwijanego **Tryb** tak jak w poprzednim zrzut ekranu, a następnie kliknij przycisk **Resetuj** . Po zakończeniu tej akcji, spróbuj ponownie uzyskać dostęp do maszyn wirtualnych.

### <a name="reset-ssh-credentials-for-a-user"></a>Resetowanie poświadczeń SSH dla użytkownika
Resetowanie poświadczeń istniejącego użytkownika, wybierz jedną z opcji `Reset SSH public key` lub `Reset password` z menu rozwijanego **Tryb** , tak jak w poprzednim zrzut ekranu. Określ nazwę użytkownika i klawisz SSH lub nowe hasło, a następnie kliknij przycisk **Resetuj** .

Można także tworzyć użytkownika z uprawnieniami sudo na maszyn wirtualnych z tego menu. Wprowadź nową nazwę użytkownika i hasło skojarzone lub SSH klawisz, a następnie kliknij przycisk **Resetuj** .


## <a name="using-the-azure-cli"></a>Za pomocą polecenie Azure
Jeśli jeszcze tego nie zrobiono, [Zainstaluj polecenie Azure i łączność z subskrypcji usługi Azure](../xplat-cli-install.md). Upewnij się, że trybie Menedżera zasobów w następujący sposób:

```
azure config mode arm
```

Jeśli została utworzona i przekazać obraz niestandardowy dysku Linux upewnić się, że [Program Microsoft Azure Linux Agent](virtual-machines-linux-agent-user-guide.md) wersji 2.0.5 lub później jest zainstalowany. Dla maszyny wirtualne utworzony przy użyciu galerii obrazów to rozszerzenie programu access jest już zainstalowane i skonfigurowane dla Ciebie.

### <a name="reset-ssh-configuration"></a>Resetowanie SSH konfiguracji
Konfiguracja SSHD, sam może być nieprawidłowo skonfigurowane lub usługi wystąpił błąd. Możesz zresetować SSHD, aby upewnić się, że konfiguracja SSH sam jest prawidłowy. Resetowanie SSHD należy najpierw rozwiązywania problemów, które możesz wykonać.

Poniższy przykład resetuje SSHD na maszyny o nazwie `myVM` w grupie zasobów o nazwie `myResourceGroup`. Używanie własnych nazw grup zasobów i maszyn wirtualnych w następujący sposób:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --reset-ssh
```

### <a name="reset-ssh-credentials-for-a-user"></a>Resetowanie poświadczeń SSH dla użytkownika
Jeśli SSHD wydaje się działać poprawnie, możesz resetować hasła dla użytkownika giver. Poniższy przykład resetuje poświadczenia dla `myUsername` do wartości określonej w `myPassword`, na maszyn wirtualnych, o nazwie `myVM` w `myResourceGroup`. Użyj własnego wartości w następujący sposób:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
     --username myUsername --password myPassword
```

Jeśli używane uwierzytelnianie oparte na kluczu SSH, możesz zresetować klucz SSH dla danego użytkownika. Poniższy przykład aktualizuje klucz SSH przechowywane w `~/.ssh/azure_id_rsa.pub` dla użytkownika o nazwie `myUsername`, na maszyn wirtualnych, o nazwie `myVM` w `myResourceGroup`. Użyj własnego wartości w następujący sposób:

```bash
azure vm reset-access --resource-group myResourceGroup --name myVM \
    --username myUsername --ssh-key-file ~/.ssh/azure_id_rsa.pub
```


## <a name="using-the-vmaccess-extension"></a>Przy użyciu rozszerzenia VMAccess
Rozszerzenie programu Access maszyn wirtualnych Linux odczytuje w pliku json, który określa akcje do wykonania. Te akcje dołączanie Resetowanie SSHD, resetowanie klucza SSH i dodawania użytkownika. Polecenie Azure nadal używać do połączeń z rozszerzeniem VMAccess, ale można ponownie użyć plików json u wielu maszyny wirtualne w razie potrzeby. Ta metoda pozwala na tworzenie repozytorium json plików, które można następnie wywołać dla określonych scenariuszach.

### <a name="reset-sshd"></a>Resetuj SSHD
Tworzenie pliku o nazwie `PrivateConf.json` o następującej treści:

```bash
{  
    "reset_ssh":"True"
}
```

Za pomocą interfejsu wiersza polecenia Azure, następnie połączenia `VMAccessForLinux` rozszerzenia Resetowanie połączenia SSHD, określając plik json. Poniższy przykład resetuje SSHD na maszyn wirtualnych, o nazwie `myVM` w `myResourceGroup`. Użyj własnego wartości w następujący sposób:

```bash
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```

### <a name="reset-ssh-credentials-for-a-user"></a>Resetowanie poświadczeń SSH dla użytkownika
Jeśli SSHD wydaje się działać poprawnie, możesz zresetować poświadczenia dla użytkownika giver. Aby zresetować hasło użytkownika, należy utworzyć plik o nazwie `PrivateConf.json`. Poniższy przykład resetuje poświadczenia dla `myUsername` do wartości określonej w `myPassword`. Wprowadź następujące wiersze do swojego `PrivateConf.json` plik, używając własnych wartości:

```bash
{
    "username":"myUsername", "password":"myPassword"
}
```

Lub aby zresetować klucz SSH dla użytkownika, najpierw należy utworzyć plik o nazwie `PrivateConf.json`. Poniższy przykład resetuje poświadczenia dla `myUsername` do wartości określonej w `myPassword`, na maszyn wirtualnych, o nazwie `myVM` w `myResourceGroup`. Wprowadź następujące wiersze do swojego `PrivateConf.json` plik, używając własnych wartości:

```bash
{
    "username":"myUsername", "ssh_key":"mySSHKey"
}
```

Po utworzeniu pliku json, za pomocą interfejsu wiersza polecenia Azure połączeń `VMAccessForLinux` rozszerzenia resetowanie poświadczeń użytkownika SSH, określając plik json. Poniższy przykład resetuje poświadczenia na maszyn wirtualnych, o nazwie `myVM` w `myResourceGroup`. Użyj własnego wartości w następujący sposób:

```
azure vm extension set myResourceGroup myVM \
    VMAccessForLinux Microsoft.OSTCExtensions "1.2" \
    --private-config-path PrivateConf.json
```


## <a name="restart-a-vm"></a>Ponowne uruchamianie maszyn wirtualnych
Jeśli masz resetowanie poświadczeń SSH konfiguracji i użytkownika lub wystąpił błąd w ten sposób, można spróbować uruchomić maszyn wirtualnych adres źródłowego problemy z obliczeń.

### <a name="azure-portal"></a>Azure portal
Aby ponownie uruchomić maszyny za pomocą portalu Azure, wybierz swojego maszyn wirtualnych, a następnie kliknij przycisk * przycisk**Uruchom ponownie** , tak jak w poniższym przykładzie:

![Ponowne uruchamianie maszyn wirtualnych w portalu Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/restart-vm-using-portal.png)

### <a name="azure-cli"></a>Polecenie Azure
Poniższy przykład uruchomieniu maszyn wirtualnych, o nazwie `myVM` w grupie zasobów o nazwie `myResourceGroup`. Użyj własnego wartości w następujący sposób:

```bash
azure vm restart --resource-group myResourceGroup --name myVM
```


## <a name="redeploy-a-vm"></a>Ponownie wdróż maszyny
Można ponownie wdróż maszyn wirtualnych do innego węzła w Azure, która może rozwiązać problemów sieciowych źródłowych. Aby dowiedzieć się, jak ponowne rozmieszczanie maszyny zobacz [ponownie wdróż maszyny wirtualnej do nowego węzła Azure](virtual-machines-windows-redeploy-to-new-node.md).

> [AZURE.NOTE] Po zakończeniu tej operacji tymczasowych dane zostaną utracone i dynamiczne adresy IP, które są skojarzone z maszyny wirtualnej zostaną zaktualizowane.

### <a name="azure-portal"></a>Azure portal
Ponownie rozmieścić maszyny za pomocą portalu Azure, wybierz swój maszyn wirtualnych i przewiń w dół do sekcji **pomocy technicznej + rozwiązywanie problemów** . Kliknij przycisk **ponownie wdróż** , tak jak w poniższym przykładzie:

![Ponownie wdróż maszyn wirtualnych w portalu Azure](./media/virtual-machines-linux-troubleshoot-ssh-connection/redeploy-vm-using-portal.png)

### <a name="azure-cli"></a>Polecenie Azure
Poniższy przykład redeploys maszyn wirtualnych, o nazwie `myVM` w grupie zasobów o nazwie `myResourceGroup`. Użyj własnego wartości w następujący sposób:

```bash
azure vm redeploy --resource-group myResourceGroup --name myVM
```

## <a name="vms-created-by-using-the-classic-deployment-model"></a>Maszyny wirtualne utworzone przy użyciu modelu wdrożenia klasyczny

Spróbuj wykonać poniższe czynności, aby rozwiązać najbardziej typowe błędy połączenia SSH dla maszyny wirtualne, które zostały utworzone przy użyciu modelu Klasyczny wdrożenia. Po wykonaniu każdej czynności spróbuj ponownie połączyć się maszyn wirtualnych.

- Resetowanie dostępu zdalnego z [Azure portal](https://portal.azure.com). W portalu Azure Wybierz swojego maszyn wirtualnych i kliknij przycisk **Resetuj zdalnego...** .

- Uruchom ponownie maszyn wirtualnych. W [portalu Azure](https://portal.azure.com)zaznacz usługi maszyn wirtualnych, a następnie kliknij przycisk **Uruchom ponownie** .

    -LUB-

    W [portalu klasyczny Azure](https://manage.windowsazure.com)wybierz **maszyn wirtualnych** > **wystąpienia** > **Uruchom ponownie**.

- Ponownie wdróż maszyn wirtualnych do nowego węzła Azure. Aby dowiedzieć się, jak ponownie rozmieścić maszyny zobacz [ponownie wdróż maszyny wirtualnej do nowego węzła Azure](virtual-machines-windows-redeploy-to-new-node.md).

    Po zakończeniu tej operacji tymczasowych dane zostaną utracone i dynamiczne adresy IP, które są skojarzone z maszyny wirtualnej zostaną zaktualizowane.

- Postępuj zgodnie z instrukcjami w temacie [Jak zresetować hasło lub SSH dla maszyn wirtualnych systemem Linux](virtual-machines-linux-classic-reset-access.md) :
    - Resetowanie hasła lub klucza SSH.
    - Utwórz konto użytkownika _sudo_ .
    - Zresetuj konfigurację SSH.

- Sprawdzanie kondycji zasobu maszyn wirtualnych na temat wszelkich problemów platformy.<br>
  Wybierz swój maszyn wirtualnych i przewiń w dół **Ustawienia** > **Sprawdzanie kondycji**.


## <a name="additional-resources"></a>Dodatkowe zasoby

- Jeśli nadal nie możesz SSH do swojego maszyn wirtualnych po wykonaniu po czynności, zobacz [szczegółowe kroki rozwiązywania problemów](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md) przejrzeć dodatkowe kroki w celu rozwiązania problemu.

- Aby uzyskać więcej informacji na temat rozwiązywania problemów z dostęp do aplikacji zobacz [Rozwiązywanie problemów z dostępem do uruchamiania aplikacji na Azure maszyn wirtualnych](virtual-machines-linux-troubleshoot-app-connection.md)

- Aby uzyskać więcej informacji na temat rozwiązywania problemów z maszyn wirtualnych, które zostały utworzone przy użyciu modelu Klasyczny wdrożenia zobacz [Resetowanie hasła lub SSH dla maszyn wirtualnych z systemem Linux](virtual-machines-linux-classic-reset-access.md).
