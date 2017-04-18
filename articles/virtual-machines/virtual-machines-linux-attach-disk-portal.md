<properties
    pageTitle="Dołączanie dyskiem danych do maszyny Linux | Microsoft Azure"
    description="Jak dołączyć dysku nowych lub istniejących danych do maszyny Linux w portalu Azure przy użyciu modelu wdrożenia Menedżera zasobów."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Dołączanie dysku danych do maszyny Linux w portalu Azure

W tym artykule pokazano, jak dołączyć dyski istniejących i nowych maszyn wirtualnych Linux za pośrednictwem portalu Azure. Można także [dołączyć dyskiem danych do maszyn wirtualnych systemu Windows w portalu Azure](virtual-machines-windows-attach-disk-portal.md). Przed wykonaniem tej czynności, przejrzyj poniższe wskazówki:

- Rozmiar maszyny wirtualnej określa liczbę dyski danych można dołączyć. Aby uzyskać szczegółowe informacje zobacz [rozmiarów maszyn wirtualnych](virtual-machines-linux-sizes.md).
- Aby korzystać z magazynu Premium, musisz Zasadami serii lub serii GS maszyny wirtualnej. Za pomocą dysków z kont miejsca do magazynowania zarówno Premium i standardowe tych maszyn wirtualnych. Magazyn Premium jest dostępna w niektórych regionach. Aby uzyskać szczegółowe informacje, zobacz [miejsca do magazynowania Premium: wysokiej wydajności miejsca do magazynowania dla Azure maszyn wirtualnych obciążenia](../storage/storage-premium-storage.md).
- Dyski dołączone do maszyn wirtualnych są faktyczną VHD pliki przy użyciu konta z miejsca do magazynowania Azure. Aby uzyskać szczegółowe informacje zobacz [temat dysków i wirtualnych dysków twardych dla maszyn wirtualnych](virtual-machines-linux-about-disks-vhds.md).
- Dla nowego dysku nie musisz utworzyć go najpierw, ponieważ Azure tworzy go w przypadku dołączania.
- Dla istniejącego dysku plik VHD muszą być dostępne na koncie Azure miejsca do magazynowania. Możesz użyć VHD, który jest już istnieje, jeśli nie jest dołączony do innego maszyn wirtualnych lub przekazywania własnych VHD plików na koncie miejsca do magazynowania.


[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a name="next-steps"></a>Następne kroki

Po dodaniu dysku należy przygotować go do użycia. Zobacz w systemie operacyjnym maszyny wirtualnej: "jak: inicjowania dysku danych w Linux" w tym [artykule](virtual-machines-linux-classic-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux).
