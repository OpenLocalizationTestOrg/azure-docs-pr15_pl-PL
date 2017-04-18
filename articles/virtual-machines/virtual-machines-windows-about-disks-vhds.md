<properties
    pageTitle="O dyskach i wirtualnych dysków twardych dla systemu Windows maszyny wirtualne | Microsoft Azure"
    description="Poznaj podstawy dysków i maszyn wirtualnych wirtualnych dysków twardych dla systemu Windows w Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Informacje o dysków i wirtualnych dysków twardych dla Azure maszyn wirtualnych

Podobnie jak dowolnego innego komputera maszyn wirtualnych w Azure dysków jako miejsce do przechowywania systemu operacyjnego, aplikacji i danych. Wszystkie Azure maszyn wirtualnych mieć co najmniej dwa dyski — dysku systemu operacyjnego Windows i tymczasowe. Dysk systemu operacyjnego jest tworzony z obrazu, a zarówno dysku system operacyjny, jak i na obrazie są wirtualnych dyskach twardych (VHD) przechowywanych w konto Azure miejsca do magazynowania. Maszyn wirtualnych mogą mieć także jeden lub więcej dysków danych, które również są przechowywane jako wirtualnych dysków twardych. Ten artykuł jest także dostępny dla [maszyn wirtualnych systemu Linux](virtual-machines-linux-about-disks-vhds.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



## <a name="operating-system-disk"></a>System operacyjny dysku

Co maszyn wirtualnych ma jeden dysk dołączony system operacyjny. Ma zarejestrowany jako dysk SATA i oznaczony jako dysk C: domyślnie. Ten dysk ma pojemność maksymalną 1023 gigabajtów (GB). 

##<a name="temporary-disk"></a>Tymczasowe dysku

Tymczasowe dysku są tworzone automatycznie. Tymczasowe dysku jest oznaczony jako dysk D: domyślnie i go używać do przechowywania pagefile.sys. 

Rozmiar dysku tymczasowe są różne w zależności od rozmiaru maszyny wirtualnej. Aby uzyskać więcej informacji zobacz [maszyn wirtualnych rozmiarów dla systemu Windows](virtual-machines-windows-sizes.md).

>[AZURE.WARNING] Nie przechowuj dane na dysku tymczasowym. Umożliwia przechowywanie tymczasowych dla aplikacji i procesów, a jest przeznaczona tylko przechowywania danych, takich jak pliki strony lub Zamień. Aby ponownie mapować tego dysku, aby inną literę dysku, zobacz [Zmienianie literę dysku tymczasowym systemu Windows](virtual-machines-windows-classic-change-drive-letter.md).

Aby uzyskać więcej informacji o używaniu Azure na dysku tymczasowym zobacz [Opis tymczasowe dysk na maszyn wirtualnych Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Dysku danych

Dysk danych jest wirtualny dysk twardy, podłączonego do komputera wirtualnych w celu przechowywania danych aplikacji lub inne dane, które należy zachować. Dyski danych są rejestrowane jako dyski SCSI i są oznaczone literą, wybranego przez użytkownika.  Każdy dysk danych ma maksymalnej pojemności 1023 GB. Rozmiar maszyny wirtualnej Określa, ile dyski danych można dołączać do go i wpisz przestrzeni dyskowej służy do obsługi dysków.

>[AZURE.NOTE] Aby uzyskać więcej informacji na temat możliwości maszyn wirtualnych zobacz [maszyn wirtualnych rozmiarów dla systemu Windows](virtual-machines-windows-sizes.md).

Azure tworzy dysk systemu operacyjnego, podczas tworzenia maszyny wirtualnej z obrazu. Jeśli używasz obraz zawierający dyski danych Azure tworzy również dyski danych podczas tworzenia maszyny wirtualnej. W przeciwnym razie dyski danych możesz dodać po utworzeniu maszyny wirtualnej.

Możesz dodać dyski danych maszyn wirtualnych w dowolnej chwili przez **dołączenie** dysku maszyn wirtualnych. Możesz użyć wirtualny dysk twardy, które zostały przekazane lub kopiowane do swojego konta miejsca do magazynowania lub taki, który tworzy Azure. Dołączanie dysku danych przypisuje pliku wirtualnego dysku twardego maszyn wirtualnych, umieszczając dzierżawy na wirtualny dysk twardy, nie może zostać usunięty z magazynu podczas nadal jest dołączony.

## <a name="about-vhds"></a>Informacje o wirtualnych dysków twardych

Wirtualnych dysków twardych używane w Azure są przechowywane jako blob strony z konta standardowy lub specjalnego magazynu platformy Azure plików VHD. Aby uzyskać więcej informacji na temat obiektów blob strony zobacz [opis bloku blob i obiektów blob strony](https://msdn.microsoft.com/library/ee691964.aspx). Aby uzyskać więcej informacji na temat przechowywania premium, zobacz [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla obciążenia Azure maszyn wirtualnych](../storage/storage-premium-storage.md).

Azure obsługuje dysk stały format wirtualnego dysku twardego. Stałe format ułożone dysku logicznego liniowo w pliku, przesunięcie tego dysku X jest przechowywany w przesunięcie obiektów blob X. Małe stopki na końcu to opisano właściwości wirtualnego dysku twardego. Często odpady stałym formacie miejsca, ponieważ większość dysków ma duże Nieużywane zakresy w nich. Jednak Azure przechowuje VHD pliki w formacie rzadkie, aby móc skorzystać z zalet stałe i dynamiczne dysków w tym samym czasie. Aby uzyskać więcej informacji zobacz [Wprowadzenie do wirtualnych dyskach twardych](https://technet.microsoft.com/library/dd979539.aspx).

Wszystkie pliki VHD w Azure, której chcesz użyć jako źródła do tworzenia dysków lub obrazy są tylko do odczytu. Po utworzeniu dysku lub obraz Azure sprawia, że kopie plików VHD. Te kopie mogą być tylko do odczytu lub odczytu i zapisu, w zależności od tego, jak używać wirtualnego dysku twardego.

Po utworzeniu maszyny wirtualnej z obrazu Azure tworzy dysku dla maszyn wirtualnych, o kopię pliku VHD źródła. Aby chronić się przed przypadkowym usunięciom, Azure umieści dzierżawę na dowolny plik VHD źródła, który jest używany do tworzenia obrazu, system operacyjny dysku lub dysku danych.

Przed usunięciem pliku VHD źródła, musisz usunąć dzierżawy przez usunięcie dysku lub obrazu. Można usunąć maszyny wirtualnej, dysk systemu operacyjnego i plik źródłowy VHD w całym dokumencie, usuwając maszyny wirtualnej i usunięcia wszystkich skojarzonych dysków. Jednak usunięcie pliku VHD, która jest źródłem danych dysku wykonać kilka kroków w kolejności zestawu. Najpierw odłączyć dysk z maszyny wirtualnej, a następnie usuń dysk, a następnie usuń plik VHD.

>[AZURE.WARNING] Jeśli usuwanie pliku VHD źródła z magazynu lub usuwanie konta miejsca do magazynowania, Microsoft nie można odzyskać danych dla Ciebie.



## <a name="next-steps"></a>Następne kroki
-  [Dołącz dyskiem](virtual-machines-windows-attach-disk-portal.md) , aby dodać dodatkowe miejsce do magazynowania dla swojego maszyn wirtualnych.
-  [Przekaż obraz maszyn wirtualnych systemu Windows Azure](virtual-machines-windows-upload-image.md) do użytku podczas tworzenia nowych maszyn wirtualnych.
-  [Zmień literę dysku tymczasowym systemu Windows](virtual-machines-windows-classic-change-drive-letter.md) , aplikacja może używać dysku D: dla danych.
