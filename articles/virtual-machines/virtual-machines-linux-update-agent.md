<properties
    pageTitle="Aktualizowanie Azure agenta Linux z GitHub | Microsoft Azure"
    description="Dowiedz się, jak update Azure Linux Agent dla swojego maszyn wirtualnych Linux platformy Azure do wersji lateset z Github"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="mingzhan"/>


# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>Jak zaktualizować agenta Linux Azure na maszyny do najnowszej wersji z GitHub

Aby zaktualizować usługi [Azure Linux agenta](https://github.com/Azure/WALinuxAgent) na maszyny Linux platformy Azure, musi już masz:

1. Bieżąca maszyn wirtualnych Linux platformy Azure.
2. Połączenie z tym maszyn wirtualnych Linux przy użyciu SSH.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [AZURE.NOTE] Jeśli będziesz wykonywać tego zadania komputera z systemem Windows, możesz użyć Kit do SSH do komputera Linux. Aby uzyskać więcej informacji zobacz [jak zalogować się do maszyn wirtualnych systemem Linux](virtual-machines-linux-mac-create-ssh-keys.md).

Zatwierdzone Azure distros Linux wprowadziły pakietu agenta Linux Azure ich repozytoria tak Sprawdź i zainstalować najnowszą wersję z tego repozytorium Distro najpierw, jeśli to możliwe.  

Aby uzyskać Ubuntu wystarczy wpisać:

    #sudo apt-get install walinuxagent

I na CentOS, wpisz:

    #sudo yum install waagent


Upewnij się, że Linux Oracle `Addons` repozytorium jest włączona. Wybierz pozycję, aby edytować plik `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) lub `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux) i zmień wiersz `enabled=0` do `enabled=1` w obszarze **[ol6_addons]** lub **[ol7_addons]** w tym pliku.

Następnie Aby zainstalować najnowszą wersję programu Azure Linux Agent, wpisz:


    #sudo yum install WALinuxAgent

Jeśli nie możesz znaleźć repozytorium dodatku możesz po prostu dodać te wiersze na końcu pliku .repo zgodnie z wersji programu Oracle Linux:

Dla maszyn wirtualnych Oracle Linux 6:

    [ol6_addons]
    name=Add-Ons for Oracle Linux $releasever ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
    gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
    gpgcheck=1
    enabled=1

Dla maszyn wirtualnych Oracle Linux 7:

    [ol7_addons]
    name=Oracle Linux $releasever Add ons ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
    gpgcheck=1
    enabled=0

Następnie należy wpisać:

    #sudo yum update WALinuxAgent

Zazwyczaj jest wszystkie potrzebne, ale jeśli jakiegoś powodu musisz zainstalować to https://github.com bezpośrednio, wykonaj następujące czynności.


## <a name="install-wget"></a>Instalowanie wget

Zaloguj się do swojego maszyn wirtualnych przy użyciu SSH.

Instalowanie wget (istnieją pewne distros, który nie instaluj domyślnie, takie jak wersje Redhat, CentOS i Oracle Linux 6.4 i 6.5), wpisując `#sudo yum install wget` w wierszu polecenia.


## <a name="download-the-latest-version"></a>Pobierz najnowszą wersję

Otwórz [wydania agenta Linux Azure w GitHub](https://github.com/Azure/WALinuxAgent/releases) na stronie sieci web i Dowiedz się, numer najnowszej wersji. (W przypadku znalezienia jego bieżącą wersję, wpisując `#waagent --version`.)

### <a name="for-version-20x-type"></a>Dla wersji 2.0.x, należy wpisać:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   Następujący wiersz używa wersji 2.0.14, na przykład:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### <a name="for-version-21x-or-later-type"></a>Dla wersji 2.1.x lub później, wpisz:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   Następujący wiersz używa wersji 2.1.0, na przykład:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## <a name="install-the-azure-linux-agent"></a>Instalowanie Azure agenta Linux

### <a name="for-version-20x-use"></a>Dla wersji 2.0.x, użyj:

 Wprowadź waagent wykonywalny:

    #chmod +x waagent

 Skopiuj nowy plik wykonywalny do usr/sbin /.

  W przypadku większości Linux Użyj:

    #sudo cp waagent /usr/sbin

  W przypadku CoreOS Użyj:

    #sudo cp waagent /usr/share/oem/bin/

  Jeśli jest to nowa instalacja agenta Linux Azure Uruchom:
 
    #sudo /usr/sbin/waagent -install -verbose

### <a name="for-version-21x-use"></a>Dla wersji 2.1.x, użyj:

Może być konieczne zainstalowanie pakietu `setuptools` najpierw — zobacz [poniżej](https://pypi.python.org/pypi/setuptools). Następnie uruchom:

    #sudo python setup.py install

## <a name="restart-the-waagent-service"></a>Ponowne uruchamianie usługi waagent

Większość linux Distros:

    #sudo service waagent restart

W przypadku Ubuntu Użyj:

    #sudo service walinuxagent restart

W przypadku CoreOS Użyj:

    #sudo systemctl restart waagent

## <a name="confirm-the-azure-linux-agent-version"></a>Potwierdź wersji Azure Linux Agent

    #waagent -version

Dla CoreOS powyższe polecenie może nie działać.

Zobaczysz, że wersji Azure Linux Agent została zaktualizowana do nowej wersji.

Aby uzyskać więcej informacji na temat agenta Linux Azure zobacz [Azure Linux agenta — Plik README](https://github.com/Azure/WALinuxAgent).
