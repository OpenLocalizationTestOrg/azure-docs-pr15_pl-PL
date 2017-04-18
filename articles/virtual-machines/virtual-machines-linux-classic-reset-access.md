<properties
        pageTitle="Resetowanie hasła maszyn wirtualnych Linux i SSH klucz polecenie | Microsoft Azure"
        description="Jak korzystać z rozszerzeniem VMAccess z interfejsu wiersza polecenia Azure (polecenie) do resetowania hasła maszyn wirtualnych Linux lub klawisz SSH, rozwiązać problem z konfiguracją SSH i sprawdzanie spójności dysku"
        services="virtual-machines-linux"
        documentationCenter=""
        authors="cynthn"
        manager="timlt"
        editor=""
        tags="azure-service-management"/>

<tags
        ms.service="virtual-machines-linux"
        ms.workload="infrastructure-services"
        ms.tgt_pltfrm="vm-linux"
        ms.devlang="na"
        ms.topic="article"
        ms.date="06/14/2016"
        ms.author="cynthn"/>

# <a name="how-to-reset-a-linux-vm-password-or-ssh-key-fix-the-ssh-configuration-and-check-disk-consistency-using-the-vmaccess-extension"></a>Jak zresetować hasło maszyn wirtualnych Linux lub klawisz SSH, rozwiązać problem z konfiguracją SSH i sprawdzanie spójności dysku przy użyciu rozszerzenia VMAccess


Jeśli nie możesz połączyć się maszyny wirtualnej Linux Azure ze względu na zapomniane hasło niepoprawny klucz Secure Shell (SSH), lub problem z konfiguracją SSH za pomocą rozszerzenia VMAccessForLinux polecenie Azure o zresetowanie hasła lub klucza SSH, rozwiązać problem z konfiguracją SSH i sprawdzanie spójności dysków. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Dowiedz się, jak [wykonać te kroki przy użyciu modelu Menedżera zasobów](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess).

Przy użyciu polecenie Azure umożliwia polecenie **set rozszerzenia azure maszyn wirtualnych** usługi interfejsie wiersza polecenia (imprezie, Terminal, wiersz polecenia) polecenia programu access. Uruchom **rozszerzenie maszyn wirtualnych azure pomocy Ustawianie** zastosowania szczegółowe rozszerzenia.

Polecenie Azure możesz wykonać następujące zadania:

