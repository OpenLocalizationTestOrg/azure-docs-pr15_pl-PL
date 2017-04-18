<properties
    pageTitle="Dostosowywanie maszyny Linux podczas tworzenia za pomocą inicjowania chmury | Microsoft Azure"
    description="Dostosowywanie maszyny Linux podczas tworzenia za pomocą inicjowania chmury"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="v-livech"
/>

# <a name="using-cloud-init-to-customize-a-linux-vm-during-creation"></a>Dostosowywanie maszyny Linux podczas tworzenia za pomocą inicjowania chmury

W tym artykule pokazano, jak utworzyć skrypt chmury inicjowania, aby ustawić hostname, opakowania zainstalowano aktualizację i zarządzać kontami użytkowników.  Podczas tworzenia maszyn wirtualnych z Azure interfejsu wiersza polecenia są nazywane skrypty chmurze inicjowania.  Wymaga tego artykułu:

- Konto Azure ([Pobierz bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/)).

- [Polecenie Azure](../xplat-cli-install.md) podczas logowania `azure login`.

- Polecenie Azure _muszą znajdować się w_ Menedżera zasobów Azure tryb `azure config mode arm`.

## <a name="quick-commands"></a>Szybkie polecenia

Utworzyć skrypt init.txt chmury, który ustawia nazwa hosta, aktualizacje wszystkich pakietów i dodaje użytkownika sudo do Linux.

```bash
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Tworzenie grupy zasobów, aby uruchomić maszyny wirtualne do.

```bash
azure group create myResourceGroup westus
```

Tworzenie maszyny Linux oraz jej konfigurowania podczas uruchamiania przy użyciu inicjowania chmury.

```bash
azure vm create \
-g myResourceGroup \
-n myVM \
-l westus \
-y Linux \
-f myVMnic \
-F myVNet \
-P 10.0.0.0/22 \
-j mySubnet \
-k 10.0.0.0/24 \
-Q canonical:ubuntuserver:14.04.2-LTS:latest \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Uzyskać szczegółowy opis

### <a name="introduction"></a>Wprowadzenie

