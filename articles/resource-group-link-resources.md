<properties 
    pageTitle="Łączenie zasobów w Menedżerze zasobów Azure | Microsoft Azure" 
    description="Tworzenie łącza między zasoby pokrewne w różnych grup zasobów w Menedżerze zasobów Azure." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/01/2016" 
    ms.author="tomfitz"/>

# <a name="linking-resources-in-azure-resource-manager"></a>Łączenie zasobów w Menedżerze zasobów Azure

Podczas wdrażania możesz oznaczyć zasobu jako zależne od innego zasobu, ale tego cyklu życia kończy się wdrożenia. Po wdrożeniu istnieje nie określić związek między zasoby zależne. Menedżer zasobów udostępnia funkcję o nazwie zasobów łączenia można ustanawiać relacje trwałych między zasobami.

Z łączenie zasobów, można udokumentować relacji, które obejmują grup zasobów. Na przykład są często mają bazy danych za pomocą własnej cyklu życia znajdują się w grupy zasobów i aplikacji z różnych cyklem znajdują się w różnych grupach zasobów. Aplikacja łączy do bazy danych, aby chcesz oznaczyć jako łącze między aplikacji i bazy danych. 

Wszystkie zasoby połączone musi należeć do tej samej subskrypcji. Poszczególnych zasobów można połączyć z 50 inne zasoby. Jedynym sposobem kwerendy zasoby pokrewne jest za pośrednictwem interfejsu API usługi REST. Jeśli są połączone zasobów usunięta lub przeniesiona, właściciel łącze musi wyczyść pozostałe łącze. Masz **nie** ostrzeżenie o tym, kiedy Usuwanie zasobu, która jest połączony z innymi zasobami.

## <a name="linking-in-templates"></a>Łączenie w szablonach

Aby zdefiniować łącze w szablonie, możesz umieścić typ zasobu, który łączy nazw dostawcy zasobów i typ zasobu źródła z **/providers/links**. Nazwa musi zawierać nazwę zasobu źródła. Podaj identyfikator zasobu zasobu docelowego. Poniższy przykład nawiązuje połączenie między witryną sieci web i kontem miejsca do magazynowania.

    {
      "type": "Microsoft.Web/sites/providers/links",
      "apiVersion": "2015-01-01",
      "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
      "dependsOn": [ "[variables('siteName')]" ],
      "properties": {
        "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
        "notes": "This web site uses the storage account to store user information."
      }
    }


Aby uzyskać pełny opis formacie szablonu zobacz [łącza zasobów — schematu szablonu](resource-manager-template-links.md).

## <a name="linking-with-rest-api"></a>Łączenie z interfejsu API usługi REST

Aby zdefiniować łącza między wdrożonym zasobów, uruchom polecenie:

    PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/providers/Microsoft.Resources/links/{link-name}?api-version={api-version}

Zamień {identyfikator subskrypcji} identyfikator subskrypcji. Zamienianie {grupa zasobów}, {obszar nazw dostawcy, {typ zasobu}, {nazwa} z wartościami, które wskazują pierwszego zasobu w polu łącze i. Zamień nazwę łącze, aby utworzyć {Nazwa łącza}. Użyj 2015-01-01 dla wersji interfejsu api.

W wezwaniu na obejmują obiektu, który definiuje drugą zasobu w polu łącze:

    {
        "name": "{new-link-name}",
        "properties": {
            "targetId": "subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/{provider-namespace}/{resource-type}/{resource-name}/",
            "notes": "{link-description}",
        }
    }

Element właściwości zawiera identyfikator drugi zasób.

W ramach subskrypcji przy użyciu kwerendy można łącza:

    https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Resources/links?api-version={api-version}

Więcej przykładów w tym również pobrać informacji dotyczących łączy, zobacz [Połączone zasobów](https://msdn.microsoft.com/library/azure/mt238499.aspx).

## <a name="next-steps"></a>Następne kroki

- Możesz również organizować zasobów przy użyciu tagów. Aby uzyskać informacje dotyczące znakowania zasobów, zobacz [Używanie znaczników w celu organizowania zasobów](resource-group-using-tags.md).
- Aby uzyskać opis sposobu tworzenia szablonów i definiowanie zasoby, które mają zostać wdrożone zobacz [Narzędzia do tworzenia szablonów](resource-group-authoring-templates.md).
