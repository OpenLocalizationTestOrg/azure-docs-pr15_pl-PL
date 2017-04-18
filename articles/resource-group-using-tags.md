<properties
    pageTitle="Organizowanie zasobów Azure za pomocą znaczników | Microsoft Azure"
    description="Pokazano, jak stosowanie znaczników w celu organizowania zasoby dotyczące rozliczeń i zarządzania nimi."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="AzurePortal"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="tomfitz"/>


# <a name="using-tags-to-organize-your-azure-resources"></a>Organizowanie zasobów Azure za pomocą znaczników

Menedżer zasobów umożliwia logicznie Organizuj zasoby, stosując znaczniki. Tagi składają się z pary klucz wartość zidentyfikuj zasoby z właściwościami, zdefiniowane przez użytkownika. Aby oznaczyć zasobów należących do tej samej kategorii, należy zastosować ten sam znacznik do tych zasobów.

Podczas wyświetlania zasoby o określony znacznik, zobaczysz zasoby z wszystkich grup zasobów. Nie są ograniczone do tylko zasobów w tej samej grupy zasobów, umożliwiają organizowanie zasobów w sposób niezależny od relacji wdrożenia. Znaczniki mogą być pomocne, gdy chcesz uporządkować zasoby dotyczące rozliczeń i zarządzania.

Każdego znacznika, który został dodany do zasobu lub grupy zasobów zostaną automatycznie dodane do taksonomii całej subskrypcji. Można też wypełnić wstępnie taksonomii dla subskrypcji z nazwami znaczników i wartości, które chcesz używać jako zasoby są oznakowane w przyszłości.

Każdy zasób lub grupa zasobów może zawierać maksymalnie 15 znaczników. Nazwa znacznika jest ograniczona do 512 znaków, a wartość tagu jest ograniczona do 256 znaków.

> [AZURE.NOTE] Znaczniki można stosować tylko do zasobów, które obsługują operacje Menedżera zasobów. Jeśli utworzono maszyn wirtualnych, wirtualną sieć lub miejsce do magazynowania za pośrednictwem modelu Klasyczny rozmieszczania (takich jak portalu klasyczny), znacznika nie można zastosować do tego zasobu. Aby obsługiwać znakowania społecznościowego, ponownie wdróż tych zasobów za pomocą Menedżera zasobów. Inne zasoby pomocy technicznej znakowania.

## <a name="templates"></a>Szablony

Aby oznakować zasobu podczas wdrażania, po prostu Dodaj element **znaczniki** do zasobu, który instalujesz i podaj nazwę znacznika i wartość. Nazwy znacznika i wartość nie trzeba wstępnie istnieje w ramach subskrypcji. Maksymalnie 15 znaczników umożliwia dla każdego zasobu.

W poniższym przykładzie pokazano konto miejsca do magazynowania ze znacznikiem.

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Obecnie Menedżera zasobów nie obsługuje przetwarzania obiektu dla nazwy znacznika i wartości. Należy przekazać obiektu dla wartości znacznika, ale należy określić nazwy znaczników, jak pokazano w poniższym przykładzie.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## <a name="portal"></a>Portal

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## <a name="powershell"></a>Programu PowerShell

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## <a name="azure-cli"></a>Polecenie Azure

[AZURE.INCLUDE [resource-manager-tag-resources-cli](../includes/resource-manager-tag-resources-cli.md)]

## <a name="rest-api"></a>INTERFEJSU API USŁUGI REST

Portal i programu PowerShell zarówno za pomocą [Interfejsu API usługi REST Menedżera zasobów](https://msdn.microsoft.com/library/azure/dn848368.aspx) w tle. Jeśli chcesz zintegrować znakowania do innego środowiska, można uzyskać znaczniki z GET na identyfikator zasobu i aktualizować zestaw znaczników z połączeniem poprawki.


## <a name="tags-and-billing"></a>Znaczniki i rozliczenia

Dla usług obsługiwanych znaczników umożliwia grupowanie danych rozliczeń. Na przykład [maszyn wirtualnych zintegrowany z Menedżerem zasobów Azure](./virtual-machines/virtual-machines-windows-compare-deployment-models.md) umożliwiają definiowanie i stosowanie znaczników w celu organizowania rozliczeń zastosowania dla maszyn wirtualnych. Jeśli używasz wielu maszyny wirtualne dla innych organizacji, można znaczników w celu zastosowania grupy przez Centrum kosztów.  
Umożliwia także znaczniki kategoryzowania kosztów przez środowisko uruchomieniowe środowiska; przykład zastosowania rozliczeń dla maszyny wirtualne działa w środowisku produkcyjnym.

Można pobrać informacji na temat znaczników przez [Użycie zasobu Azure oraz interfejsy API RateCard](billing-usage-rate-card-overview.md) lub zastosowania plik wartości rozdzielanych przecinkami (CSV). Pobierz plik zastosowania z [portal Azure konta](https://account.windowsazure.com/) lub [EA portalu](https://ea.azure.com). Aby uzyskać więcej informacji na temat programowy dostęp do informacji rozliczeniowych dotyczących zobacz [uzyskanie wniosków do usługi Microsoft Azure zużycie zasobów](billing-usage-rate-card-overview.md). Aby operacje interfejsu API usługi REST zobacz [Informacje dotyczące interfejsu API pozostałych Azure rozliczenia](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Po pobraniu zastosowania CSV dla usług, które obsługują znaczniki dotyczącej rozliczeń znaczniki są wyświetlane w kolumnie **znaczników** . Aby uzyskać więcej informacji zobacz [Opis rachunku dla platformy Microsoft Azure](billing/billing-understand-your-bill.md).

![Wyświetlić znaczniki rozliczeń](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Następne kroki

- Ograniczenia i konwencje można stosować w subskrypcji z niestandardowych zasad. Zasady zdefiniowanych może wymagać, aby wszystkie zasoby mają wartości dla określony znacznik. Aby uzyskać więcej informacji zobacz [Używanie zasad zarządzania zasobami i kontrolować dostęp](resource-manager-policy.md).
- Wprowadzenie do przy użyciu programu PowerShell Azure, wdrażając zasobów zobacz [Używanie Azure przy użyciu Menedżera zasobów Azure](./powershell-azure-resource-manager.md).
- Wprowadzenie do korzystania z platformy Azure polecenie wdrażając zasobów uzyskać [za pomocą interfejsu wiersza polecenia Azure dla komputerów Mac, Linux i systemu Windows z zarządzania zasobami Azure](./xplat-cli-azure-resource-manager.md).
- Wprowadzenie do korzystania z portalu dla [za pomocą portalu Azure do zarządzania zasobami Azure](./azure-portal/resource-group-portal.md)  
