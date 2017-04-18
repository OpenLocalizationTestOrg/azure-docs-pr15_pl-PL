<properties
    pageTitle="Dołączanie dysku do maszyny | Microsoft Azure"
    description="Podłącz dyskiem danych utworzona za pomocą modelu Klasyczny wdrażania maszyn wirtualnych systemu Windows, a następnie go zainicjować."
    services="virtual-machines-windows, storage"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="cynthn"/>

# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Dołączanie dysku danych utworzona za pomocą modelu Klasyczny wdrażania maszyn wirtualnych systemu Windows

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Jeśli chcesz korzystać z nowych portalu, zobacz [sposoby dołączania danych dysku maszyn wirtualnych systemu Windows w portalu Azure](virtual-machines-windows-attach-disk-portal.md).

Jeśli potrzebujesz dodatkowych danych dysku, pusty dysk lub istniejący dysk z danymi można dołączać do maszyny. W obu przypadkach dysków to pliki VHD, które znajdują się na koncie Azure miejsca do magazynowania. W przypadku nowego dysku po dołączeniu dysku, również musisz zainicjować go tak jest teraz gotowy do użycia przez maszyn wirtualnych systemu Windows.

Aby uzyskać więcej informacji na temat dysków Zobacz [temat dyski i wirtualnych dysków twardych dla maszyn wirtualnych](virtual-machines-windows-about-disks-vhds.md).


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>Inicjowanie dysku

1. Połącz maszyn wirtualnych. Aby uzyskać instrukcje, zobacz, [jak zalogować się do maszyny wirtualnej z systemem Windows Server][logon].

2. Po zalogowaniu się maszyn wirtualnych Otwórz **Menedżera serwera**. W okienku po lewej stronie wybierz **plików i usługi magazynu**.

    ![Otwieranie Menedżera serwera](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Rozwinięcie menu, a następnie wybierz pozycję **dysków**.

4. W sekcji **dysków** Wyświetla listę dysków. W większości przypadków będzie miał dysku 0, dysk 1 i 2. Dysk 0 jest system operacyjny, dysk 1 jest tymczasowy i dysk 2 jest dyskiem danych, który właśnie dołączone do maszyn wirtualnych. Nowy dysk danych znajduje się lista partycją jako **Nieznane**. Kliknij prawym przyciskiem myszy dysk, a następnie wybierz pozycję **zainicjować**.

5.  Wiesz, że wszystkie dane zostaną usunięte podczas inicjowania dysku. Kliknij przycisk **Tak,** zapoznaj się z ostrzeżeniem i zainicjowanie dysku. Po zakończeniu Partion zostaną wyświetlone jako **GPT**. Ponownie kliknij prawym przyciskiem myszy dysk, a następnie wybierz pozycję **Nowy**.

6.  Kończenie pracy kreatora przy użyciu wartości domyślne. Po zakończeniu Kreatora sekcji **wielkości** lista nowego woluminu. Dysk jest teraz online i gotowa do przechowywania danych.

    ![Głośność zainicjowany pomyślnie](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] Rozmiar maszyn wirtualnych określa liczbę dysków można do niego dołączyć. Aby uzyskać szczegółowe informacje zobacz [rozmiarów maszyn wirtualnych](virtual-machines-linux-sizes.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

[Jak odłączyć dysk z komputera wirtualnych systemu Windows](virtual-machines-windows-classic-detach-disk.md)

[Informacje o dysków i wirtualnych dysków twardych dla maszyn wirtualnych](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md
