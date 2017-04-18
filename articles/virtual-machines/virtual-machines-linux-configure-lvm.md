<properties 
    pageTitle="Konfigurowanie LVM na komputerze wirtualnych systemem Linux | Microsoft Azure" 
    description="Dowiedz się, jak skonfigurować LVM na Linux platformy Azure." 
    services="virtual-machines-linux" 
    documentationCenter="na" 
    authors="szarkos"  
    manager="timlt" 
    editor="tysonn"
    tag="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="szark"/>


# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Konfigurowanie LVM na maszyny Linux platformy Azure

Ten dokument przedstawimy sposobu konfigurowania Menedżer głośności logiczne (LVM) na komputerze wirtualnych Azure. Jest to możliwe, aby skonfigurować LVM na dowolnym dysku maszyn wirtualnych, domyślnie większość obrazów chmury nie będzie miał LVM skonfigurowane na dysku systemu operacyjnego. To zapobieganie problemom z grupami zduplikowanych głośność, jeśli dysku OS kiedykolwiek dołączony do innego maszyn wirtualnych tego samego rozkładu i typ, to znaczy w scenariuszu odzyskiwania. W związku z tym zaleca się tylko na dyskach danych za pomocą LVM.


## <a name="linear-vs-striped-logical-volumes"></a>Liniowy a rozłożone wielkości logicznych.

LVM umożliwia łączenie liczbę dysków fizycznych w głośność jednego miejsca do magazynowania. Domyślnie LVM zazwyczaj utworzy liniowej logiczne wielkości, co oznacza, że łączone magazynu fizycznego. W tym przypadku operacji odczytu/zapisu będą zazwyczaj tylko wysyłane do jednego dysku. Możemy również utworzyć rozłożone wielkości logiczne miejsce, w którym Odczyt i zapis są rozdzielane wiele dysków znajdujących się w grupie zbiorcza (to znaczy podobnie RAID0). Ze względu na wydajność prawdopodobne jest ma paskowych usługi logiczne wielkości tak, aby odczyt i zapis są używane na dyskach załączonych danych.

Tym dokumencie opisano, jak łączenie kilku dyskach danych w grupie pojedynczy głośności, a następnie utwórz rozkładania woluminu logiczne. Poniższe kroki są nieco uogólniony do pracy z większości dystrybucji. W większości przypadków narzędzia i przepływy pracy do zarządzania LVM Azure nie istotnie różnią się od innych środowisk. W zwykły sposób należy również zapoznać się z dostawcą Linux dokumentacji i najważniejsze wskazówki dotyczące korzystania z usługi określonego rozkładu LVM.


## <a name="attaching-data-disks"></a>Dołączanie dyski danych
Jeden będzie zwykle mają być uruchamiane z dwóch lub więcej dysków pusty danych przy użyciu LVM. W zależności od potrzeb Jo, możesz dołączyć dyski, które są przechowywane w naszym standardowego magazynu z maksymalnie 500 ei ps na dysku lub nasze magazynowania Premium z maksymalnie 5000 ei ps na dysku. W tym artykule nie przejdzie do szczegółów na temat obsługi administracyjnej i dołączyć dyski danych maszyn wirtualnych Linux. Zobacz Microsoft Azure artykuł [dołączyć dysku](virtual-machines-linux-add-disk.md) Aby uzyskać szczegółowe instrukcje dotyczące Dołącz dysk puste dane do maszyny wirtualnej Linux Azure.

## <a name="install-the-lvm-utilities"></a>Instalowanie narzędzia LVM

- **Ubuntu**

        # sudo apt-get update
        # sudo apt-get install lvm2

- **RHEL, CentOS i Oracle Linux**

        # sudo yum install lvm2

- **SLES 12 i openSUSE**

        # sudo zypper install lvm2

- **SLES 11**

        # sudo zypper install lvm2

    Na SLES11 należy również edytować /etc/sysconfig/lvm i skonfigurować `LVM_ACTIVATED_ON_DISCOVERED` umożliwiający "":

        LVM_ACTIVATED_ON_DISCOVERED="enable" 


## <a name="configure-lvm"></a>Konfigurowanie LVM
W tym przewodniku Przyjmijmy dołączono trzy dyski danych, które będą określane jako `/dev/sdc`, `/dev/sdd` i `/dev/sde`. Należy zauważyć, że te mogą nie być takich samych nazwach ścieżki w swojej maszyn wirtualnych. Można uruchamiać "`sudo fdisk -l`" lub podobne polecenia, aby wyświetlić listę dostępnych dysków.

1. Przygotowywanie wielkość fizycznej:

        # sudo pvcreate /dev/sd[cde]
          Physical volume "/dev/sdc" successfully created
          Physical volume "/dev/sdd" successfully created
          Physical volume "/dev/sde" successfully created


2.  Tworzenie grupy głośność. W tym przykładzie wywołania głośność grupy "danych — vg01":

        # sudo vgcreate data-vg01 /dev/sd[cde]
          Volume group "data-vg01" successfully created


3. Tworzenie woluminów logicznych. Polecenie poniżej możemy utworzy pojedynczy woluminu logicznego o nazwie "danych — lv01" zakresu grupy całego woluminu, ale należy pamiętać, że również można utworzyć przypisaną logicznych w tej grupie głośność.

        # sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
          Logical volume "data-lv01" created.


4. Formatowanie woluminu logicznego

        # sudo mkfs -t ext4 /dev/data-vg01/data-lv01

  >[AZURE.NOTE] Przy użyciu SLES11 "-t ext3" zamiast ext4. SLES11 obsługuje tylko dostęp do ext4 filesystems tylko do odczytu.


## <a name="add-the-new-file-system-to-etcfstab"></a>Dodawanie nowego systemu plików do /etc/fstab

**Uwaga:** Nieprawidłowo edytowania pliku /etc/fstab może spowodować w systemie będzie niemożliwy. Jeśli nie wiesz, sprawdź zapoznaj się z dokumentacją rozkład Aby uzyskać informacje dotyczące prawidłowo edytować ten plik. Zalecane jest również, utworzenia kopii zapasowej pliku /etc/fstab przed edycją.

1. Tworzenie punktu instalacji odpowiednie dla nowego systemu plików, na przykład:

        # sudo mkdir /data


2. Znajdź Ścieżka woluminu logicznych.

        # lvdisplay
        --- Logical volume ---
        LV Path                /dev/data-vg01/data-lv01
        ....


3. Otwórz /etc/fstab w edytorze tekstów i Dodaj wpis dla nowego systemu plików, na przykład:

        /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2

    Następnie zapisz i zamknij /etc/fstab.


4. Sprawdź, czy wpis fstab/itp/jest poprawny:

        # sudo mount -a

    Jeśli to polecenie nie powoduje komunikat o błędzie Sprawdź składnię w pliku fstab/itp.

    Następnie uruchom `mount` polecenie, aby upewnić się, jest zainstalowany system plików:

        # mount
        ......
        /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)


5. (Opcjonalnie) Odporność parametrów uruchamiania w /etc/fstab

    Zawiera wiele rozkład `nobootwait` lub `nofail` zainstalować parametry, które można dodać do pliku fstab/itp. Tych parametrów umożliwiający błędów podczas instalowania w określonym systemie plików i zezwolić na systemie Linux na kontynuowanie uruchamiania, nawet jeśli nie można poprawnie zainstalować system plików RAID. Zapoznaj się z dokumentacją do dystrybucji, aby uzyskać więcej informacji na temat tych parametrów.

    Przykład (Ubuntu):

        /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
