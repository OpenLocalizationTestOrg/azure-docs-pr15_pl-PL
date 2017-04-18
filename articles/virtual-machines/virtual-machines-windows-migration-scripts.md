<properties
    pageTitle="Społeczności narzędzia do zarządzania usługą Azure migracji Menedżera zasobów Azure"
    description="W tym artykule kataloguje narzędzi, które były udzielane przez społeczność ułatwiających migracji zasobów IaaS z zarządzania usługą Azure stos Azure Menedżera zasobów."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="singhkay"/>

# <a name="community-tools-for-azure-service-management-to-azure-resource-manager-migration"></a>Społeczności narzędzia do zarządzania usługą Azure migracji Menedżera zasobów Azure

W tym artykule kataloguje narzędzi, które były udzielane przez społeczność ułatwiających migracji zasobów IaaS z zarządzania usługą Azure stos Azure Menedżera zasobów.

>[AZURE.NOTE]Te narzędzia nie są oficjalnie obsługiwane przez Support firmy Microsoft. Dlatego są otwarte powierzając jej ich konserwację na Github i chętnie zaakceptować PRs poprawki lub dodatkowe scenariusze. Aby zgłosić problem, funkcja Github problemów.
>
> Migrowanie z tych narzędzi spowoduje przestoje komputera wirtualnych klasyczny. Jeśli szukasz platformy obsługiwane migracji, odwiedź stronę 
>
>- [Platformy obsługiwane migracji zasobów IaaS od klasycznej stos Azure Menedżera zasobów](./virtual-machines-windows-migration-classic-resource-manager.md)
>- [Techniczne głębokości Dive na platformie obsługiwane migracji z klasycznym do Menedżera zasobów Azure](./virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
>- [Migrowanie IaaS zasoby z klasycznym do Azure Menedżera zasobów przy użyciu programu PowerShell Azure](./virtual-machines-windows-ps-migration-classic-resource-manager.md)

## <a name="asm2arm"></a>ASM2ARM

To jest moduł skrypt programu PowerShell migracji z **jednym** maszyn wirtualnych (maszyn wirtualnych) z stos Zarządzanie usługą Azure stos Azure Menedżera zasobów. 

[Łączenie się z dokumentacją narzędzie](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## <a name="migaz"></a>migAz

migAz jest dodatkowa opcja migracji kompletny zestaw Azure usługi zarządzania IaaS zasobów do IaaS Menedżera zasobów Azure zasobów. Migracja mogą występować w obrębie tej samej subskrypcji lub między różnych subskrypcjach i typy subskrypcji (ex: subskrypcje dostawcy).

[Łączenie się z dokumentacją narzędzie](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)