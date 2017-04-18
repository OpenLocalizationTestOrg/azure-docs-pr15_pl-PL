<properties
    pageTitle="Korzystanie z rozszerzeniem CustomScript na maszyny Linux | Microsoft Azure"
    description="Dowiedz się, jak użyć rozszerzenia CustomScript do wdrażania aplikacji na maszyn wirtualnych Linux platformy Azure utworzony przy użyciu modelu Klasyczny wdrożenia."
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="gbowerman"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

#<a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Wdrażanie aplikacji światła przy użyciu rozszerzenia CustomScript Azure Linux#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Specyfikacja rozszerzenia firmy Microsoft Azure CustomScript Linux umożliwia dostosowywanie usługi wirtualnych maszyn, uruchamiając dowolnego kodu napisanego w innym języku skryptów obsługiwane przez maszyn wirtualnych (na przykład Python i imprezie). Dzięki temu bardzo elastyczne aby zautomatyzować wdrażanie aplikacji na wielu komputerach.

Można wdrażać z rozszerzeniem CustomScript przy użyciu portalu klasyczny Azure, środowiska Windows PowerShell lub interfejsu wiersza polecenia Azure (polecenie Azure).

W tym artykule, które będą używane polecenie Azure, aby wdrożyć aplikację światło prosty do maszyn wirtualnych Ubuntu utworzony przy użyciu modelu Klasyczny wdrożenia.

## <a name="prerequisites"></a>Wymagania wstępne

W tym przykładzie należy najpierw utworzyć dwa Azure maszyny wirtualne z Ubuntu 14.04 lub nowszym. Maszyny wirtualne są nazywane *maszyn wirtualnych skryptu* i *maszyn wirtualnych światło*. Po utworzeniu maszyny wirtualne za pomocą unikatowych nazw. Jeden służy do uruchamiania poleceń interfejsu wiersza polecenia i jedną służy do wdrażania aplikacji światło.

Ponadto potrzebny konta magazyn Azure i kluczem do nich dostęp (możesz uzyskać dostęp do tej z portalu klasyczny Azure).

Jeśli potrzebujesz pomocy w tworzeniu maszyny wirtualne Linux Azure odwołują się do [tworzenia maszyn wirtualnych systemem Linux](virtual-machines-linux-classic-createportal.md).

Zainstaluj założenia Ubuntu, ale możesz dopasować instalacji dla dowolnego obsługiwanego distro Linux.

Maszyn wirtualnych maszyn wirtualnych skrypt musi mieć zainstalowany za pomocą połączenia pracy Azure polecenie Azure. Aby uzyskać pomoc dotyczącą to odwołują się do [instalowania i konfigurowania interfejsu wiersza polecenia Azure](../xplat-cli-install.md).

## <a name="upload-a-script"></a>Przekazywanie skryptu

Użyjemy rozszerzenia CustomScript do uruchomienia skryptu na zdalnego maszyn wirtualnych, aby zainstalować stos światło i utworzyć stronę PHP. Aby uzyskać dostęp do skryptu z dowolnego miejsca możemy zostanie przekazany jako obiektów blob platformy Azure.

### <a name="script-overview"></a>Omówienie skryptu

Przykładowy skrypt instaluje stosem światła Ubuntu (w tym konfigurowania instalację MySQL), zapisuje prosty plik PHP i rozpoczęcie Apache.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Przekazywanie skryptu

Skrypt należy zapisać jako plik tekstowy, na przykład *install_lamp.sh*, a następnie przekazanie go do magazynu Azure. Możesz to zrobić łatwo z polecenie Azure. Poniższy przykład wysyła plik do kontenera miejsca do magazynowania o nazwie "skrypty". Jeśli kontener nie istnieje, musisz najpierw utworzyć.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Również utworzyć plik JSON opisujący sposób pobierania skrypt z magazynu Azure. Zapisz ten jako *public_config.json* (zastępując "mystorage" z nazwą swojego konta miejsca do magazynowania):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>Wdrażanie rozszerzenia

Teraz następnego polecenia umożliwia Wdroż zdalnego maszyn wirtualnych za pomocą interfejsu wiersza polecenia Azure rozszerzenia CustomScript Linux.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Polecenie poprzednie pobiera i uruchamia skrypt *install_lamp.sh* na maszyn wirtualnych, o nazwie *maszyn wirtualnych światło*.

Ponieważ aplikacji obejmuje serwer sieci web, pamiętaj, aby otworzyć port słuchanie HTTP na zdalnym maszyn wirtualnych z następnego polecenia.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Monitorowania i rozwiązywania problemów

Możesz sprawdzić, czy na jak skryptu niestandardowego jest uruchomiony, wyświetlając plik dziennika na zdalnym maszyn wirtualnych. SSH *maszyn wirtualnych światło* i zakończenie plik dziennika z następnego polecenia.

    cd /var/log/azure/customscript
    tail -f handler.log

Po uruchomieniu z rozszerzeniem CustomScript, można przeglądać na stronę PHP, utworzoną informacji. Strona PHP, na przykład w tym artykule jest *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>Dodatkowe zasoby

Te same kroki podstawowe służy do wdrażania aplikacji bardziej złożone. W tym przykładzie skrypt instalacji została zapisana jako publicznej obiektów blob w magazynie Azure. Bezpieczniejsze może być przechowywana skrypt instalacji jako bezpieczne obiektów blob przy użyciu [Podpisu bezpiecznego dostępu](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SA).

Dodatkowe zasoby dotyczące polecenie Azure, Linux i rozszerzenia CustomScript znajdują się dalej.

[Automatyzowanie zadań dostosowywania Linux maszyn wirtualnych przy użyciu rozszerzenia CustomScript](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Rozszerzenia Azure Linux (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux i Open Source obliczeniowych Azure](virtual-machines-linux-opensource-links.md)
