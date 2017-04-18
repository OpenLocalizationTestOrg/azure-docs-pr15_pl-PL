<properties
    pageTitle="Dołączanie dysku do maszyny Linux | Microsoft Azure"
    description="Dowiedz się, jak dołączyć dyskiem danych Azure maszyn wirtualnych systemem Linux i go zainicjować, dzięki czemu jest gotowa do użycia."
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
    ms.date="08/23/2016"
    ms.author="iainfou"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Jak dołączyć dysku danych maszyn wirtualnych Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zobacz Dołączanie [dysku danych przy użyciu modelu wdrożenia Menedżera zasobów](virtual-machines-linux-add-disk.md).

Można dołączyć zarówno puste dyski i które zawierają dane, aby maszyny wirtualne usługi Azure. Obydwa rodzaje dysków to pliki VHD, które znajdują się na koncie Azure miejsca do magazynowania. Jako z dodawaniem dowolny dysk do komputera Linux po dołączeniu dysk należy zainicjować i sformatuj ją, aby jest gotowa do użycia. Dołączanie zarówno puste dyski i już zawierający dane do usługi maszyny wirtualne, a także jak następnie zainicjować i sformatować nowy dysk szczegółów tego artykułu.

> [AZURE.NOTE]Jest najlepszym rozwiązaniem jest użycie jednego lub kilku osobnych dyskach do przechowywania danych maszyny wirtualnej. Po utworzeniu Azure maszyn wirtualnych ma dysku systemu operacyjnego i tymczasowe. **Nie należy używać dysku tymczasowym do przechowywania trwałych danych.** Jak wskazuje nazwa, zawiera tylko tymczasowego przechowywania danych. Zapewnia on nie nadmiarowości lub kopii zapasowej ponieważ go nie znajdują się w magazynie Azure.
> Tymczasowe dysk jest zwykle zarządzane przez agenta Linux Azure i instalowane automatycznie do **/mnt/resource** (lub **katalogu/mnt** na obrazach Ubuntu). Z drugiej strony, dysk danych może nosić nazwę przez jądro Linux podobną do `/dev/sdc`, i chcesz podzielić, formatowanie i zainstalować tego zasobu. W [Podręczniku użytkownika agenta Linux Azure] [ Agent] Aby uzyskać szczegółowe informacje.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Inicjowania dysku danych w Linux

1. SSH do swojego maszyn wirtualnych. Aby uzyskać szczegółowe informacje, zobacz, [jak zalogować się do maszyny wirtualnej systemem Linux][Logon].

