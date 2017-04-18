<properties
    pageTitle="Instalowanie MySQL na maszyn wirtualnych OpenSUSE | Microsoft Azure"
    description="Dowiedz się, jak zainstalować MySQL na komputerze OpenSUSE Linux VMirtual platformy Azure."
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
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>MySQL zostaną zainstalowane na komputerze wirtualnych systemem OpenSUSE Linux platformy Azure

[MySQL] [ MySQL] są popularne, Otwórz źródło bazy danych SQL. Ten samouczek pokazano sposób tworzenia maszyny wirtualnej systemem OpenSUSE Linux, a następnie zainstalować MySQL.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


<br>


## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Tworzenie maszyny wirtualnej systemem OpenSUSE Linux

[AZURE.INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Instalowanie i uruchamianie MySQL na komputerze wirtualnych

[AZURE.INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Następne kroki
Aby uzyskać szczegółowe informacje o MySQL, zapoznaj się z [Dokumentacją MySQL][MySQLDocs].

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com

