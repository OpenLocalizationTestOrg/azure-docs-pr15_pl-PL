<properties
    pageTitle="Co to są skali maszyn wirtualnych ustawia? | Microsoft Azure"
    description="Informacje na temat zestawów skali maszyn wirtualnych."
    keywords="Ustawia skali maszyn wirtualnych maszyn wirtualnych Linux," 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/24/2016"
    ms.author="gatneil"/>

# <a name="what-are-virtual-machine-scale-sets"></a>Co to są skali maszyn wirtualnych ustawia?

Zestawy skali maszyn wirtualnych umożliwiają zarządzanie wieloma maszyny wirtualne jako zestaw. Na wysokim poziomie zestawy skali mają następujące wad i zalet:

Zalety:

1. Wysokiej dostępności. Każdy zestaw Skala powoduje umieszczenie jej maszyny wirtualne w dostępność zestaw z 5 błędów domen (FDs) i 5 aktualizacji domen (UDs) w celu zapewnienia dostępności (Aby uzyskać więcej informacji o FDs i UDs, zobacz [dostępność maszyn wirtualnych](./virtual-machines-linux-manage-availability.md)). 
2. Łatwe Integracja z równoważenia obciążenia Azure i bramą aplikacji.
3. Łatwe Integracja z Azure Autoscale.
4. Uproszczone wdrażanie, zarządzanie i oczyszczanie maszyny wirtualne.
5. Obsługuje typowych podtypy Windows i Linux, a także niestandardowych obrazów.

Zalety:

1. Nie można dołączyć dyski danych wystąpieniach maszyn wirtualnych w zestawie Skala. Zamiast tego należy użyć magazyn obiektów Blob, pliki Azure, Azure tabele lub inne rozwiązanie miejsca do magazynowania.

## <a name="quick-create-using-azure-cli"></a>Szybkie tworzenie za pomocą interfejsu wiersza polecenia Azure

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Następne kroki

Aby uzyskać ogólne informacje zapoznaj się z [Strona początkowa głównym dla zestawów Skala](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

Więcej dokumentacji zapoznaj się z [zestawów strony głównej dokumentacji Skala](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Na przykład szablony Menedżera zasobów przy użyciu skali zestawy, wyszukaj "vmss" w [repo github Azure Szybki Start szablonów](https://github.com/Azure/azure-quickstart-templates).

