<properties 
    pageTitle="Wybieranie nazwy użytkowników Linux | Microsoft Azure" 
    description="Dowiedz się, jak zaznaczyć nazwy użytkownika dla maszyny wirtualnej Linux Azure." 
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



#<a name="selecting-user-names-for-linux-on-azure"></a>Wybieranie nazwy użytkowników Linux Azure#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Gdy świadczenie maszyny wirtualnej Linux Azure Podaj nazwę użytkownika innego niż główny, który później służy do zalogowania się do maszyn wirtualnych. Można wybrać nazwę nowego użytkownika lub jeśli inicjowania obsługi administracyjnej za pośrednictwem portalu klasyczny Azure możesz zaakceptować domyślną nazwę "azureuser".

W większości przypadków tego użytkownika nie istnieje na obrazie podstawowym i zostanie utworzony w procesie inicjowania obsługi administracyjnej. Jeśli użytkownik istnieje na obrazie podstawowym maszyn wirtualnych, a następnie agent Azure Linux po prostu konfiguruje hasło i/lub klawisz SSH dla tego użytkownika na podstawie informacji z określonym przez Ciebie podczas tworzenia maszyn wirtualnych.

**Jednak**Linux definiuje zestaw nazw użytkowników, które nie powinny być używane. Proces inicjowania obsługi administracyjnej **się nie powieść,** Jeśli zostanie próby obsługi administracyjnej maszyny Linux przy użyciu istniejącego użytkownika systemu, który jest zdefiniowany jako użytkownik mający UID 0 – 99. Typowy przykład `root` użytkownik, który ma UID 0.

 - Zobacz też: [zakresy standardowe Linux Base - identyfikator użytkownika](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

Poniżej przedstawiono listę typowych użytkowników wbudowany system CentOS i Ubuntu, że należy unikać podczas inicjowania obsługi administracyjnej maszyny wirtualnej Linux Azure. Ta lista jest po prostu przykładu, można znaleźć w dokumentacji usługi dystrybucji upewnić się, że wybrane nazwa użytkownika nie powoduje konfliktu z istniejącego użytkownika systemu.


## <a name="centos"></a>CentOS
- abrt
- adm
- audio
- Kosz
- CD-ROM
- cgred
- demon
- dbus
- inicjowania połączeń
- DIP
- dysku
- Dyskietka
- FTP
- FTP
- gry
- Gopher
- haldaemon
- zatrzymanie
- kmem
- blokady
- LP
- Poczta
- Mężczyzna
- pamięci
- nfsnobody
- nikt
- NTP
- operator
- oprofile
- postdrop
- przyrostkowe
- qpidd
- główny
- RPC
- rpcuser
- saslauth
- zamknięcie
- slocate
- sshd
- stapdev
- stapusr
- Synchronizowanie
- dotyczącego
- taśmą
- Test
- tcpdump
- Tryb TTY
- Użytkownicy
- utempter
- utmp
- UUCP
- vcsa
- klip wideo
- kółko


## <a name="ubuntu"></a>Ubuntu
- adm
- Administrator
- audio
- wykonywanie kopii zapasowych
- Kosz
- CD-ROM
- crontab
- demon
- inicjowania połączeń
- DIP
- dysku
- faksu
- Dyskietka
- Łączenie
- gry
- gnats
- IRC
- kmem
- Orientacja pozioma
- libuuid
- Lista
- LP
- Poczta
- Mężczyzna
- messagebus
- mlocate
- netdev
- wiadomości
- nikt
- nogroup
- operator
- plugdev
- Serwer proxy
- główny
- SASL
- Cień
- src
- SSH
- sshd
- personelu
- sudo
- Synchronizowanie
- dotyczącego
- SYSLOG
- taśmą
- Tryb TTY
- Użytkownicy
- utmp
- UUCP
- klip wideo
- głosowej
- whoopsie
- dane ciągu "www"

 