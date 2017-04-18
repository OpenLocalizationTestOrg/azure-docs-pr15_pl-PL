<properties
   pageTitle="Szablon Menedżera zasobów do przechowywania | Microsoft Azure"
   description="Pokazuje schemat Menedżera zasobów do wdrażania miejsca do magazynowania konta za pomocą szablonu."
   services="azure-resource-manager,storage"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="storage-account-template-schema"></a>Schematu szablonu konta miejsca do magazynowania

Tworzy konto miejsca do magazynowania.

## <a name="schema-format"></a>Format schematu

Aby utworzyć konto miejsca do magazynowania, Dodaj następującego schematu do sekcji Zasoby szablonu.

    {
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2015-06-15",
        "name": string,
        "location": string,
        "properties": 
        {
            "accountType": string
        }
    }

## <a name="values"></a>Wartości

W poniższych tabelach opisano wartości, które należy ustawić w schemacie.

| Nazwa | Wartość |
| ---- | ---- |
| Typ | Wyliczenia<br />Wymagane<br />**Microsoft.Storage/storageAccounts**<br /><br />Typ zasobu, aby utworzyć. |
| apiVersion | Wyliczenia<br />Wymagane<br />**2015-06-15** lub **2015-05-01-Podgląd**<br /><br />Wersja interfejsu API służących do tworzenia zasobu. | 
| Nazwa | Ciąg<br />Wymagane<br />Pomiędzy 3 a 24 znaki, tylko małe litery i cyfry.<br /><br />Nazwa konta przestrzeni dyskowej, aby utworzyć. Nazwa musi być unikatowa we wszystkich Azure. Warto rozważyć użycie funkcji [uniqueString](resource-group-template-functions.md#uniquestring) z Konwencja nazewnictwa, jak pokazano w poniższym przykładzie. |
| Lokalizacja | Ciąg<br />Wymagane<br />Obszar, który obsługuje konta miejsca do magazynowania. Aby określić prawidłową regionów, zobacz [obsługiwane regionów](resource-manager-supported-services.md#supported-regions).<br /><br />Region do obsługi konta miejsca do magazynowania. |
| właściwości | Obiekt<br />Wymagane<br />[właściwości obiektu](#properties)<br /><br />Obiekt, który określa typ konta przestrzeni dyskowej, aby utworzyć. |

<a id="properties" />
### <a name="properties-object"></a>właściwości obiektu

| Nazwa | Wartość |
| ---- | ---- | 
| dla konta | Ciąg<br />Wymagane<br />**Standard_LRS**, **Standard_ZRS**, **Standard_GRS**, **Standard_RAGRS**lub **Premium_LRS**<br /><br />Typ konta miejsca do magazynowania. Dozwolone wartości odpowiadają standardowy lokalnie zbędne, zbędne strefy standardowej standardowe nadmiarowe Geo, standardowy odczytu Geo-nadmiarowe i Premium lokalnie zbędne. Aby uzyskać informacji na temat tych typów kont Zobacz [replikacji magazyn Azure](./storage/storage-redundancy.md ). |

    
## <a name="examples"></a>Przykłady

Poniższy przykład wdraża standardowy lokalnie zbędne konta miejsca do magazynowania z unikatową nazwę na podstawie identyfikatora grupy zasobów.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2015-06-15",
                "name": "[concat('storage', uniqueString(resourceGroup().id))]",
                "location": "[resourceGroup().location]",
                "properties": 
            {
                    "accountType": "Standard_LRS"
            }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Szablony Szybki Start

Istnieje wiele szablonów Szybki Start, zawierające konta miejsca do magazynowania. Następujące szablony przedstawiono kilka typowych scenariuszy:

- [Tworzenie konta standardowego magazynu](https://azure.microsoft.com/documentation/templates/101-storage-account-create)
- [Proste wdrażanie maszyn wirtualnych systemu Windows](https://azure.microsoft.com/documentation/templates/101-vm-simple-windows)
- [Proste wdrażanie maszyn wirtualnych Linux](https://azure.microsoft.com/documentation/templates/101-vm-simple-linux)
- [Tworzenie profilu CDN, punkt końcowy CDN za pomocą konta miejsca do magazynowania jako pochodzenia](https://azure.microsoft.com/documentation/templates/201-cdn-with-storage-account)
- [Tworzenie wysokiej Availabilty farmy programu SharePoint z 9 maszyny wirtualne przy użyciu programu Powershell rozszerzenia DSC](https://azure.microsoft.com/documentation/templates/sharepoint-server-farm-ha)
- [Proste wdrażanie 5 węzeł bezpiecznego klaster tkaninie usługi z WAD włączone](https://azure.microsoft.com/documentation/templates/service-fabric-secure-cluster-5-node-1-nodetype-wad)
- [Tworzenie maszyny wirtualnej z obrazu systemu Windows z 4 dysków pusty danych](https://azure.microsoft.com/documentation/templates/101-vm-multiple-data-disk)


## <a name="next-steps"></a>Następne kroki

- Aby uzyskać ogólne informacje na temat miejsca do magazynowania zobacz [Wprowadzenie do magazyn Microsoft Azure](./storage/storage-introduction.md).
- Na przykład szablony, które nowe konto miejsca do magazynowania za pomocą maszyny wirtualnej, zobacz [Rozmieszczanie prostą maszyn wirtualnych Linux](https://azure.microsoft.com/documentation/templates/101-simple-linux-vm/) lub [Rozmieszczanie prostych maszyn wirtualnych systemu Windows](https://azure.microsoft.com/documentation/templates/101-simple-windows-vm/).