+ [Resetowanie hasła](#pwresetcli)
+ [Resetowanie klucza SSH](#sshkeyresetcli)
+ [Resetowanie hasła i klucza SSH](#resetbothcli)
+ [Tworzenie nowego konta użytkownika sudo](#createnewsudocli)
+ [Zresetuj konfigurację SSH](#sshconfigresetcli)
+ [Usuwanie użytkownika](#deletecli)
+ [Wyświetlanie stanu z rozszerzeniem VMAccess](#statuscli)
+ [Sprawdzanie spójności dodane dyski](#checkdisk)
+ [Naprawianie dodane dyski na maszyn wirtualnych usługi Linux](#repairdisk)


## <a name="prerequisites"></a>Wymagania wstępne

Należy wykonać następujące czynności:

- Należy [zainstalować polecenie Azure](../xplat-cli-install.md) i [połączyć do subskrypcji usługi](../xplat-cli-connect.md) Azure zasoby skojarzonego z kontem.
- Ustaw poprawne tryb model klasyczny wdrażania, wpisując następujące polecenie w wierszu polecenia:
        
        azure config mode asm
        
- Jeśli chcesz przywrócić jednego mają nowego hasła lub zestaw kluczy SSH. Jeśli chcesz przywrócić konfigurację SSH nie bez nich.


## <a name="pwresetcli"></a>Resetowanie hasła

1. Tworzenie pliku o nazwie PrivateConf.json z tymi wierszami. Zamienianie nawiasy i & #60; symbol zastępczy & #62; wartości na własne informacje.

        {
        "username":"<currentusername>",
        "password":"<newpassword>",
        "expiration":"<2016-01-01>"
        }

2. Uruchom to polecenie podstawiając nazwę komputera wirtualnych dla & #60; nazwy maszyn wirtualnych & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* –-private-config-path PrivateConf.json

## <a name="sshkeyresetcli"></a>Resetowanie klucza SSH

1. Tworzenie pliku o nazwie PrivateConf.json z tych zawartości. Zamienianie nawiasy i & #60; symbol zastępczy & #62; wartości na własne informacje.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>"
        }

2. Uruchom to polecenie podstawiając nazwę komputera wirtualnych dla & #60; nazwy maszyn wirtualnych & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="resetbothcli"></a>Zresetuj hasło i klucza SSH

1. Tworzenie pliku o nazwie PrivateConf.json z tych zawartości. Zamienianie nawiasy i & #60; symbol zastępczy & #62; wartości na własne informacje.

        {
        "username":"<currentusername>",
        "ssh_key":"<contentofsshkey>",
        "password":"<newpassword>"
        }

2. Uruchom to polecenie podstawiając nazwę komputera wirtualnych dla & #60; nazwy maszyn wirtualnych & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="createnewsudocli"></a>Tworzenie nowego konta użytkownika sudo

Jeśli zapomnisz swoją nazwę użytkownika, można utworzyć nową władzy sudo za pomocą VMAccess. W tym przypadku istniejącej nazwy użytkownika i hasła będą nie można zmodyfikować.

Aby utworzyć nowego użytkownika sudo z hasłem, skorzystaj z tego skryptu w [Resetowanie hasła](#pwresetcli) i podaj nową nazwę użytkownika.

Aby utworzyć nowy użytkownik sudo o dostęp do kluczy SSH, skorzystaj z tego skryptu w [zresetować klucz SSH](#sshkeyresetcli) i podaj nową nazwę użytkownika.

Umożliwia także [zresetować hasło i klucza SSH](#resetbothcli) do utworzenia nowego użytkownika z polach hasło i SSH klawiszy dostępu.

## <a name="sshconfigresetcli"></a>Zresetuj konfigurację SSH

Jeśli konfiguracja SSH jest w stanie niepożądane, może również utracić dostęp do maszyn wirtualnych. Rozszerzenie VMAccess służy zresetować konfigurację do stanu domyślnego. Aby to zrobić, wystarczy ustawić klucz "reset_ssh" do "True". Rozszerzenie ponownie uruchomić serwer SSH, otwórz port SSH na swojej maszyn wirtualnych i zresetuj konfigurację SSH wartości domyślne. Konto użytkownika (nazwa, hasło lub klawiszy SSH) nie będzie można zmienić.

> [AZURE.NOTE] Plik konfiguracyjny SSH, który pobiera Resetowanie znajduje się w /etc/ssh/sshd_config.

1. Tworzenie pliku o nazwie PrivateConf.json z tej zawartości.

        {
        "reset_ssh":"True"
        }

2. Uruchom to polecenie podstawiając nazwę komputera wirtualnych dla & #60; nazwy maszyn wirtualnych & #62;. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="deletecli"></a>Usuwanie użytkownika

Jeśli chcesz usunąć konto użytkownika bez rejestrowania do maszyn wirtualnych bezpośrednio, używając tego skryptu.

1. Tworzenie pliku o nazwie PrivateConf.json z tej zawartości, zastępując nazwę użytkownika, aby usunąć & #60; usernametoremove & #62;. 

        {
        "remove_user":"<usernametoremove>"
        }

2. Uruchom to polecenie podstawiając nazwę komputera wirtualnych dla & #60; nazwy maszyn wirtualnych & #62;. 

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --private-config-path PrivateConf.json

## <a name="statuscli"></a>Wyświetlanie stanu z rozszerzeniem VMAccess

Aby wyświetlić stan rozszerzenia VMAccess, uruchom polecenie.

        azure vm extension get

## <a name="a-namecheckdiskacheck-consistency-of-added-disks"></a>< name = "checkdisk" <</a>Sprawdzanie spójności dodane dyski

Aby uruchomić fsck na wszystkich dyskach na tym komputerze wirtualnych Linux, należy wykonać następujące czynności:

1. Tworzenie pliku o nazwie PublicConf.json z tej zawartości. Sprawdzanie dysku ma wartość logiczną dla czy sprawdzać dyski podłączone do komputera wirtualnych, czy nie. 

        {   
        "check_disk": "true"
        }

2. Uruchom to polecenie, aby wykonać, podstawiając nazwę komputera wirtualnych dla & #60; nazwy maszyn wirtualnych & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json 

## <a name='repairdisk'></a>Naprawianie dysków 

Aby naprawić dysków, które nie są Instalowanie lub zawierają błędy konfiguracji instalacji, należy użyć rozszerzenia VMAccess zresetować konfigurację instalacji na komputerze wirtualnych Linux. Podstawiając nazwę dysku & #60; yourdisk & #62;.

1. Tworzenie pliku o nazwie PublicConf.json z tej zawartości. 

        {
        "repair_disk":"true",
        "disk_name":"<yourdisk>"
        }

2. Uruchom to polecenie, aby wykonać, podstawiając nazwę komputera wirtualnych dla & #60; nazwy maszyn wirtualnych & #62;.

        azure vm extension set <vm-name> VMAccessForLinux Microsoft.OSTCExtensions 1.* --public-config-path PublicConf.json



## <a name="next-steps"></a>Następne kroki

* Jeśli chcesz użyć poleceń cmdlet środowiska PowerShell Azure lub szablony Menedżera zasobów Azure o zresetowanie hasła lub klucza SSH, rozwiązywanie konfiguracji SSH i sprawdzanie spójności dysku, można znaleźć w [dokumentacji rozszerzenia VMAccess na GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess). 

* Umożliwia także [Azure portal](https://portal.azure.com) do resetowania hasła lub klucza SSH maszyny Linux wdrożony w modelu Klasyczny wdrożenia. Obecnie nie można używać portalu do tego maszyny Linux wdrożony w modelu wdrożenia Menedżera zasobów.

* Aby uzyskać więcej informacji o używaniu rozszerzenia maszyn wirtualnych dla Azure maszyn wirtualnych, zobacz [temat rozszerzenia maszyn wirtualnych i funkcje](virtual-machines-linux-extensions-features.md) .
