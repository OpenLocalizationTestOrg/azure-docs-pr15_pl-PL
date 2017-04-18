<properties
    pageTitle="Przechwycony obraz maszyny Linux | Microsoft Azure"
    description="Dowiedz się, jak przechwycić obraz systemem Linux Azure maszyny wirtualnej (maszyn wirtualnych) utworzone za pomocą modelu Klasyczny wdrożenia."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="iainfou"/>


# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Jak przechwytywać klasyczny maszyny wirtualnej Linux jako obraz

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](virtual-machines-linux-capture-image.md).

W tym artykule pokazano, jak przechwycić klasyczny Azure maszyny wirtualnej systemem Linux jako obraz, aby utworzyć inne maszyn wirtualnych. Ten obraz zawiera dysku systemu operacyjnego i dyski danych dołączone do maszyny wirtualnej. Nie zawiera Konfiguracja sieciowa, więc musisz skonfigurować, który podczas tworzenia maszyn wirtualnych z obrazu.

Azure przechowuje obraz w obszarze **obrazów**oraz wszystkie obrazy, które zostały przekazane. Aby uzyskać więcej informacji o obrazach zobacz [Temat obrazów maszyn wirtualnych platformy Azure] [].

## <a name="before-you-begin"></a>Przed rozpoczęciem

W tej procedurze założono, że już utworzone Azure maszyn wirtualnych przy użyciu modelu wdrożenia klasyczny i skonfigurowany w systemie operacyjnym, w tym dołączanie wszelkie dyski danych. Jeśli chcesz utworzyć maszyny, przeczytaj [sposobu tworzenia maszyny wirtualnej Linux] [].


## <a name="capture-the-virtual-machine"></a>Przechwytywanie maszyny wirtualnej

1. [Łączenie maszyn wirtualnych](virtual-machines-linux-mac-create-ssh-keys.md) za pomocą klienta SSH wybranych przez użytkownika.

2. W oknie SSH wpisz następujące polecenie. Wynikiem `waagent` mogą się nieco różnić w zależności od wersji tego narzędzia:

    `sudo waagent -deprovision+user`

    To polecenie próbuje oczyścić system i wprowadź odpowiednie dla reprovisioning. Operacja wykonuje następujące zadania:

    - Usuwa SSH hosta klawiszy (jeśli Provisioning.RegenerateSshHostKeyPair jest "y" w pliku konfiguracji)
    - Pozwala wyczyścić konfiguracji serwera nazw w /etc/resolv.conf
    - Usuwa `root` hasła użytkownika z/itp/cień (jeśli Provisioning.DeleteRootPassword jest "y" w pliku konfiguracji)
    - Usuwa przechowywanych w pamięci podręcznej dzierżawy klientów DHCP
    - Nazwa hosta resetuje do localhost.localdomain
    - Usuwa ostatnią ustanawianie użytkownika konta (uzyskanego z /var/lib/waagent) **i skojarzonych danych**.

    >[AZURE.NOTE] Cofanie ubezpieczeń usunie pliki i dane do "uogólnić" obraz. To polecenie można uruchamiać tylko na komputerze wirtualnych, który chcesz przechwycić jako nowy szablon obrazu. Nie gwarantuje, że obraz jest wyczyszczone wszystkich informacji poufnych lub nadaje się do rozpowszechniania stronom trzecim.


3. Wpisz **y** , aby kontynuować. Możesz dodać `-force` parametr, aby uniknąć tego kroku potwierdzenia.

4. Wpisz **Zakończ** , aby zamknąć klienta SSH.

    >[AZURE.NOTE] Pozostałe kroki założono, że masz już [zainstalowany Azure interfejsu wiersza polecenia](../xplat-cli-install.md) na komputerze klienckim użytkownika. Również można wykonywać następujące czynności w [portal Azure klasyczny] [].

5. Na komputerze klienckim otwórz polecenie Azure i logowanie do subskrypcji usługi Azure. Aby uzyskać szczegółowe informacje przeczytaj [Nawiązywanie połączenia z Azure subskrypcję polecenie Azure](../xplat-cli-connect.md).

6. Upewnij się, że jesteś w trybie Zarządzanie usługą:

    `azure config mode asm`

7. Zamknij maszyny wirtualnej, która jest już wstrzymano obsługę administracyjną w poprzednie kroki:

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] Można znaleźć w maszyn wirtualnych utworzone w ramach subskrypcji przy użyciu`azure vm list`

8. Po zatrzymaniu maszyny wirtualnej przechwycić obraz przy użyciu polecenia:

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    Wpisz nazwę obrazu, który ma zamiast _nową nazwę obrazu_. To polecenie tworzy uogólniony obraz systemu operacyjnego. `-t` Polecenia usuwa oryginalną maszyny wirtualnej.

9.  Nowy obraz jest teraz dostępna na liście obrazy, których można używać w celu skonfigurowania wszelkie nowe maszyn wirtualnych. Można go wyświetlić przy użyciu polecenia:

    `azure vm image list`

    W [portalu klasyczny Azure] []jest wyświetlany na liście **obrazy** .

    ![Przechwytywanie obrazu pomyślnie](./media/virtual-machines-linux-classic-capture-image/VMCapturedImageAvailable.png)


## <a name="next-steps"></a>Następne kroki
Obraz jest gotowy do można używać do tworzenia maszyn wirtualnych. Możesz użyć polecenia polecenie Azure `azure vm create` i podaj nazwę obrazu został utworzony. Aby uzyskać szczegółowe informacje dotyczące polecenia, zobacz [za pomocą interfejsu wiersza polecenia Azure z modelu wdrożenia klasyczny](../virtual-machines-command-line-tools.md) . Możesz również utworzyć niestandardowe maszyny wirtualnej przy użyciu metody **Z galerii** i wybierając opcję obraz, który został utworzony za pomocą [portal Azure klasyczny] [] . Aby uzyskać więcej informacji, zobacz [jak tworzyć niestandardowe maszyny wirtualnej] [] .

**Zobacz też:** [Przewodnik użytkownika Azure agenta Linux](virtual-machines-linux-agent-user-guide.md)

[Portal Azure klasyczny]: http://manage.windowsazure.com
[Informacje o obrazach maszyn wirtualnych platformy Azure]: virtual-machines-linux-classic-about-images.md
[Jak utworzyć niestandardowe maszyny wirtualnej]: virtual-machines-linux-classic-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: virtual-machines-windows-classic-attach-disk.md
[Jak utworzyć maszyny wirtualnej Linux]: virtual-machines-linux-classic-create-custom.md
