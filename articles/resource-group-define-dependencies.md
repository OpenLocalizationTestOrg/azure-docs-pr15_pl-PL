<properties
   pageTitle="Zależności w szablonach Menedżera zasobów | Microsoft Azure"
   description="W tym artykule opisano, jak ustawić jeden zasób jako zależne od innego zasobu podczas wdrażania, aby upewnić się, że zasoby są rozmieszczane w odpowiedniej kolejności."
   services="azure-resource-manager"
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
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="defining-dependencies-in-azure-resource-manager-templates"></a>Definiowanie zależności w szablonach Azure Menedżera zasobów

W przypadku danego zasobu może być inne zasoby, które muszą istnieć przed wdrożeniem tego zasobu. Na przykład programu SQL server, musi istnieć przed podjęciem próby wdrażanie z bazą danych SQL. Oznaczając jeden zasób jako zależne od innego zasobu do definiowania relacji. Zazwyczaj Definiowanie współzależności z elementem **dependsOn** , ale można go również zdefiniować przy użyciu funkcji **odwołania** . 

Menedżer zasobów oblicza zależności między zasoby i wdraża je w porządku zależne. Gdy zasoby są zależne od siebie, Menedżer zasobów wdrożeniu jej równolegle.

## <a name="dependson"></a>dependsOn

W szablonie elementu dependsOn pozwala na zdefiniowanie jeden zasób jako zależną na jeden lub więcej zasobów. Wartość może być rozdzielaną średnikami listę nazw zasobów. 

W poniższym przykładzie pokazano zestaw skali maszyn wirtualnych, które zależy od usługi równoważenia obciążenia, wirtualną sieć i pętli, która umożliwia utworzenie wielu kont miejsca do magazynowania. Te inne zasoby nie są wyświetlane w poniższym przykładzie, ale muszą istnieć w innym miejscu w szablonie.

    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "name": "[variables('namingInfix')]",
      "location": "[variables('location')]",
      "apiVersion": "2016-03-30",
      "tags": {
        "displayName": "VMScaleSet"
      },
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      ...
    }

Aby zdefiniować zależność między zasobu i zasoby, które są tworzone przez pętli Kopiuj, ustaw dependsOn element nazwę pętli. Na przykład zobacz [Tworzenie wielu wystąpień zasobów w Menedżerze zasobów Azure](resource-group-create-multiple.md).

Podczas może żądać za pomocą dependsOn mapowanie relacje między zasoby, należy zrozumieć, dlaczego robimy go ponieważ go może mieć wpływ na wydajność wdrożenia. Na przykład do dokumentu, jak zasoby są połączone ze sobą, dependsOn nie jest odpowiednie podejście. Nie można zbadać, zasoby, które zostały zdefiniowane w elemencie dependsOn po wdrożeniu. Za pomocą dependsOn, możesz mogą mieć wpływ na czas wdrażania ponieważ Menedżera zasobów nie wdrożyć w równoległych dwóch zasobów, które mają współzależności. Relacje między zasoby dokumentu, użyj zamiast tego [zasobu łączenia](resource-group-link-resources.md).

## <a name="child-resources"></a>Zasoby podrzędny

Właściwość zasobów umożliwia określenie zasoby podrzędne, które dotyczą zasobów został określony. Zasoby podrzędne może zawierać tylko określonych pięciu poziomów. Należy pamiętać, nie utworzono niejawne współzależności między zasobu podrzędne i zasobów nadrzędnej. W razie potrzeby zasobu podrzędne należy wdrożyć po zasobów nadrzędnej, należy podać jawnie Zależność ta właściwość dependsOn. 

Każdemu zasobowi nadrzędnej akceptuje tylko niektórych typów zasobów jako zasoby podrzędne. Typy zasobów zaakceptowanych są określane w [schematu szablonu](https://github.com/Azure/azure-resource-manager-schemas) zasobu nadrzędnej. Nazwę podrzędny typ zasobu zawiera nazwę nadrzędny typ zasobu, takich jak **Microsoft.Web/sites/config** i **Microsoft.Web/sites/extensions** są zarówno zasobów podrzędne **Microsoft.Web/sites**.

W poniższym przykładzie pokazano programu SQL server i bazy danych SQL. Zwróć uwagę zdefiniowania jawne zależności między bazy danych SQL i programu SQL server, nawet jeśli baza danych znajduje się element podrzędny serwera.

    "resources": [
      {
        "name": "[variables('sqlserverName')]",
        "type": "Microsoft.Sql/servers",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "SqlServer"
        },
        "apiVersion": "2014-04-01-preview",
        "properties": {
          "administratorLogin": "[parameters('administratorLogin')]",
          "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
        },
        "resources": [
          {
            "name": "[parameters('databaseName')]",
            "type": "databases",
            "location": "[resourceGroup().location]",
            "tags": {
              "displayName": "Database"
            },
            "apiVersion": "2014-04-01-preview",
            "dependsOn": [
              "[variables('sqlserverName')]"
            ],
            "properties": {
              "edition": "[parameters('edition')]",
              "collation": "[parameters('collation')]",
              "maxSizeBytes": "[parameters('maxSizeBytes')]",
              "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
            }
          }
        ]
      }
    ]


## <a name="reference-function"></a>Funkcja odwołanie do

[Funkcja odwołanie](resource-group-template-functions.md#reference) umożliwia wyrażenie ma być jej wartość z innej nazwy JSON i pary wartości lub środowisko uruchomieniowe zasobów. Odwołanie wyrażeń deklarować niejawnie, że jeden zasób zależy od drugiej. 

    reference('resourceName').propertyPath

Za pomocą tego elementu lub elemencie dependsOn Określ współzależności, ale nie trzeba używać obu dla tego zasobu zależne. Jeśli to możliwe, umożliwia odwołanie niejawne nie dodawaj przypadkowo zależność niepotrzebne.

Aby uzyskać więcej informacji, zobacz temat [Funkcja odwołania](resource-group-template-functions.md#reference).

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać informacje dotyczące tworzenia szablonów Menedżera zasobów Azure, zobacz [Narzędzia do tworzenia szablonów](resource-group-authoring-templates.md). 
- Aby uzyskać listę dostępnych funkcji w szablonie zobacz [funkcje szablonu](resource-group-template-functions.md).

