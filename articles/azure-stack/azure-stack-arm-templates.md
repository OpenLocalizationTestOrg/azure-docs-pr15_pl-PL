<properties
    pageTitle="Korzystanie z szablonów Menedżera zasobów Azure w stos Azure (deweloperzy dzierżawy) | Microsoft Azure"
    description="Dowiedz się, jak za pomocą Menedżera zasobów Azure szablony w stos Azure wdrażania i obsługi administracyjnej wszystkich zasobów dla aplikacji w jednym, skoordynowanego operacji."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="helaw"/>

# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Korzystanie z szablonów Menedżera zasobów Azure w stos Azure

Azure szablony Menedżera zasobów wdrażania i obsługi administracyjnej wszystkich zasobów dla aplikacji w jednym, skoordynowanego operacji. Definiowanie zasobów dla aplikacji i jak wdrożyć.  Można także ponownie wdróż szablony, aby wprowadzić zmiany w zasoby w grupie zasobów.

Szablony te mogą być rozmieszczone z portalem Microsoft Azure stos, programu PowerShell, wiersza polecenia i programu Visual Studio.

[AZURE.VIDEO microsoft-azure-stack-tp1--foundational-skills-1-deploying-json-templates]

Następujące szablony są dostępne w [GitHub](http://aka.ms/azurestackgithub):

## <a name="deploy-sharepoint-non-high-availability"></a>Wdrażanie programu SharePoint (dostępności-wysoki)

Rozszerzenie programu PowerShell DSC umożliwia tworzenie farmie programu SharePoint 2013, w której znajdują się następujące informacje:

-   Wirtualna sieć

-   Trzy konta miejsca do magazynowania

-   Dwa urządzenia do równoważenia obciążenia zewnętrznych

-   Jeden maszyn wirtualnych skonfigurowanego jako nowy las jednej domeny kontrolera domeny

-   Jeden maszyn wirtualnych skonfigurowanego jako serwer autonomiczny program SQL Server 2014

-   Jeden maszyn wirtualnych skonfigurować jako farmy programu SharePoint 2013 na jednym komputerze

## <a name="deploy-ad-non-high-availability"></a>Wdrażanie AD (dostępności-wysoki)

Rozszerzenie programu PowerShell DSC umożliwia tworzenie AD serwera kontrolera domeny, która zawiera następujące:

-   Wirtualna sieć

-   Jedno konto miejsca do magazynowania

-   Jeden równoważenia obciążenia zewnętrznych

-   Jeden maszyn wirtualnych skonfigurowanego jako nowy las jednej domeny kontrolera domeny

## <a name="deploy-adsql-non-high-availability"></a>Wdrażanie AD-SQL (dostępności-wysoki)

Rozszerzenie programu PowerShell DSC umożliwia tworzenie serwer autonomiczny program SQL Server 2014 obejmuje następujące czynności:

-   Wirtualna sieć

-   Dwa konta miejsca do magazynowania

-   Jeden równoważenia obciążenia zewnętrznych

-   Jeden maszyn wirtualnych skonfigurowanego jako nowy las jednej domeny kontrolera domeny

-   Jeden maszyn wirtualnych skonfigurowanego jako serwer autonomiczny program SQL Server 2014

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

Rozszerzenie programu PowerShell DSC służy do konfigurowania istniejących maszyn wirtualnych Menedżer konfiguracji lokalnych (NAJMN.wsp.WIEL) i zarejestrować ją z serwerem pobierają DSC konta automatyzacji Azure.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Tworzenie maszyny wirtualnej z obrazu użytkownika

Tworzenie maszyny wirtualnej z obrazu użytkownika niestandardowego. Ten szablon rozmieszcza także wirtualnej sieci (z systemem DNS), publiczny adres IP i karty sieciowej.

## <a name="simple-vm"></a>Prosta maszyn wirtualnych

Wdrażanie prostych maszyn wirtualnych systemu Windows, zawierający wirtualną sieć (z systemem DNS), publiczny adres IP i karty sieciowej.

## <a name="cancel-a-running-template-deployment"></a>Anulowanie uruchomionego rozmieszczania szablonu

Aby anulować uruchomionego rozmieszczania szablonu, należy użyć `Stop-AzureRmResourceGroupDeployment` polecenia cmdlet programu PowerShell.


## <a name="next-steps"></a>Następne kroki

[Wdrażanie szablonów z portalu](azure-stack-deploy-template-portal.md)

[Omówienie Menedżera zasobów Azure](../azure-resource-manager/resource-group-overview.md)

