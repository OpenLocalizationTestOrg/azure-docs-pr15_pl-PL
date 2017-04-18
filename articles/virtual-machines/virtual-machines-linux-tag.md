<properties
   pageTitle="Jak oznaczać maszyny wirtualnej Linux | Microsoft Azure"
   description="Informacje na temat znakowania Linux maszyny wirtualnej utworzone w Azure przy użyciu modelu wdrożenia Menedżera zasobów."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="mmccrory"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="07/05/2016"
   ms.author="memccror"/>

# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Jak oznaczać maszyny wirtualnej Linux platformy Azure

W tym artykule opisano różne sposoby znakowanie maszyny wirtualnej Linux platformy Azure za pomocą modelu wdrożenia Menedżera zasobów. Tagi są pary klucz wartość zdefiniowane przez użytkownika, które mogą być umieszczone bezpośrednio na zasób lub grupa zasobów. Azure obsługuje obecnie maksymalnie 15 znaczników dla zasobu i grupy zasobów. Znaczniki mogą być umieszczone na zasób w momencie tworzenia lub dodane do istniejącego zasobu. Pamiętaj, znaczniki są obsługiwane w przypadku zasobów utworzone za pomocą tylko model wdrożenia Menedżera zasobów.

[AZURE.INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Znakowanie z polecenie Azure

Rozpocząć, [Instalowanie i konfigurowanie polecenie Azure](../xplat-cli-azure-resource-manager.md) i upewnij się, że jesteś w trybie Menedżera zasobów (`azure config mode arm`).

Można wyświetlić wszystkie właściwości dla danej maszyny wirtualnej, łącznie z znaczniki, przy użyciu tego polecenia:

        azure vm show -g MyResourceGroup -n MyTestVM

Aby dodać nowy znacznik maszyn wirtualnych za pośrednictwem interfejsu wiersza polecenia Azure, możesz użyć `azure vm set` polecenia wraz z parametrem znacznika **-t**:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Aby usunąć wszystkie znaczniki, można użyć parametru **-T** w `azure vm set` polecenia.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Teraz, gdy mamy zastosowano znaczniki naszych zasobów polecenie Azure i portalu, Przyjrzyjmy się przyjrzeć się informacje dotyczące sposobu użycia, aby wyświetlić znaczniki w portalu rozliczeń.

[AZURE.INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się więcej o znakowaniu Azure zasobów, zobacz [Omówienie Menedżera zasobów Azure][] i [Za pomocą znaczników w celu organizowania zasobów Azure][].
* Aby zobaczyć, jak znaczniki są pomocne przy zarządzaniu użytkowanie zasobów Azure, zobacz [Opis rachunku Azure][] i [uzyskać więcej informacji do usługi Microsoft Azure zużycie zasobów][].





[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Omówienie Menedżera zasobów Azure]: ../azure-resource-manager/resource-group-overview.md
[Organizowanie zasobów Azure za pomocą znaczników]: ../resource-group-using-tags.md
[Opis rachunku Azure]: ../billing/billing-understand-your-bill.md
[Uzyskanie wniosków do usługi Microsoft Azure zużycie zasobów]: ../billing-usage-rate-card-overview.md
