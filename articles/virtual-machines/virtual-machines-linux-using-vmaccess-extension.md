<properties
    pageTitle="Resetowanie dostęp na maszyny wirtualne Linux Azure przy użyciu rozszerzenia VMAccess | Microsoft Azure"
    description="Resetowanie dostęp na maszyny wirtualne Linux Azure przy użyciu rozszerzenia VMAccess."
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
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Zarządzanie użytkownikami, SSH i wyboru lub naprawa dysków maszyny wirtualne Linux Azure przy użyciu rozszerzenia VMAccess

W tym artykule pokazano, jak za pomocą rozszerzenia VMAcesss Azure sprawdzanie lub naprawiania dysku, resetowanie dostępu użytkowników, zarządzanie kontami użytkowników i zresetuj konfigurację SSHD w systemie Linux. Wymaga tego artykułu:

- Konto Azure ([Pobierz bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/)).

- [Polecenie Azure](../xplat-cli-install.md) podczas logowania `azure login`.

- Polecenie Azure _muszą znajdować się w_ Menedżera zasobów Azure tryb `azure config mode arm`.

## <a name="quick-commands"></a>Szybkie polecenia

Istnieją dwa sposoby korzystania z VMAccess na swojej maszyny wirtualne Linux:

- Za pomocą interfejsu wiersza polecenia Azure i wymaganych parametrów.
- Korzystanie z nieprzetworzonych plików JSON, które VMAccess przetwarza i działa na.

W sekcji Szybkie polecenia, firma Microsoft będzie używany polecenie Azure `azure vm reset-access` metody. W poniższych przykładach polecenie Zamień wartości, które zawierają "przykład" z wartościami z własnych środowiska.

## <a name="create-a-resource-group-and-linux-vm"></a>Tworzenie grupy zasobów i Linux oraz maszyn wirtualnych

```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Tworzenie Debian maszyn wirtualnych

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-g myResourceGroup \
-l westus \
-y Linux \
-n myVM \
-Q Debian
```

## <a name="reset-root-password"></a>Resetowanie hasła głównego

Aby zresetować hasło głównego:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u root \
-p myNewPassword
```

## <a name="ssh-key-reset"></a>Resetowanie klucza SSH

Aby zresetować klucz SSH administratora:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Tworzenie użytkownika

Aby utworzyć użytkownika:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-p myAdminUserPassword
```

## <a name="remove-a-user"></a>Usuwanie użytkownika

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-R myRemovedUser
```

## <a name="reset-sshd"></a>Resetuj SSHD

Aby przywrócić konfigurację SSHD:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM
-r
```


## <a name="detailed-walkthrough"></a>Uzyskać szczegółowy opis

### <a name="vmaccess-defined"></a>VMAccess zdefiniowane:

Dysk na maszyn wirtualnych usługi Linux są pokazywane błędy. Jakiś sposób zresetowanie hasła głównego maszyn wirtualnych usługi Linux lub przypadkowemu usunięciu SSH klucz prywatny. Które stało się ponownie dni centrum danych, potrzebne sterują istnieje, a następnie otwórz KVM, aby przejść do konsoli serwera. Rozszerzenie Azure VMAccess można traktować jako tego przełącznika KVM, która umożliwia dostęp do konsoli, aby zresetować dostęp do Linux lub przeprowadzić konserwację poziomu dysku.

Aby uzyskać szczegółowe informacje firma Microsoft będzie używany długą formę VMAccess, który używa plików nieprzetworzonych JSON.  Te pliki VMAccess JSON można skrót przy użyciu szablonów Azure.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Sprawdzanie i naprawianie dysku maszyny Linux przy użyciu VMAccess

Za pomocą VMAccess można wykonać fsck Uruchom na dysku w obszarze maszyn wirtualnych usługi Linux.  Można też wykonać sprawdzanie dysku i naprawiania dysku przy użyciu VMAccess.

Sprawdzać, a następnie naprawić dysk Użyj tego skryptu VMAccess:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Wykonywanie skryptu VMAccess:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Przywracanie Linux dostępu użytkowników za pomocą VMAccess

W przypadku utraty dostępu do głównego na maszyn wirtualnych usługi Linux, możesz uruchomić skrypt VMAccess o zresetowanie hasła głównego.

Aby zresetować hasło głównego, użyj tego skryptu VMAccess:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

Wykonywanie skryptu VMAccess:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Aby zresetować klucz SSH administratora-, użyj tego skryptu VMAccess:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

Wykonywanie skryptu VMAccess:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>Za pomocą VMAccess do zarządzania kontami użytkowników w systemie Linux

VMAccess jest skrypt Python, który może być używany do zarządzania użytkownikami w maszyn wirtualnych usługi Linux bez logowania i za pomocą sudo lub konta administratora.

Aby utworzyć użytkownika, użyj tego skryptu VMAccess:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

Wykonywanie skryptu VMAccess:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Aby usunąć użytkownika, użyj tego skryptu VMAccess:

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

Wykonywanie skryptu VMAccess:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>Zresetuj konfigurację SSHD za pomocą VMAccess

Jeśli wprowadzania zmian w konfiguracji SSHD maszyny wirtualne Linux i zamknij połączenie SSH przed weryfikowanie zmian, użytkownik może będą mogli SSH'ing się ponownie.  VMAccess można przywrócić konfiguracji SSHD do znanej dobrej konfiguracji bez konieczności logowania nad SSH.

Aby zresetować konfigurację SSHD użyć tego skryptu VMAccess:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Wykonywanie skryptu VMAccess:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Następne kroki

Aktualizowanie Linux przy użyciu rozszerzenia VMAccess Azure jest jednym ze sposobów Aby wprowadzić zmiany na uruchomionego maszyny Linux.  Za pomocą narzędzi, takich jak inicjowania chmury i szablony Azure do modyfikowania usługi maszyn wirtualnych Linux na uruchamiania.

[Informacje o rozszerzenia maszyn wirtualnych i funkcje](virtual-machines-linux-extensions-features.md)

[Tworzenie szablonów Menedżera zasobów Azure z rozszerzeniami Linux maszyn wirtualnych](virtual-machines-linux-extensions-authoring-templates.md)

[Dostosowywanie maszyny Linux podczas tworzenia za pomocą inicjowania chmury](virtual-machines-linux-using-cloud-init.md)
