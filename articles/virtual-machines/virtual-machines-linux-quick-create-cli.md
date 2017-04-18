<properties
   pageTitle="Tworzenie maszyny Linux Azure za pomocą interfejsu wiersza polecenia | Microsoft Azure"
   description="Tworzenie maszyny Linux Azure za pomocą interfejsu wiersza polecenia."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="v-livech"/>


# <a name="create-a-linux-vm-on-azure-by-using-the-cli"></a>Tworzenie maszyny Linux Azure za pomocą interfejsu wiersza polecenia

W tym artykule pokazano, jak szybko wdrażania maszyn wirtualnych Linux (maszyn wirtualnych) w Azure za pomocą `azure vm quick-create` w Azure interfejs wiersza polecenia (polecenie). `quick-create` Polecenia wdraża maszyn wirtualnych wewnątrz podstawowe, bezpiecznej infrastruktury można za pomocą prototypu lub szybko przetestować koncepcji. Wymaga tego artykułu:

- Konto Azure ([Pobierz bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/)).

- [Polecenie Azure](../xplat-cli-install.md) podczas logowania `azure login`.

- Polecenie Azure _muszą znajdować się w_ Menedżera zasobów Azure tryb `azure config mode arm`.

Za pomocą [Azure portal](virtual-machines-linux-quick-create-portal.md), można szybko wdrożyć maszyny Linux.

## <a name="quick-commands"></a>Szybkie polecenia

W poniższym przykładzie pokazano, jak wdrażać maszyny CoreOS i dołączanie klucza Secure Shell (SSH) (jego argumenty mogą być różne):

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Uzyskać szczegółowy opis

Następujące instruktaż zawiera maszyn wirtualnych UbuntuLTS wdrożonym, krok po kroku, z objaśnienia czynności każdego kroku.

## <a name="vm-quick-create-aliases"></a>Maszyn wirtualnych szybkie tworzenie aliasów

Szybkim sposobem wybierz rozkładu jest zamapowane najczęściej podziału OS aliasy polecenie Azure za pomocą. W poniższej tabeli wymieniono aliasy (stan polecenie Azure wersji 0,10). Wszystkie wdrożenia, które używają `quick-create` domyślne maszyny wirtualne, które kopii przez Magazyn dysków półprzewodnikowych (SSD), który oferuje dostęp do dysku szybciej obsługi administracyjnej i wysokiej wydajności. (Tych aliasów stanowi niewielką część dostępne dystrybucji Azure. Znajdź więcej obrazów w Azure Marketplace, [wyszukując obrazu w programie PowerShell](virtual-machines-linux-cli-ps-findimage.md), [w sieci web](https://azure.microsoft.com/marketplace/virtual-machines/)lub [Przekaż obraz niestandardowy](virtual-machines-linux-create-upload-generic.md).)

| Alias     | Program Publisher | Oferty        | JEDNOSTKA SKU         | Wersja |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | najnowsze  |
| CoreOS    | CoreOS    | CoreOS       | Stały      | najnowsze  |
| Debian    | credativ  | Debian       | 8           | najnowsze  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | najnowsze  |
| RHEL      | Funkcję czerwony    | RHEL         | 7.2         | najnowsze  |
| UbuntuLTS | Kanoniczny | Serwer Ubuntu | 14.04.4-LTS | najnowsze  |

Następujące sekcje użyj `UbuntuLTS` alias dla opcji **ImageURN** (`-Q`) do wdrożenia na serwerze KÓW Ubuntu 14.04.4.

Poprzedni `quick-create` przykład tylko wymienionym `-M` flagę, aby zidentyfikować klucz publiczny SSH, aby przekazać podczas wyłączania haseł SSH, zostanie wyświetlony monit o następujące argumenty:

- Nazwa grupy zasobów (dowolny ciąg wystarcza zazwyczaj pierwszej grupy zasobów Azure)
- Nazwa maszyn wirtualnych
- Lokalizacja (`westus` lub `westeurope` są dobre ustawienia domyślne)
- Linux (aby umożliwić Azure ustalić systemu operacyjnego, które chcesz)
- Nazwa użytkownika

W poniższym przykładzie wszystkie wartości tak, aby nie dodatkowo monitowania jest wymagane. Ile masz `~/.ssh/id_rsa.pub` jako ssh-rsa format publicznej klucza, to działa tak jak:

```bash
azure vm quick-create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--admin-username myAdminUser \
--ssh-public-file ~/.ssh/id_rsa.pub \
--image-urn UbuntuLTS
```

Wynik powinna wyglądać podobnie do następujących blok wyjściowy:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Zaloguj się do nowego maszyn wirtualnych

Zaloguj się do swojego maszyn wirtualnych przy użyciu publicznego adresu IP wymienione w wyniku kwerendy. Umożliwia także w pełni kwalifikowaną nazwę domeny (FQDN) podany:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

Proces logowania powinien wyglądać na przykład następujący blok danych wyjściowych:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

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

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Następne kroki

`azure vm quick-create` Polecenie jest sposobem szybkiego wdrożenia maszyny, aby można było zalogować się do powłoki imprezie i rozpoczynanie pracy. Jednak za pomocą `vm quick-create` nie daje rozległa kontroli ani nie można utworzyć bardziej złożone środowiska.  Aby wdrożyć Linux maszyn wirtualnych, który jest dostosowany do infrastruktury, można wykonać dowolną z następujących artykułów:

- [Szablon Menedżera zasobów Azure umożliwia tworzenie wdrażania](virtual-machines-linux-cli-deploy-templates.md)
- [Tworzenie niestandardowego środowiska dla maszyny Linux bezpośrednio za pomocą poleceń polecenie Azure](virtual-machines-linux-create-cli-complete.md)
- [Tworzenie SSH zabezpieczone Linux maszyn wirtualnych Azure korzystania z szablonów](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Możesz także [za pomocą `docker-machine` Azure sterownik z różnych poleceń, aby szybko utworzyć maszyny Linux jako docker host](virtual-machines-linux-docker-machine.md).
