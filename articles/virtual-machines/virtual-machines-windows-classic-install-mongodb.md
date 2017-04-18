<properties
    pageTitle="Instalowanie MongoDB na maszyn wirtualnych systemu Windows | Microsoft Azure"
    description="Dowiedz się, jak zainstalować MongoDB na maszyn wirtualnych Azure utworzone za pomocą modelu Klasyczny wdrażania z systemem Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

#<a name="install-mongodb-on-a-windows-vm"></a>Instalowanie MongoDB na maszyn wirtualnych systemu Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Aby zainstalować i skonfigurować MongoDB przy użyciu modelu wdrożenia Menedżera zasobów, zobacz [Ten artykuł](virtual-machines-windows-classic-install-mongodb.md).

[MongoDB] [ MongoDB] jest popularne Otwórz źródło, wysokiej wydajności NoSQL baza danych. W tym artykule przeprowadzi Cię przez proces tworzenia maszyny wirtualnej systemu Windows Server (maszyn wirtualnych) za pomocą [portal Azure klasyczny][AzurePortal]. Następnie utwórz i Dołącz dysku danych do maszyn wirtualnych przed instalowania i konfigurowania MongoDB. Jeśli masz istniejące maszyn wirtualnych w Azure, której chcesz użyć, można przejść bezpośrednio do [instalowania i konfigurowania MongoDB](#install-and-run-mongodb-on-the-virtual-machine).


## <a name="create-a-virtual-machine-running-windows-server"></a>Tworzenie wirtualnych komputera z systemem Windows Server

Wykonaj te instrukcje, aby utworzyć maszyny wirtualnej.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] Dodawanie punktu końcowego dla MongoDB podczas tworzenia maszyny wirtualnej i skonfiguruj go w następujący sposób: Nazwa go jako **Mongo**, za pomocą protokołu **TCP** i ustaw publicznych i prywatnych porty **27017**.

## <a name="attach-a-data-disk"></a>Dołączanie dysku danych
Aby zapewnić miejsca do magazynowania dla maszyny wirtualnej, Dołącz dysku danych i zainicjować go tak, aby móc jej używać systemu Windows. Jeśli masz już dysku danych, można dołączyć istniejącego dysku lub można dołączać pusty dysk.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

Aby uzyskać instrukcje dotyczące inicjowania dysku, zobacz "jak: inicjowania dysku danych w systemie Windows Server" w temacie [jak dołączyć dysku danych maszyn wirtualnych systemu Windows](virtual-machines-windows-classic-attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Instalowanie i uruchamianie MongoDB na komputerze wirtualnych

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Podsumowanie
W tym samouczku przedstawiono sposób tworzenia maszyny wirtualnej z systemem Windows Server, zdalnie nawiązania połączenia i dołączanie dysku danych.  Wiesz również jak zainstalować i skonfigurować MongoDB na komputerze wirtualnych systemu Windows. Teraz masz dostęp MongoDB na komputerze wirtualnych systemu Windows, wykonując poniższe tematy Zaawansowane w [dokumentacji MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com