Po uruchomieniu nowego maszyny Linux występują standardowe maszyn wirtualnych Linux pustą dostosowane lub gotowy do własnych potrzeb. [Chmura inicjowania](https://cloudinit.readthedocs.org) to standardowy sposób do dodania skryptu lub konfiguracja ustawienia w tym maszyn wirtualnych Linux podczas uruchamiania dla po raz pierwszy.

Azure, aby zmiany na maszyny Linux jego są trzy sposoby wdrożona lub uruchomiony.

- Uruchomić skrypty za pomocą inicjowania chmury.
- Uruchomić skrypty za pomocą Azure [Rozszerzenia VMAccess](virtual-machines-linux-using-vmaccess-extension.md).
- Szablon Azure za pomocą inicjowania chmury.
- Szablon Azure za pomocą [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md).

Do dodania skryptów w dowolnym momencie po uruchamiania:

- SSH, aby uruchomić polecenia bezpośrednio
- Wprowadzić skryptów za pomocą Azure [Rozszerzenia VMAccess](virtual-machines-linux-using-vmaccess-extension.md)imperatively lub w szablonie Azure
- Narzędzia zarządzania konfiguracją, takich jak Ansible, Salt, Kuchmistrz i Puppet.

>[AZURE.NOTE]: VMAccess Extension executes a script as root in the same way using SSH can.  However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Dostępność inicjowania chmury na maszyn wirtualnych Azure szybkie tworzenie aliasów obraz:

| Alias     | Program Publisher | Oferty        | JEDNOSTKA SKU         | Wersja | Chmura inicjowania |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS    | OpenLogic | Centos       | 7.2         | najnowsze  | Brak         |
| CoreOS    | CoreOS    | CoreOS       | Stały      | najnowsze  | tak        |
| Debian    | credativ  | Debian       | 8           | najnowsze  | Brak         |
| openSUSE  | SUSE      | openSUSE     | 13.2        | najnowsze  | Brak         |
| RHEL      | RedHat    | RHEL         | 7.2         | najnowsze  | Brak         |
| UbuntuLTS | Kanoniczny | UbuntuServer | 14.04.4-LTS | najnowsze  | tak        |

Firma Microsoft współpracuje z partnerami uzyskanie inicjowania chmury dostępny i pracy z obrazów, które zapewniają Azure.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Dodawanie skryptu inicjowania chmury do tworzenia maszyn wirtualnych z polecenie Azure

Aby uruchamianie skryptu chmury inicjowania podczas tworzenia maszyn wirtualnych w Azure Określ plik inicjowania chmurze za pomocą interfejsu wiersza polecenia Azure `--custom-data` przełączanie.

Tworzenie grupy zasobów, aby uruchomić maszyny wirtualne do.

```bash
azure group create myResourceGroup westus
```

Tworzenie maszyny Linux oraz jej konfigurowania podczas uruchamiania przy użyciu inicjowania chmury.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Tworzenie skrypt inicjowania chmury nazwa hosta maszyny Linux

Jedno z ustawień najprostszym i najważniejszych dla dowolnego maszyn wirtualnych Linux będzie nazwa hosta. Firma Microsoft można łatwo ustawić to inicjowania chmurze za pomocą tego skryptu.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Przykładowy skrypt inicjowania chmury o nazwie `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: myservername
```

Podczas początkowego uruchamiania maszyn wirtualnych, ten skrypt inicjowania chmury ustawia nazwa hosta `myservername`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_hostname.txt
```

Zaloguj się i sprawdź, czy nazwa hosta nowych maszyn wirtualnych.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Tworzenia skryptów inicjowania chmury zaktualizować Linux

Zabezpieczeń Aby dodać do maszyn wirtualnych Ubuntu, aby zaktualizować po pierwszym uruchomieniu komputera.  Używanie inicjowania chmury robimy można przy użyciu skryptu obserwuj, w zależności od rozkład Linux, którego używasz.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Przykładowy skrypt inicjowania chmury `cloud_config_apt_upgrade.txt` Debian rodziny

```bash
#cloud-config
apt_upgrade: true
```

Po uruchomieniu Linux, zainstalowanych pakietów zostały zaktualizowane przy użyciu `apt-get`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_apt_upgrade.txt
```

Zaloguj się i sprawdź, czy wszystkie pakiety zostaną zaktualizowane.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Tworzenie skrypt chmury inicjowania, aby dodać użytkownika do Linux

Pierwszy zadań na wszelkie nowe maszyn wirtualnych Linux jest dodawanego użytkownika dla siebie lub aby uniknąć używania `root`. SSH klucze są najważniejsze wskazówki dotyczące zabezpieczeń i użyteczności i są one dodawane do `~/.ssh/authorized_keys` plik z tego skryptu inicjowania chmury.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Przykładowy skrypt inicjowania chmury `cloud_config_add_users.txt` Debian rodziny

```bash
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Po uruchomieniu Linux, wyświetlonych użytkowników są utworzone i dodane do grupy sudo.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_add_users.txt
```

Zaloguj się i sprawdź, czy nowo utworzonego użytkownika.

```bash
ssh myVM
cat /etc/group
```

Wynik

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Następne kroki

Chmura inicjowania staje się standardowy sposób zmodyfikować usługi maszyn wirtualnych Linux na uruchamiania. Azure też ma rozszerzenia maszyn wirtualnych, które umożliwiają modyfikowanie usługi LinuxVM uruchamiania lub gdy jest uruchomiony. Na przykład Azure VMAccessExtension Umożliwia resetowanie informacje o użytkowniku lub SSH, gdy jest uruchomiona maszyn wirtualnych. Z chmury inicjowania może być konieczne ponownego uruchomienia, aby zresetować hasło.

[Informacje o rozszerzenia maszyn wirtualnych i funkcje](virtual-machines-linux-extensions-features.md)

[Zarządzanie użytkownikami, SSH i wyboru lub naprawa dysków maszyny wirtualne Linux Azure przy użyciu rozszerzenia VMAccess](virtual-machines-linux-using-vmaccess-extension.md)
