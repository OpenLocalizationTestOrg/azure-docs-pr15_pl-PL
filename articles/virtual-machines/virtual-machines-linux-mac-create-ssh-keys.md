<properties
    pageTitle="Tworzenie kluczy SSH Linux i komputerów Mac | Microsoft Azure"
    description="Generowanie i używanie klawiszy SSH na Linux i komputerów Mac dla Menedżera zasobów i modeli wdrażania klasyczny Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="v-livech"/>

# <a name="create-ssh-keys-on-linux-and-mac-for-linux-vms-in-azure"></a>Tworzenie klawiszy SSH Linux i komputerów Mac dla maszyny wirtualne Linux platformy Azure

Azure, który domyślnie za pomocą klawiszy SSH uwierzytelniania, co eliminuje potrzebę hasła, aby zalogować się przy użyciu kluczy SSH można utworzyć maszyn wirtualnych.  Hasła można złamać i otwórz pośrednictwem usługi SMS do prób bezlitosnym życie atakami odgadnięcie hasła. Maszyny wirtualne utworzone za pomocą szablonów Azure lub `azure-cli` mogą zawierać kluczem publicznym SSH jako części wdrożenia, usuwanie konfiguracji wdrażania wpisu.  Jeśli łączysz się maszyny Linux z systemu Windows, zobacz [tego dokumentu.](virtual-machines-linux-ssh-from-windows.md)

Wymaga tego artykułu:

