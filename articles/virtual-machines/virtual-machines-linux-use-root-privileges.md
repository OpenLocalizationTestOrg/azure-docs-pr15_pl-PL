<properties 
    pageTitle="Za pomocą uprawnień głównego w środowisku maszyn wirtualnych systemu Linux | Microsoft Azure" 
    description="Dowiedz się, jak używać uprawnień głównego na maszyny wirtualnej Linux Azure." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>


# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Przy użyciu uprawnień głównego w przypadku maszyn wirtualnych Linux platformy Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Domyślnie `root` użytkownika jest wyłączona w przypadku maszyn wirtualnych Linux platformy Azure. Użytkownicy mogą uruchamiać polecenia z podwyższonym poziomem uprawnień przy użyciu `sudo` polecenia. Jednak może być różna w zależności od tego, jak został zainicjowany systemu.

1. **Klucz SSH i hasła lub hasło tylko** - maszyny wirtualnej został zainicjowany albo certyfikatem (`.CER` pliku) lub klawisz SSH oraz hasło, lub tylko nazwę użytkownika i hasło. W tym przypadku `sudo` wyświetli monit o hasło użytkownika przed wykonaniem polecenia.

2. **Tylko klucz SSH** — maszyny wirtualnej został zainicjowany za pomocą certyfikatu (`.cer`, `.pem`, lub `.pub` pliku) lub klucz SSH, ale bez użycia hasła.  W tym przypadku `sudo` **nie** Monituj o hasło użytkownika przed wykonaniem polecenia.


## <a name="ssh-key-and-password-or-password-only"></a>SSH klucz i hasła lub hasło tylko

Zaloguj się do maszyny wirtualnej Linux przy użyciu funkcji uwierzytelniania SSH klucz lub hasło, a następnie uruchom przy użyciu polecenia `sudo`, na przykład:

    # sudo <command>
    [sudo] password for azureuser:

W tym przypadku użytkownik zostanie wyświetlony monit o podanie hasła. Po wprowadzeniu hasła `sudo` spowoduje uruchomienie polecenia z `root` uprawnienia.

Możesz również włączyć passwordless sudo, edytując `/etc/sudoers.d/waagent` pliku, na przykład:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Ta zmiana pozwoli na passwordless sudo przez użytkownika "azureuser".

## <a name="ssh-key-only"></a>SSH tylko klucza

Zaloguj się do maszyny wirtualnej Linux przy użyciu funkcji uwierzytelniania klucza SSH, a następnie uruchom przy użyciu polecenia `sudo`, na przykład:

    # sudo <command>

W tym przypadku użytkownik będzie **nie** zostać wyświetlony monit o podanie hasła. Po naciśnięcie `<enter>`, `sudo` spowoduje uruchomienie polecenia z `root` uprawnienia.

 
