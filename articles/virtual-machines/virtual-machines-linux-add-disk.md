<properties
    pageTitle="Dodanie dysku do maszyn wirtualnych Linux | Microsoft Azure"
    description="Dowiedz się, jak dodać trwałych dysk do maszyn wirtualnych usługi Linux"
    keywords="maszyn wirtualnych Linux, Dodaj dysk zasobu"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.topic="article"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="add-a-disk-to-a-linux-vm"></a>Dodanie dysku do maszyny Linux

W tym artykule przedstawiono sposób dołączyć dysku trwałych do swojego maszyn wirtualnych co można zachować dane — nawet wtedy, gdy usługi maszyn wirtualnych jest reprovisioned z powodu konserwacji lub zmiany rozmiaru. Aby dodać dysk, musi być [Polecenie Azure](../xplat-cli-install.md) skonfigurowana w trybie Menedżera zasobów (`azure config mode arm`).  

## <a name="quick-commands"></a>Szybkie polecenia

W poniższych przykładach polecenie Zamień wartości między &lt; i &gt; z wartościami z własnym środowisku.

```bash
azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>
```

## <a name="attach-a-disk"></a>Dołączanie dyskiem

Dołączanie nowego dysku jest szybkie. Typ `azure vm disk attach-new <myuniquegroupname> <myuniquevmname> <size-in-GB>` do tworzenia i dodany dysk GB dla swojego maszyn wirtualnych. Jeśli konto miejsca do magazynowania nie jednoznacznie zidentyfikować, dowolny utworzony dysk zostanie umieszczony w tego samego konta miejsca do magazynowania, miejsce, w którym znajduje się na dysku z systemem operacyjnym.  Jego powinien wyglądać następująco:

```bash
azure vm disk attach-new myuniquegroupname myuniquevmname 5
```

Wynik

```bash
info:    Executing command vm disk attach-new
+ Looking up the VM "myuniquevmname"
info:    New data disk location: https://cliexxx.blob.core.windows.net/vhds/myuniquevmname-20150526-0xxxxxxx43.vhd
+ Updating VM "myuniquevmname"
info:    vm disk attach-new command OK
```

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Nawiązywanie połączenia z maszyn wirtualnych Linux zainstalowanie nowego dysku

> [AZURE.NOTE] W tym temacie łączy maszyny przy użyciu nazwy użytkownika i hasła. Aby użyć pary kluczy publicznych i prywatnych na komunikowanie się z maszyn wirtualnych, zobacz, [jak używać SSH z systemem Linux Azure](virtual-machines-linux-mac-create-ssh-keys.md). Można modyfikować połączenia między **SSH** maszyny wirtualne utworzone za pomocą `azure vm quick-create` polecenia za pomocą `azure vm reset-access` polecenie, aby całkowicie Resetowanie **SSH** dostęp, dodawanie lub usuwanie użytkowników i dodawanie publicznej klucza plików z bezpiecznego dostępu.

Możesz potrzebne do SSH do swojego maszyn wirtualnych Azure partycją, formatowanie i instalacji nowego dysku, aby maszyn wirtualnych usługi Linux można jej używać. Jeśli nie znasz z połączenia z **ssh**, polecenie ma postać `ssh <username>@<FQDNofAzureVM> -p <the ssh port>`i wygląda następująco:

```bash
ssh ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com -p 22
```

Wynik

```bash
The authenticity of host 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com (191.239.51.1)' can't be established.
ECDSA key fingerprint is bx:xx:xx:xx:xx:xx:xx:xx:xx:x:x:x:x:x:x:xx.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'myuni-westu-1432328437727-pip.westus.cloudapp.azure.com,191.239.51.1' (ECDSA) to the list of known hosts.
ops@myuni-westu-1432328437727-pip.westus.cloudapp.azure.com's password:
Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-37-generic x86_64)

* Documentation:  https://help.ubuntu.com/

System information as of Fri May 22 21:02:32 UTC 2015

System load: 0.37              Memory usage: 2%   Processes:       207
Usage of /:  41.4% of 1.94GB   Swap usage:   0%   Users logged in: 0

Graph this data and manage this system at:
  https://landscape.canonical.com/

Get cloud support with Ubuntu Advantage Cloud Guest:
  http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

ops@myuniquevmname:~$
```

Teraz, gdy masz połączenie z maszyn wirtualnych, możesz dołączyć dyskiem.  Najpierw Znajdź dysku, za pomocą `dmesg | grep SCSI` (metody do znalezienia nowego dysku mogą się różnić). W tym przypadku wygląda mniej więcej tak:

```bash
dmesg | grep SCSI
```

Wynik

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