- Konto Azure ([Pobierz bezpłatną wersję próbną](https://azure.microsoft.com/pricing/free-trial/)).

- [Polecenie Azure](../xplat-cli-install.md) podczas logowania`azure login`

- Tryb Menedżera zasobów Azure _muszą znajdować się w_ polecenie Azure`azure config mode arm`

## <a name="quick-commands"></a>Szybkie polecenia

W następujących poleceń Zamień przykłady własne opcje.

SSH klucze są domyślnie przechowywane w `.ssh` katalogu.  

```bash
cd ~/.ssh/
```

Jeśli nie masz `~/.ssh` katalogu `ssh-keygen` polecenia zostanie utworzony automatycznie z odpowiednimi uprawnieniami.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

Wprowadź nazwę pliku, który jest zapisywany w `~/.ssh/` katalogu:

```bash
id_rsa
```

Wprowadź hasło dla id_rsa:

```bash
correct horse battery staple
```

Obecnie `id_rsa` i `id_rsa.pub` SSH pary kluczy w `~/.ssh` katalogu.

```bash
ls -al ~/.ssh
```

Dodawanie klucza nowo utworzonego w celu `ssh-agent` na Linux i komputerów Mac (również dodany do Pęk kluczy OSX):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Skopiuj kluczem publicznym SSH serwera Linux:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub myusername@myserver
```

Przetestuj Zaloguj się przy użyciu klawiszy zamiast hasła:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa myusername@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## <a name="detailed-walkthrough"></a>Uzyskać szczegółowy opis

Za pomocą kluczy publicznych i prywatnych SSH jest najprostszym sposobem na Zaloguj się na serwerach Linux. [Kryptograficzny kluczy publicznych](https://en.wikipedia.org/wiki/Public-key_cryptography) znajdują się bardziej bezpieczny sposób, aby zalogować się do usługi Linux lub maszyn wirtualnych BSD platformy Azure niż hasła, które mogą być wymuszone słownikowym znacznie łatwiejsze. Klucz publiczny, można je udostępnić osobie; ale tylko dla Ciebie (lub infrastruktury zabezpieczeń lokalnych) posiadają klucz prywatny.  Klucz prywatny SSH powinien mieć [bardzo bezpieczne hasło](https://www.xkcd.com/936/) (źródło:[xkcd.com](https://xkcd.com)) do ochrony go.  To hasło po prostu ma dostęp do klucz prywatny SSH i **nie jest** hasło do konta użytkownika.  Po dodaniu hasła do klucza SSH szyfrowanie klucz prywatny, aby klucz prywatny jest bezużyteczne bez hasło, aby ją odblokować.  Atakującej kraść klucz prywatny tego klawisza nie ma hasła, ta osoba będzie mogła służy do logowania na wszystkich serwerach, które mają odpowiedni klucz publiczny klucz prywatny.  Jeśli klucz prywatny jest chroniony hasłem, go nie mogą być używane przez tego atakująca, dostarczając dodatkową warstwę zabezpieczeń dla infrastruktury Azure.

W tym artykule tworzy pliki klucza *ssh-rsa* sformatowane, które są zalecane w przypadku wdrożeń na Menedżera zasobów.  klawisze *SSH-rsa* są wymagane do [portalu](https://portal.azure.com) w przypadku wdrożeń zarówno klasyczny i Menedżera zasobów.

## <a name="create-the-ssh-keys"></a>Tworzenie kluczy SSH

Azure wymaga co najmniej 2048-bitowy, ssh-rsa formatowanie klucze publiczne i prywatne. Aby utworzyć użyj klawiszy `ssh-keygen`, który zwróci się szereg pytań, a następnie zapisuje klucz prywatny i pasujące kluczem publicznym. Po utworzeniu maszyn wirtualnych Azure kluczem publicznym są kopiowane do `~/.ssh/authorized_keys`.  Klucze SSH w `~/.ssh/authorized_keys` są używane do wezwania klienta zgodnie z połączenia logowania SSH na odpowiedni klucz prywatny.

## <a name="using-ssh-keygen"></a>Za pomocą ssh-keygen

To polecenie tworzy hasło zabezpieczone (szyfrowane) SSH kluczy przy użyciu RSA 2048-bitowy i jest ujęta w celu uproszczenia jego identyfikacji.  

Rozpocznij, zmieniając katalogów, tak aby wszystkie usługi ssh klucze są tworzone w katalogu.

```bash
cd ~/.ssh
```

Jeśli nie masz `~/.ssh` katalogu `ssh-keygen` polecenia zostanie utworzony automatycznie z odpowiednimi uprawnieniami.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

_Polecenie dodatków_

`ssh-keygen`= Aplikacja użyta do utworzenia kluczy

`-t rsa`= Typ klawisz, aby utworzyć, która jest [RSA format](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= bitów klucza

`-C "myusername@myserver"`= komentarza dołączane na końcu ułatwiającej identyfikację plik klucza publicznego.  Zwykle wiadomości e-mail jest używany jako komentarz, ale możesz użyć dowolnego elementu najlepiej pasuje do infrastruktury.

### <a name="using-pem-keys"></a>Używanie klawiszy PEM

Jeśli korzystasz z klasycznego wdrożeniu modelu (klasyczny Portal Azure lub polecenie zarządzania usługi Azure `asm`), może być konieczne używanie PEM sformatowane SSH klawisze, aby uzyskać dostęp do swojego maszyny wirtualne Linux.  Poniżej opisano, jak utworzyć klucz PEM z istniejących SSH kluczem publicznym i istniejących x509 certyfikatu.

Aby utworzyć PEM sformatowany klucza z istniejącego klucza publicznego SSH:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Przykład ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 myusername@myserver
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Zapisane pliki kluczy:

`Enter file in which to save the key (/home/myusername/.ssh/id_rsa): id_rsa`

Nazwa pary kluczy w tym artykule.  O pary kluczy o nazwie **id_rsa** to ustawienie domyślne, a niektóre narzędzia by się spodziewać nazwę plik klucza prywatnego **id_rsa** , więc mających jedno jest dobrym pomysłem. Katalog `~/.ssh/` jest domyślna lokalizacja plików konfiguracji SSH i SSH pary kluczy.

```bash
ls -al ~/.ssh
-rw------- 1 myusername staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 myusername staff   410 Aug 25 18:04 rsa.pub
```
Lista `~/.ssh` katalogu. `ssh-keygen`Tworzy `~/.ssh` katalogu, jeśli nie ma, a także ustawia poprawne tryby własności i plik.

Hasło klucza:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`odwołuje się do hasła jako "hasło".  *Zdecydowanie* zaleca się Dodaj hasło do usługi pary kluczy jest. Bez użycia hasła ochrony pary kluczy każda osoba dysponująca plik klucza prywatnego służy do logowania do dowolnego serwera, który ma odpowiedni klucz publiczny. Dodawanie hasło zapewnia ochronę więcej w przypadku, gdy dana osoba jest możliwość uzyskania dostępu do pliku kluczy prywatnych podane czasu, aby zmienić klawisze użyta do uwierzytelnienia.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Za pomocą ssh agenta do przechowywania hasło klucza prywatnego

Aby uniknąć wpisywania hasło plik klucza prywatnego z każdej logowania SSH, możesz użyć `ssh-agent` buforować hasło plik klucza prywatnego. Jeśli używasz komputera Mac OSX Pęk kluczy bezpieczne są przechowywane hasła kluczy prywatnych po rozpoczęciu `ssh-agent`.

Najpierw sprawdź, czy `ssh-agent` jest uruchomiony

```bash
eval "$(ssh-agent -s)"
```

Teraz Dodaj klucz prywatny do `ssh-agent` za pomocą polecenia `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Hasło klucza prywatnego znajduje się teraz w `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Tworzenie i konfigurowanie pliku konfiguracji SSH

Jest zalecane najlepiej utworzyć i skonfigurować `~/.ssh/config` plik, aby przyspieszyć logowania dodatki i dotyczące optymalizowania Twoich zachowań klienta SSH.

W poniższym przykładzie pokazano konfiguracji standardowej.

### <a name="create-the-file"></a>Tworzenie pliku

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Edytuj plik, aby dodać nową konfigurację SSH:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Przykład `~/.ssh/config` pliku:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User myusername
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Tej konfiguracji SSH umożliwia sekcje dla każdego serwera umożliwiające ma własny dedykowane pary kluczy. Ustawienia domyślne (`Host *`) dotyczą wszelkich hostów, które nie będzie pasować do żadnej z określonych hostów znajdujące się wyżej w pliku konfiguracji.


### <a name="config-file-explained"></a>Omówienie pliku konfiguracji

`Host`= nazwy hosta wywoływana na terminal.  `ssh fedora22`Wskazuje `SSH` Aby użyć wartości w bloku ustawienia etykietą `Host fedora22` Uwaga: może to być dowolny etykietę, która jest logiczna do użycia, a nie stanowi rzeczywisty hostname dowolnego serwera.

`Hostname 102.160.203.241`= adres IP lub nazwę serwera DNS.

`User myusername`= konta użytkownika zdalnego używać podczas logowania do serwera.

`PubKeyAuthentication yes`= informuje SSH, aby zalogować się przy użyciu klucza SSH.

`IdentityFile /home/myusername/.ssh/id_id_rsa`= klucz prywatny SSH i odpowiedni klucz publiczny służącego do uwierzytelniania.


## <a name="ssh-into-linux-without-a-password"></a>SSH do Linux bez użycia hasła

Teraz, gdy masz parę kluczy SSH i skonfigurowany pliku konfiguracji SSH, możesz mogą logować się do swojego maszyn wirtualnych Linux szybkie i bezpieczne. Podczas pierwszego logowania na serwerze przy użyciu klucza SSH wierszy polecenia można dla hasło dla tego pliku klucza.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Polecenie dodatków

Gdy `ssh fedora22` jest wykonywane SSH najpierw zwraca i załadowanie wszystkie ustawienia z `Host fedora22` bloku, a następnie obciążenia wszystkie pozostałe ustawienia z ostatniego bloku `Host *`.

## <a name="next-steps"></a>Następne kroki

Następny jest tworzenie maszyny wirtualne Linux Azure za pomocą nowego klucza publicznego SSH.  Azure maszyny wirtualne utworzone za pomocą SSH kluczem publicznym jako logowania są lepiej zabezpieczone niż maszyny wirtualne utworzone przy użyciu domyślnej metody logowania, hasła.  Azure maszyny wirtualne utworzony przy użyciu klawiszy SSH są domyślnie skonfigurowano hasła wyłączona, można uniknąć wymuszone słownikowym odgadnięcie prób.

- [Tworzenie bezpiecznego maszyn wirtualnych Linux przy użyciu szablonu Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Tworzenie bezpiecznego maszyn wirtualnych Linux za pomocą portalu Azure](virtual-machines-linux-quick-create-portal.md)
- [Tworzenie bezpiecznego maszyn wirtualnych Linux za pomocą interfejsu wiersza polecenia Azure](virtual-machines-linux-quick-create-cli.md)
