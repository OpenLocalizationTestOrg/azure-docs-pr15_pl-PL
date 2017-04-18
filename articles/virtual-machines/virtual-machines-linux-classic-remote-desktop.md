<properties
    pageTitle="Pulpit zdalny do maszyny Linux | Microsoft Azure"
    description="Dowiedz się, jak zainstalować i skonfigurować pulpitu zdalnego, aby nawiązać połączenie maszyny Linux Microsoft Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/01/2016"
    ms.author="mingzhan"/>


#<a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a>Za pomocą pulpitu zdalnego, aby nawiązać połączenie maszyny Linux Microsoft Azure

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


##<a name="overview"></a>Omówienie

RDP (Remote Desktop Protocol) jest protokołem własnych używany dla systemu Windows. Jak będziemy używać RDP nawiązać Linux maszyn wirtualnych (maszyna wirtualna) zdalnie?

Te wskazówki nadać odpowiedzi! Może pomóc w zainstalowaniu i xrdp konfiguracji na maszyn wirtualnych usługi Microsoft Azure Linux i można go połączyć z pulpitu zdalnego z komputera systemu Windows. Użyjemy maszyn wirtualnych Linux systemem Ubuntu lub OpenSUSE jako przykład w tych wskazówek.

Xrdp jest serwerem RDP Otwórz źródło, które umożliwia łączenie serwera Linux za pomocą pulpitu zdalnego z komputera systemu Windows. Wykonuje znacznie wrażeń niż VNC (wirtualna sieć przetwarzania danych). VNC zawiera ten passa "JPEG" jakości i wolne zachowanie RDP jest szybkie i przejrzysty.


> [AZURE.NOTE] Musi być już maszyn wirtualnych Microsoft Azure systemem Linux. Aby utworzyć i skonfigurować maszyny Linux, zobacz [Samouczek maszyn wirtualnych Linux Azure](virtual-machines-linux-classic-createportal.md).


##<a name="create-endpoint-for-remote-desktop"></a>Utworzyć punkt końcowy dla pulpitu zdalnego
Użyjemy domyślny punkt końcowy 3389 dla pulpitu zdalnego w tym dokumencie. Tak skonfigurować 3389 punktu końcowego jako pulpitu zdalnego z Linux maszyn do wirtualnych tak jak poniżej:


![Obraz](./media/virtual-machines-linux-classic-remote-desktop/no1.png)


Jeśli nie wiesz, jak skonfigurować punkt końcowy do swojego maszyn wirtualnych, zobacz [wskazówki na temat](virtual-machines-linux-classic-setup-endpoints.md).


##<a name="install-gnome-desktop"></a>Instalowanie pulpitu Gnome

Nawiązywanie połączenia z maszyn wirtualnych Linux za pośrednictwem Kit i zainstalować `Gnome Desktop`.

W przypadku Ubuntu Użyj:

    #sudo apt-get update
    #sudo apt-get install ubuntu-desktop


W przypadku OpenSUSE Użyj:

    #sudo zypper install gnome-session

##<a name="install-xrdp"></a>Instalowanie xrdp

W przypadku Ubuntu Użyj:

    #sudo apt-get install xrdp

W przypadku OpenSUSE Użyj:

> [AZURE.NOTE] Aktualizacji wersji OpenSUSE wersją używasz do poniżej polecenia, poniżej polecenie przykładowe dla `OpenSUSE 13.2`.

    #sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
    #sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc


##<a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Uruchom xrdp i skonfiguruj xdrp w górę uruchamiania

W przypadku OpenSUSE Użyj:

    #sudo systemctl start xrdp
    #sudo systemctl enable xrdp

Dla Ubuntu, zostanie uruchomiony xrdp i eanbled w uruchamiania telefoniczne automatycznie po zakończeniu instalacji.

##<a name="using-xfce-if-you-are-using-ubuntu-version-later-than-ubuntu-1204lts"></a>Przy użyciu xfce, jeśli używasz wersji Ubuntu później niż Ubuntu 12.04LTS

Ponieważ bieżący xrdp może nie obsługuje pulpitu Gnome później niż Ubuntu 12.04LTS w wersji Ubuntu, użyjemy `xfce` pulpitu zamiast tego.

Instalowanie `xfce`, za pomocą:

    #sudo apt-get install xubuntu-desktop

Następnie włączyć `xfce`, za pomocą:

    #echo xfce4-session >~/.xsession

Edytowanie pliku konfiguracji `/etc/xrdp/startwm.sh`, za pomocą:

    #sudo vi /etc/xrdp/startwm.sh   

Dodaj wiersz `xfce4-session` przed wierszem `/etc/X11/Xsession`.

Ponowne uruchamianie usługi xrdp, za pomocą:

    #sudo service xrdp restart


##<a name="connect-your-linux-vm-from-a-windows-machine"></a>Nawiązywanie połączenia z maszyn wirtualnych Linux z komputera systemu Windows
Na komputerze systemu Windows, uruchom klienta pulpitu zdalnego, wprowadzania nazwę DNS maszyn wirtualnych Linux lub przejdź do `Dashboard` z usługi maszyn wirtualnych w portal Azure klasyczny i kliknij pozycję `Connect` nawiązać połączenie z maszyn wirtualnych Linux, pojawi się poniżej okno logowania:

![Obraz](./media/virtual-machines-linux-classic-remote-desktop/no2.png)

Zaloguj się przy użyciu `user`  &  `password` z usługi Głosowa Linux i korzystać z pulpitu zdalnego z maszyn wirtualnych usługi Microsoft Azure Linux teraz!


##<a name="next"></a>Następny
Aby uzyskać więcej informacji na umożliwia xrdp może odnosić się [tutaj](http://www.xrdp.org/).