w przypadku w tym temacie `sdc` dysk jest, w którym chcemy. Teraz partycje na dysku `sudo fdisk /dev/sdc` — przy założeniu, że w Twoim przypadku dysk był `sdc`, udostępnij go podstawowy dysk na partycją 1 i Zaakceptuj inne ustawienia domyślne.

```bash
sudo fdisk /dev/sdc
```

Wynik

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Tworzenie partycją, wpisując `p` w wierszu:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

I zapisze partycją systemu plików za pomocą polecenia **mkfs** określenie typu systemu plików i nazwę urządzenia. W tym temacie użyto programu `ext4` i `/dev/sdc1` z góry:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Wynik

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

Teraz możemy utworzyć katalog ma zostać zainstalowany za pomocą systemu plików `mkdir`:

```bash
sudo mkdir /datadrive
```

I zainstalować, przy użyciu katalogu `mount`:

```bash
sudo mount /dev/sdc1 /datadrive
```

Dysku danych jest teraz gotowy do użycia jako `/datadrive`.

```bash
ls
```

Wynik

```bash
bin   datadrive  etc   initrd.img  lib64       media  opt   root  sbin  sys  usr  vmlinuz
boot  dev        home  lib         lost+found  mnt    proc  run   srv   tmp  var
```

Aby upewnić się, że dysk jest ponownej instalacji automatycznie po ponownym uruchomieniu musi zostać dodana do pliku fstab/itp. Ponadto, zaleca się że UUID (powszechnie Unikatowy identyfikator) jest używana w/itp/fstab dotyczyć dysku, a nie tylko nazwę urządzenia (takie jak `/dev/sdc1`). Jeśli system operacyjny wykryje błąd dysku podczas uruchamiania, przy użyciu UUID pozwala uniknąć niepoprawne dysk jest zainstalowany w określonej lokalizacji. Pozostała dyski danych będzie można przyznać te same identyfikatory urządzenia. Aby znaleźć UUID nowego dysku, użyj narzędzia **blkid** :

```bash
sudo -i blkid
```

Wynik będzie podobny do następującego:

```bash
/dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
/dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

>[AZURE.NOTE] Nieprawidłowo edytowania pliku **/etc/fstab** może spowodować w systemie będzie niemożliwy. Jeśli nie wiesz, zapoznaj się z dokumentacją rozkład Aby uzyskać informacje dotyczące prawidłowo edytować ten plik. Zalecane jest również, utworzenia kopii zapasowej pliku /etc/fstab przed edycją.

Następnie otwórz plik **/etc/fstab** w edytorze tekstów:

```bash
sudo vi /etc/fstab
```

W tym przykładzie korzystamy wartość UUID dla nowego urządzenia **/dev/sdc1** , który został utworzony w poprzednich krokach i punktu instalacji **/datadrive**. Dodaj następujący wiersz na końcu pliku **/etc/fstab** :

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

>[AZURE.NOTE] Później usuwanie dysku danych bez możliwości edycji fstab może spowodować maszyn wirtualnych niepowodzenie do uruchamiania. Podać większości dystrybucji `nofail` i/lub `nobootwait` fstab opcje. Te opcje Zezwalaj system został uruchomiony, nawet w przypadku awarii dysku do zainstalowania w czasie uruchamiania. W dokumentacji usługi dystrybucji więcej informacji na temat tych parametrów.

>[AZURE.NOTE] Opcja **nofail** zapewnia, że maszyn wirtualnych ma być uruchamiany nawet wtedy, gdy system plików jest uszkodzony lub dysk nie istnieje w czasie uruchamiania. Bez tej opcji mogą wystąpić zachowanie, zgodnie z opisem w [Nie SSH do maszyn wirtualnych Linux z powodu błędów FSTAB](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)


### <a name="trimunmap-support-for-linux-in-azure"></a>PRZYCINANIE i UNMAP obsługę Linux platformy Azure
Niektóre jądra Linux obsługuje operacje PRZYCINANIE i UNMAP, aby odrzucić nieużywanych bloków na dysku. To przede wszystkim w magazynie standardowy, aby poinformować Azure usuniętego stron nie są już prawidłowe i można odrzucić. Można zapisać kosztu, jeśli Tworzenie dużych plików, a następnie usuń je.

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

- Pamiętaj, że nowego dysku nie jest dostępna dla maszyn wirtualnych, jeśli go ponownie, chyba że zapisać te informacje do pliku [fstab](http://en.wikipedia.org/wiki/Fstab) .
- Aby upewnić się, że maszyn wirtualnych usługi Linux jest poprawnie skonfigurowany, przejrzyj zalecenia [Optymalizowanie wydajności komputera Linux](virtual-machines-linux-optimization.md) .
- Rozwijanie możliwości miejsca do magazynowania, dodając dodatkowe dyski i [Konfigurowanie RAID](virtual-machines-linux-configure-raid.md) dodatkowe.