2. Następnie należy znaleźć identyfikator urządzenia dla dysku danych zainicjować. Istnieją dwa sposoby to zrobić:

    ) Grep dla urządzeń SCSI w dziennikach, takie jak następujące polecenie:

            $sudo grep SCSI /var/log/messages

    Dla ostatniej wypłaty Ubuntu, może być konieczne używanie `sudo grep SCSI /var/log/syslog` ponieważ rejestrowanie `/var/log/messages` może być domyślnie wyłączone.

    Można znaleźć identyfikator ostatni dysk danych, który został dodany do wiadomości, które są wyświetlane.

    ![Pobieranie wiadomości dysku](./media/virtual-machines-linux-classic-attach-disk/scsidisklog.png)

    LUB

    b) użyj `lsscsi` polecenie, aby dowiedzieć się, identyfikator urządzenia. `lsscsi`można zainstalować przez jeden `yum install lsscsi` (czerwony funkcję podstawą dystrybucji) lub `apt-get install lsscsi` (na Debian podstawie dystrybucji). Można znaleźć dysk, na którym szukasz przez jego _lun_ lub **liczbę jednostek**. Na przykład numer _lun_ dysków dołączysz można łatwo zobaczyć `azure vm disk list <virtual-machine>` jako:

            ~$ azure vm disk list TestVM
            info:    Executing command vm disk list
            + Fetching disk images
            + Getting virtual machines
            + Getting VM disks
            data:    Lun  Size(GB)  Blob-Name                         OS
            data:    ---  --------  --------------------------------  -----
            data:         30        TestVM-2645b8030676c8f8.vhd  Linux
            data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
            info:    vm disk list command OK

    Porównanie danych z danych wyjściowych `lsscsi` dla tych samych próbkach maszyn wirtualnych:

            ops@TestVM:~$ lsscsi
            [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
            [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
            [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
            [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc

    Numer ostatniej krotki każdy wiersz jest _lun_. Zobacz `man lsscsi` uzyskać więcej informacji.

3. W wierszu polecenia wpisz następujące polecenie, aby utworzyć urządzenia:

        $sudo fdisk /dev/sdc


4. Po wyświetleniu monitu wpisz **n** , aby utworzyć nowy partycją.


    ![Tworzenie urządzenia](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartition.png)

5. Po wyświetleniu monitu wpisz **p** , aby wprowadzić partycją partycją podstawową. Wpisz wartość **1** , aby była pierwszą partycją, a następnie wprowadź typ zaakceptuj wartości domyślnej dla Wykres walcowy. W niektórych systemach go wyświetlić wartości domyślnych pierwszego i ostatniego sektorów, zamiast Wykres walcowy. Możesz zaakceptować te ustawienia domyślne.


    ![Tworzenie partition](./media/virtual-machines-linux-classic-attach-disk/fdisknewpartdetails.png)



6. Wpisz **p** , aby wyświetlić szczegółowe informacje o dysku, na którym jest podejmowana podzielona.


    ![Informacje o dysku listy](./media/virtual-machines-linux-classic-attach-disk/fdiskpartitiondetails.png)



7. Wpisz **w** , aby zapisać ustawienia dysku.


    ![Zapisanie zmian dysku](./media/virtual-machines-linux-classic-attach-disk/fdiskwritedisk.png)

8. Teraz można utworzyć system plików na nowo. Dołącz numer identyfikatora urządzenia (w tym przykładzie `/dev/sdc1`). Poniższy przykład tworzy partycją ext4 /dev/sdc1:

        # sudo mkfs -t ext4 /dev/sdc1

    ![Tworzenie plików systemu Windows](./media/virtual-machines-linux-classic-attach-disk/mkfsext4.png)

    >[AZURE.NOTE] SuSE Linux Enterprise 11 tylko systemy dostęp tylko do odczytu dla systemów plik ext4. Dla tych systemów zaleca się nowy system plików w formacie ext3 zamiast ext4.


9. Utwórz katalog ma zostać zainstalowany nowy system plików w następujący sposób:

        # sudo mkdir /datadrive


10. Na koniec dysk, można zainstalować w następujący sposób:

        # sudo mount /dev/sdc1 /datadrive

    Dysk danych jest teraz gotowy do użycia jako **/datadrive**.
    
    ![Tworzenie katalogu i zainstalowanie dysku](./media/virtual-machines-linux-classic-attach-disk/mkdirandmount.png)


11. Dodawanie nowego dysku do /etc/fstab:

    Aby upewnić się, że dysk jest ponownej instalacji automatycznie po ponownym uruchomieniu musi zostać dodana do pliku fstab/itp. Ponadto zdecydowanie zalecane jest, że UUID (powszechnie Unikatowy identyfikator) jest używana w /etc/fstab, aby odwołać się do stacji dysków, a nie tylko nazwę urządzenia (to znaczy /dev/sdc1). Za pomocą UUID pozwala uniknąć niepoprawne dysk jest zainstalowany w określonej lokalizacji, jeśli system operacyjny wykryje błąd dysku podczas uruchamiania i wszystkie pozostałe dyski danych jest przypisywana identyfikatory tych urządzeń. Aby znaleźć UUID nowego dysku, można użyć narzędzia **blkid** :

        # sudo -i blkid

    Wynik będzie podobny do następującego:

        /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
        /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
        /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"


    >[AZURE.NOTE] Nieprawidłowo edytowania pliku **/etc/fstab** może spowodować w systemie będzie niemożliwy. Jeśli nie wiesz, zapoznaj się z dokumentacją rozkład Aby uzyskać informacje dotyczące prawidłowo edytować ten plik. Zalecane jest również, utworzenia kopii zapasowej pliku /etc/fstab przed edycją.

    Następnie otwórz plik **/etc/fstab** w edytorze tekstów:

        # sudo vi /etc/fstab

    W tym przykładzie korzystamy wartość UUID dla nowego urządzenia **/dev/sdc1** , który został utworzony w poprzednich krokach i punktu instalacji **/datadrive**. Dodaj następujący wiersz na końcu pliku **/etc/fstab** :

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2

    Lub w systemach według SuSE Linux może być konieczne użyć nieco innego formatu:

        /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    
    >[AZURE.NOTE] `nofail` Opcja zapewnia, że maszyn wirtualnych ma być uruchamiany nawet wtedy, gdy system plików jest uszkodzony lub dysk nie istnieje w czasie uruchamiania. Bez tej opcji mogą wystąpić zachowanie, zgodnie z opisem w [Nie SSH do maszyn wirtualnych Linux z powodu błędów FSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Teraz można sprawdzić, czy system plików jest zainstalowany poprawnie, odinstalowywanie, a następnie remounting systemu plików, to znaczy w tym przykładzie punkt instalacji `/datadrive` utworzony w poprzednich krokach:

        # sudo umount /datadrive
        # sudo mount /datadrive

    Jeśli `mount` polecenie tworzy komunikat o błędzie, sprawdź/itp/fstab w pliku o poprawnej składni. Jeśli utworzono dodatkowe dane dyski lub partycje, wprowadź je w polu/itp/fstab także oddzielnie.

    Wprowadź zapisywalny dysk za pomocą tego polecenia:

        # sudo chmod go+w /datadrive

>[AZURE.NOTE] Następnie usuwanie dysku danych bez możliwości edycji fstab może spowodować maszyn wirtualnych niepowodzenie do uruchamiania. Jeśli jest to wystąpienie typowych większości dystrybucji zapewniają `nofail` i/lub `nobootwait` fstab opcji, które system może uruchomić nawet w przypadku awarii dysku do zainstalowania na uruchamiania czasu. W dokumentacji usługi dystrybucji więcej informacji na temat tych parametrów.

### <a name="trimunmap-support-for-linux-in-azure"></a>PRZYCINANIE i UNMAP obsługę Linux platformy Azure
Niektóre jądra Linux obsługuje operacje PRZYCINANIE i UNMAP, aby odrzucić nieużywanych bloków na dysku. Operacje te są przydatne przede wszystkim w magazynie standardowy, aby poinformować Azure usuniętego stron nie są już prawidłowe i można odrzucić. Odrzucanie strony można zapisać kosztu, jeśli Tworzenie dużych plików, a następnie usuń je.

Istnieją dwa sposoby na umożliwienie PRZYCINANIE pomocy technicznej w Twojej maszyn wirtualnych Linux. Jak zwykle można znaleźć w sieci dystrybucyjnej zalecane podejście:

- Używanie `discard` zainstalować opcji w `/etc/fstab`, na przykład:

        UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2

- Ponadto można uruchamiać `fstrim` polecenia ręcznie z poziomu wiersza polecenia lub Dodaj go do swojej crontab regularnego uruchamiania:

    **Ubuntu**

        # sudo apt-get install util-linux
        # sudo fstrim /datadrive

    **RHEL-CentOS**

        # sudo yum install util-linux
        # sudo fstrim /datadrive

## <a name="troubleshooting"></a>Rozwiązywanie problemów
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]


## <a name="next-steps"></a>Następne kroki
Więcej o korzystaniu z maszyn wirtualnych Linux w następujących artykułach:

- [Jak zalogować się do maszyny wirtualnej systemem Linux][Logon]

- [Jak odłączyć dysk z maszyny wirtualnej Linux](virtual-machines-linux-classic-detach-disk.md)

- [Polecenie Azure za pomocą modelu wdrożenia klasyczny](../virtual-machines-command-line-tools.md)

<!--Link references-->
[Agent]: virtual-machines-linux-agent-user-guide.md
[Logon]: virtual-machines-linux-mac-create-ssh-keys.md
