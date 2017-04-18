<properties
    pageTitle="Wyłączanie hasła SSH maszyn wirtualnych usługi Linux przez skonfigurowanie SSHD | Microsoft Azure"
    description="Bezpieczny usługi maszyn wirtualnych Linux Azure przez wyłączenie hasło logowania dla SSH."
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
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="v-livech"/>

# <a name="disable-ssh-passwords-on-your-linux-vm-by-configuring-sshd"></a>Wyłączanie hasła SSH maszyn wirtualnych usługi Linux przez skonfigurowanie SSHD

W tym artykule omówiono sposób zablokować zabezpieczeń logowania usługi maszyn wirtualnych Linux.  Jak SSH port 22 zostanie otwarty na początek Boty świata próbuje się zalogować za odgadnięcie hasła.  Firma Microsoft jak to w tym artykule jest wyłączyć hasło logowania przez SSH.  Całkowite usunięcie możliwość używania haseł z tego typu atakami chronione maszyn wirtualnych Linux.  Dodatkowo jest firma Microsoft skonfiguruje SSHD Linux tylko umożliwia logowania przy użyciu kluczy publicznych i prywatnych SSH zdecydowanie Najbezpieczniejszym sposobem logowania do systemu Linux.  Możliwe kombinacje wymaga odgadnięcie klucz prywatny jest olbrzymi i w związku z tym zniechęca do Boty z nawet bothering próby atakami SSH klawiszy.


## <a name="goals"></a>Cele

- Konfigurowanie SSHD, aby uniemożliwić:
  - Hasło logowania
  - Główny logowania użytkownika
  - Uwierzytelnianie wezwanie odpowiedź
- Konfigurowanie SSHD umożliwia:
  - tylko SSH klucza logowania
- Uruchom ponownie SSHD zalogowaniu nadal
- Przetestuj nową konfigurację SSHD

## <a name="introduction"></a>Wprowadzenie

[SSH zdefiniowane](https://en.wikipedia.org/wiki/Secure_Shell)

SSHD jest SSH serwera, na którym działa na maszyn wirtualnych Linux.  SSH jest klienta, która działa z powłoki na komputerze MacBook i Linux oraz pracy.  SSH jest również protokół używany do zabezpieczania i szyfrowania komunikacji między miejscu pracy i maszyn wirtualnych Linux.

W tym artykule, który jest bardzo ważne zachować jeden Zaloguj się do swojego maszyn wirtualnych Linux otwieranie dla całego kolejne kroki.  Z tego powodu zostanie otwarty dwa terminale i SSH do maszyn wirtualnych Linux z obu z nich.  Użyjemy pierwszego terminal wprowadź zmiany w pliku konfiguracji SSHDs i uruchom ponownie usługę SSHD.  Aby sprawdzić te zmiany, po ponownym uruchomieniu usługi użyjemy drugiego terminal.  Ponieważ firma Microsoft są wyłączenie SSH haseł i Polegaj ściśle na SSH klawiszy, jeśli kluczy SSH nie są poprawne i zamknij połączenie maszyn wirtualnych, maszyn wirtualnych zostaną trwale zablokowane i nie będą mogli logowania do niego wymaganie go, aby usunąć i ponownie utworzyć.

## <a name="prerequisites"></a>Wymagania wstępne

- [Tworzenie klawiszy SSH Linux i komputerów Mac dla maszyny wirtualne Linux platformy Azure](virtual-machines-linux-mac-create-ssh-keys.md)
- Konto Azure
  - [Tworzenie konta w bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/)
  - [Azure portal](http://portal.azure.com)
- Linux maszyn wirtualnych uruchomionych azure
- SSH publicznych i prywatnych pary kluczy w`~/.ssh/`
- Klucz publiczny SSH z `~/.ssh/authorized_keys` na maszyn wirtualnych Linux
- Prawa Sudo na maszyn wirtualnych
- Port otwarty 22

## <a name="quick-commands"></a>Szybkie polecenia

_Duże doświadczenie jako administratorów Linux, którzy po prostu chcesz wersji TLDR Rozpocznij tutaj.  Szczegółowy opis i kolejne kroki dla wszystkich pozostałych użytkowników, którą chce pominąć tę sekcję._

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes

# Change PermitRootLogin to this:
PermitRootLogin no

# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no

# On the Debian Family
username@macbook$ sudo service ssh restart

# On the RedHat Family
username@macbook$ sudo service sshd restart
```

## <a name="detailed-walk-through"></a>Szczegółowe kolejne kroki

Zaloguj się do maszyn wirtualnych Linux na terminal 1 (T1).  Zaloguj się do maszyn wirtualnych Linux na końcowych 2 (T2).

Na T2 chwilę do edycji pliku konfiguracji SSHD.  

```
username@macbook$ sudo vim /etc/ssh/sshd_config
```

W tym miejscu możemy będzie edytować tylko ustawienia haseł wyłączyć i włączyć SSH klucza logowania.  Istnieje wiele ustawienia w ten plik, który należy sprawdzić i zmień zabezpieczyć Linux i SSH tak jak to potrzebne.

#### <a name="disable-password-logins"></a>Wyłączanie hasła logowania

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PasswordAuthentication to this:
PasswordAuthentication no
```

#### <a name="enable-public-key-authentication"></a>Włącz uwierzytelnianie klucza publicznego

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PubkeyAuthentication to this:
PubkeyAuthentication yes
```

#### <a name="disable-root-login"></a>Wyłączanie logowania główny

```
username@macbook$ sudo vim /etc/ssh/sshd_config

# Change PermitRootLogin to this:
PermitRootLogin no
```

#### <a name="disable-challenge-response-authentication"></a>Wyłączanie uwierzytelniania wyzwania odpowiedzi

```
# Change ChallengeResponseAuthentication to this:
ChallengeResponseAuthentication no
```

### <a name="restart-sshd"></a>Uruchom ponownie SSHD

Z powłoki T1 upewnij się, że nadal logujesz się.  Jest to krytyczne, więc możesz nie zostaną zablokowane wylogowywanie się z usługi maszyn wirtualnych Jeśli kluczy SSH nie są prawidłowe, ponieważ hasła teraz są wyłączone.  Jeśli wszystkie ustawienia są niepoprawne na swojej maszyn wirtualnych Linux T1 umożliwiają rozwiązywanie sshd_config, możesz nadal będą rejestrowane w i SSH powoduje, że połączenie aktywności podczas pracy usługi SSHD Uruchom ponownie.

Z T2 uruchamianie:

##### <a name="on-the-debian-family"></a>W ramach rodziny Debian

```
username@macbook$ sudo service ssh restart
```

##### <a name="on-the-redhat-family"></a>W ramach rodziny RedHat

```
username@macbook$ sudo service sshd restart
```

Hasła są teraz wyłączone na swojej maszyn wirtualnych chronić je przed atakami prób logowania hasło.  Tylko SSH klucze dozwolone będzie mógł zalogować szybciej i bardziej bezpieczne.
